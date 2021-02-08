---
layout: doc
h3InToc: true
contributedBy: Shoaib Yusuf
specialThanksTo: Matthew Brooks
description: Learn how to implement a POC of the Citrix SD-WAN 110 appliance to demonstrate how to work from home with secure, enhanced, and resilient connectivity.
---
# Citrix SD-WAN for Home Offices POC Guide

## Overview

This proof of concept (PoC) guide is designed to help you quickly deploy a Citrix SD-WAN Home Office environment using the Citrix SD-WAN Orchestrator service as the management tool. The [Citrix SD-WAN Home Office Design Decisions Guide](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html) outlined WAN topology decisions to integrate Citrix SD-WAN in a Home Office.

Reviewing the following use-case for the ISP + LTE option, which includes a primary WAN link provided by an ISP. It is augmented with an LTE service, to review implementation considerations.

![ISP + LTE Home Office Topology](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_ispplusltehomeofficetopology.png)

In this use-case (ISP+LTE), an Administrator must first validate that the targeted Home Office network meets the following prerequisites:

*  Have up, and running an existing internet connection provided by their local ISP.
*  The ISP has provided a managed router that serves the existing “Home Network”.
The ISP Router must have an available Ethernet port. It also functions as a DHCP Server to allocate IP, and DNS address information to the client host requesting it through Ethernet. Interface 1/2 on the SD-WAN device is enabled as the DHCP Client.
*  An activated LTE SIM card (either purchased directly by the end-user, or supplied by the Admin, or a third-party vendor).

Confirming that the prerequisites are available, before engaging the user in any on-site activity, helps ensure a rapid streamlined deployment which can scale to thousands of endpoints.

Selecting the appropriate SD-WAN platform that supports the desired design for the Home Office is essential. For example, if the design outlines that an LTE service is used as a WAN link, then selecting an SD-WAN platform that has an integrated LTE modem (for instance 110-LTE-SE, 210-LTE-SE) would help reduce on-site components, and mitigate complexity for the installer.

The design options (Single ISP, Dual ISP, ISP+LTE, ISP+LTE standby, Dual LTE) detailed earlier in this documentation are fully supported with the Citrix 110-LTE-SE, and 210-LTE-SE platforms. Each SD-WAN platform has slightly different hardware specifications, enabling some variances in supported design.

As an example, when using a 210 Standard Edition platform, the system is equipped with integrated bypass hardware which allows the SD-WAN to be deployed in Inline Mode. In the event of SD-WAN hardware failure the home worker can have continued internet connectivity with the SD-WAN failing to the underlay, or home network through the bypass interfaces.

All Editions of Citrix SD-WAN interoperate with Citrix Secure Internet Access to provide home workers secure access to web and SaaS applications. Alternatively, by selecting Advanced Edition (supported on the 210, 410, and 1100 platforms), the system is equipped with edge security features (such as IDS/IPS, web filtering, malware protection). These features can help secure the “Remote Work Network”, and enable local breakout for select internet traffic.

Another option would be using a 1100-SE platform with available PoE+ interfaces, enabling usage of Ethernet powered devices, such as a VoIP Desk phone. However, using a PoE Injector can enable usage of lower-end SD-WAN devices, like a 110-SE, for the Home Office use-cases that requires PoE. In addition to bypass interfaces, the 1100 platforms can host a third-party VNF such as a Palo Alto, or Checkpoint firewall.

Some other feature capabilities worth mentioning to help narrow the focus on the right platform to select in Home Worker network design include the 110 platform that comes Wi-Fi-Ready. After a software upgrade to R11.3.x it can function as a wireless access point for the “Remote Work Network”. (This functionality is only available for the 110-LTE-Wi-Fi-SE platform). The 110 platform also has support for an external USB LTE modem (available also for the 210, and 1100 platforms). It can be used to support the Dual LTE use case, or add a third WAN option for the ISP+LTE use-case.

## Using the Citrix SD-WAN Orchestrator service

SD-WAN Administrators centrally manage, and limit their supported deployment use-case through the Citrix SD-WAN Orchestrator service site profiles, and templates. Limiting the Home Office deployment scenarios makes it easy to manage large scale deployments, and allow quick modification to multiple sites to accommodate for future changes.
There are some additional administrative considerations to account for when building the Home Office site in the Citrix SD-WAN Orchestrator service, for example:

*  What method of Zero Touch Deployment is used to provision the remote devices?
*  After the appliance is provisioned, how does the device continue communication with the Citrix SD-WAN Orchestrator service for further configuration updates, and data collection?
*  How can we account for continued connectivity to Cloud Services in different failure scenarios?
*  If the Home Office deployment uses WAN Delivery Services, for instance Citrix Secure Internet Access or Citrix Cloud Direct, then a single Routing Domain must be used. Multiple Routing Domains is not currently supported to use in tandem with WAN Delivery Services.

We highlight some of these as we step through building the configuration.

## Create Site Profile

To set up Site Profiles in Orchestrator, an Administrator can perform the following steps:
(For more information refer to [Citrix Orchestrator profiles](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/profiles.html)).

1.  Create a new site profile (by selecting **All Sites**, then navigating to **Configuration > Profiles & Templates > Profiles**)
1.  Populate Site Details:
     *  Site Profile Name: **HomeOffice(110_ISP+LTE)**
     *  Device Model: **110**
     *  Device Edition: SE
     *  Sub-Model: **`LTE-WiFi`**
     *  Site Role: **Branch**
![Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorsitedetails.png)
1.  Next, configure usage of Interfaces to match the desired design, add each required interface:
![ISP + LTE](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_isppluslteinterfaces.png)

*  Add Interface 1/1 for LAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: LAN
    *  Security: Trusted
    *  Select Interface: **1/1** and SSID1 (this LAN interface defines the parameters for physically connect end-host devices in addition to devices connected through the Wi-Fi SSID)
    *  VLAN ID: 0
    *  Routing Domain: **HomeOffice**
(_The single routing domain (Default_RoutingDomain) can be used for SD-WAN deployments that are only connecting Home Offices, and are not connecting into an existing SD-WAN site deployment. However, in the scenario outlined earlier in the documentation, to add Home Offices to an existing deployment, a new routing domain can be introduced to segregate, and limit connectivity access of Home Office in the data center network._)
    *  Firewall Zone: **Default_LAN_Zone**
![Interfaces LAN](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslan.png)

*  Add Interface 1/2 for WAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: **WAN**
    *  Security: **Untrusted**
    *  Select Interface: **1/2**
    *  DHCP Client: **enabled**
(_The expectation is that this SD-WAN interface is cabled to the Home Offices existing home network, where a home router is configured as a DHCP Server, and is assign an IP address to interface 1/2 operating as the DHCP Client._)
    *  VLAN ID: 0
    *  Routing Domain: Default_RoutingDomain
    *  Firewall Zone: **Untrusted Internet_Zone**
![Interfaces WAN](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceswan.png)]

*  Add Interface LTE-1 for WAN:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: **WAN**
    *  Security: **Untrusted**
    *  Select Interface: **LTE-1**
    *  VLAN ID: 0
    *  DHCP Client: **enabled**
    *  Routing Domain: Default_RoutingDomain
    *  Firewall Zone: **Untrusted Internet_Zone**
![Interface LTE](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfaceslte.png)

*  Add Interface 1/4-MGMT for device administration purposes:
    *  Deployment Mode: Edge (Gateway)
    *  Interface Type: LAN
    *  Security: Trusted
    *  Select Interface: **1/4-MGMT**
    *  VLAN ID: 0
    *  Routing Domain: **Default_RoutingDomain**
(_The configuration of this management interface is required, and serves two purposes_:
   1.  _Enables continued connectivity to Cloud Services after the device has been provisioned through zero-touch deployment._
   2.  _Serves as a method for an Administrator to remotely access the device’s local web interface for troubleshooting/monitoring._

        _Assuming the Administrator is in the data center network connecting to the SD-WAN on the Default_RoutingDomain, the remote SD-WAN device’s web interface can be accessed with the in-band management feature enabled on this interface. Connectivity to the Mgmt. interface by the remote Admin is accomplished through the Virtual Path. Also, connectivity to Cloud Services, like the Citrix SD-WAN Orchestrator service, is accomplished through local internet breakout (Internet Service) being enabled for the Default_RoutingDomain. If desired internet connectivity can alternatively be backhauled through the data center, and broken out there for internet access. The management port (1/4) does not require to be cabled for the web interface, and data polling features to work on any in-band management enabled interface._)
![Management](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratormanagement.png)
    *  Firewall Zone: **Default_LAN_Zone**
![Interface Mgmt](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorinterfacesmgmt.png)

Interfaces that meet the following configuration requirements can be used for in-band management:

*  Security must be set to **Trusted**
*  Interface type must be **LAN**
*  Selected IP not set to **Private**
*  **Identity** of the IP set to true

4.Next, create new WAN links to match the desired design:

*  Add WAN Link #1 using interface 1/2:
    *  Access Type: **Public Internet**
    *  ISP Name **(Custom): ISP**
    *  Internet Category: **Internet**
    *  Link Name: Internet-ISP-1
    *  Public IP Address Auto Learn: **ENABLED**
(_The MCN/RCN denoted sites are dynamically learn the advertised public IP address of each branch site using this feature, as Branch nodes attempt the path establishment with their MCN/RCN. When using Public Internet transports, only static public IP addresses are required for the MCN/RCN denoted sites._)
    *  Egress Speed: **50** Mbps
    *  Ingress Speed: **50** Mbps
(_The upload and download speed defined on WAN link #1 is dependent on the bandwidth availability of each home network. It is recommended to stay under those bandwidth limitations and allow for some bandwidth to be used by other household members sharing that link. Prioritizing SD-WAN tunnel traffic (UDP 4980) on the ISP router helps ensure SD-WAN does not back off usage of that link when contention for the link is encountered. If needed, several Site Profiles, at different WAN links speeds, can be configured to accommodate for some variances in Home Office local internet conditions._)
    *  Virtual Interface: **VIF-2-WAN-1**
    *  Virtual Path Mode: Primary
![Wan Link 1](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorwanlinks1.png)

*  Add WAN Link #2 using interface LTE-1:
    *  Access Type: **Public Internet**
    *  ISP Name (Custom): **LTESP**
    *  Internet Category: **LTE**
    *  Link Name: LTE-LTE-SP-2
    *  Public IP Address Auto Learn: **ENABLED**
    *  Egress Speed:  **20** Mbps
    *  Ingress Speed: **20** Mbps
(_The upload and download speed defined on WAN link #2 is dependent on the LTE provider, however an Admin can limit usage by hard-setting lower bandwidth speeds. Or can set at expected rates and configure feature such as Adaptive Bandwidth Detection._)
    *  Virtual Interface: **VIF-3-WAN-2**
    *  Virtual Path Mode: Primary
    *  Active MTU detect: disabled
    *  Enable Metering: disabled
    *  Standby Mode: **Last-Resort**
(_WAN links enabled for Standby have two modes of operation: Last-Resort, or On-Demand. Last-Resort standby links only become active when all non-standby links are unavailable, or disabled. On-Demand standby links become active under similar circumstances, but also have the capability to become active when the available bandwidth of the Virtual Path is greater than the configured on-demand bandwidth limit. In both standby modes, there is still data usage on the link when not active. The amount of data usage can be controlled with the frequency of heartbeat intervals. As an example, in an inactive link state, a standby WAN link can consume 150 MB to 270 MB of data with a 1 second heartbeat interval configured just for the probe traffic._)
    *  Active Heartbeat Interval: 1
    *  Standby Heartbeat Interval: 1
![Wan Link 2](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorwanlinks2.png)

## Batch Create Sites

Once the Site Profile is created, it can be used in creating multiple Home Office sites in batch. To add new sites in Batch using the Citrix SD-WAN Orchestrator service, an Admin can perform the following:
(For more information refer to [Orchestrator Network Configuration](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/network-configuration.html))

1.  Batch add sites (by selecting All Sites, then navigating to Configuration > Network Configuration Home, and click the Batch Add Sites button)
1.  Input the # of Sites to create in batch and click Next
1.  Select the Site Profile to globally associate with the new sites and input the unique attributes to identify each site (for instance Site Name, Site Address)
![Batch Create Site](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchcreatesites.png)

## Basic Settings for Site Configuration

With the sites created and each referencing the desired site profile, clicking into each created site allows for further configuration allowing input of unique attributes that are specific to each site.

1.  **Site Details**:
Here sites can be associated to a specific **Region** when deployed in a multi-region architecture, or leave it in the default-region if deploying in the default single-region deployment.
![Batch Site Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchsitedetails.png)
1.  **Device Details**:
Here a **unique serial number**, specifically the serial number of the device shipped to the Office Site, is inputted. The serial number is used to authenticate the appliance as it calls home and retrieves this specific site configuration during the zero-touch deployment process.
![Batch Device Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchdevicedetails.png)
1.  **Wi-Fi Details**
     *  i. Enable Wi-Fi.  Configure the desired Radio and SSID settings for this Home Office site.
![Wi-Fi Details](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_wifidetails.png)
1.  **Interfaces**:
     *  i. Edit Interface 1/1 LAN.
Each site needs to be uniquely defined with a “Remote Work Network”. Select the LAN interface and input the subnet to be allocated for this specific remote site. The Primary IP address entered serves as the **LAN Gateway VIP** (for instance 172.17.35.1/29) for this remote work network.
![Batch Interfaces](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchinterfaces.png)
     *  ii. Edit Interface 1/2 WAN.
Verify the WAN interface is enabled for the DHCP Client, to automatically obtain an IP address from the “Existing Home Network”. The benefit of using this feature is that you do not have to specifically know the existing subnets of home networks, but there are dependencies on the underlay to have an available 10/100/1000 Ethernet port on your existing home router/modem and ensure any device connected to it is issued an IP address, DNS, and Internet connectivity.
![Batch WAN 12](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchinterfaceswan12.png)
     *  iii. Edit Interface LTE-1 WAN.
Verify the LTE WAN interface is enabled for DHCP Client, to automatically obtain an IP address from the LTE provider network.
![Batch LTE](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchinterfaceswanlte.png)
     *  iv. Edit Interface 1/4 -MGMT LAN.
Each site needs to be uniquely defined with a management IP address, which serves the purpose for continued connectivity to Cloud Services and web interface access by the remote Admin through the Virtual Path. Input the subnet to be allocated for this specific remote site. The Primary IP address entered serves as the **Management IP** (for instance 172.17.36.1/32) for this remote work network.
![Batch Interfaces Mgmt](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchinterfacesmgmt.png)
Select the InBand Management IP (for instance 172.17.36.1) from the drop-down menu. For more information see [in-band management](/en-us/citrix-sd-wan/11-1/inband-and-backup-management.html).
![Management IPs](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchinterfacesmgmtip.png)
1.  WAN Links:
    *  i. Edit WAN Link #1, for instance Internet-ISP-1, that uses interface 1/2:
In this example scenario, WAN Link #1 makes use of an “Existing Home Network” which can be a shared resource with other users in the home (who are considered non-workers). In this situation, there is no way for the SD-WAN to be guaranteed the speeds configured for the egress and ingress rates unless a dedicated internet service is used for the home worker. On a shared line, you can enable the **Adaptive Bandwidth Detection** feature on this WAN link, which is a feature designed for WAN links that provide varying bandwidth. When the device detects loss on that available path, due to the contending traffic, the device uses the WAN link at a reduced bandwidth rate first, and only when the available bandwidth is below the configured **Minimum Acceptable Bandwidth percentage** does the device mark the path as BAD and tries to avoid using it (i.e. use any other available link in good state). You would not be required to enable Adaptive Bandwidth Detection if the internet source is not shared and can operate at the configured speeds.
![Batch WAN Link 1](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchwanlinks1.png)
![Batch WAN Link 1 Advanced](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchwanlinks1advanced.png)
    *  ii. Edit WAN Link #2 (for instance LTE-ATT-2) that uses interface LTE-1:
WAN Link #2 makes use of an LTE network, which is variable in bandwidth rate. Enable the **Adaptive Bandwidth Detection** feature on this WAN link, which is designed for WAN links that provide varying bandwidth. When the device detects loss on that available path, which is typical for wireless transports, the device uses the WAN link at a reduced bandwidth rate first, and only when the available bandwidth is below the configured **Minimum Acceptable Bandwidth percentage**, does the device mark the path as BAD and tries to avoid using it (i.e. use any other available link in good state).
![Batch WAN Link 2](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchwanlinks2.png)
![Batch WAN Link 2 Advanced](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorbatchwanlinks2advanced.png)
1.  Routes:
Generally, defining Static Routes is not be necessary for Home Offices. If needed, here you would configure any statically defined subnets and pint to a LAN Gateway IP, so that the defined subnet would be advertised to peer SD-WAN devices.
1.  Summary:
The summary detail of the site can be reviewed and **Saved**. If the site configuration is not saved, the inputs are lost if you navigate away from the site’s Basic Settings.

## Advanced Site Configuration

With Basic Site configuration complete, we need to ensure some additional configuration items are in place for the on-premises devices to have continued connectivity after the installation has been installed and activated via zero-touch deployment. This can be done at Global configuration, with All Sites selected, under Configuration > Delivery Services > Services & Bandwidth. Internet Service can be enabled by allocating a bandwidth percentage for a WAN link type. By allocating a percentage (for instance 30%) for “Internet Link” types, this automatically configures the internet breakout for any sites that are configured with that WAN link access type.
![Service and Bandwidth](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorserviceandbandwidth.png)
Additionally, when the site configuration for that Access Type (for instance **Public Internet**) is configured and the Security setting for the related interface are set to **Untrusted**, the system automatically creates a Dynamic NAT policy to allow for local internet breakout for the site.

![Home 110 LTE WAN](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorhome110ltewan.png)

![Home 110 LTE WAN](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorhome110ltewanut.png)

If the interface is configured with the Security setting as Trusted, the Dynamic NAT policy must manually be created from the site’s **Configuration > Advanced Settings > NAT** page for the desired Routing Domain.
![Advanced NAT](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratoradvancednat.png)
If the on-premises device require to serve as a DHCP Server for the Home Worker’s LAN subnet, then the feature can be enabled under **Configuration > Advanced Settings > DHCP**.
![Advanced DCHP](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratoradvanceddhcp.png)

## Deploy the Configuration

With the site-specific detail complete, the SD-WAN Administrator can push the configuration through the central management tool. Deploying the latest configuration serves two purposes; 1) The existing SD-WAN devices (for instance MCN) are prepped to allow the incoming Virtual Path connection attempt from the new remote device and 2) The on-premises device packages are made available on the zero-touch deployment Cloud Service to hand down to the on-premises devices that are calling home through the zero-touch deployment process.

To deploy the configuration, make sure **All Sites** is selected, then navigate to the **Configuration > Network Config Home page**. Select the desired software (11.1.1.39, or greater is required if using the 110 platform). Then click **Deploy Config/Software** to stage the configuration and software packages.

![Deploy](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratordeploy.png)

The deployment tracker requires the configuration to be Staged and Activated. The activation completes for the already connected sites, which means those SD-WAN devices is ready and able to accept Virtual Path connection attempts from the new site. Sites that are not connected, wait in the Staging Pending state until the on-site installer performs the zero-touch deployment workflow.

![Stage](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratorstage.png)

With configuration pushed out to the network, the next step is on-site activity for the remote installer is to stand up the device using one of the zero touch deployment methods outlined earlier (1. [Zero-touch deployment via the WAN Interface (Citrix SD-WAN 110-SE)](https://support.citrix.com/article/CTX272229), 2. [Zero-touch deployment via the LTE Interface (Citrix SD-WAN 110-LTE-SE)](https://support.citrix.com/article/CTX272228)).
With in-band management in place and the appropriate Internet connectivity configured through local breakout, or backhaul through the data center, the activation fully completes after a few minutes from when the installer initiates the zero-touch deployment process. At this point the home worker can connect their laptop/PC to the LAN network and begin working from home to resources denoted by the Administrator.

![Activate](/en-us/tech-zone/learn/media/poc-guides_citrix-sdwan-home-office_orchestratoractivate.png)

## Endpoint Management

For an SD-WAN Administrator, remotely managing SD-WAN devices sprawled across different geographic regions is essential for a successful Home Office deployment. Having remote access to the SD-WAN devices through the central management tool is important for configuration, monitoring and troubleshooting.

### In-band Management

In-band management capabilities allow for the data interfaces to carry data and management traffic without having to configure an out-of-band management interface. The in-band management capabilities are leveraged to make the Zero Touch Deployment procedure easier, eliminating the need for on-site installers from having to configure separate management access, or even eliminating the need to access the local web interfaces at all. In-band provisioning was recently introduced to the SD-WAN 110-SE and VPX platforms starting with R11.1.1. For more details refer to
[Orchestrator In-band Management](/en-us/citrix-sd-wan/11-1/inband-and-backup-management.html).

### Fallback Configuration

Fallback configuration is another important feature to help maintain connectivity from the SD-WAN device to Citrix Cloud Services in the event of failures such as license, or software mismatch. Fallback configuration is enabled by default on appliances that have a default configuration profile with a factory image of R11.1.1, or greater. (For more detail refer to
[Orchestrator Fallback Configuration](/en-us/citrix-sd-wan/11-1/inband-and-backup-management.html))

### Delivery Services

Delivery Services can be globally defined to limit SD-WAN connectivity for Internet, Intranet, IPsec, or GRE tunnels. As an example, this can include defining policies local to each remote site to tunnel internet bound traffic through Secure Web Gateway solutions, for instance Citrix Secure Internet Access, which may be a useful feature for the Home Office use-cases. (For more detail refer to
[Orchestrator Delivery Services](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html))

### Routing Policies

Routing policies can be globally defined to enable traffic steering. As an example, this can include direct breakout of O365 traffic for low latency performance. (For more detail refer to
[Orchestrator Routing](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/routing.html))

### Firewall Policies

Firewall policies can be globally defined to limit the Home Users to only business critical applications. As an example, this can include configuring the global firewall settings to drop all traffic, and further configure policies to strictly limit only certain traffic (for instance Citrix Virtual Apps and Desktops) through the Virtual Path Service and O365 traffic through the Internet Service. (For more detail refer to
[Orchestrator Security](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/security.html))

## References

For more information refer to:

[Remote Employee Productivity](https://www.citrix.com/networking/remote-employee-productivity.html)

[Citrix SD-WAN Home Office Tech Brief](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-home-office.html)

[Citrix SD-WAN Home Office Design Decisions](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
