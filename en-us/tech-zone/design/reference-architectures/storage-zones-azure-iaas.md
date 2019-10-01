---
layout: doc
---
# Title

## Contributors

**Author:** [name](https://twitter.com/handle)

## Audience

This reference architecture document is intended for IT decision makers, consultants, solution integrators and partners who are seeking to deploy a customer-managed storage zone for Citrix Content Collaboration within their Azure subscription.

## Objective of this document

Citrix Content Collaboration allows customers to select the location where their files are stored. Customers can choose to store files within Citrix’s own cloud-native storage zone, a customer-managed storage zone location in their on-premises data center, a public cloud of their choice, or a combination. In addition to that, the customer-managed storage zone also functions as a gateway into existing repositories such as network file shares or SharePoint Server document libraries.
This document focuses specifically on guidance for deploying storage zones on Azure cloud.
The scope of this document is limited to:

*  Architectural components for an Azure storage zone
*  Provide guidance on location where to store persistent file objects and cache
*  Provide guidance on configuring the Citrix ADC for an optimal user experience
*  Best practices recommendation to integrate antivirus solutions into the storage zone

## Use Cases

Customers deploy their own customer-managed storage zones for different reasons. Typical use cases for utilizing storage zones on Azure include:

*  **Performance:**  While Citrix hosts storage zones in multiple geographic regions; customers in certain regions may not have access to a zone that provides the user experience employees expect. A customer can decide to deploy the storage zone directly in their Data center closer to the users, however this will impose the tax of CAPEX cost and ongoing operations/management costs. With 54 Azure regions across the globe, customer will have more choice deploying the storage zone in a region closer to their users. That reduces the latency when transferring files providing a better user experience to employees and external collaborators.
In the scenario when a customer deploys a Citrix Virtual App and Desktop resource location in Azure, customer should always deploy a Content collaboration storage zone in Azure in the same region closer proximity to their App/Desktops in Azure.
*  **Governance and Data Residency:**  Similar to the prior statement, a customer might be required to host the data in a specific geography where Citrix might not have that footprint. The alternative would be customer hosting in their own datacenter and potentially being covered by one of the Azure Regions.
*  **Modernize existing repositories:** Migration of files is not always possible or can be time consuming. With Citrix Content Collaboration this doesn’t mean that employees cannot have or have to wait for modern ways of accessing and working with files. By deploying a customer-managed storage zone, customers can directly unlock a modern workstyle on any device without having to migrate existing data.

## Content Collaboration platform overview

Citrix Content Collaboration is an enterprise content collaboration platform that enables IT to deliver a robust data sharing and sync service that meets the mobility and collaboration needs of users and the data security requirements of the enterprise. Citrix Content Collaboration offers the flexibility to choose the location where files are stored at rest, either inside a Citrix-managed cloud-native storage repository or inside a storage repository managed by the customer.
Citrix Content Collaboration consists of three primary components: the Content Collaboration management plane, the storage zones and the Citrix Files apps.

*  Management plane: a Citrix-managed component hosting the Content Collaboration services and business logic, hosted in either the United States or the European Union.
*  Storage zones: the location where customer files are stored. Customers have a choice in where to store files, either hosted by Citrix or hosted by the customer in their own data center or on their own public cloud service subscriptions. This reference architecture will focus on a customer-managed storage zone hosted within Azure.
*  Citrix Files (client side): native apps providing access to the Content Collaboration services. Citrix Files apps are available for Windows, MacOS, iOS, Android, in addition to Outlook and Gmail.

INSERT IMAGE

## Customer-managed storage zone architecture in Azure

The customer-managed storage zone stores all the file objects uploaded to the Content Collaboration service. It also provides access to these file objects, both to employees and external people collaborating on these files. The third and final function of the storage zone is to provide access to existing repositories hosted on-premises, such as network file shares, SharePoint Server document libraries and the OpenText Documentum document management system. To provide those capabilities, the storage zone includes these components.

*  Storage zone controllers: Windows-based web servers that host the storage zone controller services.
*  Storage repository: the location where the Citrix files objects are persistently stored. Recommendation: Microsoft Azure Blob Storage or an SMB share hosted by an Azure IaaS VM (consideration points discussed below). It is recommended you configure the file share to be highly available (using Storage Spaces Direct) and highly performant by attaching premium data disks.
*  Storage cache: the location where data files from the Citrix Files client software are temporarily cached. Recommendation: We recommend Azure Files or a standard SMB share attached to a VM. If using an SMB share, it is recommended you configure the file share to be highly available (using Storage Spaces Direct) and highly performant by attaching premium data disks.
*  Citrix ADC: the application delivery controller provides network load balancing, SSL offloading and authentication services for inbound traffic to the storage zone.

### Storage zones controllers

The storage zone controller is a Windows package consisting of ASP.NET web services and background Windows services. The controller software runs on top of a Windows Server IaaS VM with Internet Information Services (IIS). The system requirements for a storage zone controller are located [here](https://docs.citrix.com/en-us/storagezones-controller/5-0/system-requirements.html).

To reduce server overhead and attack surface, it’s recommended to use Windows Server Core (available in Azure as well) instances with the Internet Information Server and ASP.NET application server roles enabled.

The number of storage zone controllers needed depends on how the storage zone deployment is being used. This number is impacted by a number of factors. These factors include, but are not limited to:

*  Amount of new and updated files being stored: impacts the bandwidth required between the storage zone controllers and storage repository, to prevent file I/O queues from building up on the storage zone controller.
*  Size of files being stored: incoming files are uploading in multiple parts and the storage zone controllers merge these parts back together into a single file object. Merging the parts increases the CPU utilization, with larger files requiring more CPU processing power.
*  Concurrent number of sessions: the number of concurrent file transfer sessions to the storage zone, for either file downloads or file uploads, impacts the number of controller hosts needed.
*  Usage of on-premises connectors: the metadata for files and folders in existing repositories is retrieved by the storage zone controllers in real-time, when the user opens a connector or browses to a different folder.

The recommendation is to deploy at least two storage zone controllers per storage zone for high availability purposes, with the Citrix ADC providing load balancing services for inbound traffic. The baseline figure for such a deployment is 5,000 users inside the Content Collaboration tenant, with 2,500 users per additional controller host added to the storage zone. The actual usage of the storage zone will determine how many controller hosts are needed. Customer deployments range from 2 controller hosts for 250 users, where the users are working with large files, to 4 controller hosts for 250,000 users where the use case is limited to occasional file sharing.

Internal testing has shown the optimal maximum number of controllers per storage zone is 4. For deployments where more controllers are needed in a single site, it’s recommended to set up multiple storage zones.

To setup the Storage Zones Controller in Azure, we recommend:

*  Leading with DS or FS [series](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes) virtual machines as server machines – start with the DS4v2 instance type and modify from there. Modifying the instance types of the storage controller doesn’t require component reinstall, just a reboot. For details on VM limits (IOPS/throughput/CPU) please check the following [link](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-general).
*  Using a premium disk in the virtual machine is recommended with Host Cache set to Read/Write.
*  Enabling port 443 on the NSG (Network Security Groups) on the Storage Controller adapters.
*  When creating the multiple controllers make sure that they are created within an Availability Set or Availability Zone depending on your reliability/availability requirements.

See the [Performance Monitoring](#Performance-Monitoring-tips) chapter for further details.

### Storage repository

**Azure Blob Storage:** Azure Blob Storage should be used to store the file objects. The storage zone controller hosts will use the native APIs to perform file transfers.

We recommend creating a V2 storage account with premium performance in Azure with GRS (Geo-Redundant Storage) as the replication mechanism. If the user chooses not to have a Geo-redundancy, the general guidance is to configure with LRS (Locally-redundant storage). However, users need to note that there is a 20,000 IOPS limit for Azure storage accounts. Please refer to this Microsoft documentation for Storage account [limits](https://docs.microsoft.com/en-us/azure/storage/common/storage-scalability-targets).

**SMB File Share:** If users need more than 20,000 IOPS, then an SMB file share is the appropriate route. They can create a network share which supports the SMB protocol in an Azure virtual machine with managed data disks and use the shared folder as a Storage Repository. Alternatively, the user can consider Azure NetApp Files (highly available out of the box).
When architecting your highly available IaaS file share, make sure to use Storage Spaces Direct and keep a close eye on performance limits. IOPS limits are imposed on [managed disk](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types) types and sizes as well as [VM types](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-general) (whichever is lower).

### Storage cache

**Azure Files:** A local cache on Azure Files will store the latest files being uploaded to or downloaded from the storage zone. Experience shows that recently used files are often synced to different devices or shared with other people. Not having to retrieve those files from the Azure Blob storage repository each time will increase the user experience and reduce the amount of bandwidth being used between storage zone and cloud repository.

Azure Files supports the SMB protocol and shares can be mounted on a Windows machine. Please refer to this article for mounting [Azure Files](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows).

**SMB File Share:** Alternatively, an SMB file share with highly performant premium data disks can be used for the cache. Please note that this option is not highly available by design and hence it is suggested you use Storage Spaces Direct and have the VM part of an availability set.  Our recommendation is to attach a Premium SSD to a DS or FS series Azure Virtual machines.

**Note:** Our testing has shown no significant performance differences between Azure Files and Premium SSD.

INSERT IMAGE

#### Encryption of file objects

The storage zone allows configuring encryption of all file objects on a file level. Encryption is performed by the storage zone controller hosts by using an AES 256-bit encryption key that’s generated during the initial setup of the storage zone. Enabling encryption will impact the performance of the storage zone controller hosts, as encrypting and decrypting file objects will increase CPU utilization.

It’s recommended to only use this option when encryption of all files must be enabled according to the corporate security policy and no other means to encrypt the storage layer are available. Encryption by the storage zone controller hosts will remove the ability to perform data deduplication on the persistent storage folder inside the storage zone. It will also make recovering data impossible when rebuilding the storage zone without access to the SCKeys.txt file containing the encryption key or the configuration passphrase for the storage zone.

### Citrix ADC

The application delivery controller is used in front of the storage zone. The Citrix ADC performs the following capabilities:

*  **SSL offloading:** Incoming SSL sessions for the storage zone are terminated on the ADC. Depending on the security and monitoring requirements of the customer, traffic between the ADC and storage zone controller hosts can then either be encrypted or unencrypted.
*  **Load balancing:** Traffic is load balanced between the storage zone controller hosts by the ADC to provide high availability. To determine which storage zone controller hosts are operational, the ADC monitors the results of the heartbeat on each host. Hosts that have an invalid response to the heartbeat are considered offline and no file transfer sessions are directed towards those hosts.
*  **Authentication:** Users need to authenticate when accessing connectors. It’s best practice to perform this authentication inside the DMZ and not let remote users go inside the LAN unauthenticated. By configuring Kerberos Constrained Delegation on the Citrix ADC (Please refer to this [article](https://support.citrix.com/article/CTX222454), local users accessing connectors from domain-joined devices are authenticated seamlessly without being required to provide credentials.
*  **Content Switching:** Traffic to the Citrix Files repository and connectors differ from each other: connectors require authentication and traffic to the Citrix Files repository is always without authentication. This is due to allow sharing files with external users, who don’t have credentials to authenticate with the Active Directory of the customer. Due to this difference, the ADC performs content switching to separate this traffic and apply the authentication policies to the load balancer for connectors traffic. On the load balancer for Citrix Files traffic a responder policy is configured to validate the authenticity of the request: only requests originating with a request from the Content Collaboration management plane are allowed through to the storage zone controller hosts.

#### Separating local and remote user sessions

To provide the best possible user experience for local users, the configuration on the Citrix ADC is separated for local and remote users. Local users are considered to be accessing the storage zone either from a domain-joined device or the Citrix Files apps on iOS and Android managed with Citrix Endpoint Management. Another reason to deploy internal and external interfaces on the ADC is to increase security. The Citrix ADC is located inside the DMZ, with the external interface having an IP address inside the DMZ range and the internal interface having an IP address inside the local range. To allow local users to access the correct IP address, a **split DNS** needs to be configured as the storage zone has only a single, public DNS name and certificate.

Local users access the storage zone from an internal load balancing server or content switching server with an internal IP address. The choice for either a load balancing server or content switching server is based on whether authentication for connectors needs to occur on the Citrix ADC. In that case a content switching server is needed and the load balancing server for connectors for local users uses Kerberos to authenticate the users when accessing connectors. Remote users access the storage zone from an external content switch with an IP address in the DMZ range. The load balancing server for connectors for remote users uses Basic Authentication (username and password) to authenticate users when accessing connectors. Authentication for connectors on the storage zone controller hosts uses Windows Authentication, the connectors repositories use the authentication method configured on the repository. For SharePoint Server this is typically Kerberos authentication, for file servers that’s typically NTLM authentication.

INSERT IMAGE

**Note:** Instead of configuring an internal and external content switching server on a single ADC sitting on the boundary of the DMZ and local network, it’s also possible to configure a Citrix ADC inside the DMZ for external users and a Citrix ADC on the local network for internal users.

INSERT IMAGE

## Other Considerations

In addition to the key architectural components discussed above in the context of Azure, we are including general guidance when it comes to antivirus solutions and performance monitoring for storage zones.

### Integration with antivirus solutions

We recommend all files uploaded to the storage zone should be scanned for viruses and malware. Due to the separation of the file metadata from the file object, scanning for and removing infected or suspicious files directly from the storage zone is not recommended. While this will remove the file, the file metadata will persist and the user will see the file inside their Citrix Files apps and the Web UI. When they try to access the file, an error message is displayed stating the file doesn’t exist.

To provide a better user experience and still maintain the required level of security, the storage zone can leverage antivirus solutions through an ICAP interface. Upon receiving new and updated files, the storage zone controller hosts will add the file to the scanning queue. The file is sent via ICAP to the antivirus servers and after scanning the file the antivirus servers return the result. The storage zone controller hosts upload this status to be added to the file metadata stored inside the Content Collaboration management plane. Based on the configured antivirus policy for the tenant account, users will then be restricted in the ability to download or share files that are flagged infected.

### Performance Monitoring tips

The storage zone controller is an ASP.NET web application running on Internet Information Server. Monitoring the performance of the storage zone controller host follows the general principles of monitoring any web server running ASP.NET applications. Below are recommended metrics to identify performance issues.

#### Memory

*  **Available Mbytes**: Should not drop below a sustained value of 20% of total available RAM.
*  **Pages Input/Sec**: Should not exceed 15. The lower the value the better the performance of the storage zone controller.
*  **Pages/Sec**: Should not exceed a sustained value of 5 or higher.

#### Logical Disk

*  **% Disk Time**: Should not exceed a sustained value of 50% or higher.
*  **Average Disk Queue Length**: Should not exceed the number of spindles plus 2.
*  **Average Disk Read Queue Length**: Should be below 2.
*  **Average Disk Write Queue Length**: Should be below 4.
*  **Average Disk Sec/Read**: The average should be 15ms or lower, with maximums of 25ms.
*  **Average Disk Sec/Write**: The average should be 15ms or lower, with maximums of 25ms.
*  **Average Disk/Transfer**: Should be below 20ms.

#### Processor

The processor performance monitors should be captured against the system and the W3WP process running the storage zone controller app. If the values are close to each other, then the web services are consuming the CPU, if not then other processes are impacting the performance of the system.

*  **% Privileged Time**: Should not exceed a sustained value of 75% or higher.
*  **% Processor Time**: Should not exceed a sustained value of 85% or higher.

#### System

*  **Content Switches/Sec**: Should not exceed 15,000/second per CPU.

#### Web Service Cache

*  **Kernel:URI Cache Hits %**: Should not drop below 80%.

#### ASP.NET Apps v4.0.30319 (_Total_)

*  **Request Wait Time**: Should not exceed 1,000ms.

## Summary

Citrix Content Collaboration offers the ability to store files in the location that fits your organization’s needs best. This document describes the architecture to deploy the Content Collaboration storage zone in your Microsoft Azure IaaS and all the components needed.
The use case of the Content Collaboration service in your organization impacts components needed:

*  The place where persistent file objects will be stored
*  A storage cache where recently accessed files can be retrieved
*  Access to the Content Collaboration storage repository and existing repositories through storage zone connectors.
*  The device types of the users accessing the storage zone, in addition to the kind of external collaboration.
The described architecture covers the tested and supported deployment model our customers implement together with the Citrix Services teams. The most common use case is to access both the Content Collaboration storage repository and existing repositories through storage zone connectors.

## References

[Resources for Citrix Content Collaboration](https://docs.citrix.com/en-us/citrix-content-collaboration.html)

[Resources for storage zone controller](https://docs.citrix.com/en-us/storagezones-controller)

[Citrix File Deployment Guide](https://docs.citrix.com/en-us/tech-zone/build/deployment-guides/citrix-files.html)
