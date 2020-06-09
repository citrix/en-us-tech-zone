---
layout: doc
description: Learn how to work from home with secure, enhanced, and resilient connectivity using the Citrix SD-WAN 110 appliance.
---
# Citrix SD-WAN for Home Offices

## Contributors

**Author:** [Shoaib Yusuf](https://twitter.com/Shoaibys)

**Special Thanks** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

## Introduction

Businesses can now use the Citrix SD-WAN solution for Home Office Users. With it SD-WAN Administrators can quickly extend reliable, and secure workplace environments to any location.

## Challenges with Teleworking

Remote users generally encounter common challenges that impact productivity, which include:

*  Poor end-user experience, due to low quality Internet links, and consumer grade hardware
*  Unreliable connectivity due to geographically distributed remote locations
*  Shared Internet Access (SIA) that cannot differentiate between business, and recreational traffic

IT Administrators struggle with the following challenges:

*  Scaling the rollout, and onboarding of remote workers in a timely manner
*  Managing a large deployment becomes difficult, and expensive
*  Securing the environment by protecting users, data, applications, and the network

In this Tech Brief we discuss how the Citrix SD-WAN solution is capable of addressing these challenges.

## Citrix Enables Work from Anywhere

Citrix solutions enable business continuity. As the definition of workforce changes, employee needs evolve. Citrix provides solutions that extend the digital workspace to end-users across any device, on any network. A critical component of that business plan is to ensure that users remain productive while maintaining the necessary level of security, and control over user access to corporate resources. Citrix Workspace, including Citrix Virtual Apps and Desktops, enables seamless workforce productivity, giving employees the flexibility to work from anywhere, all while keeping your apps, and information secure. Sometime, due to uncontrollable network conditions, end-user experience cannot always be guaranteed.

Traditional remote branch offices are an extension of the Data Center network, connected over Private Intranet links (for instance MPLS). In the use-case of remote branch offices, Private Intranet circuits are reliable, and secure. However, depending on the location of the branch office, ensuring availability, and the cost of maintaining a high bandwidth WAN becomes challenging, which can negatively impact user-experience.

[![Branch MPLS Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_branchmplstopology.png)](tech-briefs_citrix-sdwan-home-office_branchmplstopology.png)

Home Users / Teleworkers traditionally connect into the network over Public Internet links. Like a traditional VPN solution, one option would be to utilize Citrix Gateway. It provides users with secure remote access to Citrix Virtual Apps and Desktops across a range of devices. In a VPN use-case, Public Internet links are often unreliable, and of poor quality. This depends on the geographic location of the teleworker, and the conditions of the last mile. Ensuring a high end-user experience across a questionable WAN becomes a challenge as teleworkers, and home users are introduced to the workforce.

[![Home Office Internet Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_homeofficeInternettopology.png)](tech-briefs_citrix-sdwan-home-office_homeofficeInternettopology.png)

## Introducing SD-WAN 110-LTE-Wi-Fi-SE

Citrix SD-WAN 110 platform addresses the needs of small offices, retail stores, and work-from-home users. The 110 platform ensures business continuity in a small form factor providing the highest network resiliency with subsecond failover. It also includes the best in-class application experience for virtual, SaaS, and cloud applications with end-to-end QOS.

[**SD-WAN 110-LTE-Wi-Fi-SE Overview**]( https://www.youtube.com/watch?v=a1mVYA-Ckyk )

SD-WAN 110-LTE-Wi-Fi-SE Product Specs include:

*  Four 10/100/1000 Ethernet Ports for WAN or LAN (interface 1/4 as a data port available with R11.1.1)
*  Integrated LTE with dual SIM slots (a single modem allowing the SIM to be used as active / passive)
*  USB based LTE dongle support (available with R11.1.1)
*  Integrated Wi-Fi Access Point capability (*Wi-Fi Ready)

*Starting 2Q 2020, 110-LTE-Wi-Fi will be shipped as “Wi-Fi Ready.” The Wi-Fi feature will be ready to use in the 4th quarter of 2020 through a software upgrade.

Citrix SD-WAN is a book ended solution providing reliable, and high bandwidth connectivity between site locations. It directly addresses potential limitations of the WAN as indicated with the use-cases in the section preceding.

## Citrix SD-WAN for Home Offices Overview

Concerns regarding the WAN for the Home Office use-case can be solved with the Citrix SD-WAN solution. The book ended solution can strategically be placed on the segment of the network that typically impacts user experience the most. It provides a new WAN overlay, designed to increase end-user experience, and performance.

[![Citrix SD-WAN Home Office Use Case](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecase.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecase.png)

Generally, you find more benefits with the SD-WAN solution by introducing more WAN transports. These include DLS, Broadband Internet or LTE links to augment any existing WAN transports. Diversifying the ISPs can significantly increase network reliability. In the preceding scenario, SD-WAN devices (physical or virtual) can be placed directly in the path of the existing network. With more WAN transports it increases user experience, and productivity. The benefits include:

*  link bandwidth aggregation (a.k.a. link load balancing) for remote locations with multiple Internet/LTE links
*  subsecond link blackout, and brownout detection, enabling an always-on network
*  per-packet prioritization providing dual-ended QOS with ability to deliver asymmetrically across all available paths
*  augment security protection with cloud services (for instance Zscaler, Palo Alto Prisma, and iBoss)
*  by utilizing higher-end appliance unlock more on-premises Virtual Network Function (VNF) options (for instance Palo Alto, Checkpoint) with simple integration with industry-leading security vendors
*  serve as a DHCP Server to allocate, and fix IP address for LAN devices based on MAC address
*  enable usage of LTE-based WAN with standby (last resort, and on-demand) functionality to further increase network reliability across wireless transports
*  prioritize business critical applications utilizing a Deep Packet Inspection (DPI), and a QOS engine, and with an integrated firewall solution, the ability to route, and enforce security policies to block undesired apps or allow direct Internet breakout for low latency connectivity to trusted SaaS apps (for instance Office 365)

The availability of these benefits can vary based on the available WAN transports, and the design of the site. The Network Administrator for the SD-WAN deployment plays a key role in SD-WAN deployments. The Network Administrator centrally dictates what is business critical, and the extent of connectivity for the Home Office use-case. We’ll dive into more detail on the roles, and responsibilities of an Administrator in [Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html). We'll cover the varying benefits depending on local network connectivity options in [Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html).

## Citrix SD-WAN for Home Offices Topologies

It is important Home Office requirements based on local network conditions. Some Home Offices have access to multiple Internet providers (for instance DSL, and Broadband). Others may only be limited to LTE service in their area (for instance AT&T, and Verizon). Most often, Home Offices have the flexibility of have both wired, and wireless transport options. Network Administrators can centrally control which of the Home Office use-case deployments are supported through site profiles, and templates.
Let us cover some different use-cases based on some variances of local network availability of Home Offices:

[![Citrix SD-WAN Home Office Use Cases Comparison](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeuseasescomparison.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeuseasescomparison.png)

## Citrix SD-WAN for Home Offices Deployment

Enterprises with Citrix SD-WAN already deployed can use the same workflow as adding new branch sites:

1.  Define one or many new Site Profiles to group Home Offices into a use-case depending on local network availability (for instance Dual ISP, ISP+LTE)
2.  Batch clone new sites with Site Profiles, and Templates
3.  Configure Site specific detail (for instance device serial number, LAN interfaces, and subnet to serve as the new “Remote Work Network”)
4.  Define available WAN Services (for instance Virtual Path, Internet) as a global parameter for all sites or limit to a subset of sites
5.  Configure Application QoS, and Firewall policies as a global parameter for all sites or limit to a subset of sites
6.  Finally, push the configuration changes out to production.  Thereafter you'll be able to provision the new Home Office sites to join the existing SD-WAN deployment.

As an example, illustrated following is a more detailed look at one of the Home Office use-cases leveraging available local networks of “ISP + LTE”.

[![Citrix SD-WAN Home Office Use Case Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecasetopology.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecasetopology.png)

In this scenario, the SD-WAN configuration can be limited to handle only the remote worker’s traffic leveraging Internet service provided by the ISP as the primary form of connectivity. At the same time it uses LTE for a boost when there are reliability issues with the primary (shared) Internet or when more bandwidth is needed. Existing home network users would connect normally to the existing home network, and share the available bandwidth with the encrypted UDP 4980 tunnel created by the SD-WAN device. If the ISP Router supports Q (a.k.a. traffic shaping), then it is recommended that the UDP 4980 traffic be prioritized above Home Office traffic for optimal SD-WAN performance. A new policy can be created categorizing the UDP 4980 traffic as High Priority.

[![Home ISP Router QOS](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_homeisprouterqos.png)](tech-briefs_citrix-sdwan-home-office_homeisprouterqos.png)

After the Network Administrator has pushed the new configuration, and it is actively running on the existing SD-WAN deployment, the Home Office workflow can begin. Once the home user receives their SD-WAN appliance they can run through one of the installation workflows to stand up their SD-WAN device using Zero Touch Deployment (zero-touch deployment). Depending on the availability of an active LTE SIM card, an installer can opt to choose the zero-touch deployment method using the WAN interface instead of the LTE interface. Both methods are detailed below as separate Quick Start Guides.

If there is a requirement to limit home user responsibility, the Administration team can pre-stage the appliances. A third-party company (for instance possibly the ISP or LTE provider) can be enlisted to help perform the installation workflow before the device is shipped to the Home Office. However, the workflow is simple enough for most end users to onboard their device.

With the Home Office’s SD-WAN successfully activated through zero touch deployment, a Virtual Path will be established across the available WAN, and LTE interfaces to the configured MCN/RCN in the existing SD-WAN environment. Any changes to the local administration of the device or SD-WAN application, and security policies are remotely managed by the SD-WAN Administrator.

## References

For more information refer to:

[zero-touch deployment via the WAN Interface (Citrix SD-WAN 110-SE)](https://support.citrix.com/article/CTX272229) - a Quick Start Guide for Citrix SD-WAN 110-SE appliances when performing zero-touch deployment (Zero Touch Deployment) via the WAN Interface

[zero-touch deployment via the LTE Interface (Citrix SD-WAN 110-LTE-SE)](https://support.citrix.com/article/CTX272228) - a Quick Start Guide for Citrix SD-WAN 110-LTE-SE appliances when performing zero-touch deployment (Zero Touch Deployment) via the LTE Interface

[Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html) - learn how to implement a proof of concept of Citrix SD-WAN for a Home Office

[Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html) - learn about the decisions to need to be made to implement Citrix SD-WAN for Home Offices
