---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
description: With cyberthreats rising and modern application architectures getting more complex, organizations need a simpler way to defend against bots, DDoS, zero-day exploits, and other attacks. Learn how Citrix Web App and API Protection (CWAAP) can provide effective security against these attacks.
---
# Citrix Web App and API Protection (CWAAP) service

## Introduction

Enterprises continue to migrate their web apps and apis to the Cloud to better support work from home employees, fueled by the pandemic. At the same time they still support business critical web apps hosted On Premises or in Private Clouds. Along with the growth in these internet facing services there has been growth internet borne threats.

Bad actors continue to develop new ways to compromise them to support their illicit activities. Protecting applications from attack, requires rapid identification of threats and launching of countermeasures. CWAAP provides a consistent security posture and comprehensive protection for Monolithic applications On Premises or microservices in the Cloud.

## Overview

Citrix Web App and Api Protection (CWAAP) guards against their attempts to exfiltrate, manipulate or destroy online Enterprise apps, APIs and data. It includes a comprehensive suite of technology to mitigate the risk to business critical operations. It includes four distinct sets of protection technology that work together to provide dynamic protection:

1.  Web Application Firewall - short for web application firewall, "WAF" origins are in protecting web sites hosted on premises, but now as sites are expaned or migrated to hybrid cloud or public cloud environment they have a broader attack surface to protect.
2.  API Protection - short for application programming interface, "APIs" have been around since the early days of digital computing, yet now in the Cloud Era have taken on a new signficance. Cloud native systems are designed to distribute functions to operate independently in containers and communicate over APIs. Also know as microservices, this form of development is more efficent in many ways than traditional monolithic architecture, yet by nature is primarily hosted in the cloud and subsequently APIs are vulerable to attack.
3.  BOT Protection - short for robot, a "BOT" bot simulates human activity. There are good ones such as crawler bots run by Google to categorize their search engine content. There are also bad ones such as hacker bots that incessantly probe sites for vulnerabilities, scraper bots that steal and or manipuate content in an attempt to profit illegally, or botnets that attempt to deny service to users to hurt a company financially or otherwise.
4.  DDOS Protection - Distributed Denial of Service (DDOS) Protection guards against internet born attacks that seek to disrupt access to cloud services. Typically initiated by bad actors through BOT armies, DDOS attacks focus on consuming available resources to the point customer access is disrupted or denied.

## WAF Protection

Hosted in the Cloud or on Premises Citrix WAF protects against known and unknown attacks including application-layer and zero-day threats. It inclues protections against Top 10 Web Application security risks, defined by the [OWASP Foundation](https://owasp.org/), at it's core and expands on those with ever growing security definitions and counter measures from multiple threat research sources.

Large enterprises may have hundreds or thousands of applications online that need to be secured. Therefore matching attacks, against unique web app flows to different applications, at scale is a challenge.  Citrix WAF automates protection against internet borne attacks keeping traffic at the cloud or On Premises edge. It detects and mitigates incessant onine threats around the clock. This allows Security operations to focus more on strategic security activities and address vulnerabilities elsewhere in the infrastructure.  

Citrix WAF works on both a postive and negative attack model. The positive model identifies zero-day threats by looking for abnormal activity patterns. The negative model identifes previously documented attack signatures.

### Negative Model

Injection flaws, such as SQL and LDAP injection, have been a perennial favorite among hackers along with Cross site scripting. Other web attack protections include:

*  [HTML Cross-Site Scripting Check](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/html-cross-site-scripting-check.html)
*  [HTML SQL Injection Checks](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/html-sql-injection-check.html)
*  [SQL grammar-based protection for HTML and JSON payload](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/sql-grammar-based-protection-for-html-and-json-payload.html)
*  [Relaxation and deny rules for handling HTML SQL injection attacks](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/relaxtion-and-deny-rules-for-html-sql-injection-attack.html)
*  [HTML Command Injection Protection](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/command-injection-protection-check.html)
*  [JSON Command Injection Protection](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/json-command-injection-projection-check.html)
*  [XML External Entity Protection](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/xml-entity-attack-protection.html)
*  [Buffer Overflow Check](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/buffer-over-flow-check.html)
*  [Web App Firewall Support for Google Web Toolkit](https://docs.citrix.com/en-us/citrix-adc/current-release/application-firewall/top-level-protections/securing-google-web-toolkit-applications.html)

### Positive Model

Learning rules

## API Protections

APIs have similarities to Web Apps in that they receive TCP connections on specific open ports, yet have some unique differences.

Many APIs contain vulnerabilities due to coding, thereby exposing sensitive data, such as financial, and PII. Attackers may steal or modify such weakly protected data to conduct fraud, or other crimes. Sensitive data may be compromised without extra protection, such as encryption at rest or in transit, and requires special precautions when exchanged with the browser on the users endpoint.

1.  APIs have unique paths or routes to cloud native microservices. Typically defined according to the Open Api Specification (OAS) in Swagger files API routes

## BOT Management

BOT Groups

1.  Simple (high volume / low risk) - attacks are low hanging fruit for bad actors. They can be identifed by source IP address from reputation lists, HTTP header tags, or signatures. They can be mitigated with rate limiting, or blocked with acls.

*  Thousands of BOT signatures
*  5 Million+ IP addresses in IP reputation database that's updated continuously
*  Blacklist (ie: specific IP address origin domains)
*  White list (ie: Google Search Crawler, etc.)
*  Bot Traps identifies automated BOTs that select a 1 pixel image it injects, that users would not see

1.  Moderate (medium volume / medium risk) - attacks raise the stakes.

*  
*  Captcha

1.  Sofisticated - create fake accounts, hoard inventory, password stuffing or spraying

*  Moving averages - uses statistics
*  Heuristics -
*  Machine Learning -

## DDOS Management

DDOS attacks focus on disruption through resource consumption. There are three main groups of DDOS attacks:

1.  Volume based attacks - attempt to disrupt normal service by overwhelming sites with a flood of traffic. These include UDP or ICMP floods that simply try to exceed available bandwidth denying legitimate user request from ever reaching the target site.
2.  Protocol based attacks - focus on the transport layer and take advantage of protocol operations. Packets are typically "spoofed" or sent with fake IP header information to illicit a response to consume site resources. A common example is a "SYN" attack. With this type of attack the bad actor's host, or a BOT, sends a request to establish a TCP layer session. A TCP session is ultimately required for legitimate users to establish an HTTP session to use the web service. However, the initiating host never responds to the sites SYN ACK, yet sends more SYNs, steadily consuming memory on the web service as it waits to establish TCP sessions.
3.  Application layer attacks - these attacks focus on the application layer and seek to overcome resources or take advantage of web server vulnerabilities. They attacks try to illicit excessive site responses by manipulating protocol communication with crafted GETs or POSTs.

The Citrix DDoS solution provides a holistics solution to DDOS protection buyers. It is an always-on DDoS attack management service. It features one of the world’s largest scrubbing networks with 12 Tbps capacity that protects applications from large scale DDoS attacks.

## Management

CWAAP can be configured, and monitored in several flexible ways to meet your On Premises, Cloud and Hybrid Cloud requirements. They include directly through the Citrix ADC User Interface, through the Citrix Cloud ADM service, or through the CWAAP service portal.

*  Citrix ADC - can be configured through a GUI using a mouse and browser, or CLI using text commands. Both are reliable and intuitive options that have been a part of the platform and evolved since it's inception over a decade ago.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_adcmgmt.png)

*  Citrix ADM - is a powerful part of Citrix Analytics technology hosted in Citrix Cloud. It easily scales the management of multiple Citrix ADCs. It also simplifies the configuration and monitoring of WAF , BOT , and API Protections.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_admmgmt.png)

*  CWAAP Portal - is a SaaS portal that provides the abilility to configure CWAAP in the cloud. It gives Citrix admins the ability configure and monitor CWAAP Protections centrally. While with dozens of POPs around the global it gives uses nearby access to have their web requests seamlessly proxied and inpsected.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_cwaapsvcmgmt.png)

(`*` Some protections may not available for configuration through all interfaces and are subject to change)

## Summary

Enterprises use of cloud services and mobile applications continues to drive the growth of cloud native web and api traffic, while maintaining some business critical web apps On Premises. With this growth in apps and data online they are increasingly vulnerable to the persistent attacks that come with being hosted on the public internet. Citrix Web App and API Security (CWAAP)is constantly evolving to stay ahead of these complex attacks. It includes an advanced suite of technology, based on Machine Learning and Artificial Intelligence, to protect these critical Enterprise services in the cloud and On Premises.

## References

Learn more at [Citrix® Web App and API Protection™](https://www.citrix.com/products/citrix-web-app-and-api-protection/)
