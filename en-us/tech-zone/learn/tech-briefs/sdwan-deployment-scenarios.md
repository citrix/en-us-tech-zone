---
layout: doc
h3InToc: true
contributedBy: Daljit Singh, Matthew Brooks
specialThanksTo: Dan Feller, Karthick Srivatsan, Derek Thorslund, Shoaib Yusuf
description: Learn about different scenarios to optimize user experience with Citrix Workspace by deploying Citrix SD-WAN.
tz_title: SD-WAN Deployment Scenarios
tz_products: citrix-networking;
---
# Tech Brief: SD-WAN Deployment Scenarios

## Introduction

This Tech Brief explains how to optimize user experience with Citrix Workspace by deploying Citrix SD-WAN to maximize network connection reliability and provide quality-of-service prioritization of real-time and interactive traffic.

## Overview

Citrix Workspace has been used successfully by enterprises of all sizes for nearly three decades. Customers have choices in how they license, deploy, integrate, and manage these technologies. This flexibility allows Citrix technologies to serve various use cases, business types, integration requirements, and deployment models. Broad enterprise adoption has led to a diverse set of deployments to meet use cases, and the evolution of networking technologies. Many different factors drive deployment selection including location of data centers (on-prem, cloud or hybrid model), users, branches, management service, and choices for networking connectivity.

Enterprises continue to look for ways to improve the application experience for users in geographically dispersed locations. Networking technologies used for connecting to virtual apps, and desktops can make a huge difference in a user’s experience. Existing customers looking to modernize their infrastructure, and new customers deploying Citrix virtualization are seeking better connectivity options.
There are three main factors that customers consider:

*  **Value**: Includes reliability, security, Quality of Service, and more
*  **Cost**: Customers are looking for a better solution, but with a reasonable cost
*  **Ease of deployments**: Simplified deployment, and management of network service

Citrix SD-WAN is a proven, cost-effective solution that has helped companies around the globe improve the user experience, and deliver virtualized desktops, and apps in a secure, and reliable way. SD-WAN gives IT control, and choice of robust connectivity options including Virtual Paths, Cloud Direct Service, Microsoft Azure Virtual WAN, or Direct Internet breakouts. In this brief, we cover how to choose the best way to deploy Citrix SD-WAN to suit your unique network connectivity needs.

## Scenarios

### Deployment 1: Management and resources are both located in Head Offices / Data Centers

Until recently, you'd find that all of the resources (VDAs, AD, and data servers), and managing operations (licensing, StoreFront, and Director) resided in the data center. Citrix technologies have evolved since then, and we recommend that most organizations use or plan to use cloud services when feasible. Although we recommend using the cloud service, customers can choose to continue using the on-premises or hybrid cloud solution.

[![Scenarios 1 - Without SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios1withoutsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios1withoutsdwan.png)

Normally, a branch was connected to the data center through MPLS links. MPLS links are reliable. However, costs are significantly higher than broadband connections, and MPLS links take longer to implement. Citrix SD-WAN helps reduce the cost by allowing you to use broadband alongside or instead of your MPLS while ensuring the reliability, and security of your private WAN. This use case can be implemented rapidly, and it provides many benefits:

*  **Reduced connectivity cost**: Replace, or augment, MPLS with multiple broadband links
*  **Reliability**: Multi-link redundancy with active failover/brownout detection
*  **Increased bandwidth**: Multi-link aggregation with smart load balancing options
*  **Enterprise-grade QoS**: Bi-directional QoS with HDX-specific classification, and Multi-stream ICA QoS
*  **VoIP call protection**: Expedited QoS tagging with real time rerouting to avoid call drops
*  **Extended visibility**: Quality of experience reporting beyond the network layer
*  **Security**: Encrypts all session flows across the SD-WAN overlay network

HDX sessions can receive these benefits when they traverse the SD-WAN overlay network unencrypted by Citrix Gateway. Citrix ensures proper routing using [Beacon Points](/en-us/storefront/1912-ltsr/integrate-with-citrix-gateway-and-citrix-adc/configure-beacon.html).

When a User's Workspace App registers with Enterprise Citrix Virtual Apps and Desktops FQDN StoreFront sends a provisioning file with connectivity guidance. It contains a set of internal, and external URLs to test reachability. If it can reach the internal URLs it directs the Workspace App to authenticate directly to StoreFront. Otherwise, provided it can reach the external URLs, it directs Workspace App to authenticate via Citrix Gateway.

To reach the StoreFront FQDN, the endpoint resolves an internal IP address. It is routed, across the internal network, via the SD-WAN overlay network. Subsequently when the user's launch apps, enumerated by StoreFront, session ICA files include the internal IP address of the VDA/s. Therefore virtual app, and desktop   sessions are also accessed, and delivered across the internal network, via the SD-WAN overlay network.

For more information see the [Citrix Virtual Apps and Desktops - Component Architecture](/en-us/tech-zone/learn/downloads/diagrams-posters_virtual-apps-and-desktops_poster.png)

[![Scenarios 1 - WITH SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios1withsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios1withsdwan.png)

### Deployment 2: Citrix Virtual Apps and Desktops Service with resources on-premises

As Citrix customers move to the cloud, their first step is often to use the Citrix Virtual Apps and Desktops service to manage their virtual app, and desktop workloads hosted at their on-premises locations. Citrix takes over maintaining the control systems, while customers can continue to manage their environments through Citrix Cloud.

[![Scenarios 2 - Without SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios2withoutsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios2withoutsdwan.png)

Although, in this scenario, while managing operations through the Citrix Cloud is simplified, network connectivity may not be. Session traffic is directed to the nearest Citrix Gateway service PoP. From there, it is proxied to the resource locations. While there are many PoP locations around the world, for endpoints in Remote Branches on the WAN, this step can add latency.

To address this scenario Citrix introduced [Direct Workload Connection](/en-us/citrix-workspace/workspace-network-location.html) via a new Network Location service (NLS). It enables session flows from Remote Branch hosted endpoints to access VDAs directly across the WAN. It identifies endpoints located on the Internal Network IP address ranges, and provides them with connection information to access VDAs directly on the Internal Network.

However, NLS requires branch public IP address ranges to be preconfigured. Branches that use DSL or Cable Modems use dynamic IPs that can change frequently. Home Offices can have the same issue, and can number in the tens of thousands making it too difficult to track, and update manually.

Citrix SD-WAN offers a solution to these issues, and provides other benefits in this scenario including:

*  **Reducing latency through NLS** and resolving issues where dynamic IP addresses are used by detecting endpoint public IP addresses and updating the NLS automatically
*  **Ability to replace high-cost MPLS** links to Head Offices / Data Centers
*  **Providing deep visibility into HDX traffic** allowing IT to monitor User Experience, and proactively act in the event there is degradation
*  **Citrix Cloud and Gateway Service internet breakout policy** allowing First Packet Classification, facilitating direct internet breakout from the SD-WAN branch, which reduces latency for endpoints that are unable to access StoreFront and VDAs directly over the Internal Network

[![Scenarios 2 - With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios2withsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios2withsdwan.png)

### Deployment 3: Citrix Virtual Apps, and Desktops Service with VDAs in the cloud, and other resources on-premises

In this deployment, Citrix provides a service to manage resources located both in the cloud and on-prem locations. The adoption rate of this deployment is high due to benefits including reduced complexity, and cost. Nonetheless, the complexity around network connectivity continues to provide some challenges. Citrix SD-WAN offers a simple, cost-effective solution. The figure below is a typical topology without Citrix SD-WAN where the VDAs are in public clouds, but the data servers and the Active Directory servers are located at the on-premises location.

[![Scenarios 3 - Without SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios3withoutsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios3withoutsdwan.png)

Citrix SD-WAN offers two solutions to the scenario above:

#### Public cloud to data centers

It takes time, and costs to deploy Cloud platform connectivity options such as Microsoft Azure ExpressRoute, and AWS Direct Connect. Most customers are looking for quick, and inexpensive connectivity from HQ data centers to public clouds. When using SD-WAN, these benefits can be achieved in a few hours, rather than months in many cases. SD-WAN management operations provide auto provisioning of SD-WAN in popular clouds such as Azure.

#### Branch to VDAs in public clouds

The second scenario in this deployment is the connectivity between branches, and VDAs in the cloud. The normal path for a branch user is through Citrix Cloud using the Citrix Gateway service, which is connected to VDAs through the Citrix connector. Usually, Citrix Cloud, and VDAs exist in different locations, and different clouds. The transit distance, and forwarding delays, across the best effort Internet, can add latency and disrupt user experience. Citrix SD-WAN, along with Direct Workload Connection, can improve the user experience by reducing latency, and by reducing the egress bandwidth cost since traffic is traversing out of one cloud.

[![Scenarios 3 - With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios3withsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios3withsdwan.png)

### Deployment 4: Citrix Virtual Apps and Desktops Standard for Azure

Citrix Virtual Apps and Desktops Standard for Azure (DaaS offering) makes it easy to deliver secure Windows apps, and desktops from Azure. Both management operation systems and VDA resources are hosted in Azure, and managed by Citrix. The following figure is a typical topology with connectivity from branch users.

[![Scenarios 4 - Without SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios4withoutsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios4withoutsdwan.png)

SD-WAN technology is now integrated in the Citrix Virtual Apps and Desktops Standard for Azure to simplify network connectivity from your cloud-hosted desktops to on-premises resources such as Active Directory, and data sources.

For Branches the integrated SD-WAN solution significantly cuts down the network connectivity cost, and enhances the user experience. Clients connect to the Citrix Virtual Apps and Desktops Standard for Azure over a secure virtual path, via available links, with HDX session traffic prioritized by ICA class for optimal user experience.

[![Scenarios 4 - With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios4withsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios4withsdwan.png)

### Deployment 5: Citrix SD-WAN Cloud Direct Service for Citrix Virtual Apps and Desktops Standard for Azure and Citrix Virtual Apps and Desktops Service use cases

SD-WAN Cloud Direct is a service that provides many of the benefits of SD-WAN virtual paths, without the need to install SD-WAN in the cloud. It is a good alternative for customers who are unable to manage their own virtual SD-WAN instances in the cloud. Cloud Direct supports connecting to virtual apps and desktops in the cloud, including Citrix Virtual Apps and Desktops, Citrix Virtual Apps and Desktops Standard for Azure and Microsoft Azure Virtual Desktops. Complementing Cloud Direct, Citrix SD-WAN also supports connectivity from virtual apps and desktop host virtual machines to on-prem resources (For example databases, back-end servers).

Citrix has multiple Cloud Direct service PoPs around the world that are peered with most public cloud, and SaaS providers. Customers providing access to SaaS applications, Citrix Virtual Apps and Desktops Standard for Azure / Citrix Virtual Apps and Desktops service / Azure Virtual Desktops, and other cloud providers can use this service to improve application response times. It is especially valuable for organizations accessing applications hosted by third parties. Most cloud providers charge for data leaving the cloud. So, putting SD-WAN in the cloud can add extra cost to the customer. These network bandwidth charges can be avoided using the Cloud Direct service. Here are additional benefits of using the Cloud Direct service:

*  **Improved SaaS performance**: Cloud Direct service can identify your most critical application traffic, and ensure that it’s always prioritized
*  **VoIP, and UCaaS protection**: Provide a consistent VoIP, and UCaaS connection regardless of outages, lag, packet loss or jitter
*  **VPN enhancements**: Keep your site-to-site VPN connections, and performance stable, with hitless failover, over best effort internet, without the cost, and hassle of MPLS
*  **Reliability**: Redundancy with active failover/brownout detection
*  **Increased bandwidth**: Via link aggregation/smart load-balancing
*  **Enterprise-grade QoS**: Bi-directional QoS/app classification
*  **Extended visibility**: Benefit reporting beyond the edge

**Citrix Virtual Apps and Desktops Standard for Azure**, being hosted in the cloud, enjoys those same benefits as SaaS applications. Here’s how the Cloud Direct service is used for connecting to virtual desktops.

[![Scenarios 5 - Azure With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios5azurewithsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios5azurewithsdwan.png)

Getting better experience to **Citrix Virtual Apps and Desktops service** through the Citrix Cloud is no different.

[![Scenarios 5 - Citrix Virtual Apps, and Desktops Service With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios5cvadswithsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios5cvadswithsdwan.png)

#### Solution for Remote Worker

In the wake of recent events, many remote users experiencing poor call, and video conferencing quality in their home offices. Accessing critical SaaS applications, including Unified Communications as a Service, and Citrix virtual applications, and desktops with quality performance, and experience is of the utmost importance. Most home users have a single broadband Internet connection. Citrix SD-WAN Cloud Direct solves some of the most common home networking challenges by using QoS to prioritize business apps. Citrix SD-WAN Cloud Direct has a built-in capability to identify, and prioritize traffic to Citrix Virtual Apps and Desktops service for Azure and Citrix Virtual Apps and Desktops service. This enables it to significantly improve the user experience. Therefore, by implementing a Citrix SD-WAN appliance in their home office Enterprises can improve Remote Worker’s user experience.

For more information see [Citrix SD-WAN Cloud Direct service overview](/en-us/tech-zone/learn/tech-briefs/sdwan-cloud-direct.html)

### Deployment 6: Citrix SD-WAN for Microsoft Azure Virtual WAN

Citrix is one of the preferred Microsoft partners for connecting to Azure Virtual WAN. Citrix SD-WAN has an integrated, and simplified deployment for connecting to the Azure network. If customers are already using Microsoft Azure Virtual WAN for connecting to their workloads, Citrix SD-WAN can help optimize connectivity from the branches. It provides resilient tunnels from the branch to Hubs in Azure Regions. Again, providing reliable connectivity without the need to install the SD-WAN in the cloud. Integration between Citrix SD-WAN and Azure Virtual WAN provides the following benefits:

*  **Deploy large-scale branch connectivity** between branch locations, and Azure
*  **Automated provisioning**, and configuration
*  **Scalability**, and high throughput
*  **Leverage Microsoft’s global backbone network** for branch to hub (in all Azure regions) communication.
*  **Active-Active redundant IPSec tunnels** for failover to Azure vWAN using BGP for faster convergence and scalable management of network prefix changes

[![Scenarios 6 - With SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios6withsdwan.png)](/en-us/tech-zone/learn/media/tech-briefs_sdwan-deployment-scenarios_scenarios6withsdwan.png)

## Summary

Citrix Workspace customers have diverse network connectivity requirements due to diverse deployment scenarios. Citrix SD-WAN offers many connectivity options to meet your unique requirements. It provides simplified, secure, reliable connectivity to Citrix virtual applications and desktop for low cost. No matter how complex the deployments for Citrix Virtual Apps and Desktops service, Citrix Virtual Apps and Desktops Standard for Azure, and Citrix Virtual Apps and Desktops on-premises Citrix SD-WAN has a solution for your network connectivity needs.
Visit Citrix.com to learn more about the Citrix Workspace, and Citrix SD-WAN solutions.
