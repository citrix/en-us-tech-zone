---
layout: doc
description: Understand the design decisions required to implement the Citrix SD-WAN 110 in a Home Office to provide secure, enhanced, and resilient connectivity
---
# Citrix SD-WAN for Home Offices Design Decisions

## Contributors

**Author:** [Shoaib Yusuf](https://twitter.com/Shoaibys)

**Special Thanks** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

## Overview

Citrix SD-WAN for Home Offices may be deployed in a several flexible topologies. It can also provide enhanced delivery of Citrix Virtual Apps and Desktops sessions for Home Users. This design document provides guidance regarding the design decisions to implement Citrix SD-WAN for Home Offices to enable optimimal use by Home Users.

## Network Administration

The Network Administrators for the SD-WAN deployment play a key role in successful rollout of the solution. In designing and maintaining SD-WAN deployments, Administrators are generally concerned with the following:

*  Enabling corporate network access to only those that need it, while adhering to corporate security requirements
*  Rapid streamlined deployment of hundreds or thousands of endpoints geographically distributed across different regions
*  Enabling the highest user experience with company-wide business policies via Quality of Service
*  Stabilizing performance and user experience while accounting for as many disparities as manageable

The success of any deployment hinges on the seamless execution on both a technical and business level.  Further detail and recommendations are provided in this document to help guide the Network Administrator to accomplish a successful SD-WAN deployment.

## Network Security Considerations

Enabling corporate network access to home users opens potential attack vectors, so it is highly recommended to take extra precaution to reduce as much risk as possible. Some of these can include:

1.  Segregate the network by adding barriers, limiting the reach of newly introduced segments, such as Home Users
1.  Eliminate complexity, by limiting the Home User use case to a manageable scope
1.  Control the endpoints by monitoring network connections and eliminating any local device access
1.  Monitor endpoint activity by keeping endpoints on constant surveillance and set up alerts when endpoint behaviors deviate from the norm

## DECISION: Citrix Virtual Apps and Desktops delivery

For optimal security in extending parts of the corporate network to remote resources, it is always recommended to make use of Citrix technologies, such as Citrix Virtual Apps and Desktops. This technology has led in Virtual Client Computing for many years and can help in the design of a secure network. (Here you may find more information regarding [Citrix Virtual Apps and Desktops security](/en-us/citrix-virtual-apps-desktops/1912-ltsr/secure/best-practices.html)).  Citrix SD-WAN can be coupled with this solution to better deliver to remote locations where the Wide Area Network may not always be reliable.

One security conscious approach would be to place SD-WAN in the path of Citrix Workspace traffic delivered through Citrix Gateway. In the Data Center network, the incoming front-end traffic is isolated from back-end traffic, between the CVAD infrastructure servers and services. This allows the use of separate demilitarized zones (DMZs) to isolate front-end and back-end traffic flows along with granular firewall control and monitoring.

[![Citrix Gateway ICA Proxy](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_gatewayicaproxy.png)](design-decisions_citrix-sdwan-home-office_gatewayicaproxy.png)

It is important to understand that in this scenario, the traffic that is delivered through the SD-WAN overlay will be encrypted between the user endpoint and the Citrix Gateway, meaning the SD-WAN will only see encrypted payload. Encrypted traffic means the HDX Auto-QoS feature will not work, along with detailed reporting.  HDX Auto-QoS is designed to dynamically investigate the HDX session when delivered between the SD-WAN peers and allow differentiation between the different channels that comprise a single HDX session.  This is also known as Mult-Stream ICA, which enables prioritization of interactive channels above the bulk data channels, resulting in improved end-user experience. A key component with the integration of Citrix SD-WAN is that that the CVAD servers can keep the session in Single-Stream ICA, which the SD-WAN dynamically delivers Mult-Stream ICA across the Virtual Path / Overlay.

An alternative approach would be to set the Citrix Gateway in “transparent mode”. In transparent mode, the client endpoints can access CVAD resources directly, with no intervening virtual servers on Gateway, thus the HDX traffic is not transmitted as SSL encrypted, allowing the SD-WAN devices to see the HDX protocol and initiate the Auto-QoS capabilities. The Auto-QoS feature on SD-WAN requires usage of CVAD LTSR 7-1912 or higher release.  In this scenario the HDX traffic is still protected as it traverses the WAN, since SD-WAN peers establish AES encrypted tunnels across all available WAN paths between the SD-WAN devices in the network. In addition there is a built-in authentication process with encryption key rotation helping to ensure key regeneration for every Virtual Path at intervals of 10-15 minutes, so that only trusted peers are allowed on the WAN overlay (Here you may find more information regarding [Citrix SD-WAN Security Best Practices](/en-us/citrix-sd-wan/11-1/best-practices/security-best-practices.html))

[![Citrix Gateway Transparent Mode](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_gatewaytransparentmode.png)](design-decisions_citrix-sdwan-home-office_gatewaytransparentmode.png)

In a deployment involving Citrix Virtual Apps and Desktop, CVAD keeps data and workloads secure in the Data Center. Even internet traffic access through those resources (e.g. VDAs) predominately is handled through local Internet links at the Data Center and internet traffic does not actually traverse the new WAN overlay to the remote user. If HDX redirection is enabled, then only in certain scenarios does Internet traffic like Microsoft Teams get broken out of the HDX session for the remote client host to fetch it using local internet sources. Here, the Home User SD-WAN can be enabled for Internet Service to route that traffic appropriately, either directly to Office 365 cloud, or Secure Web Gateway services, or direct to Internet breakout.

### Limit Home User Networks through Firewall DMZ

Another option would be to backhaul Home User traffic and limit resource and connectivity to Corporate Network through Firewalled DMZs. Further, firewall policies on the SD-WAN devices can be centrally mandated to all SD-WAN devices in remote Home User networks to limit the traffic allowed on the “New WAN Overlay” to only certain applications, protocols, and/or Server IPs, etc.

[![Firewall DMZ](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_firewalldmz.png)](design-decisions_citrix-sdwan-home-office_firewalldmz.png)

Selection of the network topology is central to planning the remote access architecture to ensure that it can support the necessary functionality, performance, and security requirements.  The design of the remote access architecture should be completed in collaboration with the security team to ensure adherence to corporate security requirements and standards. In either case, to address the typical pain points on the last mile of the Wide Area Network, the new SD-WAN overlay directly addresses issues typically found with shared internet access links available to most Home Users, making it a must-have solution to provide optimal user experience for happy and productive home workers.

### Segment the Network with Routing Domains (VRF-lite)

Additionally, Citrix SD-WAN allows segmenting the SD-WAN deployment for more security and manageability by using Virtual Routing and Forwarding. This capability on Citrix SD-WAN is called Routing Domains. Considering a network that already has a Citrix SD-WAN deployment, connecting remote offices to data centers, we can introduce a new Home User Routing Domain to separate Home User network traffic from Corporate network traffic. Each routing domain maintains its own unique route table on the SD-WAN devices. A Virtual Path can communicate all routing domains. On ingress of a packet into the tunnel, the packet is tagged based on the routing domain associated with the interface used to enter the system. Within the tunnel, each packet is tagged with its associated routing domain, and upon egress of the tunnel the associated routing table is used for proper delivery. The existing SD-WAN site deployment can leverage the Default Routing Domain, and a the new routing domain can be added to the head-end SD-WAN device leveraging a dedicated interface for limited access to limited Home User resources in the corporate network, as illustrated below.

[![Routing Domains](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_routingdomains.png)](design-decisions_citrix-sdwan-home-office_routingdomains.png)

(Here you may find more information regarding [Citrix SD-WAN Routing Domains](/en-us/citrix-sd-wan/11-1/routing/virtual-routing-and-forwarding.html))

## Home User Network Design Considerations

Rapid deployment of thousands of endpoints is easily accomplished through central management tools specifically designed for large-scale SD-WAN deployments. However, when possible Administrators should eliminate complexity by limiting the Home User use case to a manageable scope. This could mean keeping the initial home user network design to only support one deployment mode on a specific platform. However, the options are numerous and again heavily dependent on the local network availability of the home users. Let us outline some potential options with different local network variances in mind, along with potential benefits and things to consider for each when introducing an SD-WAN device to serve the home worker’s network.

## DECISION: WAN Links

### Single ISP (VPN replacement)

*  Providers:
    *  ISP #1 (only one)
[![Single ISP Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_singleisp.png)](design-decisions_citrix-sdwan-home-office_singleisp.png)

*  Benefits:
    *  Site-to-Site (enable many devices simultaneously connected to SD-WAN Remote Work Network), eliminating the need for multiple Point-to-Site VPN connections
    *  Faster access to corporate resources (local SD-WAN MCN/RCN regional PoPs)
    *  Better security with imbedded ICSA certified firewall for use of local internet breakout
    *  Virtual Path / Overlay benefits:
        *  Better performance with QoS (bidirectional and between protocols)
        *  Application based routing
        *  Loss mitigation (packet duplication/retransmit)
        *  Multiple DC WAN links can still provide redundancy for DC failures
        *  Local link Adaptive Bandwidth Detection
    *  Central management with SD-WAN Orchestrator (Configuration, Reports, Alerts, etc.)
        *  Link monitoring/metrics (useful for SLA compliance)
    *  (optional) DHCP Server for the Remote Work Network

*  Considerations:
    *  No SD-WAN overlay load balancing due to single ISP WAN link
    *  No SD-WAN overlay resiliency due to single ISP WAN link
    *  No local internet load balancing due to single ISP WAN link
    *  No local internet resiliency due to single ISP WAN link

### Dual ISP

*  Providers:
    *  ISP #1
    *  ISP #2
[![Dual ISP Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_dualisp.png)](design-decisions_citrix-sdwan-home-office_dualisp.png)

*  Benefits:
    *  Site-to-Site with higher, aggregated, bandwidth simultaneously using multiple ISP WAN links
    *  SD-WAN overlay load balancing simultaneously using both ISP WAN links
    *  SD-WAN overlay resiliency using both ISP WAN links
    *  Local internet load balancing simultaneously using both ISP WAN links
    *  Local internet resiliency using both ISP WAN links

*  Considerations:
    *  Additional cost of second ISP WAN link
    *  Time and effort to setup ISP #2
    *  Requires additional hardware (Modem for ISP #2)

### ISP + LTE

*  Providers:
    *  ISP #1
    *  LTE #1 (wireless transport used as second WAN link)
[![ISP + LTE Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_isppluslte.png)](design-decisions_citrix-sdwan-home-office_isppluslte.png)

*  Benefits (in addition to benefits of Dual ISP):
    *  Simpler setup of second WAN link
    *  No additional components and cables required (embedded modem)

*  Considerations:
    *  Cost is potentially higher per MB/month than wired ISP option
    *  Limited to contracted monthly bandwidth allocation (going over will be additional cost)
    *  Wireless is generally slower then wired transports

### ISP + LTE (standby)

*  Providers:
    *  ISP #1
    *  LTE #1 (used as second WAN link in Standby mode)
[![ISP + LTE (standby) Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_ispplusltestandby.png)](design-decisions_citrix-sdwan-home-office_ispplusltestandby.png)

*  Benefits (in addition to benefits of Single ISP):
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
[![LTE1 + LTE2 Decision](/en-us/tech-zone/design/media/design-decisions_citrix-sdwan-home-office_lte1pluslte2.png)](design-decisions_citrix-sdwan-home-office_lte1pluslte2.png)

*  Benefits (in addition to benefits of Single ISP):
    *  Cost can be managed with standby features of on-demand and last resort
    *  SD-WAN overlay resiliency using both ISP WAN links
    *  Local internet resiliency using both ISP WAN links

*  Considerations:
    *  No SD-WAN overlay load balancing due to single active WAN link
    *  No local internet load balancing due to single active WAN link

Each use-case described above can be augmented with additional WAN links (ISP or LTE), further increasing the available bandwidth and improving network availability for the remote home worker.

## References

For more information refer to:

[Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html) - learn how to implement a proof of concept of Citrix SD-WAN for a Home Office

[Citrix SD-WAN Home Office Tech Brief](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-home-office.html) - provides an overview of using Citrix SD-WAN for a Home Office
