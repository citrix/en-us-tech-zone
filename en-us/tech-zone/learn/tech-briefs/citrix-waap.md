---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
description: With cyberthreats rising and modern application architectures getting more complex, organizations need a simpler way to defend against bots, DDoS, zero-day exploits, and other attacks. Learn how Citrix Web App and API (CWAAP), delivered as-a-service, can provide effective security against these attacks.
---
# Citrix Web App and API Protection (CWAAP)

## Introduction

Enterprises continue to move their apps and data to the cloud to support work from home employees, fueled by the pandemic. At the same time bad actors continue to develop new ways to attack those apps and data to support their illicit activities.

Citrix Web App and Api Protection (CWAAP) guards against their attempts to exfiltrate, manipulate or destroy apps and data. CWAAP technology, based on Machine Learning and Artificial Intelligence, does this by constantly evolving to stay ahead of these increasingly complex attacks.

## Overview

It includes 3 distinct sets of protection technology that work together to provide dynamic protection:

1.  Web Application Firewall (WAF) - short for web application firewall, "WAF" origins are in protecting web sites hosted on premises, but now as sites are expaned or migrated to hybrid cloud or public cloud environment they have a broader attack surface to protect.
2.  API Protection - short for application programming interface, "APIs" have been around since the early days of digital computing, yet now in the Cloud Era have taken on a new signficance. Cloud native systems are designed to distribute functions to operate independently in containers and communicate over APIs. Also know as microservices, this form of development is more efficent in many ways than traditional monolithic architecture, yet by nature is primarily hosted in the cloud and subsequently APIs are vulerable to attack.
3.  BOT Protection - short for robot, a "BOT" bot simulates human activity. There are good ones such as crawler bots run by Google to categorize their search engine content. There are also bad ones such as hacker bots that incessantly probe sites for vulnerabilities, scraper bots that steal and or manipuate content in an attempt to profit illegally, or botnets that attempt to deny service to users to hurt a company financially or otherwise.

## BOT Protection

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

## API Protection

APIs have similarities to Web Apps in that they receive TCP connections on specific open ports, yet have some unique differences.

1.  APIs have unique paths or routes to cloud native microservices. Typically defined according to the Open Api Specification (OAS) in Swagger files API routes

## WAF Protection

WAF works on both a postive and negative attack model. The positive model identifies zero-day threats by looking for abnormal activity patterns. The negative model identifes previously documented attack signatures.

OWASP

Signatures

## Summary

Enterprise use of cloud services and mobile applications continues to drive the growth of cloud native web and api traffic. At the same time they're increasingly vulnerable to the persistent attacks that come with being hosted on the public internet. Citrix Web App and API Security (CWAAP) includes an advanced suite of technology to protect these critical Enterprise service in the cloud.
