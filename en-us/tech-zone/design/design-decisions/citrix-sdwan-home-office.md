---
layout: doc
h3InToc: true
contributedBy: Shoaib Yusuf
specialThanksTo: Matthew Brooks
description: Understand the design decisions required to implement the Citrix SD-WAN 110 in a Home Office to provide secure, enhanced, and resilient connectivity.
tz_title: Citrix SD-WAN for Home Offices
tz_products: citrix-networking;
---
# Design Decision: Citrix SD-WAN for Home Offices

## Overview

Citrix SD-WAN for Home Offices can be deployed in several flexible topologies. It can also provide enhanced delivery of Citrix Virtual Apps and Desktops sessions for home workers. This design document provides guidance regarding the design decisions to implement Citrix SD-WAN for Home Offices to enable optimal use by home workers.

## Network Administration

The Network Administrators for the SD-WAN deployment play a key role in a successful rollout of the solution. In designing and maintaining SD-WAN deployments, Administrators are concerned with the following:

*  Enabling corporate network access to only those users that need it, while adhering to corporate security requirements
*  Rapid streamlined deployment of hundreds or thousands of endpoints geographically distributed across different regions
*  Enabling the highest user experience with company-wide business policies via Quality of Service
*  Stabilizing performance and user experience while accounting for as many disparities as manageable

The success of any deployment hinges on the seamless execution on both a technical and business level. Further detail and recommendations are provided in this document to help guide the Network Administrator to accomplish a successful SD-WAN deployment.

## Network Security Considerations

Enabling corporate network access to home offices opens potential attack vectors, so it is highly recommended to take extra precaution to reduce as much risk as possible. Some of these steps can include:

1.  Segregate the network by adding barriers, limiting the reach of newly introduced segments, such as home workers
1.  Eliminate complexity, by limiting the Home Office use case to a manageable scope
1.  Control the endpoints by monitoring network connections and eliminating any local device access
1.  Monitor endpoint activity by keeping endpoints on constant surveillance and set up alerts when endpoint behaviors deviate from the norm
1.  Augment the SD-WAN solution with security protection with cloud services (for instance Citrix Secure Internet Access, Zscaler, or Palo Alto Prisma)
1.  Utilize higher-end SD-WAN Advanced Edition appliances to unlock an integrated security stack or leverage third-party firewalls (for instance Palo Alto, Checkpoint) as Virtual Network Functions (VNF) with simple integration with industry-leading security vendors

Before we dive into detail on security considerations for the network, we first outline a design decision that must be made if the Home Office traffic profile includes Citrix Virtual Apps and Desktops.

## DECISION: Citrix Virtual Apps and Desktops delivery

For optimal security in extending the corporate network to remote resources, it is always recommended to use Citrix technologies, such as Citrix Virtual Apps and Desktops. This technology has led in Virtual Client Computing for many years and can help in the design of a secure network. Here you can find more information regarding [Citrix Virtual Apps and Desktops security](/en-us/citrix-virtual-apps-desktops/1912-ltsr/secure/best-practices.html). Citrix SD-WAN can be coupled with this solution to better deliver to remote locations where the WAN may not always be reliable.

Users remotely connect to Citrix Virtual Apps and Desktops on any device, from any location. In the Data Center network, for security, the incoming front-end traffic is isolated from back-end traffic, between the Citrix Virtual Apps and Desktops infrastructure servers and services. This approach allows the use of separate demilitarized zones (DMZs) to isolate front-end and back-end traffic flows along with granular firewall control and monitoring.

![Citrix Gateway ICA Proxy](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_gatewayicaproxydmz.png)

One approach would be to place a book ended SD-WAN solution directly in the path the Citrix Virtual Apps and Desktops traffic. In this deployment, SD-WAN devices can provide features such as link aggregation, subsecond resiliency, and QoS to prioritize the encrypted HDX traffic over other traffic types.

![Citrix Gateway ICA Proxy](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_gatewayicaproxy.png)

However, it is important to understand that in this scenario the SD-WAN appliances only sees the encrypted payload. The traffic that is delivered across the SD-WAN overlay is encrypted between the user endpoint and the Citrix Gateway. Encrypted traffic means the HDX Auto-QoS feature of SD-WAN does not work, along with SD-WAN Quality of Experience (QoE) reporting for the HDX traffic. HDX Auto-QoS is designed to dynamically investigate the HDX session when delivered between the SD-WAN peers. It allows differentiation between the different channels that comprise a single HDX session. This feature is also known as Multi-Stream ICA, which enables prioritization of interactive channels above the bulk data channels, resulting in improved end-user experience. A key benefit with the integration of Citrix SD-WAN is that the Citrix Virtual Apps and Desktops servers can keep the session in Single-Stream ICA whereas SD-WAN dynamically delivers Multi-Stream ICA across the Virtual Path / Overlay.

The recommended approach would be to provide users coming in from Home Offices with SD-WAN with a different access method than typical external users coming in on the internet.

![StoreFront](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_icastorefront.png)

Home Worker client endpoints that are connecting in through the SD-WAN overlay needs to be considered secure and allowed access to Citrix Virtual Apps and Desktops resources directly, for instance an “internal” StoreFront URL, with no intervening on Citrix Gateway. In this method, the HDX traffic is not transmitted as SSL encrypted, allowing the SD-WAN devices to see the HDX protocol using port 2598 and initiate the Auto-QoS capabilities. The Auto-QoS feature on SD-WAN requires usage of Citrix Virtual Apps and Desktops LTSR 7-1912 or higher release. In this scenario the HDX traffic is still protected as it traverses the WAN, since SD-WAN peers establish AES encrypted tunnels across all available WAN paths between the SD-WAN devices in the network. In addition, there is a built-in authentication process with encryption key rotation which helps ensure key regeneration for every Virtual Path, at intervals of 10-15 minutes. Therefore, only trusted peers are allowed on the WAN overlay. Here you can find more information regarding [Citrix SD-WAN Security Best Practices](/en-us/citrix-sd-wan/11-1/best-practices/security-best-practices.html)

Another approach to bypass encryption by Gateway, after authentication, so that the SD-WAN sees unencrypted HDX is to use Beacons. Beacons are URLs that the Citrix Workspace app (CWA) attempts to contact to determine if the client host machine is on an internal network or external network. For instance a Branch Site, Home Offices with an SD-WAN 110 or a Coffee Shop or Airport respectively. If CWA cannot contact the internal beacon point and receives response for external beacon points, this means that the user device is outside the internal network and must connect to desktops and applications through Citrix Gateway, and the HDX traffic must be encrypted. SD-WAN Home Office networks need to be considered internal networks, and bypass Citrix Gateway to provide unencrypted HDX traffic. Here you can find more information regarding [Beacon Configuration](/en-us/storefront/1912-ltsr/integrate-with-citrix-gateway-and-citrix-adc/configure-beacon.html).

In a deployment involving Citrix Virtual Apps and Desktops, data and workloads are kept secure in the Data Center. Even internet traffic access is handled through local Internet links at the Data Center. Internet traffic does not actually traverse the new WAN overlay to the remote user. If HDX redirection is enabled for supported client hosts, then only in certain scenarios does Internet traffic like Microsoft Teams get broken out of the HDX session for the remote client host to fetch it using local internet sources. Here, the Home Office SD-WAN can be enabled for Internet Service to route that traffic appropriately. It can be routed directly to Office 365 cloud through Internet breakout.

### Limit Home Office Networks through Firewall DMZ

One network security design consideration would be to backhaul Home Office traffic and limit resource and connectivity to the Corporate Network through Firewalled DMZs. Further, firewall policies on the SD-WAN devices can be centrally pushed to all SD-WAN devices in remote Home Office networks. These policies limit the traffic allowed on the “New WAN Overlay” to only certain applications, protocols, and or Server IPs, and so on.

![Firewall DMZ](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_firewalldmz.png)

Selection of the network topology is central to planning the remote access architecture to ensure that it can support the necessary functionality, performance, and security requirements. The design of the remote access architecture needs to be completed in collaboration with the security team to ensure adherence to corporate security requirements and standards. In either case, to address the typical pain points on the last mile of the WAN, the new SD-WAN overlay directly addresses issues typically found with shared internet access links available to most home workers. This benefit makes it a must-have solution to provide optimal user experience for happy and productive home workers.

### Segment the Network with Routing Domains (VRF-lite)

Citrix SD-WAN allows segmenting the SD-WAN deployment for more security and manageability by using Virtual Routing and Forwarding. This capability on Citrix SD-WAN is called Routing Domains. An existing Citrix SD-WAN deployment, connecting remote offices to data centers, can introduce a new Home Office Routing Domain to separate Home Office network traffic from Corporate network traffic. Each routing domain maintains its own unique route table on the SD-WAN devices. A Virtual Path can communicate all routing domains. On ingress of a packet into the tunnel, the packet is tagged based on the routing domain associated with the interface used to enter the system. Within the tunnel, each packet is tagged with its associated routing domain, and upon egress of the tunnel the associated routing table is used for proper delivery. The existing SD-WAN site deployment can use the Default Routing Domain. The new routing domain can be added to the head-end SD-WAN device leveraging a dedicated interface. This domain can provide access to limited Home Office resources in the corporate network, as illustrated following.

![Routing Domains](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_routingdomains.png)

Here you can find more information regarding [Citrix SD-WAN Routing Domains](/en-us/citrix-sd-wan/11-1/routing/virtual-routing-and-forwarding.html)

## Home Office Network Design Considerations

Rapid deployment of thousands of endpoints is easily accomplished through central management tools designed for large-scale SD-WAN deployments. However, when possible Administrators need to eliminate complexity by limiting the Home User use case to a manageable scope. For example by keeping the initial home office network design to only support one deployment mode on a specific platform. However, the options are numerous and again heavily dependent on the local network availability of the home offices. Following we outline some potential options with different local network variances in mind. We also discuss potential benefits and things to consider for each when introducing an SD-WAN device to serve the home office network.

## DECISION: WAN Links

### Single ISP (VPN replacement)

*  Providers:
    *  ISP #1 (only one)
![Single ISP Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_singleisp.png)

*  Benefits:
    *  Site-to-Site (enable many devices simultaneously connected to the SD-WAN Remote Work Network), eliminating the need for multiple Point-to-Site VPN connections
    *  Faster access to corporate resources (local SD-WAN MCN/RCN regional PoPs)
    *  Better security with imbedded enterprise-grade firewall for use of local internet breakout
    *  Virtual Path / Overlay benefits:
        *  Better performance with QoS (bidirectional and between protocols)
        *  Application based routing
        *  Loss mitigation (packet duplication/retransmit)
        *  Multiple DC WAN links can still provide redundancy for DC failures
        *  Local link Adaptive Bandwidth Detection
        *  Central management with SD-WAN Orchestrator (Configuration, Reports, Alerts, and so on.)
        *  Link monitoring/metrics (useful for SLA compliance)
        *  (optional) DHCP Server for the Remote Work Network
        *  (optional) Citrix Secure Internet Access for internet-bound traffic

*  Considerations:
    *  No SD-WAN overlay load balancing due to single ISP WAN link
    *  No SD-WAN overlay resiliency due to single ISP WAN link
    *  No local internet load balancing due to single ISP WAN link
    *  No local internet resiliency due to single ISP WAN link

### Dual ISP

*  Providers:
    *  ISP#1
    *  ISP#2
![Dual ISP Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_dualisp.png)

*  Benefits (In addition, to benefits of Single ISP):
    *  Site-to-Site with higher, aggregated, bandwidth simultaneously using multiple ISP WAN links
    *  SD-WAN overlay load balancing simultaneously using both ISP WAN links
    *  SD-WAN overlay resiliency using both ISP WAN links
    *  Local internet load balancing simultaneously using both ISP WAN links
    *  Local internet resiliency using both ISP WAN links
    *  (optional) resiliency to Citrix Secure Internet Access for internet-bound traffic

*  Considerations:
    *  Extra cost of second ISP WAN link
    *  Time and effort to set up ISP #2
    *  Requires extra hardware (Modem for ISP #2)

### ISP + LTE

*  Providers:
    *  ISP #1
    *  LTE #1 (wireless transport used as second WAN link)
![ISP + LTE Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_isppluslte.png)

*  Benefits (In addition, to benefits of Dual ISP):
    *  Simpler setup of second WAN link
    *  No extra components and cables required (embedded modem)

*  Considerations:
    *  Cost is potentially higher per MB/month than wired ISP option
    *  Limited to contracted monthly bandwidth allocation (going over is an extra cost)
    *  Wireless is slower than wired transports

### ISP + LTE (standby)

*  Providers:
    *  ISP #1
    *  LTE #1 (used as second WAN link in Standby mode)
![ISP + LTE (standby) Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_ispplusltestandby.png)

*  Benefits (In addition, to benefits of Single ISP):
    *  Cost can be managed with standby features of on-demand and last resort
    *  SD-WAN overlay resiliency using both ISP WAN links
    *  Local internet resiliency using both ISP WAN links

*  Considerations:
    *  No SD-WAN overlay load balancing due to single active WAN link
    *  No local internet load balancing due to single active WAN link

### Dual LTE

*  Providers:
    *  LTE #1
    *  LTE #2
![LTE1 + LTE2 Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_lte1pluslte2.png)

*  Benefits (In addition, to benefits of Dual ISP):
    *  Simpler setup of second WAN link
    *  No additional components and cables required (embedded modem), besides USB LTE dongle

*  Considerations:
    *  Cost is potentially higher per MB/month than wired ISP option
    *  Limited to contracted monthly bandwidth allocation (going over is extra cost)
    *  Wireless is slower than wired transports

Each use-case described preceding can be augmented with extra WAN links (ISP or LTE), further increasing the available bandwidth and improving network availability for the remote home worker.

## References

For more information refer to:

[Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html) - learn how to implement a proof of concept of Citrix SD-WAN for a Home Office

[Citrix SD-WAN Home Office Tech Brief](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-home-office.html) - provides an overview of using Citrix SD-WAN for a Home Office
