---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Secure Internet Access

## Overview

The continuous and exponential increase in bandwidth, expansive user mobility, and the shift of applications to the cloud has made it an absolute must for enterprises to secure user internet access. Since users and devices are the new network perimeter, it must be done in the cloud.

The demand for remote work has made appliance-based network security strategies unsustainable as the projected increases in bandwidth consumption combined with backhauled mobile traffic will quickly saturate the maximum capacity of any on-prem appliance architecture.

The erosion of the network perimeter due to increased use of cloud apps and services further accelerates this backhauling, resulting in a poor end-user experience and lower productivity due to latency. Citrix Secure Internet Access (Citrix SIA) shifts the focus from defending perimeters to following users to ensure internet access is secure regardless of location.

Citrix SIA uses a “Zero Trust” role-based policy model. One of the core goals of Zero Trust is to assign policies and manage access to resources based on a user’s role and identity. This foundational concept is built into the Citrix SIA platform’s policy engine.

Citrix SIA delivers comprehensive internet security to all users in all locations including:

*  Complete web & content filtering
*  Malware protection
*  Protection for outdated browsers & OS
*  SSL/TLS traffic management
*  CASB, cloud apps & social media controls
*  Advanced, real-time reporting
*  Flexible data traffic redirection for any device, anywhere

## Architecture

Citrix SIA is a Cloud Native technology operates in any cloud. The service operates in 100+ datacenters around the globe. The architecture provides not only a delivery mechanism for connections but can also attach to other solutions to improve performance and security. For example, Citrix SIA can take signatures from other threat intel technologies and route it through our cloud as well.

Through the use of containerized gateways, the data plane is separated from customer to customer and private keys for decrypting traffic are kept specific to the customer and not shared amongst others. Every work unit processing or storing data is fully dedicated to the customer who does not need to manage any of the components. Work units can scale horizontally elastically.

Citrix SIA allows administrators to explicitly define cloud zones directly within the cloud admin portal. These zones ensure users within a given region will be secured within the region and that log events generated within a given region to stay within the region. This is important for regulations such as GDPR, which require data to stay within national boundaries. Additionally, cloud zones can be used to define how users connect to the cloud, depending on location. For example, when users move from office to office, these admin-defined zones enable the dynamic bypassing of local printers and servers that apply to each office.

### Redirecting Flow

Traditionally, internet application traffic from remote users is sent through slow and overloaded VPNs to provide network security for compliance, malware defense and data loss protection. Often this results in slow connections or downed networks preventing users from working safely and effectively. Furthermore, against the tenants of Zero Trust, access is typically provided to networks instead of to specific applications.  The result is excessive privileges, especially for users who only require targeted access to a handful of resources.

Citrix SIA eliminates the need for slow and overloaded VPN connections and sends traffic directly from the users to the necessary cloud resources or applications. Citrix SIA also reduces the risk of data loss and further segments the network by allowing users access only to specific resources and applications.

Another challenge that Citrix SIA solves is applying stream-based intrusion prevention when users travel outside the internal perimeter. This is because users move from place to place and work from networks outside of the control of the organization. When a user connects to the internet, the very first packet flows have no user identity and the source IP of their connections are only associated to the network they are originating from, such as a coffee shop’s Wi-Fi. This lack of identifiable tags on the network stream makes it hard to distinguish one user from another.

Citrix SIA is based on a containerized architecture which allows flow-based IP’s to be applied to users wherever they roam. This includes networks that are owned by the organization and networks that are not. The architecture of the Citrix SIA allows every customer to receive dedicated IP addresses that can be used in the same way local IP Addresses are used.
Citrix SIA follows users as they move in and out of the physical network perimeter, resulting in dedicated IP addresses for users regardless of location. This capability can be applied to require that users access business applications only through connections that are secured by the gateway, even when working outside of the office or on personal devices. This source IP address can be used to restrict access to resources such as admin portals by using the source IP address as a requirement for login.
There are two main methods for data redirection using either the Citrix SIA Cloud Connector (Cloud Connector) or Proxy method.

### Cloud Connector

Cloud Connector are the most popular deployment method, used most often with organizations that have managed devices. Cloud Connectors are the agents that enable Citrix SIA policies to be enforced on endpoints. Installs can be pushed remotely through a variety of methods such as:

*  Windows via GPO, MDM, SCCM deployment
*  MacOS/iOS via MDM deployment
*  Chromebook via Google admin console deployment
*  Android via native integration with Android VPN network or Always VPN
*  Linux with RedHat Fedora and Ubuntu support
*  Windows Terminal Server

Cloud Connectors ensure users are always connected Citrix SIA. The connectors take a mobile and cloud-first approach to connect users’ devices to cloud security regardless of whether they are in the office or on the road. There is no need to backhaul traffic through the datacenter as policies follow wherever the user is. They link your devices directly to Citrix SIA platform, providing secure internet access from any location and:

*  Dynamically provide user identity and group membership
*  Transparently redirect all data on a device, across all ports
*  Automatically and transparently deploy via Group Policy or MDM of your choice
*  Support GEO-Zoning dynamic traffic redirection, automatic load balancing and fail-over
*  Eliminate the need to have branch offices and remote users “backhaul” traffic to a data center before being sent to the internet.
*  Create security policies that “follow” users as they move to different locations or switch to different devices.

This provides a consistent experience without needing to tailor policies to specific networks or locations
Cloud Connectors use a proxy redirection method which takes advantage of a proxy PAC file
Proxy method: The device makes requests directly to the Citrix SIA platform
A proxy server is a server that acts as a third party for requests from clients seeking resources from other servers. For example, a client connects to the Citrix SIA proxy server, requesting some service such as accessing Google Services, the Citrix SIA proxy server evaluates the requests and determines whether the client is allowed or blocked from accessing this resource.

SD-WAN: Used to protect internet destined traffic

### Authentication

Citrix SIA can use local user identities, Microsoft Active Directory (AD), or another type of modern IdP (such as Azure AD). When using traditional AD, the security groups on Citrix SIA must match what is in AD. The platform will periodically check for group membership, on the device, when using the agent-based Cloud Connector deployment. When a user belongs to multiple security groups in AD, a priority can be set for what policies get enacted. Higher priority numbers take precedence over lower ones.
It is important to understand the user and group structure of your organization and how it will relate to Citrix SIA security groups. It is important to keep SIA groups down to a minimum of what is needed. It is advisable to group together similar users who need similar access. Policy layers can be utilized in situations where individual users within a single group have different needs.
You can associate current Active Directory security groups to Citrix SIA security groups by using the same names for both. SIA security groups can be assigned a priority number for the case in which a particular user is part of multiple groups. Citrix SIA places a user into the group that has the higher priority number. Group aliases can be assigned to associate multiple Active Directory groups to a single Citrix SIA security group.

User Authentication diagram

There are 4 different types of authentication using the Cloud Connector, Cloud Identity, SAML, and Active Directory plugin.

Cloud Connector: Authentication is handled transparently by the Cloud Connector through a session key sent over to Citrix SIA. Cloud Connectors leverage the user identity on the device an additional authentication is, therefore, not required when accessing the internet. 
When a user browses the internet, policies are applied based upon the security group mapping within the Citrix SIA. Security groups in Citrix SIA can be matched to the existing groups and OUs in your existing LDAP service.  Citrix SIA extracts what local group a user is a part of when the Cloud Connector pulls the information from a GP Result and sends that to Citrix SIA.
Citrix SIA uses that information to create an encrypted session key, which is sent back to the device. very web request gets the session key appended to the request. The request details can be seen when using Wire Shark. That information is used for policy matching.

Cloud Identity: Cloud Connectors can synchronize user groups with cloud identity providers (IdP). Currently Citrix SIA supports Google, Azure, and Okta. Configurations differ between each provider but typically consists of specifying:

*  Refresh interval
*  Client ID
*  Secret
*  Domain

SAML: SAML authentication is browser based or associated through the connector. SAML allows authentication to be handed off to a SAML identify provider, whereby the Citrix SIA platform acts as the service provider. SAML authentication must be paired with proxy data redirection and supports Okta, ADFS,

AD plugin

*  It is a server-side agent that can be installed instead across the Domain Controllers, serving logon services within the domain
*  OUs, security groups, and machines are gathered from the DCs and sent back to the platform for policy assignment

A note on Citrix SIA groups.
Organizations should associate their current security group structure to Citrix SIA groups. It is best practice to keep Citrix SIA groups to a minimum.
Aliases - Aliases can be added to a security group to capture multiple user groups under one larger Citrix SIA group
Multiple groups - When you have users that fall into multiple groups, priority is important. The higher the priority on the group, is what takes precedence for policy enforcement.
Policy Engines: There are 3 different policy engines. When a packet traverses the platform, determining which policy should be applied to the packet occurs in the following order:

*  Step 1 - IP Subnet: The packet is checked against the Local Subnets configured in the network settings based on the packet’s source IP address. If a match is found, the policy group is saved before moving on.
*  Step 2 - Device: The source IP is compared to the list of static and dynamic computers. If a match is found, the policy group from step 1 is replaced with the one found here.
*  Step 3 - User policy: If a user is logged into the device and found to be associated with the device, the group associated with that user overrides the group from step 2.

## Security Features

The containerized gateways scan data in the cloud and perform web filtering, prevent mal- ware, detect infections and prevent data loss as data moves to and from users and the Internet.

### Malware Protection

Citrix SIA uses many different layers of security when it comes to Malware Protection. Malware Protection includes:

*  malware identification and mitigation from the top signature databases
*  a proprietary malware registry
*  real-time threat information with instant database updates.

Administrators can enable both reputation-based defenses as well as malware protection which utilize advanced content analysis and sand-boxing capabilities.
Streaming Malware Reputation will process URL’s that users are accessing against a signature database and Citrix SIA’s proprietary malware registry as well as up-to-the-minute threat information with synchronous database updates. Advanced Malware Analysis Defense will auto deposit user downloaded files for behavioral analysis.

Malware rules can apply when certain criteria are met and are based off of the Malware settings. When applying malware rules to content, rules are checked from the top to the bottom of the list. As soon as a match is found, the rule is chosen, and no further rules are considered. Higher priority rules should be at the top of the list. Rules can easily be moved up and down to ensure the priority is set to the appropriate order.
Websites are given a risk score between 1-100 by analyzing various aspects of a request, including the URL, HTTP headers, and site content. Based on that score, the page is either blocked or allowed depending on the thresholds for Low Risk, Medium Risk, and High Risk.
A Risk Request Action of Allow or Block can be set separately for each risk threshold. For most configurations, the default settings provide the best balance between performance and security.

*  Advanced malware detection and prevention for polymorphic threats
*  Command and Control Callback monitoring across all ports and protocols
*  Signature and Signature-less malware prevention and breach detection
*  Incident response center
*  Intrusion detection & prevention
*  Behavioral malware sandboxing
*  Streaming Malware & Reputation Defense - Blocks malicious hosts and IP addresses using information from a combination of SIA and multiple industry-leading threat intelligence firms
*  Advanced Malware Analysis Defense - Inspects files, including archives. Malware Rules determine the action taken if a file is determined to be malicious by the AV engine

The Intrusion Prevention Systems (IPS) enables you to protect your network from malicious threats in packet streams, also known as "data in motion." As opposed to “data at rest” which is handled by Malware defense settings for items such as files and archives. The IPS policy engine runs in Citrix SIA. IPS allows you to choose the types of threats that are processed and recorded in the logs. Changes made to these settings are automatically synchronized across a node cluster. For most configurations, both of these options are enabled:

*  Enable Real-Time Intrusion, Malware, & Virus Protection: Enables packet, or stream-based, threat inspection and prevention
*  Exclude Private Subnets:  Enable to ignore local to local traffic.

When the IPS detects a threat, the Threat Rules determine the action it takes. Threat Rules have one of three actions take place:  Monitor, Block, and Disabled.

### Web Filtering

Web Categories are a quick and useful way to filter web access and traffic based on Security Group mappings. Depending upon the content of a site, it will have one or multiple categories associated with it based off of its domain. Web Categories settings should set the baseline access for a particular group. To get more granular, allow/block lists and policy layers can be configured. You can choose from several action options for each individual category. Actions can be applied to a web category and are independent from one another. Actions that can be taken by platform are:
Web policies in Citrix SIA are enforced based upon the security group association of the user. A majority of the web security policies on the platform can be applied to security groups selectively or to multiple groups.

*  Domains are categorized based upon the content of their sites
*  Each category has its own set of actions that are independently configured from the other categories
*  Domains with multiple categories assigned to it are subject to category priorities that will determine the action the user encounters
*  There are four primary actions you can assign to a web category: Allow, Block, Stealth, or Soft Override
*  SSL Decryption enabled/disabled
*  If a domain is associated with multiple categories, the action applied to it is determined by the priority number associated for the categories. Categories with higher priority values will take precedence.
*  Web Category – Soft Overrides
*  Presents the user with a block page for the category but will allow the user to choose a soft override command
    *  Allows the user to access the URL until the next day
    *  Any block page presented with a soft override will show in reporting & analytics as soft-blocked

#### Blocking or Allowing a website

#### Custom Block Pages

#### Keywords

### Data Loss Prevention

### Cloud Access Security Broker

### SSL/TLS Decryption

### Advanced Web Security

### Flow

## Citrix SDWAN

## Integration with CVAD

## Reporting