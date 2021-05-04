---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Dan Feller, Derek Thoslund, Jaskirat Singh Chauhan
description: Learn how to optimize the delivery of Citrix Virtual Apps and Desktops from on-premises servers to users at locations with a Citrix SD-WAN appliance by minimizing latency and improving session responsiveness during network issues.
tz_title: Citrix SD-WAN HDX performance improvements for Citrix Virtual Apps and Desktops on-premises environments
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: Citrix SD-WAN HDX performance improvements for Citrix Virtual Apps and Desktops on-premises environments

## Overview

This Reference Architecture explains how to optimize the delivery of Citrix Virtual Apps and Desktops from on-premises servers to users at locations with a Citrix SD-WAN appliance, by minimizing latency and improving session responsiveness during network issues.

Citrix Virtual Apps and Desktops can deliver virtual apps and desktops sessions to remote user endpoints via Citrix Gateway. When users select a virtual app or desktop in their Workspace App their endpoint is directed to resolve an FQDN. With the public IP address returned they connect to the Gateway via the Internet or VPN.

However, users connecting from a branch with a high speed connection to the Data Center where the virtual workload is hosted typically have a better path with less latency directly over the Enterprise intranet. At the same time, to provide the best user experience Citrix Virtual Apps and Desktops sessions need Quality of Service applied. Depending on the tasks the user is performing, the High Definition User Experience (HDX) sessions can be delivered using separate streams by class of service.

Citrix SD-WAN is a software defined approach to managing enterprise WANs to provide optimal network connectivity between Enterprise branch offices and workloads hosted on-premises or in the Cloud. Citrix SD-WAN can automatically disseminate an HDX session in streams according to class of service between appliance virtual paths and deliver them according to their Quality of Service (QOS)requirements.

Yet, to provide this functionality, Edge SD-WAN appliances must be able to inspect the HDX stream unencrypted. The Workspace client connects to Citrix Gateway, encrypted with HTTPs, for session management including authentication and resource enumeration. However, for app delivery the client must connect directly to StoreFront, and the Virtual Delivery Agent (VDA) it assigns, via the SD-WAN overlay network.

With Citrix [Beacons Points](/en-us/storefront/1912-ltsr/integrate-with-citrix-gateway-and-citrix-adc/configure-beacon.html), endpoints are provided with a provisioning file including links to test reachability. When “internal” links are reachable they connect directly to StoreFront. It provides the client with the internal address of the target VDA, and subsequently directs the HDX session via the SD-WAN overlay network.

[![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_overview.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_overview.png)

## Recommendation

Design Citrix Virtual Apps and Desktops environments with Beacon Points to direct branch endpoints, on the internal network, directly to StoreFront for assignment to the internal address of a VDA for app delivery. Integrate with Citrix SD-WAN so that the HDX sessions being sent over the internal network are automatically separated into multiple ICA streams by class of service, given the appropriate QOS tag, and prioritized accordingly. In this document we discuss the Conceptual Architecture and the Detailed Design of this recommendation.

## Conceptual Architecture

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_conceptualarchitecture.png)

### Components

Citrix Virtual Apps and Desktops is built on a modular architecture which allows it to scale and support access on any device, anytime, from anywhere. To best understand the architecture conceptually we consider 5 distinct layers and their pertinent components.

#### User Layer Components

The User Layer is where the supported devices host the Workspace App and users authenticate to obtain their Workspace resources.

*  **Citrix Workspace App** – is the Users’ portal to their Workspace resources from their device/s. It presents the user with authentication prompts and then their available resources.
*  **Citrix SD-WAN (Branch)** – Here SD-WAN provides several benefits.
    *  Multi-stream QoS prioritization - the branch appliance identifies HDX sessions from the endpoint and automatically identifies and breaks them into streams by Class of Service. It then queues and routes traffic by priority across available virtual paths. The virtual paths are encrypted from the branch to the data center.
    *  Reduced Latency – Citrix SD-WAN appliances automatically build virtual paths between each other across all available paths. They also monitor path performance in real-time, and route traffic over the path with the least latency.
    *  Resiliency – Citrix SD-WAN integrates with MPLS circuits, or the internet over various media including cable or DSL. If a link fails, with real-time traffic monitoring Citrix SD-WAN is able to reroute traffic, with minimal user impact and often without session layer timeouts.
    *  Bandwidth Maximization – Traditional routers are typically limited to routing over a single “best” path. Citrix SD-WAN can take advantage of all available links and simultaneously. It routes traffic on them, provided quality is maintained, based on real-time performance metrics.
    *  Quality of User Experience (QOE) – available through [Citrix Orchestrator](/en-us/citrix-sd-wan-orchestrator), QOE monitors HDX quality, and quantifies experience by session, user, or site. It assigns scores and reports in ranges including, Good (71-100), Fair (51-70), Poor (0-50), which allow for admins to prioritize focus.

#### Access Layer Components

The Access Layer is where network delivery components are hosted to coordinate and direct user sessions request to the Control and the Resource components.

*  **Citrix Gateway** – The Citrix Gateway, hosted on Citrix ADC appliances, proxies secure HTTPs access to Citrix Virtual Apps and Desktops environments. It filters against unwanted internet probes, offloads SSL traffic, and can provide multifactor authentication during _Workspace setup_. For remote users it also proxies _session delivery_, and provides policies to shape session characteristics.
*  **Citrix SD-WAN (Data Center)** – Typically the SD-WAN Master Control Node (MCN) is deployed in the primary data center. It is used to coordinate the overlay network, and establishes secure tunnels to the branch appliances.
VDAs can be collocated with delivery components in the data center, or be accessible from the Data Center via a tunnel or dedicated circuit. For more information on utilizing SD-WAN to provide access to Azure hosted VDAs see [Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity](/en-us/tech-zone/learn/poc-guides/sdwan-cloud-to-onprem-connectivity.html).
*  **StoreFront** – provides a unique role coordinating virtual app and desktop delivery on-premises. It acts as a hub communicating with access, control, and resource layers to present and deliver sessions. Remote Users, authenticated by Citrix Gateway, are passed through for session delivery. Branch users initially authenticate to Citrix Gateway to obtain their resources and the provisioning file with Beacon Point details. When they launch resources, they are directed to a StoreFront store configured for internal access.

    Once branch users select a resource, and the internal beacon points are reachable, the Workspace client is directed to StoreFront for authentication. Typically, StoreFront is configured to seamlessly authenticate domain joined endpoints, so there is no impact to the user experience. Alternatively, for non-domain joined endpoints Enterprises can configure single factor authentication. Thereafter endpoints are provided with an ICA file, with the internal IP address of their target VDA, that is accessed via the SD-WAN overlay network.

#### Control Layer Components

The Control Layer includes essential management components to coordinate the delivery of virtual apps and desktops. Thanks to the modular design this essential layer is largely unaffected by user and access layer delivery changes.

*  **Delivery Controller** – is the intelligence behind making apps and desktops into virtual sessions.
*  **Database, Studio, License server/s** - all work in conjunction to monitor and manage delivery of Citrix Virtual Apps and Desktops.

#### Resources Layer Components

Citrix Virtual Apps and Desktops resources come in various shapes and sizes depending on requirements like the operating system and capacity.

*  **Virtual Delivery Agent (VDA)** – All resource types rely on the VDA to deliver their content as a session. Thanks to the modular architecture VDAs can be collocated with Control components or hosted elsewhere such a public or private cloud.
*  **Optional - Citrix SD-WAN (Resource Location)** – SD-WAN provides maximum benefit when implemented in the Resource Location in close proximity to the VDA. It is supported on various hosts.

#### Host Layer Components

Access, Control, and Resource components can be hosted on-premises, Cloud, or Hybrid Cloud. The type of host dictates the available Citrix SD-WAN appliance platform.
For more information on these components see [Citrix Virtual Apps and Desktops architecture](/en-us/tech-zone/learn/downloads/diagrams-posters_virtual-apps-and-desktops_poster.png)

### Use Case Flows

Here we discuss session management and delivery flows for two use cases:

1.  Remote Users (External) – the “baseline” use case where we consider how all remote users, that have no access to the corporate intranet, can access and, use their Citrix Virtual Apps and Desktops environments.
2.  Branch Users (Internal) – the use case where we consider how branch users, that have access to the corporate intranet, can access and, use to their Citrix Virtual Apps and Desktops environments. Branch users can be at-home workers with a Citrix SD-WAN appliance.

#### User Authentication and Resource Enumeration

Branch users on the internal network as well as remote users that are external authenticate and obtain their resources via Citrix Gateway. The Gateways typically resolve to a common FQDN for all users. Enterprises often host the Gateways in a data center DMZ that resolves to a public IP address reachable via the internet. Enterprises can choose to host a Gateways inside the Data Center network that resolves to an internal private IP address for internal clients.

Users can be assigned to the same “external delivery group”, in the Citrix Delivery Controller, indicating they are external to the data center, whether they have access to the internal network. When users work outside of the branch, they would see the same resources in the Workspace App. Otherwise users can be assigned to a “branch delivery group”, or “remote delivery group” depending on their primary work location.

##### Remote Users (External)

Remote Users (not on the internal network) route to a Citrix Gateway, from their remote location, to authenticate and obtain their resources.

[![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_remoteusersauth.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_remoteusersauth.png)

##### Branch Users (Internal)

Branch users route to Citrix Gateway, via the local Citrix SD-WAN appliance through direct internet breakout, to the Citrix Gateway to authenticate and obtain their resources.

[![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_branchusersauth.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_branchusersauth.png)

#### Session Launch

During Session Launch is where the flows for Remote Users and Branch Users differ, based on the configuration of Beacon Points.

##### Remote Users (External)

Remote Users launch virtual app and desktop resource sessions via Citrix Gateway.

[![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_remoteuserslaunch.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_remoteuserslaunch.png)

##### Branch Users (Internal)

Branch Users, who are able to reach the internal Beacon Point successfully, are directed to Storefront. It will resolve to an IP address reachable via the SD-WAN overlay network. Subsequenlty, the user's Workspace App will be given the IP address of a VDA, to establish their session, also reachable via the SD-WAN overlay network.

[![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_branchuserslaunch.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_branchuserslaunch.png)

## Detailed Design

In this section we discuss detailed design elements of the SD-WAN Overlay Network, Quality of Service, and Beacon Points.

### SD-WAN Overlay Network

The SD-WAN Overlay Network is built over the available paths between each appliance. A secure tunnel is established on UDP port 4980 and LAN routes are exchanged.  Routes can be directly attached or learned, from the underlay network, through routing protocols. SD-WAN routes correspond to [service types](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html) and, in addition to “virtual path” service routes, they typically include “internet breakout” service routes to direct traffic identified by business application rules to ISP learned internet default routes.

In the following example Branch endpoints contact the Gateway directly through an Internet breakout service and are directed to ISP default routes by SD-WAN appliances. Branch endpoints contact the virtualization infrastructure over virtual paths that are routed over the Internet and MPLS Network which are utilized according to real-time performance monitoring.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_overlaynetwork.png)

### Quality of Service

A key differentiator of using Citrix SD-WAN with Citrix Virtual Apps and Desktops environments is its ability to automatically apply Quality of Service to HDX sessions. SD-WAN appliances identify, tag, prioritize, and stream them by class of service.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_qualityofservice.png)

Multi-stream ICA virtual channels types are assigned to the 4 classes shown below by default. In the latest versions of Citrix Virtual Apps and Desktops by default sessions are established using the Enlightened Data Transport (EDT) Protocol. EDT is a UDP based session transport protocol that efficiently adapts transmission according to available bandwidth. It falls back to TCP, and uses its built-in flow control, automatically as needed.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_virtualchannels.png)

By default, HDX sessions are sent by the VDA on UDP (EDT) port 1494 or 2598 or TCP 1494 or 2598 depending on the session protocol established. (2598 is used when session reliability is configured. Ports can be customized as needed.). The SD-WAN appliance selects the single HDX session and identifies the virtual channel by inspecting the uncompressed NSAP channel, and prepares it for transmission over a Virtual Path. It is supported on most [Workspace App platforms](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-workspace-app-feature-matrix.pdf).

The streams Virtual Path packets are assigned a tag according to class of service and then are sent to the far end. The tag is used by the SD-WAN appliance to prioritize use of outbound transmission queues, and for path selection which are monitored in real-time for quality metrics. When sent over MPLS, Differentiated Services Code Point (DSCP) tags can be mapped to pertinent [queues](/en-us/citrix-sd-wan/11-2/quality-of-service/mpls-queues.html) to allow prioritization in transit across the packet switched network. Typically, real-time traffic is assigned with an Expedited Forwarding (EF) tag, while other traffic is assigned Assured Forwarding (AF) tags in the IP header [Type of Service (TOS) field](https://en.wikipedia.org/wiki/Type_of_service).

Using Citrix SD-WAN Single Port MS Auto QOS can provide significant performance improvement for Citrix Virtual Apps and Desktops HDX sessions during congestion. Lab testing identified greater than 500% improvement in ICA Round Trip Time under severe congestion. For more information see [Measuring HDX User Experience Improvements from Citrix SD-WAN Network Performance Enhancements](/en-us/tech-zone/design/reference-architectures/sdwan-hdx-experience.html).

### Beacon Points

Beacons Points enable Workspace clients to determine whether they are on the internal for external network. By doing so they can access StoreFront and have HDX sessions streamed directly from VDAs. This allows SD-WAN to inspect session flows unencrypted and apply QOS automatically to optimize performance.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_beaconpoints.png)

Beacon points are configured in the StoreFront admin interface under **Stores > Manage Beacons**. They are entered as a URL.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_managebeacons.png)

Provisioning files represent the Beacons Points in XML format. In our example they are delivered from StoreFront, via the Gateway, to the endpoint and consumed by the Workspace app to utilize Beacon Point instructions.

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_provisioningfile.png)

## Summary

Citrix SD-WAN can significantly improve the network performance of Citrix Virtual Apps and Desktops HDX sessions resulting in a better user experience.  To achieve the maximum benefit, configure Beacons Points in accordance with the enterprise network topology to achieve direct connection to virtual apps and desktops. Route internal branch user sessions via the Citrix SD-WAN overlay network. This allows Citrix SD-WAN to minimize latency and apply features including Multi-stream HDX Auto QoS to optimize session delivery according to class of service.

## Appendix

### Deployment Considerations

Focusing on the goal of the document to provide guidance on optimal use of Citrix SD-WAN, with Citrix Gateway, in a Citrix Virtual Apps and Desktops environment we discuss pertinent hardware, software, and key configuration elements.

#### Citrix SD-WAN

Here we discuss SD-WAN appliances and pertinent deployment considerations.

##### SD-WAN Appliances

Citrix SD-WAN appliances come in various form factors to extend the overlay network between clients and VDAs whether they’re hosted in private cloud, public cloud, or on-premises data centers.

*  **Hypervisor** - Citrix SD-WAN appliances are supported on major hypervisor platforms with Intel processors.
    *  Citrix Hypervisor
    *  VMware
    *  HyperV
    *  KVM
*  **Public Cloud** – Citrix SD-WAN appliances are supported on major Cloud Platforms with sizing options to meet scalability requirements.
    *  AWS
    *  Azure
    *  GCP
*  **Hardware** – Citrix SD-WAN appliances come in various hardware models with support for various throughput and virtual paths quantity based on requirements.
For more information see the [Citrix SD-WAN data sheet](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-sd-wan-data-sheet.pdf)

##### Configuration Overview

Citrix SD-WAN can be deployed in various topologies for various use cases. To ensure it can deliver, and enhance the performance of virtual app and desktops sessions, the overlay network must be designed so that the internal beacon points are reachable, by all branch clients.
For comprehensive guidance on planning implementations complete with use cases, and recommendations see [Citrix SD-WAN Reference Architectures](/en-us/tech-zone/design/reference-architectures.html#citrix-networking)

#### Citrix Gateway

Here we discuss Citrix Gateway appliances and pertinent configuration details.

##### Gateway Appliances

Citrix Gateway appliances also come in various form factors to extend the overlay network between clients and VDAs whether they’re hosted in private cloud, public cloud, or on-premises data centers.
For more information see the [Citrix ADC hardware data sheet](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-adc-hardware-platforms.pdf)
Or the [Citrix ADC VPX data sheet](https://www.citrix.com/products/citrix-adc/resources/citrix-adc-vpx.html)

##### Configuration Overview

Citrix Gateway has been implemented optimally with Citrix Virtual Apps and Desktops for over a decade and is well documented.
For more information see:
[Integrate Citrix Gateway with StoreFront](/en-us/citrix-gateway/current-release/integrate-with-storefront.html)

#### Citrix Virtual Apps and Desktops

Here we discuss pertinent StoreFront, and Citrix Virtual Apps and Desktops environment configuration details.

##### StoreFront Instance/s

StoreFront is also a well-documented component of Citrix Virtual Apps and Desktops environments. It is typically hosted on a hypervisor, in the data center, on the internal network. Most environments have a store configured for internal users and external users integrated with Citrix Gateway. We make this assumption, therefore there are limited configuration changes to consider integrating with the Citrix SD-WAN network.

*  **Beacon Points** – Under Manage [Beacons Points](/en-us/storefront/1912-ltsr/integrate-with-citrix-gateway-and-citrix-adc/configure-beacon.html) specify the internal beacon point as a URL that is reachable from all branches. This URL can use the StoreFront server FQDN or another highly available service hosted in the same Data Center LAN.
*  **Authentication** – Under [Manage Authentication Methods](/en-us/storefront/1912-ltsr/configure-authentication-and-delegation/configure-authentication-service.html) specify the desired authentication methods. Remote Users rely on “Pass-through from Citrix Gateway”. Whereas domain joined Branch Users rely on “Domain pass-through” or non-domain joined can be authenticated again using another method such as `Username and Password`.
For more information see:
[Plan your StoreFront deployment](/en-us/storefront/1912-ltsr/plan.html)
[Integrate with Citrix Gateway and Citrix ADC](/en-us/storefront/1912-ltsr/integrate-with-citrix-gateway-and-citrix-adc.html).

##### Configuration Overview

Citrix Virtual Apps and Desktops features and functionality has expanded over several decades and is well documented.

Citrix Design Decisions guides can help you make optimal decisions regarding the configuration, optimization, and deployment of your solution. For more information see [Citrix Virtual Apps and Desktops Design Decisions](/en-us/tech-zone/design/design-decisions.html#citrix-virtual-apps-and-desktops).

Citrix Reference Architectures are comprehensive guides that assist organizations in planning their implementations complete with use cases, recommendations, and more. See [Citrix Virtual Apps and Desktops Reference Architectures](/en-us/tech-zone/design/reference-architectures.html#citrix-virtual-apps-and-desktops).

### Network Considerations

Here we discuss bandwidth considerations and pertinent ports utilized by Citrix SD-WAN, Citrix Gateway, and Citrix Virtual Apps and Desktops.

#### Bandwidth

Citrix SD-WAN appliances deployed in Gateway mode, in a branch, manage internal-intranet, and external-internet bandwidth allocation for all available links. New deployments need to plan to allocate bandwidth, and existing deployments need to reallocate it accordingly.

Citrix SD-WAN bandwidth is allocated by delivery services according to link type groups. For more information see [Delivery services](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html).

Citrix Virtual App and Desktop session bandwidth usage varies broadly by usage scenarios. For more information see [Citrix Virtual Apps and Desktops Bandwidth](https://virtualfeller.com/2020/02/19/citrix-virtual-apps-and-desktops-bandwidth/).

#### Ports

Citrix Virtual Apps and Desktops uses well defined ports to communicate between modular components. Citrix SD-WAN extends LAN like access to the User Layer components. (For more information see the diagram following.)

Citrix SD-WAN securely tunnels traffic across virtual paths between appliances, setup on UDP port 4980, using AES 128/256 bit ciphers. For more information regarding see [Configure Virtual WAN Service](/en-us/citrix-sd-wan/11-2/security/configure-virtual-wan.html).

Citrix SD-WAN can be deployed in various topologies to suit the location in the Enterprise network. For example, it can be used in Gateway mode in a branch to route all LAN traffic, or it can be deployed in Virtual Inline mode in a data center to have edge routers forward the desired traffic. For more information see [Citrix SD-WAN deployment topologies](/en-us/citrix-sd-wan/11-2/use-cases-sd-wan-virtual-routing/checklist-and-how-to-deploy.html).

Citrix SD-WAN can allow all LAN traffic, or filter according to Enterprise security policy with an onboard firewall. It can also be used to do full stack inspection. For more information see [Citrix SD-WAN Security](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-edge-security.html).

![SD-WAN HDX OnPrem Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-onprem-experience-architecture_ports.png)
