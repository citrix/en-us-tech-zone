---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: Eric Beiers, First2 Last2
description: The expansive demand for remote work and the shift of applications to the cloud has made it an absolute must for enterprises to secure user Internet access. And since users and devices are the new network perimeter, securing Internet access must be done in the cloud. Citrix Secure Internet Access (CSIA) shifts the focus from defending perimeters to following users to ensure Internet access is secure regardless of location.

---
# Citrix Secure Internet Access

## Overview

The expansive demand for remote work and the shift of applications to the cloud has made it an absolute must for enterprises to secure user Internet access. And since users and devices are the new network perimeter, securing Internet access must be done in the cloud. Citrix Secure Internet Access (CSIA) shifts the focus from defending perimeters to following users to ensure Internet access is secure regardless of location.

The demand for remote work has made appliance-based network security strategies unsustainable. The projected increases in bandwidth consumption combined with backhauled mobile traffic will quickly saturate the maximum capacity of any on-premises appliance architecture.

The erosion of the network perimeter due to increased use of cloud apps and services further accelerates this backhauling. The result will be a poor end-user experience and lower productivity due to latency.

![CSIA Unified Approach](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_traffic.png)

CSIA delivers comprehensive Internet security to all users in all locations with:

*  Complete web & content filtering
*  Malware protection
*  Protection for outdated browsers and operating systems (OS)
*  SSL/TLS traffic management
*  Cloud Access Security Broker (CASB) for cloud apps & social media controls
*  Real-time advanced reporting
*  Flexible data traffic redirection for any device and anywhere
*  Integration with Citrix Virtual Apps and Desktops and SD-WAN

CSIA uses a “Zero Trust” role-based policy model. One of the core goals of Zero Trust is to assign policies and manage access to resources based on a user’s role and identity. This foundational concept is built into CSIA's policy engine.

## Architecture

CSIA is a cloud native technology that operates in over a 100 Points of Presence around the globe. The architecture provides not only a delivery mechanism for connections but can also attach to other solutions to improve performance and security. For example, CSIA can take signatures from other threat intelligence technologies and compare to the traffic going through our cloud.

By using containerized gateways, the data plane is separated for each customer. Private keys for decrypting traffic are kept specific to the customer and not shared among others. Every work unit processing or storing data is fully dedicated to the customer who does not need to manage any of the components. Work units can scale horizontally elastically.

CSIA allows administrators to explicitly define cloud zones directly within the cloud admin portal. These zones ensure users within a given region are secured within the region and that log events generated within a given region to stay within the region. This feature is important for regulations such as GDPR, which require data to stay within national boundaries. Additionally, cloud zones can be used to define how users connect to the cloud, depending on location. For example, when users move from office to office, these admin-defined zones enable the dynamic bypassing of local printers and servers that apply to each office.

## Redirecting Flow

Traditionally, Internet application traffic from remote users is sent through slow and overloaded VPNs to provide network security for compliance, malware defense, and data loss protection. Often this results in slow connections or downed networks preventing users from working safely and effectively. Furthermore, against the tenants of [Zero Trust Architecture](en-us/tech-zone/learn/tech-briefs/zero-trust.html), access is typically provided to networks instead of to specific applications. The result is excessive privileges especially for users who only require targeted access to a handful of resources.

CSIA eliminates the need for slow and overloaded VPN connections and sends traffic directly from the users to the necessary cloud resources or applications. CSIA also reduces the risk of data loss and further segments the network by allowing users access only to specific resources and applications.

Another challenge that CSIA solves is applying stream-based intrusion prevention when end users travel outside the internal perimeter. They move from place to place to work from networks outside of the control of the organization. CSIA's containerized architecture allows flow-based IPs to be applied to users wherever they roam. The networks include those that are owned by the organization and networks that are not. The architecture of CSIA allows every customer to receive dedicated IP addresses that can be used in the same way local IP Addresses are used.

CSIA routes network data to the cloud to perform network security functions without the need for in-line appliances. This can be accomplished using proxy settings, endpoint agents/Connectors, GRE tunnels, or IPsec tunnels.

![Redirecting Traffic Flow](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_agent.png)

CSIA follows users as they move in and out of the physical network perimeter, resulting in dedicated IP addresses for users regardless of location. This capability can be applied to require that users access business applications only through connections that the gateway secures. Even when working outside of the office or on personal devices. The source IP address can be used to restrict access to resources such as admin portals by using the source IP address as a requirement for login.

**Cloud Connectors** are the most popular deployment method, used most often with organizations that have managed devices. Cloud Connectors are the agents that assist CSIA configuration with registration, authentication, and certificate management on endpoints.

Installs can be pushed remotely through various methods such as:

*  Windows via GPO, MDM, SCCM deployment
*  MacOS/iOS via MDM deployment
*  Chromebook via Google admin console deployment
*  Android via native integration with Android VPN network or Always On VPN
*  Linux with Red Hat Fedora and Ubuntu support
*  Windows Terminal Server

Cloud Connectors assist users connecting to CSIA. The connectors take a mobile and cloud-first approach to connect users’ devices to cloud security regardless of whether they are in the office or on the road. There is no need to backhaul traffic through the data center as policies follow wherever the user is. They link your devices directly to CSIA, providing secure access from any location and:

*  Configure networking to direct traffic to the CSIA service
*  Manage certificates on the user’s behalf to allow for SSL interception
*  Dynamically provide user identity and group membership
*  Transparently redirect all data on a device, across all ports

If your device has a non-standard OS, you can configure agentless data redirection by automatically generating Proxy PAC files.

**Proxy PAC** files are used for agentless data redirection that redirects traffic based on the user location (source IP). The PAC file supports geo-location to connect users to the closest gateways automatically and supports GDPR zoning. It is dynamically generated on a user-by-user basis. Proxy PAC supports load balancing, horizontal scaling, and authentication via SAML.

**IPsec/GRE** uses edge routers and firewalls to create tunnels and redirect all traffic at each location to CSIA. The tunnels point to a single CSIA DNS destination which routes traffic to gateways. The choice to use GRE or IPsec depends on what the existing edge router/firewall support. Authentication is achieved via connectors and SAML.

**SD-WANs** and CSIA can be used together with IPsec and GRE tunneling. Citrix SD-WAN integrates with CSIA by using application knowledge to intelligently steer Internet, cloud, or SaaS traffic to CSIA.

**DNS** is used for situations where the end user doesn't use a Cloud Connector, proxy PAC or SD-WAN. CSIA applies the categories and policies to DNS records, and which it forwards DNS from the on-premises DNS service. The network at each branch location redirects DNS traffic to CSIA and receives a unique security policy. Both policies and reporting logs are based on a per-location basis.

## Authentication

CSIA can use local user identities, Microsoft Active Directory (AD), or another type of modern IdP (such as Azure AD). When using traditional AD, the security groups on CSIA must match what is in AD. CSIA periodically checks for group membership, on the device, when using the agent-based Cloud Connector deployment. When a user belongs to multiple security groups in AD, a priority can be set for what policies get enacted. Higher priority numbers take precedence over lower ones.

You can associate current Active Directory security groups to CSIA security groups by using the same names for both. CSIA security groups can be assigned a priority number for the case in which a particular user is part of multiple groups. CSIA places a user into the group that has the higher priority number. Group aliases can be assigned to associate multiple Active Directory groups to a single CSIA security group.

CSIA can use four different types of authentication using the Cloud Connector, Cloud Identity, SAML, or Active Directory plug-in.

**Cloud Connector**: Cloud Connectors handle authentication transparently through a session key sent over to CSIA. Cloud Connectors use the user identity on the device. An extra authentication is, therefore, not required when accessing the Internet. When a user browses the Internet, policies are applied based on the security group mapping within CSIA. Security groups in CSIA can be matched to the existing groups and OUs. CSIA knows what local group a user is a part of when the Cloud Connector extracts and sends that group information to the CSIA service. CSIA uses that information to create an encrypted session key, which is sent back to the device. Every web request gets the session key appended to the request and is used for policy matching.

**Cloud Identity**: Cloud Connectors can synchronize user groups with cloud identity providers (IdP). Currently CSIA supports Google, Azure, and Okta. Configurations differ between each provider but typically consists of specifying the refresh interval, client ID, secret, and domain.

**SAML**: SAML authentication can be browser based or associated through the connector. SAML allows authentication to be handed off to a SAML identify provider, whereby CSIA acts as the service provider. SAML authentication must be paired with proxy data redirection and supports Okta, ADFS, and other SAML provider.

**AD plugin**: a server-side agent that can be installed across the Domain Controllers to provide logon services within the domain. Organizational Units, security groups, and machines are gathered from the DCs and sent back to CSIA for policy assignment.

Aliases can be added to a security group to capture multiple user groups under one larger CSIA group. When you have users that fall into multiple groups, priority is important. The higher the priority on the group, is what takes precedence for policy enforcement. There are 3 different policy engines. When a packet traverses CSIA, determining which policy will be applied to the packet occurs in the following order:

*  Step 1 - IP Subnet: The packet is checked against the Local Subnets configured in the network settings based on the packet’s source IP address. If a match is found, the policy group is saved before moving on.
*  Step 2 - Device: The source IP is compared to the list of static and dynamic computers. If a match is found, the policy group from step 1 is replaced with the one found here.
*  Step 3 - User policy: If a user is logged into the device and found to be associated with the device, the group associated with that user overrides the group from step 2.

## Security Features

The containerized gateways scan data in the cloud, They perform web filtering, prevent malware, detect, and prevent data loss as data moves to and from users and the Internet.

![CSIA Capabilities](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_capa.png)

### SSL/TLS Decryption

Many if not most sites and services on the Internet are encrypting their communications with users, with SSL/TLS being the most common protocol used to do so. This makes the ability to inspect SSL/TLS traffic essential for effective Internet security. Without doing so, most of an organization’s traffic goes unsecured. Malware or internal bad actors can exfiltrate corporate data unseen to the gateway.

As a critical but process-intensive task, SSL/TLS decryption can easily overburden traditional security appliances attempting to achieve full SSL/TLS visibility into content and cloud app usage. CSIA has a scalable cloud architecture that expands and contracts as needed. Each customer’s resources are fully containerized. CSIA keeps each organizations’ decryption keys isolated from other organizations.

Without SSL/TLS decryption, reporting and analytics become limited. For example, when searching the Internet without the setting enabled, CSIA can report on the search site but not on the keywords used in the search query. SSL/TLS decryption is not required for block, allow, or monitoring basic HTTP access. With SSL/TLS decryption enabled, CSIA can inspect HTTPS traffic thus offering more granular actions and visibility.

CSIA's SSL/TLS decryption works by implementing a “Man in the Middle” security procedure. SSL/TLS decryption can be performed transparently or through a proxy connection, with the only requirement being that the SSL/TLS certificate is deployed on the cloud connected device. CSIA acts as a root certification authority. It intercepts SSL/TLS requests to legitimate sites, requesting them, signing the received data with its own CA certificate and sending the data to the client.

To avoid client security warnings, connected devices will need the certificate from CSIA. The certificate can be distributed during the cloud connector install. To verify, when navigating to a site, the trusted certificate will be seen as being issued by CSIA and not by the site. SSL/TLS certificates can be pushed down through the installer, manually installed, or pushed through at root CA via any typical method.

**Decrypting all destinations note**: When configuring SSL decryption for the first time, it is recommended to start with selective SSL decryption as not all websites accept SSL decryption.

SSL Decryption can be enabled for all destinations a user travels to on the Internet. On the other hand, for websites that do not accept SSL decryption, a bypass can set to only selectively decrypt certain destinations. Below are the various types of bypasses that can be set.

*  Domains including all subdomains if not desirable for the site
*  Note: Bypass for a domain includes all of its subdomains as well
*  Web Categories if not desirable for the category
*  Groups if not desirable for specific users/groups
*  IP addresses and ranges if not desirable

### Malware Protection

CSIA uses many different layers of security for Malware Protection. Malware Protection includes:

*  Malware identification and mitigation from the top signature databases
*  A proprietary malware registry
*  Real-time threat information with instant database updates

Administrators can enable both reputation-based defenses and malware protection which utilize advanced content analysis and sand-boxing capabilities. Streaming Malware Reputation processes the URLs that users are accessing against a signature database. Advanced Malware Analysis Defense will auto deposit user downloaded files for behavioral analysis.

When applying malware rules to content, the rules are checked from the top to the bottom of the list. When a match is found, the rule is chosen, and no further rules are considered. Higher priority rules will be at the top of the list. Rules can easily be moved up and down to ensure the priority is set to the appropriate order. Websites are given a risk score between 1-100 by analyzing various aspects of a request, including the URL, HTTP headers, and site content. Based on that score, the page is either blocked or allowed depending on the thresholds for Low Risk, Medium Risk, and High Risk. A Risk Request Action of Allow or Block can be set separately for each risk threshold. For most configurations, the default settings provide the best balance between performance and security.

*  Advanced malware detection and prevention for polymorphic threats
*  Command and Control Callback monitoring across all ports and protocols
*  Signature and Signature-less malware prevention and breach detection
*  Incident response center
*  Intrusion detection & prevention
*  Behavioral malware sandboxing
*  Streaming Malware & Reputation Defense - Blocks malicious hosts and IP addresses using information from a combination of CSIA and multiple industry-leading threat intelligence firms
*  Advanced Malware Analysis Defense - Inspects files, including archives. Malware Rules determine the action taken if a file is determined to be malicious by the AV engine

The Intrusion Prevention Systems (IPS) enables you to protect your network from malicious threats in packet streams, also known as "data in motion". Malware defense settings for items such as files and archives handle “data at rest”. The IPS policy engine runs in CSIA. IPS allows you to choose the types of threats that are processed and recorded in the logs. Changes made to these settings are automatically synchronized across a node cluster. For most configurations, both of these options are enabled:

*  Enable Real-Time Intrusion, Malware, & Virus Protection: Enables packet or stream-based threat inspection and prevention
*  Exclude Private Subnets:  Enable if you want to ignore local to local traffic.

When the IPS detects a threat, the Threat Rules determine the action it takes. Threat Rules have one of three actions take place:  Monitor, Block, and Disabled.

### Web Filtering

Web Categories are a quick and useful way to filter web access and traffic based on Security Group mappings. Depending upon the domain, it has one or multiple categories associated with it based off its domain. Web Categories settings set the baseline access for a particular group. Allow/block lists and policy layers can be configured to get more granular. You can choose from several action options for each individual category. Actions can be applied to a web category and are independent from one another. Web policies in CSIA are enforced based on the security group association of the user. Most of the web security policies on CSIA can be applied to security groups selectively or to multiple groups.

*  Domains are categorized based on the content of their sites
*  Each category has its own set of actions that are independently configured from the other categories
*  Domains with multiple categories assigned to it are subject to category priorities that determine the action the user encounters
*  There are four primary actions you can assign to a web category: Allow, Block, Stealth, or Soft Override
*  SSL Decryption enabled/disabled
*  If a domain is associated with multiple categories, the action applied to it is determined by the priority number associated for the categories. Categories with higher priority values take precedence.
*  Soft Overrides - Presents the user with a block page for the category but will allow the user to choose a soft override command
    *  Allows the user to access the URL until the next day
    *  Any block page presented with a soft override shows in reporting & analytics as soft-blocked

#### Blocking or Allowing a website

Website browsing can be blocked using a Block List. Block Lists can be used to restrict access to network resources selectively. Allow Lists allow access to URLS that are blocked by a web category policy. Allow Lists can be used to grant access to specific resources.

Following are the various ways to configure Block Lists:

*  URL matching: URLs can be matched using domains, subdomains, IP addresses, IP ranges, and ports
*  Wildcards: Wildcard entries can be used in the block list by omitting the trailing forward slash from the path i.e. “mywebsite-parent-domain.com”
*  Explicit: These are non-wildcard entries; they can be created by appending a trailing forward slash to the pathi.e. “mywebsite-parent-domain.com/”
*  Regular Expressions: Regular expressions can be used to match complex patterns

#### Custom Block Pages

A Custom Block Page can be created for users who attempt to access blocked content. In addition, the block page can also apply for those users who try to access the intent while in Sleep Mode. Custom Block pages have the following associated actions:

*  Redirect: Send users to a specific URL
*  Silent Drop: prevents access without responding
*  Allow user to log in: bypass the block page by logging into an account that is created on the gateway

#### Keywords

Keyword filtering can be used to inspect URLs to find specific words.

*  The Web Gateway can take action to allow or block the user's activity depending on the associated behavior of their searched keyword.
*  CSIA includes predefined Adult and High-Risk keyword lists, which can be customized as required.
*  Administrators can choose to enable the predefined lists on a per-group basis.

### Data Loss Prevention

The Data Loss Prevention (DLP) module is an advanced analysis engine that inspects data in transit for potential loss and sensitive information. It includes built-in detection of Personally Identifiable Information (PII), Credit Card information including stripe data, and other content. In addition, custom patterns can be defined to detect content such as data from CRM or database systems.

The DLP module provides protection against unauthorized cloud use and sensitive data loss. This includes protection for the use of cloud services as to ensure sensitive data is secured and maintained within organizationally approved cloud services.
Deep file-based data loss prevention capabilities to detect, alert, and stop the transfer of sensitive data. Advanced detection capabilities detect and protect sensitive information within content by analyzing numerous file types including compressed nested files.

### Cloud Access Security Broker

The web security module has extensive social media and cloud application controls. Using the Cloud Access Security Broker (CASB) App Controls allow you to manage access to common online applications and social media. Cloud App Controls can be used to restrict activity on popular social media and cloud services. Cloud App Controls can be applied to specific security groups to allow or block items such as posting, liking, following and so forth.

*  Cloud app control can be used to manage the use of social media services such as Pinterest, Facebook, Twitter, LinkedIn, search engines, YouTube, and Google Apps
*  Controls for each of these can be adjusted on a per-group basis
*  Specific Cloud & SaaS services and social media sites require SSL decryption to be enabled

Search Engine Controls allow you to set SafeSearch settings on various search engines. These settings can also be applied globally or to specific groups. Google, Yahoo, and Bing web browser search engines are supported.

You can prevent users from uploading files to various cloud services. File upload can be blocked for Dropbox, Box, OneDrive, Google Drive and Generic file uploads. YouTube manager can restrict content, hide comments, and create a library of allow listed videos. YouTube manager settings can be applied globally or to specific security groups.

YouTube manager can perform other actions such as:

*  Home Page: You can redirect a user to the Organizations YouTube channel.
*  Bandwidth Preservation: You can force videos to play in Standard Definition
*  Allowed video libraries: You can allow access to specific videos that are blocked by the YouTube restricted search settings.

Microsoft CASB integration enables traffic policy control for unsanctioned Apps and CASB reporting visibility all within CSIA. Integration with the “Microsoft Cloud App” platform provides granular traffic policy control and assignments for blocking unsanctioned apps and CASB reporting within a single pane of glass. Real-time traffic logging and statistics are shipped via API integration to the “Microsoft Cloud App Security” platform. The integration between CSIA and Microsoft configuration is achieved by simply entering Microsoft subscription info into CSIA.

### Advanced Web Security

**Keyword filtering** is used for to inspect URLs for certain keywords and restrict user searches and phrases. Depending on the outcome of the inspection, content can be allowed or blocked. Keywords can be manually added or be sourced from a pre-defined list within CSIA. A Wildcard option can be enabled for multi-word searches/keywords that are substrings of larger words that can violate blocked keywords. Wildcard example: The keyword is “base.” A wildcard match for “base” will block both the search word “base” and “baseball.”

**Port Blocking** Connectivity to network resources can be restricted by certain ports for both UDP and TCP based protocols and direction – inbound, outbound, or both. In addition, a Port Block schedule can be applied to restrict access only during particular times of the day.
Port blocking control blocks Internet traffic on specified ports, or port ranges.

*  Any traffic using the specified ports are blocked completely.
*  To clear the Port Blocking controls, switch the enabled toggles for the port ranges you would like to disable to NO.
*  You can choose to always block the port ranges for the selected group or block ports using an Advanced Schedule.

**Browser and OS** Connectivity to network resources can be restricted by browser and OS to specific versions. The following actions can be applied to browser and OS restrictions:

*  Block the following: Blocks Internet access for specific browsers and OS versions
*  Allow only the following: Allows Internet access for specific browsers and OS versions
*  Move user: Move a user to another security group (if they violated the restrictions) for a particular period

**Content and MIME Type**
The Multipurpose Internet Mail Extensions (MIME) type is a standardized way to indicate the nature and format of a document.

*  MIME types help browsers understand how to process files it recovers from a web server. A browser matches its content type to the MIME type. You can control the type of content that can be accessed when browsing.

*  The general structure of a MIME type consists of a type and subtype in the format [type]/[subtype]. Examples include Image/jpeg, Image/gif, Image/mpeg.

**File Extensions** can be restricted to prevent users from downloading files that have specific extensions.

*  The file name extension control block specific file name extensions from being downloaded on your network.
*  Each extension may be a maximum of 15 characters in length
*  To remove an extension from the Block list, check the box next to the extension.

For example, a file with the ‘.exe’ extension may cause harm if run by a user and therefore can be restricted from downloading.

**Domain Extension** The domain extension control allows you to block or allow specific domain extensions from being accessed on a per-group basis. Manage the Domain Extensions list for each group by:

*  Blocking the domain extensions in the list
*  Only Allow the extensions in the list via the Block or Only Allow Domain Extensions
*  Each extension may be a maximum of 15 characters in length

For example, you may choose to allow only domains that end in ".com" and ".net". Any domains that do not end in those extensions will be blocked. Restrictions can be put in place to prevent users from browsing to specific Top-Level Domains.

Note: Setting the Top-Level Domain restriction does not prevent a user from brute force browsing by IP address

Policy layers offer more advanced control over Web Security Policies. You can apply Allow Lists, Block
Lists, Web Category Filters, SSL Decryption, and other settings based on various preconfigured criteria. This criterion includes but is not limited to:

*  User name
*  Security group
*  IP ranges
*  GeoIP

Policy Layers contain the concept of dynamic linking, whereby the policies are only applied if all or any of the criteria conditions are triggered. In addition, the policy layer can be set on advanced schedule – similar to that of the Internet Sleep Mode.
Proxy Rules to trigger actions in response to certain types of network activity.

*  Proxy rules are created by associating them with match patterns
*  Match patterns and rules are defined independently of each other
*  This logical separation frees them to be associated with each other in many different combinations, maximizing the logical possibilities
*  A rule is triggered when the state of the match patterns specified by the rule is evaluated as “true"
*  Actions include domain redirection, bypassing or enforcing proxy authentication, and even modifying HTTP Request headers

### Flow

CSIA allows you to route network data to the cloud to perform network security functions without the need for in-line appliances. The following diagram details the process flow from beginning to end.

![CSIA process flow](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_flow.png)

1.  User selects a link or enters a URL in local web browser
2.  CSIA agent identifies the user’s and Active Directory group membership
3.  CSIA analyzes whether the domain or URL is to be bypassed in the PAC script or agent. If so, the request is bypassed from cloud security.
4.  CSIA analyzes whether the request is to be blocked by the IPS. If so, the request is blocked.
5.  CSIA tags the user name and policy group to the request.
6.  CSIA analyzes whether the request will be decrypted. If so, the request is decrypted, and a certificate is added to the traffic flow.
7.  CSIA analyzes whether the request matches a proxy block rule. If no, it proceeds to the CASB rules. If yes, the request is checked against the Allow and Block lists. If a higher weight Block List rule exists, the request is blocked. If not, the request is allowed.
8.  CSIA analyzes whether the request matches a CASB block rule. If so, the request is blocked.
9.  CSIA analyzes whether the destination matches an entry in the Allow List.
10.  CSIA analyzes whether the destination matches an entry in the Block List.
11.  CSIA analyzes whether the destination matches a malware block rule. If so, the request is blocked.
12.  CSIA analyzes whether the request matches a DLP block rule. If so, the request is blocked.
13.  CSIA analyzes whether the destination matches an entry in the YouTube Manager rule. If so, the request is blocked.
14.  CSIA analyzes whether the destination matches a blocked web category. If so, the request is blocked.
15.  CSIA analyzes whether the destination’s IP address is to be blocked by GEO IP rules. If so, the request is blocked.
16.  CSIA analyzes whether the request matches any remaining web security policies. If so, the request is blocked. If not, the request is allowed.
17.  CSIA analyzes whether the request matches synchronized Microsoft MCAS line signatures. If no, then the request is allowed by cloud security.  If yes, then the request is checked against MCAS block policies and allowed or blocked based on the result.

## Citrix SD-WAN + SIA Integration Use cases

Citrix SD-WAN and CSIA integration offers flexibility and choice for a mixed profile of branch users in an enterprise. An enterprise typically has a mix of managed and unmanaged devices in the branch where a Citrix SD-WAN exists. With the integration, the CSIA Cloud Connector enables SD-WAN to securely breakout managed devices traffic to the CSIA cloud using the Internet service (with Load Balancing). The unmanaged devices like BYOD and Guest users are secured using the IPsec tunnel between Citrix SD-WAN and CSIA as the tunnel endpoints.

![CSIA with SD-WAN](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_sdwan.png)

There are several ways to secure users as they access cloud and SaaS apps. The methods cover use cases where the user sits behind an SD-WAN appliance at the branch, or at home, or whether the user is fully mobile.

For a user sitting at a corporate office, SD-WAN automatically creates secure connectivity to the closest CSIA point of presence. Traffic is tunneled via a GRE or IPsec tunnel. Redundancy is achieved both via the tunnel level and via multiple links to primary and secondary Points of Presence.

If a user leaves the corporate perimeter, the Cloud Connector installed on the device takes care of redirecting traffic to the CSIA cloud. The connector also serves the purpose of authenticating the user and installing appropriate certificates for SSL decryption.

For configuration information, read the following PoC guide: [CSIA and SD-WAN PoC Guide](/en-us/tech-zone/learn/poc-guides/secure-internet-access-sdwan.html)

## Integration with Citrix Virtual Apps and Desktops

Citrix Virtual Apps & Desktops deployments can be integrated with CSIA. When accessing from inside Citrix Workspace, users receive a unified experience that allows them to access all business relevant apps and desktops through a streamlined interface.

CSIA's Cloud Connectors can be configured in multiuser mode. Administrators can apply different policies to different users on multiuser systems such as Citrix Virtual Apps and Desktops and others. Instead of having users be seen as a single user, they are seen and logged as individuals.

Users sign into Citrix Workspace with single sign-on. Once inside, users have access to all published apps and desktops, including SaaS apps and browsers like Chrome. Secure Workspace Access provides secure, identity-aware, zero trust access to internal apps. CSIA provides secure access to Internet and SaaS apps from within published apps and desktops.

![CSIA process flow](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_cvadsia.png)

Now, let's say the same user decides to log off Citrix Workspace and access SaaS apps by simply opening a browser from their laptop device. Or perhaps the user decides to access a video sharing website or a personal file sharing website through a browser on their laptop device. In all those and similar instances when the user is accessing Internet and SaaS, the user continues to be protected by CSIA even when they are outside Citrix Workspace.

![CSIA with Citrix Workspace](/en-us/tech-zone/learn/media/tech-briefs_secure-internet-access_workspace.png)

For configuration information, read the following PoC guide. [CSIA and Citrix Virtual Apps and Desktops PoC Guide](/en-us/tech-zone/learn/poc-guides/secure-internet-access-cvad.html)
