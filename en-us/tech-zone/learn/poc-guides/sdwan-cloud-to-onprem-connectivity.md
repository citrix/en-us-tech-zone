---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Karthick Srivatsan
description: Learn how to implement Citrix SD-WAN rapidly to provide secure, enhanced, and resilient connectivity between your public cloud and data center environments.
tz_title: SD-WAN Cloud-to-Data Center Connectivity
tz_products: citrix-networking;
---
# PoC Guide: SD-WAN Cloud-to-Data Center Connectivity

## Overview

Use of the Cloud to deliver Enterprise services continues to grow. The recent need to work from home is driving a demand for more resources available from anywhere. However, those Cloud services typically still need to make back end calls to Data Center systems such as: service accounts authenticating to Active Directory, applications pulling records from databases, or virtual desktops copying files from shares.

Major cloud providers offer networking options to provide connectivity to data centers. For example Express Routes, or VNet Peering, are available in Microsoft Azure. Citrix SD-WAN can provide differentiated connectivity. Existing Citrix SD-WAN customers can seamlessly extend access across their overlay network, to multiple cloud sites, while providing performance benefits for app and service flows.

Citrix SD-WAN virtual appliances (VPX) can be deployed in Microsoft Azure, Amazon Web Services (AWS), or Google Cloud Platform (GCP) and integrate with Citrix SD-WAN appliances hosted in Data Centers or Private Clouds. The secure tunnel that is established between SD-WAN instances routes traffic between the Cloud and Data Center, while enhancing performance in transit.

Citrix SD-WAN uniquely transfers flows on a per packet basis across a secure UDP tunnel. Using a proprietary header attached to each packet it measures in real-time the connectivity quality including latency, loss, or jitter. Therefore, when multiple links are available it steers flows to find the best path according to quality requirements.

[![Cloud-to-Data Center Connectivity Overview](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_overview.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_overview.png)

## Conceptual Architecture

For this Proof of Concept, we demonstrate establishing connectivity between an Azure tenant environment representing the Cloud location and a Citrix Hypervisor based environment representing the Data Center. The scope is limited to establishing connectivity between test servers hosted on the LAN where the SD-WAN appliances are implemented in each environment.

**Cloud** – an SD-WAN VPX appliance is built on an Azure tenant. It offers four sizes that vary by the number of virtual paths supported. It is also supported on AWS, and GCP public cloud platforms.

1 - Cloud Citrix SD-WAN – the appliance, built in Azure, requires subnet assignments for a Management, LAN, and WAN interface. Their host addresses are dynamically allocated.

2 - Cloud Test Server - a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have its subnet bound to the Azure Route table to reach the Data Center via the appliance.

**Data Center** – an SD-WAN VPX appliance is built on a Citrix Hypervisor. It is also supported other major hypervisors and hardware platforms to meet a broad range of forwarding requirements.

3 - Data Center Citrix SD-WAN – the appliance, built on a Citrix Hypervisor, requires subnet assignments for a Management, LAN, and WAN interface and Internet access to reach the Citrix Orchestrator [Zero-Touch Deployment](/en-us/citrix-sd-wan-orchestrator/zero-touch-deployment.html) service.

4 - Data Center Test Server - a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have a static route to the Cloud LAN, via the SD-WAN appliance to reach the Cloud Test Server.

**Citrix Cloud** – the Orchestrator service is the central system for deployment and management of Citrix SD-WAN networks. SD-WAN appliances automatically contact the Orchestrator service with their serial number to verify manageability.

5 - The Orchestrator service is configured with the serial number of both the Cloud and Data Center SD-WAN instance to establish their cloud connectivity and deploy their configuration details to make their virtual paths available.

[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_conceptualarchitecture.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_conceptualarchitecture.png)

## Cloud Setup

Following are the steps to set up a Citrix SD-WAN VPX in Azure and configure a test server to route through it. Steps pertinent to the set up are documented, while other setup options not mentioned can be left as default or set to your preference.

(NOTE: Citrix SD-WAN is also supported on AWS, or GCP and once the initial setup is done the appliance uses the same configuration process.)

### SD-WAN Appliance Set-up

1.  Log in to the Azure Portal with an administrator account for your tenant.
1.  Search the Marketplace for Citrix SD-WAN Standard Edition and select “Create”.
[![Azure - Marketplace - SD-WAN SE Edition](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuremarketplacesdwanse.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuremarketplacesdwanse.png)

1.  On the first page (Basics) in the workflow enter:
    *  a. Select “Create new”
    *  b. Resource Group – Azure requires using an existing unpopulated group or creating a group
    *  c. Region - where your services are hosted
    *  d. Select “Next” at the bottom of the page

[![Azure - Basics](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbasics.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbasics.png)

1.  On the next page (General settings) enter:
    *  a.  Virtual Machine name
    *  b.  User name and Password [NOTE: To log in into the SD-WAN appliance you use the name “admin” along with the password that you set here.  The user name that you set here is not given admin rights.]
    *  c.  Select “Next” at the bottom of the page
[![Azure - General Settings](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwangeneralsettings.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwangeneralsettings.png)
1.  On the next page (SDWAN Settings) enter:
    *  a. Virtual Machine size – there are 4 options for [VM size](/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html) that determine the maximum number of virtual paths the appliance can support. We leave the default size
    *  b. Virtual network - where your services are hosted (select an existing one or select create new. If you do not have a naming convention for your Azure tenant, it is recommended to use one here to help with configuration and troubleshooting in the future). We create `vnet-sdwanamer`
    *  c. Management subnet – the IP address for the management interface are assigned from this subnet
    *  d. LAN subnet - the IP address for the LAN interface are assigned from subnet
    *  e. WAN subnet - the IP address for the WAN interface are assigned from subnet
    *  f. AUX subnet - the IP address for the AUX interface are assigned from this
    [NOTE: Auto generated subnets are offered, but can be changed as required. AUX subnets only have relevance in Azure SD-WAN HA]
    *  g. Route table name – name of route table object, for the local LAN, used in Azure. We leave the default name "SdWanHaRoute"
    *  h. Route Address Prefix – the address prefix of the data center on the far end for which all traffic within its scope is routed via the SD-WAN instance. This address adds an Azure User Defined Route (UDR) into the local LAN routing table for local LAN routing of hosts via the Azure SD-WAN instance to reach that remote prefix. We enter 192.168.0.0/16
    *  i. Select “Next” at the bottom of the page
[![Azure - SD-WAN Settings](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwansdwansettings.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwansdwansettings.png)
1.  Review + create - review the summary of settings.
    *  a.  Select “Create” at the bottom of the page

### Post Deployment Activities

1.  Once you are notified that the deployment is complete select “Go to Resource” to navigate to the SD-WAN virtual machine detail menu.
[![Azure - SD-WAN - Deployment Complete](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwandeploymentcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwandeploymentcomplete.png)
1.  Navigate to Virtual Machines, check `sdwanamer`, and select “Stop”. The IP addresses assigned by Azure are dynamic by default. It is good practice to set them to static addresses to ensure they do not change and risk interrupting connectivity.  
1.  Navigate to Virtual Machines > select `sdwanamer` > Networking.  For each NIC shown on the top (Mgmt, LAN, WAN) we select the Network Interface link.
[![Azure - SD-WAN - Networking](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwannetworking.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwannetworking.png)

    *  a.  MGMT
       *  a1. Select the private IP address, toggle Assignment to “Static” and select “Save”. (the public IP address will be set to static automatically now)
[![MGMT Networking IpConfig](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwannetworkingmgmtipconfig.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwannetworkingmgmtipconfig.png)
       *  a2. RECORD the MGMT public IP address and document it in your network diagram
[![MGMT IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagrammgmt.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagrammgmt.png)
    *  b. LAN
       *  b1. Select the private IP address, toggle Assignment to “Static” and select “Save”
       *  b2. RECORD the `sdwan-vpx-nic-lan` private IP address and document it in your network diagram
[![LAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramlan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramlan.png)
    *  c.  WAN
       *  c1. Select the private IP address, toggle Assignment to “Static” and select “Save” (the public IP address will be set to static automatically now)
       *  c2. RECORD the `sdwan-vpx-nic-wan` private IP address and public IP address and document them in your network diagram
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramwan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramwan.png)
1.  Navigate to Virtual Machines, check `sdwanamer`, and select “Start”.
1.  Once `sdwanamer` is running again, from a management server open a browser, navigate to the assigned `sdwan-vpx-nic-mgmt` IP address using https and log in with your admin password.
[![Cloud SD-WAN - Browser Log in](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowsergui.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowsergui.png)
1.  Now from the SD-WAN appliance management GUI:
    *  a. Clear the initial warning regarding the license grace period for now
    *  b. On the dashboard page RECORD the appliance serial number
[![Cloud SD-WAN - Browser Dashboard](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowserdashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowserdashboard.png)
 [NOTE: The browser reports “Not Secure” since the site link is using an IP address. You can create a record in your DNS system to eliminate it. Also note that we are accessing the appliance management interface using its public IP address.  Alternatively, you can access using its private IP address, from a jump server on the LAN. Either it is beneficial to configure an Azure rule to limit access to source IP addresses of management hosts.]
    *  c. Navigate to Configuration > Appliance Settings > Network Adapters. Verify the Primary DNS IP address was learned through Azure DHCP and is configured on the appliance
[![Cloud - SD-WAN - DNS](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowseradapter.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresdwanbrowseradapter.png)

### Cloud Test Server

Now we create a Windows Server 2016 Virtual Machine, hosted on the `sdwanamer` VNet, to test connectivity between the Cloud VNet and Data Center LAN.

1.  From the Azure menu select virtual machines and select “Add”.  Choose default settings except for the following.
1.  Basic Settings:
    *  a. Resource Group – enter the resource group where the SD-WAN instance was created
    *  b. Region – where the SD-WAN instance is created (if you select the same region you are offered to use the same VNet by the workflow)
    *  c. Enter user name and password
    *  d. Select “Next” at the bottom of the page and next again under Disk settings
1.  Networking:
    *  a. Virtual Network – select `vnet-sdwanamer`
    *  b. Subnet – select `snet-sdwanamer-lan`  
These settings allow the server to communicate directly with the SD-WAN instance and route through it without any other routing or tunnel constructs in Azure
    *  c.  Select “Review + create” at the bottom of the page
[![Azure - Server - Networking](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresvmnetworking.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azuresvmnetworking.png)
1.  Create – select “Create” after validation passes.
1.  In the search menu at the top enter “route tables” and select the service.
    *  a. Select the “SdWanLANRouteTable” that’s part of the resource group `sdwanamer`
Here we see an entry for the Route Table Name “SdWanHaRoute” and Route Address Prefix “192.168.0.0”, which we assigned in the SD-WAN appliance provisioning workflow.  It has been set to use the SD-WAN appliance LAN subnet interface “10.100.1.4”
[![Azure - LAN User Defined Route](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azureslanroutetable.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azureslanroutetable.png)
    *  b. Select **Subnets > Associate** and from the “Virtual network” drop-down menu select `vnet-sdwanamer` and from the “Subnet” drop-down menu select `snet-sdwanamer-lan`.  Then click “OK” at the bottom of the screen
[![Azure - Associate Subnet](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azureslanroutetablesubnet.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_azureslanroutetablesubnet.png)
1.  Here it’s recommended you also select “Networking” and configure an Azure rule to limit access to source IP addresses of management hosts.

## Data Center Set-up

We demonstrate configuring a Citrix SD-WAN VPX on a Citrix Hypervisor. Citrix SD-WAN also supports software-based virtual appliances on VMware, HyperV, or KVM. Citrix SD-WAN is also offer as hardware appliances with various capabilities which are described in the Citrix SD-WAN Data Sheet. The configuration process is also the same once the initial setup is done.

### SD-WAN Appliance Set-up

1.  Log in to the Citrix downloads site with your Citrix credentials. Under the Citrix SD-WAN section, select the SD-WAN Standard Edition VPX for XenServer, and download the Virtual Appliance.
[![DC - Citrix Hypervisor - XVA Download](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvadownload.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvadownload.png)
1.  Import the .xva file into your Citrix Hypervisor - XenCenter.
[![DC - Citrix Hypervisor - Import](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaimport.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaimport.png)
1.  Enter the required fields to complete the import:
    *  a. Select the target Home Server
[![DC - Citrix Hypervisor - Server](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaserver.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaserver.png)
    *  b. Select the target Storage Repository
[![DC - Citrix Hypervisor - Storage](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvastorage.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvastorage.png)
    *  c. Select network to connect VM – here you add an entry for each SD-WAN interface.  The Network typically maps to a public or private VLAN. [To create a network in Citrix Hypervisor XenCenter select your target XenServer instances Networking tab]
[![DC - Citrix Hypervisor - Network](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvanetwork.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvanetwork.png)
1.  Once the Citrix SD-WAN virtual machine boots select the “Log in” tab and login with the default credentials admin/password where you are prompted to change the default password.
[![DC SD-WAN Console](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaconsole.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverxvaconsole.png)
There are two options for the appliance to obtain management addressing:
    *  a. By default, it makes a DHCP request and if it successfully acquires an IP address, mask, gateway, and primary DNS the initial setup is complete
    *  b. To manually set the appliance details through the console with the default username/password of "admin/password" (it is recommended to change the password at your earliest opportunity)
1.  From a management server on the network open a browser, navigate to the assigned `sdwan-vpx-nic-mgmt` IP address and log in as admin.
[![DC SD-WAN - Browser Log in](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverbrowserlogin.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverbrowserlogin.png)
1.  Now from the SD-WAN appliance management GUI:
    *  a. Clear the initial warning regarding the license grace period for now, yet be sure to upload your permanent license as soon as possible
    *  b. On the dashboard page RECORD the appliance serial number
    *  c. Navigate to Configuration > Appliance Settings > Network Adapters and enter the Primary DNS IP address and select “Change settings"
[![DC SD-WAN DNS](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverbrowserdns.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverbrowserdns.png)
    *  d. RECORD the `sdwan-vpx-nic-mgmt` private IP address and document your network diagram

At this point you need to plan, allocate, and document the remaining IP address assignments for the Data Center SD-WAN instance MGMT, LAN, and WAN interfaces.

### Data Center Test Server

On our Data Center LAN, we have a Windows **Server** to test connectivity.

1.  Log in as Administrator.
1.  Open as MS-DOS prompt as Administrator.
1.  Add a persistent route to reach the Cloud `sdwan-vpx-nic-lan` via the SD-WAN appliance `sdwan-vpx-nic-lan` interface
“route -p add 10.100.0.0 mask 255.255.255.0 192.168.64.132”
[![DC Test Server - Static Route](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverserverroute.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_xenserverserverroute.png)

## Cloud-to-Data Center Connectivity Configuration

### Prerequisites

One essential parts of setting up Cloud-to-Data Center Connectivity is to ensure the necessary firewall port are opened.

*  TCP 443 – used by SD-WAN instances outbound to contact the [Zero-Touch Deployment] (/en-us/citrix-sd-wan-orchestrator/zero-touch-deployment.html) configuration service.
*  UDP 4980 – used by SD-WAN virtual paths bi-directionally to set up Virtual Paths between instances to exchange routing information, forward data flows, and so on.  Refer to the [Citrix SD-WAN Reference Architecture](/en-us/tech-zone/design/reference-architectures/sdwan.html) for more information.

### Orchestrator

Citrix Orchestrator, hosted in Citrix Cloud, is the central service for managing and monitor Citrix SD-WAN networks. Log in to your Citrix Cloud account and request a trial or navigate to [Citrix Cloud Onboarding](https://onboarding.cloud.com/) to create an account.

[![Citrix Cloud OnBoarding](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudonboarding.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudonboarding.png)
If you create a Citrix Cloud account, you receive an email with a link to log in for the first time.

[![Citrix Cloud OnBoarding Complete](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudonboardingcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudonboardingcomplete.png)

After you have logged into your Citrix Cloud account find the Orchestrator icon and select “Request Trial”
[![Citrix Cloud Trial](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudrequesttrial.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudrequesttrial.png)
You receive a notice that the account is being provisioned and confirmation when provisioning is complete.
[![Orchestrator Provisioned](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorprovisioned.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorprovisioned.png)
Select “Go back to Launchpad”

### Configuration

With the zero-touch configuration capabilities appliances can be rapidly configured and deployed with Orchestrator’s easy to use workflow. Using the details, we recorded in our network diagram, we configure Cloud-to-Data Center Connectivity in the Orchestrator.

[![Complete Network Diagram](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_diagramcomplete.png)

1.  From the main Citrix Cloud admin page select “Manage” Orchestrator service.
[![Orchestrator service](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratormanage.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratormanage.png)
1.  You are brought to the Orchestrator service dashboard.
[![Orchestrator Dashboard](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordashboard.png)

### Cloud site

First, we configure the SD-WAN Cloud appliance hosted in Azure.

1.  Select “Add Site” and after submitting site name and location a configuration workflow starts:
    *  a. Site Name - `sdwanamercloud`
    *  b. Street Address – where the SD-WAN appliance is located. We enter Miami, FL
[![Orchestrator - Cloud - New Site](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudnewsite.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudnewsite.png)
1.  Update the Site Details populated with default values:
    *  a. Device Model – select VPX
    *  b. ARP Timer – for cloud environments increase this timer to 5000 ms
[![Orchestrator - Cloud - Site Details](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudsitedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudsitedetails.png)
Click Next
1.  Enter Device Details:
    *  a. Primary Device Serial Number – refer to your network map
    *  b. Short Name – enter “Cloud”
[![Orchestrator - Cloud - Device Details](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorclouddevicedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorclouddevicedetails.png)
Click Next
1.  Add Interfaces:
    *  a. Select “+” plus to add the `sdwan-vpx-nic-lan` interface
    *  a1. Keep Deployment Mode as “Edge(Gateway)”
    *  a2. Keep Interface Type as “LAN”
    *  a3. Keep Security as “Trusted”
    *  a4. Select Physical Interface “1”
    *  a5. Enter Primary IP with CIDR mask – refer to your network map
    *  a6. Select Done twice
[![Orchestrator - Cloud - LAN Interfaces](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfaceslan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfaceslan.png)
    *  b. Select “+” plus to add the WAN interface
    *  b1. Keep Deployment Mode as “Edge(Gateway)”
    *  b2. Keep Interface Type as “WAN”
    *  b3. Set Security to “Untrusted”
    *  b4. Select Physical Interface “2”
    *  b5. Enter Primary IP with CIDR mask – refer to your network map
    *  b6. Select Done twice
[![Orchestrator - Cloud - WAN Interfaces](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfaceswan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfaceswan.png)
Click Next
[![Orchestrator - Cloud - Interfaces Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfacessummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudinterfacessummary.png)
1.  Enter WAN Link:
    *  a. Select “+” to add the WAN Link
    *  a1. Select “Done” on the Popup to create a Wan Link
    *  a2. ISP Name - select “MSN Dial-up”
    *  a3. Public IP Address – refer to your network map
    *  a4. Egress and Ingress Speed – set these speeds to 20 Mbps (Until you are able to apply your license for higher speeds.)
    *  a5. Virtual Interface – select “VIF-2-WAN-1” created in the Interfaces section
    *  a6. IP Address – the IP automatically is populated based on the Virtual Interface
    *  a7. Gateway IP Address – verify in Azure.  It is usually 0.1 in the last octet of the subnet
    *  a8. Select Done twice
[![Orchestrator - Cloud - Wan Links](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudwanlinks.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudwanlinks.png)
Click Next
[![Orchestrator - Cloud - Wan Links Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudwanlinkssummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudwanlinkssummary.png)
1.  Routes – we leave routes blank.  For this POC the `sdwan-vpx-nic-lan` subnets will automatically be exchanged by the SD-WAN instances to establish connectivity between our test servers. To extend routing beyond the LANs discuss the requirements for static or dynamic routing with your network team and refer to the SD-WAN [routing](/en-us/citrix-sd-wan/11-2/routing.html) documentation.
1.  Verify the configuration details on the Summary page, then click Save and Done.
[![Orchestrator - Cloud - Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudsummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudsummary.png)
1.  In the Network Configuration Home, you see the entry for the `sdwanamercloud` site Cloud Connectivity column change from a gray circle that says “offline” to a green circle that says “online”. If this change does not happen within 1 minute refer to the Troubleshooting section to investigate.
[![Orchestrator - Cloud - Connectivity](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudconnectivity.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorcloudconnectivity.png)

### Data Center site

Next, we configure the SD-WAN Data Center appliance hosted on a Citrix Hypervisor.

1.  Select “Add Site” and after submitting site name and location a configuration workflow starts:
    *  a. Site Name - `sdwanamerdc`
    *  b. Street Address – where the SD-WAN appliance is located.  We enter Miami, FL, USA
[![Orchestrator - DC - New Site](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcnewsite.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcnewsite.png)
1.  Update the Site Details populated with default values:
    *  a. Device Model – select VPX
[![Orchestrator - DC - Site Details](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcsitedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcsitedetails.png)
Click Next
1.  Enter Device Details:
    *  a. Primary Device Serial Number – refer to your network map
    *  b. Short Name – enter “DC”
[![Orchestrator - DC - Device Details](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcdevicedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcdevicedetails.png)
Click Next
1.  Add Interfaces:
    *  a. Select “+” plus to add the LAN interface
    *  a1. Keep Deployment Mode as “Edge(Gateway)”
    *  a2. Keep Interface Type as “LAN”
    *  a3. Keep Security as “Trusted”
    *  a4. Select Physical Interface “1”
    *  a5. Enter Primary IP with CIDR mask – refer to your network map
    *  a6. Select Done twice
[![Orchestrator - DC - LAN Interfaces](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfaceslan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfaceslan.png)
    *  b. Select “+” plus to add the WAN interface
    *  b1. Keep Deployment Mode as “Edge(Gateway)”
    *  b2. Keep Interface Type as “WAN”
    *  b3. Set Security to “Untrusted”
    *  b4. Select Physical Interface “2”
    *  b5. Enter Primary IP with CIDR mask – refer to your network map
    *  b6. Select Done twice
[![Orchestrator - DC - WAN Interfaces](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfaceswan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfaceswan.png)
Click Next
[![Orchestrator - DC - Interfaces Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfacessummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcinterfacessummary.png)
1.  Enter WAN Link:
    *  a. Select “+” to add the WAN Link
    *  a1. Select “Done” on the Popup to create a Wan Link
    *  a2. ISP Name - select “Verizon”
    *  a3. Public IP Address – refer to your network map
    *  a4. Egress and Ingress Speed – set these speeds to 20 Mbps (Until you are able to apply your license for higher speeds.)
    *  a5. Virtual Interface – select “VIF-2-WAN-1” created in the Interfaces section
    *  a6. IP Address – the IP is automatically populated based on the Virtual Interface
    *  a7. Gateway IP Address – verify with your network team.  It is often 0.1 in the last octet of the network
    *  a8. Select Done twice
[![Orchestrator - DC - Wan Links](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcwanlinks.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcwanlinks.png)
Click Next
[![Orchestrator - DC - Wan Links Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcwanlinkssummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcwanlinkssummary.png)
1.  Routes – we leave routes blank.  For this POC the `sdwan-vpx-nic-lan` subnets is automatically exchanged by the SD-WAN instances to establish connectivity between our test servers. To extend routing beyond the LANs discuss the requirements for static or dynamic routing with your network team and refer to the SD-WAN Routing documentation.
1.  Verify the configuration details on the Summary page, then click Save and Done.
[![Orchestrator - DC - Summary](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcsummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcsummary.png)
1.  In the Network Configuration Home, you see the entry for the `sdwanamerdc` site Cloud Connectivity column change from a gray circle that says “offline” to a green circle that says “online”. If this does not happen within 1 minute refer to the Troubleshooting section to investigate.
[![Orchestrator - DC - Connectivity](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcconnectivity.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcconnectivity.png)

### Provisioning

Now that both sites are configured and online, we can start the provisioning process.  

1.  Since there are new installations, we start by upgrading the Software version. From the Software Version drop-down menu select the latest version at the bottom of the list. Select “Proceed” on the popup confirming the update.
1.  Now select “Deploy/Configure” and select “Stage”. If there are any configuration issues you are notified here. To resolve them from the left menu navigate to **Configuration > Network Home** and select the SD-WAN appliance to return to the configuration workflow.  If you make changes be sure to click save and then return to “Deploy/Configure” and select “Stage”. The staging process reports back while downloading and confirm when staging is complete.
1.  Now select “Activate” to make live the downloaded software and configuration details on each SD-WAN appliance. The activation process reports on its progress and confirm “Activation Complete” for each appliance.
[![Orchestrator Provisioning - Activate](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorprovisioningactivate.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorprovisioningactivate.png)

Now select "Network Config Home" under the **Configuration** menu on the left to see the sites status become available as their virtual path is established.

[![Orchestrator - DC - Availability](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcconnectivity.gif)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordcconnectivity.gif)

### Verification

Now that our sites are upgraded, configured, and online we can verify connectivity between the SD-WAN appliances and between the test servers on their respective LANs.

1.  First select “Dashboard” and you now see the entries for `sdwanamerdc` and `sdwanamercloud` with a green square in the Availability column. This color indicates that the Virtual Path is established between the 2 sites. If the state is another color refer to the Troubleshooting section to investigate.
[![Orchestrator Network Dashboard](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratornetworkdashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratornetworkdashboard.png)
1.  Verify connectivity:
    *  a. First select the `sdwanamerdc` site
    *  b. Notice the green link between `sdwanamerdc` and `sdwanamercloud` confirming the virtual path is up
    *  c. Select “Devices” and verify the status is “Up” for both interfaces
[![Orchestrator Dashboard Devices](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorsitedashboarddevices.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorsitedashboarddevices.png)
1.  Select “Troubleshooting” from the menu on the left
    *  a. Enter the IP Address of the Cloud SD-WAN `sdwan-vpx-nic-lan` IP address and select “Run”.  Verify the pings are successful to confirm connectivity from the DC SD-WAN appliance.
[![SD-WAN Ping](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordiagnosticsping.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratordiagnosticsping.png)
    *  b. Select **Reports > Real Time > Statistics > Routes >** "Retrieve latest data". Notice that routes to the Cloud SD-WAN Cloud `sdwan-vpx-nic-lan` and WAN subnets have been learned dynamically via the SD-WAN Virtual Path show by the protocol type "Virtual_WAN"
[![SD-WAN Routes](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorstatisticsroutes.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_citrixcloudorchestratorstatisticsroutes.png)
1.  From the Data Center Test Server open an MS-DOS prompt and verify the traceroute to the Cloud Test Server.
[![Ping DC to Cloud Test Server](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_pingdctocloudtestserver.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity_pingdctocloudtestserver.png)

### Routing

Note that for this POC Guide we focused on establishing connectivity between endpoints located on the SD-WAN appliances local LANs. Both Citrix SD-WAN and Cloud platforms like Azure have many diverse options to extend routing across networks dynamically as required.

See the Citrix SD-WAN documentation for more information regarding dynamic [routing](/en-us/citrix-sd-wan/11-2/routing.html).

See Azure documentation for more information regarding [virtual network traffic routing](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).

## Troubleshooting

The Orchestrator service dashboard provides a summary of the overall network health including alerts, QOS, and Connectivity details.  It is a good place to start Troubleshooting.

### Orchestrator Connectivity

Verify that [Citrix SD-WAN system requirements](/en-us/citrix-sd-wan/11-2/updating-upgrading/system-requirements.html) have been met and check external firewall rules to ensure required ports are open.

*  Cloud Connectivity – if the “Cloud Connectivity” state is not green for a site verify that it can send outbound traffic via the internet to TCP 443 and verify the primary DNS can resolve [Zero-Touch Deployment](/en-us/citrix-sd-wan/11-2/use-cases-sd-wan-virtual-routing/zero-touch-deployment-service.html) service domains.
*  Virtual Paths – if the Availability state is not green for a site verify that it can send and receive UDP traffic on port 4980. This path is an essential requirement to set up virtual paths and establish the overlay network.

### Orchestrator Reports

Navigate to **Orchestrator > Reports > RealTime** to retrieve the latest data on key statistics including:

*  Routing – verify the Data Center SD-WAN appliance has learned the Cloud `sdwan-vpx-nic-lan` route and the Cloud SD-WAN appliance has learned the Data Center `sdwan-vpx-nic-lan` route
*  Firewall Connections – check flows between location endpoints

### Orchestrator Diagnostics

Navigate to **Orchestrator> Troubleshooting > Diagnostics** to troubleshooting connectivity:

*  Ping/Traceroute – to verify basic connectivity and path details
*  Packet Capture – capture protocol and TCP/IP flow details

## Summary

Citrix SD-WAN can be set up rapidly to provide secure and resilient connectivity between your public cloud and data center environments. It also enhances traffic delivery performance during transport.  

While this guide focused on point-to-point connectivity, the SD-WAN network can be extended to large branches or small work from home environments.  It supports dynamical routing protocols, supports integration with leading firewall vendors, and can apply business rules to steer traffic to thousands of internet services efficiently.  To learn more about Citrix SD-WAN pricing and packing visit the [Citrix website](https://www.citrix.com/) and to learn more about its technical capabilities visit [Citrix Tech Zone](/en-us/tech-zone/).
