---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Glenn Williams, Shoaib Yusuf
description: Learn about the framework, design, and architecture for Citrix SD-WAN with SD-WAN Orchestrator for single region deployment.
tz_title: SD-WAN
tz_products: citrix-networking;
---
# Reference Architecture: SD-WAN

## Audience

This document discusses the framework, design, and architecture for Citrix SD-WAN in a single region environment. This document also focuses on Citrix SD-WAN solution leveraging the SD-WAN Orchestrator and network security considerations that could be involved.

The document is intended for IT decision makers, network administrators, solution integrators, partners, cloud service providers, and managed service providers.

The topics discussed in this article are applicable to both Standard and Premium Edition of Citrix SD-WAN. The discussed topics are not applicable to WANOP Edition, which has different features and capabilities.

## Citrix SD-WAN Architecture

Citrix SD-WAN can be deployed in several deployment modes and provides the flexibility to integrate the appliances, both physical and virtual, into the customer’s existing networking design. Refer the Citrix Tech Zone link for SD-WAN overview. The following are some of the use case scenarios implemented by using SD-WAN appliances.

*  SD-WAN in Edge Mode
*  SD-WAN in Virtual-Inline Mode
*  SD-WAN in Inline Mode
*  Single Region Deployment
*  Multi-Region Deployment
*  High Availability

## SD-WAN in Edge Mode

Edge mode (also known as Gateway Mode) places the SD-WAN appliance physically in the path (two-arm deployment) and requires changes in the existing network infrastructure to insert the SD-WAN appliance(s) as the default gateway for the entire LAN network for that branch. An SD-WAN deployed in Edge mode acts as a Layer 3 device and cannot perform fail-to-wire, all interfaces involved will be configured for “Fail-to-block”. In the event of appliance failure if not deployed in High Availability(HA), the default gateway for the site will also fail, causing an outage at that site until the appliance is restored.

[![SDWAN-RA-Image-1](/en-us/tech-zone/design/media/reference-architectures_sdwan_001.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_001.png)

## SD-WAN in Inline Mode

Inline mode, in this mode, the appliance appears to be an Ethernet bridge. Most appliance models include a “fail-to-wire” (Ethernet bypass) feature for inline mode. If power fails, a relay closes, and the input and output ports become electrically connected, allowing the Ethernet signal to pass through from one port to the other as if the appliance were not there. In the fail-to-wire mode, the appliance looks like a cross-over cable connecting the two ports.

In the below diagram interfaces 1/1 and 1/2 are hardware bypass pairs and will fail to wire connecting the Core to the edge MPLS Router. Interfaces 1/3 and 1/4 are also hardware bypass pairs and will fail to wire connecting the Core to the edge Firewall.

[![SDWAN-RA-Image-2](/en-us/tech-zone/design/media/reference-architectures_sdwan_002.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_002.png)

## SD-WAN in Virtual Inline Mode

Virtual inline mode, in which a router forwards WAN destined traffic to the Citrix SD-WAN appliance and the appliance returns it to the router. This mode is the least disruptive and is commonly leveraged for data centers.

[![SDWAN-RA-Image-3](/en-us/tech-zone/design/media/reference-architectures_sdwan_003.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_003.png)

## High-Level example Citrix SD-WAN Architecture

[![SDWAN-RA-Image-4](/en-us/tech-zone/design/media/reference-architectures_sdwan_004.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_004.png)

## The Citrix SD-WAN use case in focus

In this document, we will be focusing on a Single Region deployment with 30 branches to demonstrate the architecture where the SD-WAN best fits with a customer’s existing network and provides the benefits. The context of a single region refers to having all the branch nodes terminating their Virtual  Paths directly on the Master Control Node (MCN) rather than having multiple regions which is a requirement when the number of nodes in the network is above the maximum number of Virtual Paths supported by a single appliance.
Citrix SD-WAN Orchestrator

The Citrix SD-WAN Orchestrator is a cloud-hosted, multitenant management SaaS offering, which Citrix partners, CSPs, and MSPs can leverage to offer managed SD-WAN services to their customers. The SD-WAN Orchestrator provides a single-pane of glass management interface to manage multiple customers centrally, with suitable role-based access controls.

The following are some of the key capabilities:

*  **Multi-tenancy & RBAC** – The service enables Citrix partners to on-board and manage multiple SD-WAN customers, with a single pane of glass and suitable role-based access controls.
*  **Centralized configuration** – Centralized configuration of SD-WAN networks, with guided workflows, visual aids, and profiles.
*  **Centralized Licensing** - Licensing for any connected SD-WAN devices.
*  **Zero touch provisioning** – Seamless onboarding of the network and connections for physical and virtual appliances.
*  **Application-centric policies** – Application based traffic steering, Quality of Service (QoS), and firewall policies, configurable globally or per site.
*  **Hierarchical summarization of health** – Ability to centrally monitor health, usage, quality, and performance of a network, with the ability to drill down into individual sites and associated connections.
*  **Troubleshooting** – Device & Audit Logs, Diagnostic utilities such as Ping, Traceroute, Packet Capture to troubleshoot network connectivity issues.

Each of the branch and DC sites needs to have Internet connectivity over their management interface to access SD-WAN Orchestrator. Citrix SD-WAN software version 10.1.1 is the minimum appliance software supported on SD-WAN Orchestrator.

## Single Region deployment

A well-established retail Customer XYZ has approached Citrix with a design requirement whereby they have 30 retail branches, these branches are currently backhauling all their data via their MPLS circuits to the head office – expanding these circuits has been deemed as not being a scalable solution and is cost prohibitive. During discussions it has been decided that a large quantity of the branch generated traffic can be broken out locally, this includes cloud-based SaaS as well as non-business-related traffic such as social media.

The customer was looking for guidance and has decided that social media/ non-business critical traffic will be completely blocked in some locations due to limited bandwidth availability and no current plans to expand the site capacity in the near future.

In conjunction with the above requirements, the customer has the requirement to securely segregate the network traffic whilst providing guest Wi-Fi while keeping this isolated from the corporate traffic, additionally the internally generated traffic will be split via routing domains to provide an additional layer of security such that the POS is segregated from the shop floor terminals.

The customer has also advised that they are presently not in the position to deploy an SD-WAN solution at all sites due to budget and resourcing thus connectivity will need to be established with the use of existing MPLS circuits and thus Intranet services.

## Retail Customer - Existing Network Architecture

Before proceeding with any SD-WAN configuration, the network admin always needs to properly understand the existing (underlay) network and to fully understand the limitations of it that require the need of an SD-WAN solution.

The customer currently has a resilient network by having a primary and secondary data center for business applications. The primary data center has two MPLS circuits (10 and 20 Mbps) terminated at the CE router, and the secondary data center has one MPLS circuit (20 Mbps) terminated for WAN connectivity. The primary data center will failover to the secondary data center in the event of a disaster at the primary data center.

The Point of Sales (POS) and other business-critical applications are hosted within the data center. All the branch locations are configured to access the POS and other business applications via the MPLS link to the data center. Currently a few of the branch locations have two MPLS links for redundancy, but they are configured as Active / Passive and therefore they are not fully utilizing the bandwidth they are paying for. All the branch users access the internet via the data center breakout thus all traffic must traverse the over-subscribed MPLS circuits, to address this the customer is planning to introduce local internet breakout at a few of the branches to provide direct connectivity to the internet and Cloud-hosted SaaS applications.

[![SDWAN-RA-Image-5](/en-us/tech-zone/design/media/reference-architectures_sdwan_005.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_005.png)

## Benefits of implementing Citrix SD-WAN

1.  Citrix SD-WAN enables WAN link aggregation which eliminates the downtime if one of the links fails or large packet loss occurs
2.  Improved application performance and Quality of Service (QoS) for all the branch users while accessing the business applications
3.  Reduces the WAN costs by allowing the customer to augment their MPLS circuits with lower-priced broadband and LTE connectivity at branch locations
4.  Improved business connectivity and disaster recovery capabilities that maintain connectivity even during multiple link failure
5.  Simplified branch networking by consolidating network functions into an integrated WAN edge appliance and centralizing management and policy definition

## Retail Customer – Citrix SD-WAN Network Design

[![SDWAN-RA-Image-6](/en-us/tech-zone/design/media/reference-architectures_sdwan_006.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_006.png)

The above diagram represents the proposed overlay network design incorporating Citrix SD-WAN, in the following sections we will drill down into each data center and the branches to understand the SD-WAN configuration and how it forms the virtual WAN connectivity using different access types - MPLS, Internet and 4G/LTE links.

## SD-WAN Orchestrator

The SD-WAN appliances are configured to connect with cloud-based SD-WAN Orchestrator via the Internet using the management interfaces of the devices. SD-WAN Orchestrator will distribute the configuration and software to every SD-WAN device in parallel, it also polls data from every SD-WAN device for monitoring and reporting purposes. There was a requirement for firewall rules to be applied to allow internet access for SD-WAN appliances to so that they could communicate with the Citrix Cloud SD-WAN Orchestrator Service.

[![SDWAN-RA-Image-7](/en-us/tech-zone/design/media/reference-architectures_sdwan_007.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_007.png)

## Primary data center configuration

The SD-WAN appliance at the primary data center is deployed using the Virtual Inline mode which is also sometimes referred to as Policy-Based Routing (PBR) mode. There are two methods by which traffic can be routed to the SDWAN appliance

*  Policy-Based Routing
*  Dynamic routing protocol

The Citrix SD-WAN solution supports both static routing and dynamic routing protocols.

The SD-WAN appliances can perform route discovery of Layer 3 routing advertisements within the existing customer network for each of the desired dynamic routing protocols (OSPF and BGP), this removes the requirement for SD-WAN administrators to statically define the LAN-side and the WAN-side networking environment for each appliance that is part of the SD-WAN network.

For networks with Route Learning enabled, Citrix SD-WAN provides more control over which SD-WAN routes are advertised to routing neighbors rather than advertising all or no routes. Export Filters are used to include or exclude routes for advertisement using OSPF and BGP protocols based on specific match criteria. Route filtering is implemented on LAN routes in an SD-WAN network (Datacenter/Branch) and are advertised to a non-SD-WAN network through eBGP. The SD-WAN administrator can configure 32 export filters based on their requirement. Export filter are the filters used to prioritize the specific routing domain or default routing domain based on the specific order.

*  Import Filters – (Default: import none)
*  Export Filters – (Default: export all static and tunnel routes)

The Export OSPF Route Type is Type 5 External by default, the reason being that the SD-WAN routing table is considered external to the OSPF protocol and so OSPF will prefer a route learned internal (intra-area), therefore routes advertised by SD-WAN may not take precedence. When OSPF is used across the WAN (i.e. MPLS networks), then this can be changed to Type 1 intra-area. SD-WAN can now advertise routes as intra-area routes (LSA Type 1) to get preference as per its route cost using the OSPF path selection algorithm.

### Master control node

In the context of a single region deployment the data center appliance(s) are the headend appliance(s) which are set to be the MCN (Master Control Node). MCN requires static IP address to be configured. The Master Control Node (MCN) is the central Virtual WAN Appliance that is responsible for time synchronization, routing updates and the hub for the branch devices. Head-end devices are typically deployed in High Available. Since the customer has two data centers for disaster recovery, the primary data center is configured as Primary MCN (this can be an HA pair) and the secondary data center is configured as the Secondary MCN (which also can be an HA pair).

### High availability mode

The Citrix SD-WAN High Availability (HA) configuration has two SD-WAN Appliances at the primary data center that serve in an active/standby partnership, for redundancy purposes - both appliances in an HA pair must be the same appliance model.

To define the high availability configuration, the SD-WAN administrator has configured the HA appliance name with the mode as Primary MCN. It is possible to set a failover time, in milliseconds, this specifies the wait time in the event of  heartbeat failure before invoking the standby MCN appliance as active node - the default value for the failover time is 1000ms. In addition to these settings the admin has selected the Primary Reclaim option for the primary MCN to reclaim the control upon restart after a failover event. High Availability has an additional configuration for Shared Base MAC address that can be configured to share the interface MAC addresses between the appliances in an HA pair. If a failover occurs, the secondary appliance has the same virtual MAC addresses as the failed primary appliance. For cloud-based platforms, there is an additional option to disable the Shared Base MAC address.

In the High Availability IP interfaces configuration, the admin has selected the virtual interface for heartbeat communication between the appliances in the HA pair with Primary and Secondary IP addresses. It is important to note that the failover occurs during heartbeat failure from primary to secondary only for the interfaces that are monitored, if there are interfaces that are not monitored for Heartbeat failures on the SD-WAN appliance and there is an outage on those interfaces the appliances will not failover.

The external tracker IP address was configured that ARPs the upstream router and it updates the status regarding the state of the primary appliance, if there is a failure in the response, the SD-WAN initiates the failover.

[![SDWAN-RA-Image-8](/en-us/tech-zone/design/media/reference-architectures_sdwan_008.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_008.png)

### Virtual path

The SD-WAN administrator created a WAN path tunnel across each WAN link between data center and branch SD-WAN appliances by configuring the virtual IP to represent the endpoint for each WAN link. This configuration enables to aggregate multiple WAN links (physical links) into a single virtual path allowing packets to traverse the WAN utilizing the SD-WAN overlay network instead of the existing underlay, the Virtual Path is a UDP-based tunnel using UDP port 4980. The network admin ensures that the upstream firewalls, routers, IPS and IDS devices have the necessary rules to allow the UDP 4980 packets over the WAN links to establish the virtual path between SD-WAN devices.

The SD-WAN Virtual Path service manages traffic across the Virtual Paths. A Virtual Path is a logical link between two or more WAN links connecting the SD-WAN sites. It is the grouping of different WAN access types to provide high service-levels and reliable communication between SD-WAN nodes. This is accomplished by constantly measuring the underlay WAN path conditions. The source (sending) appliance adds a header to each packet with information about the current link characteristics. The destination (receiving) appliance reads these headers and uses the data to understand the transit time, congestion, jitter, packet loss and other information about the performance and health of the path.

SD-WAN appliances measure the path unidirectionally and select the best path on a per-packet basis. Citrix SD-WAN offers packet-based path selection for rapid adaptation to any network changes. SD-WAN appliances can detect path outages after just two or three missing packets, allowing seamless sub-second failover of application traffic to the next-best WAN path. SD-WAN appliances recalculate every WAN link status in about 50ms.

A virtual path can be static or dynamic between branches however all branches must have a static path to the MCN,  Dynamic virtual paths are established directly between sites based on a configured threshold. The threshold is based on the amount of traffic occurring between those sites, Dynamic Virtual Paths are created only after the specified threshold is reached. Dynamic Virtual Path have the default route cost of 5 and there are default limits and timers:

*  Removal Virtual Path down wait time – 1 minute
*  Recreate Virtual Path Hold Time(m) – 10 minutes
*  Creation Limits
    *  Sample Time – 1 second
    *  Throughput – 600 kbps
    *  Throughput – 45 pps
*  Removal Limits
    *  Sample Time – 2 second
    *  Throughput – 45 kbps
    *  Throughput – 35 pps

### Securing the virtual path

AES-128 bit encryption is enabled for virtual paths by default with AES-256 and IPsec also available, this is for the virtual path The option “enable Encryption Key Rotation” which is enabled by default is set to ensure key regeneration for every virtual path with encryption enabled using an Elliptic Curve Diffie-Hellman key exchange at intervals of 10-15 minutes and this is configurable.

There is a secret key for each site; for each virtual path, the network configuration generates a key by combining the secret keys from the sites at each end of the virtual path. The initial key exchange that occurs after a virtual path is first set up is dependent upon the ability to encrypt and decrypt packets with that combined key.

Citrix SD-WAN supports IPsec enabling third-party devices to terminate IPsec VPN tunnels on the LAN or WAN side of a Citrix SD-WAN appliance. The administrator can secure site-to-site IPsec Tunnels terminating on an SD-WAN appliance by using a 140-2 Level 1 FIPS certified IPsec cryptographic binary. SD-WAN also supports resilient IPsec tunneling using a differentiated virtual path tunneling mechanism.

## Secondary data center configuration

The SD-WAN appliances at the secondary data center are also deployed in Virtual inline (PBR) mode, the secondary data center SD-WAN appliances are configured as Secondary MCNs in a Parallel HA configuration.

[![SDWAN-RA-Image-9](/en-us/tech-zone/design/media/reference-architectures_sdwan_009.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_009.png)

### Primary MCN failover to secondary

[![SDWAN-RA-Image-10](/en-us/tech-zone/design/media/reference-architectures_sdwan_010.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_010.png)

The secondary MCN continuously monitors the health of the primary MCN and if the primary MCN fails the secondary MCN assumes the role of the MCN, the primary MCN to secondary MCN switch over happens after 15 seconds of the primary MCN being inactive. The primary reclaim option is not available for secondary MCN as the primary reclaim happens automatically after the primary appliance is back ON and when the threshold timer expires.

### Backhaul Internet to data center MCN

The term “backhaul” indicates traffic destined for the Internet to be sent back to another predefined site which has access to the Internet via a WAN link. This may be the case for networks that do not allow Internet access directly at a branch office because of security concerns, or due to the underlay network limitations. The network admin can configure that all traffic is backhauled to the MCN whether this be for DC resources or internet access, however, other Citrix SD-WAN branch-node sites can be leveraged for access to the Internet.

For some environments, backhauling all remote site internet traffic through the hardened DMZ at the data center can be the most desired approach to provide internet access to users at branch offices. This approach does, however, have its limitations to be aware of and the underlay WAN links size appropriately.

*  Backhaul of internet traffic adds latency to internet connectivity and is variable depending on the distance of the branch site for the data center
*  Backhaul of internet traffic consumes bandwidth on the Virtual Path and should be accounted for in sizing of WAN links
*  Backhaul of internet traffic may over-subscribe the Internet WAN link at the data center especially when the virtual inline mode is leveraged.

### Citrix Virtual Apps and Desktops access for branch users

The Citrix Virtual Apps and Desktops environment is hosted at the data center locations for all the branch users who need to access the CVAD environment over the MPLS network which includes all the Citrix HDX user experience capabilities such as printing, multimedia, etc. Citrix SD-WAN allows the customer to deliver the best user experience for the branch users while accessing the Citrix Virtual Apps and Desktops environment.

## Branch-1 Configuration

The procedure for adding a branch site is the same as creating and configuring the MCN site using the SD-WAN Orchestrator however, some of the configuration steps and settings do vary slightly for a branch sites. In addition, once we have added an initial branch site, for sites that have the same appliance model we can use the clone (duplicate) feature to streamline the process of adding and configuring those sites.

The SD-WAN appliance at Branch-1 is deployed using inline mode. The fail-to-wire configuration has been applied to bridge the links in the event of hardware or software failure which will enable users to still access the business critical and POS applications over MPLS.

Since branch-1 only has a single MPLS link and there is no internet link all the internet traffic must backhaul to the data center, the customer decided to utilize the Internet link which is available in branch-2 with adequate bandwidth for the branch-1 internet access. The SD-WAN administrator has verified that the latency to reach branch-2 is less than comparing to data centers hence utilizing the internet at branch-2 would provide optimal performance for branch-1 users. This requirement is achieved via the hairpin deployment. Note that the SD-WAN administrator enabled WAN to WAN forwarding to enable this site to serve as a proxy for multi-hop sites.

### Hairpin deployment

The Hairpin deployment helps a customer to implement a configuration that allows a site to use a remote hub site’s WAN link for internet access since local internet services are not available.

The purpose of a hairpin deployment is to allow direct application access via an internet link which is not local to the branch or site. Customers will have the ability to use a remote site for internet access when needs arise and can route flows through the virtual path.

To achieve the desired requirement, the customer created and applied the hairpin deployment configuration for branch-1 SD-WAN appliance so that branch-1 users can access the internet via branch-2 and the same policies and configuration was propagated to the branches using the SD-WAN orchestrator. The customer also ensured that the branch-2 has the required amount of bandwidth over the internet link to accommodate the additional traffic from branch-1.

### Application Visibility using Deep Packet Inspection (DPI)

Citrix SD-WAN has an integrated Deep Packet Inspection (DPI) library that enables real-time discovery and classification of applications. Using the built-in DPI engine, the SD-WAN appliance analyzes the incoming packet and classifies it as belonging to an application or application family.

Using DPI based application visibility, Citrix SD-WAN can detect over 4,500 applications and sub-applications. This feature employs a variety of pattern matching, session behavior, DNS caching and port-based classification techniques to provide real time layer-7 visibility into all the applications traversing the WAN, including SSL encrypted traffic such as HTTPS, IMAPS etc. Citrix SD-WAN not only provides visibility into applications such as social media (Facebook, YouTube, and Twitter), O365, Oracle and so on, but also provides visibility into the bandwidth consumption of applications, to help make sure that business-critical apps are not starved of bandwidth.

Since the branch-1 users are backhauled to branch-2 for application access over the internet link, the customer decided to monitor the application level access and its bandwidth utilization to understand the access pattern and control the access only for the business-need applications. The customer has created application policies which block an newly created Application Group called “Social Media” which includes the likes of Facebook, YouTube, Twitter, etc.

With this configuration, branch-1 users could access the internet only for business-critical applications and non-business-related access have been blocked at the branch-1 SD-WAN itself thus saving valuable bandwidth.

[![SDWAN-RA-Image-11](/en-us/tech-zone/design/media/reference-architectures_sdwan_011.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_011.png)

### Routing Domains

Citrix SD-WAN provides the ability to segment networks for more security and manageability by using Virtual Routing and Forwarding (VRF-Lite). The customer can separate guest network traffic from employee production traffic, create distinct routing domains to segment large corporate networks, and segment traffic to support multiple customer networks. Each routing domain has its own routing table and enables the support for overlapping IP subnets.

Citrix SD-WAN appliances implement OSPF and BGP routing protocols for the routing domains to control and segment network traffic. A Virtual Path can communicate using all routing domains regardless of the definition of the access point. This is possible because the SD-WAN encapsulation includes the routing domain information for each packet therefore both end devices know where the packet belongs in respect of routing domain. Routing Domain are separated by a barrier such as VLAN, it is possible to  configure up to 255 routing domains.

As per the requirement, the admin has created two routing domains for branch-1 which will be used for Corporate traffic and Guest Wi-Fi access respectively, Citrix Virtual Apps and Desktops traffic are routed to data center using the virtual path and the guest Wi-Fi access is routed to branch-2 using the hairpin configuration.

In addition to routing domain, the customer wanted to apply higher priority for the business-critical applications over the virtual path. In the next section, we’ll discuss about application QoS and how it helps to achieve the customer requirement.

### Application QOS

The Citrix SD-WAN solution has one of the most sophisticated application QoS engines on the market. It has the broadest set of capabilities to assess not only the application traffic and prioritize it against other applications, but also to understand its needs with respect to the WAN network quality, and to pick a network path based on network quality characteristics in real time.

The application classification feature enables the Citrix SD-WAN appliance to parse incoming traffic and classify them as belonging to an application or application family. This classification allows us to enhance the QoS of individual applications or application families by creating and applying application rules.

The network administrator can filter traffic flows based on application, application family, or application object match-types and apply application rules to them. The application rules are like Internet Protocol (IP) rules.

The Citrix SD-WAN solution provides a default set of application-classifications out-of-the-box, rule-filtering, and class-assignment settings. Using classes, the admin can classify a specific type of traffic on the virtual path, and then apply rules to handle this traffic. Traffic is assigned to a specific class, as defined in the rule.

The SD-WAN system provides 17 classes (0-16). Classes 0-3 are predefined for Citrix HDX QoS prioritization, these classes are used to classify HDX traffic with different ICA priority tags. The SD-WAN administrator can edit the class types and their assigned bandwidth sharing to obtain the optimal Quality of Service but cannot edit the names of the classes.

Classes 10-16 are predefined and are associated with RealTime, Interactive, and Bulk class types. Each type can be configured further to optimize Quality of Service for its type of traffic. Classes 4-9 can be used to specify user defined classes. Classes are of one of the following three types:

**Real-time** – VoIP or VoIP like applications, such as Skype for Business or ICA audio. In general, it refers to voice only applications that use small UDP packets, that are business critical.

**Interactive** – This is the broadest category and refers to any application that has a high degree of user interaction. Some of these applications, for example, video conferencing, are sensitive to latency and requires high bandwidth. Other applications like HTTPS, may need less bandwidth but are critical to the business. Interactive applications are typically transactional in nature.

**Bulk** – This is any application that does not need a rich user experience but is more about moving data (i.e. FTP or backup/replication)

To achieve the customer’s actual requirement of prioritizing the HDX access from branch to data center, the SD-WAN administrator has created the QoS rules for Citrix HDX access for all the branches and applied the policy to branches via SD-WAN Orchestrator. The customer has applied the provisioning settings which allows the bidirectional (LAN to WAN / WAN to LAN) distribution of bandwidth for a WAN link among the various services associated with that WAN link.

## Branch-2 Configuration

The SD-WAN appliance at the Branch-2 is deployed sitting on both MPLS and Internet circuits, it is deployed in what is referred to as an inline mode with high availability – this means the existing routing devices/firewalls remain in situ.

The customer has installed 210 LTE appliances which introduced 4G / LTE internet link to provide a back-up for the existing internet link of this branch. Using the Deep Packet Inspection feature, the business applications and SaaS applications that require internet access have to be broken out at the branch locally and traffic must be routed via the dedicated internet link or 4G/LTE link. All the social media access needs to be broken out at the branch and routed via the internet link.

Using the Local Internet Breakout service, the SD-WAN administrator configured the application policies to divert the traffic via the local internet link for the branch. When using this Internet Breakout service, the internet traffic is directly sent to the public internet from branch-2 instead of backhauled to the data center.

### Local Internet Breakout Service

The Internet Service is used for traffic between sites and the public internet, the Internet service traffic is not encapsulated by Citrix SD-WAN and does not have the same capabilities as traffic that is delivered across the Virtual Path Service including QoS.

However, it is important to classify and take account for this internet traffic, traffic that is identified as Internet Service enables the added ability of SD-WAN being able to actively manage WAN link bandwidth by rate-limiting Internet traffic relative to traffic delivered across the Virtual Path and Intranet services per the configuration established by the administrator. In addition to bandwidth provisioning capabilities, SD-WAN has the added capability to load balance traffic delivered across the Internet Service making use of multiple Internet WAN links.

Internet traffic control using the Internet Service on Citrix SD-WAN can be configured in the following deployment modes:

*  Direct Internet Breakout at Branch with Integrated Firewall
*  Direct Internet Breakout at Branch forwarding to Secure Web Gateway

[![SDWAN-RA-Image-12](/en-us/tech-zone/design/media/reference-architectures_sdwan_012.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_012.png)

### Zscaler Cloud Security Platform

To secure traffic and enforce policies, enterprises backhaul branch internet destined traffic to the corporate data center. The data center applies security policies, filters traffic through security appliances and routes the traffic through an ISP. Such backhauling over private MPLS links is expensive and consumes valuable resources. It also results in significant latency, which creates a poor user experience at the branch site.

An alternative to backhauling is to add security appliances at the branch. However, the cost and complexity increase as multiple appliances are installed. Also if there a large numbers of branches cost and management becomes impractical.

The ideal solution to enforce security without adding cost, complexity, or latency is to route all branch Internet traffic from the Citrix SD-WAN appliance to the Zscaler Cloud Security Platform. The customer can then use a central Zscaler console to create granular security policies for the users. The policies are applied consistently whether the user is at the data center or a branch site. Because the Zscaler security solution is cloud-based, the customer doesn’t have to add additional security appliances to the network.

The Zscaler Cloud Security Platform acts as a series of security check posts in more than 100 data centers around the world. By simply redirecting the customer’s internet traffic to Zscaler, they can immediately secure the datastores, branches, and remote locations. Zscaler connects users and the internet, inspecting every packet of traffic, even if it is encrypted or compressed. Citrix SD-WAN appliances can connect to a Zscaler cloud network through GRE tunnels or IPsec tunnels from the customer’s site.

To achieve the breakout of internet service and secure the internet traffic, the administrator configured the local internet breakout service at the branch-2 SD-WAN appliance and created an IPsec tunnel to the Zscaler cloud network. With this configuration, the customer was able to divert the internet traffic from branch-2 to Zscaler in a secure tunnel and then to the public internet. By achieving this goal, the MPLS link bandwidth was reduced and thus saved cost.

## Branch-3 Configuration

The SD-WAN appliance at Branch-3 is deployed using inline mode with fail-to-wire configuration. This branch has internet link and a 4G / LTE internet link for backup. Branch-3 has the configuration to backhaul to internet traffic to data center MCN. The customer decided to divert the few internet traffic areas like news, SaaS, and other cloud services access via the local internet link to provide uninterrupted and optimal access to the users.

[![SDWAN-RA-Image-13](/en-us/tech-zone/design/media/reference-architectures_sdwan_013.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_013.png)

### Application Routes

The branch office users need to access the applications which are hosted at the primary or secondary data centers, the cloud apps, or the SaaS applications using the SD-WAN virtual path. The application routing feature allows steering the applications through the network easily and cost-efficiently. Using this feature when a Branch-3 user is trying to access a SaaS application, the traffic can be routed on the internet directly, without having to go through the data center first.

Citrix SD-WAN defines the application routes for Virtual Path, internet, Intranet, Local, GRE Tunnel, and LAN IPsec Tunnel. To perform service steering for applications, it is important to identify an application on the first packet itself. Initially, the packets flow through the IP route once the traffic is classified and the application is known, the corresponding application route is used. First packet classification is achieved by learning the IP subnets and ports associated with application objects. These are obtained using historical classification results of the DPI classifier, and user configured IP port match types.

At Branch-3, the customer has an internet link and wanted to utilize the link for application traffic which directs to the internet. The customer has configured the application routing for Azure, AWS, and Google Cloud access to access with the local internet link.

### Standby and Metering WAN Links

Citrix SD-WAN enables standby links, which can be configured such that user traffic is only transmitted on a specific Internet WAN link when all other available WAN links are disabled.

Standby links conserve bandwidth on links that are billed based on usage. With the standby links, the administrator can configure the links as the Last Resort link, which disallows the usage of the link until all other non-metered links are down or degraded.

Set Last Resort is typically enabled when there are three WAN links to a site (that is, MPLS, Broadband Internet, 4G/LTE) and one of the WAN links is 4G/LTE and may be too costly for a business to allow usage unless it is necessary. Metering is not enabled by default and can be enabled on a WAN link of any access type (Public Internet / Private MPLS / Private Intranet).

The customer also has a requirement to enable backup/standby for the local internet link which is critical for the cloud internet traffic. The administrator has enabled the 4G-LTE link as the metered last resort link so that if the dedicated local internet link saturated or fails the internet traffic will also use the 4G-LTE link.

## Branch-29 Configuration

The SD-WAN appliance at Branch-29 is deployed using inline mode with Parallel HA configuration to connect the 2 x MPLS links and a  4G-LTE link which is terminated at SD-WAN for internet access. Since we have 3 WAN links for this branch, the customer wanted to segregate the virtual path for internet and intranet traffic. Internet traffic should always use one of the MPLS links and if it fails, the 4G-LTE link should be utilized and for the Intranet Non-SD-WAN branch traffic, the second MPLS link needs to be utilized. The customer also ensured that they have configured the intranet services in all the branches to communicate with the non-SD-WAN branches.

[![SDWAN-RA-Image-14](/en-us/tech-zone/design/media/reference-architectures_sdwan_014.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_014.png)

### Intranet Service

Intranet service denotes routes that are reachable through a private WAN link (MPLS, P2P, VPN etc.) to Branch-30 that is on the MPLS network but does not have an SD-WAN appliance. It is assumed that these routes need to be forwarded to a certain WAN router. Intranet service is not enabled by default hence the SD-WAN administrator has enabled this service and applied the routes. Any traffic matching this route (subnet) is classified as an intranet for this appliance for delivery to a site that does not have an SD-WAN solution.

### Internet Service

Internet service is like Intranet but is used to define traffic flowing to public Internet WAN links rather than private WAN links. One unique difference is that the Internet service can be associated with multiple WAN links and set to load balance (per flow) or be active/backup. Default Internet routes get created when the internet service is enabled (it is off by default). Any traffic matching this route (subnet) is classified as the Internet for this appliance for delivery to public internet resources.

### Intranet and Internet Routes

For the Intranet and Internet service types, the SD-WAN administrator must define an SD-WAN Link to support those types of services. This is a pre-requisite for any defined routes for either of these services. If the WAN link is not defined to support the Intranet Service, it will be considered as a local route. The Intranet, Internet, and Passthrough routes are only relevant to the site/appliance they are configured for.

When defining Intranet, Internet or Passthrough routes the following are design considerations:

*  Must have service defined on the WAN link (Intranet/Internet – required)
*  Relevant to local SD-WAN device
*  Intranet routes can be learned via the Virtual Path but are done so at a higher cost
*  With Internet Service, there is automatically a default route created (0.0.0.0/0) catch all route with a cost 5 and it is configurable
*  Do not assume Passthrough will work, this should be tested/verified, also test with Virtual Path down/disabled to verify the desired behavior
*  Route tables are static unless the route learning feature is enabled

As per the requirement, the SD-WAN administrator has created the intranet service with one of MPLS links for establishing the communication with branch-30. To fulfill the requirement of the internet service, the administrator has created the virtual path with the second MPLS link to communicate with all other branches.

## Branch-30 Configuration

Branch-30 is a new branch connected with the data center and other branches via MPLS network without SD-WAN installed at the branch. The intranet services configured at the data center and other branches has the routes forwarded to certain routers. Any traffic matching this route is classified as intranet and delivery to a branch-30 that does not have an SD-WAN solution. This way the users from the branch can connect to the data center to access the business and POS applications.

[![SDWAN-RA-Image-15](/en-us/tech-zone/design/media/reference-architectures_sdwan_015.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan_015.png)

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s5efacfd45654b079).

## References

[Citrix SD-WAN Orchestrator](/en-us/citrix-sd-wan-orchestrator.html)

[Citrix SD-WAN how to articles](/en-us/citrix-sd-wan/10-2/how-to-articles.html)

[Best Practices for SD-WAN](/en-us/citrix-sd-wan/10-2/best-practices.html)

[Citrix SD-WAN for Citrix Workspace](/en-us/tech-zone/learn/tech-briefs/sdwan-workspace.html)

[Deployment Modes](/en-us/citrix-sd-wan/10-2/use-cases-sd-wan-virtual-routing.html)

[GRE Tunnel for a Branch Site](/en-us/citrix-sd-wan/10-2/gre-tunnel/configure-gre-tunnels-branch-site.html)

[Backhaul Internet](/en-us/citrix-sd-wan/10-2/internet-service/backhaul-internet.html)

[Metering and Standby WAN Links](/en-us/citrix-sd-wan/10-2/standby-wan-links.html)

[Application Classes and Rules](/en-us/citrix-sd-wan/10-2/quality-of-service/customize-classes.html)

[MPLS Queues](/en-us/citrix-sd-wan/10-2/quality-of-service/mpls-queues.html)

[Dynamic Routing](/en-us/citrix-sd-wan/10-2/routing/dynamic-routing.html)

[Configure Virtual Router Redundancy Protocol](/en-us/citrix-sd-wan/10-2/routing/configure-virutal-router-redundancy-protocol.html)

[IPsec tunnel between SD-WAN and third-party devices](/en-us/citrix-sd-wan/10-2/security/ipsec-tunnel-termination/how-to-configure-ipsec-tunnel-for-third-party-devices.html)

[Zscaler Integration by using GRE tunnels and IPsec tunnels](/en-us/citrix-sd-wan/10-2/security/citrix-sd-wan-secure-web-gateway/sd-wan-web-secure-gateway-using-gre-tunnels-and-ipsec-tunnels.html)
