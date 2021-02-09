---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Dan Feller, Derek Thoslund, Jaskirat Singh Chauhan, Karthick Srivatsan
description: Learn how to optimize the delivery of the Citrix Virtual Apps and Desktops service from cloud Resource Locations to users at branches or home offices, with a Citrix SD-WAN appliance, by minimizing latency, and improving session responsiveness during network issues.
---
# Citrix SD-WAN HDX performance improvement architecture for Citrix Virtual Apps and Desktops service Cloud environments

## Overview

This Reference Architecture explains how to optimize the delivery of Citrix Virtual Apps and Desktops service from Cloud Resource locations to users at locations with Citrix SD-WAN appliances. This minimizes latency, and improves session responsiveness during network issues.

Citrix Virtual Apps and Desktops service can deliver virtual apps, and desktops sessions to remote user endpoints via the Citrix Gateway service. When users select a virtual app or desktop in their Workspace App their endpoint is directed to resolve an FQDN. Then the Citrix Gateway service responds with the public IP address for it to connect to for session delivery via the Internet.

However, users connecting from a branch with a direct connection to the Resource Location where the virtual workload is hosted typically have a better path with less latency. At the same time, to provide the best user experience Citrix Virtual Apps and Desktops service sessions need to have Quality of Service (QOS) applied. Depending on the tasks the user is performing, the High Definition User Experience (HDX) sessions can be delivered using separate streams by Class of Service.

Citrix SD-WAN is a software defined approach to managing enterprise WANs to provide optimal network connectivity between Enterprise branch offices, and workloads hosted in public, private, or hybrid cloud Resource Locations. Citrix SD-WAN can automatically disseminate an HDX session into streams according to Class of Service between appliance virtual paths, and prioritize their delivery according to their Quality of Service requirements.

However, to provide this functionality, edge SD-WAN appliances must be able to inspect the HDX stream unencrypted. The Workspace client connects to the Citrix Gateway service, encrypted with HTTPs, for session management including authentication and resource enumeration. However, for app delivery the client must connect directly to the Virtual Delivery Agent (VDA), assigned by the Citrix Virtual Apps and Desktops service, via the SD-WAN overlay network.

With the Citrix Network Location Service (NLS) endpoints can Optimize connectivity to virtual apps, and desktops with Direct Workload Connection. It maintains a database of the public IP address ranges that are assigned dynamically as the source IP address of clients on the internal network, through Network Address Translation (NAT), on egress upon connecting to the Internet.

Enterprises with Citrix SD-WAN environment can automate the configuration. The Citrix Cloud hosted Orchestrator service, used to manage, and monitor Citrix SD-WAN environments, relays its network configuration details to the NLS. Otherwise, admins are able configure NLS manually using a PowerShell script that populates it through a Citrix Cloud API.

Then when a Workspace client contacts the Citrix Virtual Apps and Desktops service, to launch a virtual app or desktop, it queries the NLS to determine the endpoint’s location on the network. When the Citrix Virtual Apps and Desktops service is aware the client is hosted on the internal network it replies with the IP address of the target VDA, on the internal network. The address is included in session details provided for virtual app, and desktop launch requests, and with this directs the HDX session via the SD-WAN overlay network.

VDAs can be hosted in a Resource Location collated in a data center.
![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_overviewcollocated.png)

Alternatively, VDAs can be hosted in the public, private, or hybrid cloud Resource Location with access to the data center via a Citrix SD-WAN overlay network or other network path.
![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_overviewpubliccloud.png)

## Recommendation

Design Citrix Virtual Apps and Desktops service environments with the Network Location service to optimize connectivity to workspaces with Direct Workload Connection. Integrate with Citrix SD-WAN so that the HDX sessions being sent over the internal network are automatically disseminated into multiple ICA streams by Class of Service, given the appropriate QOS tag, and prioritized accordingly. In this document we discuss the Conceptual Architecture, and the Detailed Design of this recommendation.

## Conceptual Architecture

[![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitecture.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitecture.png)

### Components

Citrix Virtual Apps and Desktops service environments are built on a modular architecture which allows it to scale, and support access on any device, anytime, from anywhere. To best understand the architecture conceptually we consider 5 distinct layers, and their pertinent components.

#### User Layer Components

The User Layer is where the supported devices host the Workspace App, and users authenticate to obtain their Workspace resources.

*  **Citrix Workspace App** – is the Users’ portal to their Workspace resources from their device/s. It presents the user with authentication prompts, and subsequently their available resources.
*  **Citrix SD-WAN (Branch)** – Here SD-WAN provides several benefits.
    *  Multi-stream QoS prioritization - the branch appliance identifies HDX sessions from the endpoint, and automatically identifies, and breaks them into streams by Class of Service. It then queues, and routes traffic by priority across available virtual paths. The virtual paths are encrypted from the branch to the Resource Location.
    *  Reduced Latency – Citrix SD-WAN appliances automatically build virtual paths between each other across all available paths. They also monitor path performance in real time, and route traffic over the path with the least latency.
    *  Resiliency – Citrix SD-WAN integrates with MPLS circuits, or the internet over various media including cable or DSL. If a link fails, with real time traffic monitoring Citrix SD-WAN is able to reroute traffic, with minimal user impact, and often without session layer timeouts.
    *  Bandwidth Maximization – Traditional routers are typically limited to routing over a single “best” path. Citrix SD-WAN can take advantage of all available links, and simultaneously. It routes traffic on them, provided Quality is maintained, based on real time performance metrics.
    *  Quality of User Experience – available through [Citrix Orchestrator](/en-us/citrix-sd-wan-orchestrator), QOE monitors HDX quality, and quantifies experience by session, user, or site. It assigns scores, and reports in ranges including, Good (71-100), Fair (51-70), Poor (0-50), which allow for admins to prioritize focus. Other metrics include:
    *  Application: The application or application object name.
    *  Site: The name of the site.
    *  Virtual Service: The virtual path service used.
    *  Real-time QoE: The QoE score for real-time traffic.
    *  Interactive QoE: The QoE score for interactive traffic.
    *  Real-time Latency: The latency in milliseconds for real time traffic.
    *  Real-time Loss: The loss percentage for real-time traffic.
    *  Real-time Jitter: The jitter observed in milliseconds for real time traffic.
    *  Interactive Latency: The latency in milliseconds for interactive traffic.
    *  Interactive Loss: The loss percentage for interactive traffic.
    *  Interactive Jitter: The jitter observed in milliseconds for interactive traffic.

#### Access Layer Components

The Access Layer coordinates, and directs user sessions to the Control, and Resource components.

*  **Citrix Gateway service** – The Citrix Gateway service proxies secure HTTPs access to Citrix Virtual Apps and Desktops service environments. It includes a DNS location service to direct users to the nearest POP among dozens hosted around the world. For more information see [the Citrix Gateway service – Points-of-Presence (PoPs)](https://support.citrix.com/article/CTX270584)
Citrix Gateway service establishes HDX sessions from the user endpoints to VDAs, in their Resource Location via Cloud Connectors. Updated VDAs running Version 1912 or later can enable Rendezvous protocol. It allows HDX sessions to bypass the Citrix Cloud Connector and connect directly and securely to the Citrix Gateway service for increased scalability. For more information see [the Citrix Gateway service – Rendezvous protocol](/en-us/citrix-virtual-apps-desktops-service/hdx/rendezvous-protocol.html)

*  **Workspace service** – It is the front door for clients accessing Citrix Cloud services. It acts as a hub communicating with access, control, and resource layers to present, and help deliver sessions. Both Branch Users, and Remote Users are authenticated by the Workspace service, and it populates their available resources.
*  **Citrix SD-WAN (Resource Location)** – Typically the SD-WAN `Master Control Node` (MCN) is deployed in the primary Resource Location.  It is used to coordinate the overlay network, and establishes secure tunnels to the branch appliances. VDAs can be collocated with delivery components in the data center, or be accessible from public, private, or hybrid cloud via a tunnel or dedicated circuit. For more information on utilizing SD-WAN to provide access to Azure hosted VDAs see [Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity](/en-us/tech-zone/learn/poc-guides/sdwan-cloud-to-onprem-connectivity.html).

#### Control Layer Components

The Control Layer includes essential management components to coordinate the delivery of virtual apps, and desktops.

*  **Network Location Service** – stores the range of public IP addresses that internal clients are `natted` to when they access the internet.
*  **Orchestrator** – provides central management, and monitoring of the entire SD-WAN network. It also populates with NLS with SD-WAN appliance public IP addresses.
*  **Citrix Virtual Apps and Desktops service** – provides a unique role coordinating virtual app, and desktop delivery from Citrix Cloud. When Remote Users select a virtual app or desktop, it is proxied via the Citrix Gateway service for session delivery. When Branch Users select a virtual app or desktop the Citrix Virtual Apps and Desktops service checks with the NLS to verifies whether its source IP address, extracted from the HTTP header, is located on the internal network. Thereafter endpoints are provided with an ICA file, with the internal IP address of their target VDA, that can be accessed via the SD-WAN overlay network.

#### Resources Layer Components

Citrix Virtual Apps and Desktops service resources come in various shapes, and sizes depending on requirements like the operating system, and capacity.

*  **Virtual Delivery Agent (VDA)** – all resource types rely on the VDA to deliver their content as a session. They may be hosted in public, private hybrid cloud Resource Locations.
*  **Cloud Connector** – provides a reverse tunnel between the Resource Location, and Citrix Cloud for communication with the VDA (and, Active Directory if used as the Identity Provider).

#### Host Layer Components

Resource components can be hosted on public, private, or hybrid cloud locations.
For more information see [Citrix Virtual Apps and Desktops service component architecture see](/en-us/tech-zone/learn/downloads/diagrams-posters_virtual-apps-and-desktops_poster.png)

### Use Case Flows

Here we discuss Citrix Virtual Apps and Desktops service flows for two use cases:

1.  Remote Users (External) – This group is the “baseline” use case where we consider how all remote users, that have no access to the corporate intranet, need to access, and use their Citrix Virtual Apps and Desktops service environments.
2.  Branch Users (Internal) – This group is the use case where we consider how branch users, that have access to the corporate intranet, need to access, and use to their Citrix Virtual Apps and Desktops service environments. Branch users can be at-home workers with a Citrix SD-WAN appliance.

#### Remote Users (External)

Remote Users (not on the internal network) connect to the Workspace service, from their remote location, to authenticate, and obtain their list of resources. Their sessions are delivered via the Citrix Gateway service.
[![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitectureremoteusers.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitectureremoteusers.png)

#### Branch Users (Internal)

Branch users connect to the Workspace service, via the local Citrix SD-WAN appliance through direct internet breakout, to authenticate, and obtain their list of Workspace resources. By default, SD-WAN appliances have an internet service, DNS forwarder, and business application rule to direct the connection to the Workspace, and the Citrix Gateway service FQDNs, to breakout via the Internet. For more information see [Citrix SD-WAN Citrix Gateway optimization](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/citrix-cloud-and-gateway-service-optimization.html).

Upon selecting a virtual app or desktop, the Citrix Virtual Apps and Desktops service verifies whether the source IP address, extracted from the connecting client HTTP header, is in the NLS IP address range. Provided it is the ICA connection information sent back to the client contains the direct private IP address of a VDA. When the Workspace App sends a connection request to that address it is routed via the SD-WAN overlay network. Multi-steam QOS is automatically applied to the established HDX session, across the virtual path between the Branch, and Resource Location Citrix SD-WAN appliances.

[![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitectureinternalusers.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_conceptualarchitectureinternalusers.png)

## Detailed Design

In this section we discuss detailed design elements of the SD-WAN Overlay Network, Quality of Service, and the Network Location Service.

### SD-WAN Overlay Network

The SD-WAN Overlay Network is built over the available paths between each appliance. A secure tunnel is established on UDP port 4980, and LAN routes are exchanged.  Routes can be directly attached or learned, from the underlay network, through routing protocols. SD-WAN routes correspond to [service types](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html), and in addition to “virtual path” service routes they typically include “internet breakout” service routes to direct traffic identified by business application rules to ISP learned internet default routes.

In our example below Branch endpoints contact the Gateway direct through Internet breakout, and are directed to ISP default routes by SD-WAN appliances. Branch endpoints contact the virtual environment over virtual paths that are routed over the Internet, and MPLS Network which are utilized according to  Real-time performance.

![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_sdwanoverlaynetwork.png)

### Quality of Service with Citrix SD-WAN, and Citrix Virtual Apps and Desktops service

A key differentiator of using Citrix SD-WAN with Citrix Virtual Apps and Desktops service environments is its ability to automatically apply Quality of Service to HDX sessions. SD-WAN appliances identify, tag, prioritize, and streams them by class of service.

![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_cvadqos.png)

Multi-stream ICA virtual channels types are assigned to the 4 classes shown below by default. In the latest versions of Citrix Virtual Apps and Desktops service by default sessions are established using the Enlightened Data Transport (EDT) Protocol. EDT is a UDP based session transport protocol that efficiently adapts transmission according to available bandwidth. It falls back to TCP, and utilizes its built-in flow control, automatically as needed.

![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_virtualchannels.png)

By default, HDX sessions are sent by the VDA on UDP (EDT) port 1494 or 2598 or TCP 1494 or 2598 depending on the session protocol established. (2598 is used when session reliability is configured. Ports can be customized as needed.). The SD-WAN appliance selects the single HDX session, and identify the virtual channel by inspecting the uncompressed NSAP channel, and prepares it for transmission over a Virtual Path. It is supported on most [Workspace App platforms](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-workspace-app-feature-matrix.pdf).

The streams Virtual Path packets are assigned a tag according to class of service, and subsequently sent to the far end. The tag is used by the SD-WAN appliance to prioritize use outbound transmission queues, and for path selection which the are monitored in real time for quality. When sent over MPLS, Differentiated Services Code Point (DSCP) tags can be mapped to pertinent [queues](/en-us/citrix-sd-wan/11-2/quality-of-service/mpls-queues.html) allow prioritization in transit across the packet switched network. Typically, Real Time traffic is assigned with an Expedited Forwarding (EF) tag, while other traffic is assigned to an Assured Forwarding (AF) tags in the IP header [Type of Service (TOS) field](https://en.wikipedia.org/wiki/Type_of_service).

Using Citrix SD-WAN Single Port MS Auto QOS can provide significant performance improvement for the Citrix Virtual Apps and Desktops service HDX sessions during congestion. Lab testing identified greater than 500% improvement in ICA Round Trip Time under severe congestion. For more information see [Measuring HDX User Experience Improvements from Citrix SD-WAN Network Performance Enhancements](/en-us/tech-zone/design/reference-architectures/sdwan-hdx-experience.html)

### Network Location Service

The NLS enables the Citrix Virtual Apps and Desktops service to determine whether clients are on the internal or external network. By doing so they can access Resource Locations via the SD-WAN overlay network, and have HDX sessions streamed directly from VDAs. This allows SD-WAN to inspect session flows unencrypted, and apply QOS automatically to optimize performance.

![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_sdwannls.png)

## Summary

Citrix SD-WAN can significantly improve the network performance of the Citrix Virtual Apps and Desktops service HDX sessions resulting in a better user experience.  To achieve the maximum benefit, configure the Network Location Service in accordance with the enterprise network topology to achieve direct workload connection to virtual apps, and desktops. Route internal branch user sessions via the Citrix SD-WAN overlay network. This allows Citrix SD-WAN to minimize latency, and apply features including Multi-stream HDX Auto QoS to optimize session delivery according to Class of Service

## Appendix

### Deployment Considerations

Focusing on the goal of the document to provide guidance on optimal use of Citrix SD-WAN, in a Citrix Virtual Apps and Desktop service environment, we discuss pertinent hardware, software, and key configuration elements.

![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_cvadsdwandeploymentconsiderations.png)

The environment setup includes components in 3 distinct areas.

*  **Branch Office (Home Office)** – the home office or branch office has 1 or more endpoints with the Workspace App installed. It communicates with Citrix Cloud, and the Data Center via the SD-WAN appliance most likely in Gateway mode, included a dedicated public IP address with direct access to the internet via ISPs.
*  **Resource Location** – the Resource Location houses components needed to deliver virtual apps, and desktops. It hosts a SD-WAN appliance to communicate with internal endpoints, and communicates with Citrix Cloud via Cloud Connector/s. The appliance can be in Gateway mode or other deployment scenario.
*  **Citrix Cloud** – the Citrix Cloud service components Workspace, Citrix Virtual Apps & Desktops, and Citrix Gateway work in conjunction to manage virtual app, and desktop setup. Citrix Orchestrator manages the SD-WAN overlay network, and configures the Network Location service to route sessions directly to VDAs.

[![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_cvadsdwandeploymentrecommendations.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_cvadsdwandeploymentrecommendations.png)

#### Branch Office (Home Office)

The Branch Office is where the supported devices host the Workspace App to:

1.  Communicate with Citrix Cloud, through Citrix SD-WAN, to authenticate, and obtain their Citrix Virtual Apps and Desktops resources.
2.  Communicate with the Data Center, through Citrix SD-WAN, to contact their VDAs to establish direct virtual High Definition User Experience (HDX) sessions with Multi-stream Auto QOS applied.

*  **Citrix Workspace App**

    *  _Download_ from www.citrix.com/downloads
    *  For more information on _setup, and configuration_ for Windows 10 clients see [Citrix Workspace app for Windows – quick start guide](/en-us/tech-zone/build/tech-papers/citrix-workspace-app.html)
    *  _NOTES:_ Find/specify the Workspace FQDN, to connect to from the Workspace App, by logging into Citrix Cloud, and navigating to **Workspace Configuration** from the hamburger menu.
*  **Citrix SD-WAN (Branch)**

    *  _Download_ VPX versions for hypervisor hosting from www.citrix.com/downloads
    *  For more information on Citrix SD-WAN appliances see [Citrix SD-WAN data sheet](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-sd-wan-data-sheet.pdf)
    *  For more information on _setup, and configuration_ of 100 series appliances for Home Offices see [Proof of Concept Guide: Citrix SD-WAN for Home Offices](/en-us/tech-zone/learn/poc-guides/citrix-sdwan-home-office.html)
    *  _NOTES:_ Consider implementing Advanced Edition or Secure Internet Access (SIA) to enable full stack internet security.

#### Resource Location

The Resource Location is where virtual app, and desktop workload resources are hosted.

*  **Citrix SD-WAN (Resource Location)**
    *  _Download_ VPX versions for hypervisor hosting from www.citrix.com/downloads
    *  For more information on Citrix SD-WAN appliances see [Citrix SD-WAN data sheet](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-sd-wan-data-sheet.pdf)
    *  For more information on _setup, and configuration_ see [Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity](/en-us/tech-zone/learn/poc-guides/sdwan-cloud-to-onprem-connectivity.html)
    *  _NOTES:_ Follow the data sheet guidelines to ensure the appliance/s meet throughput, and virtual path requirements for your environment.

*  **Cloud Connector**
    *  _Download_ the Cloud Connector installer by logging into your [Citrix Cloud account](https://cloud.citrix.com), and navigate to Resource Locations from the hamburger menu.
    *  For more information see [Citrix Cloud Connector](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html)
    *  _NOTES:_ Cloud Connector runs on a Windows Server. Log in to Citrix Cloud from that server to download the installer. It must be able to reach a set of domain names on the internet which are specified in the documentation.

*  **Virtual Delivery Agent (VDA)**
    *  _Download_ the ISO installer at www.citrix.com/downloads under **Downloads > Citrix Virtual Apps and Desktops**
    *  For more information see [Proof of Concept guide for Getting Started with Citrix Virtual Apps and Desktop Service – Install Citrix Virtual Delivery Agent](/en-us/tech-zone/learn/poc-guides/cvads.html#install-citrix-virtual-delivery-agent)
    *  _NOTES:_ The installation follows intuitive prompts.  For a POC environment you can select “Enable Brokered Connections to a Server” to use the same server to as the virtual desktop that is streamed, rather than creating a main image that would be used to provision desktops to scale in production.
*  **Active Directory (AD)**
    *  _Download_ VPX versions for hypervisor hosting from www.citrix.com/downloads
    *  For more information see [Active Directory Domain services](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)
    *  _NOTES:_ AD can be installed through the Windows Server menu by following prompts. The Cloud Connector, and VDA Servers must be joined to the domain before setup. Right-click the computer, and select properties in Windows Explorer to join each server to the domain.

#### Citrix Cloud

Citrix Cloud hosts the Workspace service, and the Citrix Virtual Apps and Desktops service, the Citrix Gateway service, and the Network Location (micro) service, and the Orchestrator service.

*  **Workspace service**
    *  Create the Workspace service by creating a Citrix Cloud account.
    *  For more information see Proof of Concept guide for [Getting Started with Citrix Virtual Apps and Desktop Service – Create a Citrix Cloud Account](/en-us/tech-zone/learn/poc-guides/cvads.html#create-citrix-cloud-account)
    *  _NOTES:_ After creating a Citrix Cloud account log in to it on your target server/VMs to install the Cloud Connector, and establish communication with Citrix Cloud. Workspace configuration options are available under the hamburger menu. There you enable the service integration with the Citrix Virtual Apps and Desktops service, and configure the “site” (Resource Location).
*  **Citrix Virtual Apps and Desktops service**
    *  From your Citrix Cloud account, under Citrix Virtual Apps and Desktops, select “Request Trial” to speak to a Citrix sales representative regarding enabling the service for a trial.
    *  For more information see Proof of Concept guide for [Getting Started with Citrix Virtual Apps and Desktop Service – Obtain a Citrix Virtual Apps and Desktops Trial Account](/en-us/tech-zone/learn/poc-guides/cvads.html#obtain-a-citrix-virtual-apps-and-desktops-trial-account)
    *  _NOTES:_ After installing your Cloud Connector server/VM/s install your VDA/s to complete your Resource Location setup. Then select “Manage” in the Citrix Virtual Apps and Desktops service pane from the Citrix Cloud console to get access to the management studio. You enter a “machine catalog” including the FQDN/s of the VDA/s in your Resource Location. You also specify a “delivery group” which lists the virtual desktops, and or virtual apps from those VDA/s to publish to user groups.
*  **Citrix Gateway service**
    *  From your Citrix Cloud account, select “Request Trial” (if not already available), and a 60 day Gateway service trial is provisioned automatically.
    *  For more information see [Citrix Gateway support for Citrix Virtual Apps and Desktops](/en-us/citrix-gateway-service/support-for-citrix-virtual-apps-and-desktops.html)
    *  _NOTES:_ Select “Manage” under the Gateway service pane from the Citrix Cloud console, and follow the prompts. It directs you to the Workspace configuration, also available under the hamburger menu, to enable Gateway service.
*  **Orchestrator**
    *  From your Citrix Cloud account, select “Request Trial” (if not already available), and a 60 day Orchestrator service trial is provisioned automatically.
    *  For more information see Proof of Concept guide for [Getting Started with Citrix Virtual Apps and Desktop service – Create a Citrix Cloud Account](/en-us/tech-zone/learn/poc-guides/cvads.html#create-citrix-cloud-account)
    *  _NOTES:_ Select “Manage” under the Orchestrator service pane from the Citrix Cloud console. Select “Add site”, and follow the prompts for both the Branch, and Data Center / Resource Location sites. After entering Site Details, you enter the appliance serial number in Device Details. Then after following the config prompts the “Availability” column shows green if the appliance is communication with the configuration service over HTTPS/TCP 443 outbound. Then the Cloud Connectivity shows green once the appliance is able to setup a virtual path over UDP 4980 inbound, and outbound.
*  **Network Location service** –
    *  The Network Location service is included with a Citrix Cloud account. Orchestrator service can automate the configuration, for Citrix SD-WAN environments, or it can be configured manually through a PowerShell script.

        **Automated** - Orchestrator sites enabled for NLS share the Public IP address of all its internet WAN links along with other site details such as geographical location, time zone with the NLS database. With these details, the NLS determines whether the user connecting to Citrix Virtual Apps and Desktops is on a network front ended by Citrix SD-WAN. For more information see [Citrix SD-WAN Orchestrator – Network Location Service](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html#network-location-service)

        **Manual** – using a PowerShell script, provided by Citrix, configure the NLS with public IP ranges of the networks where your internal users connect to VDAs from [Optimize connectivity to workspaces with Direct Workload Connection](/en-us/citrix-workspace/workspace-network-location.html)

    *  _NOTES:_
To enable NLS in the Citrix Orchestrator service, at the network level, navigate to **Configuration > Delivery Services > Network Location Service**.

        The NLS PowerShell script submits public IP address ranges used in dynamic NAT, at Edge devices, when Enterprise clients access the Internet. For example, when a user enters “what is my IP address” in their web browser there are various sites that extract, and report the public IP carried in their HTTP request header. That public IP is typically from the range that would be configured in the PowerShell script. Enter a Client ID, and Secret from your Citrix Cloud instance to give the script access to it through an API.

### Network Considerations

Here well discuss bandwidth considerations, and pertinent ports utilized by Citrix SD-WAN, Citrix Gateway, and the Citrix Virtual Apps and Desktops service.

#### Bandwidth

Citrix SD-WAN appliances deployed in Gateway mode, in a branch, manage internal-intranet, and external-internet bandwidth allocation for all available links. New deployments need to plan to allocate, and existing deployments need to reallocate accordingly.

Citrix SD-WAN bandwidth is allocated by delivery services according to link type groups. For more information see [Delivery services](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/delivery-services.html)

Citrix Virtual App and Desktop session bandwidth usage varies broadly by usage scenarios.  For more information see [Citrix Virtual Apps and Desktops service bandwidth](https://virtualfeller.com/2020/02/19/citrix-virtual-apps-and-desktops-bandwidth/)

#### Ports

The Citrix Virtual Apps and Desktops service uses well defined ports to communicate between modular components. Citrix SD-WAN extends LAN like access to the User Layer components. (For more information see the diagram below.)

Citrix SD-WAN securely tunnels traffic across virtual paths between appliances, setup on UDP port 4980, using AES 128/256 bit ciphers. For more information regarding see [Configure Virtual WAN Service](/en-us/citrix-sd-wan/11-2/security/configure-virtual-wan.html)

Citrix SD-WAN can be deployed in various topologies to suit the location in the Enterprise network. For example, it can be used in Gateway mode in a branch to route all LAN traffic, or it can be deployed in Virtual Inline mode in a data center to have edge routers forward the desired traffic. For more information see [Citrix SD-WAN deployment topologies](/en-us/citrix-sd-wan/11-2/use-cases-sd-wan-virtual-routing/checklist-and-how-to-deploy.html)

Citrix SD-WAN can allow all LAN traffic, or filter according to Enterprise security policy with an onboard firewall. It can also be used to do full stack inspection if required. For more information see [Citrix SD-WAN Security](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-edge-security.html)
[![SD-WAN HDX Cloud Experience Architecture](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_ports.png)](/en-us/tech-zone/design/media/reference-architectures_sdwan-hdx-cloud-experience-architecture_ports.png)
