---
layout: doc
description: Understand the design decisions required to implement the Citrix SD-WAN 110 to work from home with secure, enhanced, and resilient connectivity
---
# Citrix SD-WAN for Home Offices Design Decisions

## Contributors

**Author:** [Shoaib Yusuf](https://twitter.com/Shoaibys)

**Special Thanks** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

## Home User Network Design Considerations

Rapid deployment of thousands of endpoints is easily accomplished through central management tools specifically designed for large-scale SD-WAN deployments. However, when possible Administrators should eliminate complexity by limiting the Home User use case to a manageable scope. This could mean keeping the initial home user network design to only support one deployment mode on a specific platform. However, the options are numerous and again heavily dependent on the local network availability of the home users. Let us outline some potential options with different local network variances in mind, along with potential benefits and things to consider for each when introducing an SD-WAN device to serve the home worker’s network.

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
