---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Shoaib Yusuf
description: Provides optimal network connectivity between Enterprise branch offices and their Workspace hosted in data resource locations on-premises or in the cloud.
tz_title: SD-WAN for Workspace
tz_products: citrix-networking;
---
# Tech Brief: SD-WAN for Workspace

## SD-WAN Overview

SD-WAN is a software defined approach to managing enterprise WANs to provide optimal network connectivity between enterprise branch offices and data resource locations on-premises or in the cloud.

![ Citrix Software Defined WAN (SD-WAN)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_arch1.png)

### Citrix Software Defined WAN (SD-WAN)

Citrix SD-WAN solves complex routing with simplicity. Each SD-WAN instance hosted in on-premises office locations, or Hybrid cloud hosted resource locations, across an Enterprise intranet communicate regarding their view of the network in real time. They establish a web of virtual paths between virtual IP addresses assigned to each interface type and establish secure virtual tunnels across path options between locations. Then they identify traffic, and route it on a per packet basis across the optimal path with knowledge of the quality each path can offer including latency, loss, and jitter characteristics and reassemble flows for delivery at their destination providing the best possible application user experience.
![Traffic Optimization](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb1.png)

## SD-WAN Editions

Citrix SD-WAN is available in three editions.

### Standard Edition (SE)

The Standard Edition or SE focuses on traffic optimization. It aggregates bandwidth across available WAN links, monitors path quality in real-time, and routes traffic based on priority and session characteristics.

*  **WAN Aggregation** - over diverse network links with real-time unidirectional per packet dynamic path selection.
*  **Quality of Service** - prioritization with advanced queuing and bandwidth allocation algorithms.
*  **Network Control** – with dynamic Routing, stateful Firewall, and web Gateway integration.

### WanOp Edition (WO)

The WanOp Edition focuses on session optimization. It optimizes session bandwidth use to maximize throughput and is an ideal remedy for branch offices with limited leased lines and minimal bandwidth.

*  **Acceleration** - proxies client-server TCP connections to implement protocol level changes to maximize session throughput across the network.
*  **Caching** - stores session content segments on the WAN Edge pairs and transmits token markers in their place to save bandwidth.
*  **Compression** – applies standard TCP supported protocols to uncompressed traffic to further reduce session bandwidth.

### Premium Edition (PE)

The Citrix SD-WAN Premium edition combines the functionality of the Standard and WanOp editions to offer a broad set of differentiated use cases to optimize Enterprise networks.

![Optimizing Citrix Virtual Apps & Desktops](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb2.png)
For more information see:
[HDX](https://www.citrix.com/blogs/2014/08/18/citrix-xendesktopxenapp-what-is-hdx-its-not-just-ica/)
[EDT](https://www.citrix.com/blogs/2017/11/17/hdx-adaptive-transport-and-edt-icas-new-default-transport-protocol-part-i/)
[ICA](https://blog.citrix24.com/citrix-multistream-ica-explained/)

## Key Workspace Use Cases

### HDX Session and Traffic Optimization

Citrix Virtual App and Desktop sessions are delivered by the HDX protocol suite. The protocol provides the ability to tag session traffic by using a unique port or QOS tag. Yet either options might require extensive firewall changes and or routing QOS changes to implement. Also, they do not utilize a multi-path approach whereas Citrix SD-WAN can uniquely identify Multi-stream ICA, using the default configuration, over a single port and prioritize traffic over multiple paths simultaneously according to QOS requirements.
![ICA single port Auto-Qos](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb3.png)

### Citrix SD-WAN Virtual Instance for Azure Resource Locations

Microsoft Azure is a popular cloud platform and many Citrix customers utilize it to host hybrid Resource Location systems such as Virtual Delivery Agents (VDA). Both the [SD-WAN VPX Standard Edition](/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html) and [WANOP Edition VPX](/en-us/citrix-sd-wan-platforms/vpx-models/vpx-wanop/install-on-azure.html) can be deployed in Microsoft Azure and integrated with an Enterprise SD-WAN environment to facilitate such features as link bonding, instantaneous failover and selective packet duplication to Azure hosted resources.
![ Azure hosted Citrix SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_arch2.png)

### Integration with Microsoft Azure global Virtual WAN

Citrix SD-WAN solution is a preferred partner for Microsoft Azure Virtual WAN. Citrix SD-WAN can be used to facilitate Azure Virtual WAN gateway service, for IPSEC connectivity and automated setup, enabling ease of management of scaled deployments.
![ Citrix SD-WAN with Microsoft Azure global Virtual WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_arch3.png)

## Topologies

Citrix SD-WAN monitors and manages a complex web of paths with various delivery characteristics across the corporate network to deliver traffic with optimal user experience.

### Underlay Network

Operates across the existing enterprise network. The deployment team consults current network diagrams to design the SD-WAN integration to take advantage of existing network links and equipment.

### Management Network

The first step in implementing and managing SD-WAN is applying an IP address from the existing management network to SD-WAN platforms in branches and data centers across the corporate intranet.

### Overlay Network

Operates on top of the underlay network using secure UDP tunnels between Citrix SD-WAN platforms. With full visibility across the WAN a complex web of available paths with measured QOS characteristics are used to route traffic efficiently.

![Branch Hardware Consolidation](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb4.png)

## Deployment Modes

Citrix SD-WAN offers flexible options for deployment depending on use cases, or high availability requirements in the primary Data Center, Branch, or Cloud Resource locations.

### Inline

Hardware appliances can be deployed seamlessly in the path between the LAN and the WAN edge network. If the device goes offline for any reason the physical Ethernet connection, through the equipment between switches or routers, remains intact.

### Virtual Inline

An approach that offers similar usage to Inline, without the need to physically interrupt the primary data center network. The Citrix SD-WAN equipment is implemented on a VLAN next to the Edge router which uses policy routing to direct pertinent traffic to it for optimization.

### Edge / Gateway Mode

Takes over responsibility for WAN edge networking including terminating leased lines, routing, firewall, and internet proxy filtering in addition to providing full SD-WAN functionality. This mode allows enterprises to consolidate branch hardware to maximize network throughput, availability, and security while minimizing complexity and cost.

### Fail-to-Wire

Hardware platforms include fail-to-wire (Ethernet bypass) cards for direct in-path deployment. If power fails, a relay closes, and the input and output ports become electrically connected, allowing the Ethernet single to passthrough from one port to the other, defaulting to the existing underlay network.

### High-Availability

The various deployment options (Inline, Virtual Inline, Edge) support high-availability. A pair of devices can be deployed at a site location in Active/Standby roles. This high availability deployment operates similar to a Virtual Router Redundancy Protocol (VRRP), ensuring the SD-WAN Overlay is continuously active.

### Geographically Distributed MCN

A secondary data center or branch site can be assigned the role of Secondary MCN (also known as GEO MCN). This site takes the responsibility of the primary data center SD-WAN and ensures that the SD-WAN Overlay continues to operate until the primary site comes back online.

![Zero Touch Deployment](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb5.png)
For more information see:
[Zero Touch Deployment service](/en-us/citrix-sd-wan/10-1/use-cases-sd-wan-virtual-routing/zero-touch-deployment-service.html)

## Deployment

### Initial Setup

Citrix SD-WAN supports various hardware and software [appliances](https://www.citrix.com/products/citrix-sd-wan/citrix-networking-data-sheet.html), including support for leading Cloud platforms, hypervisor platforms, and Citrix SDX, including various speeds and feeds with flexible resources to meet throughput needs. Some models oriented for branch offices may be configured using the Zero Touch Deployment Cloud Service whereby once units are connected to the network and powered on they are able automatically contact the cloud and obtain the configuration made centrally by and administrator. Other models can easily be configured through their console to access the management network.

### Initial Configuration

The configuration GUI can be accessed by connecting to the management IP with a browser. Under the **Configuration** tab licensing is applied in the Appliance Settings, settings like the date and time can be configured under System Management and the Configuration Editor to configure the overlay network and SD-WAN functionality can be found under **Virtual WAN**.
![Configuration Editor](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb6.png)

### Initial Deployment

The global configuration of all SD-WAN appliance is done central on the Master Control Node (MCN). The first configuration package for each is downloaded through the MCN and applied to the GUI of each branch, yet all further configuration is done centrally through the MCN. See SD-WAN documentation more information.
![Change Management](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb7.png)

## Monitoring & Management

### Master Control Node (MCN)

The monitoring tab in the GUI connected to the MCN shows essential information such as WAN link status, throughout, and overlay paths.
![Paths Status](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb8.png)

### SD-WAN Orchestrator

A cloud-hosted, multitenant management SaaS offering available through Citrix Cloud. It provides a central management and reporting with multitenancy administration. Contains key capabilities such as guided workflow for site creation, site profiles/templates for easy management of network configuration.

### SD-WAN Center

An on-prem, customer managed, SD-WAN management solution. A software appliance that can be installed on most popular hypervisor platforms. It provides more comprehensive reporting on the SD-WAN environment and is essential to some configuration tasks such as integration with Azure virtual WAN and Zero Touch Deployment.
![Paths Status](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb9.png)

### Application Delivery Management (ADM)

ADM is a Citrix Cloud service that provides broad visibility and management capabilities across the Citrix Networking product. It can add Citrix SD-WAN WanOp instances to provide advanced workspace session flow information using Deep Packet Inspection (DPI) capabilities.
![Deep Packet Inspection](/en-us/tech-zone/learn/media/tech-briefs_sdwan-workspace_exhb10.png)
