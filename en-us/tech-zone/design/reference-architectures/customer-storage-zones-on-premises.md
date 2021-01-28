---
layout: doc
h3InToc: true
contributedBy: Rob Sanders
description: Learn about the architecture and design considerations for deploying an on-premises customer-managed storage zone to provide the best user experience and security for Citrix Content Collaboration.
---
# Content Collaboration with on-premises storage zones

## Audience

This reference architecture document is intended for IT decision makers, consultants, solution integrators, and partners who are seeking to deploy a customer-managed storage zone for Citrix Content Collaboration inside the customer data center.

## Objective

Citrix Content Collaboration allows customers to select the location where their files are stored. Customers can choose to store files within Citrix's own cloud-native storage zone, a customer storage zone location in the data center or cloud of their choice, or a combination. In addition to that location, the on-premises storage zone also functions as a gateway into existing repositories such as network file shares and SharePoint Server document libraries. The scope of this document is limited to:

-  Architectural components for an on-premises storage zone
-  Provide guidance on location where to store persistent file objects
-  Provide guidance on configuring the Citrix ADC for an optimal user experience
-  Integrate antivirus solutions into the on-premises storage zone

## Use Cases

Customers deploy their own customer-managed storage zones for different reasons. Typical use cases include:

-  **Data sovereignty**: To comply with local or internal compliance regulations, customers can have the requirement to store all files in their own data center. With a customer-managed storage zone the customer can benefit from the productivity and collaboration capabilities of Citrix Content Collaboration, while storing all files inside their own data center.
-  **Performance**: Citrix hosts storage zones in multiple geographic regions, but customers in certain regions cannot have access to a zone that provides the user experience employees expect. By deploying a customer-managed storage zone, those customers can host the file objects closer to their users, reduce the latency when transferring files. This provides a better user experience to employees and external collaborators.
-  **Modernize existing repositories**: Migration of files is not always possible or can be time consuming. With Citrix Content Collaboration this doesn't mean that employees cannot have or have to wait for modern ways of accessing and working with files. By deploying a customer-managed storage zone, customers can directly unlock a modern work style on any device without having to migrate existing data.
-  **Protect infrastructure investments**: By hosting the storage zone on-premises and using already deployed storage hardware such as a NAS, customers can provide a modern cloud-native collaboration platform without existing their storage hardware becoming obsolete.

## Content Collaboration platform overview

Citrix Content Collaboration is an enterprise content collaboration platform that enables IT to deliver a robust data sharing and sync service that meets the mobility and collaboration needs of users and the data security requirements of the enterprise. Citrix Content Collaboration offers the flexibility to choose the location where files are stored at rest, either inside a Citrix-managed cloud-native storage repository or inside a storage repository managed by the customer.

Citrix Content Collaboration consists of three primary components: the Content Collaboration management plane, the storage zones, and the Citrix Files apps.

-  **Management plane**: a Citrix-managed component hosting the Content Collaboration services and business logic, hosted in either the United States or the European Union.
-  **Storage zones**: the location where customer files are stored. Customers have a choice in where to store files, either hosted by Citrix or hosted by the customer in their own data center or on their own public cloud service subscriptions. This reference architecture focuses on a customer-managed storage zone hosted inside the customer data center.
-  **Citrix Files**: native apps providing access to the Content Collaboration services. Citrix Files apps are available for Windows, macOS, iOS and Android, in addition to Outlook and Gmail.

![Citrix Content Collaboration overview](/en-us/tech-zone/design/media/reference-architectures_customer-storage-zones-on-premises_001.png)

## Customer-managed storage zone architecture

The customer-managed storage zone stores all the files objects uploaded to the Content Collaboration service. It also provides access to these file objects, both to employees and external people collaborating on these files. The third and final function of the storage zone is to provide access to existing repositories hosted on-premises, such as network file shares, SharePoint Server document libraries, and the OpenText Documentum document management system. To provide those capabilities, the storage zone includes these components.

-  **Storage zone controllers**: Windows-based web servers that host the storage zone controller services.
-  **Storage repository**: the location where the Citrix files objects are stored. This location is typically a network file share, but can also be a public cloud storage location such as Amazon S3, Microsoft Azure Storage or Google Cloud Storage.
-  **Citrix ADC**: the application delivery controller provides network load balancing, SSL offloading, and authentication services for inbound traffic to the storage zone.

### Storage zone controllers

The storage zone controller is a Windows package consisting of ASP.NET web services and background Windows services. The controller software runs on top of Microsoft Windows Server with Internet Information Services (IIS). The system requirements for a storage zone controller are located here: [/en-us/storagezones-controller/5-0/system-requirements.html](/en-us/storagezones-controller/5-0/system-requirements.html).

To reduce server overhead and attack surface, it's recommended to use Windows Server Core instances with the Internet Information Server and ASP.NET application server roles enabled.

#### Scalability

The number of storage zone controllers needed depends on how the storage zone deployment is being used. This number is impacted by various factors. These factors include, but are not limited to:

-  **Amount of new and updated files being stored**: impacts the bandwidth required between the storage zone controllers and the storage repository, to prevent file I/O queues from building up on the storage zone controller.
-  **Size of files being stored**: incoming files are uploading in multiple parts and the storage zone controllers merge these parts back together into a single file object. Merging the parts increases the CPU utilization, with larger files requiring more CPU processing power.
-  **Concurrent number of sessions**: the number of concurrent file transfer sessions to the storage zone, for either file downloads or file uploads, impacts the amount of controller hosts needed.
-  **Usage of on-premises connectors**: the metadata for files and folders in existing repositories is retrieved by the storage zone controllers in real time, when the user opens a connector or browses to a different folder.

The recommendation is to deploy at least two storage zone controllers per storage zone for high availability purposes, with the Citrix ADC providing load-balancing services for inbound traffic. The baseline figure for such a deployment is 5,000 users inside the Content Collaboration tenant, with 2,500 users per extra controller host added to the storage zone. The actual usage of the storage zone determines how many controller hosts are needed. Customer deployments range from 2 controller hosts for 250 users, where the users are working with large files, to 4 controller hosts for 250,000 users where the use case is limited to occasional file sharing.

Internal testing has shown that the optimal maximum number of controllers per storage zone is 4. For deployments where more controllers are needed in a single site, it's recommended to set up multiple storage zones.

See the [Performance Monitoring](#performance-monitoring) chapter for further details.

#### Connectors

The storage zone connectors functionality allows employees to securely access existing repositories. Supported repositories are network file shares, SharePoint Server document libraries and OpenText Documentum. Users are requested to authenticate with their Active Directory credentials when accessing a configured connector. The storage zone controller uses AD Impersonation to access the underlying repositories on behalf of the user. For this, all storage zone controller hosts need to be domain members and delegation needs to be configured in the Active Directory to allow the hosts to access those resources.

### Storage repository

#### Network file share

A network file share is the most commonly used repository for customer-managed storage zones. The network file share can be hosted on a separate file server or directly on a storage repository that supports this capability. It's recommended to use a low latency, high IOPS NAS (NAS) to host the Content Collaboration repository to provide the best user experience for file transfers. The storage zone creates multiple folders inside the persistent storage folder to increase the total number of files that can be stored inside the repository. It's recommended to use modern file servers to be able to use SMBv3 for file transfers between the storage zone controller hosts and the storage zone repository.

**Important**: Using the replication capability of Microsoft's *Distributed File System* (DFS-R) is not supported. DFS-R locks the files for replication, which potentially breaks the successful upload of files to the storage zone. All files being uploaded to the storage zone are split in multiple parts, when all parts have arrived and are validated, the storage zone controllers merge these parts into a single file. With the individual parts being locked by DFS-R, the storage zone controllers cannot complete this action and the upload fails.

![Customer-managed storage zone with on-premises repository](/en-us/tech-zone/design/media/reference-architectures_customer-storage-zones-on-premises_002.png)

#### Public cloud storage

Instead of using a network file share, a public cloud storage repository can be used to store the file objects. The storage zone controller hosts use the native APIs to perform file transfers. In this architecture, a separate network file share is being used as a local cache. The local cache stores the latest files being uploaded to or downloaded from the storage zone. Experience learns that recently used files are often synced to different devices or shared with other people. Not having to retrieve those files from the public cloud storage repository each time increases the user experience and reduce the amount of bandwidth being used between storage zone and cloud repository.

![Customer-managed storage zone with cloud repository](/en-us/tech-zone/design/media/reference-architectures_customer-storage-zones-on-premises_003.png)

#### Encryption of file objects

The storage zone allows configuring encryption of all file objects on a file level. Encryption is performed by the storage zone controller hosts by using an AES 256-bit encryption key that's generated during the initial setup of the storage zone. Enabling encryption impacts the performance of the storage zone controller hosts, as encrypting and decrypting file objects increase CPU utilization.

It's recommended to only use this option when encryption of all files must be enabled according to the corporate security policy and no other means to encrypt the storage layer are available. Encryption by the storage zone controller hosts removes the ability to perform data deduplication on the persistent storage folder inside the storage zone. It will also make recovering data impossible when rebuilding the storage zone without access to the SCKeys.txt file containing the encryption key or the configuration passphrase for the storage zone.

### Citrix ADC

The application delivery controller is being used to front the storage zone. The Citrix ADC performs the following capabilities:

-  **SSL offloading**: incoming SSL sessions for the storage zone are terminated on the ADC. Depending on the security and monitoring requirements of the customer, traffic between the ADC and storage zone controller hosts can then either be encrypted or unencrypted.
-  **Load balancing**: traffic is load balanced between the storage zone controller hosts by the ADC to provide high availability. To determine which storage zone controller hosts are operational, the ADC monitors the results of the heartbeat on each host. Hosts that have an invalid response to the heartbeat are considered offline and no file transfer sessions are directed towards those hosts.
-  **Authentication**: users must authenticate when accessing connectors. It's best practice to perform this authentication inside the DMZ and not let remote users go inside the LAN unauthenticated. By configuring Kerberos Constrained Delegation on the Citrix ADC, local users accessing connectors from domain-joined devices are authenticated seamlessly without being required to provide credentials.
-  **Content Switching**: traffic to the Citrix files repository and connectors differs from each other: connectors require authentication and traffic to the Citrix files repository is always without authentication. This is due to allow sharing files with external users, who don't have credentials to authenticate with the Active Directory of the customer. Due to this difference, the ADC performs content switching to separate this traffic and apply the authentication policies to the load balancer for connectors traffic. On the load balancer for Citrix files traffic a responder policy is configured to validate the authenticity of the request: only requests originating with a request from the Content Collaboration management plane are allowed through to the storage zone controller hosts.

#### Separating local and remote user sessions

To provide the best possible user experience for local users, the configuration on the Citrix ADC is separated for local and remote users. Local users are considered to be accessing the storage zone either from a domain-joined device or the Citrix Files apps on iOS and Android managed with Citrix Endpoint Management. Another reason to deploy internal and external interfaces on the ADC is to increase security. The Citrix ADC is located inside the DMZ, with the external interface having an IP address inside the DMZ range and the internal interface having an IP address inside the local range. To allow local users to access the correct IP address, a **split DNS** needs to be configured as the storage zone has only a single, public DNS name and certificate.

Local users access the storage zone from an internal load-balancing server or content switching server with an internal IP address. The choice for either a load-balancing server or content switching server is based on whether authentication for connectors needs to occur on the Citrix ADC. In that case a content switching server is needed and the load-balancing server for connectors for local users uses Kerberos to authenticate the users when accessing connectors. Remote users access the storage zone from an external content switch with an IP address in the DMZ range. The load-balancing server for connectors for remote users uses Basic Authentication (user name and password) to authenticate users when accessing connectors. Authentication for connectors on the storage zone controller hosts uses Windows Authentication, the connectors repositories use the authentication method configured on the repository. For SharePoint Server this is typically Kerberos authentication, for file servers that's typically NTLM authentication.

![Single Citrix ADC deployment](/en-us/tech-zone/design/media/reference-architectures_customer-storage-zones-on-premises_004.png)

**Note**: instead of configuring an internal and external content switching server on a single ADC sitting on the boundary of the DMZ and local network, it's also possible to configure a Citrix ADC inside the DMZ for external users and a Citrix ADC on the local network for internal users.

![Dual Citrix ADC deployment](/en-us/tech-zone/design/media/reference-architectures_customer-storage-zones-on-premises_005.png)

### Integration with antivirus solutions

All files uploaded to the storage zone should be scanned for virusses and malware. Due to the separation of the file metadata from the file object, scanning for and removing infected or suspicious files directly from the storage zone is not recommended. While this will remove the file, the file metadata persists and the user will see the file inside their Citrix Files apps and the WebUI. When they try to access the file, an error message is displayed that the file doesn't exist.

To provide a better user experience and still maintain the required level of security, the storage zone can apply antivirus solutions through an ICAP interface. Upon receiving new and updated files, the storage zone controller hosts add the file to the scanning queue. The file is sent via ICAP to the antivirus servers and after scanning the file the antivirus servers return the result. The storage zone controller hosts upload this status to be added to the file metadata stored inside the Content Collaboration management plane. Based on the configured antivirus policy for the tenant account, users will then be restricted in the ability to download or share files that are flagged infected.

### Performance monitoring

The storage zone controller is an ASP.NET web application running on the Internet Information Server. Monitoring the performance of the storage zone controller host follows the general principles of monitoring any web server running ASP.NET applications. Below are recommended metrics to identify performance issues.

#### Memory

-  **Available Mbytes**: It is not recommended to drop below a sustained value of 20% of total available RAM.
-  **Pages Input/Sec**: It is not recommended to exceed 15. The lower the value the better the performance of the storage zone controller.
-  **Pages/Sec**: Cannot exceed a sustained value of 5 or higher.

#### Logical Disk

-  **% Disk Time**: It is not recommended to exceed a sustained value of 50% or higher.
-  **Average Disk Queue Length**: It is not recommended to exceed the number of spindles plus 2.
-  **Average Disk Read Queue Length**: It is not recommended to be below 2.
-  **Average Disk Write Queue Length**: It is not recommended to be below 4.
-  **Average Disk Sec/Read**: The average is not recommended to be 15 ms or lower, with maximums of 25 ms.
-  **Average Disk Sec/Write**: The average is not recommended to be 15 ms or lower, with maximums of 25 ms.
-  **Average Disk/Transfer**: It is not recommended to be below 20 ms.

#### Processor

The processor performance monitors cannot be done against the system, and for the W3WP process running the storage zone controller app. If the values are close to each other, then the web services are consuming the CPU, if not then other processes are impacting the performance of the system.

-  **% Privileged Time**: Cannot exceed a sustained value of 75% or higher.
-  **% Processor Time**: Cannot exceed a sustained value of 85% or higher.

#### System

-  **Content Switches/Sec**: Cannot exceed 15,000/second per CPU.

#### Web Service Cache

-  **Kernel:URI Cache Hits %**: Cannot drop below 80%.

#### ASP.NET Apps v4.0.30319 (_Total_)

-  **Request Wait Time**: Cannot exceed 1,000 ms.

## Summary

Citrix Content Collaboration offers the ability to store files in the location that fits your organization's needs best. This document describes the architecture to deploy the Content Collaboration storage zone in your own data center and all the components needed.

The use case of the Content Collaboration service in your organization impacts the components needed:

-  The place where persistent file objects are stored, either on-premises or in a public cloud storage repository.
-  Access to the Content Collaboration storage repository and existing repositories through storage zone connectors.
-  The location and device types of the users accessing the storage zone, and the kind of external collaboration.

The described architecture covers the most common deployment model our customers implement together with the Citrix Services teams. The most common use case is to access both the Content Collaboration storage repository and existing repositories through storage zone connectors.

## References

[Resources for Citrix Content Collaboration](/en-us/citrix-content-collaboration.html)

[Resources for storage zone controller](/en-us/storage-zones-controller/5-0.html)
