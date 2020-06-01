---
layout: doc
description: Learn how to work from home with secure, enhanced, and resilient connectivity using the Citrix SD-WAN 110
---
# Citrix SD-WAN for Home Offices

## Contributors

**Author:** [Shoaib Yusuf](https://twitter.com/Shoaibys)

## Introduction

Businesses can now leverage the Citrix SD-WAN solution for Home Office Users. With it SD-WAN Administrators can quickly extend reliable and secure workplace environments to any location.

## Challenges with Teleworking

Remote users generally encounter common challenges that impact productivity, which include:

*  Poor end-user experience, due to low quality internet links and consumer grade hardware
*  Unreliable connectivity due to geographically distributed remote locations
*  Shared Internet Access (SIA) that cannot differentiate between business and recreational traffic

IT Administrators struggle with the following challenges:

*  Scaling the rollout and onboarding of remote workers in a timely manner
*  Managing a large deployment becomes difficult and expensive
*  Securing the environment by protecting users, data, applications, and the network

In this Tech Brief we will discuss how the Citrix SD-WAN solution is capable of addressing these challenges.

## Citrix Enables Work from Anywhere

Citrix solutions enable business continuity. As the definition of workforce changes, employee needs evolve. Citrix provides solutions that extend the digital workspace to end-users across any device, on any network. A critical component of that business plan is to ensure that users remain productive while maintaining the necessary level of security and control over user access to corporate resources. Citrix Workspace, including virtual apps and desktops, enables seamless workforce productivity, giving employees the flexibility to work from anywhere, all while keeping your apps and information secure. Sometime, due to uncontrollable network conditions, end-user experience cannot always be guaranteed.

Traditional remote branch offices are an extension of the Data Center network, connected over Private Intranet links (e.g. MPLS). In the use-case of remote branch offices, Private Intranet circuits are generally reliable and secure. However, depending on the location of the branch office, ensuring availability and the cost of maintaining a high bandwidth WAN becomes challenging, which can negatively impact user-experience.

[![Branch MPLS Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_BranchMplsTopology.png)](tech-briefs_citrix-sdwan-home-office_BranchMplsTopology.png)

Home Users / Teleworkers traditionally connect into the network over Public Internet links. Like a traditional VPN solution, one option would be to utilize Citrix Gateway to provide users with secure remote access to Citrix Virtual Apps and Desktops across a range of devices. In a VPN use-case, Public Internet links are generally unreliable and of poor quality, again depending on the geographic location of the teleworker and the conditions of the last mile. Ensuring a high end-user experience across a questionable Wide Area Network (WAN) becomes a challenge as teleworkers and home users are introduced to the workforce.

[![Home Office Internet Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_HomeOfficeInternetTopology.png)](tech-briefs_citrix-sdwan-home-office_HomeOfficeInternetTopology.png)

## Introducing SD-WAN 110-LTE-WiFi-SE

Citrix SD-WAN 110 platform addresses the needs of small offices, retail stores, and work-from-home users. The 110 platform ensures business continuity in a small form factor providing the highest network resiliency with sub-second failover, along with the best in-class application experience for virtual, SaaS, and cloud applications providing end-to-end Quality of Service (QoS).

[**SD-WAN 110-LTE-WiFi-SE Overview**]( https://www.youtube.com/watch?v=DFCeF68NXM0&feature=youtu.be )

SD-WAN 110-LTE-WIFI-SE Product Specs include:

*  Four 10/100/1000 Ethernet Ports for WAN or LAN (interface 1/4 as a data port available with R11.1.1)
*  Integrated LTE with dual SIM slots (a single modem allowing the SIM to be used as active / passive)
*  USB based LTE dongle support (available with R11.1.1)
*  Integrated WiFi Access Point capability (*WiFi Ready)

*Starting 2Q 2020, 110-LTE-WiFi will be shipped as “WiFi Ready.” The WiFi feature will be ready to use in 4Q’20 through a software upgrade.

Citrix SD-WAN is a bookended solution providing reliable and high bandwidth connectivity between site locations, directly addressing potential limitations of Wide Area Network as indicated with the use-cases in the section above.

## Citrix SD-WAN for Home Offices Overview

Concerns regarding the WAN for the Home User use-case can be solved with the Citrix SD-WAN solution.  The bookended solution can strategically be placed on the segment of the network that typically impacts user experience the most, providing a new WAN overlay, designed to increase end-user experience and performance.

[![Citrix SD-WAN Home Office Use Case](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCase.png)](tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCase.png)

Generally, you will find more benefits with the SD-WAN solution by introducing additional WAN transports such as DLS, Broadband Internet and/or LTE links to augment any existing WAN transports. Diversifying the Internet Service providers can significantly increase network reliability. In the above scenario, SD-WAN devices (physical or virtual) can be placed directly in the path of existing network with additional WAN transports to increase user experience and productivity. The benefits include:

*  link bandwidth aggregation (a.k.a. link load balancing) for remote locations with multiple Internet/LTE links
*  sub-second link blackout and brownout detection, enabling an always-on network
*  per-packet prioritization providing dual-ended Quality of Service with ability to deliver asymmetrically across all available paths
*  augment security protection with cloud services (e.g. Zscaler, Palo Alto Prisma, and iBoss) or by utilizing higher-end appliance unlock additional on-prem VNF options (e.g. Palo Alto, Checkpoint) with simple integration with industry-leading security vendors
*  serve as a DHCP Server to allocate and fix IP address for LAN devices based on MAC address
*  enable usage of LTE-based WAN with standby (last resort and on-demand) functionality to further increase network reliability across wireless transports
*  prioritize business critical applications utilizing a Deep Packet Inspection (DPI) and a Quality of Service engine, and with an integrated firewall solution, the ability to route and enforce security policies to block undesired apps or allow direct internet breakout for low latency connectivity to trusted SaaS apps (e.g. Office 365)

The availability of these benefits may vary based on the available WAN transports and the design of the site. The Network Administrator for the SD-WAN deployment plays a key role in SD-WAN deployments. The Network Administrator centrally dictates what is business critical and the extent of connectivity for the Home User use-case. We’ll dive into more detail on the roles and responsibilities of an Administrator in [Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html), as well as cover the varying benefits depending on local network connectivity options in [Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html).

## Citrix SD-WAN for Home Offices Topologies

It is important for the Network Administrator to identify the Home User opportunity based on the requirements of the individual Home Users and their potentially different local network conditions. The local network conditions of the home user can enable some variance in Home User use-cases. Some Home Users may have access to multiple internet providers (e.g. DSL and Broadband), while other may only be limited to LTE service (e.g. AT&T and Verizon) in their area.  Most often, Home Users have the flexibility of have both wired and wireless transport options. Network Administrators can centrally control which of the Home User use-case deployments are supported through site profiles and templates.
Let us cover some different use-cases based on some variances of local network availability of Home Users:

[![Citrix SD-WAN Home Office Use Cases Comparison](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCasesComparison.png)](tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCasesComparison.png)

## Citrix SD-WAN for Home Offices Deployment

Assuming a Network Administrator already has Citrix SD-WAN deployed (e.g. MCN and Branch sites), onboarding individual Home Users makes use of the same workflow as adding new branch sites:

1.  Define one or many new Site Profile to group Home Users into a use-case depending on local network availability (e.g. Dual ISP, ISP+LTE)
2.  Batch clone new sites making use of Site Profile and Templates
3.  Configure Site specific detail (e.g. device serial number, LAN interfaces and subnet to serve as the new “Remote Work Network”)
4.  Define available WAN Services (e.g. Virtual Path, Internet) as a global parameter for all sites or limit to a subset of sites
5.  Configure Application QoS and Firewall policies as a global parameter for all sites or limit to a subset of sites
6.  Finally, push the configuration changes out to production in preparation to provision the new Home User sites to join the existing SD-WAN deployment
As an example, illustrated below is a more detailed look at one of the Home User use-cases leveraging available local networks of “ISP + LTE”.

[![Citrix SD-WAN Home Office Use Case Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCaseTopology.png)](tech-briefs_citrix-sdwan-home-office_CitrixSD-WANHomeOfficeUseCaseTopology.png)

In this scenario, the SD-WAN configuration can be limited to handle only the remote worker’s traffic leveraging Internet provided by the ISP as the primary form of connectivity, while using the LTE for a boost when there are reliability issues with the primary (shared) internet or when additional bandwidth is needed. Existing home network users would connect normally to the existing home network and share the available bandwidth with the encrypted UDP 4980 tunnel created by the SD-WAN device. If the ISP Router supports quality of service (a.k.a. traffic shaping), then it is recommended that the UDP 4980 traffic be prioritized above home user traffic for optimal SD-WAN performance. A new policy can be created categorizing the UDP 4980 traffic as High Priority.

[![Home ISP Router QOS](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_HomeISPRouterQOS.png)](tech-briefs_citrix-sdwan-home-office_HomeISPRouterQOS.png)

After the Network Administrator has pushed the new configuration and it is actively running on the existing SD-WAN deployment, the Home User workflow can begin. Once the Home User receives their SD-WAN appliance they can run through one of the installation workflows to stand up their SD-WAN device using Zero Touch Deployment (ZTD). Depending on the availability of an active LTE SIM card, an installer may opt to choose the ZTD method using the WAN interface instead of the LTE interface. Both methods are detailed below as sperate Quick Start Guides.

If there is a requirement to limit home user responsibility, the Administration team can pre-stage the appliances or a third-party company (e.g. possibly the vendors providing the ISP or LTE) can be enlisted to help perform some or all the installation workflow before the device is shipped to the Home User, however the workflow is simple enough for most end-user to directly manage the onboard of their device.

With the Home User’s SD-WAN successfully activated through zero touch deployment, a Virtual Path will be established across the available WAN and LTE interfaces to the configured MCN/RCN in the existing SD-WAN environment. Any changes to the local administration of the device or SD-WAN application and security policies are remotely managed by the SD-WAN Administrator.

For more information refer to:

[ZTD via the WAN Interface (Citrix SD-WAN 110-SE)](https://support.citrix.com/article/CTX272229)

[ZTD via the LTE Interface (Citrix SD-WAN 110-LTE-SE)](https://support.citrix.com/article/CTX272228)

[Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html)

[Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
