---
layout: doc
description: Provides users with secure remote access to Citrix Virtual Apps and Desktops without having to deploy Citrix Gateway in the on-premises DMZ or reconfigure firewalls.
---
# Citrix Gateway service for HDX Proxy

## Contributors

**Authors:** [Matthew Brooks](https://twitter.com/tweetmattbrooks), [Daniel Feller](https://twitter.com/djfeller)

**Special Thanks:** Vijaya Raghavan, Kenneth Bell, Jeff Kann, and Hrushikesh Paralikar

Citrix Gateway service for HDX Proxy provides users with secure remote access to Citrix Virtual Apps and Desktops without having to deploy Citrix Gateway in the on-premises DMZ or reconfigure firewalls. The entire infrastructure overhead of managing remote access moves to the cloud and is hosted by Citrix.

## On-Premises vs. Cloud

### On-Premises

Citrix Virtual Apps and Desktops have served Enterprises well for decades, yet they have additional requirements to provide remote access such as:

*  Implementing and maintaining multiple sites for redundancy
*  Implementing and maintaining public IP addresses
*  Implementing and maintaining network devices
*  Implementing and maintaining firewall rules

![On Premises](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_1.png)

### Cloud

With Citrix Cloud and Citrix Gateway service Enterprises now may provide remote access to Citrix Virtual Apps and Desktops without those additional requirements along with other benefits:

*  Multiple sites are implemented and maintained globally by Citrix
*  Public IP addresses are implemented and maintained by Citrix
*  Citrix Cloud advanced security with Citrix Analytics
*  Predictive DNS provides better user experience
*  No changes to the Virtual Apps and Desktops environment are required
*  Certificates are implemented and maintained by Citrix
*  Elastic scalability and High Availability is provided and managed by Citrix
*  Enterprises pay as they grow and reduce operating expenses
*  Faster onboarding for new customers

![Cloud](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_2.png)

## Citrix Cloud services

### Citrix Workspace

The Citrix Workspace aggregates all user resources into a single, personalized interface using a locally installed [Workspace app](/en-us/tech-zone/learn/tech-briefs/workspace-app.html) (desktop and mobile) or a local browser. The Citrix Workspace communicates with the Citrix Virtual Apps and Desktops controller over Citrix Cloud Connector.

### Citrix Cloud Connector

Citrix Cloud Connector runs on Windows Server instances hosted in Resource Locations and creates a reverse proxy to route traffic between the site/(s) and Citrix Cloud. It provides connectivity from Citrix Cloud to Resource Locations including access to Active Directory and delivery of control channel traffic between the Citrix Workspace and Citrix Virtual Apps and Desktops controllers. Cloud Connector also creates a connection from the Resource Location to the nearest Citrix POP to establish an initial data channel.

### Citrix Gateway service

Built on the code that evolved with the Citrix Gateway appliance over more than a decade, while used by the largest companies in the world, the Citrix Gateway service is an integral part of Citrix Cloud services to provide secure remote access. It relies on Citrix Intelligent Traffic Management (ITM) service to direct clients to the closest global Citrix Gateway service POP where it coordinates secure connectivity between Citrix Workspace clients and virtualization resources to deliver the lowest latency sessions with the best possible user experience.

### Rendezvous protocol

Each Cloud Connector supports a limit of 1,000 concurrent sessions, and while adding more connectors grows capacity Citrix provides a more efficient solution to scale. [Rendezvous protocol](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/rendezvous-protocol.html) enables HDX sessions to be setup, through secure TLS transport, directly from the VDA to the Gateway service without going through the Cloud Connector first. It is available in Citrix Virtual Apps and Desktops release 1912+ and can be enabled through a Citrix Policy setting. If the Rendezvous protocol is enabled and it cannot reach Gateway service for any reason, it will fall back to proxying traffic through the Cloud Connector.

### Citrix Intelligent Traffic Management (ITM) service

The Citrix Gateway service uses the Citrix Intelligent Traffic Management (ITM) service to help provide fast and reliable sessions. Developed to use a broad set of monitor sources and robust algorithms Citrix ITM directs workspace users from anywhere in the world to their nearest Citrix POP to help deliver the most efficient and reliable sessions possible.

Citrix ITM provides resiliency and a better user experience by directing workspace endpoints and Virtual Desktop Agents (VDAs) to the closest Citrix POP. ITM acts as the authoritative DNS for Citrix Gateway service Fully Qualified Domain Names (FQDNs), yet instead of providing static IP addresses to Citrix POPs, and following simple load balancing methods like round robin, it provides dynamic responses based on near real-time traffic analysis from a variety of sources including active users of the FQDN. Based on analysis of the plethora of data sources Citrix ITM responds to each DNS query with the nearest, fastest, most reliable POP available at the moment. Using proprietary collectors and analysis algorithms ITM can adapt to a variety of factors that could affect user experience such as CDN caching issues, ISP congestion, International Internet backbone outages.

![Citrix Intelligent Traffic Management (ITM)](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_3.png)

## Resiliency

Citrix Gateway service operates in multiple POPs around the world in conjunction with ITM which monitors the health of each site. If for any reason a POP goes down or connectivity is degraded past thresholds, Citrix ITM responds to subsequent DNS queries with the public IP address of the next closest POP. The Workspace app and the Citrix Virtual Apps and Desktops controller will initiate retries and timeouts based on session [connection](/en-us/citrix-virtual-apps-desktops/manage-deployment/connections.html) and [timer](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-limits-policy-settings.html) settings.

*  Each POP is configured for High Availability
*  14 POPs (11 Azure, 3 AWS).
*  Four 9s of reliability

![Citrix Global Points of Presence](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4.png)

## Deployment

### Initial Setup

Transitioning access from your On-premises Gateway to Citrix Gateway service begins with creating [your Citrix Cloud environment](https://onboarding.cloud.com/). Thereafter you will login to your environment from Windows Server instances, with network access to your Citrix Virtual Apps and Desktops controllers, where you will [install Citrix Cloud Connectors]( /en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) to provide connectivity to Citrix Cloud.
![Cloud Connector](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4a.png)

### Initial Configuration

Once the Citrix Cloud Connectors are installed and up at your Resource Location the Citrix Workspace may be configured in Citrix Cloud. After specifying whether Authentication will occur using Active Directory (AD) or Azure Active Directory (AAD), and implementing any desired customizations to the Workspace app, Gateway and Citrix Virtual Apps and Desktops On-Premises Sites must be enabled under Service Integrations and then the site may be added and configured. The site configuration includes specifying whether the controller is pre or post version 6.5, specifying the FQDN of the controller hosted in your Resource Location, verifying the domain identified through the Citrix Cloud Connector install, and specifying that Gateway service should be used for connectivity.
![Workspace Configuration](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4b.png)

### Initial Deployment

After the Citrix Workspace is configured in Citrix Cloud the new FQDN may be added to Workspace app. Users may login with the same AD credentials they would use with the On-premises Citrix Gateway and will have the same apps and desktops enumerated.
![Workspace app](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4c.png)

## Manageability

Citrix Gateway service simplifies the requirements to access On-Premises Virtual Apps and Desktops and thus reduces needed infrastructure and the operational overhead to maintain it. The need to maintain gateways with SSL Certificates and public IPs in multiple POPs are eliminated. Admins may then focus more on management of their own IT business service priorities.

*  24 x 7 x 365 monitoring and maintenance by Citrix Cloud experts
*  Deliver a unified Workspace faster with existing IT staff
*  Reduce the need for specialized IT skills

[![Citrix Cloud Operations](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_5.png)](https://status.cloud.com)

## Session Connectivity

A user selects a virtual app or desktop, from their Workspace, and their endpoint receives a launch ticket. It is directed to connect to Gateway service which, in turn, contacts the VDA. The VDA established a direct TLS connection back to the requesting Gateway, or utilizes Cloud Connector if it has any connectivity issues. Then Gateway establishes a connection between the endpoint and the VDA.

*  Sessions are linked via Citrix Gateway service across cloud partner’s WANs
*  VDAs and Workspace endpoints rendezvous at the Citrix POP closest to the user
*  High quality sessions

[![Citrix Gateway service and HDX Proxy: Traffic Flow](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_6.png)](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_6.png)

## Other Tech Zone content

[Learn -> Tech Brief -> Intelligent Traffic Management](/en-us/tech-zone/learn/tech-briefs/itm.html) - Citrix Intelligent Traffic Management (ITM) service brings “intelligence” to DNS by incorporating a broad set of capabilities to determine the best query response based on analysis of real time conditions.

[Learn -> Tech Brief -> SD-WAN Cloud Direct service](/en-us/tech-zone/learn/tech-briefs/sdwan-cloud-direct.html) - Optimize SaaS access for branch users by tunneling session traffic to Internet Exchanges with direct connectivity to popular sites.

[Learn -> Diagrams and Posters -> Citrix ADM - Overview Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adm.html) - One-page summary of the ADM Platform detailing system requirements, deployment modes, protocols and ports, common log files, common issues/failures, and more.

[Learn -> PoC Guides -> Citrix Web Application Firewall Deployment Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/citrix-waf-deployment.html) - Learn how to deploy Citrix Web Application Firewall (WAF) standalone or as a part of a Citrix ADC deployment. Protect web servers or applications from various attacks including Cross Site Scripting, SQL Injection, Buffer Overflow, Forceful Browsing and more. Deploy in any public cloud or your on-premises environment.

[Learn -> Tech Brief -> SD-WAN for Workspace](/en-us/tech-zone/learn/tech-briefs/sdwan-workspace.html) - Provides optimal network connectivity between Enterprise branch offices and their Workspace hosted in data resource locations on-premises or in the cloud.

[Learn -> Tech Insight -> Office 365 Optimization for Branch Offices](/en-us/tech-zone/learn/tech-insights/office365-optimization.html) - Learn how Citrix SD-WAN implements Microsoft Connectivity Principles to support Office 365 Optimization for Branch Offices.

[Learn -> Tech Insight -> SD-WAN](/en-us/tech-zone/learn/tech-insights/sdwan.html) - Optimize delivery of Citrix Virtual Apps and Desktops traffic with Citrix SD-WAN.

[Learn -> Tech Insight -> AlwaysOn VPN](/en-us/tech-zone/learn/tech-insights/citrix-gateway-alwayson.html) - Manage remote domain joined Windows endpoints 24x7 by providing LAN-like access with AlwaysOn VPN.

[Learn -> Tech Insight -> Intelligent Traffic Management - Radar](/en-us/tech-zone/learn/tech-insights/itm.html) - Learn how Citrix ITM can direct users to cloud service mirror sites with the most bandwidth available between them.

[Learn -> Tech Insight -> YouTube Optimization for Branch Offices](/en-us/tech-zone/learn/tech-insights/youtube-optimizations.html) - Optimize YouTube delivery in branch offices with Citrix Virtual Apps and Desktops and Citrix SD-WAN.

[Learn -> Diagrams and Posters -> Citrix ADC - nFactor Basics Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-nfactor.html) - One-page summary of nFactor authentication detailing concepts, how it works, nFactor Visualizer information, configuration steps, and more.

[Learn -> Diagrams and Posters -> Citrix ADC - File System and Process Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-file-system-process.html) - One-page summary of most common system directories, files, processes/daemons and logs.

[Learn -> Diagrams and Posters -> Citrix ADC - Troubleshooting High Availability Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-troubleshooting-high-availability.html) - One-page summary of high availability and troubleshooting tips.

[Learn -> Diagrams and Posters -> Citrix ADC - Troubleshooting GSLB MEP Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-troubleshooting-gslb.html) - One-page summary of GSLB, MEP protocol and troubleshooting tips.

[Learn -> Diagrams and Posters -> Citrix ADC - SDX Basics and Log File Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-sdx-basics.html) - One-page summary of SDX components and how to access them, common SVM ports, LOM configuration, Link Aggregation on the SDX, and Common Log files for both SVM and Citrix Hypervisor.

[Learn -> Diagrams and Posters -> Citrix ADC - nsconmsg Commands Cheat Sheet](/en-us/tech-zone/learn/diagrams-posters/cheat-sheet-adc-nsconmsg.html) - One-page summary of nsconmsg syntax and troubleshooting tips.

[Design -> Reference Architectures -> Application Delivery Management](/en-us/tech-zone/design/reference-architectures/citrix-adm.html) - See how the Citrix Application Delivery Management software is deployed to simplify management and monitoring of your application delivery infrastructure.

[Design -> Reference Architectures -> SD-WAN Multi-Region](/en-us/tech-zone/design/reference-architectures/sd-wan-multi-region.html) - Discover the framework, design, and architecture for Citrix SD-WAN multi-region deployment with SD-WAN Orchestrator.

[Design -> Reference Architectures -> SD-WAN](/en-us/tech-zone/design/reference-architectures/sdwan.html) - Learn about the framework, design, and architecture for Citrix SD-WAN with SD-WAN Orchestrator for single region deployment.

[Design -> Reference Architectures -> SD-WAN for Content Collaboration](/en-us/tech-zone/design/reference-architectures/sdwan-content-collaboration.html) - Learn about the deployment architecture and how Citrix SD-WAN WANOP helps to optimize Citrix Content Collaboration for customer-managed storage zones including relevant test data.
