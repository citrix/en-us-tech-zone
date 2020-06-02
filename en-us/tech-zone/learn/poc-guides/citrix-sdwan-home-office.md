---
layout: doc
description: Learn how to implement a POC of the Citrix SD-WAN 110 appliance to demonstrate how to work from home with secure, enhanced, and resilient connectivity
---
# Citrix SD-WAN for Home Offices

## Contributors

**Author:** [Shoaib Yusuf](https://twitter.com/Shoaibys)

**Special Thanks** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

## Overview

This proof of concept (PoC) guide is designed to help you quickly deploy an SD-WAN Home Office deployment using SD-WAN Orchestrator as the management tool. The [Citrix SD-WAN Home Office Design Decisions Guide](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html) outlined WAN topology decisions to integrate Citrix SD-WAN in a Home Office. Below we use the ISP + LTE option, which includes a primary WAN link provided by an ISP and is augmented with an LTE service, to review implementation considerations.

[![ISP + LTE Home Office Topology](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_ispplusltehomeofficetopology.png)](poc-guides_citrix-sdwan-home-office_ispplusltehomeofficetopology.png)

In this use-case (ISP+LTE), an Administrator must first validate that the targeted home user network meets the following prerequisites:

*  Have up and running an existing internet connection provided by their local ISP.
*  The ISP has provided a managed router that serves the existing “Home Network”.
The ISP Router will need to have an available Ethernet port.  It will also need to function as a DHCP Server to allocate IP and DNS address information to client host requesting it through Ethernet. Interface 1/2 on the SD-WAN device will be enabled as the DHCP Client.
*  An activated LTE SIM card (either purchased directly by the end-user or supplied by the Admin or a third-party vendor).

Confirming the prerequisites are available before engaging the user in any on-site activity will help ensure rapid streamlined deployment of thousands of endpoints.

Selecting the appropriate SD-WAN platform that supports the desired design for the home user is essential. For example, if the design outlines that an LTE service will be used as a WAN link, then selecting a SD-WAN platform that has an integrated LTE modem (e.g. 110-LTE-SE, 210-LTE-SE) would help reduce on-site components and mitigate complexity for the installer.

The design options (Single ISP, Dual ISP, ISP+LTE, ISP+LTE standby, Dual LTE) detailed earlier in this documentation are fully supported with the Citrix 110-LTE-SE and 210-LTE-SE platforms. Each SD-WAN platform has slightly different hardware specifications, enabling some variances in supported design.

As an example, when using a 210 Standard Edition platform, the system is equipped with integrated bypass hardware which allows the SD-WAN to be deployed in Inline Mode, and in the event of SD-WAN hardware failure the home worker can have continued internet connectivity with the SD-WAN failing to the underlay or home network through the bypass interfaces.  

Alternatively, by selecting Advanced Edition (targeted to be supported on the 210, 410 and 1100 platforms), the system is equipped with edge security features (such as IDS/IPS, web filtering, malware protection), which can help secure the “Remote Work Network” and enable local breakout for select internet traffic.  

Another example would be using an 1100-SE platform with available PoE+ interfaces, enabling usage of Ethernet powered devices, such as a VoIP Deskphone. However, making use of a PoE Injector can enable usage of lower-end SD-WAN devices, like a 110-SE, for the home user use-case that require PoE. In addition to bypass interfaces, the 1100 platforms have the capability to host a third-party VNF such as a Palo Alto or Checkpoint firewall.

Some other feature capabilities worth mentioning to help narrow the focus on the right platform to select in Home Worker network design include the 110 platform that comes WiFi-Ready, which after a software upgrade to R11.3.x will be able to function as a wireless access point for the “Remote Work Network” (targeted only for the 110-LTE-WiFi-SE platform in Q4’20).  The 110 platform also has support for an external USB LTE modem (targeted also for the 210 and 1100 platforms with R11.1.1), which can be used to support the Dual LTE use case or add a third WAN option for the ISP+LTE use-case.

## Using SD-WAN Orchestrator

SD-WAN Administrators centrally manage and limit their supported deployment use-case through Citrix SD-WAN Orchestrator site profiles and templates. Limiting the Home User deployment scenarios makes it easy to manage large scale deployments and allow quick modification to multiple sites to accommodate for future changes.
There are some additional administrative design considerations to account for when building the home user site in SD-WAN Orchestrator, for example:

*  What method of Zero Touch Deployment will be used to provision the remote devices?
*  After the appliance is provisioned, how will the device continue communication with SD-WAN Orchestrator for further configuration updates and data collection?
*  How can we account for continued connectivity to Cloud Services in different failure scenarios?
We will highlight some of these as we step through building the configuration.

We will highlight some of these as we step through building the configuration.

### Create Site Profile

To set up Site Profiles in Orchestrator, an Administrator can perform the following steps:
(For more information refer to [Citrix Orchestrator profiles](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/profiles.html)).

1.  Create a new site profile (by selecting All Sites, then navigating to Configuration > Profiles & Templates > Profiles)
1.  Populate Site Details:
     *  Site Profile Name: HomeUser(110_ISP+LTE)
     *  Device Model: 110
     *  Device Edition: SE
     *  Sub-Model: LTE-WiFi
     *  Site Role: Branch
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorsitedetails.png)](poc-guides_citrix-sdwan-home-office_orchestratorsitedetails.png)
1.  Next, configure usage of Interfaces to match the desired design, add each required interface:
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_isppluslteinterfaces.png)](poc-guides_citrix-sdwan-home-office_isppluslteinterfaces.png)

*  Add Interface 1/1 for LAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: LAN
    *  Security: Trusted
    *  Select Interface: **1/1**
    *  VLAN ID: 0
    *  Routing Domain: **HomeUser**
(_The single routing domain (Default_RoutingDomain) can be used for SD-WAN deployments that are only connecting home users and are not connecting into an existing SD-WAN site deployment. However, in the scenario outlined earlier in the documentation, to add home users to an already existing deployment, a new routing domain can be introduced to segregate and limit connectivity access of home user in the data center network._)
    *  Firewall Zone: **Default_LAN_Zone**
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslan.png)](poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslan.png)

*  Add Interface 1/2 for WAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: **WAN**
    *  Security: **Untrusted**
    *  Select Interface: **1/2**
    *  DHCP Client: **enabled**
(_The expectation is that that this SD-WAN interface will be cabled to the home users existing home network, where a home router will be configured as a DHCP Server and will assign an IP address to interface 1/2 operating as a DHCP Client._)
    *  VLAN ID: 0
    *  Routing Domain: Default_RoutingDomain
    *  Firewall Zone: **Untrusted Internet_Zone**
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceswan.png)](poc-guides_citrix-sdwan-home-office_orchestratorinterfaceswan.png)

*  Add Interface LTE-1 for WAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: **WAN**
    *  Security: **Untrusted**
    *  Select Interface: **LTE-1**
    *  VLAN ID: 0
    *  DHCP Client: **enabled**
    *  Routing Domain: Default_RoutingDomain
    *  Firewall Zone: **Untrusted Internet_Zone**
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslte.png)](poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslte.png)

*  Add Interface 1/4-MGMT for device administration purposes:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: LAN
    *  Security: Trusted
    *  Select Interface: **1/4-MGMT**
    *  VLAN ID: 0
    *  Routing Domain: **Default_RoutingDomain**
(_The configuration of this management interface is required and serves two purposes_:
   1.  _Enables continued connectivity to Cloud Services after the device has been provisioned through ZTD._
   2.  _Serves as a method for an Administrator to remotely access the device’s local web interface for troubleshooting/monitoring._

        _Assuming the Administrator is in the data center network connecting to the SD-WAN on the Default_RoutingDomain, the remote SD-WAN device’s web interface can be accessed with the in-band management feature enabled on this interface. Connectivity to the Mgmt. interface by the remote Admin is accomplished through the Virtual Path. Also, connectivity to Cloud Services, like SD-WAN Orchestrator, is accomplished through local internet breakout (Internet Service) being enabled for the Default_RoutingDomain.  If desired internet connectivity can alternatively be backhauled through the data center and broken out there for internet access. The management port (1/4) does not require to be cabled for the web interface and data polling features to work on any in-band management enabled interface._)
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratormanagement.png)](poc-guides_citrix-sdwan-home-office_orchestratormanagement.png)
    *  Firewall Zone: **Default_LAN_Zone**
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfacesmgmt.png)](poc-guides_citrix-sdwan-home-office_orchestratorinterfacesmgmt.png)

Interfaces that meet the following configuration requirements can be used for in-band management:

*  Security must be set to Trusted
*  Interface type must be LAN
*  Selected IP should not be set to Private
*  Identify of the IP should be set to true

Site Profiles allow for selection of the in-band management IP that meet those requirements

[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfacessummary.png)](poc-guides_citrix-sdwan-home-office_orchestratorinterfacessummary.png)
Find more detail on [in-band management](/en-us/citrix-sd-wan/11-1/inband-and-backup-management.html)

1.  Next, create new WAN links to match the desired design:
     *  Add WAN Link#1 using interface 1/2:
     *  Access Type: Public Internet
     *  ISP Name (Custom): ISP
     *  Internet Category: Internet
     *  Link Name: Internet-ISP-1
     *  Public IP Address Auto Learn: ENABLED
(_The MCN/RCN denoted sites will dynamically learn the advertised public IP address of each branch site using this feature, as Branch nodes attempt the path establishment with their MCN/MCN. When using Public Internet transports, only static public IP addresses are required for the MCN/RCN denoted sites._)
     *  Egress Speed:  50 Mbps
     *  Ingress Speed: 50 Mbps
(_The upload and download speed defined on WAN link#1 will be dependent on the bandwidth availability of each home network. It is recommended to stay under those bandwidth limitations and allow for some bandwidth to be used by other household members sharing that link. Prioritizing SD-WAN tunnel traffic (UDP 4980) on the ISP router will help ensure SD-WAN does not back off usage of that link when contention for the link is encountered. If needed, several Site Profiles, at different WAN links speeds, can be configured to accommodate for some variances in home user local internet conditions._)
     *  Virtual Interface: VIF-2-WAN-1
     *  Virtual Path Mode: Primary
[![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorwanlinks.png)](poc-guides_citrix-sdwan-home-office_orchestratorwanlinks.png)
