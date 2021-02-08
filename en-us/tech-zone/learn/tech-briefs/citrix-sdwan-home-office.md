---
layout: doc
h3InToc: true
contributedBy: Shoaib Yusuf
specialThanksTo: Matthew Brooks
description: Learn how to work from home with secure, enhanced, and resilient connectivity using the Citrix SD-WAN 110 appliance.
---
# Citrix SD-WAN for Home Offices Brief

## Introduction

Businesses can now use the Citrix SD-WAN solution for Home Office users. With it SD-WAN Administrators can quickly extend reliable, and secure workplace environments to any location.

## Challenges with Teleworking

Remote users generally encounter common challenges that impact productivity, which include:

*  Poor end-user experience, due to low quality Internet links, and consumer grade hardware
*  Unreliable connectivity due to geographically distributed remote locations
*  Shared internet access that cannot differentiate between business, and recreational traffic

IT Administrators struggle with the following challenges:

*  Scaling the rollout, and onboarding of remote workers in a timely manner
*  Managing a large deployment becomes difficult, and expensive
*  Securing the environment by protecting users, data, applications, and the network

In this Tech Brief we discuss how the Citrix SD-WAN solution can address these challenges.

## Citrix Enables Work from Anywhere

Citrix solutions enable business continuity. As the definition of workforce changes, employee needs evolve. Citrix provides solutions that extend the digital workspace to end-users across any device, on any network. A critical component of that business plan is to ensure that users remain productive while maintaining the necessary level of security, and control over user access to corporate resources. Citrix Workspace, including Citrix Virtual Apps and Desktops, enables seamless workforce productivity. It gives employees the flexibility to work from anywhere, while keeping your apps, and information secure. Sometime, due to uncontrollable network conditions, end-user experience cannot always be guaranteed.

Traditional remote branch offices are an extension of the Data Center network, connected over Private Intranet links (for instance MPLS). In the use-case of remote branch offices, Private Intranet circuits are reliable, and secure. However, depending on the location of the branch office, ensuring availability, and paying for a high bandwidth WAN becomes challenging. Issues with either can be a detriment to a good user-experience.

[![Branch MPLS Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_branchmplstopology.png)](tech-briefs_citrix-sdwan-home-office_branchmplstopology.png)

Home Workers / Teleworkers traditionally connect into the network over Public Internet links. Like a traditional VPN solution, one option would be to utilize Citrix Gateway. It provides users with secure remote access to Citrix Virtual Apps and Desktops across a range of devices. In a VPN use-case, Public Internet links are often unreliable, and of poor quality. The quality depends on the geographic location of the teleworker, and the conditions of the last mile. Ensuring a high end-user experience across a questionable WAN becomes a challenge as teleworkers, and home workers are introduced to the workforce.

[![Home Office Internet Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_homeofficeinternettopology.png)](tech-briefs_citrix-sdwan-home-office_homeofficeinternettopology.png)

## Introducing SD-WAN 110-LTE-Wi-Fi-SE

Citrix SD-WAN 110 platform addresses the needs of small offices, retail stores, and work-from-home users. The 110 platform ensures business continuity in a small form factor providing the highest network resiliency with subsecond failover. It also includes the best in-class application experience for virtual, SaaS, and cloud applications with end-to-end QOS.

[**SD-WAN 110-LTE-Wi-Fi-SE Overview**]( https://www.youtube.com/watch?v=a1mVYA-Ckyk )

SD-WAN 110-LTE-Wi-Fi-SE Product Specs include:

*  Four 10/100/1000 Ethernet Ports for WAN, or LAN (interface 1/4 as a data port available with R11.1.1 and above)
*  Integrated LTE with dual SIM slots (a single modem allowing the SIM to be used as active / passive)
*  USB based LTE modem dongle support (available with R11.1.1 and above)
*  Integrated Wi-Fi Access Point capability (available with R11.3.0 and above)

Citrix SD-WAN is a book ended solution providing reliable, and high bandwidth connectivity between site locations. It directly addresses potential limitations of the WAN as indicated with the use-cases in the section preceding.

## Citrix SD-WAN for Home Offices Overview

Concerns regarding the WAN for the Home Office use-case can be solved with the Citrix SD-WAN solution. The book ended solution can strategically be placed on the segment of the network that typically impacts user experience the most. It provides a new WAN overlay, designed to increase end-user experience, and performance.

[![Citrix SD-WAN Home Office Use Case](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecase.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecase.png)

Generally, you find more benefits with the SD-WAN solution by introducing more WAN transports. These include DLS, Broadband Internet, or LTE links to augment any existing WAN transports. Diversifying the ISPs can significantly increase network reliability. In the preceding scenario, SD-WAN devices (physical, or virtual) can be placed directly in the path of the existing network. With more WAN transports it increases user experience, and productivity. The benefits include:

*  link bandwidth aggregation (a.k.a. link load balancing) for remote locations with multiple Internet/LTE links
*  subsecond link blackout, and brownout detection, enabling an always-on network
*  per-packet prioritization providing dual-ended QOS with the ability to deliver asymmetrically across all available paths
*  augment security protection with cloud services (for instance [Citrix Secure Internet Access](https://www.citrix.com/products/citrix-secure-internet-access/), Zscaler, or Palo Alto Prisma)
*  utilize higher-end SD-WAN Advanced Edition appliances to unlock an integrated security stack or leverage third-party firewalls (for instance Palo Alto, Checkpoint) as Virtual Network Function (VNF) with simple integration with industry-leading security vendors
*  serve as a DHCP Server to allocate or fix IP address for LAN devices
*  enable usage of LTE-based WAN with standby (last resort, and on-demand) functionality to further increase network reliability across wireless transports
*  prioritize business critical applications through a Deep Packet Inspection (DPI), and a QOS engine, and with an integrated firewall solution
*  route and enforce security policies to block undesired apps
*  allow direct internet breakout for low latency connectivity to trusted SaaS apps, for instance Microsoft 365

The availability of these benefits can vary based on the available WAN transports, and the design of the site. The Network Administrator for the **SD-WAN deployment plays a** key role in SD-WAN deployments. The Network Administrator centrally dictates what is business critical, and the extent of connectivity for the Home Office use-case. We dive into more detail regarding the roles, and responsibilities of an Administrator in the [Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html). We cover the varying benefits, depending on local network connectivity options, in [Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html).

## Citrix SD-WAN for Home Offices Topologies

It is important Home Office requirements are based on local network conditions. Some Home Offices have access to multiple Internet providers (for instance DSL, and Broadband). Others might be limited to LTE service in their area (for instance AT&T, and Verizon). Most often, Home Offices have the flexibility of having both wired, and wireless transport options. Network Administrators can centrally control which of the Home Office use-case deployments are supported through site profiles, and templates.
Let us cover some different use-cases based on some variances of local network availability of Home Offices:

[![Citrix SD-WAN Home Office Use Cases Comparison](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeuseasescomparison.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeuseasescomparison.png)

We’ll dive into more detail on the roles and responsibilities of a Network Administrator in the [Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html), in addition to cover the varying benefits depending on local network connectivity options in [Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html).

## Citrix SD-WAN for Home Offices Deployment

Enterprises with Citrix SD-WAN already deployed can use the same workflow as adding new branch sites to add a new Home Office site:

1.  Define one, or many new Site Profiles to group Home Offices. They can be grouped into a use-case depending on local network availability. For instance Dual ISP, ISP+LTE.
2.  Batch clone new sites with Site Profiles, and Templates.
3.  Configure Site specific detail. For instance device serial number, LAN interfaces, and subnet to serve as the new “Remote Work Network”.
4.  Define available WAN Services. For instance Virtual Path, Internet, or Secure Internet Access as a global parameter for all sites, or limit to a subset of sites.
5.  Configure Application QoS, and Firewall policies as a global parameter for all sites, or limit to a subset of sites.
6.  Finally, push the configuration changes out to production. Thereafter you can provision the new Home Office sites to join the existing SD-WAN deployment.

As an example, illustrated below is a more detailed look at one of the Home Office use-cases which uses available local networks of “ISP + LTE” and makes use of the SD-WAN interface 1/1 to physically connect LAN devices and Wi-Fi to connect wireless devices.

[![Citrix SD-WAN Home Office Use Case Topology](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecasetopology.png)](tech-briefs_citrix-sdwan-home-office_citrixsd-wanhomeofficeusecasetopology.png)

In this scenario, the SD-WAN configuration can be limited to handle only the remote worker’s traffic. The SD-WAN delivers traffic from any device connected physically or wirelessly through the SD-WAN overlay, leveraging the Internet service provided by the ISP as the primary form of connectivity. At the same time it uses LTE for a boost when there are reliability issues with the primary (shared) Internet, or when more bandwidth is needed. Existing home network users, for instance family or kids, would connect normally to the existing home network, and share the available bandwidth with the encrypted UDP 4980 tunnel created by the SD-WAN device. If the ISP Router supports QOS, otherwise known as traffic shaping, then it is recommended that the UDP 4980 traffic be prioritized above Home Office traffic for optimal SD-WAN performance. A new policy can be created categorizing the UDP 4980 traffic as High Priority.

[![Home ISP Router QOS](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-home-office_homeisprouterqos.png)](tech-briefs_citrix-sdwan-home-office_homeisprouterqos.png)

After the Network Administrator has pushed the new configuration, and it is actively running on the existing SD-WAN deployment, the Home Office workflow can begin. Once the home worker receives their SD-WAN appliance they can run through one of the installation workflows to stand up their SD-WAN device using Zero Touch Deployment. Depending on the availability of an active LTE SIM card, an installer can opt to choose the Zero-touch deployment method using the WAN interface instead of the LTE interface. Both methods are detailed following as separate Quick Start Guides.

If there is a requirement to limit home worker responsibility, the Administration team can pre-stage the appliances. A third-party company can be enlisted to help perform the installation workflow before the device is shipped to the Home Office. (for instance possibly the ISP, or LTE provider) However, the workflow is simple enough for most end users to onboard their device.

With the Home Office’s SD-WAN successfully activated through zero touch deployment, a Virtual Path is established across the available WAN, and LTE interfaces. The Virtual Paths connect to the network MCN/RCN in the existing SD-WAN environment. Any changes to the local administration of the device, or SD-WAN application, and security policies are remotely managed by the SD-WAN Administrator.

## References

For more information refer to:

[Zero-touch deployment via the WAN Interface (Citrix SD-WAN 110-SE)](https://support.citrix.com/article/CTX272229) - a Quick Start Guide for Citrix SD-WAN 110-SE appliances when performing Zero-touch deployment via the WAN Interface

[Zero-touch deployment via the LTE Interface (Citrix SD-WAN 110-LTE-SE)](https://support.citrix.com/article/CTX272228) - a Quick Start Guide for Citrix SD-WAN 110-LTE-SE appliances when performing zero-touch deployment via the LTE Interface

[Citrix SD-WAN Home Office POC Guide](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html) - learn how to implement a proof of concept of Citrix SD-WAN for a Home Office

[Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html) - learn about the decisions to need to be made to implement Citrix SD-WAN for Home Offices
