---
layout: doc
description: Copy & paste description from TOC here
---
# Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity

## Contributors

**Author:** [Matt Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Karthick Srivatsan

## Overview

Use of the Cloud to deliver Enterprise services continues to grow. The recent need for significant portions of the workforce to work from home is driving demand for more scalable resources available from anywhere on demand.  However, those Cloud services typically still need to make backend calls to Data Centers systems such as; service account authenticating to Active Directory, applications pulling records from databases, virtual desktops copying files from shares.

Networking options like Express Routes, or Vnet Peering, available in Microsoft Azure for example, have disadvantages including complexity, cost, or scalability.  Also, they are limited to passing point-to-point traffic without providing performance benefits, require integration with complex routing to expand their use, and are not portable between public cloud platforms.

Citrix SD-WAN virtual appliances (VPX) may be deployed in Microsoft Azure, Amazon Web Services (AWS), or Google Cloud Platform (GCP) and integrate with Citrix SD-WAN appliances hosted in Data Centers or Private Clouds. The “virtual path” or secure tunnel that is established between SD-WAN instances seamlessly routes traffic between the Cloud and Data Center, across the Internet, while enhancing performance in transit.

Citrix SD-WAN uniquely transfers flows on a per packet basis across a secure UDP tunnel.  Using the proprietary header attached to each packet it measures in real-time the connectivity quality including latency, loss, or jitter. Therefore, when multiple links are available it steers flows to find the best path according to quality requirements.

[![Cloud-to-Data Center Connectivity Overview](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)

## Conceptual Architecture

For this Proof of Concept, we will demonstrate establishing connectivity between an Azure tenant environment representing the Cloud location and a Citrix Hypervisor based environment representing the Data Center location. The scope is limited to establishing connectivity between test servers hosted on the LAN where the SD-WAN appliances are implemented in each environment respectively.
**Cloud** – a SD-WAN VPX appliance will be built on an Azure tenant. It offers four sizes that vary by the number of virtual paths supported. It is also supported on AWS, and GCP public cloud platforms.

1.  Cloud Citrix SD-WAN –  the appliance, built in Azure,  requires subnet assignments for a Management, LAN, and WAN interface. Their host addresses will be dynamically allocated.
1.  Cloud Test Server -  a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have its subnet bound to the Azure Route table to reach the Data Center via the appliance.
**Data Center** – a SD-WAN VPX appliance will be built on a Citrix Hypervisor. It is also supported other major hypervisors and hardware platforms to support a broad range for forwarding requirements.
1.  Data Center Citrix SD-WAN –  the appliance, built on a Citrix Hypervisor,  requires subnet assignments for a Management, LAN, and WAN interfaces and Internet access to reach the Orchestrator Citrix [Zero Touch Deployment (ZTD)](https://docs.citrix.com/en-us/citrix-sd-wan-orchestrator/zero-touch-deployment.html) service.
1.  Data Center Test Server -  a Windows Server 2016 VM, provisioned on the SD-WAN LAN, must have a static route to the Cloud LAN, via the SD-WAN appliance in order to reach the Test Server
**Citrix Cloud** – the Orchestrator service is the central system for deployment and management of Citrix SD-WAN networks. SD-WAN appliances automatically contact the Orchestrator service with their serial number to verify manageability.
1.  Orchestrator service – will be configured with the serial number of both the Cloud and Data Center SD-WAN instance to establish their cloud connectivity and deploy their configuration details to make their virtual paths available.

## Cloud Setup

Following are the steps to setup Citrix SD-WAN VPX in Azure and configure a test server to route through it. Steps pertinent to the setup are documented, while other setup options not mentioned may be left as default or set to your preference.

(NOTE: Citrix SD-WAN is also supported on AWS, or GCP and once the initial setup is done the appliance uses the same configuration process.)
SD-WAN Appliance Setup

1.  Login to Azure Portal and administrator for your tenant
1.  Search the Marketplace for Citrix SD-WAN Standard Edition and select “Create”
1.  On the first page (Basics) in the workflow enter:
    *  a.  Select “Create new”.
    *  b.  Resource Group – Azure requires using an existing unpopulated group or creating a new one
    *  c. Region - where your services will be hosted
    *  d. Select “Next” at the bottom of the page
