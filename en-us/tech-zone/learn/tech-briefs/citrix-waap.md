---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
description: With cyberthreats rising and modern application architectures getting more complex, organizations need a simpler way to defend against bots, DDoS, zero-day exploits, and other attacks. Learn how Citrix Web App and API (CWAAP), delivered as-a-service, can provide effective security against these attacks.
---
# Citrix Web App and API Protection (CWAAP)

## Introduction

Enterprises continue to move their apps and data to the cloud to support work from home employees, fueled by the pandemic. At the same time bad actors continue to develop new ways to attack those apps and data to support their illicit activities.

Citrix Web App and Api Protection (CWAAP) guards against their attempts to exfiltrate, manipulate or destroy these business critical apps and data. CWAAP technology, based on Machine Learning and Artificial Intelligence, does this by constantly evolving to stay ahead of these increasingly complex attacks.

## Overview

It includes four distinct sets of protection technology that work together to provide dynamic protection:

1.  Web Application Firewall - short for web application firewall, "WAF" origins are in protecting web sites hosted on premises, but now as sites are expaned or migrated to hybrid cloud or public cloud environment they have a broader attack surface to protect.
2.  API Protection - short for application programming interface, "APIs" have been around since the early days of digital computing, yet now in the Cloud Era have taken on a new signficance. Cloud native systems are designed to distribute functions to operate independently in containers and communicate over APIs. Also know as microservices, this form of development is more efficent in many ways than traditional monolithic architecture, yet by nature is primarily hosted in the cloud and subsequently APIs are vulerable to attack.
3.  BOT Protection - short for robot, a "BOT" bot simulates human activity. There are good ones such as crawler bots run by Google to categorize their search engine content. There are also bad ones such as hacker bots that incessantly probe sites for vulnerabilities, scraper bots that steal and or manipuate content in an attempt to profit illegally, or botnets that attempt to deny service to users to hurt a company financially or otherwise.
4.  DDOS Protection - Distributed Denial of Service (DDOS) Protection guards against internet born attacks that seek to disrupt access to cloud services. Typically initiated by bad actors through BOT armies, DDOS attacks focus on consuming available resources to the point customer access is disrupted or denied.

## WAF Protection

WAF works on both a postive and negative attack model. The positive model identifies zero-day threats by looking for abnormal activity patterns. The negative model identifes previously documented attack signatures.

OWASP

Signatures

## API Protections

APIs have similarities to Web Apps in that they receive TCP connections on specific open ports, yet have some unique differences.

1.  APIs have unique paths or routes to cloud native microservices. Typically defined according to the Open Api Specification (OAS) in Swagger files API routes

## BOT Protections

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

## DDOS Protections

DDOS focus on disruption through resource consumption. There are three main groups of DDOS attacks:

1.  Volume based attacks - attempt to disrupt normal service by overwhelming sites with a flood of traffic. These include UDP or ICMP floods that simply try to exceed available bandwidth denying legitimate user request from ever reaching the target site.
2.  Protocol based attacks - focus on the transport layer and take advantage of protocol operations. Packets are typically "spoofed" or sent with fake IP header information to illicit a response to consume site resources. A common example is a "SYN" attack. With this type of attack the bad actor's host, or a BOT, sends a request to establish a TCP layer session. A TCP session is ultimately required for legitimate users to establish an HTTP session to use the web service. However, the initiating host never responds to the sites SYN ACK, yet sends more SYNs, steadily consuming memory on the web service as it waits to establish TCP sessions.
3.  Application layer attacks - these attacks focus on the application layer and seek to overcome resources or take advantage of web server vulnerabilities. They attacks try to illicit excessive site responses by manipulating protocol communication with crafted GETs or POSTs.

## Management

CWAAP can be configured, and monitored in several flexible ways to meet your On Premises, Cloud and Hybrid Cloud requirements. They include directly through the Citrix ADC User Interface, through the Citrix Cloud ADM service, or through the CWAAP service portal.

*  Citrix ADC - can be configured through a GUI using a mouse and browser, or CLI using text commands. Both are reliable and intuitive options that have been a part of the platform and evolved since it's inception over a decade ago.

*  Citrix ADM - is a powerful part of Citrix Analytics technology hosted in Citrix Cloud. It easily scales the management of multiple Citrix ADCs. It also simplifies the configuration and monitoring of WAF , BOT , and API Protections.

*  CWAAP Portal - is a SaaS portal that provides the abilility to configure CWAAP in the cloud. It gives Citrix admins the ability configure and monitor CWAAP Protections centrally. While with dozens of POPs around the global it gives uses nearby access to have their web requests seamlessly proxied and inpsected.

![CWAAP](/en-us/tech-zone/learn/media/tech-briefs_citrix-waap_portal.png)

## Summary

Enterprise use of cloud services and mobile applications continues to drive the growth of cloud native web and api traffic. At the same time they're increasingly vulnerable to the persistent attacks that come with being hosted on the public internet. Citrix Web App and API Security (CWAAP) includes an advanced suite of technology to protect these critical Enterprise service in the cloud.

## References

Learn more about Citrix Web App and API Protection at [Citrix WAAP](https://www.citrix.com/waap)
