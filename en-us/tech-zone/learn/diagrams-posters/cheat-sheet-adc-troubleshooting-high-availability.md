---
layout: doc
---
# Troubleshooting Citrix ADC High Availability Cheat Sheet

## Contributors

**Author:** [Gene Whitaker](mailto:gene.whitaker@citrix.com)

A high availability (HA) deployment of two Citrix ADC appliances can provide uninterrupted operation in any transaction. With one appliance configured as the primary node and the other as the secondary node, the primary node accepts connections and manages servers while the secondary node monitors the primary. If, for any reason, the primary node is unable to accept connections, the secondary node takes over.

By default, Citrix ADC sends heartbeats every 200ms and dead interval is 3sec. After three seconds, a peer node is marked DOWN if heartbeat messages are not received from the peer node.

The Citrix ADC high availability and sync cheat sheet provides you with the most commonly used resources to troubleshoot Citrix ADC high availability and sync issues.

Use the following link to [download Troubleshooting Citrix ADC High Availability Cheat Sheet](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability.pdf).

[![Cheat Sheet](/en-us/tech-zone/learn/media/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability_1.png)](/en-us/tech-zone/learn/downloads/diagrams-posters_cheat-sheet-adc-troubleshooting-high-availability.pdf)
