# Citrix SD-WAN Cloud Direct service overview
## The Cloud Direct service extends Citrix SD-WAN optimal routing and delivery benefits to SaaS.  It hosts Cloud Direct service PoPs at major Internet Exchanges and transports branch office endpoint traffic in tunnels across the internet to the front door of popular SaaS sites.
![ Citrix SD-WAN Cloud Direct Service](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_SDWAN-CDs.png)

The [Cloud Direct service]( https://docs.citrix.com/en-us/citrix-sd-wan-center/11/cloud-direct-service.html) virtually bundles multiple branch office internet access links such as DSL, Cable, and LTE.  It creates redundant UDP tunnels between a Cloud Direct service process hosted on branch office SD-WAN appliances and Cloud Direct service gateways hosted in Points of Presence (PoP) at Internet Exchanges, near popular SaaS sites.  It is able to utilize and monitor each link within the bundles to mitigate against data link or network issues in the 1st mile which is the segment of the circuit between the branch office and the Internet Service Provider PoP where issues often occur.  Further it aides with optimal transport of sessions throughout their path across the internet to target sites in the cloud.

It can protect delivery sensitive VoIP traffic by applying bi-directional quality-of-service (QoS) prioritization and shaping with class-of-service (CoS) tagging as it sends that traffic through the links with the least latency, loss, and jitter, improving the mean opinion score (MOS).  In parallel it automatically identifies other traffic types and applies pertinent QoS tags to map to one of six classes of service.  Subsequently it prioritizes traffic for egress transmission. Once on the Cloud Direct network infrastructure that traffic is transport according to CoS marking over a high-speed private delivery network. 

![ How DNS works](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_HowDNSWorks.PNG)
### Cloud Era SaaS
When Enterprises subscribe to SaaS services in the cloud, they can expect access to multiple mirror sites hosted on public clouds with global Points-of-Presence (PoPs).  Therefore, providing optimal performance for users entails transporting traffic from their endpoint to the nearest PoP.  For most enterprises this means solving access for distributed branch offices.

![ Cloud Era Application to SaaS Migration](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CES-Migration.png)
#### Cloud Era Application to SaaS Migration
Enterprises continue to adopt SaaS applications in lieu of traditional hosted applications. In fact, many organizations host business critical applications on SaaS, therefore ensuring enterprise reliability and performance is a key consideration.

![ SaaS backhauled via the Data Center](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CES-DataCenter.png)
 #### SaaS backhauled via the Data Center
In a traditional model, backhauling branch internet traffic worked when the minority of web traffic was not performance sensitive.  Now with a significant number of business-critical applications powered by SaaS -- data center centric network infrastructure must be redesigned.

![ SaaS direct breakout from Branch Offices](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CES-BranchOffice.png)
#### SaaS direct breakout from Branch Offices
With SD-WAN, individual applications can be routed directly to the internet to reduce latency.  However, the internet still lacks quality mechanisms to transport enterprise SaaS optimally.  The internet delivers all traffic indiscriminately with no preference or in a “best effort” manner.

![ SaaS via Citrix SD-WAN Cloud Direct](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CES-CloudDirect.png)
#### SaaS via Citrix SD-WAN Cloud Direct
SaaS apps are deployed across multiple cloud locations globally. To reach those applications more efficently and reliably, Cloud Direct PoPs are strategically located at major internet and cloud service provider exchanges with direct peering to thousands of SaaS applications and networks, enabling higher performance connectivity and service reliability.

![ Best Effort Internet](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BestEffortInternet.png)

### Cloud Access Optimization
For branch office users to reach public SaaS services their sessions must traverse the internet.  This network is composed of a vast and complex web of circuits, service providers, network equipment, data centers, and routing domains. The internet is considered a “best effort” network because there are no guarantees of transport reliability or quality. Therefore, trusting delivery over the best effort internet is a risk.  

Cloud Direct branch tunnels terminate into redundant Cloud Direct PoPs. These PoPs are purpose-built for reliability, performance, and optimal connectivity. Each PoP consists of scalable, fully redundant servers connected to carrier-grade switches and routers. They are connected to multiple upstream transit circuits and interconnected via a diverse and dedicated private high-speed backbone.

Each Cloud Direct PoP is strategically located at key internet exchange points and peered with over 150 cloud, content, and carrier networks. Therefore, the proximity of a customer’s branch to a Cloud Direct PoP is less important than the location of SaaS to a Cloud Direct PoP. If the SaaS application is better accessed from a Cloud Direct PoP that is different than the PoP the customer’s branch site is connected to, that traffic will benefit from the private high-speed backbone that connects the Cloud Direct PoPs together. 

With these capabilities the Cloud Direct service optimizes delivery over the path between branch office and SaaS application. There are 3 distinct parts of the path that the Cloud Direct service optimizes:
* Branch Office Optimization
* Internet Optimization
* Cloud Core Optimization

![ Branch – 1st Mile Circuits](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BranchFirstMile.PNG)

### Branch Optimization
Branch office optimization occurs on the Citrix SD-WAN appliance located in the local office. Here it interfaces with the Internet Service Provider’s (ISP) access links.  The physical and data link layers vary including DSL, Cable Modem, or Mobile LTE, yet on the network layer they ultimately provide internet access.  While this segment is typically the shortest portion of the overall journey to the cloud, many issues may occur over it that can affect quality.  The Cloud Direct service provides several features to optimize access at the branch.

* Link Aggregation
* Prioritization
* Intelligent Steering

#### Link Aggregation
![ Link Aggregation](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-LnkAgr.png)
With link aggregation, the Citrix SD-WAN appliance treats up to four links as a single bundle and load balances traffic according to capacity.  In the absence of this functionality, a typical router would have multiple default routes to each respective ISP and may only select one as the active primary, or route traffic across multiple routes unevenly.

Citrix SD-WAN can allocate a fixed portion of each circuit to the Cloud Direct service or general internet use. Cloud Direct utilizes all available bandwidth allocated to the service. Probes continuously monitor availability and upon detecting a link outage the “virtual” aggregated circuit is reduced accordingly by removing the bad link from the bundle and the session is rerouted over a good link.  Sessions are rerouted at the network layer avoiding interruption to user sessions.

#### Prioritization
![ Prioritization](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-Priori.png)
Traditionally branch routers may transmit traffic on a First-In-First-Out basis, or otherwise rely on prioritization tags coming from the LAN which may be inconsistent or inexistent.  The Citrix SD-WAN appliance hosted Cloud Direct service process prioritizes egress transmission queues according to class of service. On reception they automatically identify traffic by 1 of 6 types, apply a QoS tag, and prioritize for appropriate handling.

The traffic is prioritized according to class to ensure the highest priority sessions hit the egress transmit queues first.  The limited bandwidth of the local DSL, Cable, or LTE access links may be the bottleneck over the entire path to the cloud, therefore ensuring appropriate prioritization of traffic is critical to delivering maximum end-to-end QoS.

#### Intelligent Steering
![ Intelligent Steering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-IntStr.png)
Traditionally branch routers will consider a link “up” state indication that it can handle the full bandwidth associated with the interface, irrespective of its actual throughput capability.  The Cloud Direct service sends probes at 10 millisecond (ms) intervals over each link, constantly monitors latency, jitter, loss, congestion, and state. 

Changes in state may vary from a black out where an entire link goes down, to a brown out where limited throughput or performance falls below the QoS that the class requires. When a state change is detected the service seamlessly reroutes sessions over to a “good” link. Intelligent steering avoids impact to session connections and subsequently maintains good user experience.

![ Peering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_Peering.PNG)

### Internet Optimization
The general Internet lies between the branch office Citrix SD-WAN and Cloud Direct PoPs.  The majority of the segment is up to local ISPs to manage and traffic is typically transported as best effort with no quality or service level agreements.  Nevertheless, the Cloud Direct service includes several benefits to optimize traffic while in route.

* Hitless Failover
* Extended Visibility
* VoIP Protection

#### Hitless Failover
![ Hitless Failover](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-Hitles.png)
Sessions originated from the Cloud Direct service process, on the branch office hosted Citrix SD-WAN appliance, are transported across non-encrypted OpenVPN UDP based tunnels to a primary Cloud Direct service PoP gateway, with a secondary PoP on standby.  The tunnel source and destination both are always public IP addresses, assigned and managed by the Cloud Direct service.  

On the branch Citrix SD-WAN appliance the tunnel is sourced from a fixed LAN side public IP address. Each ISP circuit connected to the SD-WAN appliance will direct traffic over a different path across the internet to the Cloud Direct POP. When a failure occurs over one path, Cloud Direct reroutes sessions over one of the other available paths at the network layer. 

If a failure were to occur at the primary Cloud Direct service PoP the route for the tunnel endpoint, already be advertising from the secondary PoP by BGP with a lower preference, would become the preferred path immediately once the route change converged over the internet.

Thus, with Hitless Failover, transport or application layer protocols are unaware of the outage and subsequently sessions do not incur typical re-setup delay at those layers, and thus the user experience is not impacted. This includes, but is not limited to VPN, virtual desktop, SSH, VoIP, or Web Conference sessions.

#### Extended Visibility
![ Extended Visibility](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-ExtVis.png)
Enterprises may have some visibility of traffic statistics over their 1st mile circuits, between the branch and their respective ISP PoPs, yet are limited beyond that.  The Cloud Direct service provides traffic details including latency, loss, jitter, and throughput from the Citrix SD-WAN appliance interface to each respective 1st mile circuit, across the internet, to the Cloud Direct service PoP gateways.

The Cloud Direct service also provides reporting on performance statistics:

* Total uptime improvement – additional time of internet availability as a result of the Cloud Direct service intervention
* QoS adaptations – the volume of intelligent path changes to protect sensitive traffic like VoIP
* Major Issues Fixed – the number of circuit blackout or brownouts avoided by intelligent routing
* Circuit Performance - Overall % uptime for each circuit and aggregate time of major and minor issues incurred respectively

Administrators will have the ability to view this data on per day/week/month via [SD-WAN Orchestrator]( https://docs.citrix.com/en-us/citrix-sd-wan-orchestrator.html).  They can also have a report emailed on a regular basis.

#### VoIP Protection
![ VoIP Protection](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-VoIPro.png)
Delivering voice-over-IP (VoIP) over the Internet is a challenge because it does not include the QoS protection provided on an Enterprise network.  While Cloud Direct tags VoIP packets appropriately for expedited forwarding and honors those over the Cloud Direct network the ISPs between the branch office and the Cloud Direct PoPs will not. 

Jitter is one of the biggest factors in poor call quality, often caused by Internet congestion where ISPs cannot forward packets at the rate they are received and thus must buffer them.  Eventually they are forwarded, albeit with different inter-packet spacing, causing jitter.  It is not as significant a problem for other forms of traffic, but to deliver good VoIP call quality it is essential that buffering in transit is minimized.  With its constant monitoring, Cloud Direct has visibility across all available paths for jitter caused by congestion and it dynamically moves the flows to better paths as needed.

![ Enterprise QoS](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_EnterpriseQoS.png)

### Cloud Core Optimization
At the Cloud Direct service PoP, the OpenVPN tunnel that was originated at the branch office, after transport across the internet, is de-encapsulated and ready for delivery to the destination cloud provider, that hosts the target SaaS site.  The Cloud Direct service provides several benefits to transport traffic efficiently across this last segment:

* Enterprise QoS
* Extensive Peering
* Private Backbone

#### Enterprise QoS
![ Enterprise QoS](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-EntQos.png)
Upon reception of packets, the Citrix SD-WAN appliance hosted Cloud Direct service process automatically identifies six types of traffic, maps them to a class of service and applies a QoS tag accordingly.  

1. VoIP signaling traffic
2. VoIP bearer traffic
3. Urgent traffic (Remote Desktop, Citrix HDX, Interactive Apps)
4. Interactive Apps, VPN
5. Bulk Data (Netflix, Dropbox)
6. Other

Packets originated at the branch office are transported across the Cloud Direct service tunnel.  Then at the PoP on the far end traffic is further delivered over the Cloud Direct private backbone over which QoS tags are honored as packets are transferred hop-to-hop.  

#### Extensive Peering
![ Extensive Peering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-ExPeer.png)
With 150+ peering agreements with premium cloud and service providers per PoP (see insert), Cloud Direct service provides efficient paths to SaaS sites reducing latency from branch users accessing them.  After transport over the Cloud Direct service tunnel, PoP gateways check their route tables for the shortest path to the target site and will often find one a few hops away thanks to Extensive Peering agreements (see insert).  

#### Private Backbone
![ Private Backbone](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-PrivBB.png)
The Cloud Direct service PoPs are connected by a private backbone with redundant high-speed circuits. This allows it to honor traffic with QoS tags and deliver it without typical best effort internet challenges such as oversubscription, congestion, and outage. When targets are not immediately reachable from the primary PoP gateway route table the respective traffic is sent, over the Cloud Direct private backbone, to the PoP with the shortest route.

![  as a Service](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_SaaSUcaaSDaaS.PNG)

### Use Cases
Citrix SD-WAN is a powerful networking technology that provides optimal routing across all available paths.  With the addition of the Cloud Direct service it provides premium transport over the public internet including enterprise-grade reliability and performance. Let’s discuss some use cases where the it provides the most benefits.

* SaaS
* UCaaS
* DaaS

#### SaaS
The Cloud Direct service’s primary benefits for SaaS services include; its tunneling capability, which provides link load-balancing for up to four customer-provided Internet circuits; bi-directional application-aware QoS; link-to-link and PoP-to-PoP same-IP failover; and real-time service monitoring with benefit reporting.

As the SaaS application or customer’s branch site are further away from the Cloud Direct PoPs, the performance and reliability benefits provided by Cloud Direct can decrease, due to the increasing number of hops from the SaaS application to a Cloud Direct PoP increasing the length of the Cloud Direct tunnel to the customer’s branch site. In those cases, simple application routing rules make it easy to send that traffic over the internet rather than through the Cloud Direct service. For example, customers with branch sites in China accessing SaaS applications within China should not send that traffic through the Cloud Direct network. However, when those sites need to access SaaS applications hosted in either the US or Western Europe, sending that traffic through the Cloud Direct service can result in better performance and reliability.

#### UCaaS
The Cloud Direct service’s primary benefits for UCaaS include maintaining communication sessions during outages in the communication path with hitless failover; monitoring and dynamically switching internet paths when call quality degrades with VoIP Protection; and maintaining call quality with Enterprise QoS.

UCaaS includes VoIP based communication solutions such as Microsoft Teams and Skype.  Call setup is done via one of multiple redundant sites hosted, across the globe, on the public internet.  DNS queries, to resolve the sites, are transported via the Cloud Direct service tunnel and the source of the query is the Cloud Direct service POP ensuring the nearest site is returned.  After call setup the bearer traffic may be proxied via the same site or setup directly to a peer on the intranet or internet.  Citrix SD-WAN ensures that the session traffic is routed via the most efficient path while the Cloud Direct service ensures paths over the public internet are transported optimally.

#### DaaS
The Cloud Direct service provides several benefits for [ Citrix Virtual Apps and Desktops service]( https://docs.citrix.com/en-us/citrix-virtual-apps-desktops.html) and [ Citrix Managed Desktops]( https://docs.citrix.com/en-us/citrix-managed-desktops.html)  users to improve user experience including; Better use experience when links are congested; and seamless aggregation and steering to avoid session outages when local access links fail, or internet paths encounter brown outs.

One of the many benefits of Citrix Virtual Apps and Desktops and Citrix Managed Desktops is that its virtual delivery agents (VDA) may be hosted in a variety of hybrid cloud locations.  After a Citrix SD-WAN appliance identifies Citrix Virtual Apps and Desktops HDX (the delivery protocol) session it checks all available virtual paths for the optimal route.  If it is destined for a public internet site and the service is mapped to Cloud direct the session traffic is tagged, prioritized as high priority interactive and transported accordingly. Citrix SD-WAN with Cloud Direct service is the ideal solution to route and deliver Citrix Virtual Apps and Desktops and Citrix Managed Desktops optimally across the public internet.

## Contributors
**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:**  Mark King, Daniel Feller 
