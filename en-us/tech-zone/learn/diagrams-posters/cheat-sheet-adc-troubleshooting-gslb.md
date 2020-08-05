---
layout: doc
description: One-page summary of GSLB, MEP protocol and troubleshooting tips.
---
# Troubleshooting Citrix GSLB MEP Cheat Sheet

## Contributors

**Author:** [Gene Whitaker](mailto:gene.whitaker@citrix.com)

**Special thanks:** Adrianna Pellitteri

## Overview

Citrix ADC appliances configured for global server load balancing (GSLB) provide for disaster recovery and ensure continuous availability of applications by protecting against points of failure in a wide area network (WAN). GSLB can balance the load across data centers by directing client requests to the closest or best performing data center, or to surviving data centers if there is an outage.

*  GSLB Metric Exchange Protocol (MEP) is a Citrix proprietary protocol used by Citrix ADC to exchange site and network metrics, persistence data, and other information to other sites participating in GSLB
*  Communication process is accomplished between each GSLB site on TCP port 3011 (or 3009 for secure communication).
*  MEP communication is initiated from the lower site IP address to the higher site address for site metrics exchange and higher to lower for network metrics.

The Citrix ADC GSLB and sync cheat sheet provides you with the most commonly used resources to troubleshoot Citrix ADC GSLB and sync issues.

Use the following link to [download Troubleshooting Citrix GSLB MEP Cheat Sheet](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-gslb.pdf).

[![Cheat Sheet](/en-us/tech-zone/learn/media/diagrams-posters_cheat-sheet-adc-troubleshooting-gslb_1.png)](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-gslb.pdf)

## Other Tech Zone content

[Learn -> Tech Brief -> Intelligent Traffic Management](/en-us/tech-zone/learn/tech-briefs/itm.html) - Citrix Intelligent Traffic Management (ITM) service brings “intelligence” to DNS by incorporating a broad set of capabilities to determine the best query response based on analysis of real time conditions.

[Learn -> Tech Brief -> SD-WAN Cloud Direct service](/en-us/tech-zone/learn/tech-briefs/sdwan-cloud-direct.html) - Optimize SaaS access for branch users by tunneling session traffic to Internet Exchanges with direct connectivity to popular sites.

[Learn -> PoC Guides -> Citrix Web Application Firewall Deployment Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/citrix-waf-deployment.html) - Learn how to deploy Citrix Web Application Firewall (WAF) standalone or as a part of a Citrix ADC deployment. Protect web servers or applications from various attacks including Cross Site Scripting, SQL Injection, Buffer Overflow, Forceful Browsing and more. Deploy in any public cloud or your on-premises environment.

[Learn -> Tech Brief -> Gateway service for HDX Proxy](/en-us/tech-zone/learn/tech-briefs/gateway-hdxproxy.html) - Provides users with secure remote access to Citrix Virtual Apps and Desktops without having to deploy Citrix Gateway in the on-premises DMZ or reconfigure firewalls.

[Learn -> Tech Brief -> SD-WAN for Workspace](/en-us/tech-zone/learn/tech-briefs/sdwan-workspace.html) - Provides optimal network connectivity between Enterprise branch offices and their Workspace hosted in data resource locations on-premises or in the cloud.

[Learn -> Tech Insight -> Office 365 Optimization for Branch Offices](/en-us/tech-zone/learn/tech-insights/office365-optimization.html) - Learn how Citrix SD-WAN implements Microsoft Connectivity Principles to support Office 365 Optimization for Branch Offices.

[Learn -> Tech Insight -> SD-WAN](/en-us/tech-zone/learn/tech-insights/sdwan.html) - Optimize delivery of Citrix Virtual Apps and Desktops traffic with Citrix SD-WAN.

[Learn -> Tech Insight -> AlwaysOn VPN](/en-us/tech-zone/learn/tech-insights/citrix-gateway-alwayson.html) - Manage remote domain joined Windows endpoints 24x7 by providing LAN-like access with AlwaysOn VPN.

[Learn -> Tech Insight -> Intelligent Traffic Management - Radar](/en-us/tech-zone/learn/tech-insights/itm.html) - Learn how Citrix ITM can direct users to cloud service mirror sites with the most bandwidth available between them.

[Learn -> Tech Insight -> YouTube Optimization for Branch Offices](/en-us/tech-zone/learn/tech-insights/youtube-optimizations.html) - Optimize YouTube delivery in branch offices with Citrix Virtual Apps and Desktops and Citrix SD-WAN.

[Learn -> Diagrams and Posters -> Citrix ADC - nFactor Basics Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-nfactor.html) - One-page summary of nFactor authentication detailing concepts, how it works, nFactor Visualizer information, configuration steps, and more.

[Learn -> Diagrams and Posters -> Citrix ADC - File System and Process Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-file-system-process.html) - One-page summary of most common system directories, files, processes/daemons and logs.

[Learn -> Diagrams and Posters -> Citrix ADM - Overview Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adm.html) - One-page summary of the ADM Platform detailing system requirements, deployment modes, protocols and ports, common log files, common issues/failures, and more.

[Learn -> Diagrams and Posters -> Citrix ADC - Troubleshooting High Availability Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-troubleshooting-high-availability.html) - One-page summary of high availability and troubleshooting tips.

[Learn -> Diagrams and Posters -> Citrix ADC - SDX Basics and Log File Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-sdx-basics.html) - One-page summary of SDX components and how to access them, common SVM ports, LOM configuration, Link Aggregation on the SDX, and Common Log files for both SVM and Citrix Hypervisor.

[Learn -> Diagrams and Posters -> Citrix ADC - nsconmsg Commands Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-nsconmsg.html) - One-page summary of nsconmsg syntax and troubleshooting tips.

[Design -> Reference Architectures -> Application Delivery Management](/en-us/tech-zone/design/reference-architectures/citrix-adm.html) - See how the Citrix Application Delivery Management software is deployed to simplify management and monitoring of your application delivery infrastructure.

[Design -> Reference Architectures -> SD-WAN Multi-Region](/en-us/tech-zone/design/reference-architectures/sd-wan-multi-region.html) - Discover the framework, design, and architecture for Citrix SD-WAN multi-region deployment with SD-WAN Orchestrator.

[Design -> Reference Architectures -> SD-WAN](/en-us/tech-zone/design/reference-architectures/sdwan.html) - Learn about the framework, design, and architecture for Citrix SD-WAN with SD-WAN Orchestrator for single region deployment.

[Design -> Reference Architectures -> SD-WAN for Content Collaboration](/en-us/tech-zone/design/reference-architectures/sdwan-content-collaboration.html) - Learn about the deployment architecture and how Citrix SD-WAN WANOP helps to optimize Citrix Content Collaboration for customer-managed storage zones including relevant test data.
