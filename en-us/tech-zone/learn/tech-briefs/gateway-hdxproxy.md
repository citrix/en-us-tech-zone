---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks, Daniel Feller
specialThanksTo: Vijaya Raghavan, Kenneth Bell, Jeff Kann, Hrushikesh Paralikar, Simon Jackson, Pradeep Vasu
description: Provides users with secure remote access to Citrix Virtual Apps and Desktops without having to deploy Citrix Gateway in the on-premises DMZ or reconfigure firewalls.
tz_title: Gateway service for HDX Proxy
tz_products: citrix-networking;
---
# Tech Brief: Gateway service for HDX Proxy

Citrix Gateway service for HDX Proxy provides users with secure remote access to Citrix Virtual Apps and Desktops without having to deploy Citrix Gateway in the on-premises DMZ or reconfigure firewalls. Citrix hosts the entire infrastructure overhead of managing remote access in the cloud.

## On-Premises vs. Cloud

### On-Premises

Citrix Virtual Apps and Desktops have served Enterprises well for decades, yet they have extra requirements to provide remote access such as:

*  Implementing and maintaining multiple sites for redundancy
*  Implementing and maintaining public IP addresses
*  Implementing and maintaining network devices
*  Implementing and maintaining firewall rules

![On-Premises](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_1.png)

### Cloud

With Citrix Cloud and Citrix Gateway service Enterprises now can provide remote access to Citrix Virtual Apps and Desktops without those additional requirements along with other benefits:

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

The Citrix Workspace aggregates all user resources into a single, personalized interface using a locally installed [Workspace app](/en-us/tech-zone/learn/tech-briefs/workspace-app.html) (desktop and mobile) or a local browser. The Citrix Workspace communicates with the Citrix Virtual Apps and Desktops controller over a Citrix Cloud Connector.

### Citrix Cloud Connector

Citrix Cloud Connector runs on Windows Server instances hosted in Resource Locations and creates a reverse proxy to route traffic between the site/s and Citrix Cloud. It provides connectivity from Citrix Cloud to Resource Locations. It also includes access to Active Directory and delivery of control channel traffic between the Citrix Workspace and Citrix Virtual Apps and Desktops controllers. Cloud Connector also creates a connection from the Resource Location to the nearest Citrix POP to establish an initial data channel.

### Citrix Gateway service

The Citrix Gateway service is a part of the Citrix Cloud Services to provide secure remote access. It has been developed upon for more than a decade, all the while being used by the largest companies in the world. It relies on the Citrix Optimal Gateway Routing to direct clients to the closest global Citrix Gateway service POP. From there, it coordinates secure connectivity between Citrix Workspace clients and virtualization resources to deliver sessions with the lowest latency and the best user experience possible.

### Rendezvous protocol

Each Cloud Connector supports a limit of 1,000 concurrent sessions, and while adding more connectors grows capacity Citrix provides a more efficient solution to scale. [Rendezvous protocol](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/rendezvous-protocol.html) enables HDX sessions to be set up, through secure TLS transport, directly from the Virtual Delivery Agent (VDA) to the Citrix Gateway service without going through the Cloud Connector first. It is available in Citrix Virtual Apps and Desktops release 1912+ and can be enabled through a Citrix Policy setting. If the Rendezvous protocol is enabled and it cannot reach the Gateway service for any reason, it falls back to proxying traffic through the Cloud Connector.

## Resiliency

Citrix Gateway service operates in multiple POPs. If for any reason a POP goes down or connectivity is degraded past thresholds, Citrix Optimal Gateway Routing responds to subsequent DNS queries with the public IP address of the next closest POP. The Workspace app and the Citrix Virtual Apps and Desktops controller will initiate retries and timeouts based on session [connection](/en-us/citrix-virtual-apps-desktops/manage-deployment/connections.html) and [timer](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-limits-policy-settings.html) settings.

*  Each POP is configured for High Availability
*  Four 9 s of reliability
*  20 Global POPs

![Citrix Global Points of Presence](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4.png)

## Deployment

### Initial Setup

Transitioning access from your On-premises Gateway to Citrix Gateway service begins with creating [your Citrix Cloud environment](https://onboarding.cloud.com/). Afterwards you log in to your environment from Windows Server instances, with network access to your Citrix Virtual Apps and Desktops controllers. Then you can [install Citrix Cloud Connectors](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) to provide connectivity to Citrix Cloud.
![Cloud Connector](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4a.png)

### Initial Configuration

Once the Citrix Cloud Connectors are installed and up at your Resource Location the Citrix Workspace can be configured in Citrix Cloud. After specifying whether Authentication occurs using Active Directory (AD) or Azure Active Directory (AAD), and implementing any desired customizations to the Workspace app, Gateway, and Citrix Virtual Apps and Desktops On-Premises Sites must be enabled under **Service Integrations**. Then the site can be added and configured. The site configuration includes: specifying whether the controller is pre or post version 6.5, specifying the FQDN of the controller hosted in your Resource Location, verifying the domain identified through the Citrix Cloud Connector install, and specifying that the Gateway service is used for connectivity.
![Workspace Configuration](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4b.png)

### Initial Deployment

After the Citrix Workspace is configured in Citrix Cloud the new FQDN can be added to the Workspace app. Users can log in with the same AD credentials they would use with the On-premises Citrix Gateway and have the same apps and desktops enumerated.
![Workspace app](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_4c.png)

## Deployment Considerations

When a user launches an app within Workspace app, a DNS query for a FQDN hosted on Citrix Gateways, is relayed to the endpoint's local DNS name server. It typically relays it to an ISP DNS name server which makes a recursive query. As the authoritative name server Citrix Gateway Service returns the public IP address of the nearest POP based on the location of the IP address of the ISPs name server/s that made the recursive query. Therefore it is essential that the name server is in close proximity to the endpoint. If not sessions may incur performance issues.

### Web/SSL Proxy

It is recommended to exclude Gateway Service FQDNs from any DNS filtering and traffic inspection `(*.nssvc.net / *.g.nssvc.net / *.c.nssvc.net)`

Proxies can cause the following issues:

*  Randomize the DNS source IP, which leads to users being directed to a sub-optimal POP
*  Add latency to connections that are directed to the wrong POP (100ms+, with excessive jitter)
*  TLS inspection breaks Gateway Service since it does not support TLS interception

To implement it with Zscaler:

*  Update your ZPA to bypass certain applications
*  Under `Edit Application Segment` enter application entries for Gateway Service FQDNs `(*.nssvc.net / *.g.nssvc.net / *.c.nssvc.net)`

    For more information see [ZPA – Configuring Bypass Settings](https://help.zscaler.com/zpa/configuring-bypass-settings)

### VPN

It is recommended for VPNs to implement local breakout for Gateway Service domains `(*.nssvc.net / *.g.nssvc.net / *.c.nssvc.net)`

*  Enable split tunneling, so that the VPN Client sends only traffic destined for internal networks protected by the VPN tunnel
*  Traffic destined for Citrix Gateway Service would be sent directly via their local internet, rather than being backhauled over the VPN tunnel and internal network

To implement it with Citrix Gateway VPN make the following changes:

*  Enable spit tunneling under the VPN session policy Client Experience tab by setting the “Split Tunnel” field to “ON”
*  Configure transparent Intranet Application entries with the Internal Network IP address ranges
*  Under the Client Experience tab, advanced setting, ensure that “Split DNS” is set to Local. Also configure the DNS Suffix List under **Traffic Management > DNS > DNS Suffix**. Matching queries will be forwarded to the gateway, while others will be forwarded to the local DNS

    For more information see [configuring split tunneling in a full VPN setup on Citrix Gateway](/en-us/citrix-gateway/current-release/vpn-user-config/configure-full-vpn-setup.html#configure-split-tunneling)

## Manageability

Citrix Gateway service simplifies the requirements to access On-Premises Virtual Apps and Desktops and thus reduces the needed infrastructure and the operational overhead to maintain it. The need to maintain gateways with SSL Certificates and public IPs in multiple POPs are eliminated. Admins can then focus more on the management of their own IT business service priorities.

*  24x7x365 monitoring and maintenance by Citrix Cloud experts
*  Deliver a unified Workspace faster with existing IT staff
*  Reduce the need for specialized IT skills

[![Citrix Cloud Operations](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_5.png)](https://status.cloud.com)

## Session Connectivity

A user selects a virtual app or desktop, from their Workspace, and their endpoint receives a launch ticket. It is directed to connect to the Citrix Gateway service which, in turn, contacts the VDA. If configured to use the Rendezvous protocol the VDA establishes a TLS connection directly back to the requesting Citrix Gateway service POP, otherwise it uses Cloud Connector. Then the Citrix Gateway service establishes the session between the endpoint and the VDA.

*  Sessions are linked via Citrix Gateway service across cloud partner’s WANs
*  VDAs and Workspace endpoints rendezvous at the Citrix Gateway service POP closest to the user
*  High quality sessions

[![Citrix Gateway service and HDX Proxy: Traffic Flow](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_6.png)](/en-us/tech-zone/learn/media/tech-briefs_gateway-hdxproxy_6.png)
