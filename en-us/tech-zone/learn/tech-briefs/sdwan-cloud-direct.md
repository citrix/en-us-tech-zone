---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Karthick Srivatsan, Mark King, Daniel Feller
description: Optimize SaaS access for branch users by tunneling session traffic to Internet Exchanges with direct connectivity to popular sites.
tz_title: SD-WAN Cloud Direct service
tz_products: citrix-networking;
---
# Tech Brief: SD-WAN Cloud Direct service

Citrix SD-WAN Cloud Direct extends Citrix SD-WAN optimal routing and delivery optimization benefits to SaaS. The service tunnels branch office endpoint traffic, to the front door of popular SaaS sites, via PoPs hosted at major Internet Exchanges.

[![Citrix SD-WAN Cloud Direct](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_sdwan-cds.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_sdwan-cds.png)

[Citrix SD-WAN Cloud Direct](/en-us/citrix-sd-wan-center/11/cloud-direct-service.html) virtually bundles multiple branch office internet access links such as DSL, Cable, and LTE. It creates redundant UDP tunnels. On one end is a Citrix SD-WAN Cloud Direct process, hosted on branch office SD-WAN appliances. On the other end are gateways hosted in Citrix SD-WAN Cloud Direct PoPs. The gateways are hosted in Points of Presence (PoP) at Internet Exchanges, near popular SaaS sites. It is able to utilize and monitor each link within the bundles to mitigate against data link or network issues in the first mile. The first mile is the segment of the circuit between the branch office and the ISP PoP where issues often occur. Further it aides with optimal transport of sessions throughout their path across the internet to target sites in the cloud.

It can protect delivery sensitive VoIP traffic by applying bi-directional quality-of-service (QoS) prioritization and shaping with class-of-service (CoS) tags. It sends that marked traffic through the links with the least latency, loss, and jitter, improving the [mean opinion score (MOS)](https://en.wikipedia.org/wiki/Mean_opinion_score). In parallel it automatically identifies other traffic types and applies pertinent QoS tags to map to one of six classes of service. Then, it prioritizes traffic for egress transmission. Once on the Cloud Direct network infrastructure that traffic is transported, according to its CoS marking, over a high-speed private delivery network.

## Cloud Era SaaS

When Enterprises subscribe to SaaS services in the cloud, they can expect to have access to multiple mirror sites, hosted on public clouds, with global PoPs. Therefore, providing optimal performance for users entails transporting traffic from their endpoint to the nearest PoP. For most enterprises to accomplish this they must solve access for distributed branch offices.

### Cloud Era Application to SaaS Migration

Enterprises continue to adopt SaaS applications in lieu of traditional hosted applications. As these applications become more business critical, ensuring enterprise reliability and performance is a key consideration.

### SaaS backhauled via the Data Center

In a traditional model, backhauling branch internet traffic worked when the minority of web traffic was not performance sensitive. Now with a significant number of business-critical applications, powered by SaaS, data center centric network infrastructure must be redesigned.

### SaaS direct breakout from Branch Offices

Individual applications can be routed directly to the internet, to reduce latency, with Citrix SD-WAN. However, the internet still lacks quality mechanisms to transport enterprise SaaS optimally. The internet delivers all traffic indiscriminately with no preference or in a “best effort” manner.

### SaaS via Citrix SD-WAN Cloud Direct

Citrix SD-WAN Cloud Direct provides optimal SaaS access for distributed branch offices.

SaaS apps are deployed across multiple cloud locations globally. Cloud Direct PoPs are strategically located at major internet and cloud service provider exchanges. There they have reach to thousands of SaaS applications and networks, enabling higher performance, connectivity, and service reliability.

#### Best Effort Internet

Accessing business critical SaaS applications over the internet is problematic since traffic delivery over the internet is best effort by default. It is a collection of service providers connected using various network equipment, administrators, and circuits with varying levels of reliability. Therefore, SaaS connections are susceptible to disruption due to outages, oversubscription, and congestion.

*  **Outage** – may be caused by various factors such as the failure of circuits from cuts due to unplanned digging, failure of equipment from anomalies, or failure of data centers from natural disasters.
*  **Oversubscription** – lower tier providers depend on paid transit circuits to access certain cloud providers and SaaS sites.  To minimize their costs, they may over subscribe those circuits routing more customer traffic bandwidth than there is capacity available causing packet loss, retransmission, and ultimately latency.
*  **Congestion** – similarly, due to oversubscription or just peak usage, ISP routes may need to buffer traffic destine over congested circuits which are at full transfer capacity. Accessing business critical SaaS applications over the internet is problematic since traffic delivery over the internet is best effort by default. It is a collection of service providers connected using various network equipment, administrators, and circuits with varying levels of reliability. Therefore, SaaS connections are susceptible to disruption due to outages, oversubscription, and congestion.

![Best Effort Internet](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BestEffortInternet.PNG)

## Cloud Access Optimization

For branch office users to reach public SaaS services their sessions must traverse the internet. This network is composed of a vast and complex web of circuits, service providers, network equipment, data centers, and routing domains. The internet is considered a “best effort” network because there are no guarantees of transport reliability or quality. Therefore, trusting delivery over the best effort internet is a risk.

Cloud Direct branch tunnels terminate into redundant Cloud Direct PoPs. These PoPs are purpose-built for reliability, performance, and optimal connectivity. Each PoP consists of scalable, fully redundant servers connected to carrier-grade switches and routers. They are connected to multiple upstream transit circuits and interconnected via a diverse and dedicated private high-speed backbone.

Each Cloud Direct PoP is strategically located at key internet exchange points. There Cloud Direct PoPs each peer with over 150 cloud, content, and carrier networks. Therefore, the proximity of a customer’s branch to a Cloud Direct PoP is less important than the location of SaaS to a Cloud Direct PoP. SaaS traffic, destined to sites via Cloud Direct PoPs, benefits from traversing a private high-speed backbone that connects them together.

With these capabilities Citrix SD-WAN Cloud Direct optimizes delivery over the path between branch offices and SaaS applications. There are 3 distinct parts of the path that Citrix SD-WAN Cloud Direct optimizes:

*  Branch Office Optimization
*  Internet Optimization
*  Cloud Core Optimization

### Branch – First Mile Circuits

1st mile circuits are access links to ISPs that are typically lower speed, lower cost, and subsequently lower reliability compared to circuits used by large enterprise and data centers. While the Citrix SD-WAN appliances connect to the modems via Ethernet the data link technologies vary including:

*  **DSL** – is a family of technologies that are used to transmit digital data over telephone lines. The bit rate typically ranges from 256 kbps to over 100 Mbps and varies by upload and download speed depending on several factors.
*  **Cable** – like DSL cable provides connectivity from the ISP to a branch office. And akin to how DSL is delivered over the telephone network infrastructure it is delivered over the cable television infrastructure. Download speeds range from 384 kbps to more than 50 Mbps.
*  **4G LTE** – is a standard for wireless communication for mobile devices, yet it is increasingly used as an alternative to fixed internet circuits providing fairly high-speed access, up to 30 Mbps upload and download, virtually anywhere.

## Branch Optimization

Branch office optimization occurs on the Citrix SD-WAN appliance located in the local office. Here it interfaces with the ISP access links. The physical and data link layers vary including DSL, Cable Modem, or Mobile LTE. They ultimately provide internet access on the network layer. The segment is typically the shortest portion of the overall journey to the cloud, however many issues can occur over it that can affect quality. Citrix SD-WAN Cloud Direct provides several features to optimize access at the branch.

*  Link Aggregation
*  Prioritization
*  Intelligent Steering

### Link Aggregation

![Link Aggregation](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-LnkAgr.png)

Citrix SD-WAN Cloud Direct can aggregate up to four Internet links as a single bundle and load balance traffic according to capacity. A typical router could have multiple default routes to each respective ISP. It may only select one as the active primary, or route traffic across multiple routes unevenly.

Citrix SD-WAN can allocate a fixed portion of each circuit to Citrix SD-WAN Cloud Direct or general internet use. Cloud Direct utilizes all available bandwidth allocated to the service. Probes continuously monitor availability. Upon detecting a link outage, the “virtual” aggregated circuit is condensed by removing the “bad link” from the bundle, and rerouting sessions over “good links”. Sessions are rerouted at the network layer avoiding interruption to user sessions.

### Prioritization

![Prioritization](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-Priori.png)

Traditionally branch routers may transmit traffic on a First-In-First-Out basis, or otherwise rely on prioritization tags coming from the LAN which may be inconsistent or inexistent. Citrix SD-WAN Cloud Direct identifies, prioritizes, and tags traffic into 1 of 6 classes of service to be processed accordingly throughout Citrix SD-WAN Cloud Direct infrastructure. This happens in both directions, automatically.

The traffic is prioritized according to class. This ensures that the highest priority sessions are sent to the egress transmit queues first. The limited bandwidth of the local DSL, Cable, or LTE access links may be the bottleneck over the entire path to the cloud. Therefore, ensuring appropriate prioritization of traffic is critical to delivering maximum end-to-end QoS.

### Intelligent Steering

![Intelligent Steering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_BO-IntStr.png)

Traditionally branch routers consider a link “up” state indication that it can handle the full bandwidth associated with the interface, irrespective of its actual throughput capability. Citrix SD-WAN Cloud Direct sends probes at 10 millisecond (ms) intervals over each link to constantly monitors latency, jitter, loss, congestion, and state.

Changes in state can vary between black out and brown out. A black out is when an entire link goes down. A brown out is when limited throughput or performance falls below the QoS that the class requires. When a state change is detected the service seamlessly reroutes sessions over to a “good” link. Intelligent steering avoids an impact to session connections, allowing it to maintain a good user experience.

#### Peering

Peering is a voluntary interconnection of internet autonomous systems to expedite traffic between hosted content and users. The average user or enterprise accessing the internet through a typical service provider must pass through several hops before reaching their desired destination. Having direct access to service providers with Extensive Peering agreements reduces their traffic hops and ultimately reduces their session latency.

*  **BGP** – the Border Gateway Protocol is the primary protocol use to exchange routes between ISPs and is the mechanism used to determine which traffic is passed as a result of peering agreements.
*  **IX** – exchange points are typically large data centers where public peering takes place between providers over large switch fabrics.
*  **Transit** – service providers that do not have sufficient peering typically pay for circuits to access higher tier provides to provide access or “transit” to destinations needed by their customers.

![Peering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_Peering.PNG)

## Internet Optimization

The general Internet lies between the branch office Citrix SD-WAN and Cloud Direct PoPs. Most of the segment is up to local ISPs to manage and traffic is typically transported as best effort with no quality or service level agreements. Nevertheless, Citrix SD-WAN Cloud Direct includes several benefits to optimize traffic while in route.

*  Hitless Failover
*  Extended Visibility
*  VoIP Protection

### Hitless Failover

![Hitless Failover](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-Hitles.png)

Sessions originated from the Citrix SD-WAN Cloud Direct process, on the branch office hosted Citrix SD-WAN appliance, are transported across non-encrypted OpenVPN UDP based tunnels. The tunnels terminate on a primary Cloud Direct service PoP gateway, with a secondary PoP on standby. The tunnel source and destination both are always public IP addresses, assigned and managed by Citrix SD-WAN Cloud Direct.

On the branch Citrix SD-WAN appliance the tunnel is sourced from a fixed LAN side public IP address. Each ISP circuit, connected to the SD-WAN appliance, directs traffic over a different path across the internet to the Cloud Direct PoP. When a failure occurs over one path, Cloud Direct reroutes sessions over one of the other available paths at the network layer.

Citrix SD-WAN Cloud Direct advertises a route to the tunnel end point from a secondary PoP via BGP with a lower preference. If a failure occurred at the primary Cloud Direct service PoP the secondary site would become the route once the change converged over the internet.

Thus, with Hitless Failover, transport or application layer protocols are unaware of the outage. Later sessions do not incur typical delay to get reestablished at those layers, and the user experience is not impacted. This benefit applies to various session types including VPN, virtual desktop, SSH, VoIP, or Web Conference sessions.

### Extended Visibility

![Extended Visibility](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-ExtVis.png)

Enterprises have some visibility of traffic statistics between the branch and their respective ISP PoPs yet are limited beyond their first mile circuits. Citrix SD-WAN Cloud Direct provides traffic details including latency, loss, jitter, and throughput. The service has visibility across the tunnel path between the Citrix SD-WAN appliance and the PoP gateway.

Citrix SD-WAN Cloud Direct also provides reporting on performance statistics:

*  Total uptime improvement – extra time of internet availability as a result of Citrix SD-WAN Cloud Direct intervention
*  QoS adaptations – the volume of intelligent path changes to protect sensitive traffic like VoIP
*  Major Issues Fixed – the number of circuit blackout or brownouts avoided by intelligent routing
*  Circuit Performance - Overall % uptime for each circuit and aggregate time of major and minor issues incurred respectively

Administrators have the ability to view this data on per day/week/month via [SD-WAN Orchestrator](/en-us/citrix-sd-wan-orchestrator.html). They can also have a report emailed regularly.

### VoIP Protection

![ VoIP Protection](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_IO-VoIPro.png)

Delivering voice-over-IP (VoIP) over the Internet is a challenge. It does not include the QoS protection provided on an Enterprise network. Cloud Direct tags VoIP packets appropriately for expedited forwarding and honors them over the Cloud Direct network. However, ISPs between the branch office and the Cloud Direct PoPs, do not honor the tags.

Jitter is one of the biggest factors in poor call quality. Jitter is often caused by Internet congestion, where ISPs cannot forward packets at the rate they are received, and thus must buffer them. Eventually they are forwarded, albeit with different inter-packet spacing, causing jitter. It is not as significant a problem for other forms of traffic. However, to deliver good VoIP call quality it is essential that buffering in transit is minimized. Cloud Direct has visibility across all available paths for jitter that is often caused by congestion. It dynamically moves the flows to better paths as needed.

#### Enterprise QoS factors

Depending on the application Enterprise network traffic has different QoS requirements:

*  **Latency** – pertains to the length of time it takes a packet to reach its destination which grows based on distance, congestion, or other limiting factors on the network.
*  **Loss** – pertains to the rate of packet loss due to congestion avoidance, queue starvation, dirty WAN data links, or other network issues. This affects available bandwidth and retransmission.
*  **Jitter** – pertains to the average difference in packet arrival time relative to their transmission time. This has the most effect on VoIP call quality.

![Enterprise QoS](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_EnterpriseQoS.PNG)

## Cloud Core Optimization

At the Citrix SD-WAN Cloud Direct PoP, the OpenVPN tunnel that was originated at the branch office is de-encapsulated. Thereafter the traffic is delivered to the destination cloud provider that hosts the target SaaS site. Citrix SD-WAN Cloud Direct provides several benefits to transport traffic efficiently across this last segment:

*  Enterprise QoS
*  Extensive Peering
*  Private Backbone

### Enterprise QoS

![Enterprise QoS](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-EntQos.png)

Upon reception of packets, the Citrix SD-WAN appliance hosted Cloud Direct service process automatically identifies six types of traffic. Once identified it maps them to a CoS and applies a QoS tag accordingly.

1.  VoIP signaling traffic
2.  VoIP bearer traffic
3.  Urgent traffic (Remote Desktop, Citrix HDX, Interactive Apps)
4.  Interactive Apps, VPN
5.  Bulk Data (Netflix, Dropbox)
6.  Other

Packets originated at the branch office are transported across the Citrix SD-WAN Cloud Direct tunnel. Then at the PoP, on the far end, they routed to the target SaaS site via the shortest path. If they traverse the Cloud Direct private backbone, QoS tags are honored, as packets are transferred hop-to-hop.

### Extensive Peering

![Extensive Peering](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-ExPeer.png)

Each Citrix SD-WAN Cloud Direct PoP has over 150 peering agreements with premium cloud and service providers. With the access these peers offer the service is able to provide efficient paths, reducing latency for branch users accessing SaaS sites. When PoP gateways check their route tables, they often find the shortest path to the target site a few hops away.

### Private Backbone

![Private Backbone](/en-us/tech-zone/learn/media/tech-briefs_sdwan-cloud-direct_CO-PrivBB.png)

Citrix SD-WAN Cloud Direct PoPs are connected by a private backbone with redundant high-speed circuits. The private backbone allows it to honor traffic with QoS tags appropriately. The private backbone also allows it to deliver it without typical best effort internet challenges such as oversubscription, congestion, and outage. The primary PoP gateway routes traffic, over the private backbone, to the PoP with the shortest path.

#### As a Service

*  **Software as a Service**
SaaS is a software licensing and delivery model in which software is licensed on a subscription basis and is centrally hosted within a publicly accessible cloud computing platform. SaaS applications are typically accessed by users using a web browser or other thin client technology. SaaS has become a common delivery model for many business applications, including office productivity, payroll processing, point-of-sale, customer relationship management, enterprise resource planning, virtualization, development, and other enterprise software.
*  **Unified Communications as a Service**
UCaaS is a way of delivering enterprise communication service through a managed cloud service model. This can include voice and telephony, meeting solutions, unified and instant messaging, contact and collaboration services, and other similar functions.
*  **Virtual Applications and Desktops Service**
A solution that helps you optimize productivity with universal access to Virtual Applications and Desktops as a Service (DaaS) from any device. Virtual Apps and Desktops may be hosted in public or private clouds and accessed from private networks or over the public internet.

## Use Cases

Citrix SD-WAN is a powerful networking technology that provides optimal routing across all available paths. With the addition of Cloud Direct functionality, it provides premium transport over the public internet including enterprise-grade reliability and performance. Let’s discuss some use cases where it provides the most benefits.

*  SaaS
*  UCaaS
*  DaaS

### SaaS

Citrix SD-WAN Cloud Direct has several benefits for access to SaaS. It provides link load-balancing for up to four customer-provided Internet circuits. It helps ensure quality with bi-directional application aware QoS. It includes resiliency with link-to-link and PoP-to-PoP same-IP failover. It provides visibility with real-time service monitoring with benefit reporting.

The performance and reliability benefit the service provides decrease the further the SaaS sites and branch offices are from Cloud Direct PoPs. More hops increase the length of the Cloud Direct tunnel to the customer’s branch site. In those cases, traffic may be routed directly, over the internet rather than through Citrix SD-WAN Cloud Direct, with simple application routing rules. For example, customers with branch sites in China, accessing SaaS applications within China, should not send that traffic through the Cloud Direct network. When those sites need to access SaaS applications hosted in either the US or Western Europe, sending that traffic through Citrix SD-WAN Cloud Direct can result in better performance and reliability.

### UCaaS

Citrix SD-WAN Cloud Direct has several benefits for access to UCaaS services. Sessions persist during outages in the communication path with hitless failover. The service monitors and dynamically switches internet paths when call quality degrades with VoIP Protection. It also maintains call quality with Enterprise QoS.

UCaaS includes VoIP based communication solutions such as Microsoft Teams and Skype. Call setup is done via one of multiple redundant sites hosted, across the globe, on the public internet. DNS queries are transported via the Citrix SD-WAN Cloud Direct tunnel. The source of the query is the Citrix SD-WAN Cloud Direct PoP ensuring the nearest site is returned. After call setup the bearer traffic can be proxied via the same site. Alternatively, it may be set up directly to a peer on the intranet or internet. Citrix SD-WAN Cloud Direct ensures the session traffic is routed via the most efficient path.

### DaaS

Citrix SD-WAN Cloud Direct provides several benefits for [Citrix Virtual Apps and Desktops service](/en-us/citrix-virtual-apps-desktops.html) and [Citrix Virtual Apps and Desktops Standard for Azure](/en-us/citrix-managed-desktops.html) users to improve user experience. The service provides better user experience when links are congested. It also aggregates and steers traffic to avoid session outages when local access links fail.

Citrix Virtual Apps and Desktops VDAs may be hosted in various hybrid cloud locations. After a Citrix SD-WAN appliance identifies an HDX session (the delivery protocol) it checks all available virtual paths for the optimal route. If the session is mapped to Citrix SD-WAN Cloud Direct, its traffic is tagged as interactive. Then the session is prioritized and transported accordingly. Citrix SD-WAN with Cloud Direct service is the ideal solution to route and deliver Citrix Virtual Apps and Desktops optimally across the public internet.
