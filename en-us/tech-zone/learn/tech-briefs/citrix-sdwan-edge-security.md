---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Ravi Pegada, Valerie DiMartino, Reshma Padmanabha
description: Learn how Citrix SD-WAN Advanced Edition provides Edge Security for your Enterprise network.
tz_title: SD-WAN Edge Security
tz_products: citrix-networking;
---
# Tech Brief: SD-WAN Edge Security

## Introduction

With the continued growth of cloud and SaaS, Enterprises are searching for solutions to provide optimal Internet access irrespective of where the endpoint is located. Citrix SD-WAN excels at steering intranet traffic across the WAN or Internet traffic, at the Edge. It applies business rules to identify applications using a database of 4,500+ predefined entries.

However, to provide direct Internet access Enterprises also need to secure remote offices to protect the intranet. Direct Internet access exposes remote networks, and next the entire corporate network, to threats. Ingress and egress traffic must be filtered, inspected, and scanned to protect against vulnerabilities.  

Citrix SD-WAN has a native [onboard ICSA certified firewall](https://www.citrix.com/blogs/2019/08/13/do-you-trust-your-sd-wan-firewall/). It integrates with leading third party [Secure Web Gateway (SWG)](/en-us/citrix-sd-wan/11-2/internet-service/dia-with-secure-web-gateway.html) vendors via cloud proxy. Citrix SD-WAN also hosts onboard SWGs as a [Virtual Network Function (VNF)](https://www.citrix.com/blogs/2020/05/14/citrix-check-point-software-support-choice-in-protecting-the-wan-edge/). Security partners include:

*  [iboss](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/iboss-integration.html)
*  [Check Point](/en-us/citrix-sd-wan/11-2/hosted-firewalls/checkpoint-integration-on-1100-platform.html)
*  [Palo Alto](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/palo-alto-integration-by-using-ipsec--tunnels.html)
*  [Forcepoint](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/firewall-traffic-redirection-support-by-using-forcepoint.html)
*  [Zscaler](/en-us/citrix-sd-wan/11-2/security/citrix-sd-wan-secure-web-gateway/sd-wan-web-secure-gateway-using-gre-tunnels-and-ipsec-tunnels.html)

Now Citrix has added native onboard Next Generation Firewall – Edge Security functionality to the arsenal of SD-WAN protection options. The release of Citrix SD-WAN version 11.2 Advanced Edition combines full stack security functionality, with its application performance enhancement capabilities, including:

*  [Intrusion Prevention](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
*  [Web Filtering](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)
*  [Malware Protection](/en-us/tech-zone/design/design-decisions/citrix-sdwan-home-office.html)

As a result, Enterprises are now able to enhance application performance securely from the edge of their network entirely with Citrix. They also can confidently consolidate their Branch Office networks with a Citrix SD-WAN appliance that can provide steering to Internet services while inspecting traffic, and protecting it. Enterprises also can manage and monitor Edge Security through a single plane of glass with Citrix Cloud hosted Orchestrator service.

## Getting Started

With only a handful of requirements, Citrix SD-WAN customers can quickly get up and running with Advanced Edition - Edge Security:

*  [Citrix Orchestrator](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/security.html#intrusion-prevention)
*  [11.2+ software](/en-us/citrix-sd-wan/11-2.html)
*  [1100 AE appliance](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-sd-wan-data-sheet.pdf)
*  [Advanced Edition (AE) license](/en-us/citrix-sd-wan/11-2.html#citrix-sd-wan-platform-editions)
*  [Firewall Security Profile](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#security-profiles)
*  [Firewall Policy Action](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#edge-security-firewall-policy)

### Network Configuration – Software Version 11.2.0.88 & 1100 appliance

![Network Configuration](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_networkconfiguration.png)

### Basic Settings – Device Edition AE

![Basic Settings](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_basicsettings.png)

### Firewall Security Profile

![Firewall Security Profile](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_firewallsecurityprofile.png)

### Firewall Profile Action

![Firewall Profile Action](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_firewallpolicyaction.png)

## Intrusion Prevention

Intrusion Prevention Systems (IPS) mitigate the risk of threats to networks by identifying, logging and or blocking known attacks.  The patterns of attacks are regularly updated and added to a common database. Citrix SD-WAN Edge Protection – Intrusion Prevention includes over 34,000 signature detections and heuristic signatures for port scans.

![Intrusion Prevention](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_intrustionprevention.png)

Citrix SD-WAN Edge Security Intrusion Prevention functionality is powered by the [Suricata](https://suricata-ids.org/) open source network threat detection engine. Suricata is owned and supported by the Open Information Security Foundation (OISF). It inspects the network traffic after passing through the firewall using extensive rules and a powerful signature language.

Rules look for attacks such as

*  Port scans where bots or bad actors test for open ports susceptible to penetration.
*  Any traffic from known “bad” public IP addresses.
*  Denial of Service attacks where attackers seek to overrun network bandwidth and receive buffers by sending certain protocol sequences excessively or a high volume of flows.

![Intrusion Prevention Profile](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_ipsprofile.png)

Citrix SD-WAN Intrusion Prevention includes predefined rules grouped into one of four priority categories based on severity:

*  Critical
*  High
*  Medium
*  Low

![Intrusion Prevention Rule](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_ipsrule.png)

Rules, which are customizable, include fields to specify patterns to identify in a logical statement. Once matched the following actions can be taken:

*  Recommended - Perform the recommended action defined for the signatures.
*  Enable Log - Allow and log traffic that matches any of the signatures in the rule.
*  allow list - The signature’s source and destination networks are modified to exclude networks defined by the allow list variable.
*  Disable - The signatures are disabled. Allow the traffic to continue to the destination without logging.
*  Enable Block - Drop the traffic that matches any of the signatures in the rule.
*  Enable Block if Recommended is Enabled - If the rule action is Recommended, and the signature’s recommended action is Enable Log, drop the traffic matching any of the signatures in the rule.

You can maintain visibility by reporting on Intrustion Prevention events.

![Intrustion Prevention Events](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_intrusionpreventionreport.png)

For more information see [Citrix SD-WAN Edge Security – Intrusion Prevention](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/security.html#intrusion-prevention)

## Web Filtering

Web Filtering mitigates the risk of unsuspecting users clicking links on sites behind known “unsafe” domains and infecting their endpoints with malware, viruses, ransomware, or other threats. It also helps protect users from accessing sites that would violate compliance rules or the Enterprise Acceptable Use Policy.

![Web Filtering](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_webfiltering.png)

Citrix SD-WAN utilizes [Webroot’s BrightCloud](https://www.webroot.com/us/en/business/threat-intelligence/internet/web-classification-and-reputation-services) machine learning-based Web Classification and Web Reputation engine to filter domain names. It includes over 32 billion URLs and 750 million domains. When users visit a site, the URL is sent for classification. A temporary local cache is maintained to expedite the lookup the next time the URL is requested.

Web filtering is enabled, and preferences are configured within a Security Profile.

![Web Filtering Security Profile](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_webfilteringsecurityprofile.png)

Existing categories can be selected to “block” access to the website or “flag” (log) access. Alternatively, specific sites may be “bypassed” by domain name or IP address.

![Web Filtering Categories](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_webfilteringsecuritycategories.png)

You can maintain visibility by reporting on Web Filtering events.

![Web Filtering Events](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_webfilteringreport.png)

For more information see [Citrix SD-WAN Edge Security – Web Filtering](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#web-filtering)

## Anti-Malware

Anti-Malware protects users against ransomware and viruses. It guards against files delivered by HTTP download or opened from SMTP delivered email. Powered by [Bitdefender](https://www.bitdefender.com/), it assembles and scans files destine for users through email or HTTP downloads. Anti-Malware can then block the file delivery, log its presence, and or send a notification regarding the risk identified.

![Anti-Malware](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_antimalware.png)

Citrix SD-WAN Anti-Malware takes a four-step process to evaluate files. It can scan 41 file-type extensions and 10 MIME types for email content. If the file fails any of the tests below it is considered malware and the download is blocked:

1.  Query a threat intelligence database for information about the file metadata based on its fingerprint.
1.  A local scan using Bitdefender's signature database is run on the server while the cloud lookup is being performed.
1.  Perform a heuristic scan to look for suspicious patterns in executable files.
1.  Lastly, evaluate code in an emulator and look for malicious activity.

![Anti-Malware Security Profile](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_antimalwaresecurityprofile.png)

Anti-Malware can be enabled or disabled for specific file types by selecting their extension.

![Anti-Malware File Types](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_antimalwarefiletypes.png)

Also, you can choose to exclude certain MIME types from scanning.

![Anti-Malware MIME Types](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_antimalwaremimetypes.png)

You can maintain visibility by reporting on Anti-Malware events.

![Anti-Malware Events](/en-us/tech-zone/learn/media/tech-briefs_citrix-sdwan-edge-security_antimalwarereport.png)

For more information see [Citrix SD-WAN Edge Security – Anti-Malware](/en-us/citrix-sd-wan-orchestrator/network-level-configuration/edge-security.html#anti-malware)

## Differentiation

With Edge Security, that comes with 11.2 Advanced Edition, Citrix is now one of the few SD-WAN vendors that provides Intrusion Detection, Web Filtering, and Anti-Malware natively in On-premises appliances. With Edge Security Citrix SD-WAN offers significant value to Enterprises including:

*  **Maximize User Experience & Minimize Network Bandwidth** – Enterprises no longer must backhaul internet traffic over their Wan to meet compliance requirements and guard against the perils of the Internet. Now they can steer flows to the nearest business SaaS sites while protecting endpoints.
*  **Secure the Enterprise Perimeter** – while direct Internet access enables better application performance it exposes branches and subsequently the corporate network to risk. Edge Security mitigates that risk by guarding against vulnerabilities at the point of remote office Internet access.
*  **Branch Office consolidation** – Enterprises can simplify the equipment they must manage and host in remote office utility closets. All of the following equipment functionality can be replaced with a Citrix SD-WAN 1100 AE appliance:
    *  Stand alone routers
    *  SWG appliances and firewalls
    *  DHCP and DNS servers
    *  WAN Optimization appliances
*  **Single Point of Contact** – customers can contact Citrix regarding Network forwarding or Network security. Having a single point of contact avoids finger pointing if complex issues arise. Enterprises can partner with Citrix Services to support operations.
*  **Simplified Monitoring & Management** -  with Citrix Cloud hosted Orchestrator service Enterprise admins have an easy to use dashboard to onboard, configure, and monitor their Citrix SD-WAN Edge Security implementations.

## Summary

Enterprises can provide a great Internet service user experience while securing their intranet at Edge egress points with **Citrix SD-WAN Edge Security**. With the **11.2 Advanced Edition** Citrix provides **Intrusion Prevention, Web Filtering, and Anti-Malware** on the **SD-WAN 1100 AE appliance**. User endpoints are protected in Branch Offices, while Administrators manage and monitor their network centrally through the Citrix Cloud hosted Orchestrator service.
