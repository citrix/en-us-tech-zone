---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Frank Bunger
description: With cyber threats rising and modern application architectures getting more complex, organizations need a more straightforward way to defend against bots, DDoS, zero-day exploits, and other attacks. Learn how Citrix Web App and API Protection (CWAAP) service can provide effective security against these attacks.
tz_title: Citrix Web App and API Protection service
tz_products: citrix-networking;
---
# Citrix Web App and API Protection service

## Introduction

Enterprises continue to migrate their web apps and APIs to the Cloud to better support work from home employees, fueled by the pandemic. At the same time, they still support business-critical web apps hosted On-Premises or in Private Clouds. Along with the growth in these internet-facing services, there has been growth in internet-borne threats.

Bad actors continue to develop new ways to compromise them to support their illicit activities. Protecting applications from attack requires rapid identification of threats and launching of countermeasures. Citrix Web App and API Protection (CWAAP) service provides a consistent security posture and comprehensive protection for Monolithic applications On-Premises or microservices in the Cloud.

## Overview

CWAAP service guards against their attempts to exfiltrate, manipulate or destroy online Enterprise web apps, APIs and data. It includes a comprehensive suite of technology to mitigate the risk to business-critical operations.

*  Web Application Firewall - short for web application firewall, "WAF" origins are in protecting web sites hosted on-premises. However, in the Cloud Era the scope of WAFs are expanded to hybrid cloud or public cloud environments with broader attack surfaces to protect. Protection also extends to APIs used by Cloud-native systems to communicate between distributed functions that operate independently in containers. Also know as microservices, this form of development is more efficient in many ways than traditional monolithic architecture, yet by nature is primarily hosted in the Cloud, and subsequently, APIs are vulnerable to attack.
*  DDOS Protection - Distributed Denial of Service (DDOS) Protection guards against internet-born attacks that seek to disrupt access to cloud services. Typically initiated by bad actors through BOT armies, DDOS attacks focus on consuming available resources to the point customer access is disrupted or denied.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_cwaapservice.png)

## WAF Protection

Hosted in the Cloud or on Premises Citrix WAF protects against known and unknown attacks, including application-layer and zero-day threats. It includes protections against Top 10 Web Application security risks, defined by the [OWASP Foundation](https://owasp.org/), at its core and expands on those with ever-growing security definitions and countermeasures from multiple threat research sources.

Large enterprises may have hundreds or thousands of applications online that need to be secured. Therefore matching attacks, against unique web app flows to different applications at scale is a challenge. Citrix WAF automates Protection against internet-borne attacks keeping traffic at the Cloud or On-Premises edge. It detects and mitigates incessant online threats around the clock. This allows Security operations to focus more on strategic security activities and address vulnerabilities elsewhere in the infrastructure.

Citrix WAF works on both a positive and negative attack model. The positive model identifies zero-day threats by looking for abnormal activity patterns. The negative model identifies previously documented attack signatures.

### Negative Security Model

Injection flaws, such as SQL and LDAP injection, have been a perennial favorite among hackers, along with Cross-site scripting attacks. Other web attack protections include:

#### Core

*  HTML SQL Injection - provides defenses against injection of unauthorized SQL code that might break security.
*  HTML Cross-Site Scripting - examines both the headers and the POST bodies of user requests for possible cross-site scripting attacks.
*  Cross-Site Request Forgery (CSRF) Form Tagging - tags each web form sent by a protected website to users with a unique and unpredictable FormID and then examines the web forms returned by users to ensure that the supplied FormID is correct.
*  Buffer Overflow Check - detects attempts to cause a buffer overflow such as URLs, cookies, or headers that are longer than the configured length.

#### Advanced

*  Cookie Consistency - examines cookies returned by users to verify that they match the cookies that the protected website set for that user. If a modified cookie is found, it is stripped from the request before the request is forwarded to the webserver.
*  Field Consistency - examines the web forms returned by users in response to HTML requests and verifies that unauthorized changes to the structure were not made.
*  Field Format - verifies the data that users send, including the length and type.
*  Content-Type - ensures the Content-Type headers are either `“application/x-www-form-urlencoded”, “multipart/form-data,” or “text/x-gwt-rpc”` types. Any request that has any other content type designated is blocked.
*  HTTP RFC - inspects the incoming traffic for HTTP RFC protocol compliance and drops any request with RFC violations.
*  Deny URL - examines and blocks connections to URLs commonly accessed by hackers and malicious code.
*  POST Body Limit - Limits the request payload (in bytes) inspected for signatures.

#### XML

*  XML SQL Injection - examines and blocks user requests with injected SQL in XML payloads.
*  XML XSS - examines and blocks user requests with cross-site scripting attacks in XML payloads.
*  XML Format - examines and blocks incoming requests that are not well-formed or that do not meet the specification for properly-formed XML documents.
*  XML SOAP Fault - examines responses from your protected web services and filters out XML SOAP faults. This prevents the leaking of sensitive information to attackers.
*  Web Service Interoperability - examines and blocks requests and responses that do not adhere to the WS-I standard and might not interact with XML application appropriately.

### Positive Security Model

Citrix WAF supports a positive protection model with learning rules that build on the negative model by creating a profile of allowed traffic. Advanced heuristics analyze traffic to identify standard behavior and make recommendations for tuning of countermeasures. This helps ensure Protection against zero-day attacks that are not addressed in existing signatures.

## DDOS Management

DDOS attacks focus on disruption through resource consumption. There are three main groups of DDOS attacks:

1.  Volume-based attacks - attempt to disrupt regular service by overwhelming sites with a flood of traffic. These include UDP or ICMP floods that try to exceed available bandwidth, denying legitimate user requests from ever reaching the target site.
2.  Protocol-based attacks - focus on the transport layer and take advantage of protocol operations. Packets are typically "spoofed" or sent with fake IP header information to illicit a response to consuming site resources. A typical example is a "SYN" attack. With this type of attack, the bad actor's host, or a BOT, sends a request to establish a TCP layer session. A TCP session is ultimately required for legitimate users to establish an HTTP session to use the web service. However, the initiating host never responds to the site's SYN ACK yet sends more SYNs, steadily consuming memory on the web service as it waits to establish TCP sessions.
3.  Application layer attacks - these attacks focus on the application layer and seek to overcome resources or take advantage of web server vulnerabilities. The attacks try to illicit, excessive site responses by manipulating protocol communication with crafted GETs or POSTs.

The Citrix DDoS solution provides a holistic solution to DDOS protection buyers. It is an always-on DDoS attack management service. It features one of the world's largest scrubbing networks with 14 POPs and 12 Tbps capacity that protects applications from large-scale volumetric DDoS attacks.

## Management

CWAAP is configured and managed through a flexible SaaS portal. The CWAAP service portal, accessible through a browser, enables security admins to configure attack protections, monitor attack activity through a dashboard, or report on events.

### Setup

CWAAP is setup with the help of Citrix using one of two methods:

*  DNS - using DNS is the most common and easiest method. Customers direct their site A records to a Citrix CWAAP domain setup for their site. With this method, customers maintain control and can schedule a quick transition using a low TTL setting for the record.
*  BGP - using BGP requires customers to transfer control of their pertinent routing block to Citrix. Then Citrix announces the route on behalf of the customer, and traffic to target sites without the block is directed to the CWAAP for inspection.

### Configuration

CWAAP WAF policies and custom rules are configurable through the SaaS portal:

*  WAF Policies - there are three primary sets of policies, and when detected, administrators can configure them to block, log, or both. Core policies are the most common types of attacks, including SQL injection, cross-site scripting, and buffer overflow. Advanced are more complex attacks using cookies or trying to take advantage of the HTTP protocol, while the last group pertains to XML-specific attacks. Signatures to identify the attacks are continuously developed by a Citrix research team and are updated automatically when published.
*  Responder Policies - admins may configure custom rules to drop, log, or redirect connections based on specific parameters such as the hostname, source IP address, or destination port.
*  Network Controls - admins may configure ACLs to block specific IP address ranges or traffic from specific countries.
*  Alerts - admins may configure custom alerts when the amount of traffic within a certain interval is exceeded from a specific parameter such as the same ASN, country, or User-Agent.
*  Trusted Sources - admins may configure an allow list of trusted sources that bypass inspection policies.
*  Assets - admins configure the target site or sites these configured policies should apply to.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_portalpolicies.png)

### Monitoring

The CWAAP Portal gives Citrix admins the ability to monitor CWAAP Protections centrally. The main dashboard provides an aggregate view of site volume, including total traffic, broken down by clean traffic and mitigated traffic.

It also includes a detailed log of attacks, including volume, duration, and countermeasures deployed.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_cwaapsvcmgmt.png)

## Summary

Enterprises' use of cloud services and mobile applications continues to drive the growth of cloud-native web and API traffic while maintaining some business-critical web apps On-Premises. With this growth in apps and APIs online, they are increasingly vulnerable to the persistent attacks that come with being hosted on the public internet. Citrix Web App and API Security service is constantly evolving to stay ahead of these complex attacks. It includes an advanced suite of technology based on Machine Learning and Artificial Intelligence to protect these critical Enterprise services in the Cloud and On-Premises.

## References

Learn more at [Citrix® Web App and API Protection™](https://www.citrix.com/products/citrix-web-app-and-api-protection/)
