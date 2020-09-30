---
layout: doc
description: Copy & paste description from TOC here
---
# Citrix Gateway and Citrix Virtual Apps and Desktops

## Overview

Citrix Gateway is the best secure remote access solution for Citrix Workspace. It provides a myriad of unique integrations that enhance security and user experience. Moreover, Citrix Gateway consolidates access to any app, from any device, through a single URL.

Citrix Gateway enables encrypted and contextual access (authentication and authorization) to Citrix Workspace. Its Citrix ADC-powered load balancing distributes user traffic across the Citrix Virtual Apps and Desktops servers. Citrix Gateway also accelerates, optimizes, and provides visibility into the traffic flow that is useful to ensuring optimal user performance in a Citrix Virtual Apps and Desktops deployment.

The following integrations add value to a Citrix Workspace deployment:

* Contextual Authentication – Validate the user and device with multifactor (nFactor) authentication
* Contextual Access – Control access to resources by modifying Citrix HDX connection behavior
* Contextual Control – Control access to resources by modifying Citrix HDX connection behavior at the network edge
* End to End Monitoring – Identify the source of delays and triage issues which impact user performance
* Adaptive Network Transport – Deliver a superior user experience by dynamically responding to changing network conditions
* Optimal Routing – Ensure a better user experience by always launching apps and desktops from the local gateway
* Custom Availability Monitors – Provide deep application health monitoring of backend services running on the StoreFront and Delivery Controller servers

## Conceptual architecture

Citrix Gateway is a hardened appliance (physical or virtual) that proxies and secures all Citrix Workspace traffic with standards-based SSL/TLS encryption. The most common deployment configuration is to place the Citrix Gateway appliance in the DMZ that lies between an organization’s secure internal network and the Internet (or any external network).

Many organizations protect their internal network with a single DMZ (Figure 1), however multiple Citrix Gateway appliances can be deployed for more complex deployments requiring a double-hop DMZ (Figure 2).

Figure 1: Virtual App and Desktops with Citrix Gateway deployed in a single DMZ

Some organizations use multiple firewalls or to protect their internal networks. The three firewalls in Figure 2 divide the DMZ into two stages (double-hop) to provide an extra layer of security for the internal network.

Figure 2: Virtual Apps and Desktops with Citrix Gateway deployed in a double-hop DMZ

Citrix administrators can deploy Citrix Gateway appliances in a double-hop DMZ to control access to servers running Citrix Virtual Apps and Desktops. In a double-hop deployment the security functions are split across the two appliances.
The Citrix Gateway in the first DMZ handles user connections and performs the security functions such as encryption, authentication, and access to the servers in the internal network.

The Citrix Gateway in the second DMZ serves as a Citrix Gateway proxy device. This Citrix Gateway enables the HDX traffic to traverse the second DMZ to complete user connections to the server farm. Communications between Citrix Gateway in the first DMZ and the Secure Ticket Authority (STA) in the internal network are also proxied through Citrix Gateway in the second DMZ.

In enterprises where multiple Storefront and Delivery Controllers servers are deployed, Citrix recommends load balancing their services by using the Citrix ADC appliance. Use one virtual server to load balance all of the Storefront servers and another for all of the Delivery Controller servers. Application intelligent health monitoring ensures that only fully functioning servers are marked available to take user requests.

Citrix ADC appliances configured for global server load balancing (GSLB) can balance the Citrix Workspace load across data centers by directing user requests to the closest or best performing data center, or to surviving data centers in case of an outage. This provides for disaster recovery across multiple data centers and ensures continuous availability of applications by protecting against points of failure.

In Figure 3, the ADCs use the Metric Exchange Protocol (MEP) and the DNS infrastructure to connect the users to the data center that best meets the criteria set by administrators. The criteria can designate the least loaded data center, the closest data center, the data center that responds most quickly to requests from the client’s location, a combination of those metrics, and SNMP metrics from server resources.



## Contributors

**Author:** [Name](URL)

