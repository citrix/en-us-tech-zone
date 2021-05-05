---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
description: Learn why Citrix Gateway is the best secure remote access solution for Citrix Virtual Apps and Desktops.
tz_title: Citrix Gateway and Citrix Virtual Apps and Desktops
tz_products: citrix-networking;
---
# Tech Brief: Citrix Gateway and Citrix Virtual Apps and Desktops

## Overview

Citrix Gateway is the best secure remote access solution for Citrix Workspace. It provides a myriad of unique integrations that enhance security and user experience. Moreover, Citrix Gateway consolidates access to any app, from any device, through a single URL.

Citrix Gateway enables encrypted and contextual access (authentication and authorization) to Citrix Workspace. Its Citrix ADC-powered load balancing distributes user traffic across the Citrix Virtual Apps and Desktops servers. Citrix Gateway also accelerates, optimizes, and provides visibility into the traffic flow that is useful to ensuring optimal user performance in a Citrix Virtual Apps and Desktops deployment.

The following integrations add value to a Citrix Workspace deployment:

*  Contextual Authentication – Validate the user and device with multifactor (nFactor) authentication
*  Contextual Authorization - Limit app and desktop availability based on the user, location, and device properties
*  Contextual Access -  Limit access to HDX capabilities by modifying Citrix HDX connection behavior
*  End to End Monitoring – Identify the source of delays and triage issues which impact user performance
*  Adaptive Network Transport – Deliver a superior user experience by dynamically responding to changing network conditions
*  Optimal Routing – Ensure a better user experience by always launching apps and desktops from the local gateway
*  Custom Availability Monitors – Provide deep application health monitoring of back-end services running on the StoreFront and Delivery Controller servers

## Conceptual architecture

Citrix Gateway is a hardened appliance (physical or virtual) that proxies and secures all Citrix Workspace traffic with standards-based SSL/TLS encryption. The most common deployment configuration is to place the Citrix Gateway appliance in the DMZ. That places the Citrix Gateway between an organization’s secure internal network and the Internet (or any external network).

Many organizations protect their internal network with a single DMZ (Figure 1). However, multiple Citrix Gateway appliances can be deployed for more complex deployments requiring a double-hop DMZ (Figure 2).

![Single Hop DMZ Diagram](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig1.png)

Figure 1: Virtual App and Desktops with Citrix Gateway deployed in a single DMZ

Some organizations use multiple firewalls or to protect their internal networks. The three firewalls in Figure 2 divide the DMZ into two stages (double-hop) to provide an extra layer of security for the internal network.

![Double Hop DMZ Diagram](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig2.png)

Figure 2: Virtual Apps and Desktops with Citrix Gateway deployed in a double-hop DMZ

Citrix administrators can deploy Citrix Gateway appliances in a double-hop DMZ to control access to servers running Citrix Virtual Apps and Desktops. In a double-hop deployment the security functions are split across the two appliances.

The Citrix Gateway in the first DMZ handles user connections and performs the security functions such as encryption, authentication, and access to the servers in the internal network.

The Citrix Gateway in the second DMZ serves as a Citrix Gateway proxy device. This Citrix Gateway enables the HDX traffic to traverse the second DMZ to complete user connections to the server farm. Communications between Citrix Gateway in the first DMZ and the Secure Ticket Authority (STA) in the internal network are also proxied through Citrix Gateway in the second DMZ.

In enterprises where multiple StoreFront and Delivery Controllers servers are deployed, Citrix recommends load balancing the services by using the Citrix ADC appliance. One virtual server load balances all StoreFront servers and another load balances all Delivery Controller servers. Application intelligent health monitoring ensures that only fully functioning servers are marked available to take user requests.

Citrix ADC appliances configured for global server load balancing (GSLB) can balance the Citrix Workspace load across data centers. GLSB directs user requests to the closest or best performing data center, or to surviving data centers if there is an outage. Using GSLB provides for disaster recovery across multiple data centers and ensures continuous availability of applications by protecting against points of failure.

In Figure 3, the ADCs use the Metric Exchange Protocol (MEP) and the DNS infrastructure to connect the users to the data center that best meets the criteria set by administrators. The criteria can designate the least loaded, closest, or quickest data center to respond to requests.

![GSLB for Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig3.png)

Figure 3: Global Server Load Balancing for Virtual Apps and Desktops

## Contextual Authentication (nFactor and EPA)

Citrix Gateway consolidates remote access authentication infrastructure for all applications whether in a data center, in a cloud, or if the apps are delivered as SaaS apps. Moreover, Citrix Gateway provides:

*  The single point of access to all applications
*  Secure access management, granular and consistent access control across all apps
*  A better user experience that improves productivity
*  Multifactor authentication that improves security

![Citrix Gateway Single Sign On consolidation](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig4.png)

Figure 4: Citrix Gateway Single Sign On consolidation

Citrix Gateway authentication incorporates local and remote authentication for users and groups. Citrix Gateway includes support for the following authentication types.

*  Local
*  Lightweight Directory Access Protocol (LDAP)
*  RADIUS
*  SAML
*  TACACS+
*  Client certificate authentication (including smart card authentication)

Citrix Gateway also supports multifactor authentication solutions such RSA SecurID, Gemalto Protiva (Thales), Duo (Cisco), and SafeWord (Aladdin) using a RADIUS server configuration.

![Citrix Gateway nFactor options](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig5.png)

Figure 5: Citrix Gateway nFactor options

### nFactor Authentication

Citrix Gateway extends two-factor authentication with true multifactor capabilities and gives flexibility to administrators for authentication, authorization, and auditing. For example, dynamic login forms and on-failure actions are possible by using nFactor authentication. Administrators can configure two types of multifactor authentication on Citrix Gateway:

*  Two-factor authentication that requires users to log on by using two types of authentication
*  Cascading authentication that sets the authentication priority level

If the Citrix Gateway deployment has multiple authentication servers, administrators can set the priority of the authentication polices. The priority levels determine the order in which order the authentication server validates users’ credentials. When administrators configure a cascade, the system traverses each authentication server to validate a user’s credentials.

nFactor authentication enables dynamic authentication flows based on the user profile. The flows can be simple, or they can be coupled with more complex security requirements using other authentication servers. The following are some use cases enabled by Citrix Gateway:

1.  Dynamic user name and password selection: Traditionally, the Citrix clients (including browsers and the Workspace app) use the Active Directory (AD) password as the first password field. The second password is reserved for the One-Time-Password (OTP). However, to secure AD servers against brute force and lockout attacks, customers can require that the second factor such as OTP is to be validated first. nFactor can do this without requiring client modifications.
2.  Multitenant authentication endpoint: Some organizations use different Citrix Gateway login points for certificate and non-certificate users. With users using their own devices to log in, their access levels can vary on the Citrix Gateway based on the device being used. Citrix Gateway can cater to different authentication needs on the same login point – reducing complexity and improving user experience.
3.  Authentication based on group membership: Some organizations obtain user properties from AD servers to determine authentication requirements which can vary for individual users. For example, group extraction can be used to determine if a user is an employee or a vendor and present the appropriate second factor authentication.

### End Point Analysis Scans

Endpoint Analysis (EPA) scans are used to check user device compliance to endpoint security requirements. They are policy-based pre-authentication and post-authentication scans configured on the Citrix Gateway appliance. EPA scans are a part of contextual authentication if the state of the endpoint is involved in the authentication policies.

When a user device tries to access Citrix Workspace through the Citrix Gateway appliance, the device is scanned for compliance before being granted access. For example, EPA can be used to check parameters such as operating system, antivirus, web browser, specific processes, file system or registry key, and user or device certificates.

Administrators can configure two types of EPA scans using the OPSWAT EPA engine scan and a System scan using the Client Security engine. Using the OPSWAT EPA engine, administrators can configure product, vendor, and generic scans. These checks look for a particular product offered by a particular vendor, a vendor in a specific category, or a category across all vendors and products respectively.

System scans validate system level attributes such as MAC address or device certificates. Device certificates can be configured in nFactor as an EPA component and administrators can selectively allow or block access to corporate intranet resources based on device certificate authentication.

## Contextual Authorization (SmartAccess)

SmartAccess uses EPA post-authentication policies to limit user access to apps and desktops. For example, a sensitive Human Resources application can be enabled or disabled when a user connects from a managed or unmanaged device respectively.

Using SmartAccess policies, administrators can identify the resources available on a per user and per app basis. Factors such as the end user, source IP range, specific registry key, or file on the user endpoint are used to determine if compliance is met. Similarly, SmartAccess scans can be used to identify specific peripherals attached to a computer and show applications that require that device.

SmartAccess is configured on both the Citrix Gateway and inside Citrix Studio. The results of an EPA scan are matched to the corresponding access policies in Citrix Studio. In Figure 6, a user logs on to Citrix Workspace through Citrix Gateway with managed device and passes the compliance scan. Since the scan passed, the associated Citrix Gateway virtual server and session policy trigger the Citrix Studio policy to enable access for the app.

![EPA scan with compliance fail](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig13.gif)

Figure 6: EPA scan with compliance pass and app is allowed

In Figure 7, the same user logs on to Citrix Workspace through Citrix Gateway with personal device and fails the compliance scan. Conversely, failing the EPA scan doesn't not trigger the enabling of the app for this user and device.

![EPA scan with compliance fail](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig14.gif)

Figure 7: EPA scan with compliance fail and app is restricted

## Contextual Access (SmartAccess and SmartControl)

Citrix administrators can also modify Citrix HDX connection behavior based on how users connect to Citrix Gateway. Some examples include disabling client drive mappings, disabling access to specific apps and desktops, and disabling access to printing.

In Figure 8, a user logs on to Citrix Workspace through Citrix Gateway with personal device and fails the compliance scan. The results of the EPA scan performed by the Citrix Gateway are communicated with Citrix Workspace. Using SmartAccess, the Delivery Controller enforces the results of the scan and prohibits clipboard access and client drive mappings.

![EPA scan with compliance fail](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig6.gif)

Figure 8: EPA scan with compliance fail

In Figure 9, the same user connects to the same Citrix Gateway with a compliant device. The EPA results now allow clipboard access and client drive mappings.

![EPA scan with compliance pass](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig7.gif)

Figure 9: EPA scan with compliance pass

SmartControl helps customers meet security requirements that stipulate that access conditions are evaluated at the edge of the network. Customer security policies can require the ability to block access to resources even before a user has gained access to the corporate network. SmartControl can be used to block or allow certain components such as printer access, audio redirection, and client device drive redirection – at the Citrix Gateway.

SmartAccess and SmartControl are similar, however, SmartControl is configured exclusively on Citrix Gateway, while SmartAccess requires configuration on both Citrix Gateway and inside Citrix Studio. When administrators want to make access policy decisions for the entire farm, they can use SmartControl on Citrix Gateway that applies to the entire farm. One difference is that SmartAccess lets administrators control the visibility of published icons, while SmartControl does not. Figure 10 compares SmartAccess and SmartControl feature support.

![SmartAccess and SmartControl feature comparison](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig8.png)

Figure 10: SmartAccess and SmartControl feature comparison

SmartControl policies are designed to not enable any type of access if prohibited at the individual Delivery Controller level. The options are to default to the policy setting at the Delivery Controller level or prohibit a certain access even if it is allowed at the Delivery Controller. SmartAccess and SmartControl policies can be defined concurrently, and the most restrictive policy set is applied. Below is a list of SmartControl settings:

*  Connect Client LPT Ports – Blocks LPT port redirection used for printers
*  Client Audio Redirection – Redirect audio from VDA to client device
*  Local Remote Data Sharing – Allows or disallows data sharing using Receiver HTML5
*  Client Clipboard Redirection – Redirects client clipboard contents to VDA
*  Client COM Port Redirection – Redirect COM (serial) ports from client to VDA
*  Client Drive Redirection – Redirect client drives from client to VDA
*  Client Printer Redirection – Redirects client printers from client to VDA
*  Multistream – Allow or disable multistream
*  Client USB Drive Redirection – Redirect USB drives from client to desktop VDA only

## Adaptive Network Transport (EDT)

Citrix HDX is a set of technologies that ensure an unparalleled user experience when connecting to a remote Citrix resource. With the HDX engine in the Citrix Workspace app and the HDX protocol, Citrix HDX lets users interact seamlessly with resources even in challenging network conditions.

A recent optimization to HDX is the Citrix UDP-based reliable transport protocol called Enlightened Data Transport (EDT). EDT is faster, improves application interactivity, and is more interactive on challenging long-haul WAN and internet connections. EDT delivers a superior user experience by dynamically responding to changing network conditions while maintaining high server scalability and efficient use of bandwidth.

EDT is built on top of UDP and improves data throughput for all ICA virtual channels, including Thinwire display remoting, file transfer (Client Drive Mapping), printing, multimedia redirection. When EDT is not available, EDT intelligently switches to TCP ICA to deliver the best performance. Citrix Gateway supports EDT and Datagram Transport Layer Security (DTLS) which must be enabled to encrypt the UDP connection used by EDT.

![HDX Adaptive Transport](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig9.png)

Figure 11: HDX Adaptive Transport

**Watch this video to [learn more](https://www.youtube.com/watch?v=5iWffZOq57Y&feature=youtu.be):**

## End to End Monitoring (HDX Insight)

Citrix HDX Insight provides end-to-end visibility for HDX traffic to Virtual Apps and Desktops passing through Citrix Gateway. Using Citrix Application Delivery Management (ADM), administrators can view real-time client and network latency metrics, historical reports, end-to-end performance data, and troubleshoot performance issues.

By parsing HDX traffic, HDX Insight can identify the source of delays and triage issues which impact user performance. For example, a user may experience delays while accessing Citrix Virtual Apps and Desktops. To identify the root cause of the issue, administrators can use HDX Insight to analyze WAN Latency, Data Center Latency, and Host Delay. Using HDX Insight helps determine if the latency is on the server, data center network, or client network side.

Figure 12 shows an example where a specific user has normal WAN latency but high Data Center latency. This information is crucial to helping administrators triage a performance issue.

![HDX Insight session visibility](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig10.png)

Figure 12: HDX Insight session visibility

An important capability of HDX Insight is the ability to capture and display latency at the Layer 7 (L7). L7 latency calculation is done at the HDX layer and thus provides end-to-end latency detection regardless of the existence of TCP proxies. Looking at Figure 10, visibility into the application layers helps administrators diagnose latency by detecting that it is coming from apps and not the network for example in the situation of an overloaded server or back end.

The L7 latency thresholding actively detects end-to-end network latency issues at the application. This capability is contrasted to capturing Layer 4 network latency, which does not require HDX parsing but suffers from the major drawback of an incomplete view of latency end-to-end.

Successful user logons, latency, and application-level details for virtual HDX applications and desktops are provided by HDX Insight. Endpoint analysis (EPA), authentication, single sign-on (SSO), and application launch failures for a user are available with Gateway Insight.

Gateway Insight also provides visibility into the reasons for application launch failure for virtual applications. Gateway Insight enhances an administrator’s ability to troubleshoot any kind of logon or application launch failure issues and also view the:

*  Number of applications launched
*  Number of total and active sessions
*  Number of total bytes and bandwidth consumed by the applications
*  Details of the users, sessions, bandwidth, and launch errors for an application
*  Number of gateways
*  Number of active sessions
*  Total bytes and bandwidth used by all gateways associated with a Citrix Gateway appliance at any given time
*  Details of all users associated with a gateway and their logon activity

![HDX Insight L7 session visibility](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig11.png)

Figure 13: HDX Insight L7 session visibility

## HDX Optimal Gateway Routing

Citrix delivers the best app and desktop user experience with Citrix Workspace. In a hybrid cloud deployment, customers simplify the user experience with Global Server Load Balancing (GSLB). GSLB makes it easy for users to access apps, desktops and data regardless of their location. But with multiple Citrix Gateways, customers ask: “how can we send users to a specific data center where users’ unique data or back end application dependencies reside?”

HDX Optimal Gateway Routing gives administrators the ability to point Citrix Gateway connections to zones. This ensures that the Citrix Gateway picks the zone it typically has the best connection as defined by the administrator. Using GSLB also routes the user to the optimal Citrix Gateway based on their location, and that Gateway would be connected to a zone for complete optimal gateway routing.

HDX Optimal Gateway Routing ensures that the user experience is both simple and consistent by de-coupling the authentication gateway from the optimal launch gateway. This ensures that users always launch their apps and desktops from the local gateway, thus ensuring a better user experience when working from anywhere, on any device.

GSLB powered zone preference is a feature that integrates with Workspace, StoreFront, and the ADC appliance to provide user access to the most optimized data center on the basis of the user location. With this feature, the client IP address is examined when an HTTP request arrives at the Citrix Gateway appliance, and the real client IP address is used to create the data center preference list that is forwarded to StoreFront.

If the ADC is configured to insert the zone preference header, StoreFront 3.5 or later can use the information provided by the appliance to reorder the list of delivery controllers and connect to an optimal delivery controller in the same zone as the client. StoreFront selects the optimal gateway VPN virtual server for the selected data center zone, adds this information to the ICA file with appropriate IP addresses, and sends it to the client. StoreFront then tries to launch applications hosted on the preferred data center’s delivery controllers before trying to contact equivalent controllers in other data centers.

![HDX Insight Optimized Gateway Routing](/en-us/tech-zone/learn/media/tech-briefs_citrix-gateway-virtual-apps-desktops_fig12.png)

Figure 14: HDX Insight Optimized Gateway Routing

## Custom Availability Monitors

Citrix Gateway deployed with Citrix Workspace ensures the efficient delivery of applications. Using Citrix Gateway, incoming user requests from Citrix Workspace app can be load balanced between multiple StoreFront nodes in a server group. While some other solutions utilize simple ICMP-Ping or TCP port monitors, Citrix Gateway has deep application health monitoring of back end services running on the StoreFront and Delivery Controller servers.

StoreFront services are monitored by probing a Windows service that runs on the StoreFront server. The Citrix Service Monitor Windows service has no other service dependencies and can monitor and report the failure of the following critical services on which StoreFront relies for correct operation:

*  W3SVC (IIS)
*  WAS (Windows Process Activation Service)
*  CitrixCredentialWallet
*  CitrixDefaultDomainService

Citrix Gateway also has custom Delivery Controller monitors to make sure the Delivery Controllers are alive and responding before the Citrix Gateway load balances to the resource.  The monitors probe will validate a user’s credentials and confirm application enumeration to confirm whether the XML service is working. This prevents black hole scenarios where requests might be sent to an unresponsive server.

## Summary

Citrix Gateway has the most integration points with Citrix Workspace of any HDX proxy solutions. Citrix Gateway provides secure remote access to Citrix Virtual Apps and Desktops and is augmented with visibility and optimization features that are useful to ensure optimal user performance. The Citrix ADC provides intelligent global server load balancing that enhances availability and user experience. The following features enhance security and user experience:

*  Contextual Authentication – Multifactor (nFactor) authentication to validate the user and device
*  Contextual Authorization - Limit app and desktop availability based on the user, location, and device properties
*  Contextual Access -  Limit access to HDX capabilities by modifying Citrix HDX connection behavior
*  End to End Monitoring - Identify the source of delays and triage issues which impact user performance
*  Adaptive Network Transport – Delivers a superior user experience by dynamically responding to changing network conditions
*  Optimal Routing – Ensure a better user experience by always launch apps and desktops from the local gateway
*  Custom Availability Monitors – Deep application health monitoring of back end services running on the StoreFront and Delivery Controller servers
