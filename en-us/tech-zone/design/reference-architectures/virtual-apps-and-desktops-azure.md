---
layout: doc
h3InToc: true
contributedBy: Loay Shbeilat
specialThanksTo: Nitin Mehta
description: Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.
---
# Citrix Virtual Apps and Desktops Service on Azure

## Introduction

This guide assists with the Architecture and deployment model of Citrix Virtual Apps and Desktops services on Microsoft Azure.

The combination of Citrix Cloud and Microsoft Azure makes it possible to spin up new Citrix virtual resources with greater agility and elasticity, adjusting usage as requirements change. Virtual Machines on Azure support all the control and workload components required for a Citrix Virtual Apps and Desktops service deployment. Citrix Cloud and Microsoft Azure have common control plane integrations that establish identity, governance, and security for global operations.

This document also provides guidance on prerequisites, architecture design considerations, and deployment guidance for customer environments.
The document highlights the design decisions and deployment considerations across the following five key architectural principles:

*  **Operations** - Operations includes a wide variety of topics such as image management, service monitoring, business continuity, support, and others. Various tools are available to assist with automation of operations including Azure PowerShell, Azure CLI, ARM Templates, and Azure API.

*  **Identity** - One of the cornerstones of the entire picture of Azure is the identity of a person and their role-based access (RBAC). Azure identity is managed through Azure Active Directory (Azure AD) and Azure AD Domain Services. The customer must decide which way to go for its identity integration.

*  **Governance** - The key to governance is establishing the policies, processes, and procedures associated with the planning, architecture, acquisition, deployment, and operational management of Azure resources.

*  **Security** - Azure provides a wide array of configurable security options and the ability to control them so that customers can customize security to meet the unique requirements of their organization’s deployments. This section helps to understand how Azure security capabilities can help you fulfill these requirements.

*  **Connectivity** - Connecting Azure virtual networks with the customer’s local/cloud network is referred to as hybrid networking. This section explains the options for network connectivity and network service routing.

## Planning

The three most common scenarios for delivering Citrix Apps and Desktops through Azure are:

*  Greenfield deployment with Citrix Cloud delivering resource locations in Azure. This scenario is delivered via the Citrix Virtual Apps and Desktops service and used when customers prefer to go to a subscription model and outsource control plane infrastructure to Citrix.
*  Extending an on-premises deployment into Azure. In this scenario, the customer has a current on-premises control layer and would like to add Azure as a Citrix resource location for new deployments or migration.
*  Lift and shift. With this scenario, customers deploy their Citrix Management infrastructure into Azure and treat Azure as a site, using Citrix ADC and StoreFront to aggregate resources from multiple sites.

This document focuses on the Citrix Cloud deployment model. Customers can plan and adopt these services based on their organization needs:

### Citrix Virtual Apps and Desktops Service

Citrix Cloud Virtual Apps and Desktops services simplifies the delivery and management of Citrix technologies, helping customers to extend existing on-premises software deployments or move 100 percent to the cloud. Deliver secure access to Windows, Linux, and Web apps and Windows and Linux virtual desktops. Manage apps and desktops centrally across multiple resource locations while maintaining a great end-user experience.

### Citrix Virtual Desktops Essentials Service

Citrix and Microsoft customers have more options to deploy Windows 10 Enterprise within their organization. Citrix Virtual Desktops Essentials Service accelerates Windows 10 Enterprise migration for customers who prefer Microsoft Azure cloud solutions. This enables customers who have licensed Windows 10 Enterprise (Current Branch for Business) on a per-user basis the option to deliver a high-performance Windows 10 Enterprise virtual desktop experience from Azure.

### Citrix Virtual Apps Essentials Service

Citrix Virtual Apps Essentials, the new application virtualization service, combines the power and flexibility of the Citrix Cloud platform with the simple, prescriptive, and easy-to-consume vision of Microsoft Azure RemoteApp. Citrix Virtual Apps Essentials provides superior performance and flexibility by moving the back-end infrastructure to the cloud, simplifying app delivery without sacrificing management or end-user experience.

## Conceptual Reference Architecture

This conceptual architecture provides common guidelines for deployment of a Citrix Cloud resource location in Azure which will be discussed in the following sections.

[![Azure-RA-Image-1](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_001.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_001.png)

Diagram-1: Citrix Cloud Conceptual Reference Architecture

Refer to the [design guide](/en-us/tech-zone/design/design-decisions/azure-instance-scalability.html) on the scalability and economics of delivering Citrix Virtual Apps and Desktops services on Microsoft Azure

## Operations

In the operations subject area, this guide dives deeper into planning for the workspace environment requirements and hierarchy for foundational services. At the top layer, is found the subscription, resource group, and regional design considerations. Followed by common questions for VM storage, user profile storage, and master image management/provisioning. Also provided is guidance on Reserved instance optimization with Autoscale and planning for Business Continuity/Disaster Recovery.

### Naming Conventions

The naming of resources in Microsoft Azure is important because:

*  Most resources cannot be renamed after creation
*  Specific resource types have different naming requirements
*  Consistent naming conventions make resources easier to locate and can indicate the role of a resource

The key to success with naming conventions is establishing and following them across your applications and organizations.

When naming Azure subscriptions, verbose names make understanding the context and purpose of each subscription clear. Following a naming convention can improve clarity when working in an environment with many subscriptions.

A recommended pattern for naming subscriptions is:

| **Variable**      | **Example**                                                                                         | **Description**                                                                              |
| ----------------- | :-------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **[System]**      | CTX (Citrix), CORE (Azure)                                                                          | Three letter identifier for the product, application, or service that the resource supports. |
| **[Role]**        | XAW (XenApp Workers), VDA (Virtual Delivery Agent), CC (Cloud Connector), CVA (Citrix Virtual Apps) | Three letter identifier for a subsystem of the service.                                      |
| **[Environment]** | D, T, P (dev, test, or prod)                                                                        | Identifies the environment for the resource                                                  |
| **##**            | 01, 02                                                                                              | For resources that have more than one named instance (web servers, and so on).               |
| **[Location]**    | WU (West US), EU (East US), SCU (South Central US)                                                  | Identifies the Azure region into which the resource is deployed                              |

When naming resources in Azure use common prefixes or suffixes to identify the type and context of the resource. While all the information about type, metadata, context, is available programmatically, applying common affixes simplifies visual identification. When incorporating affixes into your naming convention, it is important to clearly specify whether the affix is at the beginning of the name (prefix) or at the end (suffix).

A well-defined naming scheme identifies the system, role, environment, instance count, and location of an Azure resource. Naming can be enforced using an Azure Policy.

| **Service**                 | **Scope**       | **Suggested Pattern**                                                             | **Example**                                        |
| --------------------------- | --------------- | --------------------------------------------------------------------------------- | -------------------------------------------------- |
| **Subscriptions**           | Global          | `[System][Environment]##[Location]-sub`                                           | `WSCD01scu-sub`                                    |
| **Resource Groups**         | Global          | `[System]-[Role]-[Environment]##-[Location]-rg`                                   | `CTX-Apps-P01-CUS-rg`                              |
| **Virtual Network**         | Resource Group  | `[System][Environment]##[Location]-vnet`                                          | `CTXP01cus-vnet`                                   |
| **Subnet**                  | Parent VNET     | `[Descriptive Context]`                                                           | `DMZ - 10.0.1.0/24   Infrastructure - 10.0.2.0/24` |
| **Storage Account**         | Resource Group  | `[System][Role][Environment]##[Location]`   Note: Must be lower case alphanumeric | `ctxinfd01scu`                                     |
| **Container**               | Storage Account | `[Descriptive Context]`                                                           | `vhds`                                             |
| **Virtual Machine**         | Resource Group  | `[System][Role][Environment]##[Location]` Note: Must be 15 characters or less.    | `CTXSTFD01scu`                                     |
| **Network Interface**       | Resource Group  | `[vmname]-nic#`                                                                   | `CTXSTFD01scu-nic1`                                |
| **Public IPs**              | Resource Group  | `[vmname]-pip`                                                                    | `CTXSTFD01scu-pip`                                 |
| **Virtual Network Gateway** | Virtual Network | `[System][Environment]##[Location]-vng`                                           | `WSCD01scu-vng`                                    |
| **Local Network Gateway**   | Resource Group  | `[System][Environment]##[Location]-lng`                                           | `WSCD01scu-lng`                                    |
| **Availability Sets**       | Resource Group  | `[System][Role]-as`                                                               | `CTXSTF-as`                                        |
| **Load Balancer**           | Resource Group  | `[System][Role]-lb`                                                               | `CTXNSG-lb`                                        |
| **Workspaces**              | Subscription    | `[System][Environment]-analytics`                                                 | `CTXP-analytics`                                   |
| **Tags**                    | Resource        | `[Descriptive Context]`                                                           | `Finance`                                          |
| **Key Vault**               | Subscription    | `[System][Environment]-vault`                                                     | `CTXP-vault`                                       |

### Subscriptions

Selecting a subscription model is a complex decision that involves understanding the growth of the customer’s Azure footprint within and outside the Citrix deployment. Even if the Citrix deployment is small, the customer might still have a large amount of other resources that are reading/writing heavily against the Azure API, which can have a negative impact on the Citrix environment. The reverse is also true, where many Citrix resources can consume an inordinate number of the available API calls, reducing availability for other resources within the subscription.

#### Single Subscription workspace model

In a single subscription model, all core infrastructure and Citrix infrastructure are located in the same subscription. This is the configuration recommended for deployments that require up to 1,200 Citrix VDAs (can be session, pooled VDI, or persistent VDI). The limits are subject to change, check the following for most [up to date VDA limits](/en-us/citrix-virtual-apps-desktops-service/limits.html). Refer to the following [blog](https://www.citrix.com/blogs/2020/05/06/improving-azure-performance-with-machine-creation-services/) for the latest start-shutdown scale numbers within a single subscription,

Diagram-2: Azure Single Subscription workspace model
[![Azure-RA-Image-2](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_002.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_002.png)

#### Multi-Subscription workspace model

In this model, core infrastructure and Citrix infrastructure are in separate subscriptions to manage the scalability in large deployments. Often enterprise deployments with multi-region infrastructure design are broken into multiple subscriptions to prevent reaching Azure subscription limits.

[![Azure-RA-Image-3](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_003.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_003.png)

Diagram-3: Azure Multi-Subscription workspace model

The following questions provide guidance to help customer’s understand the Azure subscription options and plan their resources.

| Component                                                                                    | Requirement                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Will the Azure subscription contain only Citrix resources?                                   | Determine if the Azure subscription will be used for dedicated Citrix resources or if the Citrix resources will be shared with other systems.                                                                                                                                                                                                                                                                                               |
| Single or Multiple subscription deployment?                                                  | Typically, multiple subscription deployments are for larger deployments where single subscription limitations are an issue and more granular security controls are necessary.                                                                                                                                                                                                                                                               |
| What Azure Limits are likely to be reached? How many resources are in a resource Group?      | Resource Groups has limits and Machine Creation Services (MCS) requires either 2 or 3 disks per VM resource. Review [Azure subscription limits](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits) while planning the solution.                                                                                                                                                                                      |
| What permissions are necessary for the CVAD service principle on the Azure subscription?     | Citrix Virtual Apps and Desktops requires to create resource groups and resources within the subscription. For example, when the service principle cannot be granted full access to a subscription, then it needs to be granted Contributor access to a pre-created resource group.                                                                                                                                                         |
| Will Development and Test environments be created in separate subscriptions from Production? | Isolating Development and Test subscriptions from Production enables the application and change of global Azure services in an isolated environment and silos resource utilization. This practice has benefits for security, compliance, and subscription performance. Creating separate subscriptions for these environments does add complexity to image management. These trade-offs should be considered based on the customer's needs. |

### Azure Regions

An Azure region is a set of data centers deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network. Azure gives customers the flexibility to deploy applications where they need to. Azure is generally available in 42 regions around the world, with plans announced for 12 more regions as of Nov 2018.

A geography is a discrete market, typically containing two or more Azure regions, that preserve data residency and compliance boundaries. Geographies allow customers with specific data-residency and compliance needs to keep their data and applications close.

Availability Zones are physically separate locations within an Azure region. Each Availability Zone is made up of one or more data centers equipped with independent power, cooling and networking. Availability Zones allow customers to run mission-critical applications with high availability and low-latency replication. To ensure resiliency, there’s a minimum of three separate zones in all enabled regions.

Consider these factors when choosing your region.

| **Component**                                                 | **Requirement**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Compliance and data residency                                 | Do customers have specific compliance or data-residency requirements? Microsoft can copy customer data between Regions within a given Geo for data redundancy or other operational purposes. For example, Azure Globally Redundant Storage (GRS) replicates Blob and Table data between two regions within the same Geo for enhanced data durability if there is a major data center disaster. Certain Azure services do not enable the customer to specify the region where the service will be deployed. These services can store customer data in any of Microsoft's data centers unless specified. Review [Azure Regions map](https://azure.microsoft.com/en-us/global-infrastructure/regions/) website for latest updates. |
| Service availability                                          | Review service availability within the tentative regions. Service Availability by region helps the customer to determine which services are available within a region. While an Azure Service can be supported in a given region, not all Service features are available in sovereign clouds, such as Azure Government, Germany, and China.                                                                                                                                                                                                                                                                                                                                                                                     |
| Determine the target Azure regions for the Citrix deployment. | Review proximity of Azure region to users and customer data centers.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Are multiple Azure regions required?                          | Multiple Azure regions are typically considered for the following high-level reasons: - Proximity to application data or end users - Geographic Redundancy for Business Continuity and Disaster Recovery - Azure Feature or Service availability                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |

### Availability Sets

An Availability Set is a logical grouping capability that can be used in Azure to ensure that the VM resources placed within an Availability Set are isolated from each other when they are deployed within an Azure data center. Azure ensures that the VMs placed within an Availability Set run across multiple physical servers, compute racks, storage units, and network switches. If a hardware or Azure software failure occurs, only a subset of your VMs is impacted, and the overall application stays up and remains available to customers. Availability Sets are an essential capability when customers want to build reliable cloud solutions.

Each component of a Citrix deployment should be in its own Availability Set to maximize overall availability for Citrix. For example, a separate Availability Set should be used for Cloud Connectors, another for Citrix Application Delivery Controllers (ADC), StoreFront, and so forth.

Once availability sets are optimized, the next step is to build resiliency around VM downtime within the availability sets. That minimizes/eliminates service downtime when VMs are restarted or redeployed by Microsoft. This can be expanded to planned maintenance events as well. There are two features you can use which can increase the reliability of the overall service.

These two features do not protect against unplanned maintenance/crashes.

*  Azure Planned Maintenance
*  Azure Scheduled Events

### Azure Planned Maintenance

Azure periodically performs updates to improve the reliability, performance, and security of the host infrastructure in Azure. If maintenance requires a reboot, Microsoft sends a notice. Using Azure Planned Maintenance, it is possible to capture these notices and proactively take action on them on the customer’s schedule, instead of on Microsoft’s schedule.

Make use of the planned maintenance feature by sending email notifications to the service owner of each tier (for manual intervention) and build runbooks to automate the service protection.

### Azure Scheduled Events

Azure Scheduled Events is an Azure Metadata Service that gives notices programmatically to applications to alert of immediate maintenance. It provides information about upcoming maintenance events (for example reboot) so the application administrator can prepare for and limit disruption. While it might sound like planned maintenance, it is not. The key difference is that these events are fired for planned maintenance and sometimes non-planned maintenance. For example, if Azure is doing host healing activities and needs to move VMs on a short notice.

These events are consumed programmatically, and will give the following advance notice:

*  Freeze – 15 Minutes
*  Reboot – 15 Minutes
*  Redeploy – 10 Minutes

### Disaster Recovery (DR)

Azure can provide a highly cost-effective DR solution for Citrix customers looking to gain immediate value from cloud adoption today. The deployment model topology determines the DR solution implementation.

### Extending the Architecture

Under this topology, the management infrastructure remains on-premises, but workloads are deployed to Azure. If the on-premises data center is not reachable, existing connected users remain connected, but new connections will not be possible because the management infrastructure is unavailable.

To protect the management infrastructure, pre-configure Azure Site Recovery to recover the management infrastructure into Azure. This is a manual process and once recovered, your environment can be made operational. This option is not seamless and cannot recover components such as ADC VPX, however for organizations with more a more flexible recovery time objective (RTO) it can reduce the operational costs.

### Hosting Architecture

When deploying this topology, the Citrix Management infrastructure is deployed into Azure and treated as a separate site. This provides functional isolation from on-premises deployment in the event of a site failure. Use Citrix ADC and StoreFront to aggregate resources and provide users a near instant failover between Production and Disaster Recovery resources.

The presence of the Citrix Infrastructure in Azure means that no manual processes need to be invoked and no systems need to be restored before users can access their core workspace.

### Cloud Services Architecture

When using Citrix Cloud, Azure becomes just another resource location. This topology provides the simplest deployment as the management components are hosted by Citrix as a Service, and Disaster Recovery workloads can be achieved without deploying duplicate infrastructure to support it. The user experience during failover in the event of a disaster can be seamless.

The items in the following table help the customer with their DR planning:

| **Component**                                                                                                                               | **Requirement**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| What are the RTO and RPO requirements of the Citrix environment?                                                                            | RTO - Targeted duration of time and a service level within which a business process must be restored after a disaster. RPO - The interval of time that might pass during a disruption before the quantity of data lost during that period exceeds the Business Continuity Plan’s maximum allowable threshold or “tolerance.”                                                                                                                                                                                    |
| What is the desired outcome when a service disruption occurs in the entire region where your Azure virtual machine application is deployed? | These options should be reviewed in alignment with the customer's RTO and RPO for DR. Disaster Recovery of a Citrix environment in Azure can be addressed with Azure Site Recover, passive Secondary Site and active Site Azure Site. Recovery only supports Server OS (Citrix infrastructure and Server VDAs). Client OS is not supported (for example persistent desktops created using ARM Templates). Also, Machine Catalogs created by MCS (Server or Client VDA) must be recreated using a Recovery Task. |

### Resource Groups

Resource Groups (RG) in Azure is a collection of assets in logical groups for easy or even automatic provisioning, monitoring, and access control, and for more effective management of their costs. The benefit of using RGs in Azure is grouping related resources that belong to an application together, as they share a unified lifecycle from creation to usage and finally, de-provisioning.

The key to having a successful design of resource groups is understanding the lifecycle of the resources that are included in them.

Resource Groups are tied to Machine Catalogs at creation time and cannot be added or changed later. To add extra Resource Groups to a Machine Catalog, the Machine Catalog must be removed and recreated.

### Image Management

Image management is the process of creating, upgrading, and assigning an image that is consistently applied across development, test, and production environments. The following should be considered when developing an image management process:

### On-Demand Provisioning

The customer needs to determine if MCS be used to manage the Azure non-persistent machines or create their own Azure Resource Manager (ARM) templates. When a customer uses MCS to create machine catalogs, the Azure on-demand provisioning feature reduces storage costs, provides faster catalog creation and faster virtual machine (VM) power operations. With Azure on-demand provisioning, VMs are created only when Citrix Virtual Apps and Desktops initiates a power-on action, after the provisioning completes. A VM is visible in the Azure portal only when it is running, while in Citrix Studio, all VMs are visible, regardless of power status. Machines created via ARM templates or MCS can be power managed by Citrix using an Azure host connection in Citrix Studio.

### Storage Account Containers

The customer needs to decide the organizational structure for the storing source (or golden) images from which to create the virtual machines using Citrix Machine Creation Services (MCS). Citrix MCS images can be sourced from snapshots, managed or unmanaged disks and can reside on standard or premium storage. Unmanaged disks are accessed through general-purpose storage accounts and are stored as VHDs within Azure Blob storage containers. Containers are folders which can be used to separate Production, Test, and Development images.

### Image Replication

The customer needs to determine the appropriate process for replicating images across regions and how Citrix App Layering technology might be used within the overall image management strategy. PowerShell scripts cam be used with Azure Automation to schedule image replication. More information on Citrix App Layering can be found [here](/en-us/citrix-app-layering/4.html), but keep in mind that Elastic Layering requires an SMB File share that does not reside on Azure Files. See the **File Servers** section for supported SMB share technologies that support Elastic Layering.

### File Server Technologies

Azure offers several file server technologies that can be used to store Citrix user data, roaming profile information or function as targets for Citrix Layering shares. These options include the following:

*  Standalone File Server
*  File Servers using Storage Replica
*  Scale Out File Server (SOFS) with Storage Spaces Direct (S2D)
*  Distributed File System – Replication (DFS-R)
*  Third-party storage appliances from Azure Marketplace (such as NetApp, and others)

The customer should select file server technologies that best meet their business requirements. The table below outlines some benefits and considerations for each of the different file serving technologies.

| **Options**                           | **Benefits**                                                                                                                                                                                                        | **Considerations**                                                                                                                |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| Standalone File Server                | Well known and tested. Compatible with existing backup/restore products                                                                                                                                             | Single point of failure. No data redundancy. Outage for monthly patching, measured in minutes.                                    |
| File Servers using Storage Replica    | Block Level Replication. SMB 3.0. Storage Agnostic (SAN, Cloud, Local, and so on). Offers Synchronous and Asynchronous Replication. Recommended when multi-region access is required                                | Manual failover needed. Uses 2x disk space. Manual failover still has downtime, measured in minutes. DNS dependency.              |
| SOFS on Storage Spaces Direct         | Highly available. Multi-node and Multi-disk HA. Scale up or scale out. SMB 3.0 and 3.1. Transparent failover during planned and unplanned maintenance activities. Recommended for user profile storage within Azure | Uses 2-3x disk space. Third party back-up software support can be limited by the vendor. Does not support multi-region deployment |
| Distributed File System – Replication | Proven technology for file-based replication. Supports PowerShell                                                                                                                                                   | Domain-based. Cannot be deployed in an active-active configuration.                                                               |
| Third-party storage applications      | Deduplication technologies. Better use of storage space.                                                                                                                                                            | Extra cost. Proprietary management tools.                                                                                         |

The recommended file server virtual machine types are generally DS1, DS2, DS3, DS4, or DS5, with the appropriate selection depending on customer use requirements. For best performance, ensure that premium disk support is selected. Extra guidance can be found on Microsoft Azure [documentation](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment).

### Infrastructure Cost Management

Two technologies are available that can be used to reduce the costs of the Citrix environment in Azure, reserved instances and Citrix Autoscale.

### Reserved Instances

Azure Reserved VM Instances (RIs) significantly reduce costs—up to 72 percent compared to pay-as-you-go prices—with one-year or three-year terms on Windows and Linux virtual machines (VMs). When customers combine the cost savings gained from Azure RIs with the added value of the Azure Hybrid Benefit, they can save up to 80 percent. The 80% is calculated based on a three-year Azure Reserved Instance commitment of a Windows Server when compared to the normal pay-as-you-go rate.

While Azure Reserved Instances require making upfront commitments on **compute** capacity, they also provide flexibility to exchange or cancel reserved instances at any time. A reservation only covers the virtual machine compute costs. It does not reduce any of the additional software, networking, or storage charges. This is good for the Citrix infrastructure and minimum capacity needed for a use case (on and off hours).

Citrix Autoscale feature supports reserved instances as well to further reduce your costs - you can now use Autoscale for bursting in the cloud. In a delivery group you can tag machines that need to be autoscaled and exclude your reserved instances (or on-premises workloads) - you can find more info here: [Restrict Autoscale to certain machines in a Delivery Group](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/restrict-autoscale.html#restrict-autoscale-to-certain-machines-in-a-delivery-group).

### Citrix Autoscale

Autoscale is a feature exclusive to the Citrix Virtual Apps and Desktops service that provides a consistent, high-performance solution to proactively power manage your machines. It aims to balance costs and user experience. Autoscale incorporates the deprecated Smart Scale technology into the Studio power management solution.

| **Machine Type**                                                                             | **Schedule-based**                                                                                                                                                                                                                                                                                      | **Load-based**                                                                               | **Load and schedule-based**                                                                  |
| -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Server OS** machines hosting published applications or hosted shared desktops (Server VDI) | Supported                                                                                                                                                                                                                                                                                               | Supported                                                                                    | Supported                                                                                    |
| **Desktop OS** machines hosting static persistent (dedicated) VDI desktops                   | Supported. During periods when machines are powered off (for example, after working hours), users can trigger machines to power on through the Citrix Receiver. You can set Autoscale’s Power Off Delay so Autoscale does not automatically power machines off before the user can establish a session. | Supported only for unassigned machines.                                                      | Supported only for unassigned machines.                                                      |
| **Desktop OS** - machines hosting - random non-persistent VDI desktops (pooled VDI desktops) | Supported                                                                                                                                                                                                                                                                                               | Supported. Use the Session Count scaling metric and set the maximum number of sessions to 1. | Supported. Use the Session Count scaling metric and set the minimum number of machines to 1. |

[![Azure-RA-Image-4](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_004.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_004.png)

Diagram-4: Citrix Autoscale Flow

You can read more about Citrix Autoscale [here](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html).

### Optimizing End-User Experience

Optimizing the end-user experience includes balancing the end user’s perception of responsiveness with the business needs of staying within a budget. This section discusses the design concepts and decisions around providing an environment that is correctly sized for the business and the end user.

### Defining User Workspace

When planning for a user assessment, the [Citrix VDI Handbook and Best Practices](/en-us/xenapp-and-xendesktop/7-15-ltsr/downloads/handbook-715-ltsr.pdf) documentation provides invaluable guidance. Review the following high-level questions to better understand existing use cases and the resources needed for their end users.

| **Topic**                 | **Question**                                                                                                                                                                                                                                                                    |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Number of Users           | How many users are expected within the environment? Did the assessment phase determine the appropriate VDI Model? (Virtual Apps or Virtual Desktops)                                                                                                                            |
| Use Cases                 | What types of applications will be consumed by the end users? What are the VDA requirements for the applications? How will the applications be delivered best? (Virtual Apps vs Virtual Desktops)                                                                               |
| User Group working hours  | When will users be accessing the environment? What are the peak hours? What is the expected consumption throughout the day? (The consumption of users during specific hours helps identify workspace requirements for scale automation and Azure reserved Instance purchasing.) |
| Location                  | Where are the end users located? Should workspaces be deployed across multiple regions or only in a single region?                                                                                                                                                              |
| User and Application Data | Where is the user and application data stored? Will data be contained solely in Azure, only on-premises, or a mix of both? What is the maximum tolerable latency for accessing the user data?                                                                                   |

### Azure VM Instance Types

Each Citrix component uses an associated virtual machine type in Azure. Each VM series available is mapped to a specific category of workloads (general purpose, compute-optimized, and so forth) with various sizes controlling the resources allocated to the VM (CPU, Memory, IOPS, network, and others).

Most Citrix deployments use the D-Series and F-Series instance types. The D-Series are commonly used for the Citrix infrastructure components and sometimes for the user workloads when they require extra memory beyond what is found in the F-Series instance types. F-Series instance types are the most common in the field for user workloads because of their faster processors which bring with them the perception of responsiveness.

**Why D-Series or F-Series?** From a Citrix perspective, most infrastructure components (Cloud Connectors, StoreFront, ADC, and so on) use CPU to run core processes. These VM types have a balanced CPU to Memory ratio, are hosted on uniform hardware (unlike the A-Series) for more consistent performance and support premium storage. Certainly, customers can and should adjust their instance types to meet their needs and their budget.

The size and number of components within a customer’s infrastructure will always depend on customer’s requirements, scale, and workloads. However, with Azure we have the ability to scale dynamically and on-demand! For cost-conscious customers, starting smaller and scaling up is the best approach. Azure VMs require a reboot when changing size so plan these events within scheduled maintenance windows only and under established change control policies.

### How about Scale-up or Scale-out?

The following high-level questions should be reviewed to better understand a customer’s use case and the resources needed for their end users. This also helps them to plan their workload well in advance.

Scaling up is best when the cost per user per hour needs to be the lowest and a larger impact can be tolerated should the instance fail. Scaling out is preferred when the impact of a single instance failure needs to be minimized. The table below provides some example instance types for different Citrix components.

| **Component**                          | **Recommended Instance Type**                                                                                                                                                                                                                                                                                                         |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Delivery Controllers, Cloud Connectors | Standard DS2_v2 or DS2_v3 with Premium SSD storage                                                                                                                                                                                                                                                                                    |
| Scale Up Server OS User Workloads      | Standard_F16s_v2 VMs with Virtual App were identified to have the lowest $/user/hr cost compared to other instances. Standard_DS5_v2 VMs were also cost competitive compared to other instances                                                                                                                                       |
| Scale Out Server OS User Workloads     | Standard_F4_v2 and Standard_F8_v2 instances support a lower user count however provide more flexibility of power management operations due to smaller user container sizes. This allows machines to be more effectively deallocated to save costs on Pay-as-You-Go instances. Also, the failure domains are smaller when scaling out. |
| Desktop OS User Workloads              | Standard_F2_v2 has the lowest dual-core cost and performs well with Windows 10.                                                                                                                                                                                                                                                       |

The latest instance type study was done to provide great insight in this area and we highly recommend the [read](https://www.citrix.com/content/dam/citrix/en_us/documents/white-paper/citrix-virtual-app-and-desktop-services-microsoft-azure.pdf). In all cases, customers should evaluate the instance types with their workloads.

For graphic-intensive workloads, consider the [NVv4-series](https://docs.microsoft.com/en-us/azure/virtual-machines/nvv4-series) virtual machines. They are powered by AMD EPYC 7002 processors and virtualized Radeon MI25 GPU. These virtual machines are optimized and designed for VDI and remote visualization. With partitioned GPUs, NVv4 offers the right size for workloads requiring smaller GPU resources at the most optimal price. Alternative the NVv3 series is optimized and designed for remote visualization, streaming, gaming, encoding, and VDI scenarios using frameworks such as OpenGL and DirectX. These VMs are backed by the NVIDIA Tesla M60 GPU. For further GPU options check the other [offerings](https://docs.microsoft.com/en-us/azure/virtual-machines/sizes-gpu) from Azure.

While scaling up is usually a preferred model to reduce the cost, Autoscale can benefit from smaller instances (15–20 sessions per host). Smaller instances host fewer user sessions than larger instances. Therefore, in the case of smaller instances, Autoscale puts machines into drain state much faster because it takes less time for the last user session to be logged off. As a result, Autoscale powers off smaller instances sooner, thereby reducing costs. You can read more about instance size considerations for Autoscale in [official documentation](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html#instance-size-considerations).

### Storage

Just like any other computer, a virtual machines in Azure use disks as a place to store an operating system, applications, and data. All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk. The operating system disk is created from an image, and both the operating system disk and the image are stored within Azure as virtual hard disks (VHDs). Virtual machines may also have extra disks attached as data disks, also stored as VHDs.

Azure Disks are designed to deliver enterprise-grade durability. Three performance tiers for storage exist that can be selected when creating disks: Premium SSD Disks, Standard SSD, and Standard HDD Storage, and the disks may be either managed or unmanaged. Managed disks are the default and are not subject to the storage account limitations like the unmanaged disks.

Managed Disks are recommended over the unmanaged disk by Microsoft. Unmanaged disks should be considered by exception only. Standard Storage (HDD and SSD) includes transaction costs (storage I/O) that must be considered but have lower costs per disk. Premium Storage has no transaction costs but have higher per disk costs and offers an improved user experience.

The disks offer no SLA unless an Availability Set is used. Availability Sets are not supported with Citrix MCS but should be included with Citrix Cloud Connector, ADC, and StoreFront.

## Identity

The section focuses on Identity controls, workspace user planning, and the end-user experience. The primary design consideration is managing identities within both Azure and Citrix Cloud tenants.

Microsoft Azure Active Directory (Azure AD) is an identity and access management cloud solution that provides directory services, identity governance, and application access management. A single Azure AD directory is automatically associated with an Azure subscription when it is created.

Every Azure subscription has a trust relationship with an Azure AD directory to authenticate users, services, and devices. Multiple subscriptions can trust the same Azure AD directory, but a subscription will only trust a single Azure AD directory.

Microsoft’s identity solutions span on-premises and cloud-based capabilities, creating a single user identity for authentication and authorization to all resources, regardless of location. This concept is known as Hybrid Identity. There are different design and configuration options for hybrid identity using Microsoft solutions, and in some case, it might be difficult to determine which combination will best meet the needs of an organization.

### Common Identity Design Considerations

Usually extending the customers Active Directory Site to Azure utilizes the use of Active directory replication to provide identity and authentication with the Citrix Workspace. A common step is to use AD Connect to replicate user to Azure Active Directory which provides you with the subscription-based activation required for Windows 10.

It is recommended to extend local Active Directory Domain Services to the Azure Virtual Network Subnet for full features and extensibility.
Azure Role-Based Access Control (RBAC) helps provide fine-grained access management for Azure resources. Too many permissions can expose and account to attackers. Too few permissions mean that employees can’t get their work done efficiently. Using RBAC, administrator can give employees the exact permissions they need.

### Authentication

Domain Services (either AD DS or Azure AD DS) are required for core Citrix functionality. RBAC is an authorization system built on the Azure Resource Manager that provides fine-grained access management of resources in Azure. RBAC allows you to granularly control the level of access that users have. For example, you can limit a user to only manage virtual networks and another user to manage all resources in a resource group. Azure includes several built-in roles that you can use.

Azure AD Authentication is supported for the Workspace Experience Service and Citrix ADC/StoreFront authentication. For full SSON with Azure AD, Citrix Federated Authentication Service (FAS) or Azure AD DS (for core Domain Services) must be used.

Citrix FAS is not yet supported for the Workspace Experience Service therefore StoreFront is required as part of the deployment architecture.
Azure MFA server (Cloud Service, Azure MFA Server, Azure MFA NPS Extension) can enable the usage of Azure MFA without requiring a SAML policy and the use of Citrix FAS for full SSON. However this is an IaaS VM and must be managed by the customer.

Active Directory and Azure Active Directory Outcomes

*  Azure Active Directory Provisioned Tenant
*  List of desired Organizational roles for Azure RBAC with mapping to Built-In or Custom Azure Roles
*  List of desired Admin access levels (Account, Subscription, Resource Group and so on)
*  Procedure to grant access/role to new users for Azure
*  Procedure to assign JIT (just in time) elevation for users for specific tasks

Below is an example architecture of namespace layout and authentication flow.

[![Azure-RA-Image-5](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_005.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_005.png)

Diagram-5: Architecture of namespace layout and authentication flow

### Citrix Cloud Administration + Azure AD

By default, Citrix Cloud uses the Citrix Identity provider to manage the identity information for all users who access the Citrix Cloud. Customers can change this to use Azure Active Directory (AD) instead.
By using Azure AD with Citrix Cloud, Customers can:

*  Use their own Active Directory, so they can control auditing, password policies, and easily disable accounts when needed.
*  Configure multifactor authentication for a higher level of security against the possibility of stolen sign-in credentials.
*  Use a branded sign-in page, so your users know they’re signing in at the right place.
*  Use federation to an identity provider of your choice including ADFS, Okta, and Ping, among others.

Citrix Cloud includes an Azure AD app that allows Citrix Cloud to connect with Azure AD without the need for you to be logged in to an active Azure AD session. Citrix Cloud Administrator Login allows Azure AD identities to be used in the customers Citrix Cloud tenant.

*  Determine if Citrix Cloud administrators use their Citrix Identity or Azure AD to access the Citrix Cloud the URL will follow the format `https://citrix.cloud.com/go/{Customer Determined}`
*  Identify the Authentication URL for Azure AD authentication into Citrix Cloud

## Governance

Azure Governance is a collection of concepts and services that are designed to enable management of your various Azure resources at scale. These services provide the ability to organize and structure your subscriptions in a logical way, to create, deploy, and reusable Azure native packages of resources. This subject is focused on establishing the policies, processes, and procedures associated with the planning, architecture, acquisition, deployment, operation, and management of Azure resources.

### Citrix Cloud Administrator Login

Determine if Citrix Cloud administrators use their Citrix Identity, Active Directory Identity, or Azure AD to access Citrix Cloud. Azure AD integration enables multifactor authentication into Citrix Cloud for administrators. Identify the Authentication URL for Azure AD authentication into Citrix Cloud. URL follows the format `https://citrix.cloud.com/go/{Customer Determined}`.

### RBAC permissions & delegation

Using Azure AD customers can implement their governance policies using Role-Based Access Control (RBAC) of Azure resources. One of the primary tools for the application of these permissions is the concept of a Resource Group. Think of a Resource Group as a bundle of Azure resources that share lifecycle and administrative ownership.

In the context of a Citrix environment these should be organized in a way that will allow for proper delegation between teams and promote the concept of least privilege. A good example is when a Citrix Cloud deployment uses a Citrix ADC VPX provisioned from the Azure Marketplace for external access. Although a core piece of Citrix infrastructure, the Citrix ADCs might have a separate update cycle, set of admins, and so on This would call for separating the Citrix ADCs from the other Citrix components into separate Resource Groups so the Azure RBAC permissions can be applied through the administrative zones of tenant, subscription, and resources.

### MCS Service Principal

To access resources that are secured by an Azure AD tenant, the entity that requires access must be represented by a security principal. This is true for both users (user principal) and applications (service principal). The security principal defines the access policy and permissions for the user/application in the Azure AD tenant. This enables core features such as authentication of the user/application during sign-in, and authorization during resource access.

Determine the permissions allocated to the Service Principal used by the Citrix MCS service.

Subscription scope service principals have Contributor rights to the applicable subscription used by the Citrix environment. Narrow Scope service principals have granular RBAC applied to the Resource Groups containing the network, master images, and VDAs. Narrow Scope Service Principals are recommended to limit the permissions only to the permissions required by the service. This adheres to the security concept of "least privilege".

### Tagging

Customer applies tags to their Azure resources giving metadata to logically organize them into a taxonomy. Each tag consists of a name and a value pair. For example, they can apply the name "Environment" and the value "Production" to all the resources in production.

Customer can retrieve all the resources in your subscription with that tag name and value. Tags enable them to retrieve related resources from different resource groups. This approach is helpful when admin need to organize resources for billing or management.

There is a limit of 15 tags per Resource. Citrix MCS creates 2 tags per VM therefore a customer is limited to 13 tags for MCS machines. MCS non-persistent machines are deleted during reboot. This removes Azure VM-specific characteristics such as tags, boot diagnostics If tags are required, it is recommended to create an Azure Append policy and apply it to the applicable MCS Resource Groups.

### Azure Policy

Azure policies can control aspects such as tagging, permitted SKUs, encryption, Azure region, and naming convention. There are default policies available and the capability to enforce custom policies. Azure policies can be applied at the subscription or Resource Group level. Multiple policies can be defined. Policies applied at the Resource Group level take precedence over Subscription Level policy.

Identify aspects of Azure that should be controlled and standardized across the Citrix environment. Hard quota forces the policy and not permits exceptions. Soft quota audits for policy enforcement and notify if the policy is not met. Refer Azure documentation for more detailed information to define the policies.

[![Azure-RA-Image-6](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_006.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_006.png)

Diagram-6: Azure Governance Access Policy and RBAC

## Security

Security is integrated into every aspect of Azure. Azure offers unique security advantages derived from global security intelligence, sophisticated customer-facing controls, and a secure hardened infrastructure. This powerful combination helps protect applications and data, support compliance efforts, and provide cost-effective security for organizations of all sizes.

### Securing storage accounts provisioning by CVAD service

As stated previously, MCS is the service (within CVAD) responsible for spinning up machines in the customer subscription. MCS utilizes uses an AAD identity – Application service principal for access to Azure resource groups to perform different actions.  For storage account type of resources, MCS requires the `listkeys` permission to acquire the key when needed for different actions (write/read/delete).  Per our current implementation, an MCS requirement for:

*  Storage account network is access from the public internet.
*  Storage account RBAC is `listkeys` permission

For some organizations keeping the Storage account endpoint public is a concern.  Below is an analysis of the assets created and stored when deploying VMs with managed disk (the default behavior).

*  Table Storage:
We maintain machine configuration and state data in table storage in the primary storage account (or a secondary one, if the primary one is being used for Premium disks) for the catalog.  There is no sensitive information within the tables.
*  Locks:
For certain operations (allocating machines to storage accounts, replicating disks), we use a lock object to synchronize operations from multiple plug-in instances.  Those files are empty blobs and include no sensitive data.

For machine catalogs created before Oct 15 2020, MCS creates an additional storage account for identity disks:

*  Disk Import:
When importing disks (identity, instruction), we upload the disk as a page blob.  We then create a managed disk from the page blob and delete the page blob. The transient data does include sensitive data for computer object names and password. This does not apply for all machine catalogs created post Oct 15 2020.

Using a narrow Scope Service Principal applied to the specific resource groups is recommended to limit the permissions only to the permissions required by the service. This adheres to the security concept of "least privilege". Refer to [CTX219243](https://support.citrix.com/article/CTX219243) and [CTX224110](https://support.citrix.com/article/CTX224110) for more details.

### IaaS - Azure Security Center Monitoring

Azure Security Center analyzes the security state of Azure resources. When the Security Center identifies potential security vulnerabilities, it creates recommendations that guide the customer through the process of configuring the needed controls. Recommendations apply to Azure resource types: virtual machines (VMs) and computers, applications, networking, SQL, and Identity and Access. There are few best practices that you have to follow:

*  Control VM access and Secure privileged access.
*  Provisioning antimalware to help identify and remove malicious software.
*  Integrate your antimalware solution with the Security Center to monitor the status of your protection.
*  Keep your VMs current & ensure at deployment that the images you built include the most recent round of Windows and security updates.
*  Periodically redeploy your VMs to force a fresh version of the OS.
*  Configuring network security groups and rules to control traffic to virtual machines.
*  Provisioning web application firewalls to help defend against attacks that target your web applications.
*  Addressing OS configurations that do not match the recommended baselines.

## Network Design

Network security can be defined as the process of protecting resources from unauthorized access or attack by applying controls to network traffic. The goal is to ensure that only legitimate traffic is allowed. Azure includes a robust networking infrastructure to support your application and service connectivity requirements. Network connectivity is possible between resources located in Azure, between on-premises and Azure hosted resources, and to and from the internet and Azure.

### Virtual Network (VNet) Segmentation

Azure virtual networks are similar to a LAN on your on-premises network. The idea behind an Azure virtual network is that you create a single private IP address space–based network on which customers can place all their Azure virtual machines. The best practice is to segment the larger address space into subnets and create network access controls between subnets. Routing between subnets happens automatically, and you don’t need to manually configure routing tables.

Use a **Network Security Group** (NSG). NSGs are simple, stateful packet inspection devices that use the 5-tuple (the source IP, source port, destination IP, destination port, and layer 4 protocol) approach to create allow/deny rules for network traffic. Rules allow or deny traffic to and from a single IP address, to and from multiple IP addresses, or to and from entire subnets.

Customers can create custom, or user-defined, routes called User-defined Routes (UDRs) in Azure to override Azure's default system routes, or to add extra routes to a subnet's route table. In Azure, admins can create a route table, then associate the route table to zero or more virtual network subnets. Each subnet can have zero or one route table associated to it.

NSGs and UDRs are applied at the subnet-level within a Virtual Network. When designing a Citrix Virtual Network in Azure it is recommended to design the virtual network with this in mind, creating subnets for similar components, allowing for the granular application of NSGs and UDRs as needed. An example of this would be segmenting the Citrix infrastructure into its own subnet, with a corresponding subnet for each use case.

Identify the ports and protocols required for Citrix and the supporting technologies. Review to verify these ports are allowed within the Network Security Groups used in the environment. Network Security Groups can limit inbound and outbound communications to a defined set of IP, Virtual Networks, Service Tags, or Application Security Groups.

[![Azure-RA-Image-7](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_007.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_007.png)

Diagram-7: Azure Security Center and Network Security using NSG and ASG

## Connectivity

Connecting Azure virtual networks with customers local / cloud network is referred to as hybrid networking. This section explains the options for network connectivity and network service routing.
Customers can connect their on-premises computers and networks to a virtual network using any combination of the following options:

*  Point-to-site virtual private network (VPN): Established between a virtual network and a single computer in customer network. Each computer that wants to establish connectivity with a virtual network must configure its connection. This connection type is great for just getting started with Azure, or for developers, because it requires little or no changes to the customer's existing network. The communication between your computer and a virtual network is sent through an encrypted tunnel over the internet.
*  Site-to-site VPN: Established between on-premises VPN device and an Azure VPN Gateway that is deployed in a virtual network. This connection type enables any on-premises resource that the customer authorizes to access a virtual network. The communication between on-premises VPN device and an Azure VPN gateway is sent through an encrypted tunnel over the internet.
*  Azure ExpressRoute: Established between customer’s network and Azure, through an ExpressRoute partner. This connection is private. Traffic does not go over the internet.

The primary considerations for Azure to Customer connectivity are bandwidth, latency, security, and cost. Site to Site VPNs have lower bandwidth limits than Express Route and are dependent on the performance of the edge router used by the customer. SLAs are available on the VPN Gateway SKUs. Site to Site VPNs use IPSEC over the internet.

Express Routes are dedicated private connections and not over the internet. This results in lower latency when using Express Route. Also, Express Route can scale up to 10 Gbps. Express Route is configured using a certified partner. Configuration time by these providers should be considered during project planning. Express Route costs have a Microsoft component and an Express Route provider component.

Typically these connections are shared across multiple services (database replication, domain traffic, application traffic, and so on) In a hybrid cloud deployment there may be scenarios where internal users require their ICA traffic to go through this connection to get to their Citrix apps in Azure, therefore monitoring its bandwidth is critical.

With ADC and traditional StoreFront optimal gateway routing may also be used to direct a user’s connection to a ADC using an office’s ISP rather than the Express Route or VPN to Azure.

### User-Defined Routes (UDRs)

Typically customers use a UDR to route Azure traffic to a firewall appliance within Azure or a specific virtual network. For example, North/South traffic from a VDA to the internet. If large amounts of traffic are routed to third party firewall appliances within Azure this can create a resource bottleneck or availability risk if these appliances are not sized or configured appropriately. NSGs can be used to supplement third-party firewalls and should be utilized as much as possible where appropriate. Consider Azure Network Watcher if traffic introspection is required.

### Virtual network peering

Virtual network peering seamlessly connects two Azure virtual networks. Once peered, the virtual networks appear as one, for connectivity purposes. The traffic between virtual machines in the peered virtual networks is routed through the Microsoft backbone infrastructure, much like traffic is routed between virtual machines in the same virtual network, through private IP addresses only.

Azure supports:

*  VNet peering - connecting VNets within the same Azure region
*  Global VNet peering - connecting VNets across Azure regions

Customers deploying workloads on multiple VNets should consider to use the VNet peering to enable the communication between VMs between VNets.

[![Azure-RA-Image-8](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_008.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-azure_008.png)

Diagram-8: Data center Connectivity and Routes

### SD-WAN

Software-defined WAN (SD-WAN) technology makes it possible to deliver a great user experience, even over challenging connections. It’s a natural fit for virtual apps and desktops delivery.

*  Aggregate all available bandwidth into an active/active connection, providing more bandwidth.
*  Use the unique HDX Quality of Experience technology to optimize performance and tune network policies.
*  Ensure always-on connections for virtual apps and desktop users with the highest possible-quality experience—even for rich media and high-definition video.

Customers using VPN might use SD-WAN to add redundancy to the Azure and Customer data center connectivity or to provide application-specific routing. Citrix SD-WAN automatically redirecting traffic across any available connections. In fact, the experience is so seamless, users won’t even realize any change has occurred. Their primary access IP address remains unchanged, allowing users to access their apps and data using the same methods and devices.

### Citrix ADC

Citrix ADC on Microsoft Azure ensures that organizations have access to secure and optimized applications and assets deployed in the cloud and provides the flexibility to establish a networking foundation that adjusts to the changing needs of an environment. In the event of a data center failure, Citrix ADC automatically redirects user traffic to a secondary site, with no interruptions for users. Load balancing and global server load balancing across several data centers further ensures optimum server health, capacities, and utilization.

Discuss with customer and define the following use case for each Resource Location:

| **Access Method**                                                      | **Considerations**                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Internal only                                                          | A Citrix ADC is not required if only internal access is needed.                                                                                                                                                                                                                                                                                                                                                                                                                               |
| External access via Citrix ADC Gateway Service.                        | The Citrix Cloud ADC Gateway Service provides ICA Proxy (secure remote connectivity only).                                                                                                                                                                                                                                                                                                                                                                                                    |
| External access via Citrix ADC VPX deployed in Azure Resource Location | A customer needs to consider a Citrix ADC VPX appliance in Azure if they require the following:  1. Multifactor authentication with full SSON 2. Endpoint scanning 3. Advanced authentication or pre-authentication policies 4. Citrix SmartAccess policies. Note: These requirements prompt the need for authentication to occur at the Citrix ADC rather than the Workspace Experience service. StoreFront is required if authentication is managed by a Citrix ADC Gateway virtual server. |

### Citrix ADC - Deployment Model

Active-Active deployments use standalone Citrix ADC nodes that can be scaled out using the Azure Load Balancer. Active-Passive pairs facilitate stateful failover of ICA traffic in the event of a node failure however they are limited to the capacity of a single VPX. Active-Passive nodes also require Azure Load Balancer.

Citrix ADC is limited to 500 Mbps per Azure NIC. Multiple NICs are recommended to isolate the SNIP, NSIP, and VIP traffic to maximize the throughput available for Citrix ADC Gateway or other services.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sa656b2ebbc3543a388db602cf2bf8dc1).

## References

### Operations

*  General Governance
    *  [AWS to Azure services comparison](https://docs.microsoft.com/en-us/azure/architecture/aws-professional/services)
    *  [Governance in Azure](https://docs.microsoft.com/en-us/azure/security/governance-in-azure)
    *  [Azure Blueprint Automation: Financial Services Blueprint for Regulated Workloads](https://docs.microsoft.com/en-us/azure/security/blueprints/financial-services-regulated-workloads)
    *  [Azure Blueprint Automation: Payment Processing for PCI DSS-compliant environments](https://docs.microsoft.com/en-us/azure/security/blueprints/payment-processing-blueprint)
    *  [Azure Blueprint Automation - Web Applications for FedRAMP](https://docs.microsoft.com/en-us/azure/security/blueprints/fedramp)

*  Subscription Governance

    *  [Organize your resources with Azure Management Groups](https://docs.microsoft.com/en-us/azure/billing/billing-enterprise-mgmt-group-overview)
    *  [Azure enterprise scaffold - prescriptive subscription governance](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-subscription-governance)

*  Tagging
    *  [Use tags to organize your Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags)

*  Cost Management
    *  [Cloud (Cost Management)](https://docs.microsoft.com/en-us/azure/cost-management/)
*  Policy
    *  [What is Azure Policy?](https://docs.microsoft.com/en-us/azure/azure-policy/azure-policy-introduction)
    *  [Templates for Azure Policy](https://docs.microsoft.com/en-us/azure/azure-policy/json-samples)

*  Naming Conventions
    *  [Prescriptive Guidance](https://docs.microsoft.com/en-us/azure/architecture/best-practices/naming-conventions)
*  Resource Locks
    *  [Lock resources to prevent unexpected changes](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-lock-resources)

### Identity

*  Reference Architectures

    *  [Best Practices](https://docs.microsoft.com/en-us/azure/security/azure-security-identity-management-best-practices)

    *  [Built-in Roles](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-built-in-roles)

    *  [Custom Roles](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles)

    *  [Role Assignment](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-create-custom-roles-for-internal-external-users)

    *  [Audit Report for Roles](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-access-change-history-report)

    *  [Just-In-Time Access](https://docs.microsoft.com/en-us/azure/security-center/security-center-just-in-time)

    *  [Azure-Managed Service Identity](https://docs.microsoft.com/en-us/azure/active-directory/msi-overview)

*  AAD Privileged Identity (PIM)

    *  [Privileged Identity Management - 2 Min Read](https://docs.microsoft.com/en-us/azure/active-directory/pim-azure-resource)

    *  [Privileged Identity Management - 8 Min Read](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-configure)

    *  [Privileged Identity Management for Azure Resources](https://docs.microsoft.com/en-us/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)

    *  [Topologies for Azure AAD Connect for Ad Sync/ ADFS](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-topologies)

    *  [XenApp and XenDesktop Service support for Azure AD DS](https://www.citrix.com/blogs/2017/04/11/xenapp-xendesktop-services-support-azure-ad-domain-services/)

    *  [Azure Active Directory and Citrix XenApp and XenDesktop](https://support.citrix.com/article/CTX224111)

    *  [Two DCs in HA](https://github.com/Azure/azure-quickstart-templates/tree/master/active-directory-new-domain-ha-2-dc)

### Governance

*  Level 100

    *  [Introduction to Azure Security](https://docs.microsoft.com/en-us/azure/security/azure-security)

    *  [Persistent information security for your sensitive data](https://www.microsoft.com/en-us/cloud-platform/information-protection)

*  Level 200

    *  [Azure Security Services and Technologies](https://docs.microsoft.com/en-us/azure/security/azure-security-services-technologies)

*  Level 400

    *  [Isolation in Azure - Compute, Storage](https://docs.microsoft.com/en-us/azure/security/azure-isolation)

    *  [[Video\] Encryption key management strategies for compliance](https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK2000?term=Encryption)

*  Reference Architectures:

    *  [Azure security best practices and patterns](https://docs.microsoft.com/en-us/azure/security/security-best-practices-and-patterns)

    *  [Azure security encryption by service](https://docs.microsoft.com/en-us/azure/security/azure-security-encryption-atrest#azure-resource-providers-encryption-model-support)

    *  [Azure security for IaaS workloads](https://docs.microsoft.com/en-us/azure/security/azure-security-iaas)

    *  [All about GDPR readiness](https://www.microsoft.com/en-us/TrustCenter/Privacy/gdpr/default.aspx)

    *  [This template enables encryption on a running Windows VM Scale Set](https://azure.microsoft.com/en-us/resources/templates/201-encrypt-running-vmss-windows/)

    *  [Enable encryption on a running Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)

### Security

*  Level 100

    *  [Introduction to Azure Security](https://docs.microsoft.com/en-us/azure/security/azure-security)

    *  [Persistent information security for your sensitive data](https://www.microsoft.com/en-us/cloud-platform/information-protection)

*  Level 200

    *  [Azure Security Services and Technologies](https://docs.microsoft.com/en-us/azure/security/azure-security-services-technologies)

*  Level 300

    *  [Isolation in Azure - Compute, Storage](https://docs.microsoft.com/en-us/azure/security/azure-isolation)

    *  [[Video\] Encryption key management strategies for compliance](https://channel9.msdn.com/events/Ignite/Microsoft-Ignite-Orlando-2017/BRK2000?term=Encryption)

*  Reference Architectures:

    *  [Azure security best practices and patterns](https://docs.microsoft.com/en-us/azure/security/security-best-practices-and-patterns)

    *  [Azure security encryption by service](https://docs.microsoft.com/en-us/azure/security/azure-security-encryption-atrest#azure-resource-providers-encryption-model-support)

    *  [Azure security for IaaS workloads](https://docs.microsoft.com/en-us/azure/security/azure-security-iaas)

    *  [All about GDPR readiness](https://www.microsoft.com/en-us/TrustCenter/Privacy/gdpr/default.aspx)

### Azure Monitor

*  Level 100

    *  [Azure Monitor - high level](https://azure.microsoft.com/en-us/services/monitor/)

*  Level 200

    *  [Azure Monitoring overview](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/)

*  Reference Architectures

    *  [Azure Architecture Center Best Practices for Monitoring](https://docs.microsoft.com/en-us/azure/architecture/best-practices/monitoring)

*  Documentation

    *  [Azure Monitor videos](https://azure.microsoft.com/en-us/resources/videos/index/?services=monitor)

    *  [Azure Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics)

### Connectivity

*  Level 100

    *  [Introduction to Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview)

    *  [Azure virtual network traffic routing](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/improve-the-xendesktop-experience-for-branch-and-mobile-workers-with-netscaler-sdwan.pdf)

    *  [Citrix ADC](https://www.citrix.com/content/dam/citrix/en_us/documents/product-overview/netscaler-gateway-service-product-overview.pdf)

*  Level 200

    *  [Virtual Network Integration for Azure Services](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-for-azure-services)

    *  [Improve the Citrix Virtual Apps and Desktops experience for branch and mobile workers with SD-WAN](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/improve-the-xendesktop-experience-for-branch-and-mobile-workers-with-netscaler-sdwan.pdf)

*  Documentation

    *  [Citrix ADC Documentation](/en-us/citrix-virtual-apps-desktops-service/netscaler.html)

    *  [NetScaler VPX Deployment Guide](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/netscaler-vpx-deployment-with-xendesktop-and-xenapp-on-microsoft-azure.pdf)
