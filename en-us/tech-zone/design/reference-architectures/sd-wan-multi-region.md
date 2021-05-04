---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Glenn Williams, Ravisankar Pegada, Karthick Srivatsan, Shoaib Yusuf
description: Discover the framework, design, and architecture for Citrix SD-WAN multi-region deployment with SD-WAN Orchestrator.
tz_title: SD-WAN Multi-Region
tz_products: citrix-networking;
---
# Reference Architecture: SD-WAN Multi-Region

## Audience

This document discusses the framework, design, and architecture for a Citrix SD-WAN multi-region deployment. This document also focuses on delivering Citrix SD-WAN solutions using SD-WAN Orchestrator.

The document is intended for IT decision makers, Network Administrators, Solution Integrators, Partners, Cloud Service Providers, and Managed Service Providers.

Refer to the [Citrix SD-WAN Single Region Reference Architecture](/en-us/tech-zone/design/reference-architectures/sdwan.html) to gain adequate knowledge on Citrix SD-WAN concepts and terminology.

## Citrix SD-WAN Multi Region Architecture

Citrix SD-WAN Multi-Region architecture is well suited for organizations which has several branch offices with large mesh network in multiple regions. Those large organizations can deploy a Citrix SD-WAN multi-region architecture, with each region’s maximum client node support being constrained by the selected device model. The supported number of regions is based on the network design and deployment which is spread across 6000 sites. The supported number of sites was 2500 in earlier release to 11.

The multi-region SD-WAN network supports a distributed network architecture with a Master Control Node (MCN) controlling multiple Regional Control Nodes (RCNs). Each RCN, in turn, controls multiple client sites within their own region. The MCN can also be optionally used to control some of the client sites directly, denoted as the Default Region. This hierarchical and distributed architecture enables higher scale, and effective delegation of regional administration. When operating in Multi-Region architecture, the MCN is no longer required to have direct Static Virtual Path connectivity to every Client Node in the overlay. Enabling the overlay to scale beyond the limitations of maximum supported Static Virtual Paths for the device model selected as the MCN.

The maximum supported Static Virtual Paths is on a per platform basis, refer the Citrix SD-WAN Data Sheet. The scale capabilities for Multi-Region deployment’s is limited by the selected platform operating as the MCN and RCNs in the overlay. There can be numerous variations in the initial design, but each deployment needs considering the end-goal of supporting the desired total number of sites, plus room to grow. Multi-region deployment supports up to 128 regions. The supported number of regions was 64 in earlier releases.

### Citrix SD-WAN Multi-Region Conceptual Architecture

The high-level architecture of an SD-WAN multi-region deployment for a large customer is following:

[![SDWAN_RCN-RA-Image-1](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_001.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_001.png)

### Master Control Node (MCN)

The Master Control Node (MCN) is the central SD-WAN device that is responsible for time synchronization, routing updates and the hub for the branch devices. MCN deployment can be achieved via SD-WAN Orchestrator. A deployment can have Primary MCN and Secondary MCN that provides redundancy. Also, MCN can be configured in a HA configuration with two SD-WAN devices at a site. The MCN serves as the controller of the network hence only one active device is designated as the MCN, all others must be denoted as RCNs or Client Nodes.

### Regional Control Node (RCN)

Regional Control Node (RCN) supports hierarchical network architecture, enabling multi-region network deployment. MCN directly connects and controls multiple RCNs. Each RCN directly connects and controls multiple Client Nodes. Region Control Node is also considered as the MCN for a region as the functionality and responsibility is similar.

### Client Node

Appliances at the branch sites that receive configuration from the MCN or RCN or SD-WAN Orchestrator and participate in establishing overlay functionalities to the other branch offices. There can be multiple branch sites for a region, the maximum number of Client Nodes in the network is limited by device platform selected as the RCN or MCN.

### SD-WAN Orchestrator

The MCN, RCN, and Client Node appliances are configured to connect with cloud-based SD-WAN Orchestrator via the Internet using the management interfaces of the devices. SD-WAN Orchestrator is used to distribute the configuration and software to every SD-WAN device in parallel, it also polls data from every SD-WAN device for monitoring and reporting purposes.

Let’s review a sample use case for SD-WAN Multi-Region Architecture:

### Sample Use Case: Financial customer network design

A large financial organization which has several branch offices spread across different countries and regions. Network design for each branch varies depending on the branch size and the requirements of WAN and Internet links. Each regional branch is categorized as small, medium, and large which is based on the number of users, WAN or Internet links, and the bandwidth requirement.

This organization has 400 sites in the America region, 300 sites in the EMEA region, and 500 sites in the APJ region. All these regional sites are connected via the available Private and Public WAN links. The total number of sites for this organization requiring SD-WAN technology is 1200. Each region has a site denoted as a data center that supports the local set of branch sites for the region.

### Pain Points (Without SD-WAN)

Managing this large network and ensuring the stability and scalability is a pain-point for the organization. The network is spread across different regions and those network devices are monitored and managed individually at each site.

The major outages that occurred in multiple regions when there is a network failure in one of the regional branches which causes huge downtime and productivity loss. The IT Manager is keen to segregate the branch office networks into region-wide networks to ease the management and monitor the network at the regional level. This segregation can help to avoid the downtime in multiple regions.

With new applications being added frequently, the requirement of bandwidth grows rapidly. It becomes tough for IT team to upgrade the WAN links or bandwidth as it requires addition budget. These issues manifest to inconsistent application delivery and performance that maps back to the business goals.

The lack of visibility into how much bandwidth, a specific application consumes and how well they are performing is another major pain point.

Finally, the existing network architecture has inability to route the traffic between links efficiently to optimize the bandwidth, cost, and performance for the enterprise.

The customer’s high-level existing network design is following:

[![SDWAN_RCN-RA-Image-2](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_002.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_002.png)

To overcome the pain points, simplify the manageability, and increase scalability, the CTO of this company is looking for a Software Defined WAN solution, with functionality to:

*  Connect all the branch networks within and across the regions with a cloud-based centralized management tool to govern and monitor the entire network
*  Offer management of the control plane and health of branch network devices and link status at the region level and global level
*  Allow to create an overlay network with dynamic path selection
*  Load balance and effective utilization of links to aggregate the available bandwidth
*  Help avoid downtime during a failure or degraded performance in one or multiple links at a regional branch
*  Enable IT to consolidate the network footprint at branch sites by eliminating routers and firewalls throughout a tech refresh
*  Improve Quality of Service and application acceleration for specific application protocols from the branch-to-branch or branch-to-data center
*  Reduce the recurring WAN link cost and utilizes all the links effectively
*  Utilize the low-cost broadband and 4G LTE connectivity WAN links to augment the low-bandwidth MPLS
*  Enable local internet breakout at the branch level to provide access to cloud-based applications and social media directly from the branch
*  Segregate the guest Wi-Fi and Corporate network traffic to tighten security in the network
*  Enable connectivity of the SD-WAN branches with the Non-SD-WAN branches seamlessly
*  Support cloud migration with multi-level security and enabling direct branch office connections to the Internet cloud
*  Secure data across the WAN and to the cloud with strong encryption, application-level security policies, and data segmentation
*  Support WAN Optimization and Cloud Connectivity

### Implementing Citrix SD-WAN

Scaling beyond the site support of a single Master Control Node (MCN) is accomplished using Multi-Region architecture. In a multi-region deployment, the network is segmented into regions, each managed by a Regional Control Node (RCN). The MCN is then able to manage multiple RCN nodes to scale the network as needed. Citrix SD-WAN solves all the preceding issues and pain points for this customer, and the Multi-Region Architecture directly addresses the need to support 1200 sites. Let’s discuss more in detail:

SD-WAN Orchestrator helps to manage the large network more efficiently and scale the environment intelligently. The Regional Control Nodes (RCN) is introduced to offload the MCN Static Virtual Path limitations and to manage the networks with regional grouping structure.

SD-WAN Orchestrator is the distribution point, for subsequent configuration and software distribution. Without SD-WAN Orchestrator (that is, using SD-WAN Center instead), that responsibility is provided to the MCN and RCN denoted devices. With the SD-WAN Orchestrator, monitoring and managing the large networks becomes simpler and the resources on those devices are utilized towards overlay delivery as opposed to network management.

The concept of Secondary Geo MCN and RCN for disaster recovery can be deployed with multi-region deployments. The concept of WAN-to-WAN Forwarding and Intermediate nodes adds value for multi-region deployments.

The high-level design for the financial customer, with integrated Citrix SD-WAN multi-region architecture is following:

[![SDWAN_RCN-RA-Image-3](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_003.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_003.png)

### Regions

A region in SD-WAN architecture is a geographical or administrative management domain defined by the customer, typically to divide a large network into two or more logical segments. The MCN, RCN, and Client nodes within a region are recommended to be in close latency proximity due to the potential backhaul requirements of internet traffic and proxy through the MCN or RCN for branch communications.

For a multi-region deployment, the default region is associated to the Master Control Node(MCN). The MCN manages multiple Regional Control Nodes (RCNs) and some of the client sites directly. Each RCN, in turn, manages multiple client sites.

[![SDWAN_RCN-RA-Image-4](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_004.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_004.png)

### MCN and RCN Redundancy

The Master Control Node (MCN) is the head-end appliance in the overlay network. It acts as a master controller of the overlay and the central administration point for the client nodes, hence the availability of the MCN is crucial. To ensure the maximum availability of MCN, the recommendation is to deploy in High Availability pair and add another as a Secondary Geo MCN to increase the levels of redundancy.

For the Secondary Geo MCN or RCN configuration, a site in a different geographical location from the primary site (for example, Primary Data Center) is configured to enable disaster recovery. This is to ensure the maximum availability of MCN and it is called as Secondary Geo MCN.

The Secondary Geo MCN continuously monitors the health of the primary MCN. If the primary MCN fails, the Secondary Geo MCN takes over the role of the active MCN. The Secondary Geo MCN appliance may not be the same platform model as the primary MCN. The appliance model can be selected based on the usage, bandwidth requirement, and the number of sites to be supported. The number of sites that are establishing a Virtual Path to the primary MCN, is also required to have a second Static Virtual Path to the Secondary Geo MCN site. For that reason, the models selected to be used for both the Primary MCN and the Secondary Geo MCN responsibility must both support the require number of Static Virtual Paths.

The best way to configure a Secondary Geo MCN would be to clone the existing MCN as it retains most of the MCN configuration. When a site is cloned, the entire set of configuration settings for that site are copied and then modified according to the **Secondary Geo MCN** settings.

Similar to MCN, the RCN has the same operation. A Primary RCN and Secondary Geo RCN can be configured, and all the above-mentioned capabilities are applicable for RCN as well.

[![SDWAN_RCN-RA-Image-5](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_005.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_005.png)

### MCN / RCN High-Availability

Citrix SD-WAN appliances can be deployed in a high availability configuration as a pair of appliances in Active or Standby High Availability (HA) roles. In HA configuration, two appliances are configured within the same subnet or same site to ensure fault tolerance.

High Availability can be configured for the MCN, RCN, and Client Node in the network. There is no dependency for HA between sites. In other words, if the Primary MCN site is deployed in HA, it is not required that the Secondary Geo MCN site is also deployed in HA.

For information on configuring High Availability, refer to the [Product Documentation](/en-us/netscaler-sd-wan/10/use-cases-sd-wan-virtual-routing/ha-deployment-modes.html) and [FAQ](/en-us/citrix-sd-wan/10-1/faqs.html).

[![SDWAN_RCN-RA-Image-6](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_006.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_006.png)

### SD-WAN Scalability

Citrix SD-WAN allows IT to create overlay tunnels across each WAN Path between the available WAN links at peer sites. Each WAN path utilizes Virtual IP Addresses to represent the endpoint for the tunnel. The aggregation of all potential WAN Paths between peer SD-WAN sites aggregates to provide a single virtualized path is known as Static or Dynamic Virtual Path. The Virtual Path available between peer SD-WAN sites allow packets to traverse the WAN utilizing the SD-WAN overlay network instead of the existing underlay which is less intelligent and cost inefficient.

In single-region deployment, the MCN is required to have a Virtual Path to every node in the network. However, in multi-region deployment, the MCN is only required to have Virtual Path to Regional Control Nodes. RCN denoted devices are required to have Virtual Path to only Client Nodes within a region, along with the MCN and other RCNs in the network. This Virtual Path is needed for the branches to communicate across the regions. MCN no longer required to be the Intermediate Node (or Mediator) for Dynamic Virtual Path Creation for all nodes in the network, the role is offloaded to the RCNs for each region.

### Virtual Path

Citrix SD-WAN allows IT to create a WAN path tunnel across each WAN link with the Virtual IP to represent the endpoint for each WAN link. SD-WAN aggregates all the WAN path into a single Virtual Path. This allows packets to traverse the WAN utilizing the SD-WAN overlay network instead of the existing underlay which is least intelligent and cost inefficient.

The MCN is only required to have Virtual Path to RCN nodes. RCN nodes are required to have Virtual Path to only Client Nodes within a region. MCN by default establishes a virtual path with RCN and RCN establishes a virtual path with connected branches. This virtual path is needed for the branches to communicate across the regions. MCN is no longer required to be the Intermediate Node (or Mediator) for Dynamic Virtual Path Creation, the role is offloaded to the RCNs.

### WAN-To-WAN Forwarding

The WAN-To-WAN Forwarding (W2WF) Group is used to allow client sites to communicate through an intermediary site with each other. When enabled the routing tables are shared between the site with WAN-To-WAN forwarding enabled and all client site in the specific WAN-To-WAN group. Also, when using Dynamic Virtual Paths WAN-To-WAN forwarding must be enabled. By default, all sites are in a default WAN-To-WAN forwarding group.

With WAN-To-WAN enabled on the MCN, remote site routes are advertised by MCN to serve as a proxy between branch networks. SD-WAN appliances running in client mode are not aware of other branches subnets until WAN-to-WAN forwarding is enabled on MCN. Once this option is enabled, branch SD-WAN nodes become aware of other branch subnets and all the traffic destined to other branches is forwarded to MCN.

SD-WAN Orchestrator deployed networks have WAN-To-WAN forwarding enabled by default. The destination route is advertised to Client Nodes in the network, and traffic is received by those nodes to the destination subnet, the MCN routes it to the correct destination using its Virtual Path to that SD-WAN hosting that subnet. A network can have multiple WAN-To-WAN Forwarding Groups. When WAN-to-WAN forwarding is not enabled on the MCN, Branch to Branch communication fails due to the lack of routes in the Client Nodes routing tables. Generally routes that are not in the local SD-WAN route table defaults to the passthrough or discard the routes.

### Virtual Path Routing

Dynamic Virtual Paths between the regional branch is not supported. As an example, Branch-1 from the Americas Region can communicate with Branch-902 on the EMEA Region only via their respective RCNs. Static Virtual Path between RCNs is created to avoid backhaul through the MCN. Branches connected with RCN can be configured to backhaul the Internet traffic. WAN-to-WAN Forwarding must be enabled on both RCNs. Refer to the [Citrix Documentation Page](/en-us/netscaler-sd-wan/10/best-practices/routing.html) for the best practices on routing.

[![SDWAN_RCN-RA-Image-7](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_007.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_007.png)

### Dynamic Virtual Path

As the demand grows for VoIP, screen-share, and video-conferencing applications in the enterprise network, the traffic is increasingly moving between offices. Network administrators have to configure a mesh network topology which enables connectivity between all the branches which is inefficient, time consuming and hard to manage.

With Citrix SD-WAN, there is no need to configure paths between every office. The solution enables Dynamic Virtual Path (DVP) feature which automatically creates paths between offices only on a as needed basis.

Dynamic Virtual Paths are established directly between sites, based on a configured threshold. The threshold is typically based on the amount of traffic occurring between those sites. Dynamic Virtual Paths are operational only after the specified threshold is reached, either through the packet per second count or byte count.

Branch to Branch communication initially uses the existing Static Virtual Path through the MCN as a proxy site.

As bandwidth and time threshold is met for DVP creation, a Dynamic Virtual Path is created by the MCN (that is, Intermediate Node) to allow the direct branch to branch communication without involving the MCN. Virtual Paths exist only when they are needed and reduce the amount of traffic getting transmitted to and from the data center. This action results in efficient usage of resources, because the bandwidth allocated for DVP is then reallocated back to the Static Virtual Path to the data center when the ranch to branch communication is noticed to subside.

Dynamic Virtual Path can only be formed between:

*  Branches of the same region
*  Between RCNs
*  Between RCN and Branch of default region

### Multi-Hop Routing Support

Citrix SD-WAN supports dynamic routing protocols, which allows easier administration of routes between branches from two regions, where traffic needs to traverse multiple SD-WAN tunnels. If Branch-401 (APJ Region) wants to communicate to Branch-902 (EMEA Region), the multi-region routing enables the communication from Branch-401 -> APJ RCN -> EMEA RCN to Branch-902.

### Route Summarization

With thousands of sites potentially in a multi-region architecture, the SD-WAN need to maintain the large number of routes in their routing table. So SD-WAN requires increased CPU, memory, and bandwidth resources to look up the large routing tables.

The route summarization feature in SD-WAN reduces the number of routes that the SD-WAN must maintain. A summary route is used as a single route that represents multiple routes. It saves bandwidth by sending a single route advertisement, which saves memory because only one route address is maintained as opposed to multiple. The CPU resources are used more efficiently by avoiding recursive lookups.

Regional Control Node(RCN) is capable to summarize all the regional network subnets and send only the summary route to MCN.

For example, if we define a subnet 172.16.0.0/16 as part of Region-1, and all the branches belonging to that region-1 uses the networks (172.16.1.0/24, 172.16.2.0/24, and so on) within the subnet defined as part of region definition. The RCN for Region-1 send the summary route (172.16.0.0/16) to the MCN and peer RCNs in the network.

SD-WAN administrator can configure a summary route with Local and Discard service types. This summary route is advertised to the next-hop devices. To learn more about summary route types, refer to [Product Documentation](/en-us/netscaler-sd-wan/10/routing/overlay-routing.html).

### SD-WAN Center Support

SD-WAN Center (SDWC) 10.0 supports multi-region deployment with two functional components - the SD-WAN Center Head-end and the SD-WAN Center Collector. In a multi-region deployment, each region is expected to have an SD-WAN Center Collector, which interfaces with all the sites of that region, to collect and store statistics data for that region.

The SD-WAN Center Head-end interfaces with all the Regional Collectors associated with the RCNs and doubles up as a collector for the default region. The SD-WAN Center Head-end provides a single point of access for monitoring and management purposes across the entire network, potentially spanning multiple regions.

The advantage is that the SDWC Collectors only contain data for their region. SDWC is useful for multi-region deployments where visibility to the data needs to be segmented between regions. SDWC Head-end would then be the central point that contains all data from all regions with the ability to filter by region.

[![SDWAN_RCN-RA-Image-8](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_008.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_008.png)

### SD-WAN Orchestrator

When using SD-WAN Orchestrator as the management tool for a multi-region deployment, a single SD-WAN Orchestrator is used to centrally manage all the regions, including the default region. The advantage is a simpler network with elasticity to scale resources on demand.

[![SDWAN_RCN-RA-Image-9](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_009.png)](/en-us/tech-zone/design/media/reference-architectures_sd-wan-multi-region_009.png)

### Partial Site Upgrade

With large scaled deployments upgrading hundreds of sites to the matching software version can be unsettling. This is where the Partial Site Update feature was introduced for multi-region deployments which allows the admin to stage a new software release on a small subset of sites (that is, lab or test sites) to verify stability before activating the staged software network-wide across the entire SD-WAN environment. Once software stability is confirmed, the Staged software can be then activated by completing the Activate Staged step in Change Management. The new features of the updated software can be fully tested with peace of mind that the new software has been proven stable. Some requirements include:

*  Must use the same major version number as the active software (for example, R10.1.2 active with staged R10.2.0, NOT R10.1.1 active with staged R11.0)
*  Configuration version of Active and Partially upgraded site must be identical to the rest of the network
*  High Availability sites must have the standby HA unit service disabled before the local change management on the primary HA unit is done

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s9e97c2ae82844eca).

## References

[Citrix SD-WAN Single Region Reference Architecture](/en-us/tech-zone/design/reference-architectures/sdwan.html)

[High Availability Deployment Modes](/en-us/netscaler-sd-wan/10/use-cases-sd-wan-virtual-routing/ha-deployment-modes.html)

[FAQ on High Availability Configuration](/en-us/citrix-sd-wan/10-1/faqs.html)

[Routing Best Practices](/en-us/netscaler-sd-wan/10/best-practices/routing.html)

[Summary Route Types](/en-us/netscaler-sd-wan/10/routing/overlay-routing.html)
