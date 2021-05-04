---
layout: doc
h3InToc: true
contributedBy: Gene Whitaker
specialThanksTo: Adrianna Pellitteri
description: One-page summary of high availability and troubleshooting tips.
tz_title: Citrix ADC - Troubleshooting High Availability Cheat Sheet
tz_products: citrix-networking;
---
# Diagrams and Poster: Citrix ADC - Troubleshooting High Availability Cheat Sheet

## Overview

A high availability (HA) deployment of two Citrix ADC appliances can provide uninterrupted operation in any transaction. One appliance configured as the primary node and the other as the secondary node. The primary node accepts connections and manages servers. The secondary node monitors the primary. If for any reason the primary node is unable to accept connections, the secondary node takes over.

By default, the Citrix ADC sends heartbeats every 200 ms. A dead interval is sent every 3 sec. After three seconds, a peer node is marked DOWN if heartbeat messages are not received from the peer node.

The Citrix ADC high availability and sync cheat sheet provides you with the most commonly used resources to troubleshoot Citrix ADC high availability and sync issues.

Use the following link to [download Troubleshooting Citrix ADC High Availability Cheat Sheet](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability.pdf).

[![Cheat Sheet](/en-us/tech-zone/learn/media/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability_1.png)](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability.pdf)
