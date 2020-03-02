---
layout: doc
---
# Citrix Cloud Virtual Apps and Desktops Service Reference Architecture and Deployment Methods

## Contributors

**Author:** [Vivekananthan Devaraj](mailto:Vivekananthan.Devaraj@Citrix.com)

**Special thanks:** [Martin Zugec](https://twitter.com/MartinZugec), [Allen Furmanski](https://twitter.com/tekguyallen), [James Hsu](https://twitter.com/JamesHsu2Go), and [Carlos Jimenez](mailto:carlos.jimenez1@Citrix.com)

## Audience

This document is intended for IT decision-makers, consultants, solution integrators, and partners who are seeking to deploy or expand their existing Citrix Apps & Desktops deployment solutions with Citrix Cloud.

## Objective of this document

When designing a virtual apps and desktops solution using Citrix Cloud, several considerations are starting from the type of desktops and apps required, to how users access the desktops and apps with their data, how to build an environment that can scale along with an organization’s growth and expansion, and how the solution is made highly available to meet business needs.

This reference architecture focuses on providing a conceptual view of the solution, including what is Citrix Cloud and its deployment models for the Citrix Virtual Apps and Desktops Service. The goal is to ensure positive user experience and maximize the availability of services while integrating with hybrid-cloud workloads.

## What is Citrix Cloud?

Citrix Cloud is a cloud-based platform comprised of various service offerings. Many of these services function as a management plane that is kept evergreen by Citrix with the workloads and data residing in the data center or cloud of the customer’s choice. This approach allows customers to focus on the most strategic part of IT and resource delivery with the security, availability, and functionality that business demands.
At the time of writing, Citrix Cloud offers the below services:

*  Virtual Apps and Desktops
*  Endpoint Management
*  Gateway
*  Application Delivery Management
*  Web AppFirewall
*  ADC VPX
*  Content Collaboration
*  Access Control
*  Analytics
*  App Layering
*  ITSM Adapter
*  Intelligent Traffic Management
*  Secure Browser
*  Workspace Environment Management

These services can be accessed together as an integrated "workspace" or independently.

[![CVAD-Image-1](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_001.PNG)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_001.PNG)

### Why Citrix Cloud?

Citrix service commitment is to maintain at least 99.5% monthly uptime. The status of the platform and its services are accessible at [https://status.cloud.com](https://status.cloud.com). Citrix Cloud-hosted services are managed by Citrix experts and are updated continuously, so IT doesn’t have to worry about large-scale platform upgrades resulting in a secure, evergreen environment that saves time and reduces costs. With Citrix Cloud, customers can move to a more predictable subscription cost of opex rather than an often unpredictable and potentially expensive CAPEX model. Citrix Workspace provides an integrated experience to access applications and content from any device, anywhere, at any time. Users benefit from a seamless, engaging work experience regardless of the type of app, device, network, or location they connect from.

## Citrix Cloud Virtual Apps and Desktops Service (CVADS)

A traditional Citrix deployment for apps and desktops consists of delivery controllers, StoreFront servers, a highly available SQL database, Studio and Director consoles, a License Server, and Citrix Gateway. These components are part of the management plane or control plane for the environment and are deployed at a data center or cloud that is a customer or partner-managed. The resources for end-users made available from server and desktop Virtual Delivery Agents (VDAs) are also hosted on dedicated hypervisors within datacenter(s) and/or on private or public clouds. These components are called the Citrix workloads. Both management components and workloads are managed entirely by the customer or perhaps a partner.

In the Citrix Cloud Virtual Apps and Desktops Service, the management or control plane for a customer deployment is provisioned and managed by Citrix on Citrix Cloud. Customers don’t handle the core product installation, setup, configuration, upgrades, monitoring, or scaling of the management plane as that is all left to Citrix to handle and keep evergreen and secure.

[![CVAD-Image-2](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_002.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_002.png)

All of the control plane components - StoreFront, Delivery Controllers, even the SQL database are made highly available and part of the cloud service offering. Customers can focus on the workload resources of the server and desktop VDAs hosted on the hypervisor or cloud of their choice. Each workload location defined to a hypervisor or cloud with specific resources is known as a Resource Location in Citrix Cloud.

The Cloud Connector is a new component that is installed in the resource location to connect the resources up to Citrix Cloud. It is placed next to the VDAs, within the hypervisor(s) or public cloud(s), and the Active Directory environment. Citrix Cloud Connector designed for seamless integration and to deliver the best user experience on any device under any network condition.

Citrix Workspace Experience, an enhanced version and successor of StoreFront in Citrix Cloud, is the industry’s first solution offering the integration of Windows, Linux, Web, SaaS, and mobile applications in a unified and simple-to-use interface. Citrix Workspace fully aggregates apps and data from both on-premises and cloud environments to deliver the required resources with the right experience to the right user at the right time. With this enhanced architecture, customers still own and maintain complete control of over-provisioned resources like desktops, applications, policies, and users using the Citrix Cloud Portal.

## Generic & Conceptual Architecture

The Citrix Virtual Apps and Desktops architecture is divided up into layers. All layers flow together to create a complete, end-to-end solution for an organization.

[![CVAD-Image-3](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_003.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_003.png)

*  **User Layer** - This layer describes the end-user environment and end-point devices that are used to connect to resources. This section also explains the use case for Citrix Workspace and Citrix Workspace app.

*  **Access Layer** - This layer describes details surrounding external and internal access to the Citrix environment. This section provides design details for virtual networks, resource location connectivity, Citrix Gateway, and StoreFront placement and configuration.

*  **Resource Layer** - This layer captures information for the user’s personalization, applications, and images for the Citrix environment.

*  **Control Layer** - This layer describes details surrounding the components used to support the rest of the environment, which includes site design for the Citrix Cloud Services, Cloud Connector, and Active Directory integration.

*  **Platform Layer** - This layer describes the hardware components, private, public, and hybrid cloud that will be used for the Citrix environment – hardware, storage, and virtualization details.

*  **Operations Layer** – This layer explains the procedures and tools that support the core product and solution.

## Preparing for Citrix Cloud

### Sign up for Citrix Cloud

To sign up for Citrix Cloud, customers need a Citrix.com account or MyCitrix account to manage, access and allocate licenses for Citrix Cloud services. This account is known as the Citrix Cloud Account. The account uses an organization ID (OrgID) as a unique identifier for each customer, and it is associated with a physical site address (typically a customer’s organization address). To begin with, Citrix Cloud, navigate to [https://citrix.cloud.com](https://citrix.cloud.com) and create an account or sign in with a Citrix account to activate the trails of the Cloud Services.

A Citrix Cloud account allows admins to have broad administrative access on the services; therefore, Citrix expects that the first admin who creates the Citrix Cloud account has to explicitly give access to other admins as required - even if the other admin is already a member of the existing MyCitrix account.

### Citrix Cloud Region

A Citrix Cloud Region is a geographical boundary where Citrix operates, stores, and replicates services and data for delivery of Citrix Cloud services. When a customer is onboarded to Citrix Cloud and signs in for the first time, they are asked to choose one of the following regions:

*  The United States
*  European Union
*  Asia Pacific South

The customer has to choose a region that maps to where most users and resources are located. Citrix Cloud Services are used on a global basis. All services are globally available, regardless of the region, a customer selects for their organization. Certain services, like the Virtual Apps and Desktops Service, have dedicated regional instances; however, some services have US-based instances only.

An important point to note is that admins can choose a region only once, and it cannot be changed later. Customers who opted for the US Region with Cloud Connectors in Australia see minimal impact from latency however.

### Resource Locations

A resource location is where the customer’s Citrix workload and other operation tools reside, whether that’s a public or private cloud, a branch office, or a data center. Resource locations contain different resources depending on which Citrix Cloud services the customer is using and the services that they want to provide to subscribers. Typical resources include:

*  Active Directory domains
*  Citrix ADC appliances
*  Hypervisors
*  Virtual Delivery Agents (VDAs)
*  StoreFront servers
*  Citrix Cloud Connectors

A primary resource location is a resource location that the customer designates as “most preferred” for communications between the customer domain and Citrix Cloud. The resource location selected as “primary” should have adequate Cloud Connectors that have the best performance and connectivity to the customer AD domain, Citrix workloads, and resources. Designating the primary resource location allows users to log on quickly and access the Citrix Cloud environment. There is no restriction on the number of resource locations that a customer can have.

Setting up resource locations begins with the following:

*  Set up or configure Active Directory
*  Set up your host (hypervisor/cloud) environment

[![CVAD-Image-4](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_004.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_004.png)

Active Directory on each resource location is required to access and verify the authentication requests via the Cloud Connector. In case the primary resource location is not available, the authentication requests are handled from other resource locations from where the Cloud Connector is connected with the Active Directory Domain.

Reference: [*Citrix Docs: Resource Location*](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)

### Citrix Cloud Identity and Access Management

Citrix Cloud Identity and Access Management define the identity providers and accounts used for administrators and subscribers (end users) to Citrix Cloud and its offerings.

### Identity Providers

Citrix Cloud uses the Citrix owned identity provider to manage the identity information of all users in a Citrix Cloud account. Customers can change this to use Azure Active Directory or on-premises Active Directory.

### Subscribers

A subscriber’s (end-user) identity defines the services to which they have access in Citrix Cloud. This identity comes from Active Directory domain accounts provided from the domains within the resource location. Assigning a subscriber to a Library offering (resources like desktops and apps) authorizes the subscriber to access that offering.

Admins can control which domains are used to provide these identities on the Domain panel. If a customer is planning to use domains from multiple forests, they need to install at least two Cloud Connectors in each forest to maintain a highly available environment.

Reference: [*Citrix Docs: Identity and Access Management*](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html)
Reference: [*Citrix Docs: Library Offerings and User Assignment*](/en-us/citrix-cloud/citrix-cloud-management/assign-users-to-offerings-using-library.html)

### Active Directory

Active Directory is a vital role for authentication and authorization with Citrix Cloud. The Kerberos infrastructure in Active Directory is used to guarantee the authenticity and confidentiality of communications with the cloud-based Delivery Controllers. Connecting a customer’s Active Directory domain to Citrix Cloud can be achieved via two methods:

#### Connecting Azure AD to Citrix Cloud

Customers who have integrated their Active Directory with Azure AD, can now easily connect their Azure AD with Citrix Cloud. Once the Azure AD is connected with Citrix Cloud, it helps users to log in seamlessly. Citrix Cloud includes an Azure AD app that allows Citrix Cloud to connect with Azure AD without the need for users to log in for a new Azure AD session.

[![CVAD-Image-5](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_005.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_005.png)

By using Azure AD with Citrix Cloud, customers can:

*  Leverage their own Active Directory to control auditing, password policies, and easily disable accounts when needed
*  Configure multifactor authentication for a higher level of security
*  Use a branded sign-in page, so users know they’re signing in at the right place
*  Use federation to an identity provider of your choice including ADFS, Okta, Ping, and others

#### Connecting an On-Premises AD Domain to Citrix Cloud

Connecting the resource location (on-premises) Active Directory to Citrix Cloud involves installing Cloud Connectors in the customer’s Citrix domain environment. Citrix highly recommends installing a minimum of two Cloud Connectors for high availability. Once the Cloud Connectors are installed, the resource location domain is populated and enabled on Citrix Cloud for user/ subscriber authentication.

[![CVAD-Image-6](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_006.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_006.png)

### What is a Cloud Connector?

The Citrix Cloud Connector is an essential component in a resource location that serves as a channel for communication between Citrix Cloud and resource locations. It enables cloud management without requiring any complex networking or infrastructure configuration. In a resource location, the Cloud Connector acts as a proxy for the Delivery Controller provisioned on Citrix Cloud. This removes all the hassle of the customer managing the delivery infrastructure.

Citrix recommends installing the Cloud Connector on a machine running Windows Server 2016 or Windows Server 2012 R2. This Connector machine must be joined to the customer domain and be able to communicate with the resources that the customer wants to manage from Citrix Cloud. In each resource location, two or more Cloud Connectors are recommended to support the required load and ensure high availability.

#### Load Balancing the Cloud Connectors

The Citrix Cloud Connector is stateless, and the load can be distributed across all available Cloud Connectors. To manage adequate load, install multiple Cloud Connectors in each resource location. There is no need to configure load balancing functionality as it is automated. The health of the Cloud Connectors can be monitored via the Citrix Cloud portal.

#### Cloud Connector Functions

The Cloud Connector enables the following functions for the Citrix Cloud Virtual Apps and Desktops Service:

*  Active Directory: Enables AD management, allowing the use of AD forests and domains within resource locations. It removes the need for adding any additional AD trusts.
*  Virtual Apps and Desktops publishing: Enables publishing resources from all resource locations.
*  Delivery group provisioning: Enables provisioning of machines directly into resource locations.

[![CVAD-Image-7](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_007.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_007.png)

#### Cloud Connector Communication

The Cloud Connector authenticates and encrypts all communication between Citrix Cloud and resource locations by utilizing the Internet connection available in the resource location. The Cloud Connector also supports connections to Citrix Cloud through the Internet via a web proxy server. The Web proxy needs to understand Connect tunneling and WebSocket persistent connections. During the installation, both the installer and the services it installs, need communication with Citrix Cloud. Internet access needs to be available at both these points.

All communication between the Cloud Connector and Citrix Cloud is “outbound.” All connections are established from the Cloud Connector to the Citrix Cloud control plane using the standard HTTPS port (443) with the web standard TLS 1.2 protocol. No incoming connections are accepted. The Cloud Connector cannot transverse domain-level trusts; therefore, additional Cloud Connectors should be installed per user domain. It is essential to understand that Citrix Cloud only stores metadata, such as user names, application names, and icons, while the corporate data and resources remain within each resource location configured. Nonetheless, all data between Citrix Cloud and the Cloud Connectors is encrypted with TLS while in transit.

Enabling SSL decryption on certain proxies might prevent the Cloud Connector from connecting successfully to Citrix Cloud. For more information, refer to [CTX221535](https://support.citrix.com/article/CTX221535).

As long as there is at least one Cloud Connector available, there will be no loss in communication with Citrix Cloud from a resource location; however, to handle the required load, customers must ensure they have adequate connectors installed and available all the time. The end user’s connection (HDX) to the resources in the resource location does **not** rely on a connection to Citrix Cloud in specific deployment methods, wherever possible. This enables the resource location to provide users access to their resources regardless of a connection being available to Citrix Cloud.

Reference: [*Citrix Docs: Cloud Connector Internet Connectivity Requirements*](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html)

### Zones

In a Citrix Virtual Apps and Desktops Service environment, **each resource location is considered a Zone**. When customers create a resource location and install a Cloud Connector, a zone is automatically created. Each zone can have a different set of resources based on an organization’s unique needs and the type of environment.

Citrix Virtual Apps and Desktops Service deployments that span widely dispersed locations connected by a WAN might face challenges from network latency and reliability. Zones can help users in remote regions to connect to resources without necessarily forcing their connections to traverse large segments of the WAN.

Zones can be helpful in deployments of all sizes. Customers can use zones to keep applications and desktops closer to users, which improves performance. Zones can be used for disaster recovery, geographically distant data centers, branch offices, a cloud, or an availability zone in a cloud.

Zones in a Citrix Virtual Apps and Desktops Service environment are not identical to zones in an on-premises Citrix Virtual Apps and Desktops deployment. In the Citrix Virtual Apps and Desktops Service, zones are created automatically when customers create a resource location and add a Cloud Connector to it. Unlike an on-premises deployment, a service environment does not classify zones as primary or satellite. In XenApp version 6.5 and earlier, each zone had an assigned data collector server that handled dynamic information about all servers in the zone, such as load levels. The Citrix Virtual Apps and Desktops Service do not use data collectors for zones. Also, failover and preferred zones work differently.

[![CVAD-Image-8](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_008.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_008.png)

#### Why Zones?

Placing Citrix components in a zone affects how the service interacts with them and with other objects related to them.

*  When a hypervisor connection is placed in a zone, it is assumed that all the hypervisors managed through that connection also reside in that zone.
*  When a machine catalog is placed in a zone, it is assumed that all VDAs in the catalog are in the zone.
*  Citrix Gateway instances can be added to zones. While creating a resource location, it is offered with the option to add a Citrix Gateway. When a Citrix Gateway is associated with a zone, it is preferred for use when connections to VDAs in that zone are used. Ideally, Citrix Gateway in a zone is used for user connections coming into that zone from other zones or external locations.
*  After creating more resource locations and installing Cloud Connectors in them (which automatically creates more zones), customers can move resources between zones. This flexibility comes with the risk of separating items that work best in close proximity. For example, moving a catalog to a different zone than the connection (host) that creates the machines in the catalog can affect performance. So, consider potential unintended effects before moving items between zones. Keep a catalog and the host connection it uses in the same zone.
*  If the connection between a zone and Citrix Cloud fails, the Local Host Cache feature enables a Cloud Connector in the zone to continue brokering connections to VDAs in that zone (note the zone must have StoreFront installed). For example, this is effective in an office where workers use the local StoreFront site to access their local resources, even if the WAN link connecting their office to the corporate network fails.

#### VDA Registration in a Zone

A VDA in a zone registers with a local Cloud Connector.

*  As long as that Cloud Connector can communicate with Citrix Cloud, normal operations continue.
*  If that Cloud Connector is operational but cannot communicate with Citrix Cloud (and that zone has a local StoreFront), it enters Local Host Cache outage mode.
*  If a Cloud Connector fails, VDAs in that zone attempt to register with other local Cloud Connectors. A VDA in one zone never attempts to register with a Cloud Connector in another zone.

Adding or removing a Cloud Connector in a zone is done using the Citrix Cloud management console if auto-update is enabled, VDAs in that zone receive the updated lists of available local Cloud Connectors to register and accept connections.

When moving a machine catalog to another zone using Studio, the VDAs in that catalog re-register with Cloud Connectors in the zone where moved. In this scenario, ensure to move any associated host connection to the same zone.

#### Zone Preference

In a multi-zone site environment, the zone preference feature offers the administrator more flexibility to control which VDA is used to launch an application or desktop. This feature can be utilized for disaster recovery situations to provide access to resources effortlessly.

There are three zone preference options which can be used for session launch:

*  Application home – the zone where application-related data is stored
*  User Home – the zone where user profile or home drive data is stored
*  User Location – the zone that is close by to the user

#### Selection of Preferred Zone

When a launch request for a desktop or application is received by a delivery controller, it must determine a ‘preferred’ zone to use when finding a VDA to satisfy the request. A preference order is now used to select a single zone from those available. The default order is the first application home zone, then the user home zone, then the user location zone. So, the selection of the preferred zone is:

*  For application launches, if the application has a home zone, then this is the preferred zone.
*  Otherwise, if the user has a home zone, then this is the preferred zone.
*  Otherwise, if a user location zone is provided, then this is the preferred zone.

#### How preferred zones affect session use

When a user launches an application or desktop, the broker prefers using the preferred zone rather than using an existing session.

If the user launching an application or desktop already has a session that is suitable for the resource being launched (for example, that can use session sharing for an application, or a session that is already running the resource being launched), but that session is running on a VDA in a zone other than the preferred zone for the user/application, then the system may create a new session. This satisfies launching in the correct zone (if it has available capacity), ahead of reconnecting to a session in a less-preferred zone for that user’s session requirements.

To prevent an orphaned session that can no longer be reached, reconnection is allowed to existing disconnected sessions, even if they are in a non-preferred zone.

The order of desirability for sessions to satisfy a launch is:

1.  Reconnect to an existing session in the preferred zone.
2.  Reconnect to an existing disconnected session in a non-preferred zone.
3.  Start a new session in the preferred zone.
4.  Reconnect to an existing connected session in a non-preferred zone.
5.  Start a new session in a non-preferred zone.

Reference: [*Citrix Docs: Citrix Cloud–Zones*](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html)

### Local Host Cache

The Local Host Cache (LHC) is a feature with Cloud Connectors that enables users to continue to work by handling the connection brokering operations in a Citrix Virtual Apps and Desktops Service deployment when a Cloud Connector cannot communicate with Citrix Cloud. Local Host Cache engages when the network connection is lost between the Cloud Connector and Citrix Cloud for 20 seconds. With Local Host Cache, users who are connected can continue working uninterrupted. Reconnections and new connections experience minimal connection delays.

LHC uses SQL Server Express LocalDB, which is installed during Cloud Connector installation to store the required data for uninterrupted service during an outage. A Cloud Connector’s CPU configuration, particularly the number of cores available to the SQL Server Express LocalDB, directly affects Local Host Cache performance. CPU overhead is observed only during the outage period when the database is unreachable, and the High Availability Service is active. Citrix recommends using multiple sockets with multiple cores for Cloud Connector machines. In Citrix testing, a 2 socket, 3 core configuration provided better performance than 4x1 and 6x1 configurations. Local Host Cache works only in resource locations containing an on-premises StoreFront.

For customers requiring a higher level of resiliency, Citrix recommends the StoreFront with the Local Host Cache deployment model.

[![CVAD-Image-9](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_009.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_009.png)

Reference: [*Citrix Docs: Local Host Cache*](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html)

#### How to Secure the Cloud Connector?

Refer to the [secure deployment guide](/en-us/citrix-cloud/overview/secure-deployment-guide-for-the-Citrix-cloud-platform.html) on Citrix Docs.

#### Sizing Guidelines

Refer to the [sizing guidelines](/en-us/citrix-cloud/advanced-concepts.html) for Cloud Connector and Local Host Cache on Citrix Docs.

### Virtual Delivery Agent

Physical or virtual machines that deliver applications and desktops to users must have the Citrix Virtual Delivery Agent (VDA) software installed. The VDA registers with a Cloud Connector, and users’ connections are brokered through a connector to those VDA resources. VDAs establish and manage the connection between the machine and the user device; during the connection launch or reconnection, it applies policies that are configured for the session.

The VDA communicates session information to the Cloud Connector through a broker agent in the VDA. The broker agent hosts multiple plug-ins and collects real-time data.

VDAs are available for Windows and Linux server and desktop operating systems. VDAs for Windows and Linux server operating systems allow multiple users to connect to the server at one time. VDAs for Windows and Linux desktop operating systems allow only one user to connect to the desktop at a time.

Reference: [*Citrix Docs: Virtual Delivery Agent*](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html)

### RDS License

Citrix Virtual Apps environments require an RDS (Remote Desktop Services) CAL. Citrix Virtual Apps environments run on top of VMs running Windows Server 2019/2016/2012R2 with Microsoft RDS technology (formerly Terminal Services) and thus need to be licensed with RDS CALs (per user or device).

The VDA must be able to contact an RDS license server to request RDS CALs. Administrators should install and activate the license server in the resource locations.

### Machine Management

There are no compute resources to host VDAs for users on Citrix Cloud. Hypervisors that are deployed in on-premises data centers or resources in customer-owned Cloud Services (resource locations) are used to provision the VDAs required for the environment. This is to ensure that customers can control and implement all the required business and technical requirements. To provision multiple virtual machines and to manage those virtual machines for updates and changes required for environments, customers can use:

*  **Machine Creation Services:** The MCS technology is built into the Studio management console and is accessed through the Citrix Cloud portal. MCS creates copies of a master image to provision VMs in resource locations on hypervisors or public clouds using disk-based cloning.
*  **Citrix Provisioning:** The Citrix Provisioning technology streams a master image to multiple target devices on hypervisors over the network. Cloud Connector plug-ins are used to communicate with Citrix Provisioning deployments from within the Citrix Cloud Console.

### Citrix Federated Authentication Service

The Federated Authentication Service (FAS) is a Citrix component that integrates with your Active Directory certificate authority (CA), allowing users to be seamlessly authenticated within a Citrix environment. When enabled, the FAS delegates user authentication decisions to trusted StoreFront servers. The StoreFront has a comprehensive set of built-in authentication options designed around modern web technologies and is easily extensible using the StoreFront SDK or third-party IIS plug-ins. The fundamental design goal is that any authentication technology that can authenticate a user to a website can now be used to log in to a Citrix Apps and Desktops deployment.

The FAS is authorized to issue smart card class certificates automatically on behalf of Active Directory users who are authenticated by StoreFront. This uses similar APIs to tools that allow administrators to provision physical smart cards. When a user is brokered to a Citrix Virtual Delivery Agent (VDA), the certificate is attached to the machine, and the Windows domain sees the logon as a standard smart card authentication.

This allows Windows authentication without prompts to enter user credentials or smart card PINs, and without using “saved password management” features such as the single sign-on service. This can be used to replace the Kerberos Constrained Delegation logon features available in earlier versions of XenApp.

To enable FAS together with the Citrix Cloud, the following Citrix components must be built outside of the Citrix Virtual Apps and Desktops Service:

*  FAS servers
*  Citrix StoreFront (minimum version 3.6)
*  Server OS / Desktop OS VDAs (minimum version 7.9)
*  (optional) Citrix Gateway may be required if configured as a SAML IdP or SP to front the StoreFront servers configured for FAS authentication, but Citrix ADC is not required for FAS itself

The above components may be built in a public cloud or in an on-premises data center. The FAS and StoreFront servers should be configured according to Citrix documentation just as they would for a fully customer-owned solution to support FAS authentication, including such items as applying Group Policy settings and configuring Microsoft Certificate Authority servers.

Reference: [*Citrix Docs: Citrix FAS*](/en-us/xenapp-and-xendesktop/7-15-ltsr/secure/federated-authentication-service/fas-architectures.html)

### Citrix Cloud components

### Delivery Controller

The Delivery Controller is the central control layer component in a deployment that is provisioned and managed by Citrix on Citrix Cloud. The Controller’s services communicate through the Cloud Connectors in each resource location to:

*  Distribute applications and desktops.
*  Authenticate and manage user access.
*  Broker connections between users and their virtual desktops and applications.
*  Optimize user connections and load balance these connections.
*  Track which users are logged on and where, which session resources the users have, and if users need to reconnect to existing applications.
*  Manage the state of desktops, starting and stopping them based on demand and administrative configuration.

### SQL Database

Data from the Delivery Controller services is stored in a Microsoft SQL Server site database on Citrix Cloud. A deployment also uses a Configuration Logging database, plus a monitoring database used by Director. The SQL Server services and the databases required for deployment are managed by Citrix; hence customers do not have the visibility of SQL configuration and manageability.

### Citrix License

License management functionality communicates with the Controller to manage to license for each user’s session and allocate license files. The administrator does not need to configure or manage any licensing, and this is done automatically in Citrix Cloud. The License Usage tab can help admins to monitor the license usage in Citrix Cloud.

License Usage in Citrix Cloud enables admins to stay on top of license consumption for the cloud services that they have purchased. Using the summary and detail reports, they can:

*  View license availability and assignments at a glance
*  Drill down to see individual license assignment details and usage trends
*  Export license usage data to CSV

### Citrix Studio

The studio is the management console to configure and manage connections, to create machine catalogs, and Delivery Groups. The studio can be launched via the Manage tab on the Citrix Cloud console.

### Citrix Policies

Citrix policies are the most efficient method of controlling connection, session, security, and bandwidth settings. Citrix policies enable admins to create a collection of settings that define how sessions, bandwidth, and security are managed for a group of users, devices, or connection types.

With the Virtual Apps and Desktops Service administrators have the option to configure Citrix policies via Citrix Studio or through Active Directory Group Policy Management Console using Citrix ADMX files, which provide advanced filtering mechanisms to select the required settings to optimize the performance, bandwidth and to implement security measures.

All Citrix Local Policies are created and managed in the Citrix Studio console and stored in the Site Database. Group Policies are created and managed by using the Microsoft Group Policy Management Console (GPMC) and stored in Active Directory. Microsoft Local Policies are created in the Windows Operating System and are stored in the registry.

When creating policies for groups of users, devices, and machines, some members might have different requirements and would need exceptions to some policy settings. Exceptions are made by way of filters in Studio and the GPMC that determine who or what the policy affects.

#### Policy Processing Order and Precedence

Group policy settings are processed in the following order:

1.  Local GPO
2.  Virtual Apps and Desktops Site GPO (stored in the Site database)
3.  Site-level GPOs
4.  Domain-level GPOs
5.  Organizational Units

However, if a conflict occurs, policy settings that are processed last can overwrite those that are processed earlier. This means that policy settings take precedence in the following order:

1.  Organizational Units
2.  Domain-level GPOs
3.  Site-level GPOs
4.  Virtual Apps and Desktops Site GPO (stored in the Site database)
5.  Local GPO

#### Citrix Policy Workflow

In Studio, policy settings are sorted into categories based on the functionality or feature they affect.

*  **Computer Settings:** The policy settings applying to machines define the behavior of virtual desktops and are applied when a virtual desktop starts. These settings apply even when there are no active user sessions on the virtual desktop.

*  **User Settings:** The policy settings are applying to the user and define the user experience when connecting using the Citrix ICA protocol. User policies are applied when a user connects or reconnects using ICA. User policies are not applied if a user connects using Microsoft RDP or log in directly to the console.

### Policy Templates

Built-in Citrix Policy templates that are readily available with predefined settings optimized for specific environments or network conditions. These templates can be used as a source to create policies with predefined settings for customer environments.

The following policy templates are available:

*  **Very High Definition User Experience:** This template enforces default settings that maximize the user experience. Use this template in scenarios where multiple policies are processed in order of precedence.
*  **High Server Scalability:** Apply this template to economize on server resources. This template balances user experience and server scalability. It offers a good user experience while increasing the number of users you can host on a single server. This template does not use video codec for compression of graphics and prevents server-side multimedia rendering.
*  **High Server Scalability-Legacy OS:** This High Server Scalability template applies only to VDAs running Windows Server 2008 R2 or Windows 7 and earlier. This template relies on the Legacy graphics mode, which is more efficient for those operating systems.
*  **Optimized for Citrix SD-WAN:** Apply this template for users working from branch offices with Citrix SD-WAN for optimizing the delivery of Virtual Apps and Desktops.
*  **Optimized for WAN:** This template is intended for task workers in branch offices using a shared WAN connection or remote locations with low bandwidth connections accessing applications with graphically simple user interfaces with little multimedia content. This template trades off video playback experience and some server scalability for optimized bandwidth efficiency.
*  **Optimized for WAN-Legacy OS:** This Optimized for WAN template applies only to VDAs running Windows Server 2008 R2 or Windows 7 and earlier. This template relies on the Legacy graphics mode, which is more efficient for those operating systems.
*  **Security and Control:** Use this template in environments with low tolerance to risk, to minimize the features enabled by default in Citrix Virtual Apps and Desktops. This template includes settings that disable access to printing, clipboard, peripheral devices, drive mapping, and port redirection. Applying this template may use more bandwidth and reduce user density per server.

### StoreFront / Workspace

Citrix StoreFront is an interface for users to access Citrix Virtual Apps and Desktops from the office or remotely with multiple devices. It acts as an enterprise self-service app store for users and enables administrators to provide users with self-service central access to their virtual desktops and applications. Citrix's StoreFront enables single sign-on access to users to which provides flexibility for the users to access the virtual apps and desktops.

It also keeps track of users’ application subscriptions, shortcut names, and other data to ensure they have a consistent experience across multiple devices. With the Citrix Cloud Virtual Apps and Desktops Services, the StoreFront deployment is flexible for customers to achieve their organization requirements and use cases.

1.  **On-Premises StoreFront**: Customers may also use an existing on-premises StoreFront to aggregate applications and desktops in Citrix Cloud. This use case offers greater security, including support for two-factor authentication, and prevents users from entering their password into the cloud service. The benefit of using an existing StoreFront is that the Citrix Cloud Connector provides encryption of user passwords. Credentials are encrypted by the connector using AES-256 with a random-generated one-time key. This key is returned directly to the Citrix Workspace app and never sent to the cloud. Citrix Workspace app then supplies it to the VDA during session launch to decrypt the credentials and provide a single sign-on experience into Windows. It also allows customers to customize their domain names and URLs. To use the Local Host Cache feature on-premises StoreFront is a requirement.

2.  **Cloud-hosted StoreFront / Workspace**: The Virtual Apps and Desktops Service in Citrix Cloud hosts a StoreFront / Workspace service for each customer. The benefit of the cloud-hosted StoreFront is that there is zero effort to deploy, and it is kept evergreen by Citrix. The customer is provided with a cloud-based URL to access the apps and desktops.

3.  A **combination** of both on-premises StoreFront and cloud-hosted StoreFront

**Note:** For customers requiring a higher level of resiliency, Citrix recommends the StoreFront with the Local Host Cache deployment model.

### Workspace Configuration

Citrix Workspace platform is a foundational component of Citrix Cloud that enumerates and delivers all digital workspace resources to the Citrix Workspace user with the best experience. Workspace Configuration enables admins to:

*  Easily modify and customize the workspace URL to be company-specific and modify the external connectivity options for each service
*  Select the best authentication option for user sign-on
*  Customize the look and feel
*  Manage services available to users
*  Aggregate sites from traditional on-premises Apps and Desktops deployments

Reference: [*Citrix Docs: Workspace Configuration*](/en-us/citrix-cloud/workspace-configuration.html)

### Citrix Gateway Service

Citrix Gateway provides users with secure remote access to Citrix Virtual Apps and Desktops applications across a range of devices, including laptops, desktops, thin clients, tablets, and smartphones. The Citrix Gateway Service enables secure, remote access to Citrix Virtual Apps and Desktops applications, without having to deploy Citrix Gateway in the DMZ or reconfiguring the customer-owned on-premises firewall.

The entire infrastructure overhead of using Citrix Gateway moves to the cloud and is hosted by Citrix. After enabling the service, users can access their VDAs from outside their network. The Citrix Gateway Service is enabled for use with HDX traffic as part of the Virtual Apps and Desktops Service only. Other Citrix Gateway functionality is not enabled.

*  The Citrix Cloud Connector located in a resource location communicates with Citrix-managed cloud services through the internet. This communication channel does not support authentication at outbound proxies
*  All network traffic is protected by SSL, but to provide the Citrix Gateway functionality, HDX traffic is present in memory in an unencrypted form
*  To use the Citrix Gateway Service, the customer must use StoreFront hosted within Citrix Cloud
*  SmartAccess does not work for sessions connected through the Citrix Gateway service.

[![CVAD-Image-10](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_010.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_010.png)

Citrix Gateway Service is ideally suited to address these following use cases:

1.  Customers are looking to refresh on-premises Gateway to support the on-premises deployment of Citrix Virtual Apps and Desktops environments. Customers using basic ICA Proxy can now use Citrix Gateway Service for basic ICA Proxy to on-premises Virtual Apps and Desktops.
2.  Customers are looking to subscribe to Citrix Cloud Virtual Apps and Desktops service. Customers can now use Citrix Gateway Service for SSO to the Citrix Virtual Apps and Desktops Service, along with on-premises Apps and Desktops, any web, and SaaS applications.
3.  Customers are looking to consolidate their access management solutions. Customers using Citrix Gateway for their Citrix Apps and Desktops solutions/services, and are using a third-party cloud service for SSO to SaaS and web applications, can now use the Citrix Gateway Service for SSO to all applications. They don’t need to have two different access management solutions, thereby reducing costs, providing a better end-user experience, and enforcing consistent access control policies across all applications.

#### Citrix Gateway Service Features

*  **Simplicity:** Reduce ADC deployment and management complexity
*  **Always Current:** Simplify management of Citrix Gateway with an always up-to-date product
*  **Security and High availability:** Improve security and availability of the Apps and Desktops Service
*  **Speed:** Provide a faster and easier way to deploy and manage Citrix Gateway
*  **Convenience:** Gateway services packaged and sold together to simplify meeting the use cases that IT most commonly faces

Reference: [*Citrix Docs: Citrix Gateway Service*](/en-us/citrix-virtual-apps-desktops-service/netscaler.html)

### Director

Director is a monitoring and troubleshooting console for the Citrix Virtual Apps and Desktops Service. The Director dashboard provides a centralized location for monitoring the real-time and historical health and usage of a Site. Director functionality is available on the Monitor tab of the Citrix Virtual Apps and Desktops Service console.

Administrators and help-desk personnel can use the Director as a real-time web tool to monitor, troubleshoot, and perform support tasks for subscribers. It helps admin to get the details of User sessions and sessions in use, logon performance, user connections, and machines that include failures, load evaluation, historical trends, and monitor infrastructure hosts.

Reference: [*Citrix Docs: Citrix Director*](/en-us/xenapp-and-xendesktop/7-15-ltsr/director.html)

### Delegated Administration

Delegated Administration in Citrix Cloud helps to configure the access permissions with a different set of roles for admins to manage and monitor their Citrix environment.

#### Delegated Administration uses three concepts for custom access

**Administrators:** An administrator represents a person identified by their Citrix Cloud sign-in, which is typically an email address. Each administrator is associated with one or more roles and scope pairs.

**Roles:** A role represents a job function, and has permissions associated with it. These permissions allow specific tasks that are unique to the service. The service offers several built-in custom access roles. Admins cannot create other custom access roles and cannot change the permissions within these built-in custom access roles, or delete those roles. They can change which roles an administrator has. A role is always paired with a scope.

**Scopes:** A scope represents a collection of objects. Scopes are used to group objects in a way that is relevant to your organization. Objects can be in more than one scope. There is one built-in scope: All, which contains all objects. Citrix Cloud and Help Desk administrators are always paired with the All scope.

Reference: [*Citrix Docs: Delegated Administration*](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/delegated-administration.html)

### Host Connection

A host connection enables the communication between Citrix Cloud components in the control plane and to a hypervisor or cloud service in a resource location. Connection specifications include:

*  The hypervisor or cloud service address and credentials to access those resources
*  Tools used to create VMs
*  The storage method used to provision VMs
*  Network segments for VMs

Reference: [*Citrix Docs: Hosting Connections*](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)

### Machine Catalog

A machine catalog is a collection or grouping of VDAs that have the same operating system type, either Server or Desktop. A machine catalog containing Server OS machines can contain either Windows or Linux machines, not both.

A virtual machine that is created on the hypervisor or on the cloud service named as a master image is also known as a template. This VM has all the user applications and other security tools Installed. Finally, the Citrix Virtual Delivery Agent is installed that allows Citrix tools to create multiple VMs from this master image via Citrix Studio.

Once the template or master image is ready, create a machine catalog using a Citrix tool (MCS or Citrix Provisioning) or customer-owned other tools. With Citrix tools, the machine catalog creation process provisions identical VMs from that image. If there are other tools to provision VMs or physical machines, the catalog creation process adds those machines to the catalog.

Studio guides the admin to create the first machine catalog. After the first catalog, Studio guides the admin to create the first Delivery Group to provision the desktops and applications for users. Alternatively, admins can use the Citrix Cloud console to add subscriptions in the library.

### Delivery Group

A Delivery Group is a collection of machines selected from one or more machine catalogs. The Delivery Group can specify which users can access those desktop machines, and which applications are available to those users. Alternatively, admins can specify those users and applications through the Citrix Cloud console.

Creating a Delivery Group is the next step in configuring the Citrix deployment after creating the first machine catalog. The Delivery Group has additional features and settings that can be configured after creating it.

### Citrix App Layering

The Citrix Cloud portal provides a management console for customers to manage the on-premises Enterprise Layer Manager VMs of Citrix App Layering.

Citrix App Layering is a Windows Operating System and application management solution designed for on-premises, private clouds, and public clouds. Citrix App Layering's underlying technology, called layering, enables all components of a virtual machine to be independently assigned, patched, and updated. This includes the Windows OS, applications, and user settings and data. Built around this core innovation is a management system that encompasses application conflict resolution, image creation, application assignment, and integration technologies that can be used for any virtualization platform.

Citrix App Layering uses a single virtual appliance to manage the layers and then hands them off to other platforms for image and application distribution.

Citrix App Layering radically reduces the overhead involved in managing Windows applications and images. Regardless of which hypervisor or provisioning solution a customer uses, the App Layering service allows:

*  To install the operating system, platform tools, and applications in separate layers
*  To select the combination of layers needed for each of the images in an image template, then using the image template to provision systems for groups of users
*  To elastically assign specific App layers for on-demand delivery to users when they log in

This reduces the number of images to maintain. The App Layering solution works for both desktops and session host servers. Layer types in this context are the OS layer, Platform Layer, App layer, and User Layer.

### Workspace Environment Management Service

The Workspace Environment Management (WEM) Service provides intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Windows Virtual Apps and Desktops deployments. It is a software-only, driver-free solution.

*  **Resource management:** To provide the best experience for users, Workspace Environment Management monitors and analyzes user and application behavior in real-time, then intelligently adjusts RAM, CPU, and I/O in the user workspace environment.

*  **Profile management:** To deliver the best possible logon performance, WEM replaces commonly used Windows Group Policy Object objects, logon scripts, and preferences with an agent that is deployed on each Virtual Delivery Agent or virtual machine or server. The agent is multi-threaded and applies changes to user environments only when required, ensuring that users always have access to their desktop and apps as quickly as possible.

*  **Simplified setup:** The Workspace Environment Management service eliminates most of the admin setup tasks required by the on-premises version of WEM. Admins can access the web-based administration console via the Citrix Cloud portal to fine-tune WEM related tasks in the infrastructure.

[![CVAD-Image-11](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_011.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_011.png)

### Profile Management

Citrix Profile Management ensures that the user’s personal settings are applied to the user’s virtual desktop and applications, regardless of the location and endpoint device.

Citrix Profile Management is enabled through a profile optimization service that provides an easy, reliable way for managing these settings in Windows environments to ensure a consistent experience by maintaining a single profile that follows the user. It auto-consolidates and optimizes user profiles to minimize management and storage requirements and requires minimal administration, support, and infrastructure while providing users with improved logon and logout.

Citrix Profile Management addresses user profile deficiencies in environments where simultaneous domain logons by the same user introduce complexities and consistency issues to the profile. For example, if a user starts sessions to two different virtual resources based on a roaming profile, the profile of the session that terminates last overrides the profile of the first session. This problem, known as “last write wins” discards any personalization settings that the user makes in the first session.

At login, users’ registry entries and files are copied from the user store. If a locally cached profile exists, the two sets are synchronized. This makes all settings for all applications and silos available during the session. And it is no longer necessary to maintain a separate user profile for each silo. Citrix streamed user profiles can further enhance logon times.

Profile Management optimizes profiles in a secure and reliable way. At interim stages and at logoff, registry changes, and files and folders in the profile, are saved to the user store for each user. If, as is typical, a file exists, it is overwritten if it has an earlier timestamp.

Some user settings and data can be redirected by folder redirection. However, if folder redirection is not used, these settings are stored within the user profile.

## Planning for a Citrix Cloud Deployment

The Citrix Cloud Virtual Apps and Desktops Service(CVADS) can be deployed in several ways depending on organization requirements and use cases. Citrix Cloud provides flexibility and enables customers to choose the deployment model for their requirements. Below are a few conceptual reference architectures that can help customers with their needs and their use case(s):

*  **CVADS with Hybrid-Cloud/On-Premises Citrix workloads**
*  **CVADS and Gateway Service with On-Premises Citrix workloads**
*  **CVADS with On-Premises Gateway, StoreFront, and Citrix workloads**

## CVADS with Hybrid-Cloud/On-Premises Citrix Workloads

In this deployment scenario, a company wants to enable its internal users to access their resources (apps and data) by using Citrix Cloud along with on-premises Citrix workloads. Since the applications are not cloud-ready, they are hosted in their on-premises data center and managed by their internal admins. These applications need to be enabled via Citrix for the users. The company does not have any IT specialists to deploy the Citrix management components, so they have decided to go with Citrix Cloud.

As a future expansion, the company is also planning to extend its on-premises workloads to the public cloud to offload the resource-intensive web components and to scale the Citrix workloads in the cloud. The company decided to go with this deployment model as it can provide higher availability for business continuity and disaster recovery, provided they extend their Citrix workloads to any private or public cloud. In case of any disaster and if users are unable to access the resources from on-premises data centers, they can quickly enable the resources and provide access from cloud-hosted Citrix workloads.

The conceptual architecture for this deployment is shown below. Let’s review the design framework of each layer on this deployment to understand how it delivers a complete solution for the company.

[![CVAD-Image-12](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_012.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_012.png)

**User Layer:** Users within corporate who needs to access the Citrix environment. The company has enabled its corporate devices to access Citrix resources. Workspace app is available for Windows, Mac, Android, and iOS. The internal IT team has installed the latest Workspace app on corporate devices. The company enabled the printer and device redirection policy for users to access their company printers and drives for data management.

The company has identified its users and segregated as different workload types based on their application usage.

|Type of Users   | App Usage  |   CPU   |  Memory |  IOPS  |      Resource     |
|----------------|------------|---------|---------|--------|-------------------|
| Task Users     | 2 to 7     | 1 vCPU  |  1 GB   |   6    | Virtual Apps      |
| Office Users   | 5 to 8     | 1 vCPU  | 1.5 GB  |   8    | Virtual Apps      |
| Knowledge Users| 6 to 10    | 2 vCPU  |  2 GB   | 10-20  | Pooled Desktop    |
| Power Users    | 8 to 12    | 2 vCPU  | 3-4 GB  | 15-25  | Dedicated Desktop |

Upon user segregation, the Citrix admin has created the respective machine catalog and Delivery groups in Citrix Studio for each workload type.

**Access Layer:** Citrix StoreFront / Workspace Service is the front end or entry point for Citrix users to access the Citrix environment. Citrix Cloud has provisioned StoreFront / Workspace to access the company’s Citrix environment, and users are provided with the Workspace URL (`https://company.cloud.com`) from Citrix Cloud. The Workspace configuration tab allowed company admins to change the Workspace URL and the customization of the Workspace / StoreFront Page.

Installation of the Cloud Connector enables extending the customer Active Directory domain to Citrix Cloud for authentication. Authentication configuration in Workspace Configuration allows the admin to select the authentication source for users to sign in and access the Citrix resources. The admin has selected on-premises Active Directory to authenticate the Citrix subscribers on Citrix Cloud. Users accessing the Citrix Cloud URL `https://company.cloud.com` are asked to enter the domain credentials, which will then be validated against their on-premises Active Directory domain via Cloud Connector.

Once the credential is validated, users are then presented with the workspace page where they can access the apps, desktops, and content that are assigned. When the user launches an application or desktop, the user is presented with an ICA file that will launch the Workspace app, and the ICA connection (TCP port 1494/2598) is established from the user system to the VDA which is assigned by the controller for this connection.

[![CVAD-Image-13](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_013.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_013.png)

**Control Layer:** Delivery controllers, SQL database, Studio, Director, and Licensing are the core components in the control layer which are provisioned on Citrix Cloud by Citrix during the activation of the Virtual Apps and Desktops Service. The Cloud Admin logs in to the Citrix Cloud portal and selects the Virtual Apps and Desktops Service to manage the components. When admins select the “manage” button on Citrix Cloud, it will launch the Studio console for them to administer the environment.

Citrix Studio helped admins to configure the Hosting Connection, Machine Catalog, Delivery Groups, Applications, and Policies for Citrix users. Admin selects the Monitor tab on the Citrix Cloud Virtual Apps and Desktop Service page to open the Citrix Director. This helps the company admins to monitor the complete Citrix environment from Citrix Cloud.

[![CVAD-Image-14](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_014.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_014.png)

**Resource Layer**: The Resource Layer is where all the Citrix workloads reside in this deployment, and it is called a Resource location in Citrix Cloud. As a starting point for integrating the on-premises Citrix workloads with Citrix Cloud, the admin installed 2x Windows Server 2016 VMs on their hypervisor and installed the Citrix Cloud Connector on this virtual machine to connect with Citrix Cloud.

The Cloud Connector installation created the resource location on Citrix Cloud, and also it added the Company.com domain as an authentication domain in Citrix Cloud for users.

With the help of Citrix Studio and using the master image templates created using App Layering, the admin has deployed 20 Virtual Apps Servers with Windows Server 2016 using Citrix Provisioning Services (PVS) for task workers, 50 virtual desktops with Windows 10 using Machine Creation Services (MCS) for knowledge workers, and 10 virtual servers with Red Hat Linux using Machine Creation Services for office users on the hypervisor.

Virtual Delivery Agents installed with these VMs are registered with Cloud Connectors; thus, the admin created machine catalog and delivery groups to provision the desktops and apps for users. Citrix policies are created and assigned using Studio for the delivery groups to optimize and secure the HDX connections.

[![CVAD-Image-15](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_015.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_015.png)

**Platform Layer:** The customer has procured the server hardware, which is required for the Citrix workload based on their requirements and design decisions. The Citrix admin has installed the hypervisor to host the Citrix workloads. The network admin has enabled the Firewall rules for the new environment on corporate devices. The storage admin helped to configure and assign adequate storage to the new Citrix environment.

**Operations Layer:** The components which are required to manage the Citrix workloads in Resource Location are covered under the Operations Layer. The IT Admin has deployed a file server cluster to store the user profiles from Citrix workloads. Also, they have deployed RDS CAL Server (Per user / device) to issue the RDS licenses for Virtual Apps Servers in the Resource Location.

Since the company has a future plan to move a few of their workloads to the public cloud, they decided to deploy the Citrix App Layering Manager in the Resource location. With the App Layering Service, they created the OS Layer, Platform Layer, and Application Layers as per the company requirement. By combining these layers, the admin has created the master image templates to provision the Citrix workloads for the users. To optimize the Citrix Windows environment, they opted to deploy the Workspace Environment Manager Service in the Resource location.

Using Workspace Environment Management, the admin has configured the user profile settings using the Citrix Profile Management solution. All the user profiles and folder redirection data for users are configured to be stored in the file server cluster hosted in the resource location. To optimize the Citrix user experience, the admin also applied the settings for CPU, memory, and logon optimization via the WEM console.

[![CVAD-Image-16](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_016.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_016.png)

## CVADS and Gateway Service with On-Premises Citrix workloads

An existing Citrix customer who has a legacy XenApp environment (on-premises) for their internal users, which is due for an upgrade as the product reached the end of life. As part of the upgrade plan, the customer wants to build a new parallel Citrix environment with the latest version of Citrix components and also wants to extend the Citrix environment access to external (Internet) users that include existing legacy XenApp since the existing setup does not have an ADC / gateway configured.

The customer wanted to aggregate the Citrix resources from both the old and new environments within this new deployment.

The customer decided to go with the Citrix Cloud Virtual Apps and Desktop Service for the control plane, and for the new Citrix workloads, they wanted to deploy those resources in a different site/datacenter considering the future growth. The customer planned to use the current data center resource(s) for higher availability and the disaster recovery site for the new site once the new hybrid cloud environment is in production with users migrated. The customer opted to utilize the Citrix Gateway Service on Citrix Cloud to enable external access for the Citrix users. The customer is also planning to implement two-factor authentication to secure the environment.

The customer pursues this deployment model as it helps them to enable the external access to users, and it provides the ability for customers to plan and mitigate the disaster recovery situations.

The **conceptual architecture** for this deployment is shown below, Let’s go through the design framework of each layer on this deployment to understand how Citrix Cloud enables the complete solution for the customer.

[![CVAD-Image-17](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_017.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_017.png)

**User Layer:** Users within corporate (internal) networks and over the internet (external) needs to access both new Citrix hybrid-cloud and old legacy Citrix environment. The IT team has enabled firewall rules on their network devices to access the new Citrix environment resources from their corporate devices. The Internal IT team has installed the latest Workspace app on the corporate devices and upgraded the old Citrix Receiver wherever applicable.

External users who are accessing the Citrix Cloud environment over the internet use their personal devices along with corporate devices; hence the company recommended installing the latest Workspace app on their personal devices. External users can also utilize the HTML5 version of the Workspace, where they cannot install the Workspace app on the devices.

User types and their workloads are identified from the existing legacy Citrix environment.

|Type of Users   | App Usage  |   CPU   |  Memory |  IOPS  |      Resource     |
|----------------|------------|---------|---------|--------|-------------------|
| Task Users     | 2 to 7     | 1 vCPU  |  1 GB   |   6    | Virtual Apps      |
| Office Users   | 5 to 8     | 1 vCPU  | 1.5 GB  |   8    | Virtual Apps      |

The Citrix admin started replicating the machine catalogs and delivery groups similar to the existing legacy environment, which are now pointing to Citrix Cloud Connectors. In addition to that, the customer wants to create additional machine catalogs and delivery groups for the knowledge and power users as per new requirements.

|Type of Users   | App Usage  |   CPU   |  Memory |  IOPS  |      Resource     |
|----------------|------------|---------|---------|--------|-------------------|
| Knowledge Users| 6 to 10    | 2 vCPU  |  2 GB   | 10-20  | Pooled Desktop    |
| Power Users    | 8 to 12    | 2 vCPU  | 3-4 GB  | 15-25  | Dedicated Desktop |

**Access Layer:** On Citrix Cloud workspace configuration, the admin has enabled the Citrix Gateway Service, which enabled the external access. Citrix Gateway is the front end entry point for all the users to access the Citrix environment. This helps corporate network admins to enable the globally approved HTTPS access towards Citrix Cloud from corporate devices.

Citrix Cloud has provisioned StoreFront / Workspace to access the Company’s Citrix environment, and users are provided with the Workspace URL (`https://customer.cloud.com`) from Citrix Cloud. The Workspace configuration tab allows company admins to change the Workspace URL and the customization of the Workspace / StoreFront Page.

Installation of Cloud Connector enables the extension of the customer’s Active Directory domain to Citrix Cloud for authentication. The authentication configuration in Workspace Configuration allows the admin to select the authentication source for users to sign-in and access the Citrix resources. The admin has selected on-premises Active Directory to authenticate the Citrix subscribers on Citrix Cloud.

Users accessing the Citrix Cloud URL `https://customer.cloud.com` is asked to enter the domain credentials, which will then be validated against their on-premises Active Directory via Cloud Connector. Once validated, the user is then presented with the workspace page where they can access the apps and desktops which are assigned.

When the user launches an application or desktop, the user is presented with an ICA file which will be launched via the Workspace app and the ICA connection (SSL) is established from user system to Citrix Gateway and then the connection is passed to Cloud connector and then (on port 1494/2598) to the VDA which is assigned by the controller for this connection. On a successful connection, the HDX session (2598) is launched and presented to the user.

[![CVAD-Image-18](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_018.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_018.png)

### Site Aggregation for Legacy Citrix Environments

To aggregate the resources from legacy Citrix environments, the admin has installed two Cloud Connectors within the legacy environment, and that allows the Cloud admin to configure the site aggregation on Citrix Cloud workspace configuration. The admin has provided the legacy environment details in the Sites tab on Workspace configuration and enabled the Service Integrations – Virtual Apps and Desktops on-premises sites allowed to aggregate the Citrix resources from the legacy Citrix environment via Citrix Cloud.

The user accessing the Citrix Cloud URL `https://customer.cloud.com` asked to enter the domain credentials which are validated against their on-premises Active Directory via Cloud Connector. Once validated, the user is then presented with the workspace page with the apps and desktops from both old and new environments.

When a user launches an application from the legacy environment, the user is presented with an ICA file, which will then be launched via the Workspace app. The workspace app initiates the ICA connection from the user system to Citrix Gateway and then to the Cloud connector and then to the VDA assigned for this connection. On a successful connection, the HDX session is launched and presented to the user.

[![CVAD-Image-19](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_019.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_019.png)

**Control Layer:** For the new hybrid-cloud environment, delivery controllers, SQL Database, Studio, Director, and Licensing are the core components in the Control layer which are provisioned on Citrix Cloud by Citrix during the activation of the Virtual Apps and Desktop Service. The admin logs in to the Citrix Cloud portal and selects the Virtual Apps and Desktops Service to manage the components. When the admin selects the “manage” button on the Virtual Apps and Desktop page, it then launches Citrix Studio for them to administer the environment.

Using Citrix Studio, the admin configured the Hosting Connection, Machine Catalog, Delivery Groups, Applications and Policies for Citrix users.

The admin selects the Monitor tab on the Citrix Cloud Virtual Apps and Desktop Service page to open Citrix Director. This helps Cloud admins to monitor the complete Citrix environment from Citrix Cloud.

There are no changes in the legacy Citrix environment. The admin has installed two new Citrix Cloud Connectors to establish the cloud communication for site aggregation and to access the resources via Citrix Cloud.

[![CVAD-Image-20](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_020.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_020.png)

**Resource Layer:** The Resource Layer is where all the Citrix workloads reside in this deployment, and it is called a Resource Location in Citrix Cloud.

As a starting point for integrating the new hybrid Citrix workloads with Citrix Cloud, the admin installed 2 x Windows Server 2016 VMs on their hypervisor, and then Citrix Cloud Connectors are installed on both the virtual machine to connect with Citrix Cloud.

The Cloud Connector installation created the resource location on Citrix Cloud, and also it added the customer.com domain as an authentication domain in Citrix Cloud for users. There are two resource location created on Citrix Cloud. One for a new hybrid-cloud solution, and the second is for the legacy Citrix environment.

With the help of Citrix Studio and using the master image templates created using App Layering, the admin has deployed 50 Virtual Apps Servers with Windows Server 2016 using Citrix Provisioning (PVS) for task workers, 100 Virtual Desktops with Windows 10 using Machine Creation Services (MCS) for Power workers, and 50 Virtual Desktops with Red Hat Linux using Machine Creation Services for Linux users on the hypervisor.

Virtual Delivery Agents installed with these VMs are registered with Cloud Connectors, so the admin created a Machine Catalog and Delivery Groups to provision the Desktops and Apps for users. Citrix Policies are created and assigned using Studio for the delivery groups to optimize and secure the HDX connections.

[![CVAD-Image-21](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_021.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_021.png)

**Platform Layer:** The Customer has procured the server hardware required for the Citrix workload based on their requirements and design decisions. The Citrix admin has installed the hypervisor to host the Citrix workloads. The network admin has enabled the Firewall rules for the new environment on corporate devices. The storage admin helped to configure and assign adequate storage to the new Citrix environment.

**Operations Layer:** The tools or components which are required to manage the Citrix workloads within Resource Location are covered under the Operations Layer. The IT Admin has deployed a file server cluster to store the user profiles from Citrix workloads.

Using Citrix Workspace Environment Manager Service, the Citrix admin has applied the Profile Management Policies to WEM Agents. The customer opted to use the latest Citrix User Profile Manager on VDAs to map the user’s profiles to the file server cluster. The IT Admin has created dedicated file shares for the user profiles and folder redirection. Profile Management policies optimize Citrix workloads for quick login and logoff.

The Citrix admin also applied resource management policies to optimize the CPU and memory utilization on VDA agents. Using the Workspace Environment Manager, the cloud admin applied application security and process management policies to control the end-user activity.

The customer has enabled Remote Desktop Services (RDS) Client Access License Server to issue the RDS licenses for Virtual Apps workloads in the Resource Location.

To manage multiple master images and update each master image with OS and application patches, the customer decided to deploy Citrix App Layering Manager in the Resource Location and enable the management plane via Citrix Cloud. With the App Layering Service, admins created OS Layers, Platform Layers, and Application layers as per the workload requirement. By combining these layers, the admin has created the master image templates to provision the Citrix workloads.

[![CVAD-Image-22](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_022.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_022.png)

### Disaster Recovery

The customer migrated their legacy Citrix environment to the new hybrid-cloud environment in a phased approach, and they successfully completed the migration in the defined timelines.

As per the design strategy, the hardware resources which were used for legacy environments were refreshed to create the disaster recovery site for the production environment. The Citrix admin has installed the hypervisor on the hardware to create Citrix workloads.

All the components which are used in a production environment are replicated to the disaster recovery site to ensure the availability of the services during a disaster.

Citrix App Layering helped to deploy the workloads by replicating the master templates, which were created in the production environment to the DR site with no effort. The Citrix Workspace Environment Manager Service helped to configure and apply the profile settings in the DR Site.

The IT admin has configured Storage replication to replicate the user profiles and data stored on the production file server cluster to DR site file servers.

The cloud admin has created a new resource location named DR-Site by installing the two Cloud Connectors in the disaster recovery site. They also configured the hosting connection, Machine Catalog, and Delivery Group, which resembles the production environment.

Once the environment is ready, the customer wants to test the DR environment with a planned disaster simulation in the production environment. During the scheduled window, the production environment Cloud Connectors were shut down, and there is no communication between the production site and Citrix Cloud.

The cloud admin has configured the zone preferences for the subscribers in Library (Apps and Desktops) offerings, which enabled the end-users to see the desktops and apps from “DR Site” on storefront/workspace page.

When a user launches an application or desktop, the user is presented with an ICA file which will be launched via the Workspace app and the ICA connection (SSL) is established from the user system to Citrix Gateway and then the connection is passed to the DR Site Cloud Connector and then (on port 1494/2598) to the VDAs which is assigned by the controller for this connection from the DR Site. On a successful connection, the HDX session (2598) is launched and presented to the user.

[![CVAD-Image-23](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_023.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_023.png)

Citrix WEM user profile configuration helped users to access their data from the DR file server to do their day-to-day activity.

## CVADS with On-Premises Gateway, StoreFront, and Citrix workloads

In this scenario, a large enterprise customer wants to deploy a Citrix Virtual Apps and Desktops Service environment for their corporate users that are spread across different regions in all over the world. This Citrix environment is focused on providing access to internal-only web applications and other resources. The customer also has a requirement to enable pooled and dedicated desktops for their web developers. The customer wants to place the Citrix environment (VDAs) close by user proximity in their region to provide the best user experience. They decided to split the Citrix workloads up and host them in their existing data center across three regions the USA, Europe, and Asia, with Citrix Cloud to manage their control plane.

The Information Security Team claims to implement either SAML based or multifactor authentication with their existing third-party solution to secure the authentication and auditing requirements. Since it is an enterprise company, users are traveling across regions, and they need to access the Citrix environment from other regions. The customer wants to have a unique URL for each region to access their environment. As part of disaster recovery planning, the customer wants to allocate 20% additional hardware and reserve those Citrix workloads for other region users to access internal-only web applications during a disaster.

The customer decided to go with this deployment model to satisfy the security requirement and to provide the best user experience. Along with Citrix workloads, the customer placed two Cloud Connectors, two StoreFront servers, and two Citrix ADC Gateways in all three resource locations. The conceptual architecture for this deployment shown below, the design framework of each layer of this deployment, explains how Citrix Cloud enables the best solution for the customer.

[![CVAD-Image-24](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_024.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_024.png)

**User Layer:** The customer has decided on these URLs for each region users to access the Citrix environment.

*  `https://amrapps.company.com` – USA users
*  `https://eurapps.company.com` – Europe users
*  `https://apjapps.company.com` – Asia users

Users both internal (within corporate) and external (over the internet) who will be accessing the Citrix environment do so using these Public URLs. The IT team has enabled firewall rules on their network devices to access the new Citrix environment resources from their corporate devices. The IT has installed the latest Workspace app on the corporate devices and upgraded the old Citrix Receiver wherever applicable.

External users may also use their personal devices along with corporate devices, so it is recommended to install the latest Workspace app on users’ personal devices. External users can also utilize the HTML5 version of Citrix Workspace using the latest web browsers where they cannot install the Workspace app on the devices.

The company has identified its users’ types and segregated as different workload types based on their application usage.

|Type of Users   | App Usage  |   CPU   |  Memory |  IOPS  |      Resource     |
|----------------|------------|---------|---------|--------|-------------------|
| Task Users     | 2 to 7     | 1 vCPU  |  1 GB   |   6    | Virtual Apps      |
| Office Users   | 5 to 8     | 1 vCPU  | 1.5 GB  |   8    | Virtual Apps      |
| Knowledge Users| 6 to 10    | 2 vCPU  |  2 GB   | 10-20  | Pooled Desktop    |
| Power Users    | 8 to 12    | 2 vCPU  | 3-4 GB  | 15-25  | Dedicated Desktop |

Upon user segregation, the Citrix admin has created the respective Machine Catalog and Delivery Groups in Citrix Studio for each workload type on each cloud region subscription.

The cloud admin has applied Citrix Policies for printer and device redirection to access their company printers and drives for data management.

**Access Layer:**

[![CVAD-Image-25](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_025.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_025.png)

**On-Premises Gateway:**

On each data center in respective regions, the customer has deployed an HA-pair of Citrix Gateway ADCs and two StoreFront servers in a group to allow the Citrix environment access for users. To enable the Citrix access via Citrix Gateway ADC, the customer has procured an SSL certificate (SAN) from the third-party Public Certificate Authority with all three DNS fully qualified domain names amrapps.company.com, eurapps.company.com, and apjapps.company.com.

The Citrix ADC admin has configured the Citrix Gateway ADCs with SSL certificates on each region. This helps corporate network admins to enable the globally approved HTTPS (443) inbound access to the Citrix environment from the outside world on each region data center with a dedicated public IP for each URL.

To achieve the multifactor authentication requirement for Citrix environment access, the customer opted to utilize the existing Azure-MFA (NPS-Plugin) solution. Each data center has two NPS-MFA servers to ensure high availability of service. The ADC admin has configured the authentication policy with Azure MFA (LDAP as primary, Azure MFA as secondary) along with session policies (pointing to on-premises StoreFront Stores) in Citrix Gateway on each region.

**On-Premises StoreFront:**

The Citrix admin has deployed two StoreFront servers to integrate with Citrix Gateway ADCs in each data center. They created a store named “VAD” in StoreFront to configure the additional settings, which enable the admin to configure the authentication settings and delivery controller for resource enumeration. The admin has added two cloud connectors as the delivery controller for each site in the store configuration.

The user accessing the Citrix URL `https://amrapps.company.com` is asked to enter the domain credentials, which will then be validated against their NPS-MFA Servers. Once validated, the user is then asked to enter the second factor, which is as per configuration on MFA for each user.

Once the authentication is successful, the on-premises StoreFront communicates with the Citrix Cloud Connector for resource enumeration. The Cloud connector communicates with cloud-hosted Delivery Controllers and then enumerate the resources. Once the enumeration is completed, the user is presented with the StoreFront page where can they can access the apps and desktops which are assigned.

When a user launches an application or desktop, they are presented with an ICA file which will be launched via the Workspace app and the ICA connection (SSL) is established from the user system to on-premises Citrix Gateway and then (on port 1494/2598) to the VDA which is assigned by the controller for this connection. On a successful connection, the HDX session (2598) is launched and presented to the user.

The workflow remains the same for other regions as each region Citrix FQDN points to the respective data center on the respective region.

**Control Layer:** Delivery controllers, SQL Database, Studio, and Licensing are the core components in the Control Layer, which are provisioned on Citrix Cloud for the customer by Citrix. The admin can log in to the Citrix Cloud portal and select Virtual Apps and Desktops Service to manage the components. When the admin selects the “manage” button on Citrix Cloud, it launches Studio for them to administer the environment.

Citrix Studio helps admins to configure the Hosting Connection, Machine Catalog, Delivery Groups, Applications, and Policies for each region data center.

Each data center has a dedicated hosting connection to communicate with hypervisors to provision and manage the virtual machines. The admin has created separate machine catalogs and delivery groups for each region. The admin was able to enable the subscribers on respective delivery groups, thus allowing users to access their apps and desktops.

The admin selects the Monitor tab on the Citrix Cloud portal of Virtual Apps and Desktop Service page to open Citrix Director. This helps the company admins to monitor the complete Citrix environment from Citrix Cloud.

[![CVAD-Image-26](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_026.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_026.png)

**Resource Layer:** The Resource layer is where all the Citrix workloads reside in this deployment, and it is called a Resource Location in Citrix Cloud.

As a starting point for integrating the hybrid Citrix workloads with Citrix Cloud, the admin installed 2x Windows Server 2016 VMs on their hypervisor in each data center, and then Citrix Cloud Connectors are installed on both of the virtual machines to connect with Citrix Cloud.

The Cloud Connector installation created the resource location on Citrix Cloud, and also it added the customer.com domain as an authentication domain in Citrix Cloud for users.

With the help of Citrix Studio and using the master image templates created using App Layering, the admin has deployed virtual apps servers with Windows Server 2016 using Citrix Provisioning (PVS) for task workers, virtual desktops with Windows 10 using Machine Creation Services (MCS) for Power workers, and virtual desktops with Red Hat Linux using Machine Creation Services for Linux users on the hypervisor.

Virtual Delivery Agents installed with these VMs are registered with Cloud Connectors, and the admin created a Machine Catalog and Delivery Groups to provision (Library) the Desktops and Apps for region-specific users using an AD security group (subscribers). Citrix Policies are created and assigned using Studio for the delivery groups to optimize and secure the HDX connections.

[![CVAD-Image-27](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_027.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_027.png)

**Platform Layer:** The customer has procured the server hardware required to host Citrix workloads based on their requirements and design decisions. During the design decision step, the customer has allocated an extra 20% resources to cater and handle disaster recovery situations in any other region. The Citrix admin has installed the hypervisor to host the Citrix workloads. The network admin has enabled the required firewall rules for the new environment. The storage admin helped to configure and assign adequate storage to the new Citrix environment.

**Operations Layer:** The tools or components which are required to manage the Citrix workloads within Resource Location are covered under the Operations Layer. The IT Admin has deployed a file server cluster on each data center to store the user profiles from Citrix workloads.

Using Citrix Workspace Environment Manager Service, the Citrix admin has applied the Profile Management Policies to WEM Agents. The customer opted to use the latest Citrix User Profile Manager on the VDAs to map the user’s profiles to the file server cluster. The IT Admin has created dedicated file shares and NFS shares for the user profiles and folder redirection. Profile Management policies optimized Citrix workloads for quick login and logoff.

The Citrix admin also applied resource management policies to optimize the CPU and memory utilization on VDA agents. Using the Workspace Environment Manager, the cloud admin applied application security and process management policies to control the end-user activity.

The Customer has enabled Remote Desktop Services (RDS) Client Access License Server to issue the RDS licenses for Virtual Apps workloads in a Resource Location.

To manage multiple master images for each region and to update each master image with OS and application patches, the customer decided to deploy Citrix App Layering Manager in the Resource Location and enable the management plane via Citrix Cloud. With the App Layering Service, admins created OS Layers, Platform Layers, and Application Layers as per the workload requirement. By combining these layers, the admin has created the master image templates to provision the Citrix workloads. This simplified the process of creating and managing the master images for all three regions.

[![CVAD-Image-28](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_028.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_028.png)

**Disaster recovery:** During a disaster in a region, the cloud admin has to enable access to other region subscribers for the apps and desktops access. The reserved workloads are utilized during the disaster in the region. The access is enabled only for identified resources to handle the disaster situation.

Identified users will be educated by an email message which helps them with the new URL and steps explaining how to access the required resources during the disaster.

[![CVAD-Image-29](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_029.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_029.png)

## CVADS using Citrix Workspace with On-Premises Gateway and Citrix workloads

An enterprise customer wants to enable external access to their existing Citrix Virtual Apps and Desktops environment for their corporate users who are spread across different regions. This Citrix environment is focused on enabling access to intranet applications and other productivity resources. The customer wants Citrix experts to manage the control infrastructure; hence they opted to go with Citrix Cloud. Citrix Cloud enables customers to utilize their existing Citrix Gateway devices along with Citrix Cloud Services to enable the HDX access for the internal resources.

The customer decided to go with this deployment model, which enables them to offload managing the Control infrastructure to Citrix experts to keep the environment always green and up to date. Citrix Cloud allows the customer to utilize their existing Citrix Gateway devices to provide the best user experience. The customer installed two Cloud Connectors in each resource location along with Citrix workloads and Citrix ADC Gateways.

The conceptual architecture for this deployment shown below, the design framework of each layer of this deployment, explains how Citrix Cloud enables the best solution for the customer.

[![CVAD-Image-30](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_030.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_030.png)

**User Layer:** Users both internal (within corporate) and external (over the internet) who will be accessing this Citrix environment using the Citrix Workspace URL. The IT team has enabled firewall rules on their network devices to access the Citrix environment resources. The IT has installed the latest Workspace app on the corporate devices and upgraded the old Citrix Receiver wherever applicable.

External users may also use their personal devices along with corporate devices, so it is recommended to install the latest Workspace app on users’ personal devices. External users can also utilize the HTML5 version of Citrix Workspace using the latest web browsers where they cannot install the Workspace app on the devices.

The Citrix admin started replicating the machine catalog and delivery groups like the existing legacy environment, which are pointing to Citrix Cloud Connectors. In addition to that, the customer wants to create additional machine catalogs and delivery groups for the knowledge and power users as per new requirements.

**Access Layer:** Citrix Cloud has provisioned Workspace to access the Company’s Citrix environment, and users are provided with the Workspace URL (https://customer.cloud.com) from Citrix Cloud. The Workspace configuration tab allows company admins to change the Workspace URL and the customization of the Workspace Page.

[![CVAD-Image-31](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_031.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_031.png)

Installation of Cloud Connector enables the extension of the customer’s Active Directory domain to Citrix Cloud for authentication. The authentication configuration in Workspace Configuration allows the admin to select the domain for users to sign-in and access the Citrix resources. The admin has selected on-premises Active Directory to authenticate the Citrix subscribers on Citrix Cloud.

Users accessing the Citrix Cloud URL https://customer.cloud.com is asked to enter the domain credentials, which will then be validated against their on-premises Active Directory via Cloud Connector. Once validated, the user is then presented with the workspace page where they can access the apps and desktops which are assigned.

On workspace configuration, the Citrix admin has enabled the Traditional Gateway, which enabled the external access to Citrix workloads via on-premises Gateway. Citrix Workspace is the front-end entry point for all the users to access the Citrix environment. This helps corporate network admins to enable the globally approved HTTPS access towards Citrix Cloud from corporate devices.

When the user launches an application or desktop, the user is presented with an ICA file which will be launched via the Workspace app and the ICA connection (SSL) is established from user system to on-premises Gateway and then the connection is passed to the VDA  (on port 1494/2598) which is assigned by the controller for this connection. On a successful connection, the HDX session (2598) is launched and presented to the user.

**Control Layer:** Delivery controllers, SQL Database, Studio, and Licensing, are the core components in the Control Layer, which are provisioned on Citrix Cloud for the customer by Citrix. The admin can log in to the Citrix Cloud portal and select Virtual Apps and Desktops Service tile to manage the components. When the admin selects the “manage” button, it launches Studio to administer the environment.

Citrix Studio helps admins to configure the Hosting Connection, Machine Catalog, Delivery Groups, Applications, and Policies for each region data center. Each data center has a dedicated hosting connection to communicate with hypervisors to provision and manage the virtual machines. The admin has created separate machine catalogs and delivery groups for each region. The admin was able to enable the subscribers on respective delivery groups, thus allowing users to access their apps and desktops.

[![CVAD-Image-32](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_032.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_032.png)

The admin selects the Monitor tab on the Citrix Cloud portal of Virtual Apps and Desktop Service page to open Citrix Director. This helps the company admins to monitor the complete Citrix environment from Citrix Cloud.

**Resource Layer:** The Resource layer is where all the Citrix workloads reside in this deployment, and it is called a Resource Location in Citrix Cloud.

As a starting point for integrating the Citrix workloads with Citrix Cloud, the admin installed Cloud Connectors to connect the on-premises workloads with Citrix Cloud.

The Cloud Connector installation created the resource location on Citrix Cloud, and also it added the customer.com domain as an authentication domain in Citrix Cloud for users.

With the help of Citrix Studio and using the master image templates created using App Layering, the admin has deployed virtual apps servers with Windows Server 2016 using Citrix Provisioning (PVS) for task workers, virtual desktops with Windows 10 using Machine Creation Services (MCS) for Power workers, and virtual desktops with Red Hat Linux using Machine Creation Services for Linux users on the hypervisor.

[![CVAD-Image-33](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_033.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_033.png)

Virtual Delivery Agents installed with these VMs are registered with Cloud Connectors, and the admin created a Machine Catalog and Delivery Groups to provision (Library) the Desktops and Apps for region-specific users using an AD security group (subscribers). Citrix Policies are created and assigned using Studio for the delivery groups to optimize and secure the HDX connections.

**Platform Layer:** In this layer, the customer has organized the server hardware required to host Citrix workloads based on their requirements and design decisions. During the design decision step, the customer has allocated an extra 20% resources to cater to and handle disaster recovery situations. The Citrix admin has installed the hypervisor to host the Citrix workloads. The network admin has enabled the required firewall rules for the new environment. The storage admin helped to configure and assign adequate storage to the new Citrix environment.

**Operations Layer:** The tools or components which are required to manage the Citrix workloads within Resource Location are covered under the Operations Layer. The IT Admin has deployed a file server cluster on each data center to store the user profiles from Citrix workloads.

[![CVAD-Image-34](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_034.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_034.png)

Using Citrix Workspace Environment Manager Service, the Citrix admin has applied the Profile Management Policies to WEM Agents. The customer opted to use the latest Citrix User Profile Manager on the VDAs to map the user’s profiles to the file server cluster. The IT Admin has created dedicated file shares and NFS shares for the user profiles and folder redirection. Profile Management policies optimized Citrix workloads for quick login and logoff.

The Citrix admin also applied resource management policies to optimize the CPU and memory utilization on VDA agents. Using the Workspace Environment Manager, the cloud admin applied application security and process management policies to control the end-user activity.

The Customer has enabled Remote Desktop Services (RDS) Client Access License Server to issue the RDS licenses for Virtual Apps workloads in a Resource Location.

To manage multiple master images for each region and to update each master image with OS and application patches, the customer decided to deploy Citrix App Layering Manager in the Resource Location and enable the management plane via Citrix Cloud. With the App Layering Service, admins created OS Layers, Platform Layers, and Application Layers as per the workload requirement. By combining these layers, the admin has created the master image templates to provision the Citrix workloads. This simplified the process of creating and managing the master images.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-scf6abc634394c1fa).

## References

[Citrix Cloud Sign-up Process](/en-us/citrix-cloud/overview/signing-up-for-Citrix-cloud/signing-up-for-Citrix-cloud.html)

[Resource Location](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)

[Identity and Access Management](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html)

[Library Offerings and User Assignment](/en-us/citrix-cloud/citrix-cloud-management/assign-users-to-offerings-using-library.html)

[Cloud Connector Internet Connectivity Requirements](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html)

[Citrix Cloud – Zones](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html)

[Local Host Cache](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html)

[Cloud Connector Secure Deployment](/en-us/citrix-cloud/overview/secure-deployment-guide-for-the-Citrix-cloud-platform.html)

[Sizing Guidelines for Cloud Connector and LHC](/en-us/citrix-cloud/advanced-concepts.html)

[Workspace Configuration](/en-us/citrix-cloud/workspace-configuration.html)

[Citrix FAS](/en-us/xenapp-and-xendesktop/7-15-ltsr/secure/federated-authentication-service/fas-architectures.html)

[Citrix Gateway Service](/en-us/citrix-virtual-apps-desktops-service/netscaler.html)

[Citrix Director](/en-us/xenapp-and-xendesktop/7-15-ltsr/director.html)

[Delegated Administration](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/delegated-administration.html)

[Virtual Delivery Agent](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html)

[Hosting Connections](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)
