---
layout: doc
description: Steps to implement a Proof of Concept of Citrix SD-WAN Cloud-to-Data Center Connectivity
---
# Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity

## Contributors

**Author:** [Matt Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Karthick Srivatsan

## Overview

Use of the Cloud to deliver Enterprise services continues to grow. The recent need for significant portions of the workforce to work from home is driving demand for more scalable resources available from anywhere on demand.  However, those Cloud services typically still need to make backend calls to Data Centers systems such as; service account authenticating to Active Directory, applications pulling records from databases, or virtual desktops copying files from shares.

Networking options like Express Routes, or Vnet Peering, available in Microsoft Azure for example, have disadvantages including complexity, and cost.  Also, they are limited to passing point-to-point traffic without providing performance benefits, can require complex integration to expand their use, and are not portable between public cloud platforms.

Citrix SD-WAN virtual appliances (VPX) may be deployed in Microsoft Azure, Amazon Web Services (AWS), or Google Cloud Platform (GCP) and integrate with Citrix SD-WAN appliances hosted in Data Centers or Private Clouds. The “virtual path” or secure tunnel that is established between SD-WAN instances seamlessly routes traffic between the Cloud and Data Center, across the Internet, while enhancing performance in transit.

Citrix SD-WAN uniquely transfers flows on a per packet basis across a secure UDP tunnel.  Using the proprietary header attached to each packet it measures in real-time the connectivity quality including latency, loss, or jitter. Therefore, when multiple links are available it steers flows to find the best path according to quality requirements.

[![Cloud-to-Data Center Connectivity Overview](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)

## Conceptual Architecture

For this Proof of Concept, we will demonstrate establishing connectivity between an Azure tenant environment representing the Cloud location and a Citrix Hypervisor based environment representing the Data Center location. The scope is limited to establishing connectivity between test servers hosted on the LAN where the SD-WAN appliances are implemented in each environment respectively.

**Cloud** – a SD-WAN VPX appliance will be built on an Azure tenant. It offers four sizes that vary by the number of virtual paths supported. It is also supported on AWS, and GCP public cloud platforms.

1 - Cloud Citrix SD-WAN –  the appliance, built in Azure,  requires subnet assignments for a Management, LAN, and WAN interface. Their host addresses will be dynamically allocated.

2 - Cloud Test Server - a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have its subnet bound to the Azure Route table to reach the Data Center via the appliance.

**Data Center** – a SD-WAN VPX appliance will be built on a Citrix Hypervisor. It is also supported other major hypervisors and hardware platforms to support a broad range for forwarding requirements.

3 - Data Center Citrix SD-WAN –  the appliance, built on a Citrix Hypervisor,  requires subnet assignments for a Management, LAN, and WAN interfaces and Internet access to reach the Orchestrator Citrix [Zero Touch Deployment (ZTD)](https://docs.citrix.com/en-us/citrix-sd-wan-orchestrator/zero-touch-deployment.html) service.

4 - Data Center Test Server -  a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have a static route to the Cloud LAN, via the SD-WAN appliance in order to reach the Test Server

**Citrix Cloud** – the Orchestrator service is the central system for deployment and management of Citrix SD-WAN networks. SD-WAN appliances automatically contact the Orchestrator service with their serial number to verify manageability.

5 - Orchestrator service – will be configured with the serial number of both the Cloud and Data Center SD-WAN instance to establish their cloud connectivity and deploy their configuration details to make their virtual paths available.

[![Cloud-to-Data Center Connectivity Overview](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-conceptualarchitecture.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-conceptualarchitecture.png)

## Cloud Setup

Following are the steps to setup a Citrix SD-WAN VPX in Azure and configure a test server to route through it. Steps pertinent to the setup are documented, while other setup options not mentioned may be left as default or set to your preference.

(NOTE: Citrix SD-WAN is also supported on AWS, or GCP and once the initial setup is done the appliance uses the same configuration process.)

### SD-WAN Appliance Setup

1.  Login to Azure Portal with an administrator account for your tenant
1.  Search the Marketplace for Citrix SD-WAN Standard Edition and select “Create”
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuremarketplacesdwanse.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuremarketplacesdwanse.png)
1.  On the first page (Basics) in the workflow enter:
    *  a.  Select “Create new”.
    *  b.  Resource Group – Azure requires using an existing unpopulated group or creating a new one
    *  c. Region - where your services will be hosted
    *  d. Select “Next” at the bottom of the page
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbasics.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbasics.png)
1.  On the next page (General settings) enter:
    *  a.  Virtual Machine name
    *  b.  Username and Password
[NOTE: To login into the SD-WAN appliance you use the name “admin” along with the password that you set here.  The username that you set here is not given admin rights.]
    *  c.  Select “Next” at the bottom of the page
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwangeneralsettings.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwangeneralsettings.png)
1.  On the next page (SDWAN Settings) enter:
    *  a. Virtual Machine size – there are 4 options for [VM size](https://docs.citrix.com/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html) that determine the maximum number of virtual paths the appliance can support. We will leave the default size.
    *  b. Virtual network - where your services will be hosted (select an existing one or select create new. If you do not have a naming convention for your Azure tenant, it is recommended to use one here to help with configuration and troubleshooting in the future). We will create "vnet-sdwanamer"
    *  c. Management subnet – the IP address for the management interface will be assigned from this
    *  d. LAN subnet - the IP address for the LAN interface will be assigned from this
    *  e. WAN subnet - the IP address for the WAN interface will be assigned from this
    *  f. AUX subnet - the IP address for the AUX interface will be assigned from this
    [NOTE: Auto generated subnets are offered, yet may be changed as required and AUX subnet will only have relevance in Azure SD-WAN HA]
    *  g. Route table name – name of route table object, for the local LAN, used in Azure. We will leave the default name "SdWanHaRoute"
    *  h. Route Address Prefix – the address prefix of the data center on the far end for which all traffic within its scope will be routed via the SD-WAN instance. This will add an Azure User Defined Router (UDR) into the local LAN routing table for local LAN routing of hosts via the Azure SD-WAN instance to reach that remote prefix. We will enter 192.168.0.0/16
    *  i. Select “Next” at the bottom of the page
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwansdwansettings.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwansdwansettings.png)
1.  Review + create - review the summary of settings
    *  a.  Select “Create” at the bottom of the page

### Post Deployment Activities

1.  Once you are notified that the deployment is complete select “Go to Resource” to navigate to the SD-WAN virtual machine detail menu.
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwandeploymentcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwandeploymentcomplete.png)
1.  Navigate to Virtual Machines, check sdwanamer, and select “Stop”. The IP addresses assigned by Azure are dynamic by default. It is good practice to set them to static addresses to ensure they do not change and risk interrupting connectivity.  
1.  Navigate to Virtual Machines > select “sdwanamer” > Networking.  For each Nic shown on the top (Mgmt, LAN, WAN) we will select the Network Interface link.
[![Cloud-to-Data Center Connectivity Conceptual Architecture](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwannetworking.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwannetworking.png)

    *  a.  MGMT
       *  a1.  Select the private IP address, toggle Assignment to “Static” and select “Save”. (the public IP address should be set to static automatically now)
[![MGMT Networking Ipconfig](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwannetworkingmgmtipconfig.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwannetworkingmgmtipconfig.png)
       *  a2. RECORD the MGMT public IP address and document it in your network diagram
[![MGMT IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagrammgmt.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagrammgmt.png)
    *  b.  LAN
       *  b1.  Select the private IP address, toggle Assignment to “Static” and select “Save”
       *  b2. RECORD the sdwan-vpx-nic-lan private IP address and document it in your network diagram
[![MGMT IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramlan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramlan.png)
    *  c.  WAN
       *  c1.  Select the private IP address, toggle Assignment to “Static” and select “Save” (the public IP address should be set to static automatically now)
       *  c2. RECORD the sdwan-vpx-nic-wan private IP address and public IP address and document them in your network diagram
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramwan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramwan.png)
1.  Navigate to Virtual Machines, check sdwanamer, and select “Start”
1.  Once sdwanamer is running again, from a management server open a browser, navigate to the assigned sdwan-vpx-nic-mgmt IP address using https and login with your admin password.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowsergui.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowsergui.png)
1.  Now from the SD-WAN appliance management GUI:
    *  a.  Clear the initial warning regarding the license grace period for now
    *  b.  On the dashboard page RECORD the appliance serial number
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowserdashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowserdashboard.png)
 [NOTE: The browser will report “Not Secure” since the site link is using an IP address. You can create a record in your DNS system to eliminate it. Also note that we are accessing the appliance management interface using its public IP address.  Alternatively, you can access using its private IP address, from a jump server on the LAN. Either way you should configure an Azure rule to limit access to source IP addresses of management hosts.]
    *  c.  Navigate to Configuration > Appliance Settings > Network Adapters and verify the Primary DNS IP address was learned through Azure DHCP and is configured on the appliance
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowseradapter.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresdwanbrowseradapter.png)

### Cloud Test Server

Now we will create a Windows Server 2016 Virtual Machine, hosted on the sdwanamer Vnet, to test connectivity between the Cloud Vnet and Data Center LAN.

1.  From the Azure menu select virtual machines and select “Add”.  Chose default settings except for the following.
1.  Basic Settings
    *  a.  Resource Group – enter the resource group where the SD-WAN instance was created
    *  b.  Region – where the SD-WAN instance is created (if you do not select the same region you will not be offered to use the same vNet by the workflow)
    *  c.  Enter username and password
    *  d.  Select “Next” at the bottom of the page and next again under Disk settings
1.  Networking
    *  a.  Virtual Network – select vnet-sdwanamer
    *  b.  Subnet – select snet-sdwanamer-lan
This will allow the server to communicate directly with the SD-WAN instance and route through it without any other routing or tunnel constructs in Azure
    *  c.  Select “Review + create” at the bottom of the page
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresvmnetworking.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azuresvmnetworking.png)
1.  Create – select “Create” after validation passes
1.  In the search menu at the top enter “route tables” and select the service
    *  a.  Select the “SdWanLANRouteTable” that’s part of the resource group “sdwanamer”
Here we will see an entry for the Route Table Name “SdWanHaRoute” and Route Address Prefix “192.168.0.0”, which we assigned in the SD-WAN appliance provisioning workflow.  It has been set to use the SD-WAN appliance LAN subnet interface “10.100.1.4”
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azureslanroutetable.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azureslanroutetable.png)
    *  b.  Select Subnets > Associate and from the “Virtual network” dropdown menu select “vnet-sdwanamer” and from the “Subnet” dropdown menu select “snet-sdwanamer-lan”.  Then click “Ok” at the bottom of the screen
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azureslanroutetablesubnet.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-azureslanroutetablesubnet.png)
1.  Here it’s recommended you also select “Networking” and configure an Azure rule to limit access to source IP addresses of management hosts

## Data Center Setup

We will demonstrate configuring a Citrix SD-WAN VPX on a Citrix Hypervisor. Citrix SD-WAN also supports software-based virtual appliances on VmWare, HyperV, or KVM. Citrix SD-WAN is also offer as hardware appliances with a variety of capabilities which are described in the Citrix SD-WAN Data Sheet. The configuration process is also the same once the initial setup is done.

### SD-WAN Appliance Setup

1.  Login to the Citrix downloads site with your Citrite credentials. Under the Citrix SD-WAN section, select the SD-WAN Standard Edition VPX for XenServer, and download the Virtual Appliance
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvadownload.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvadownload.png)
1.  Import the .xva file into your Citrix Hypervisor - XenCenter
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvaimport.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvaimport.png)
1.  Enter required fields to complete the import:
    *  a.  Select the target Home Server
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvaserver.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvaserver.png)
    *  b.  Select the target Storage Repository
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvastorage.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvastorage.png)
    *  c.  Select network to connect VM – here you will add an entry for each SD-WAN interface.  The Network typically maps to a public or private vlan. [To create a network in Citrix Hypervisor XenCenter select your target XenServer instances Networking tab]
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvanetwork.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverxvanetwork.png)
1.  Once the Citrix SD-WAN virtual machine boots select the “Login” tab and login with the default credentials admin/password where you will be prompted to change the default password
There are two options for the appliance to obtain management addressing:
    *  a.  By default, it will make a DHCP request and if it successfully acquires an IP address, mask, gateway, and primary DNS the initial setup will be complete
    *  b.  To manually set the appliance details through the console with the default username/password of "admin/password" (the password should be changed at your earliest opportunity)
1.  From a management server on the network open a browser, navigate to the assigned sdwan-vpx-nic-mgmt IP address and login as admin
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverbrowserlogin.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverbrowserlogin.png)
1.  Now from the SD-WAN appliance management GUI:
    *  a.  Clear the initial warning regarding the license grace period for now, yet be sure to upload your permenant license as soon as possible
    *  b.  On the dashboard page RECORD the appliance serial number
    *  c.  Navigate to Configuration > Appliance Settings > Network Adapters and enter the Primary DNS IP address and select “Change settings"
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverbrowserdns.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverbrowserdns.png)
    *  d.  RECORD the sdwan-vpx-nic-mgmt private IP address and document your network diagram

At this point you will need to plan, allocate, and document the remaining IP address assignments for the Data Center SD-WAN instance MGMT, LAN, and WAN interfaces.

### Data Center Test Server

On our Data Center LAN, we have a Windows Server to test connectivity.

1.  Login as Administrator
1.  Open as MS-DOS prompt as Administrator
1.  Add a persistent route to reach the Cloud sdwan-vpx-nic-lan via the SD-WAN appliance sdwan-vpx-nic-lan interface
“route -p add 10.100.0.0 mask 255.255.255.0 192.168.64.132”
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverserverroute.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-xenserverserverroute.png)

## Cloud-to-Data Center Connectivity Configuration

### Prerequisites

One essential parts of setting up Cloud-to-Data Center Connectivity is to ensure necessary firewall port are opened.

*  TCP 443 – used by SD-WAN instances outbound to contact the [Zero Touch Deployment (ZTD)](https://docs.citrix.com/en-us/citrix-sd-wan-orchestrator/zero-touch-deployment.html) configuration service.
*  UDP 4980 – used by SD-WAN virtual paths bi-directionally to setup Virtual Paths between instances to exchange routing information, forward data flows, etc.  Refer to the [Citrix SD-WAN Reference Architecture](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/sdwan.html) for more information.

### Orchestrator

Citrix Orchestrator, hosted in Citrix Cloud, is the central service for managing and monitor Citrix SD-WAN networks. Login to your Citrix Cloud account and request a trial or navigate to [Citrix Cloud Onboarding](https://onboarding.cloud.com/) to create a new account.

[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudonboarding.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudonboarding.png)
If you created a new Citrix Cloud account, you will receive an email with a link to login for the first time.

[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudonboardingcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudonboardingcomplete.png)

After you have logged into your Citrix Cloud account find the Orchestrator icon and select “Request Trial”
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudrequesttrial.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudrequesttrial.png)
You will see a notice that the account is being provisioned and confirmation when provisioning is complete.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorprovisioned.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorprovisioned.png)
Select “Go back to Launchpad”

### Configuration

With the zero-touch configuration capabilities appliances may be rapidly configured and deployed with Orchestrator’s easy to use workflow. Using the details, we recorded in our network diagram, we will configure Cloud-to-Data Center Connectivity in Orchestrator.

[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramcomplete.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-diagramcomplete.png)

1.  From the main Citrix Cloud admin page select “Manage” Orchestrator service
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratormanage.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratormanage.png)
1.  You’ll be brought to the Orchestrator service dashboard.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordashboard.png)

### Cloud site

First, we will configure the SD-WAN Cloud appliance hosted in Azure.

1.  Select “Add Site” and after submitting site name and location a configuration workflow will start
    *  a. Site Name - sdwanamercloud
    *  b. Street Address – where the SD-WAN appliance is located. We will enter Miami, FL
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudnewsite.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudnewsite.png)
1.  Update the Site Details populated with default values:
    *  a. Device Model – select VPX
    *  b. Arp Timer – for cloud environments increase this to 5000ms
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudsitedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudsitedetails.png)
Click Next
1.  Enter Device Details
    *  a. Primary Device Serial Number – refer to your network map
    *  b. Short Name – enter “Cloud”
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorclouddevicedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorclouddevicedetails.png)
Click Next
1.  Add Interfaces
    *  a. Select “+” plus to add the sdwan-vpx-nic-lan interface
    *  a1. Keep Deployment Mode as “Edge(Gateway)”
    *  a2. Keep Interface Type as “LAN”
    *  a3. Keep Security as “Trusted”
    *  a4. Select Physical Interface “1”
    *  a5. Enter Primary IP with CIDR mask – refer to your network map
    *  a6. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudinterfaceslan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudinterfaceslan.png)
    *  b. Select “+” plus to add the WAN interface
    *  b1. Keep Deployment Mode as “Edge(Gateway)”
    *  b2. Keep Interface Type as “WAN”
    *  b3. Set Security to “Untrusted”
    *  b4. Select Physical Interface “2”
    *  b5. Enter Primary IP with CIDR mask – refer to your network map
    *  b6. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudinterfaceswan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudinterfaceswan.png)
Click Next
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudinterfacessummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudinterfacessummary.png)
1.  Enter WAN Link
    *  a. Select “+” to add the WAN Link
    *  a1. Select “Done” on the Popup to create a new Wan Link
    *  a2. ISP Name - select “MSN Dial up”
    *  a3. Public IP Address – refer to your network map
    *  a4. Egress and Ingress Speed – set these to 20 Mpbs (Until you are able to apply your license for higher speeds.)
    *  a5. Virtual Interface – select “VIF-2-WAN-1” created in the Interfaces section
    *  a6. IP Address – the IP will automatically be populated based on the Virtual Interface
    *  a7. Gateway IP Address – verify in Azure.  It is usually .1 in the last octet of the subnet
    *  a8. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudwanlinks.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudwanlinks.png)
Click Next
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudwanlinkssummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudwanlinkssummary.png)
1.  Routes – we will leave routes blank.  For this POC the sdwan-vpx-nic-lan subnets will automatically be exchanged by the SD-WAN instances to establish connectivity between our test servers. To extend routing beyond the LANs discuss the requirements for static or dynamic routing with your network team and refer to SD-WAN [routing](https://docs.citrix.com/en-us/citrix-sd-wan/11-1/routing.html) documentation
1.  Verify the configuration details the on the Summary page, then click Save and Done
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudsummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudsummary.png)
1.  In the Network Configuration Home, you should see the entry for the sdwanamercloud site Cloud Connectivity column change from a gray circle that says “offline” to a green circle that says “online”. If this does not happen within 1 minute refer to the Troubleshooting section to investigate
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorcloudconnectivity.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorcloudconnectivity.png)

### Data Center site

Next, we will configure the SD-WAN Data Center appliance hosted on a Citrix Hypervisor.

1.  Select “Add Site” and after submitting site name and location a configuration workflow will start
    *  a. Site Name - sdwanamerdc
    *  b. Street Address – where the SD-WAN appliance is located.  We will enter Miami, FL, USA
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcnewsite.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcnewsite.png)
1.  Update the Site Details populated with default values:
    *  a. Device Model – select VPX
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcsitedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcsitedetails.png)
Click Next
1.  Enter Device Details
    *  a. Primary Device Serial Number – refer to your network map
    *  b. Short Name – enter “DC”
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcdevicedetails.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcdevicedetails.png)
Click Next
1.  Add Interfaces
    *  a. Select “+” plus to add the lan interface
    *  a1. Keep Deployment Mode as “Edge(Gateway)”
    *  a2. Keep Interface Type as “LAN”
    *  a3. Keep Security as “Trusted”
    *  a4. Select Physical Interface “1”
    *  a5. Enter Primary IP with CIDR mask – refer to your network map
    *  a6. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcinterfaceslan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcdinterfaceslan.png)
    *  b. Select “+” plus to add the WAN interface
    *  b1. Keep Deployment Mode as “Edge(Gateway)”
    *  b2. Keep Interface Type as “WAN”
    *  b3. Set Security to “Untrusted”
    *  b4. Select Physical Interface “2”
    *  b5. Enter Primary IP with CIDR mask – refer to your network map
    *  b6. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcinterfaceswan.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcinterfaceswan.png)
Click Next
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcinterfacessummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcinterfacessummary.png)
1.  Enter WAN Link
    *  a. Select “+” to add the WAN Link
    *  a1. Select “Done” on the Popup to create a new Wan Link
    *  a2. ISP Name - select “Verizon”
    *  a3. Public IP Address – refer to your network map
    *  a4. Egress and Ingress Speed – set these to 20 Mpbs (Until you are able to apply your license for higher speeds.)
    *  a5. Virtual Interface – select “VIF-2-WAN-1” created in the Interfaces section
    *  a6. IP Address – the IP will automatically be populated based on the Virtual Interface
    *  a7. Gateway IP Address – verify with your network team.  It is often .1 in the last octet of the network
    *  a8. Select Done twice
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcwanlinks.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcwanlinks.png)
Click Next
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcwanlinkssummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcwanlinkssummary.png)
1.  Routes – we will leave routes blank.  For this POC the sdwan-vpx-nic-lan subnets will automatically be exchanged by the SD-WAN instances to establish connectivity between our test servers. To extend routing beyond the LANs discuss the requirements for static or dynamic routing with your network team and refer to SD-WAN Routing documentation.
1.  Verify the configuration details the on the Summary page, then click Save and Done.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcsummary.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcsummary.png)
1.  In the Network Configuration Home, you should see the entry for the sdwanamerdc site Cloud Connectivity column change from a gray circle that says “offline” to a green circle that says “online”. If this does not happen within 1 minute refer to the Troubleshooting section to investigate.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordcconnectivity.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordcconnectivity.png)

### Provisioning

Now that both sites are configured and online, we can start the provisioning process.  

1.  Since these are new installations, we will start by upgrading the Software version. From the Software Version drop down menu select the latest version at the bottom of the list. Select “Proceed” on the popup confirming the update.
1.  Now select “Deploy/Configure” and select “Stage”. If there are any configuration issues you will be notified here. To resolve them from the left menu navigate to Configuration > Network Home and select the SD-WAN appliance to return to the configuration workflow.  If you make changes be sure to click save and then return to “Deploy/Configure” and select “Stage”. The staging process will report back while downloading and confirm when staging is complete.
1.  Now select “Activate” to make live the downloaded software and configuration details on each SD-WAN appliance. The activation process will report on its progress and confirm “Activation Complete” for each appliance.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorprovisioningactivate.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorprovisioningactivate.png)

### Verification

Now that our sites are upgraded, configured, and online we can verify connectivity between the SD-WAN appliances and between the test servers on their respective LANs

1.  First select “Dashboard” and you should now see the entries for sdwanamerdc and sdwanamercloud with a green square in the Availability column.  This indicates that the Virtual Path is established between the 2 sites. If the state is another color refer to the Troubleshooting section to investigate.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratornetworkdashboard.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratornetworkdashboard.png)
1.  Verify connectivity
    *  a. First select the sdwanamerdc site
    *  b. Notice the green link between sdwanamerdc and sdwanamercloud confirming the virtual path is up
    *  c. Select “Devices” and verify the status is “Up” for both interfaces
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorsitedashboarddevices.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorsitedashboarddevices.png)
1.  Select “Troubleshooting” from the menu on the left
    *  a. Enter the IP Address of the Cloud SD-WAN sdwan-vpx-nic-lan IP address and select “Run”.  Verify the pings are successful to confirm connectivity from the DC SD-WAN appliance.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratordiagnosticsping.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratordiagnosticsping.png)
    *  b. Select Reports > Real Time > Statistics > Routes > "Retrieve latest data". Notice that routes to the Cloud SD-WAN Cloud sdwan-vpx-nic-lan and WAN subnets have been learned dynamically via the SD-WAN Virtual Path show by the protocol type "Virtual_WAN"
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-citrixcloudorchestratorstatisticsroutes.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-citrixcloudorchestratorstatisticsroutes.png)
1.  From the Data Center Test Server open a MS-DOS prompt and verify it can trace route to the Cloud Test Server.
[![WAN IP Addresses](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-pingdctocloudtestserver.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-pingdctocloudtestserver.png)

### Routing

Note that in this POC Guide we focused on establishing connectivity between endpoints located on the SD-WAN appliances local LANs. Both Citrix SD-WAN and Cloud platforms like Azure have many diverse options to extend routing across networks dynamically as required.

See Citrix SD-WAN documentation for more information regarding dynamic [routing](https://docs.citrix.com/en-us/citrix-sd-wan/11-1/routing.html).

See Azure documentation for more information regarding [virtual network traffic routing](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-udr-overview).

## Troubleshooting

The Orchestrator service dashboard provides a summary of the overall network health including alerts, QOS, and Connectivity details.  It is a good place to start Troubleshooting.

### Orchestrator Connectivity

Verify that [Citrix SD-WAN system requirements](https://docs.citrix.com/en-us/citrix-sd-wan/11-1/updating-upgrading/system-requirements.html) have been met and check external firewall rules to ensure required ports are open.

*  Cloud Connectivity – if the “Cloud Connectivity” state is not green for a site verify that it can send outbound traffic via the internet to TCP 443 and verify the primary DNS can resolve [Zero Touch Deployment (ZTD)](https://docs.citrix.com/en-us/citrix-sd-wan/11-1/use-cases-sd-wan-virtual-routing/zero-touch-deployment-service.html) service domains.
*  Virtual Paths – if the Availability state is not green for a site verify that it can send and receive UDP traffic on port 4980. This is an essential requirement to setup virtual paths and establish the overlay network.

### Orchestrator Reports

Navigate to Orchestrator > Reports > Realtime to retrieve the latest data on key statistics including:

*  Routing – verify the Data Center SD-WAN appliance has learned the Cloud sdwan-vpx-nic-lan route and the Cloud SD-WAN appliance has learned the Data Center sdwan-vpx-nic-lan route
*  Firewall Connections – check flows between location endpoints

### Orchestrator Diagnostics

Navigate to Orchestrator> Troubleshooting > Diagnostics to troubleshooting connectivity:

*  Ping/Traceroute – to verify basic connectivity and path details
*  Packet Capture – capture protocol and TCP/IP flow details

## Summary

Citrix SD-WAN can be setup rapidly to provide secure and resilient connectivity between your public cloud and data center environments. It also enhances traffic delivery performance during transport.  

While this guide focused on point-to-point connectivity, the SD-WAN network may be extended to large branches or small work from home environments.  It supports dynamical routing protocols, supports integration with leading firewall vendors, and can apply business rules to steer traffic to thousands of internet services efficiently.  To learn more about Citrix SD-WAN pricing and packing visit the [Citrix web site](https://www.citrix.com/) and to learn more about its technical capabilities visit [Citrix Tech Zone](https://docs.citrix.com/en-us/tech-zone/).
