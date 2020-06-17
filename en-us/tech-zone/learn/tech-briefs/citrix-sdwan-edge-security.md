---
layout: doc
description: Learn how Citrix SD-WAN Advanced Edition provides Edge Security for your Enterprise network.
---
# Citrix SD-WAN Edge Security

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks** Ravi Pegada

## Introduction

With the continued growth cloud and SaaS, Enterprises are searching for solutions to provide optimal Internet access irrespective of where the endpoint is located. Citrix SD-WAN excels at steering intranet-bound traffic across the WAN or Internet-bound traffic, at the Edge, using business rules for 4.500+ predefined applications.

However, in order to provide direct Internet access Enterprise also need to secure to remote offices to protect the intranet. Direct Internet access exposes remote networks, and subsequently the entire corporate network, to threats. Ingress and egress traffic must be filtered, inspected, and scan to protect against vulnerabilities.  

Citrix SD-WAN has a native [onboard ICSA certified firewall](https://www.citrix.com/blogs/2019/08/13/do-you-trust-your-sd-wan-firewall/), and integrates with leading third party [Secure Web Gateway (SWG)](/en-us/citrix-sd-wan/11-2/internet-service/dia-with-secure-web-gateway.html) vendors via cloud proxy, or through onboard hosted [Virtual Network Function (VNF)](https://www.citrix.com/blogs/2020/05/14/citrix-check-point-software-support-choice-in-protecting-the-wan-edge/). Security partners including:

*  [iBoss](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/iboss-integration.html)
*  [Check Point](/en-us/citrix-sd-wan/11-2/hosted-firewalls/checkpoint-integration-on-1100-platform.html)
*  [Palo Alto](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/palo-alto-integration-by-using-ipsec--tunnels.html)
*  [Force Point](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/firewall-traffic-redirection-support-by-using-forcepoint.html)
*  [Zscaler](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/sd-wan-web-secure-gateway-using-gre-tunnels-and-ipsec-tunnels.html)

Now Citrix has added native onboard Next Generation Firewall – Edge Security functionality to the arsenal of SD-WAN protection options. The release of Citrix SD-WAN version 11.2 Advanced Edition combines full stack security functionality, with its market leading application performance enhancement capabilities, including:

*  [Intrusion Prevention](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
*  [Web Filtering](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
*  [Malware Protection](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)

As a result, Enterprises can now enhance application performance securely from the edge of their network entirely with Citrix. They can confidently consolidate their Branch Office networks with a Citrix SD-WAN appliance that can provide steering to Internet services while filtering, protecting, and centrally monitoring. They also can manage and monitor Edge Security through a single plane of glass with Citrix Cloud hosted Orchestrator service.

## Getting Started

With only a handful of requirements, Citrix SD-WAN customers can quickly get up and running with Advanced Edition - Edge Security:

*  [Citrix Orchestrator](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/security.html#intrusion-prevention)
*  [11.2+ software](/en-us/citrix-sd-wan/11-2.html)
*  [1100 AE appliance](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-sd-wan-data-sheet.pdf)
*  [Advanced Edition (AE) license](/en-us/citrix-sd-wan/11-2.html#citrix-sd-wan-platform-editions)
*  [Firewall Security Profile](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#security-profiles)
*  [Firewall Policy Action](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#edge-security-firewall-policy)

**Network Configuration  – Software Version 11.2.0.88 & 1100 appliance**
![Network Configuration ](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_networkconfiguration.png)
