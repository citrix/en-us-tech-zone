---
layout: doc
h3InToc: true
contributedBy: Anudeep Athlur
specialThanksTo: Jaskirat Singh, Karthick Srivatsan
description: Copy & paste description from TOC here
---
# Citrix SD-WAN + Azure Virtual WAN Blueprint: Introduction and Use Cases

## Audience

This document is intended for cloud solution architects, network designers, technical professionals, partners, and consultants who are assessing Microsoft Azure Virtual WAN and Citrix SD-WAN solutions. It is also intended for network administrators, Citrix administrators, managed service providers or anyone looking to deploy these solutions.

## Overview

This document highlights the advantages offered by Azure Virtual WAN, the benefits of a joint Azure VWAN and Citrix SD-WAN solution, the use-cases they cater to, and corresponding configuration guidance.

Broadly, the configuration guide covers how to:

1.  Establish connectivity in Azure Virtual WAN

    *  Hub-to-Hub connectivity

    *  VNet-to-Hub peering

1.  Establish connectivity between SD-WAN and Azure VWAN.

    *  Establish cross-region communication using SD-WAN

    *  Configure HA on SD-WAN Expected

## Use-cases

This document focuses on achieving three use-cases designed to meet an organisation’s typical requirements.

**Use Case # 1:** Securely connect to resources deployed in a single Azure region from globally distributed branches.

**Use Case # 2:** On premise infrastructure securely connecting to resources deployed in different Azure regions.

**Use Case # 3:** Globally distributed branch locations communicating with each other over Azure backbone.

## Azure Virtual WAN: Overeview

Azure Virtual WAN (VWAN) is a networking service from Microsoft that provides a high-speed global transit network and enables secure any-to-any connectivity across branches, data-centres, hubs and users. It supports site-to-site VPN (branch-to-Azure), user VPN (Point-to-Site), and ExpressRoute connectivity. It automates Hub-to-VNet connectivity (between the VNet/workload virtual network and the Hub). Customers can also leverage the Azure VWAN to connect branches across regions using the private Azure backbone.

![Azure VWAN](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_01.png)

## Better together: Benefits of a joint Azure VWAN and Citrix SD-WAN Solution

Citrix SD-WAN provides a secure and reliable WAN edge solution that reduces cost, complexity, and time to connect branch offices to Azure by automating network deployments.
It augments the benefits of Azure Virtual WAN by providing a high level of redundancy, automation, monitoring and control on a global scale, while allowing you to take full advantage of Microsoft’s high-speed global backbone.

1.  **Benefit 1: SD-WAN to automate network deployments to connect hundreds of branch offices to Azure hub**

    Setting up site-to-site VPN tunnels from hundreds of on-premises locations to Azure can be very time consuming and error prone. Citrix SD-WAN’s integration with Azure virtual WAN can speed that process up by orders of magnitude. Citrix SD-WAN Orchestrator provides a single pane of glass management to connect on-premises Citrix SD-WAN appliances to Azure virtual WAN by the virtue of API integration.
    An alternative architecture is to deploy Citrix SD-WAN VPX on Azure (instead of leveraging Azure Hubs) to form a book-ended solution with an on-premise SD-WAN appliance; it can bring benefits such as link bonding and load balancing, sub-second failover owing to industry leading per packet load balancing technology, selective packet duplication and bi-directional QoS among others to make the network more resilient and improve application-experience for users.  The SD-WAN VPX instance approach may be bandwidth limited (i.e. maximum 2Gbps), in which case the Azure VWAN solution provides higher throughput capabilities.

1.  **Benefit 2: SD-WAN to lower WAN Operational Costs**

    MPLS and ExpressRoute can provide a private, high-speed and secure channel to connect to the Azure VWAN. However, they are typically expensive solutions, especially when having to consider all the branch sites that would require connectivity to the private intranet. SD-WAN, by aggregating multiple cost-effective connection types (4G/LTE, DSL, Internet, etc.) can help lower WAN operational costs, in addition to providing higher resiliency to connect to Azure resources from the branch.
    Depending on an enterprise’s security requirements, traffic from one branch may have to be back-hauled to the enterprise-HQ to ensure security-compliance, straining the circuit and leading to added latency. Deploying a virtual Citrix SD-WAN appliance on Azure can provide local internet breakout by processing security policies created with the built in L2-L7 firewall within the Citrix SD-WAN appliance. This would limit the need to backhaul traffic to the HQ, and hence reduce the egress data charges incurred on metered plans and also help preserve user experience.

1.  **Benefit 3: SD-WAN to improve last-mile connectivity**

    As more and more workloads move to the cloud, last mile connectivity to-and-from remote offices becomes all the more important. It may not always be possible to provide MPLS/ExpressRoute connectivity to remote branches – over and above purely economic reasons, provisioning MPLS/Express Route network takes time and may not be feasible to implement.
    An SD-WAN deployment on a remote branch-site can help improve last-mile connectivity experience to Azure by aggregating multiple network links (which can be configured as primary and secondary) and identifying the closest Azure/Microsoft POP. By constantly monitoring the network, SD-WAN can automatically steer traffic from one link to another depending on the link quality, providing an optimal experience to remote-users.

1.  **Benefit 4: A book-ended solution to provide network resiliency and optimized WAN experience**

    While it is possible to connect a branch-office to Azure VWAN through native IPsec, a single IPsec tunnel may be prone to packet loss and link congestion and cannot mitigate the risks of outages. Any network outage can in cases lead to millions of dollars of productivity and revenue losses.

    Taking the alternative approach of deploying a Citrix SD-WAN VPX Network Virtual Appliance (NVA) on Azure to form a book-ended solution with an on-premises SD-WAN appliance, brings benefits such as link bonding and load balancing, sub-second failover, selective packet replication and bi-directional QoS among others to make the network more resilient and improve application experience for users.
1.  **Benefit 5: SD-WAN Orchestrator for enhanced visibility**
    Citrix SD-WAN orchestrator provides a single pane of glass to manage and track your network health and usage. Orchestrator’s template-based cloning simplifies large-scale deployments of SD-WAN and network expansions, helping scale changes to an entire fleet of branch-offices, and save time.
    SD-WAN Orchestrator also provides automated on-ramps to Microsoft Azure through point-and-click provisioning of SD-WAN VPX instances in new or existing VNets.

## Reference Topology

Azure workloads deployed in a specified region (Ex: Azure VWAN East Hub) can connect to multiple branch locations using Citrix SD-WAN appliances through Internet, or 4G/LTE. Within a Virtual Network (VNet) infrastructure, the SD-WAN Standard Edition VM (SD-WAN VPX) is deployed in Gateway mode.

[![Reference Topology](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_02.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_02.png)

To achieve the use-cases outlined above, we need to establish connectivity/pairing between the following entities:

1.  SDWAN VPX and Azure VWAN Hub over IPsec tunnels and BGP peering

1.  Azure VWAN Hub and VNets using local and global VNet peering

1.  SD-WAN branch appliances and SD-WAN VPX (on Azure) using Virtual Paths

Of the three, establishing connectivity between the SD-WAN branch appliance and SD-WAN VPX (3) over Citrix SD-WAN’s proprietary virtual path technology is what brings capabilities such as per-packet load balancing, selective packet replication and bi-directional QoS and provides the benefits of the joint solution.  
The document below walks you through the steps needed to set up Azure Virtual WAN and deploy an SD-WAN virtual appliance alongside.

## Azure Provisioning & Networking

Before deploying Azure Virtual WAN from SD-WAN Orchestrator, ensure that resources related to Azure Virtual WAN have been provisioned and network design has been finalised. This will simplify configuring Azure Virtual WAN from the SD-WAN Orchestrator management console.

This is a flowchart of how you can go about creating resources related to Azure Virtual WAN.

[![Summary of Steps](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_03.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_03.png)

**Please note** that while we have followed this flow, you could also resort to creating VNet’s before creating resource groups. Here’s a summary of the steps undertaken while building this topology; each step is covered in detail in subsequent sections.

### Summary of steps

1.  Create Resource Groups
1.  Create a VNet each for East and West region (for the topology discussed above)

    *  Same would be applicable for any number of VNet’s in different regions, per the topology in Azure
1.  Create Azure Virtual WAN resource
1.  Create Azure Virtual WAN Hubs governing the East and West regions (with inherent Hub to Hub connectivity within the single Virtual WAN.
    *  Create Hub in the East region.
    *  Create Hub in the West region.
1.  Peer the VNet’s of East and West regions (created in step2) with their respective Hubs.
    *  For instance a VNet in East will Peer with Hub in the East region so that it gets visibility to the SD-WAN subnets (that connect from On-Prem to SD-WAN VPX in the Azure which eventually connects to the Hub providing the subnets via BGP)

## Create Resource Groups

1.  Resource Group in East region

    *  Create a Resource Group for the region. In this architecture, we have named it AzureVWANEastRescGrp
    *  Make sure to select the region as US East (this is for the East region workloads/VNet’s). You can choose to provision it in any region per your topology.
    *  Review and Create the resource group

    [![Create Resource Group in East](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_04.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_04.png)

2.  Resource Group in West region

    *  Create a Resource Group with a name. In this architecture we have used AzureVWANWestRescGrp
    *  Make sure to select the region as US West (this is for the West region workloads/VNet’s)
    *  Review and Create the resource group

    [![Create Resource Group in West](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_05.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_05.png)

## Create a VNet each in the East and West regions (per the reference topology)

You may follow this [link](https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal) to create VNet’s for the regions.

## Create Azure Virtual WAN resources

1.  Search for Virtual WAN in the Azure search bar and click on Virtual WANs.

    [![Search Azure VWAN](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_06.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_06.png)

1.  Click on ‘+Add’ to add a new Virtual WAN

    [![Click Add](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_07.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_07.png)

1.  Start filling out the basic details of Azure Virtual WAN

    *  We have selected the East Resource group created at the start of the document, named  *AzureVWANEastRescGrp*
    *  Select location as US East (per the topology above)
    *  Provide a name to the Azure Virtual WAN resource being created. In this document we’ve named it *“EastUSVWANANDHub”*

    **Note : Please select the SKU as “STANDARD” to ensure the ability to achieve hub to hub communication across regions. However, if you don’t require inter-hub communication capabilities, you may choose the “BASIC” SKU.**

    [![Create WAN](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_08.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_08.png)

1.  When the validation passes, click Create
    [![Validate](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_09.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_09.png)

1.  After the resource gets created, you should be able to see it in the Azure Virtual WAN’s global section .

1.  As you see below, we see “EastUSVWANANDHub” listed with resource group as AzureVWANEastRescGrp and location as East US2.

    [![Confirmation](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_10.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_10.png)

**Note: Please note that while the Azure Virtual WAN Resource has been deployed in the East resource group in the East US 2 region, you may choose to deploy it in just about any region/resource group per your network topology.**

## Create Azure VWAN Hubs in each region

### 1. Azure Virtual WAN Hub creation for East region

Select the Virtual WAN Resource created in the previous step – *“EastUSVWANANDHub”*

*  Inside the Virtual WAN resource, you will find “Hubs” under Connectivity section.

    [![Find Hubs](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_11.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_11.png)

*  Click on Hubs and you will now be able to ADD a new Hub

*  Click on the “+ New Hub” in the Hubs section and it will open a new configuration pane

    [![Add Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_12.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_12.png)

*  Enter the Basic details

    *  Select the region you want the Hub to be created in.
    *  In this case, the region is US East 2 (Hub will be created in this region)
    *  Give the hub a name. In this case, it is “EastUSHub”
    *  Provide a unique address space for this Hub that does not overlap or is redundant with any other subnets in this resource group

    [![Configure Virtual Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_13.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_13.png)

    *  MOST Important step is to configure the “Site to Site” section.
        *  Select “Yes” for Do you want to create a site to site.
        *  The ASN is automatically populated and for this Hub it will be 65515.
        *  Choose the Gateway Scale Units as per your need.
            *  In this it is selected as 1 Scale Unit – 500 Mbps x 2

        Note : Leave every other section as Default
    *  Click on “Review and Create” to create the Azure Virtual WAN Hub in the East region.
    *  The hub creation process may take up to 30 minutes to complete.

    [![Configure Virtual Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_14.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_14.png)

    *  After the resource gets created, in the Hub section under the Virtual WAN, you will see a new entry under the Geo MAP with the name of the Hub we created.
        *  The Hub Status should reflect as “Succeeded”.
        *  Address space must be as we configured.
        *  Since there are No VPN sites still connected, it will show ‘0’. We’ll connect the VPN sites in subsequent steps.

    [![Configure Virtual Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_15.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_15.png)

    *  You can click on the EastUSHub link under the Geo MAP which will take you into Hub’s details. You should verify below attributes in the Hub details :
        *  Routing Status – Provisioned
        *  Hub Status – Succeeded
        *  Location – East US2
        *  VPN Gateway Provisioning status – Succeeded

    [![Verify Status](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_16.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_16.png)

### 2. Azure Virtual WAN Hub creation for West region

*  Inside the Virtual WAN resource EastUSVWANANDHub (where we initially provisioned our East US Hub), you will find “Hubs” under Connectivity section.

*  *Click on the Hubs and you will be able to add a New Hub.

[![Add a new hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_17.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_17.png)

*  Enter the Basic details
    *  Select the region you want the Hub to be created in.
    *  In this case, the region is US West 2 (Hub will be created in this region)
    *  Give the hub a name. In this case, it is “WestHubUS”.
    Provide a unique address space for this Hub that does not overlap or is redundant with any other subnets in this resource group.

[![Configure Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_18.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_18.png)

*  The MOST Important step is to configure the “Site to Site” section.
    *  Select “Yes” for Do you want to create a site to site (VPN Gateway)
    *  The ASN is automatically populated and for this Hub it will be 65515
    *  Choose the Gateway Scale Units as per your choice (Definition of throughput)
        *  In this it is selected as 1 Scale Unit – 500 Mbps x 2.

    **Note: Leave every other section as default.**

[![Configure Hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_19.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_19.png)

*  Click on “Review and Create” to create the Virtual WAN Hub in the West region.
*  The hub creation is a time-consuming process and may take somewhere around 30 minutes to finish creation.

[![Review and Create hub](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_20.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_20.png)

*  After the resource gets completely created, you can check the status of the WestHubUS like below
*  You can click on the WestUSHub link under the Geo MAP which will take you into Hub’s details. You should verify below attributes in the Hub details:

    *  Routing Status – Provisioned
    *  Hub Status – Succeeded
    *  Location – West US2
    *  VPN Gateway Provisioning status – Succeeded

[![Confirmation](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_21.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_21.png)

## Peer VNet to the Hub

### 1. Peering the East VNet to East Hub

*  Within the Virtual WAN resource "EastUSVWANANDHub", click on “Virtual Network Connections” and click on “+Add connection”

[![Add Connection](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_22.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_22.png)

#### **Fill out the details for the connection to peer the VNet1 in East US region to the Virtual Hub**

1.  Connection Name – Name of the Virtual Network Connection – VNet1EastPeerHub
1.  Hubs – Name of the Hub to peer with – EastUSHub
1.  Subscription – Ensure to select the subscription as the one you used for creating the Virtual WAN and the Virtual Hub resources
1.  Resource Group – Will be the resource group of the East region VNet we are trying to peer with the Hub
1.  Virtual Network – The VNet associated to the East region resource group that we will peer
1.  Propagate to None – Set it to NO
1.  Associate Route Table  - Select Default
1.  Propagate to Route Tables – Select Default(EastUSHub) which is the default routing table for East resource VNets
1.  Leave others as default
1.  Click on “Create"

[![Configure Connection](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_23.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_23.png)

*  Once the connection is created, the Virtual Networks should look like below. This enables the propagation of the East VNet prefixes to the EastUSHub (Virtual Hub), and allows the EastUSHub to propagate the prefixes that it has (via BGP) to the routing table of the East VNet.

[![Verify Connection](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_24.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_24.png)

### 2. Peering the East VNet to West Hub

*  Within the Virtual WAN resource EastUSVWANANDHub, click on “Virtual Network Connections” and click on “+Add connection”.

1.  Connection Name – Name of the Virtual Network Connection – WestHubVNet1Peer
1.  Hubs – Name of the Hub to peer with – WestHubUS
1.  Subscription – Ensure to select the subscription as the one you used for creating the Virtual WAN and the Virtual Hub resources
1.  Resource Group – Will be the resource group of the West region VNet we are trying to peer with the Hub
1.  Virtual Network – The VNet associated to the West region resource group that we will peer
1.  Propagate to None – Set it to NO
1.  Associate Route Table  - Select Default
1.  Propagate to Route Tables – Select Default(WestHubUS) which is the default routing table for West resource VNets
1.  Leave others as default
1.  Click on “Create”

[![Configure Connection](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_25.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_25.png)

*  Once the connection is created, the Virtual Networks should look like below. This enables the propagation of the West VNet prefixes to the WestHubUS (Virtual Hub), and allows the WestHubUS to propagate the prefixes that it has (via BGP) to the routing table of the West VNet.

[![Verify Connection](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_26.png)](/en-us/tech-zone/build/media/deployment-guides_sdwan-azure-virtualwan_26.png)
