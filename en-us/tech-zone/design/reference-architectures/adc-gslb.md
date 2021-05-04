---
layout: doc
h3InToc: true
contributedBy: Rajendra Soebhag, Albert Lee
specialThanksTo: Brendan Lin, Sarah Steinhoff
description: Learn the architecture and deployment considerations for Global Server Load Balancing configuration with Citrix Application Delivery Controller.
tz_title: Application Delivery Controller - Global Server Load Balancing
tz_products: citrix-networking;
---
# Reference Architecture: Application Delivery Controller - Global Server Load Balancing

## Overview

Citrix Application Delivery Controller (ADC) Global Server Load Balancing (GSLB) is a DNS-based solution which describes a range of technologies to distribute resources around multi-site data center locations. This document describes the deployment topology and configuration architecture needed to set up GSLB between multi-sites where Citrix Virtual Apps and Desktops StoreFront servers are load-balanced by Citrix Gateway and Citrix ADC.

## Fundamental Design Factors

The following includes fundamental design factors during an assessment and design phase that affects the formation of the design to cater for requirements. It highlights those considerations and provides background information and insight to support these.

*  **Multi-site Geo-dispersed Data center deployment with ADC** -  Customer operates Citrix ADC appliances deployed across data center sites (that is, data center 1 and data center 2). A Citrix ADC high availability pair deployment consisting of two appliances commonly shares physical peripheral hardware components placed within the same data center site. It is intended to protect against Citrix ADC services outages caused by Citrix ADC appliance or peripheral hardware component failures (that is, network switches, power distribution units, and so on). As Citrix ADC appliances are deployed to two different sites (that is, data center 1 and data center 2) not physically sharing peripheral hardware components (that is, network switches, power distribution units, and so on), the design caters for a deployment that uses Citrix ADC GSLB to provide for resilience and redundancy.

*  **Business continuity** - For component resilience and redundancy, business requirements exist for the design to cater for single systems failure within and across data center sites without affecting services availability and performance. A disaster can involve a single data center failure or failure of individual services within a single data center site resulting in failing over services and client connections to another data center site. Citrix ADC GSLB is used to cater to network traffic distribution, high availability, and failover services across both data center 1 and data center 2 sites.

*  **Network traffic flow efficiency** - The design incorporates network traffic flows involving multiple serial hops to access individual services within the customer infrastructure. To ensure network traffic flow efficiency and eliminate routing inefficiency, network traffic flows are designed to remain within each local data center site. As such, the design caters to primary traffic flows to use back-end systems within the same data center site, and secondary (backup) traffic flows use back-end systems within the opposite data center site.

## Global Server Load Balancing

### GSLB Feature Overview

With ordinary DNS, when a client sends a domain name system (DNS) request, it receives a list of IP addresses of the domain or service. Generally, the client chooses the first IP address in the list and initiates a connection with that server. The DNS server uses a technique called DNS round robin to rotate through the IPs on the list. It sends the first IP address to the end of the list and promotes the others after it responds to each DNS request. This technique ensures equal distribution of the load, but it does not support disaster recovery, load balancing based on load or proximity of servers, or persistency.

Fundamentally, GSLB based on DNS works the same way as standard DNS, with the exception that more logic is in place to determine what addresses to return. The logic in most situations is based on:

*  The load and capacity of resources on the network
*  The IP address or interface the query came from
*  Previous requests made from the same IP or network
*  The health state of resources

To ensure the various pieces of information are in place, the ADC system makes use of several ways to determine state so that proper decision making can occur:

*  Via explicit monitors that check for availability of remote resources by accessing the resource itself
*  Via Metric Exchange Protocol (MEP), which is a channel of communication between distinct NetScaler devices, and provides a mechanism for one ADC to provide state information about resources to another ADC
*  Through SNMP based load monitors, which poll a remote resource for statistics such as CPU load, network load

[![Citrix-ADC-GSLB-Image-1](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_001.png)](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_001.png)

Figure-1 A Typical DNS Flow to Application Access

When you configure GSLB on ADC appliances and enable MEP, the DNS infrastructure is used to connect the client to the data center that best meets the set criteria. The criteria can designate the least loaded data center, the closest data center, the data center that responds most quickly to requests from the client’s location, a combination of those metrics, and SNMP metrics. An appliance tracks the location, performance, load, and availability of each data center. It uses these factors to select the data center to send the client request.

### GSLB Deployment

#### Deployment Types

Citrix ADC appliances configured for GSLB provide for disaster recovery and ensure the continuous availability of applications by protecting against points of failure in a WAN. GSLB can balance the load across data centers by directing client requests to the closest or best-performing data center, or to surviving data centers in the event of an outage.

The following are some of the typical GSLB deployment types:

Active-Active site deployment - An active-active site consists of multiple active data centers. Client requests are load balanced across active data centers. This deployment type can be used when you require global distribution of traffic in a distributed environment.

All the sites in an active-active deployment are active, and all the services for a particular application/domain are bound to the same GSLB virtual server. Sites exchange metrics through the Metrics Exchange Protocol (MEP). Site metrics exchanged between the sites include the status of each load balancing and content switching virtual server, the current number of connections, current packet rate, and current bandwidth usage. The Citrix ADC appliance needs this information to perform load balancing across the sites.

An active-active deployment can include a maximum of 32 GSLB sites, because MEP cannot synchronize more than 32 sites. No backup sites are configured in this deployment type.

The Citrix ADC appliance sends client requests to the appropriate GSLB site as determined by the GSLB method specified in the GSLB configuration.

[![Citrix-ADC-GSLB-Image-2](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_002.png)](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_002.png)

Figure-2 Active-Active Site Deployment

Active-Passive site deployment - An active-passive site consists of an active and a passive data center. This deployment type is ideal for disaster recovery.

In this type of deployment, some of the sites (remote sites) are reserved only for disaster recovery. These sites do not participate in any decision making until all the active sites are DOWN. A passive site does not become operational unless a disaster event triggers a failover.

Once you have configured the primary data center, replicate the configuration for the backup data center and designate it as the passive GSLB site by designating a GSLB virtual server at that site as the backup virtual server.

An active-passive deployment can include a maximum of 32 GSLB sites, because MEP cannot synchronize more than 32 sites.

[![Citrix-ADC-GSLB-Image-3](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_003.png)](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_003.png)

Figure-3 Active-Passive Site Deployment

Parent-child topology deployment - Citrix ADC GSLB provides global server load balancing and disaster recovery by creating mesh connections between all the involved sites and making intelligent load balancing decisions. Each site communicates with the others to exchange server and network metrics through Metric Exchange Protocol (MEP), at regular intervals. However, with the increase in number of peer sites, the volume of MEP traffic increases exponentially because of the mesh topology. To overcome this, you can use a parent-child topology. The parent-child topology also supports larger deployments. In addition to the 32 parent sites, you can configure 1024 child sites.

#### Entities

A GSLB configuration consists of a group of GSLB entities on each appliance in the configuration. These entities include GSLB sites, GSLB services, GSLB service groups, GSLB virtual servers, and ADNS services.

##### GSLB Sites

A typical GSLB setup consists of data centers, each of which has various network appliances that may or may not be Citrix ADC appliances. The data centers are called GSLB sites. Each GSLB site is managed by a Citrix ADC appliance that is local to that site. Each of these appliances treats its own site as the local site and all other sites, managed by other appliances, as remote sites.

If the appliance that manages a site is the only Citrix ADC appliance in that data center, the GSLB site hosted on that appliance acts as a bookkeeping placeholder for auditing purposes, because no metrics can be collected. Typically, this happens when the appliance is used only for GSLB, and other products in the data center are used for load balancing or content switching.

##### Relationships among GSLB Sites

The concept of sites is central to Citrix ADC GSLB implementations. Unless otherwise specified, sites form a peer relationship among themselves. This relationship is used first to exchange health information and then to distribute load as determined by the selected algorithm. In many situations, however, a peer relationship among all GSLB sites is not desirable. Reasons for not having an all-peer implementation can be:

*  To clearly separate GSLB sites. For example, to separate sites that participate in resolving DNS queries from the traffic management sites.
*  To reduce the volume of MEP traffic, which increases exponentially with an increasing number of peer sites.

These goals can be achieved by using parent and child GSLB sites.

##### GSLB Services

A GSLB service is usually a representation of a load balancing or content switching virtual server, although it can represent any type of virtual server. The GSLB service identifies the virtual server’s IP address, port number, and service type. GSLB services are bound to GSLB virtual servers on the Citrix ADC appliances managing the GSLB sites. A GSLB service bound to a GSLB virtual server in the same data center is local to the GSLB virtual server. A GSLB service bound to a GSLB virtual server in a different data center is remote from that GSLB virtual server.

##### GSLB Virtual Servers

A GSLB virtual server has one or more GSLB services bound to it, and load balances traffic among those services. It evaluates the configured GSLB methods (algorithms) to select the appropriate service to which to send a client request. Because the GSLB services can represent either local or remote servers, selecting the optimal GSLB service for a request has the effect of selecting the data center that should serve the client request.

The domain for which global server load balancing is configured must be bound to the GSLB virtual server, because one or more services bound to the virtual server serve requests made for that domain.

Unlike other virtual servers configured on a Citrix ADC appliance, a GSLB virtual server does not have its own virtual IP address (VIP).

##### ADNS Services

An ADNS service is a special kind of service that responds only to DNS requests for domains for which the Citrix ADC appliance is authoritative. When an ADNS service is configured, the appliance owns that IP address and advertises it. Upon reception of a DNS request by an ADNS service, the appliance checks for a GSLB virtual server bound to that domain. If a GSLB virtual server is bound to the domain, it is queried for the best IP address to which to send the DNS response.

##### DNS VIPs

A DNS virtual IP is a virtual IP (VIP) address that represents a load balancing DNS virtual server on the Citrix ADC appliance. DNS requests for domains for which the Citrix ADC appliance is authoritative can be sent to a DNS VIP.

##### Metrics Exchange Protocol (MEP)

The data centers in a GSLB setup exchange metric with each other through the MEP, which is a proprietary protocol for the Citrix ADC appliance. The exchange of the metric information begins when you create a GSLB site. These metrics comprise load, network, and persistence information.

MEP is required for health checking of data centers to ensure their availability. A connection for exchanging network metric (round-trip time) can be initiated by either of the data centers involved in the exchange, but a connection for exchanging site metrics is always initiated by the data center with the lower IP address. By default, the data center uses a subnet IP address (SNIP) to establish a connection to the IP address of a different data center. However, you can configure a specific SNIP, virtual IP (VIP) address, or the NSIP address, as the source IP address for metrics exchange. The communication process between GSLB sites uses TCP port 3011 or 3009, so this port must be open on firewalls that are between the Citrix ADC appliances.

#### Load Balancing Methods

Unlike traditional DNS servers that simply respond with the IP addresses of the configured servers, a Citrix ADC appliance configured for GSLB responds with the IP addresses of the services, as determined by the configured GSLB method. By default, the GSLB virtual server is set to the least connection method. If all GSLB services are down, the appliance responds with the IP addresses of all the configured GSLB services.

GSLB methods are algorithms that the GSLB virtual server uses to select the best-performing GSLB service. After the host name in the Web address is resolved, the client sends traffic directly to the resolved service IP address.

The Citrix ADC appliance provides the following GSLB methods:

| Method | Description |
| ------------------------ | :----------------------------------------------------------- |
| **Round Robin**           | When a GSLB virtual server is configured to use the round robin method, it continuously rotates a list of the services that are bound to it. When the virtual server receives a request, it assigns the connection to the first service in the list and then moves that service to the bottom of the list. |
| **Least Response Time** | When the GSLB virtual server is configured to use the least response time method, it selects the service with the lowest value. Where, lowest value = current active connections X average response time. |
| **Least Connections**   | When a GSLB virtual server is configured to use the least connection GSLB algorithm (or method), it selects the service with the fewest active connections. This is the default method, because, in most circumstances, it provides the best performance. |
| **Least Bandwidth**     | A GSLB virtual server configured to use the least bandwidth method selects the service that is currently serving the least amount of traffic, measured in megabits per second (Mbps). |
| **Least Packets**       | A GSLB virtual server configured to use the least packets method selects the service that has received the fewest packets in the last 14 seconds. |
| **Source IP Hash**      | A GSLB virtual server configured to use the source IP hash method uses the hashed value of the client IPv4 or IPv6 address to select a service. To direct all requests from source IP addresses that belong to a particular network to a specific destination server, you must mask the source IP address. For IPv4 addresses, use the netmask parameter. For IPv6 addresses, use the v6NetMaskLength parameter. |
| **Custom Load**         | Custom load balancing is performed on server parameters such as CPU usage, memory, and response time. When using the custom load method, the Citrix ADC appliance usually selects a service that is not handling any active transactions. If all of the services in the GSLB setup are handling active transactions, the appliance selects the service with the smallest load. A special type of monitor, known as a load monitor, calculates the load on each service in the network. The load monitors do not mark the state of a service, but they do take services out of the GSLB decision when those services are not UP. |

For GSLB methods to work with a remote site, either MEP must be enabled, or explicit monitors must be bound to the remote services. If MEP is disabled, RTT, Least Connections, Least Bandwidth, Least Packets, and Least Response Time methods default to Round Robin.

#### Monitor GSLB services

When you bind a remote service to a GSLB virtual server, the GSLB sites exchange metric information, including network metric Information, which is the round-trip-time and persistence Information.

If a metric exchange connection is momentarily lost between any of the participating sites, the remote site is marked as DOWN, and load balancing is performed on the remaining sites that are UP. When the metric exchange for a site is DOWN, the remote services belonging to the site are marked DOWN as well.

The Citrix ADC appliance periodically evaluates the state of the remote GSLB services by using either MEP or monitors that are explicitly bound to the remote services. Binding explicit monitors to local services is not required, because the state of the local GSLB service is updated by default using the MEP. However, you can bind explicit monitors to a remote service. When monitors are explicitly bound, the state of the remote service is not controlled by the metric exchange.

## Reference Architecture

### Design GSLB

The following details the Citrix ADC instances network address configurations in terms of IP addressing and routing in data center site data center 1 and 2:

*  NSIP – Citrix ADC Management IP address
*  SNIP – Citrix ADC Subnet IP and ADNS Listener IP
*  GSLB – GSLB Site IP
*  VIP – Citrix ADC VIP for Citrix Gateway
*  VIP Citrix ADC Load Balancing (LB) VIP for StoreFront

[![Citrix-ADC-GSLB-Image-4](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_004.png)](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_004.png)

Figure-4 DNS and GSLB workflow

Figure 4 describes a DNS workflow from the client's application access request via DNS, which will be handled by GSLB entities. As a DNS request comes into the global DNS server, which delegates the request subzone to each ADNS IP as subzone name servers. Upon reception of a DNS request by an ADNS service, the appliance checks for a GSLB virtual server bound to that domain. If a GSLB virtual server is bound to the domain, it is queried for the best IP address to which to send the DNS response.

Figure 5 diagram illustrates its actual deployment architecture topology. It lists all necessary interfaces associated with designated ADC IP addresses accordingly (that is, NSIP, SNIP/ADNS IP, Gateway IP, Load Balance IP) overlays with GSLB topology and services.

[![Citrix-ADC-GSLB-Image-5](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_005.png)](/en-us/tech-zone/design/media/reference-architectures_adc-gslb_005.png)

Figure-5 GSLB Deployment Architecture

Those specific GSLB entities, as described in the earlier chapter, are:

**ADNS Listener IP:** An ADC IP that listens for DNS queries.

*  The ADNS listener IP is typically an existing SNIP on the ADC appliance.
*  For external DNS, create a public IP for the ADNS Listener IP, and open UDP 53 and TCP 53, so Internet-based DNS servers can access it.

**GSLB Site IP / MEP listener IP:** An ADC IP that is used for ADC-to-ADC GSLB communication. The communication, MEP transmits the following between GSLB-enabled ADC pairs: load balancing metrics, proximity, persistence, and monitoring.

*  GSLB Sites – On ADC, you create GSLB Sites. GSLB Sites are the endpoints for the MEP communication. Each ADC pair is configured with the MEP endpoints for the local appliance pair, and all remote appliance pairs.
*  TCP Ports – MEP uses port TCP 3009 or TCP 3011 between the ADC pairs. TCP 3009 is encrypted.
*  The ADNS IP address can be used as the MEP endpoint IP.
*  GSLB Sync Ports: To use GSLB Configuration Sync, open ports TCP 22 and TCP 3008 (secure) from the NSIP (management IP) to the remote public MEP IP. The GSLB Sync command runs a script in BSD shell and thus NSIP is always the Source IP.

**Public IP Addresses:** In summary, for public GSLB, if MEP and ADNS are listening on the same IP, then you need one new public IP that is NAT’d to the DMZ IP that is used for ADNS and MEP (GSLB Site IP).

*  Each data center has a separate public IP.
*  DNS is delegated to all public ADNS IP listeners.

### Other Dependencies

The infrastructure for the solution provides a set of common components used by the entire solution.

*  Network Time Services - Most components in the overall solution require integration with Network Time Services (NTP). The following table details the key NTP settings within Client infrastructure to be used with the Citrix Delivery Network deployment.
*  Domain Name Services - Most components in the overall solution require integration with Domain Name Services (DNS). The following table details key DNS infrastructure within the Customer network to be used by the Citrix Delivery Network deployment
*  Security and Authentication - Secure sessions are handled by Citrix Gateway. The following table details key decisions pertaining to SAN certificates for use in each production, acceptance, and test infrastructure.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sa75c12c15894e2b8).

## Citrix Product Documentation References

The deliverable provides guidelines for the implementation and configuration references. However, it does not provide step-by-step instructions on how to install or maintain the components discussed. Therefore, Citrix Consulting recommends Client design and operations teams involved in the design and deployment to review the following documents, articles, and guides prior to implementing the environment provided for production. These documents, articles, guides, and more are available from the online Citrix Knowledge Center, online Citrix Product Documentation, or online Citrix Community.

Citrix Online Product Documentation [Citrix ADC](/en-us/citrix-adc.html)

Citrix Online Product Documentation [Citrix ADC VPX Virtual Machines](/en-us/citrix-adc/current-release/deploying-vpx.html)

Citrix Online Product Documentation [Global Server Load Balancing](/en-us/citrix-adc/current-release/global-server-load-balancing.html)

Citrix Whitepaper [CTX129514](https://support.citrix.com/article/CTX129514) – Secure Deployment Guide for Citrix ADC MPX, VPX, and SDX Appliances

Citrix Whitepaper [CTX123976](https://support.citrix.com/article/CTX123976) – Citrix ADC Global Server Load Balancing Primer: Theory and Implementation

Citrix Knowledgebase Article [CTX122619](https://support.citrix.com/article/CTX122619) – DNS and GSLB Primer

Citrix Knowledgebase Article [CTX121713](https://support.citrix.com/article/CTX121713) – How to Delegate Subdomains in a Microsoft DNS or a BIND for Global Server Load Balancing on a Citrix ADC Appliance

Citrix Knowledgebase Article [CTX110488](https://support.citrix.com/article/CTX110488) – Delegating DNS Subdomains to the GSLB Setup of the Citrix ADC Appliances

Citrix Online Product Documentation [Load Balancing](/en-us/citrix-adc/current-release/load-balancing.html)

Citrix Online Product Documentation [SSL Offload and Acceleration](/en-us/citrix-adc/current-release/ssl.html)

Citrix Community Blog [Gateway Integration with StoreFront Lessons Learned](https://www.citrix.com/blogs/2014/10/15/gateway-integration-with-storefront-lessons-learned/)

Citrix Community Blog [StoreFront and Citrix Gateway GSLB considerations](https://www.citrix.com/blogs/2018/05/25/storefront-and-citrix-gateway-gslb-considerations/)
