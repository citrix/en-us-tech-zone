---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Gabe Carrejo, Miguel Contreras
description: Discover the use cases and learn about the detailed architecture of the Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.
tz_title: Remote PC Access
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: Remote PC Access

## Overview of Remote PC Access

The Citrix Remote PC Access solution enables end users to securely access their physical windows desktops and laptops in their office from anywhere and from any device using the full stack of HDX capabilities for the best user experience.

The Virtual Delivery Agent (VDA) which is installed on the office PC, registers its states with the Cloud Connector (Citrix Cloud) or Delivery Controller (on-premises). It allows administrators to manage the physical PCs within the VDI implementation including access, policy, and full HDX stack of user experience capabilities.

A user can have access to multiple desktops, including more than one Remote PC Access or a combination of Remote PC Access and VDI desktops. This solution is an extension of Citrix Virtual Apps and Desktops, so providing and managing remote access for users to their office PCs is as simple as it is for virtual applications and desktops.

## Sample use-cases for Citrix Remote PC Access solution

In this section, we discuss the use cases where organizations can plug in Citrix Remote PC Access or replace an existing cumbersome remote access solution with Citrix Remote PC Access to deliver the best user experience for remote access.

## Use-Case #1

A customer who has deployed an on-premises Citrix Virtual Apps and Desktops solution is also keen to enable remote access for their end-users by providing connectivity to their office PCs when users need to work from home. To fulfill the requirement, Citrix administrators typically publish the RDP client application from a Citrix Virtual Apps solution, which allows the user to access the RDP client application via an HDX connection. The user types-in their desktop IP address or machine name on the RDP client, then the user authenticates to establish the RDP connection from the Virtual Apps Server. This connection is then using the Virtual Apps server as a protocol transition proxy server to access the physical PC and required resources instead of end-to-end single protocol.

The customer can replace the restrictive RDP proxy solution with the Remote PC Access HDX solution and securely integrate within an existing Citrix Virtual Apps and Desktops solution. Using the Remote PC Access, users are able to access the Office PCs with single sign-on via the secure HDX connection eliminating multiple authentication prompts. Also, it allows the enterprise administrators to control the office PC access by applying the restricted HDX policies and allowing the HDX access only to the assigned desktop. The restricted HDX policies help to enable/disable the Clipboard Redirection, Printer Redirection, and Client-Drive Mapping.

## Use-Case #2

An enterprise organization has a VPN solution to allow their employees to access the enterprise network remotely. To achieve this requirement, the network administrator has enabled VPN tunnels with dual-factor authentication. After the VPN authentication, users then create their own RDP links to access the machines which reside on the LAN. Users access the remote desktop and applications via the VPN secure connection. In this solution, administrators need to apply Network Access Controls to ensure users are connecting from allowed systems and apply policies to enable/disable certain protocols to restrict the data access. Organization security policy insists that the user connection will be allowed only when pre-authentication scans are valid in remote user’s PCs, and sometimes require certain OS patch levels to maintain a secure perimeter from viruses and malware. The organization has found that the users are unhappy with the VPN solution due to frequent disconnection and rejection of VPN connections when antivirus updates occur and other security failures.

The IT team can implement the Citrix Remote PC Access solution to replace the VPN/RDP solution. They can deploy a dedicated Remote PC Access site for allowing users to access their allocated office physical PCs. Using Remote PC Access, users are able to seamlessly access their Office PCs over an HDX connection with the adequate SmartAccess security policies which disable the client drive, clipboard mapping and printer connections. The Citrix HDX policies allow the enterprise administrators to control the user access to the desktop and their data by preventing the Key-logging and screen capture technologies.

## Use-Case #3

An existing Citrix Cloud Virtual Apps and Desktops Service customer who is looking to gain more value from Citrix to enable exciting new ways to deliver remote access to Office PCs alongside their existing Citrix deployment.

To achieve the above requirement, the IT administrator can deploy the Remote PC Access solution to enable remote access to Office PCs and integrate within the existing Citrix deployment. To enable the access for end users, the Citrix administrator can create a Remote PC Access Machine Catalog and Delivery Group on Citrix Cloud and assign the machines to respective users. This allows the end users to access their Office PCs using the existing Citrix URL. The existing Citrix HDX and SmartAccess policies still allow the Citrix administrators to control the access to the office desktops and their data.

## Use-Case #4

An enterprise customer opted to deploy the Remote PC Access solution to provide access to office PCs. Citrix Remote PC Access was considered since it is easier to migrate their office PCs to Citrix VDI during the PC refresh cycle so that the organization can save capex costs.

With VDI, organizations can avoid an upgrade or hardware acquisition costs associated with a PC refresh by extending the use of current hardware and eliminating countless IT hours spent on managing them. Migrating to cloud-based VDI (DaaS) has the additional benefit of helping to improve cost savings, reduce administrative investments and provide a workspace of the future.

## Conceptual architectures for Remote PC Access on Citrix Cloud

### Remote PC Access via Citrix Cloud with Workspace and Gateway service

In this architecture, the control plane is hosted on Citrix Cloud and managed by Citrix along with the Workspace and Gateway Service which enables the users to connect the Remote PC Access via the Citrix Cloud environment.

The conceptual architecture for the Citrix Remote PC Access deployment is shown below. Let’s review the design framework of the Remote PC Access solution for both Citrix Cloud and on-premises deployments regarding each layer on this architecture to understand the workflow of the Remote PC Access solution.

[![RemotePC-RA-Image-1](/en-us/tech-zone/design/media/reference-architectures_remote-pc_001.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_001.png)

### Remote PC Access via Citrix Cloud with Workspace and on-premises Gateway

In this architecture, the control plane is hosted on Citrix Cloud and managed by Citrix along with Workspace. The on-premises Gateway is included to enable the users to connect to the Remote PC Access solution over the internet.

[![RemotePC-RA-Image-2](/en-us/tech-zone/design/media/reference-architectures_remote-pc_002.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_002.png)

### Remote PC Access via Citrix Cloud with on-premises Gateway and StoreFront

In this architecture, the control plane is hosted on Citrix Cloud and managed by Citrix along with Workspace. The on-premises StoreFront and Gateway which enables the users to connect to the Remote PC Access over the internet via the on-premises StoreFront and Gateway.

[![RemotePC-RA-Image-3](/en-us/tech-zone/design/media/reference-architectures_remote-pc_003.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_003.png)

The conceptual architecture for the Citrix Remote PC Access deployment was discussed above. Let’s review the design framework of the Remote PC Access solution for Citrix Cloud regarding each layer on this architecture to understand the workflow of Remote PC Access.

### User Layer

This layer describes the end user for the Citrix environment and the end-point devices that are used to connect to office resources.

Users connect to their office PCs remotely over the Internet by using the Citrix Remote PC Access solution. Users use their personal devices like desktops, laptops, and tablet devices to connect to their office PC hence it is recommended to install the latest Citrix Workspace app client on the personal endpoint devices. Also, users can utilize the HTML5 version of Workspace in cases where they cannot install the full version of Workspace app on the devices.

Users navigate to the Citrix Cloud Workspace URL `https://customer.cloud.com` via the browser to access the office PC from their endpoint device over the internet. The login page is presented to the user to validate their identity using various authentication methods. Once authenticated, the user is presented with the resources page where the assigned applications and desktop are shown. The user clicks the **Remote PC Access Desktop** icon to launch the desktop. Citrix Workspace app which is installed on their endpoint device launches the desktop and provides the seamless and optimal HDX experience as if the user was working from the office.

[![RemotePC-RA-Image-4](/en-us/tech-zone/design/media/reference-architectures_remote-pc_004.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_004.png)

### Access Layer

This layer describes how the end users connect to the Citrix Remote PC Access environment and it provides design details of access methodology via Citrix Cloud, resource location connectivity, Citrix Gateway, and StoreFront requirements for on-premises access methodology.

**Citrix Workspace platform** is a foundational component of Citrix Cloud that enumerates and delivers all the digital workspace resources to the users. Users access the Workspace, which is the Citrix Cloud hosted portal that presents users with their resources. They do so by navigating to their company’s cloud.com URL (for example `https://customer.cloud.com`) or a custom URL. Once there, users are prompted to enter their credentials to gain access to their resources.

Workspace supports various authentication methods: Active Directory, two-factor authentication with Active Directory + one-time password, and Azure AD. More authentication options are being added in the future. For more details, refer to the Workspace [documentation](/en-us/citrix-cloud/workspace-configuration.html).

The Cloud Connector is a component that is installed at the resource location to connect the resources up to Citrix Cloud. A set of Cloud Connectors installed at the resource location enables access to the customer’s Active Directory domain on Citrix Cloud for authentication. Workspace configuration has multiple options to configure various authentication methods and access flow for the users that includes the traditional on-premises Citrix Gateway and StoreFront that can be used to access the environment.

When the users access the Citrix Cloud workspace URL (`https://customer.cloud.com`), they are asked to enter the Active Directory domain credentials along with the various authentication methods, then it is validated against their on-premises Active Directory domain via Cloud Connector.

[![RemotePC-RA-Image-5](/en-us/tech-zone/design/media/reference-architectures_remote-pc_005.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_005.png)

Once the credentials are validated, users are then presented with the workspace resources page, where they can access the virtual apps, desktops, and Remote PC Access resources which are assigned. When the user selects the Remote PC Access Desktop (Office PC) to launch, the user connects using the Workspace app through SSL to the Gateway with HDX. The HDX connection is established from the user’s personal device to the Citrix Gateway Service on the Citrix Cloud.

**Citrix Gateway Service** provides a secure remote access solution with diverse Identity and Access Management (IdAM) capabilities, delivering a unified experience into SaaS apps, heterogeneous Virtual Apps and Desktops, Remote PC Access and so forth. The Gateway service then establishes the connection to on-premises Cloud Connectors via SSL and it connects to the Remote PC Access Desktop via TCP port 1494/2598 to provide a seamless HDX experience.

### Control Layer

This layer describes details surrounding the management components used to support and control the Citrix environment, which includes site design for the Citrix Cloud services. For the Citrix environment, delivery controllers, SQL database, Studio, Director, and Licensing are the core components in the Control layer and those are provisioned on Citrix Cloud and managed by Citrix.

[![RemotePC-RA-Image-6](/en-us/tech-zone/design/media/reference-architectures_remote-pc_006.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_006.png)

The cloud-provisioned delivery controllers communicate with on-premises Cloud Connectors to update the resource status and Active Directory for authentication. Citrix Administrators use the Citrix Cloud portal to manage the Virtual Apps and Desktops environment and entitlements. The **“Manage”** button on the Virtual Apps and Desktops page allows admins to launch Citrix Studio to administer the environment. Using Citrix Studio, the Machine Catalog and Delivery Group for Remote PC Access are created along with Citrix Policies to secure the environment.

[![RemotePC-RA-Image-7](/en-us/tech-zone/design/media/reference-architectures_remote-pc_007.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_007.png)

The “Monitor” tab in the Citrix Cloud portal allows Citrix admins to access the Citrix Director console to monitor the app and desktop infrastructure with session control, reporting, alerting, and more.

### Resource Layer

This layer captures information about the resources which is accessed by the end user from the Citrix environment.

The Resource Layer is focused on where all the Office PCs reside in the deployment and it is called a Resource Location on Citrix Cloud. A resource location is where the customer’s Citrix workload and other operation tools reside, whether that’s a public or private cloud, a branch office, or a data center. Resource locations contain different resources depending on which Citrix Cloud services the customer is using and the services that they want to provide to subscribers.

Citrix Cloud can have multiple resource locations for a cloud subscription. The Citrix Virtual Delivery Agent, which is installed on Office PCs, registers the state of the PC with Cloud Connectors. Cloud Connectors help in updating the resource status to the delivery controllers on Citrix Cloud. The network administrator configures the necessary firewall rules for Office PCs to communicate with Cloud Connectors.

[![RemotePC-RA-Image-8](/en-us/tech-zone/design/media/reference-architectures_remote-pc_008.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_008.png)

The Citrix Administrator can create multiple Machine Catalogs and Delivery Groups for Remote PC Access to identify the Office PCs by location, department, or other factors. All these resources can be monitored using the Citrix Director console from Citrix Cloud.

### Platform Layer

This layer describes the components and cloud provisioning methods that are used for the Citrix environment focusing on hardware, storage, and virtualization details.

In this architecture for Remote PC Access, the core control infrastructure components are residing within the Citrix Cloud and managed by Citrix hence the requirement is to deploy only the Cloud Connectors in the data center so that the VDAs (Office PCs) can communicate with Cloud Connector to register its state with the Citrix Cloud.

To host the Cloud Connector virtual machines, the administrator deploys the server hardware with the required amount of resources. The Citrix administrator installs and configures the Citrix Hypervisor on the server hardware to create the Virtual Machines for Cloud Connectors. Once the Virtual Machine is created, the Citrix Admin accesses the Citrix Cloud portal from the virtual machine and installs the Cloud Connectors using their subscription account.

[![RemotePC-RA-Image-9](/en-us/tech-zone/design/media/reference-architectures_remote-pc_009.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_009.png)

### Operations Layer

This Layer focuses on the tools or components which are required to manage the Citrix workloads and Remote PC Access desktops within Resource Locations.

For the Citrix Cloud architecture, the key tool is the Cloud Portal to access the control infrastructure hosted on Citrix Cloud. The Cloud portal enables administrators to access the Citrix Studio and Citrix Director consoles. Citrix Studio helps administrators to configure the Machine Catalogs, Delivery Groups, and Citrix policies.

Citrix Director helps to monitor the complete Citrix environment. Using the Cloud portal, administrators can monitor Cloud Connector status and also it helps to configure various authentication methods and different access methodologies.

## Conceptual architecture for Remote PC Access with on-premises deployment

The conceptual architecture for the Citrix Remote PC Access with on-premises deployment is shown below.

[![RemotePC-RA-Image-10](/en-us/tech-zone/design/media/reference-architectures_remote-pc_010.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_010.png)

Let’s review the design framework of the Remote PC Access solution for on-premises deployments regarding each layer on this architecture to understand the workflow.

### User Layer

This layer describes the end user for the Citrix environment and the end-point devices that are used to connect to office resources.

Users connect to their office PCs remotely over the Internet by using the Citrix Remote PC Access solution. Users use their personal devices like desktops, laptops, and tablet devices to connect to their office PC hence it is recommended to install the latest Citrix Workspace app client on the personal endpoint devices. Also, users can utilize the HTML5 version of Workspace where they cannot install the full version of the Workspace app on the devices.

Users navigate to the on-premises Citrix Gateway URL `https://citrix.company.com` via the browser to access the office PC and other resources from their endpoint device over the internet. The login page is presented to the user to validate their identity using multifactor authentication. Once authenticated the user is presented with the resources page where the assigned applications and desktops are shown. The user clicks the **Remote PC Access Office Desktop** icon to launch the desktop. Citrix Workspace app which is installed on their endpoint device launches the desktop and provides the seamless and optimal HDX experience as if the user was working from the office.

[![RemotePC-RA-Image-11](/en-us/tech-zone/design/media/reference-architectures_remote-pc_011.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_011.png)

### Access Layer

This layer describes how the end users connect to the Citrix Remote PC Access environment and it provides design details of access methodology for an on-premises deployment.

Users access the existing Citrix Gateway URL (`https://citrix.company.com`) which was configured for the Citrix Virtual Apps and Desktops solution to access the Remote PC Access as well. When navigating to the Citrix Gateway URL, users are presented with a login page with multiple authentication methods including Active Directory. Citrix Gateway supports various authentication methods, refer to the product documentation for complete details.

[![RemotePC-RA-Image-12](/en-us/tech-zone/design/media/reference-architectures_remote-pc_012.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_012.png)

Once the credentials are validated, users are then presented with the traditional Citrix StoreFront/Workspace resources page where they can access the Virtual Apps, Desktops, and Remote PC Access which are assigned. When the user selects the Remote PC Access (Office PC) to launch, the user connects using the Workspace app through SSL to the Gateway with HDX. The HDX connection is established from the user’s personal device to the on-premises Citrix Gateway with SSL and then it connects to the Office PC via TCP port 1494/2598 to provide a seamless HDX experience.

### Control Layer

This layer describes details surrounding the management components used to support and control the Citrix environment, which includes site design for the on-premises Citrix environment.

The Control Layer for an on-premises deployment includes all infrastructure related components supporting the overall Citrix solution which includes the Citrix Delivery controllers, SQL Database, and Licensing.

[![RemotePC-RA-Image-13](/en-us/tech-zone/design/media/reference-architectures_remote-pc_013.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_013.png)

Existing Virtual Apps and Desktops deployments can be easily configured with Remote PC Access by just creating the Machine Catalog and Delivery Groups by selecting Remote PC Access.

Customers also have the option to have a dedicated environment for Remote PC Access by deploying the new delivery controllers with a new Remote PC Access Site, licensing, and SQL database which can be integrated with the existing StoreFront and Citrix Gateway for unified and seamless access. Using Citrix Studio, the Machine Catalog and Delivery Group for Remote PC Access are created along with Citrix Policies to secure the environment.

[![RemotePC-RA-Image-14](/en-us/tech-zone/design/media/reference-architectures_remote-pc_014.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_014.png)

### Resource Layer

The Resource Layer captures the information about where the Office PCs reside in the enterprise network and how these machines can be configured for Remote PC Access.

In this architecture, the Office PC resides on the LAN segment at the customer environment. Those Office PCs are installed with Virtual Delivery Agents (VDAs) to register with on-premises Delivery Controllers. The Citrix Administrator can configure a Remote PC Access Machine Catalog and Delivery Group to enable access for the end users.

Deploying the VDA can be managed by existing Electronic Software Delivery (ESD) systems, like a Microsoft System Center Configuration Manager (SCCM). Best practice for upgrades is to reboot, uninstall the VDA software, reboot, install the latest VDA, then reboot a final time.

[![RemotePC-RA-Image-15](/en-us/tech-zone/design/media/reference-architectures_remote-pc_015.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_015.png)

Citrix Director enables the Citrix admin to monitor the Remote PC Access environment along with their existing Citrix Virtual Apps and Desktop environment.

### Platform Layer

This layer describes the hardware components and cloud provisioning methods that are used for the Citrix environment mainly focusing on hardware, storage, and virtualization details.

For the on-premises environment, the platform layer covers the server hardware requirements to host the core control components. The core components include two or more delivery controllers, a License Server, two VMs for the SQL Database to configure the Cluster or Always On, a VM for Citrix Director and other components.

[![RemotePC-RA-Image-16](/en-us/tech-zone/design/media/reference-architectures_remote-pc_016.png)](/en-us/tech-zone/design/media/reference-architectures_remote-pc_016.png)

To host these components, the enterprise administrator has deployed the server hardware with the ample amount of resources. The Citrix administrator installed and configured the Citrix Hypervisor on the server hardware to create the Virtual Machines for all the components. Once the Virtual Machines are created, the Citrix Administrator has configured the Remote PC Access Site. StoreFront is configured with the new delivery controllers to enumerate the Remote PC Access resources for the end users.

### Operations Layer

This Layer focuses on the tools and components which are required to manage the Citrix workloads and Remote PC Access desktops within Resource Locations.
For the on-premises environment, the operations layer focuses on the tools like Citrix Studio and Citrix Director which helps in controlling the infrastructure and monitoring the complete Citrix environment.

Citrix Studio helps administrators to create multiple machine catalogs and delivery groups and apply Citrix HDX policies for the Remote PC Access solution. Citrix Director helps to monitor the environment.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s71653ee85184a339).

## References

[Technical Requirements and Considerations](/en-us/citrix-virtual-apps-desktops/install-configure/remote-pc-access.html#technical-requirements-and-considerations)

[Remote PC Access Security Considerations](/en-us/citrix-virtual-apps-desktops/secure/best-practices.html#remote-pc-access-security-considerations)

[Remote PC Access Use Case Video](https://www.youtube.com/watch?v=dyE-OEzbsWU)
