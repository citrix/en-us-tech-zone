---
layout: doc
---
# Citrix Managed Desktops

## Contributors

**Author:** [Nagaraj Manoli](mailto:nagaraj.manoli@citrix.com)

**Special Thanks:** [Kireeti Valicherla](mailto:kireeti.valicherla@citrix.com), [Swaroop JV](mailto:Swaroop.JosephVarghese@citrix.com), [Alan Goldman](mailto:alan.goldman@citrix.com)

## Audience

This document is intended for technical professionals, IT decision-makers, partners, and system-integrators. This document also allows the administrator to explore, and adopt Citrix Managed Desktops to provide cloud-based workspaces to their employees. The reader must have a basic understanding of Citrix products, Citrix Cloud, and Microsoft® Azure services.

## Objective of this document

This document comprises of a technical overview, architectural concepts, and adoption methodology on Citrix Managed Desktops. Multiple use cases on different verticals with conceptual architecture are also included to allow readers to understand and formulate the cloud-based virtual desktop solution.

## Introduction to Citrix Managed Desktops

Citrix Managed Desktops (CMD) is a cloud-based virtual apps and desktops solution. It enables businesses to deliver cloud-hosted virtual apps and desktops to any device, over the network, from any location. The operating system runs inside virtual machines on the Azure public cloud. All the necessary infrastructure (IaaS) support is provided from Citrix. Citrix managed Azure subscription is owned and managed by Citrix where Virtual Delivery Agents (VDAs) are running. The virtual apps and desktops are presented over the secured network to a customer’s endpoint devices where end-users access them through the Citrix Workspace app or a web browser.

[![CMD-Image-1](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_001.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_001.png)

The Citrix Managed Desktops architecture is divided up into multiple segments. All segments flow together to create a complete, end-to-end cloud-based virtual desktop solution for an organization.

*  **Users segment-** This section describes the end-user environment and end-point devices that are used to connect to resources. This section covers the end-point devices and Citrix Workspace app.

*  **Access segment-** This section describes external connectivity to devices in the user segment. This section covers the Citrix Gateway service and Workspace configuration details.

*  **Control segment-** This section describes components used to support the rest of the environment, which includes site design for the Citrix Cloud service, image management, and monitoring of Citrix Managed Desktops.

*  **Resource segment-** This section captures information for the users personalization, applications, and images for the CMD environment.

*  **Platform segment-** This section describes the cloud platform used to provision catalogs. Citrix Managed Azure Platform is based on Microsoft® Azure platform for Citrix VDAs, that is completely managed by Citrix. Customer-managed Azure subscriptions are used to provide hybrid connectivity to additional resources as required.

*  **Operations segment-** This section contains customer-managed components such as Windows Active Directory service, Azure Active Directory, file servers, and Windows license servers. It is possible to provision file servers and license servers on customer-managed Azure subscription or in a customer data center with hybrid connectivity between Azure to an on-premises data center.

The layered picture gives a holistic view of the Citrix Managed Desktops services. The details and internal architectures of Citrix Managed Azure Platform, hybrid connectivity, image management approach, and multiple deployment methodologies are explained in upcoming sections.

### Citrix Managed Desktops and traditional desktop solution

Organizations typically adopt virtual desktop solutions based on their business needs. As companies expand their global footprint and increase productivity, IT is often left with the challenge to meet the growing demands and use cases. Organizations have started to adopt digital workspace solutions based on traditional desktop solutions. The virtual desktop solution has been around for a long time and traditionally was the only way to run a virtual desktop. This is cost-effective for companies that have a large employee base in a single region or geographic location.

The traditional virtual desktop solution requires substantial support from IT professionals. This includes customization of desktops, maintaining efficiency with the latest updates, assuring solid connectivity, and providing great user experience.

In a traditional virtual desktop infrastructure solution, IT administrators are responsible for the management of the implementations. Organizations often require a significant investment in capex for infrastructure. Apart from this process, the additional investment in securing the infrastructure must be in place, for example, localized threat detection solutions must be implemented to avoid any data breaches.

Most of the cloud service providers have observed these challenges faced by customers. To overcome this problem many leading vendors introduced Desktop as a Service (DaaS) to the market. DaaS solutions allow customers to quickly realize the benefits of VDI, switch from a capex to opex model, and reduce the required ongoing management effort. Citrix, one of the pioneers in the desktop virtualization field, has introduced the DaaS solution called “**Citrix Managed Desktops**.”

Citrix Managed Desktops (CMD) is similar to a VDI solution. It offers the same user experience, and flexibility with greater visibility to the infrastructure and overcomes initial deployment haul and investments. CMD differs from the traditional desktop solution because instead of hosting desktops in an on-premises data center or even a public cloud location with full infrastructure management required, CMD uses a cloud-based back end from the Microsoft® Azure cloud platform while greatly simplifying setup and management tasks in a simple, turnkey solution.

Citrix Managed Desktops requires only a minimal investment to start, which is a good fit for small and medium-sized businesses. The organization does not need to invest in VDI or virtualization experts - CMD is well-suited for IT generalists to handle.

The following table compares the Citrix Managed Desktops solution with a traditional virtual desktop solution:

| **Benefits**                              | **CMD**                                                      | **Traditional virtual desktop solution**              |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| Simplified virtual desktop implementation | Easy to deploy                                          | Moderate deployment steps                              |
| Centralized desktop management            | Complete centralized management solution                     | Multiple silos of infrastructure to manage             |
| Data security                             | Reduced attack surface with key back-end maintained by Citrix | Additional considerations for ongoing optimal security |
| Issue resolution and recovery             | Simplified                                                   | Moderate with sturdy IT resources                      |
| CapEx ($)                                 | Upfront investments not required                             | Moderate to the significant investment required        |
| OpEx ($)                                  | Minimal deployment cost                                 | Moderate for small scale deployment                    |
| Geographic coverage (workloads)           | Supports multiple Geos across Azure regions                  | Bound to data center locations                         |
| Complexity and risk                       | Minimal                                                 | Moderate                                               |
| Pay-as-you-go model                     | Highly applicable                                            | Not applicable                                         |
| Skilled resource requirement              | Basic IT generalists                                         | Skilled specialists often required                     |

## Why organizations adopt a Citrix Managed Desktops solution

Many organizations desire to benefit from this era of the digital workspace. Their workforce expects a different set of work culture which means accessing their work from anywhere on any device at any time. Meanwhile, management wants to control IT spending. Cloud-based virtual apps and desktop solutions are the better choices when companies are looking for a cost-effective, simple solution to securely deliver the apps and desktops to their workforce.

Citrix Managed Desktops is a viable solution for customers looking for centralized management with standardized virtual desktop infrastructure. In the era of the digital workspace, Citrix enables an organization to deliver do-it-yourself cloud-based apps and desktop.

[![CMD-Image-2](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_002.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_002.png)

The above diagram shows a high-level skeleton view for Citrix Managed Desktops. The control plane is hosted in Citrix Cloud and all the resources are provisioned on the Microsoft® Azure platform which is managed by Citrix. CMD customers have an option to choose the workload region (supported by Citrix) depending on their requirements.

Citrix Managed Desktops offers many advantages to IT by simplifying operations and delivering Windows desktops and apps securely to its workforce. Citrix Managed Desktops has many benefits including:

*  Windows Virtual Desktop (WVD) instances hosted on a Citrix Managed Azure platform

*  Superior user experience which enables users to collaborate with their work culture

*  Simplified management and monitoring through Citrix Cloud

*  Secure remote access with multifactor authentication

*  Citrix Virtual Delivery Agents (VDAs) running within the Microsoft® Azure platform (Citrix Managed). The administrator can bring multiple Azure services including ExpressRoute, VPN services, Azure Files from customer-managed Azure subscription.

## Citrix Managed Desktops use cases

Citrix Managed Desktops has the ability to manage unforeseen business shifts in IT infrastructure on different vertical segments. Citrix Managed Desktops gives liberty to businesses from the complex act of implementation and management of the IT infrastructure. Complexity in terms of purchasing, supporting, upgrading, and most important is security.

In this section, some of the useful and constructive use cases on different verticals are discussed.

### Citrix Managed Desktops in Education services

Today most of the universities are transforming their learning paths and skill development engagements. Students and lecturers must be there at the campus to access software and services required for their studies. This process may hamper or decrease overall time efficiency. With the current IT infrastructures, many education services are facing the challenges to meet the demand. Usually IT organizations purchase and build out an infrastructure that sits unused much of the year.

To overcome all the challenges faced by universities, Citrix has seen the opportunity to improve student productivity and engagements by hosting all necessary software and tools on the Citrix Managed Desktops service. Students can access their necessary resources anytime, anywhere, and from any device.

[![CMD-Image-11](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_011.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_011.png)

The above diagram shows a conceptual Citrix Managed Desktops usage in universities. The administrator has to create the catalogs for individual machine types. The Student catalog uses a non-domain joined catalog. The administrator has to install the required application during the master image creation. For the Lecturers catalog, the separate image could be used to provision the VDAs which are domain joined desktops. If the university is already utilizing Azure AD with Office 365, no VNet peering or customer-managed components are necessary.

Here multiple catalogs share a resource location and virtual network. A virtual network is a unique per-connection for domain-joined catalogs, and per region for non-domain joined catalogs.

### Health Care Solution using Citrix Managed Desktops

In health care, field professionals have a pressing need to access real-time patient data and applications remotely and from multiple devices. Today many health care institutes are facing regulatory burdens and shrinking IT budgets which made their hands tied to traditional desktop technology. Citrix Managed Desktops offers a straightforward way to increase technological presence without a major refurbishment of a company’s systems and processes.

[![CMD-Image-12](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_012.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_012.png)

Overall cost reduction is the top challenge. Citrix Managed Desktops generally saves money and time and there is no cost associated with the implementations. The above diagram shows the health care institutes using Citrix Managed Desktops in multiple resource locations and providing anywhere access, centralized management, and flexible working are the key drivers for this solution.

Key benefits of using Citrix Managed Desktops in health care institutes:

*  Access file and patient’s data from anywhere

*  There is no cost associated with the upgrading and managing the IT infrastructure

*  Easier data backups and recovery

*  Flexible and predictable scale

*  Security and compliance

*  Focus on the primary mission, delivering the best care to patients

### Citrix Managed Desktops for the Logistic industry

The logistic company workforce always faces challenges in accessing the mission-critical data from anywhere and anytime. This data includes an inventory list from the warehouse, tracking orders, billing information, and so on. These kinds of circumstances always recede productivity.

By hosting their desktops and applications in the Citrix Managed Desktops, customers can focus on their core business without having to waste time and money on procurement and managing the infrastructure.

Key benefits by adopting Citrix Managed Desktops:

*  Low TCO

*  Mobility

*  Scalability

*  Security

*  Centralized Management

### Mergers and Acquisitions

Today there are huge barriers when brining two separate companies’ assets into one. Mergers and acquisitions take place in today’s business world to grow and expand the business market. But the real challenges are faced when merging IT infrastructure.

Positioning the IT infrastructure of two separate entities needs future infrastructure planning, utilization, and the management of legacy applications with their infrastructures.

[![CMD-Image-13](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_013.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_013.png)

In the case organizations have multiple forests and multiple exchange servers, it is more cumbersome to consolidate.

The above diagram depicts the multiple Active Directory forest present on-premises when the merger or acquisitions take place. When organizations have multiple forests, all forests must be reachable by a single Azure AD Connect sync server. It is not necessary to join the server to a domain. In a case where it is necessary to reach all forests, the administrator can place the server in a perimeter network such as the DMZ. The goal of this solution is that a user is represented only once in Azure AD.

The advantage of Citrix Managed Desktops is, the administrator can easily consolidate two forests with a single Azure AD. The new demand and requirement of desktops are easily provisioned on Citrix Managed Desktops. This solution enhances user productivity.

There are other topologies available for Azure AD Connect and those are:

*  Multiple forests, single sync server, users are represented in only one directory

*  Multiple forests: full mesh with optional GAL Sync

*  Multiple forests: account-resource forest

For more information on different topologies for Azure AD Connect refer to the following [link](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/plan-connect-topologies).

### Non-Domain Joined Desktops for DevOps and seasonal workers

In Citrix Managed Desktops a virtual desktop can be domain joined or non-domain joined. When there is an urgent requirement to involve third-party developers in the project it is hard to provide a desktop with reliable connectivity. For example, a seasonal worker or contractor whose involvement in the project is as required as organization employees. Apart from this task, the administrator has to make sure that they are accessing resources securely.

[![CMD-Image-14](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_014.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_014.png)

The above diagram shows a non-domain-joined desktop provisioned for contractors and any dev-ops worker. The administrator creates a catalog for the dev-ops team and provisions a desktop with a necessary application that is not joined to the corporate domain. The administrator can provide or leverage Azure multifactor authentication to add one more layer of security.

The end-user has to access their desktop through Citrix Workspace app using URL the provided by the Citrix administrator.

## Technical Overview of Citrix Managed Desktops architecture

Citrix Managed Desktops is the simplest, fastest way to deliver Windows apps and desktops hosted in Microsoft® Azure. The Citrix Managed Desktops solution is managed through Citrix Cloud. The resource provisioning, capacity management, and workspace configuration are all done through the Citrix Cloud web-based tools.

[![CMD-Image-3](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_003.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_003.png)

The above diagram depicts the architecture for Citrix Managed Desktops. In the Citrix Managed Desktops solution, Azure subscription is managed by Citrix. Customer-managed Azure subscriptions and customer data center resources are owned and managed by the customer. The administrators have the power to provision and monitor VDAs from Citrix Cloud. End-users can access their desktops from anywhere, and any device. The above diagram is divided into four sections:

**Citrix Cloud:** The Citrix Cloud hosts a complete control plane for Citrix Managed Desktops. Delivery Controllers, database instances, monitoring tools, and other infrastructure components are part of CMD. Apart from Citrix Managed Desktops, other services including Citrix Gateway service, Access Control, SD-WAN orchestrator, Content Collaboration, and other services are hosted on Citrix Cloud.

**Citrix-Managed Azure Platform:** Citrix VDAs are provisioned on this platform. Windows server and client operating systems are hosted on this platform. This platform communicates with Citrix Cloud through Citrix Cloud Connectors. A pair of Cloud Connectors is hosted within a virtual network in each resource location. End-customers do not have access to the Citrix managed Azure subscription including Cloud Connectors. Therefore, Citrix is responsible for the performance and management of the Cloud Connectors.

In case customers want to communicate with existing resources on their Azure platform, they can use a VNet peering between two virtual networks.  From the Citrix Cloud, customers can configure a VNet peering to their customer-managed virtual network by giving existing Azure subscription credentials.

**Customer-Managed Azure Platform:** This platform is completely owned by the customer. This Azure platform is the home for multiple Azure services provisioned by the customer. These services are leveraged by providing high throughput connectivity to the Citrix Managed Azure platform using VNet peering. Currently, VNet peering is scoped to a single region.

**Customer Data Center:** The resources in the data center are completely managed by the customer. In case the organization needs to leverage services that are running to Citrix Managed Desktops, the customer has to establish connectivity between the Azure Platform (owned by the customer) and data center by using site-to-site VPN or SD-WAN network services.

### Citrix Managed Desktops instance types

Citrix Managed Desktops uses general-purpose compute power from the Azure platform to provide different types of catalogs. A catalog is a group of identical virtual machines. CMD utilizes the D-series of VMs from Azure. The D-series VMs feature with fast CPUs and optimal CPU-to-memory configuration making them suitable for desktop workloads.

The administrator has the option to select a machine (a combination of CPU and memory) during catalog creation. Available machine types are as follows:

**Multi-Session:** Contains machines that are accessed by more than one user simultaneously. Supported master images are Windows 10 EVD (Multi-session) and Windows 2016 Server.

| **Machine type** | **Sessions** | **Virtual CPU** | **Memory (GB)** |
| ------------------ | ------------ | --------------- | ----------------- |
| Light (D2s v3)   | 16           | 2               | 8                 |
| Medium (D2s v3)  | 10           | 2               | 8                 |
| Heavy (D2s v3)   | 4            | 2               | 8                 |
| Custom             | -            | 2, 4, 8           | 4, 8, 16, 32         |

**Static (personal desktops):** This machine is a dedicated machine that has been assigned to a user during login. The desktop will only be used by that particular user. Any changes that are made to the desktop are retained at logoff.

**Random (pooled desktops):** This machine is a non-persistent desktop. Any changes made in the desktops are discarded after logoff. Any authenticated users can access this machine.

| **Machine type** | **Virtual CPU** | **Memory (GB)** |
| ------------------ | --------------- | ----------------- |
| B2s                | 2               | 4                 |
| D2s v3             | 2               | 8                 |
| D4s v3             | 4               | 16                |
| D8s v3             | 8               | 32                |

## Citrix Managed Desktops Networking

To provide users with best possible experience, Citrix Managed Desktops supports a hybrid or all-in cloud strategy using networking services. The administrators have an option to connect with their existing cloud and on-premises infrastructure and services through VNet peering only if a customer has Azure subscription present in that workload region.

To set up a connection to the corporate data center or other resources, the administrators have to make use of the Azure VNet Peering feature. This process requires Microsoft® Azure Subscription Owner privileges.

### Azure VNet Peering

Virtual Network (VNet) is the fundamental building block for the private network in the Azure platform. Virtual machines can securely communicate with each other, the Internet, and on-premises networks. The Citrix Managed Azure platform is completely managed by Citrix including creating VNets, applying the policies, and so on. Customers only have visibility of the machines provisioned for them which makes Citrix Managed Desktops a pure DaaS solution.

The Citrix Managed Desktops portal gives an option to connect a customer’s existing Azure resources by configuring a VNet peering between Citrix Managed to Customer Managed Azure Virtual Networks. VNet peering is scoped to a single region.

[![CMD-Image-4](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_004.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_004.png)

The VNet Peering is required for VDAs in Citrix Managed Desktops to contact on-premises domain controllers, file shares, and other resources. The customer’s Azure Platform needs to have connectivity to their on-premises resources using ExpressRoute, IPsec tunnels, SD-WAN, and other site-to-site VPN technologies.

Learn more about VNet concepts, and VNet Peering by visiting the [link](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview).

### Hybrid Connectivity to on-premises resources

To enable VDAs running on Citrix Managed Desktops to communicate with on-premises resources, customers have to make a hybrid connection between a customer-managed Azure platform to their on-premises resources. Microsoft® Azure gives multiple options for hybrid connectivity. This section outlines a feasible solution for connecting an on-premises network to Azure.

### VPN connection

A VPN gateway is used to send encrypted traffic between a customer’s Azure virtual network and an on-premises location over the public Internet. Each VNet can have only one VPN gateway, but administrators can create multiple connections to the same VPN gateway. In such cases, all VPN tunnels share the available gateway bandwidth.

VPN based architecture is suitable for the hybrid application if traffic between the Azure cloud and on-premises is lightweight and the customer is willing to trade latency for the processing power of the cloud.

This service is more suitable when the customer is using on-premises Active Directory services, accessing tools and services from existing virtual machines, connecting branch offices, and so on.

### Azure ExpressRoute

The on-premises networks to Microsoft® Azure cloud connection are extended over a private connection provided by the service provider using Azure ExpressRoute. With this connection, the organization can utilize on-premises resources effectively for Citrix Managed Desktops. For example, it is possible to store their profiles on-premises file server, share point, or existing storage for content collaboration services, and so on.

The key benefits for deploying ExpressRoute are layer 3 connectivity between the on-premises network and Azure cloud, higher reliability, dynamic routing, and global connection to Microsoft® services across all regions.

ExpressRoute circuits are available in a wide range of bandwidth from 50 Mbps to 10 Gbps. Customers need to check with their local service provider.

## Citrix SD-WAN for Citrix Managed Desktops

An organization using virtual desktops often struggles to obtain a high-quality user experience and always-on connectivity. Reliable, high-performance connectivity is even more important if the virtual desktops are used with cloud-based VOIP or video conferencing. The Citrix Managed Desktops solution offers various hybrid connectivity options to end customers with Citrix SD-WAN providing a sound choice for optimum user experience and cost-effective connectivity.

### Citrix SD-WAN Architecture

Let’s review the Citrix SD-WAN architecture that helps an organization to benefit from reliable network connectivity.

[![CMD-Image-15](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_015.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_015.png)

The preceding diagram represents the Citrix SD-WAN architecture along with the Citrix Managed Desktops solution.

### Overview of Citrix SD-WAN Architecture

Citrix SD-WAN provides the flexibility that it can be deployed in several deployment modes and to integrate the appliances, both physical and virtual, into the customer’s existing networking design as well as with Cloud deployments. Refer to the [Citrix Tech Zone link](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/sdwan.html)for the SD-WAN overview.

The solution demonstrates that the SD-WAN fits well with a customer’s existing network design and how it eases the integration process in the Azure platform for Citrix Managed Desktops. The administrator deploys a Citrix SD-WAN VPX (virtual appliance) that utilizes the internet path to establish the network connectivity from the Azure environment to a data center and branch office networks.

The Master Control Node (MCN) establishes a virtual path with Azure environment VPX using the available Internet links. This eliminates the need for establishing direct connectivity from the customer’s hosted location to Azure subscription.

Citrix recommends implementing a Citrix SD-WAN solution for Citrix Managed Desktops to improve the network connectivity and performance, required to deliver the DaaS offering and all associated applications. Also, the virtual desktop access via Citrix SD-WAN is specially optimized for **Citrix HDX technologies** thus providing the best user experience.

Citrix SD-WAN offers an always-on experience, changing network paths within 10 milliseconds when there’s a failure in a specific path ensuring highly-available connectivity to Citrix Managed Desktops.

[![CMD-Image-16](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_016.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_016.png)

The preceding diagram depicts the deployment architecture of a Citrix SD-WAN solution for optimizing every connection needed to deliver a great user experience for Citrix Managed Desktops and applications. The Zscalar cloud security platform (or other SWG) is an optional component.

### How Citrix SD-WAN helps in establishing connectivity to customer hosted locations

Citrix SD-WAN virtual appliances hosted on a virtual network within Azure for Citrix Managed Desktops establishes virtual network connectivity with on-premises SD-WAN appliances at office locations and, if applicable, an on-premise data center. The connectivity begins over the inexpensive internet where customers can use broadband, landline or 4G/LTE of their choice. The Citrix SD-WAN solution is much cheaper than other hybrid connectivity options when achieving low latency and QoS features.

**Citrix SD-WAN is faster and cheaper to deploy when compared to Azure ExpressRoute**

*  Inexpensive business internet connectivity from Azure to on-premises (Cable, DSL,4G/LTE)

*  Augment or eliminate existing expensive MPLS links

*  Eliminate long timelines associated with the deployment of MPLS and ExpressRoute

*  No licensing cost for Citrix SD-WAN VPX (included with Citrix Managed Desktops)

Citrix SD-WAN features can help:

*  ICA connections between users and their virtual desktops and applications

*  Internet access from the virtual desktops and applications to web sites, SaaS apps, and other cloud resources

*  Access from the Citrix Managed Desktops back to on-premises resources such as Windows Active Directory Services and database servers

*  Real-Time Transport Protocol from the media engine in the Citrix Workspace app to cloud-hosted unified communications services such as Microsoft® Teams

*  Client-side fetching of videos from sites like YouTube and Vimeo and filtering the access using Secure Web Gateway and third party (Zscaler, Palo Alto, Symantec or Check Point) cloud security solution

[![CMD-Image-17](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_017.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_017.png)

The preceding diagram depicts a skeleton view of Citrix SD-WAN integration with Citrix Managed Desktops. The Citrix SD-WAN solution offers the easiest and best way to deliver Citrix Managed Desktops and all associated Windows, SaaS and cloud applications. It also enables the best virtualized **Microsoft® Office 365 experience**.

*  HDX real-time media processing performed directly on the user device for Microsoft® Teams

*  Local breakout of audio-video traffic from Citrix Managed Desktops will go through Azure SD-WAN VPX and then steer to nearest Microsoft® Office 365 PoP

*  Unified communications and video breakout to the internet

### The Key Benefits of Integrating Citrix SD-WAN with Citrix Managed Desktops

The Citrix SD-WAN solution, when used with Citrix Managed Desktops, provides various benefits compared to different hybrid connectivity solutions. A few of them are described below.

**Citrix SD-WAN offers the best end-user experience**

*  Reliable and high-performance connectivity through advanced SD-WAN features

*  Benefits across all connections (VDA-to-DC, user-to-VDA, VDA-to-cloud, user-to-cloud)

*  Reduces latency compared to backhauling traffic to the data center

*  Link bonding (more bandwidth for faster performance)

*  High availability with seamless link failover and SD-WAN redundancy on Azure

*  Optimized VoIP experience (packet racing for reduced jitter and minimal packet loss, QoS, local break-out for reduced latency)

*  Traffic management: QoS across HDX traffic stream, HDX fair sharing between users and QoS between HDX and other traffic

Reference: [Citrix SD-WAN integration with Citrix Managed Desktops](https://www.citrix.com/blogs/2019/10/02/network-connectivity-options-for-citrix-managed-desktops/)

## Citrix SD-WAN for Microsoft® Azure

Citrix SD-WAN and Azure cloud enable an organization to redesign their existing networks for optimized cloud access. Citrix SD-WAN delivers a simplified and cost-effective on-premises connection.

Citrix SD-WAN offers several benefits over Azure ExpressRoute. The primary advantage of SD-WAN is security. Many organizations prefer network architectures that integrate security, orchestration, and policy. Citrix SD-WAN covers all these factors with secured connectivity.

Benefits of Citrix SD-WAN for delivery with Citrix Managed Desktops.

*  SD-WAN provides economical and highly reliable connectivity

*  SD-WAN enables direct access to video-on-demand that is rendered from the customer data center

*  SDWAN provides intelligent traffic steering from the VDA to other on-premises properties

*  VoIP and real-time video traffic are navigated from the corporate data center

[![CMD-Image-5](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_005.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_005.png)

The above diagram depicts SD-WAN usage in a Citrix Managed Desktops environment using the customers Azure platform. Citrix SD-WAN provides two secure connectivity options:

*  Standard-based high-speed IPsec

*  Intelligent SD-WAN controller

Citrix SD-WAN is available directly from the Azure Marketplace. Customers can bring their own license and use it with a subscription or perpetual licensing options.

## Citrix Managed Desktops Deployment scenarios

Citrix Managed Desktops support multiple deployment frameworks along with user authentication. In a simplified way, these are based on either the Active Directory service or Azure Active Directory Domain service usage.

### Non-domain-joined catalogs

All non-domain joined catalogs feature VDAs which are not joined to a domain. In addition, the VDAs cannot have any access to an on-premises network.

Typical uses for non-domain-joined VDAs are DevOps and providing access to third-party users for code development and testing activities. In a normal workgroup, the client machine administrator does not have control over managing the users, deploy updates, and so on. But in Citrix Managed Desktops administrators have control over the machines and users the are authenticated. For user authentication there are three options:

1.  Non-domain-joined catalogs with Citrix managed Azure AD

    Citrix-managed Azure AD is used to manage users. Here customers don’t need to access resources from the on-premises network. This deployment is the simplest way to conduct POCs for non-domain-joined VDAs. Limited user management is performed through the CMD UI. The Citrix managed Azure AD does not offer all the options of Azure AD authentication, for example, MFA is not configurable with the Citrix managed Azure AD.

2.  Non-domain-joined catalogs with customer-managed Azure AD

    For end-user authentication, a customer-managed Azure AD likely to be used. Here customers can use Azure multifactor authentication (MFA) to help safeguard access to data and applications. This method is the simplest for end-users and MFA based configuration decisions taken care of by the administrators.

3.  Non-domain-joined catalogs with customer on-premises Active Directory

    End-user authentication uses a customer’s on-premises Active Directory service. Here the administrator has to install Citrix Cloud Connectors in the customer on-premises network. This method enables Citrix Workspace to access the on-premises Active Directory and authenticate users. However, the VDAs will not have any access to the on-premises network.

### Domain-joined catalogs

Domain-joined catalogs feature domain-joined VDAs that have access to a customer-managed Active Directory. This Active Directory is used for user authentication.

1.  Domain-joined VDAs using Azure Active Directory Domain Services (AADDS)

    In this type of deployment, the customer sets up an AADDS in their customer-managed Azure platform. They can establish a VNet peering between the Citrix-managed VNet and the customer-managed VNet so the VDAs have access to the AADDS. For end-user authentication, customers can use either Azure Active Directory that is available from their Azure Subscription, or they can use the AADDS for regular Active Directory authentication.

    The customer-managed Azure Subscription resources including AADDS are being leveraged using VNet peering between two virtual networks within that region or location. Azure AD Domain Services is featuring all types of domain services including domain join, group policy, LDAP, Kerberos/NTLM authentication that is compatible with Windows Server Active Directory.

2.  Domain Joined VDAs using customer on-premises Active Directory Domain Services

    In this type of deployment, all VDAs are domain-joined. For end-user authentication, customers can use Windows Active Directory Domain services hosted in their on-premises environment. Here Citrix Managed Desktops is running in any part of the Azure region (within CMD scope).

[![CMD-Image-6](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_006.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_006.png)

In case the customer is using Azure Active Directory authentication, the customer has to install Azure AD connect. The organization must run Azure AD connect on their data center which will be synced with Azure AD. Azure AD is hosted on Azure by the customer using their Azure Subscription. There will be a VNet peering required with the Citrix Managed Desktops environment. For authentication, this type of deployment leverages the customer AD hosted within their data center.

Reference: [Deployment scenarios](/en-us/citrix-managed-desktops.html)

## Profile Management

The user’s personal settings that are applied to the user’s virtual desktop and applications are retained using profile management. Citrix Profile management ensures that personal settings, documents, shortcuts, templates, desktop wallpapers, cookies, and favorites always follow the user in non-persistent desktops.

The Active Directory Group Policy Objects allow the administrators to control the behavior of the Citrix user profiles. Profile Management optimizes profiles in an easy and reliable way. During, the logoff and at interim stages, registry changes, files, and folders in the profile are saved to the user store for each user.

Citrix Managed Desktops offers multiple catalogs including multi-session, random-desktops supported by Windows 10 EVD and Windows 2016 servers. Customers have to create a user store in their Azure Subscription and profiles are fetched and written to the customer’s file share. The administrators have to make sure there is reliable network connectivity between Citrix Managed Desktops (multi-session and random catalog) and the file servers storing the user profiles.

[![CMD-Image-7](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_007.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_007.png)

The user store is the central network location for storing Citrix user profiles. File servers are created in customer’s Azure Subscription with Availability set. For a user store, any SMB or CIFS files share that can be used. Make sure that the shared path is accessible by the accounts used with Citrix user profiles.

Profiles are loaded to any non-persistent desktops from that shared path over the network. VNet peering uses Azure backbone which provides low latency and robust network. VNet peering supported within the region, so this option is scoped singled region or location.

Reference: [Profile Management](/en-us/profile-management/current-release.html)

## Profile Management using FSLogix profile container with Azure Files

Most organizations integrate some form of a profile management solution. As a result of the profile solution, end-users can sometimes face challenges in accessing their profile over the network due to the profile management being unable to handle large files and modern settings.

To overcome these problems Microsoft® introduced the FSLogix container solution for profile and office 365. FSLogix provides the best performing end-user computing environment, reduces management costs, simplifies the computing infrastructure.

### FSLogix Profile Containers

In Citrix Managed Desktops, multi-session and random desktop catalogs will have user profiles that are not retained after logoff. FSLogix is designed to roam profiles in a Citrix Managed Desktops environment. This solution is accomplished by allowing the complete user profile to be stored in a single container.

During logon, the container is dynamically attached to the machine (VDA) that is running on the Citrix Managed Azure Subscription using VHD or VHDX Microsoft® services. These profiles are available to the user’s system as a native-like user profile. The large files are elegantly handled by the FSLogix Profile Containers solution to help ensure optimal user experience.

### Office 365 Containers

Users demand productive experience with Microsoft® Office including fast email, fast searching, fast access to OneDrive files, and so on. The data and search indexes are stored in the container vs the whole user profile.

This solution easy to integrate over any existing profile management solution. In case using a profile management solution and office container, it is recommended to exclude the portion of the profile managed by Office 365 container.

### Azure Files

As previously stated, the user profile is stored as a container and this container is dynamically attached to the VDAs during logon. Supported by VHD or VHDX the user profile containers are immediately available and appear after user login. Containers or disks stored in the cloud that are accessible via SMB protocol. Azure Files offers complete management of file shares hosted in the customer-managed Azure Subscription.

Azure Files are easily managed through the Azure portal, providing greater resiliency (LRS, ZRS). In terms of scalability, it can grow to 100 TB with multiple partitions. There are also advantages of performance factors given that Azure files provide 10,000 to 100,000 IOPS.

[![CMD-Image-8](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_008.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_008.png)

The administrator has to create Azure Files for storing profiles in customer-managed Azure subscription. The above picture shows Citrix Managed VDAs are fetching the profiles from Azure Files over the network. The customer has to set up the VNet peering between the subscriptions.

FSLogix profile container binaries have to be installed on the base image during the master image preparation. Once all the tools and applications are installed, the VM has to be shut down. When the user logs in to a non-persistent VDA (newly created) a profile is created on the file share. Any changes made in that desktop will be stored on that particular container.

Learn more about the FSLogix Profile container solution, at this [link](https://docs.microsoft.com/en-us/azure/virtual-desktop/fslogix-containers-azure-files).

Reference: [FSLogix](https://docs.fslogix.com/)

## Monitoring of Citrix Managed Desktops

The Citrix Managed Desktops **Monitor** dashboard gives the details of desktop usage, sessions, and machines in the deployment. The administrators can also control sessions, power-managed machines, end-user running applications, and end-user running processes.

Monitor provides:

*  Real-time data from the CMD services running in the background

*  Historical data stored in the Monitor database to access the usage report

*  Visibility of machines running on Citrix Managed Azure subscription

*  Controlling capability for applications running on the VDA

*  Also provides session control capability with power management options

[![CMD-Image-9](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_009.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_009.png)

The Managed Desktop usage page appears when the administrator selects the **Monitor** tab. Here it gives complete information about all catalogs. It also gives graphs with the number of powered-on machines and peaks concurrent sessions at regular points during the time period selected.

### Controlling user’s applications and sessions

From the Monitor dashboard, administrators can apply filters to log off or disconnect a session. This option can be found by searching for a user.

### Power-control machines

Single session or multi-session machines are displayed by applying the filtered search. By clicking on Power Control action on the portal the administrator gets an option to restart, force restart, shutdown, force shutdown, and start the machines running on the Citrix Managed Desktops environment.

Reference: [Monitor](/en-us/citrix-managed-desktops/monitor.html)

## User Access and Authentication

Citrix Workspace app is the entry point to access desktops and apps running on the Citrix Managed Desktops platform. The user has to first be authenticated when they log in to Citrix Workspace. Citrix Managed Desktops supports the following user authentication methods:

*  Citrix Managed Azure AD (AAD)

*  Customer Managed Active Directory

*  Customer Managed Azure AD

**Citrix Managed Azure AD:** Azure Active Directory service is provided and managed by Citrix. Here the administrator does not need to provide or own any Azure infrastructure. Citrix has made it simple to manage, the admins just have to add their users to the directory using the CMD UI.

**Customer Managed Active Directory:** In this authentication method, customers are using their on-premises Active Directory services or leveraging the Azure Active Directory service.

[![CMD-Image-10](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_010.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-managed-desktops_010.png)

One option is to synchronize user identities between cloud and local directories so that Citrix Managed Desktops users can access their resources from Citrix Cloud. Using a single set of credentials users are able to access their CMD resources through Citrix Workspace App. To achieve this, customers have to install and configure Azure AD Connect on their on-premises.

Azure AD Connect sync takes care of all the operation that is related to synchronize identity data between the on-premises environment and Azure AD.

Another option is to use **customer-managed Azure AD** with a non-domain joined catalog. In this scenario, the customer-managed Azure AD would be entirely in the cloud with no on-premises connection or synchronization. This method is ideal for customers who do not have any legacy Active Directory or on-premises infrastructure.

Reference: [User Authentication](/en-us/citrix-managed-desktops/configure-manage.html#add-and-delete-users-in-managed-azure-ad)

### Connect Citrix Cloud to Azure AD

To connect the existing Azure Active Directory with Citrix Cloud, the administrator must have Global Admin privileges in the Azure AD. Citrix Cloud needs permission to access the user profile in addition to the basic profiles of the users in the Azure AD. The administrator has to select the “Identity and Access Management” from Citrix Cloud. Fill in all the required details and sign in to the Azure account to connect. Then Citrix Cloud accesses the account and acquires the information required for connection.

After the Azure AD user accounts are connected, users can sign in to Citrix Cloud using a URL that is configured during the initial connection or by selecting “Sign in with my company credentials”.

Reference: [AAD to Citrix Cloud](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html)

Advanced multifactor authentication is provided by Azure AD. Turn-on the available features on the customer Azure AD, which enables Citrix Cloud users to leverage those capabilities by default.

## Image Management

Image Management is the process of creating a master or golden image that contains the operating systems and all the required applications. A single image is delivered to multiple target virtual machines by the image provisioning mechanism. The Citrix Managed Desktops solution uses Citrix proven Machine Creation Service (MCS) technology to provision virtual machines on the Citrix Managed Azure Subscription.

Machine Creation Services configures, starts, stops, and deletes virtual machines using Microsoft® Azure APIs. MCS is a disk-based provisioning approach that integrates well within the Microsoft® Azure cloud platform. Citrix Managed Desktops provides several Citrix-managed master images:

*  Windows 10 Enterprise (single session)

*  Windows 10 EVD and Windows Server 2016 (multi-session)

Citrix Managed Desktops supports both server and desktops OS environments. Citrix administrators can create three types of machine catalogs using Citrix Machine Creation Services. Machine types are:

*  **Multi-session** - Windows 10 EVD (Multi-session) or Windows Server 2016 with latest VDA

*  **Static (personal desktops)** - Windows 10 Pro with latest VDA

*  **Random (pooled desktops)** - Windows 10 Pro with latest VDA

Citrix offers customers an option to build their own image by using existing master images. These master images are used by the administrator to create a virtual machine to build their customized image for Citrix Managed Desktops.

### Import a master image from Azure

In the case customer has their customized images available in their Azure Subscription, images are directly imported to CMD. The administrator has to enter the Azure-generated URL for the Virtual Hard Disk (VHD).

Citrix runs the validation test on the imported image. The administrator has to make sure the image has all the requirements to run on Citrix Managed Desktops. The requirement is:

*  Operating system support (Windows 10 Enterprise, Windows 10 Enterprise Virtual Desktop preview, or Windows Server 2016)

*  No configured Delivery Controllers

*  Valid Citrix VDA newer than 7.11 installed and matching the operating system (e.g. server VDA on server OS)

*  The personality.ini file must exist on the system drive (VDA is set to MCS provisioning)

Learn more about importing a master image from Azure and installation of VDA on the master image, at this [link](/en-us/citrix-managed-desktops.html#about-master-images).

To create and managed master images refer to the [link](/en-us/citrix-managed-desktops/configure-manage.html#prepare-a-new-master-image).

### Optimization of the image

The image may contain multiple unused services that used to create the machine catalog on Citrix Managed Desktops. OS optimization helps to remove or disable unwanted services running on an image. Citrix has developed an optimization tool called “Citrix Optimizer.” This tool helps Citrix administrators to optimize various components in an environment. The tool is PowerShell based and also includes a graphical UI.

Citrix Optimizer runs in three different modes:

*  Analyze - analyze the current system against a specified template and display any differences

*  Execute - apply the optimization from the template

*  Rollback - revert the optimization changes applied previously

The administrator has to choose the relevant template from the selection for OS optimization. Also, the administrator has an option to select and disable unwanted services from the image. By disabling such services master image becomes easier to use on the Azure cloud platform. This tool helps in optimizing resource consumptions and overall performance.

Learn more about the installation and updates of Citrix Optimizer [CTX224676](https://support.citrix.com/article/CTX224676).

## Licensing and Azure Subscription

Licensing for Citrix Managed Desktops handled by Citrix. The administrator has privileges to set up a Microsoft® RDS license server for Windows Server workloads.

The multi-session catalog requires a Remote Desktop Services client access license (RDS CAL). The Remote Desktop Services license server issues client access licenses to devices and users. The administrator can activate the license server by using the Remote Desktop Licensing Manager.

Activate the license server in any one of the available machines through Remote Desktop Licensing Manager. The VM must always be available and Citrix VDAs must be able to reach this license server. Specify the license server address and per-user license mode using Microsoft® Group Policy. The Remote Desktop licensing mode configured on the remote desktop server must match the type of RDS CALs available on the license server.

### Azure Subscription

Citrix Managed Azure Subscription: This subscription is completely owned by Citrix, where the end user's desktop is running on this platform.

Customer Managed Azure Subscription: This subscription owned by a customer running their own licensed workloads on Microsoft® Azure platform has to go through the Azure Hybrid Benefit.

### Bring-Your-Own Azure Subscription

The Citrix Managed Desktops solution allows customers to use Citrix’s Azure subscription or a customer’s Azure subscription. Customers can bring their own Azure subscription which in turn allows any existing Microsoft® Azure customer to easily adopt Citrix Managed Desktops as part of their overall Azure cloud strategy.

To enable BYO Azure flexibility, a customer must add one or more existing Azure subscriptions to Citrix Managed Desktops.  This action authorizes Citrix Managed Desktops to access customer subscriptions.

There are a few limitations on the BYO Azure subscription model that are described below:

*  Supports only domain-joined catalogs

*  Customers can only get a “custom to create” catalogs option

*  Customer must create a new virtual network or have the option to select the existing one

With the BYO Azure subscription model, the customer has full control over the VDAs running on their subscription. Also, image management becomes easier in terms of importing the image and managing the existing images.

Reference: [Bring-your-own Azure Subscription](https://docs.citrix.com/en-us/citrix-managed-desktops.html#about-azure-subscriptions)

### Windows 10 EVD licensing

Windows Virtual Desktops (WVD) also known as Windows 10 Enterprise Virtual Desktops service and capabilities are extended and enriched by Citrix Managed Desktops through Citrix Cloud. This progression of remote desktop service available only on the Microsoft® Azure platform.

To deploy and manage a multi-session catalog using Windows Virtual Desktops customer gets a Windows 10 Enterprise E3 entitlement. A new Windows 10 EVD operating system licensed as part of Windows Virtual Desktops in Azure.

Additional benefits of Windows Virtual Desktop are customer gets Windows 7 virtual desktop with free Extended Security Updates. WVD offers flexible service allowing administrators to virtualize both desktops and apps.

## Best Practices and Design Considerations for Citrix Managed Desktops

While adopting the Citrix Managed Desktops service for an organization, solution architects and administrators have to consider best practices and design considerations. It is important to align these with business needs.

The Citrix Managed Desktops service components computing, storage, and networking (in Citrix managed Azure subscription) are all managed by Citrix. The architects do not need to worry about the infrastructure side unless on-premises connectivity is required. There are a few important points to considered though:

*  Conducting a pre-assessment before moving to CMD

*  Deployment considerations and image optimization

*  Power Management

*  Security

*  Applying patches and antivirus software

*  Multifactor authentication

*  Profile management

### Conducting a pre-assessment before moving to Citrix Managed Desktops

To gain an understanding of the workloads, the solution architect has to do pre-assessment of the desktops and applications that will run in the Citrix Managed Desktops environment. It is critically imperative to design and gather resource requirement data for CMD.

Pre-assessment is useful to determine the specific applications required for the users, the licensing requirement, and calculate the cost associated with the deployment. Citrix provides the Citrix Managed Desktops cost calculator to understand cost estimation for the deployment. Actual prices may vary depending upon other factors including date of purchase, type of agreement with Citrix, and so on.

Citrix Managed Desktop Cost Calculator [link](https://costcalculator.apps.cloud.com/daas/advanced).

### Deployment considerations and image optimization

Many organizations confront the challenge of effectively scaling resources to meet demand. In case merger and acquisition take place then the organization faces challenges when the data center is under-resourced. Citrix Managed Desktops makes it possible to mitigate the scalability constraints. Customers can provision compute on an availability region nearest to their data center situated with reliable connectivity.

All the end-users use Citrix Workspace app to access their resources and that is the only single-point of entry. In case customers are using a file server or any other services running on their Azure Subscription, it is the customer’s responsibility to implement VNet peering between subscriptions. At the backend, the Azure backbone network with a bandwidth of 25 Gbps used to link between subscriptions.

There is a cost associated with CPU, memory, and bandwidth consumption so customers have to plan their deployment depending on the need.

Image Optimization is an important task where administrators have to perform on the master image. Disable all the unwanted services and remove the application which is not used by users. This optimization reduces fewer CPU cycles and memory consumption.

### Power Management

Power management options are available for catalogs containing multi-session and single-session machines. This task helps in minimizing the cost associated with resource consumption on the Citrix Managed Azure platform.

The administrator has an option to set the working hours depending on the time zone. In some of the use cases, machines do not need to be powered-on after working hours. In such scenarios, power management is a viable option for administrators to power-off those machines. Here administrators can set the start and stop time. Perhaps there are other options available as well that are tuned by administrators to get the best balance of price to responsiveness:

*  Disconnect idle sessions: Idle desktops are disconnected, and the user has to login to Workspace to start the machine

*  Log off disconnected session: The administrator can set the time to log off disconnected sessions. Users experience longer logon times and lose any unsaved work, but the machine can power off if there are no logged-in sessions

*  Power-off delay: Set the time for a machine to be powered-on before it is eligible for power-off

*  Capacity buffer option: Gives the option to meet a sudden spike in demand by keeping a buffer of machines powered-on. This step has to be entered in percentage, a lower value decreases the cost while higher value ensures optimal user experience (no wait time)

To create a power management schedule, refer to the reference section.

Reference: [Power Management](/en-us/citrix-managed-desktops/configure-manage.html#power-management-schedules).

### Security

Secure virtual desktops significantly mitigate risks to a company by being within the protected network. Network-level isolation secures the Citrix Managed Desktops from external threats. Only outbound connectivity to the Internet is enabled by default in virtual machines. To access these machines Citrix Workspace app is the single entry point for end-users. The administrators can use the support and troubleshoot option for machines that are domain-joined. Citrix prepared images to have Windows Defender Antivirus enabled by default. The customer has the option to make their images more secure during the master image preparation by installing different antivirus software. There are other considering factors an administrator need to take into the account when considering security:

*  Applying patches and antivirus software: During the master image creation, it is best practice to apply relevant patches. This process makes the environment safe from malicious users that exploit vulnerabilities left open when operating systems are not kept up-to-date

*  Multi-factor authentication: This layer of security in the form of multifactor authentication protects critical corporate resources accessed via Citrix Managed Desktops solutions. Most of the data breaches arising from credentials compromised by enabling MFA it mitigates the risk

Azure multifactor authentication helps safeguard access to Citrix Managed Desktops. With Conditional Access, the administrator can implement automated access control for accessing Citrix Managed Desktops based on conditions. To learn more about Azure MFA follow the Microsoft® documentation.

For more information on security refer to the technical security overview documentation.

Reference: [Technical security overview](/en-us/citrix-managed-desktops/security.html)

### Profile Management

Profile management ensures that the user’s personal settings are retained for non-persistent desktops. Profile management gives reliable roaming experience to users. The end-user personal settings, documents, shortcuts, templates, desktop wallpapers, cookies, and favorites always follow the user across different Windows machines on any device. Before implementing profile management on the environment few factors to be considered. This includes the type of catalog, understanding the application behavior, pilot test on a few users then going for all users, and network share to store all the profiles.

For a detailed explanation of deciding factors, refer the [Profile management](/en-us/profile-management/current-release.html) documentation from Citrix.

For the user share to store the profile, it is recommended to create a Windows file share on customers Azure subscription and all the user profiles are stored in that particular share. During, the login, the profile is fetched over the network using Azure backbone network. This reduces the latency and faster logon times to the machines.

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sef322f326524856b).

## References

The following resources are referenced for a better understanding of Citrix Managed Desktops:

[Citrix Managed Desktops](/en-us/citrix-managed-desktops.html)

[CMD Architecture](/en-us/citrix-managed-desktops/security.html)
