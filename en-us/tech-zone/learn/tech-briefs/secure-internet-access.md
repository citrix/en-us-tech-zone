---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Secure Internet Access

## Overview

The expansive demand for remote work and the shift of applications to the cloud has made it an absolute must for enterprises to secure user internet access. And since users and devices are the new network perimeter, securing internet access must be done in the cloud.

The demand for remote work has made appliance-based network security strategies unsustainable as the projected increases in bandwidth consumption combined with backhauled mobile traffic will quickly saturate the maximum capacity of any on-prem appliance architecture.

The erosion of the network perimeter due to increased use of cloud apps and services further accelerates this backhauling, resulting in a poor end-user experience and lower productivity due to latency. Citrix Secure Internet Access (Citrix SIA) shifts the focus from defending perimeters to following users to ensure internet access is secure regardless of location.

Citrix SIA delivers comprehensive internet security to all users in all locations with:

*  Complete web & content filtering
*  Malware protection
*  Protection for outdated browsers and operating systems
*  SSL/TLS traffic management
*  Cloud Access Security Broker (CASB) for cloud apps & social media controls
*  Real-time advanced reporting
*  Flexible data traffic redirection for any device and anywhere

Citrix SIA uses a “Zero Trust” role-based policy model. One of the core goals of Zero Trust is to assign policies and manage access to resources based on a user’s role and identity. This foundational concept is built into Citrix SIA's policy engine.

## Architecture

Citrix SIA is a cloud native technology that operates in any cloud and in over 100 data centers around the globe. The architecture provides not only a delivery mechanism for connections but can also attach to other solutions to improve performance and security. For example, Citrix SIA can take signatures from other threat intel technologies and route them through our cloud as well.

Through the use of containerized gateways, the data plane is separated for each customer. Private keys for decrypting traffic are kept specific to the customer and not shared amongst others. Every work unit processing or storing data is fully dedicated to the customer who does not need to manage any of the components. Work units can scale horizontally elastically.

Citrix SIA allows administrators to explicitly define cloud zones directly within the cloud admin portal. These zones ensure users within a given region will be secured within the region and that log events generated within a given region to stay within the region. This is important for regulations such as GDPR, which require data to stay within national boundaries. Additionally, cloud zones can be used to define how users connect to the cloud, depending on location. For example, when users move from office to office, these admin-defined zones enable the dynamic bypassing of local printers and servers that apply to each office.

### Redirecting Flow

Traditionally, internet application traffic from remote users is sent through slow and overloaded VPNs to provide network security for compliance, malware defense, and data loss protection. Often this results in slow connections or downed networks preventing users from working safely and effectively. Furthermore, against the tenants of Zero Trust, access is typically provided to networks instead of to specific applications. The result is excessive privileges especially for users who only require targeted access to a handful of resources.

Citrix SIA eliminates the need for slow and overloaded VPN connections and sends traffic directly from the users to the necessary cloud resources or applications. Citrix SIA also reduces the risk of data loss and further segments the network by allowing users access only to specific resources and applications.

Another challenge that Citrix SIA solves is applying stream-based intrusion prevention when end users travel outside the internal perimeter and move from place to place to work from networks outside of the control of the organization. Citrix SIA's containerized architecture allows flow-based IP’s to be applied to users wherever they roam. This includes networks that are owned by the organization and networks that are not. The architecture of the Citrix SIA allows every customer to receive dedicated IP addresses that can be used in the same way local IP Addresses are used.

Citrix SIA follows users as they move in and out of the physical network perimeter, resulting in dedicated IP addresses for users regardless of location. This capability can be applied to require that users access business applications only through connections that are secured by the gateway, even when working outside of the office or on personal devices. This source IP address can be used to restrict access to resources such as admin portals by using the source IP address as a requirement for login.

### Cloud Connector

Cloud Connectors are the most popular deployment method, used most often with organizations that have managed devices. Cloud Connectors are the agents that enable Citrix SIA policies to be enforced on endpoints. Installs can be pushed remotely through a variety of methods such as:

*  Windows via GPO, MDM, SCCM deployment
*  MacOS/iOS via MDM deployment
*  Chromebook via Google admin console deployment
*  Android via native integration with Android VPN network or Always On VPN
*  Linux with RedHat Fedora and Ubuntu support
*  Windows Terminal Server

Cloud Connectors ensure users are always connected Citrix SIA. The connectors take a mobile and cloud-first approach to connect users’ devices to cloud security regardless of whether they are in the office or on the road. There is no need to backhaul traffic through the datacenter as policies follow wherever the user is. They link your devices directly to Citrix SIA, providing secure internet access from any location and:

*  Dynamically provide user identity and group membership
*  Transparently redirect all data on a device, across all ports
*  Automatically and transparently deploy via Group Policy or MDM of your choice
*  Support GEO-Zoning dynamic traffic redirection, automatic load balancing and fail-over
*  Eliminate the need to have branch offices and remote users “backhaul” traffic to a data center before being sent to the internet
*  Create security policies that “follow” users as they move to different locations or switch to different devices

If your device has a non-standard OS, you can configured agentless data redirection by automatically generating Proxy PAC files. For example, a client connects to the Citrix SIA proxy server, requesting some service such as accessing Google Services, the Citrix SIA proxy server evaluates the requests and determines whether the client is allowed or blocked from accessing this resource. SAML can be used for authentication and this is the preferred method for IoT devices.

### Authentication

Citrix SIA can use local user identities, Microsoft Active Directory (AD), or another type of modern IdP (such as Azure AD). When using traditional AD, the security groups on Citrix SIA must match what is in AD. The platform will periodically check for group membership, on the device, when using the agent-based Cloud Connector deployment. When a user belongs to multiple security groups in AD, a priority can be set for what policies get enacted. Higher priority numbers take precedence over lower ones.

It is important to understand the user and group structure of your organization and how it will relate to Citrix SIA security groups. It is important to keep SIA groups down to a minimum of what is needed. It is advisable to group together similar users who need similar access. Policy layers can be utilized in situations where individual users within a single group have different needs.

You can associate current Active Directory security groups to Citrix SIA security groups by using the same names for both. SIA security groups can be assigned a priority number for the case in which a particular user is part of multiple groups. Citrix SIA places a user into the group that has the higher priority number. Group aliases can be assigned to associate multiple Active Directory groups to a single Citrix SIA security group.

Citrix SIA can use four different types of authentication using the Cloud Connector, Cloud Identity, SAML, or Active Directory plugin.

**Cloud Connector**: Authentication is handled transparently by the Cloud Connector through a session key sent over to Citrix SIA. Cloud Connectors leverage the user identity on the device an additional authentication is, therefore, not required when accessing the internet. When a user browses the internet, policies are applied based upon the security group mapping within the Citrix SIA. Security groups in Citrix SIA can be matched to the existing groups and OUs in your existing LDAP service.  Citrix SIA extracts what local group a user is a part of when the Cloud Connector pulls the information from a GP Result and sends that to Citrix SIA. Citrix SIA uses that information to create an encrypted session key, which is sent back to the device. very web request gets the session key appended to the request. The request details can be seen when using Wire Shark. That information is used for policy matching.

**Cloud Identity**: Cloud Connectors can synchronize user groups with cloud identity providers (IdP). Currently Citrix SIA supports Google, Azure, and Okta. Configurations differ between each provider but typically consists of specifying the refresh interval, client ID, secret, and domain.

**SAML**: SAML authentication is browser based or associated through the connector. SAML allows authentication to be handed off to a SAML identify provider, whereby the Citrix SIA platform acts as the service provider. SAML authentication must be paired with proxy data redirection and supports Okta, ADFS,

**AD plugin**: a server-side agent that can be installed across the Domain Controllers to provide logon services within the domain. Organziationl Units, security groups, and machines are gathered from the DCs and sent back to Citrix SIA for policy assignment.

**A note on Citrix SIA groups**: Organizations should associate their current security group structure to Citrix SIA groups. It is best practice to keep Citrix SIA groups to a minimum.

Aliases: Aliases can be added to a security group to capture multiple user groups under one larger Citrix SIA group
Multiple groups: When you have users that fall into multiple groups, priority is important. The higher the priority on the group, is what takes precedence for policy enforcement.

Policy Engines: There are 3 different policy engines. When a packet traverses the platform, determining which policy should be applied to the packet occurs in the following order:

*  Step 1 - IP Subnet: The packet is checked against the Local Subnets configured in the network settings based on the packet’s source IP address. If a match is found, the policy group is saved before moving on.
*  Step 2 - Device: The source IP is compared to the list of static and dynamic computers. If a match is found, the policy group from step 1 is replaced with the one found here.
*  Step 3 - User policy: If a user is logged into the device and found to be associated with the device, the group associated with that user overrides the group from step 2.

## Security Features

The containerized gateways scan data in the cloud and perform web filtering, prevent malware, detect infections and prevent data loss as data moves to and from users and the Internet.

### Malware Protection

Citrix SIA uses many different layers of security when it comes to Malware Protection. Malware Protection includes:

*  Malware identification and mitigation from the top signature databases
*  A proprietary malware registry
*  Real-time threat information with instant database updates

Administrators can enable both reputation-based defenses as well as malware protection which utilize advanced content analysis and sand-boxing capabilities.
Streaming Malware Reputation will process URL’s that users are accessing against a signature database and Citrix SIA’s proprietary malware registry as well as up-to-the-minute threat information with synchronous database updates. Advanced Malware Analysis Defense will auto deposit user downloaded files for behavioral analysis.

Malware rules can apply when certain criteria are met and are based off of the Malware settings. When applying malware rules to content, rules are checked from the top to the bottom of the list. As soon as a match is found, the rule is chosen, and no further rules are considered. Higher priority rules should be at the top of the list. Rules can easily be moved up and down to ensure the priority is set to the appropriate order. Websites are given a risk score between 1-100 by analyzing various aspects of a request, including the URL, HTTP headers, and site content. Based on that score, the page is either blocked or allowed depending on the thresholds for Low Risk, Medium Risk, and High Risk.
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

Block List

Website browsing can be blocked using a Block List. Block Lists can be used to restrict access to network resources selectively. Allow Lists allow access to URLS that may be blocked by a web category policy. Allow Lists can be used to grant access to specific resources. Allow Lists are configured much the same as

Block
Below are the various ways to configure Block Lists:

*  URL matching: URLs can be matched using domains, subdomains, IP addresses, IP ranges, and ports
*  Wildcards: Wildcard entries can be used in the block list by omitting the trailing forward slash from the path i.e. “mywebsite-parent-domain.com”
*  Explicit: These are non-wildcard entries; they can be created by appending a trailing forward slash to the pathi.e. “mywebsite-parent-domain.com/”
*  Regular Expressions: Regular expressions can be used to match complex patterns

Domains are tagged with web categories based on their content. The categories of visited websites are recorded in Reporting Analytics.

Citrix SIA has a Scrape Tool that can be used to identify all the domains associated with a website, which then can be used to be added to the Block List.
URL Look Up Tool
Citrix SIA platform has a URL Look Up Tool can be used to:
Check to see how a website is categorized
Check to see if a website is associated with malware
The URL Look Up tool is useful when creating Web Security policies. Additionally, if the tool is unable to categorize the entered URL, it can be submitted for categorization to the platform.

#### Custom Block Pages

A Custom Block Page can be created for users who attempt to access blocked content. In addition, the block page can also apply for those users who try to access the intent while in Sleep Mode. Custom Block pages have the following associated actions:

*  Redirect: Send users to a specific URL
*  Silent Drop: prevents access without responding
*  Allow user to login: bypass block page by logging into an account that is created on the gateway

#### Keywords

Keyword filtering can be used to inspect URLs to find specific words.

*  The Web Gateway can take action to allow or block the user's activity depending on the associated behavior of their searched keyword.
*  The platform includes predefined Adult and High-Risk keyword lists.
*  Administrators can choose to enable the predefine lists on a per-group basis.
*  Alter the predefined lists by enabling or disabling a subset of the words.
*  Elect not to use the predefined lists and instead compile their own list of words.

### Data Loss Prevention

The Data Loss Prevention (DLP) module is an advanced analysis engine that inspects data for potential loss and sensitive information. It includes built-in detection of Personally Identifiable Information (PII), Credit Card information including stripe data, and other content. In addition, custom patterns can be defined to detect content such as data from CRM or database systems.

The DLP module provides protection against unauthorized cloud use and sensitive data loss. This includes protection for the use of cloud services as to ensure sensitive data is secured and maintained within organizationally approved cloud services.
Deep file-based data loss prevention capabilities to detect, alert, and stop the transfer of sensitive data. Advanced detection capabilities detect and protect sensitive information within content by analyzing numerous file types including compressed files.

### Cloud Access Security Broker

The web security module has extensive social media and cloud application controls. Using the Cloud Access Security Broker (CASB) App Controls allow you to manage access to common online applications and social media. Cloud App Controls can be used to restrict activity on popular social media and cloud services. Cloud App Controls can be applied to specific security groups to allow or block items such as posting, liking, following etc.

*  Cloud app control can be used to manage the use of social streaming radio, Pinterest, Facebook, Twitter, LinkedIn, search engines, YouTube, and Google Apps
*  Controls for each of these can be adjusted on a per-group basis
*  Specific Cloud & SaaS services and social media sites require SSL decryption to be enabled

Search Engine Controls allow you to set SafeSearch settings on various search engines. These settings can also be applied globally or to specific groups. Google, Yahoo, and Bing web browser search engines are supported.
You can prevent user from uploading files to a variety of Cloud Services. File upload can be blocked for Dropbox, Box, OneDrive, Google Drive and Generic file uploads
YouTube Manager can restrict content, hide comments, and create a library of whitelisted videos. YouTube Manager settings can be applied globally or to specific security group(s).
Other actions that can be performed by the YouTube manager are:

*  Home Page: You can redirect a user to the Organizations YouTube channel.
*  Bandwidth Preservation: You can force videos to play in Standard Definition
*  Whitelist libraries: You allow access to specific videos that are blocked by the SafeSearch option into a video library

Microsoft CASB integration enables traffic policy control for unsanctioned Apps and CASB reporting visibility all within the Citrix SIA platform. Integration with “Microsoft Cloud App” platform and the platform provides granular traffic policy control and assignments for blocking Unsanctioned Apps as well as CASB reporting within a single pane of glass. Real-time traffic logging and statistics are shipped via API integration to the “Microsoft Cloud App Security” platform. The integration between the platform and Microsoft configuration is achieved by simply entering Microsoft subscription info into the platform
Note: a majority of social media and cloud services will require SSL decryption to be enabled in order to perform Cloud App Controls, YouTube Manager controls, and Search Engine Controls.

### SSL/TLS Decryption

Without SSL Decryption, reporting and analytics become limited. For example, when searching on google without the setting enabled – the SIA platform can report on google.com but not on any of the keywords used in the search query. SSL is not required for block, allow, or monitoring basic HTTP access. With SSL Decryption enabled, the SIA platform can inspect HTTPS traffic thus offering more granular actions and visibility. With SSL Decryption enabled, the connected devices will need the certificate from the platform. The certificate can be distributed during the Cloud Connector install. To verify this, when navigating to a site, the trusted certificate should be seen as being issued by Citrix SIA and not by the site.

Many if not most sites and services on the internet are encrypting their communications with users, with SSL/TLS being the most common protocol used to do so. This makes the ability to inspect SSL/TLS traffic essential for effective internet security. Without doing so, a growing majority of an organization’s traffic will go unsecured, allowing for malware or internal bad actors to exfiltrate corporate data unseen to the gateway.
As a critical but process-intensive task, SSL/TLS decryption can easily overburden traditional security appliances attempting to achieve full SSL/TLS visibility into content and cloud app usage. Citrix SIA has a scalable cloud architecture that expands and contracts as needed. Each customer’s resources are fully containerized. Citrix SIA keeps each organizations’ decryption keys completely isolated from others.
SSL/TLS Decryption is the ability to view inside of a secure HTTPS traffic. Citrix SIA SSL/TLS decryption works by having the Citrix SIA act as a man in the middle between the client machine and SSL/TLS secured site. The Citrix SIA acts as a root certification authority, intercepting SSL requests to legitimate sites, requesting them, signing the received data with its own CA certificate and sending the data to the client.
SSL/TLS Decryption on the CSIA platform works by implementing a “Man in the Middle” security procedure. SSL/TLS decryption can be performed transparently or through a proxy connection, with the only requirement being that the SSL/TLS certificate is deployed on the Cloud Connected device. SSL/TLS certificates can be pushed down through the installer or manually installed.
SSL/TLS Decryption relies on a MITM certificate. The certificate can be generated from the console or can be inputted in PEM format. The certificate is automatically included in the Windows and MacOS cloud connector download. Certificates can also be deployed out by a trusted root CA via any typical method
Prior to enabling SSL decryption, a certificate must be downloaded from the platform and installed on the clients machine.
Decrypting all destinations
Note: When configuring SSL decryption for the first time, it is recommended to start with selective SSL decryption as not all websites will accept SSL decryption.

*  SSL Decryption can be enabled for all destinations are user travels to on the internet. On the other hand, for websites that do not accept SSL decryption, a bypass can set to only selectively decrypt certain destinations. Below are the various types of bypasses that can be set.
*  Bypassing SSL Decryption: Domains
*  Domain bypass can be configured if the site does support SSL Decryption or it is not desirable to set up SSL Decryption for the site. Certain sites can be bypassed by adding the domain to the Selective SSL Decryption.
*  Note: Bypass for a domain includes all of its subdomains as well.
*  Bypassing SSL Decryption: Web Categories
*  It may not be desirable to allow SSL Decryption for certain web categories, especially when it pertains to the privacy of users. If a site belongs to multiple Web Categories, SSL Decryption will be bypassed if any of the Web Categories have the setting for bypass enabled.
*  Bypassing SSL Decryption: Groups
*  SSL Decryption bypass can be configured for specific users/groups from the Group Management section of the console.
*  Bypassing SSL Decryption: IP Address
*  SSL Decryption Bypass can be enabled for devices dependent upon their IP address. IP addresses and IP ranges, public or private, can be added to the bypass list.

### Advanced Web Security

**Keyword filtering** is used for to inspect URLs for certain keywords as well as restrict user searches and phrases. Depending on the outcome of the inspection, content can be allowed or blocked. Keywords can be manually added or be sourced from a pre-defined list within the platform. A Wildcard option can be enabled for multi-word searches/keywords that are substrings of larger words that may violate blocked keywords.
Wildcard example: The keyword is “base.” A wildcard match for “base” will block both the search word “base” and “baseball.”
Port Blocking
Connectivity to network resources can be restricted by certain ports for both UDP and TCP based protocols as well as direction – inbound, outbound, or both. In addition, a Port Block schedule can be applied to restrict access only during particular time(s) of the day.
Port blocking control blocks internet traffic on specified ports, or port ranges.

*  Any traffic using the specified ports will be blocked completely.
*  To clear the Port Blocking controls, switch the enabled toggles for the port ranges you would like to disable to NO.
*  You can choose to always block the port ranges for the selected group or block ports using an Advanced Schedule.

**Browser and OS** Connectivity to network resources can be restricted by Browser and OSes to specific versions. The following actions can be applied to Browser and OS restrictions:

*  Block the following: Blocks internet access for specific browsers and OS versions
*  Allow only the following: Allows internet access for specific browsers and OS versions
*  Move user: Move a user to another security group (if they violated the restrictions) for a particular period of time

**Content and MIME Type**
The Multipurpose Internet Mail Extensions (MIME) type is a standardized way to indicate the nature and format of a document.

*  Browsers often use the MIME type (and not the file extension) to determine how to process a document or domain.
*  The general structure of a MIME type consists of a type and subtype in the format [type]/[subtype].
*  When entering a MIME type, ensure you have no spaces between the type and subtype, for example, "audio/mpeg“
Content type is used to indicate the media type of the requested resource. MIME types help browsers understand how to process files it recovers from a web server. A browser matches its content type to the MIME type. You can control the type of content that can be accessed when browsing. Common types of restrictions include:

*  Image/jpeg
*  Image/gif
*  Image/webp
*  Image/png

**File Extensions** can be restricted to prevent users from downloading files that have specific extensions.

*  The File Extension control block specific file extensions from being downloaded on your network.
*  Each extension may be a maximum of 15 characters in length
*  To remove an extension from the Block list, check the box next to the extension.

For example, an ‘.exe’ file may cause harm if executed by a user and therefore can be restricted from downloading.

**Domain Extension** The domain extension control allows you to block or allow specific domain extensions from being accessed on a per-group basis.

*  Manage the Domain Extensions list for each group by
*  Blocking the domain extensions in the list
*  Only Allow the extensions in the list via the Block or Only Allow Domain Extensions
*  Each extension may be a maximum of 15 characters in length.
*  For example, you may choose to allow only domains that end in ".com" and ".net". Any domains that do not end in those extensions will be blocked
Restrictions can be put in place to prevent users from browsing to specific Top-Level Domains

Note: setting the Top-Level Domain restriction does not prevent a user from brute force browsing by IP address

Policy layers offer more advanced control over Web Security Policies. You can apply Allow Lists, Block
Lists, Web Category Filters, SSL Decryption, and other settings based on a variety of preconfigured criteria. This criterion includes but is not limited to:

*  Username
*  Security group
*  IP ranges
*  GeoIP

Policy Layers contain the concept of dynamic linking, whereby the policies will only be applied if all or any of the criteria conditions are triggered. In addition, the policy layer can be set on advanced schedule – similar to that of the Internet Sleep Mode.
Proxy Rules to trigger actions in response to certain types of network activity.

*  Proxy rules are created by associating them with match patterns
*  Match patterns and rules are defined independently of each other
*  This logical separation frees them to be associated with each other in many different combinations, maximizing the logical possibilities
*  A rule is triggered when the state of the match patterns specified by the rule is evaluated as “true"
*  Actions include domain redirection, bypassing or enforcing proxy authentication, and even modifying HTTP Request headers

### Flow

Citrix SIA allows you to route network data to the cloud to perform network security functions without the need for in-line appliances. The diagram below details the process flow from beginning to end.

[CSIA process flow](/en-us/tech-zone/learn/media/secure-internet-access-flow.png)

1.  User selects a link or enters a URL in local web browser
2.  CSIA agent identifies the user’s and Active Directory group membership
3.  CSIA analyzes whether the domain or URL is to be bypassed in the PAC script or agent. If so, the request is bypassed from cloud security.
4.  CSIA analyzes whether the request is blocked by the IPS. If so, the request is blocked.
5.  CSIA tags the username and policy group to the request.
6.  CSIA analyzes whether the request should be decrypted. If so, the request is decrypted, and a certificate is added to the traffic flow.
7.  CSIA analyzes whether the request matches a proxy block rule. If so, the request is blocked.
8.  CSIA analyzes whether the request matches an App-ID CASB block rule. If so, the request is blocked.
9.  CSIA analyzes whether the request matches a DLP block rule. If so, the request is blocked.
10.  CSIA analyzes whether the destination matches an entry in the Allow List. If so, the request is checked against the Block List. If a higher weight Block List rule exists, the request is blocked. If not, the request is allowed.
11.  CSIA analyzes whether the destination matches an entry in the Block List. If so, the request is blocked.
12.  CSIA analyzes whether the destination matches a blocked web category. If so, the request is blocked.
13.  CSIA analyzes whether the destination’s IP address is blocked by GEO IP rules. If so, the request is blocked.
14.  CSIA analyzes whether the request matches any remaining web security policies. If so, the request is blocked. If not, the request is allowed.

## Citrix SD-WAN + SIA Integration Use cases

Citrix SD-WAN and Citrix SIA integration offers flexibility and choice for a mixed profile of branch users in an enterprise. An enterprise typically has a mix of managed and unmanaged devices in the branch where a Citrix SD-WAN exists. With the integration, the Citrix SIA agent allows to securely breakout managed devices traffic to the Citrix SIA cloud via the SD-WAN using the internet service (with Load Balancing). The unmanaged devices like BYOD and Guest users are secured using the IPsec tunnel between Citrix SD-WAN and Citrix SIA as the tunnel endpoints.

There several ways to secure users as they access cloud and SaaS apps and those methods cover cases where the user sits behind an SD-WAN appliance weather at the branch and at home or whether the user is fully mobile, working for example from a coffee shop.

For a user sitting at a corporate office, SD-WAN automatically creates secure connecivity to the closest Citrix SIA point of presence. Traffic is tunnelled via a GRE or IPsec tunnel and redundancy is achieved both a the tunnel level as well as via multiple links to primary and secondary pops.

If a user leaves the corporate perimeter and sits at a coffee shops for example, working off of her tablet, the Cloud Connector installed on the device takes care of redirecting traffic to the Citrix SIA cloud. The connector also serves the purpse of authenticating the user as well as installing appropriate certificates for SSL decryption.

For configuration information, please read the following PoC guide.

[CSIA and SD-WAN PoC Guide](/en-us/tech-zone/learn/poc-guides/secure-internet-access-sdwan.html)

## Integration with CVAD

Citrix Virtual Apps & Desktop deployment can be integrated with Citrix Secure Internet Access. Users within the virtual sessions will 
For configuration information, please read the following PoC guide.

[CSIA and CVAD PoC Guide](/en-us/tech-zone/learn/poc-guides/secure-internet-access-cvad.html)

## Reporting

Reporting
UAT

UAT is an important part of the deployment process after the Cloud Connector agents have been deployed out. The reporter is helpful for validating configurations and looking into any issues.

Users groups and device management

The need to stream log event data to external SIEMs or logging databases is a typical requirement for organizations. The SIEMs typically have processes built around them that are extensive and required. Since users are always connected to Citrix SIA, regardless of location, their internet data is always secure and log events are continuously generated. Those log events are displayed in the included reporting dashboards
of Citrix SIA but can also be streamed in real-time to any external SIEM

security framework will provide unique centralized reporting of threat information with username, IP, and computer name identifiers. The Threat Console reporter will provide threat and network information in an efficient and effective manner
