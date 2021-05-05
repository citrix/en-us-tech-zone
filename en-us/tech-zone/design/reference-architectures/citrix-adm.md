---
layout: doc
h3InToc: true
contributedBy: Albert Lee
specialThanksTo: Andrew Gravett
description: See how the Citrix Application Delivery Management software is deployed to simplify management and monitoring of your application delivery infrastructure.
tz_title: Application Delivery Management
tz_products: citrix-networking;
---
# Reference Architecture: Application Delivery Management

## Overview

Citrix Application Delivery Management (ADM) is a centralized management solution. It simplifies operations by providing administrators with enterprise-wide visibility and automating management jobs that are getting ran across multiple instances. You can manage and monitor Citrix application networking products including Citrix Application Delivery Controllers (ADC) MPX, VPX, SDX, CPX, BLX, Citrix Gateway, Citrix Web Application Firewall (WAF), and Citrix SD-WAN. You can use ADM to manage, monitor, and troubleshoot the entire global application delivery infrastructure from a single, unified console.

ADM also addresses the application visibility challenge by collecting detailed information about web-application and virtual-desktop traffic including application flow, security events, user-session-level information, webpage performance data, and database information flowing through the managed Citrix Appliances, and providing actionable reports. This approach enables administrators to troubleshoot and proactively monitor customer issues in a matter of minutes.

Citrix ADM Software virtual appliances can be deployed in several deployment modes and provide the flexibility to integrate within your existing Citrix networking design. The following are some of the deployment scenarios implemented by using ADM Software appliances.

*  Single Server
*  High Availability (Recommended)
*  Disaster Recovery Mode
*  ADM Agent Deployment (for adding remote Sites)

This ADM Reference document defines a set of architectural building blocks for delivering Citrix Application Delivery Management (Citrix ADM). The target audiences are technical professionals and architects seeking knowledge on how to key components to support the following objectives.

## ADM Appliance Software Architecture

The Citrix Application Delivery Management (ADM) software uses a built-in data store to provide integration with the server, and the server manages all the key processes, such as data collection, NITRO API calls. In its data store, the server stores an inventory of instance details, such as host name, software version, running, and saved the configuration, certificate details, entities configured on the instance. Single server deployment is suitable if you want to process small amounts of traffic or store data for a limited time.

The following image shows the different internal and external subsystem components of a Citrix ADM appliance and the communication flow between the internal ADM server components and externally managed networking appliances and instances.

[![Citrix-ADM-Image-1](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_001.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_001.PNG)

The Citrix ADM NITRO Service acts as a web server handling HTTP requests and responses sent to other subsystems within the appliance from the management GUI or APIs/SDKs, using ports 80 and 443. These requests travel via the Message Bus (message processing system) by using the inter-process communication (IPC) mechanism. Initially, the HTTP requests sent to the Control subsystem, which either processes the information or sends it to another, more appropriate subsystem. Each of the other subsystems including Inventory, StyleBooks, Data Collector, Configuration, AppFlow Decoder, AppFlow Analytics, Performance, Events, Entities, SLA Manager, Provisioner, Journal, and daemons (aaad/snmpd/ntpd/syslogd/monit/sshd/pitboss), have specific roles.

## ADM Systems Design

Citrix ADM is a centralized management solution that simplifies operations by providing administrators with enterprise-wide visibility and automating management jobs that are getting ran across multiple instances.

To manage and monitor applications and the network infrastructure, you must first install Citrix ADM on one of the hypervisors. You can deploy Citrix ADM either as a single server or in a high availability mode. When using Citrix ADC Insight Center, you can migrate to Citrix ADM and avail of the management, monitoring, orchestration, and application management features in addition to the analytics features.

*  Single-server deployment. In a Citrix ADM single server deployment, the database is integrated with the server, and a single server processes all the traffic. You can deploy Citrix ADM with Citrix Hypervisor, VMware ESXi, Microsoft Hyper-V, and Linux KVM.
*  High availability deployment. A high availability deployment (HA) of two Citrix ADM servers provides uninterrupted operations. In a high availability setup, both Citrix ADM nodes must be deployed in active-passive mode, on the same subnet using the same software version and build, and same configurations. With HA deployment the ability to configure the floating IP address on the Citrix ADM primary node eliminates the need for a separate Citrix ADC load balancer.

The following diagram depicts the high-level ADM HA appliance deployment.

[![Citrix-ADM-Image-2](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_002.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_002.PNG)

## ADM Key System Requirements

Before importing a Citrix ADM appliance to your current platform (that is, Hypervisors), understanding the critical system licensing, hypervisor requirements, appliance image requirements, and ADC Build Integration limitations is a must.

### Licensing Overview

Citrix ADM requires a verified Citrix ADC license to manage and monitor the Citrix ADC instances.

You can manage and monitor any number of supported instances and entities without a license. However, you can select and configure Analytics for an initial 30 discovered applications on the App Dashboard and view analytics data for 30 virtual servers without applying for extra licenses. To collect Analytics for more than 30 discovered applications (30 virtual servers), you must purchase and apply the desired licenses.

Full information on licensing is available in the Citrix ADM product documentation about [licensing](/en-us/citrix-application-delivery-management-software/current-release/licensing.html).

### Supported Hypervisors

An ADM appliance deployed on-premises as virtual appliances can run on Citrix Hypervisor, VMware ESXi, Microsoft Hyper-V, and Linux KVM.

The following table lists the hypervisors supported by Citrix ADM.

| **Hypervisor**    | **Versions**              | **Product Documentation**                                  |
| ----------------- | ------------------------- | ------------------------------------------------------------ |
| Citrix Hypervisor | 7.1 and 7.4               | [Citrix ADM with Citrix Hypervisor](/en-us/citrix-application-delivery-management-software/current-release/deploy/install-mas-on-xenserver.html) |
| VMware ESXi       | 6.0 and 6.5               | [Citrix ADM with VMware ESXi](/en-us/citrix-application-delivery-management-software/current-release/deploy/install-mas-on-esxi.html) |
| Microsoft Hyper-V | 2012 R2 and 2016          | [Citrix ADM with Microsoft Hyper-V](/en-us/citrix-application-delivery-management-software/current-release/deploy/install-mas-on-hyper-v.html) |
| Generic KVM       | RHEL 7.4 and Ubuntu 16.04 | [Citrix ADM with Linux KVM server](/en-us/citrix-application-delivery-management-software/current-release/deploy/install-mas-on-kvm.html) |

### Requirements for ADM appliance and agent Images

Citrix ADC instances deployed in remote data centers can be managed and monitored from Citrix ADM running in a primary data center. Citrix ADC instances sent data directly to the primary Citrix ADM that resulted in the consumption of WAN bandwidth. Also, the processing of analytics data utilizes CPU and memory resources of primary Citrix ADM.

Customers have their data centers located across the globe. Agents play a vital role in following scenarios where the customers can choose:

*  To install agents in remote data centers so that there is a reduction in WAN bandwidth consumption.
*  To limit the amount of instances directly sending traffic to primary Citrix ADM for data processing.

#### Requirements for Citrix ADM appliance

| **Component**              | **Requirement**                                              |
| -------------------------- | ------------------------------------------------------------ |
| RAM                        | 32 GB required                                               |
| Virtual CPU                | Eight vCPUs required                                         |
| Storage space              | Citrix recommends using solid-state drive (SSD) technology for Citrix ADM deployments. The default value is 120 GB. Actual storage requirement depends on Citrix ADM sizing estimation. If your Citrix ADM storage requirement exceeds 120 GB, you to have to attach an extra disk. You can add only one extra disk.   Citrix recommends you estimate storage and attach the extra disk at the time of initial deployment. Use the [sizing calculator](https://citrix.sharefile.com/share/getinfo/se7df156b4de42888) to do the exact sizing estimation for your Citrix ADM deployment, and for more information, see How to Attach an Additional Disk to Citrix ADM. |
| Virtual network interfaces | 1                                                            |
| Throughput                 | 1 Gbps or 100 Mbps                                           |

#### Requirements for Citrix ADM on-prem agent

Agents work as an intermediary between the primary Citrix ADM and the discovered instances across different data centers. Following are the benefits of installing agents:

*  The instances are configured to agents so that the unprocessed data is sent directly to agents instead of primary Citrix ADM. Agents do the first level of data processing and send the processed data in a compressed format to the primary Citrix ADM for storage.
*  Agents and instances are co-located in the same data center so that the data processing is faster.
*  Clustering the agents provides redistribution of Citrix ADC instances on agent failover. When one agent in a site fails, traffic from Citrix ADC instances switched to another available agent on the same site.

The following is the minimum requirements for Citrix ADM on-prem agent:

| **Component**              | **Requirement**                                              |
| -------------------------- | ------------------------------------------------------------ |
| RAM                        | 8 GB required  **Note:** The default value is 32 GB.  Citrix recommends that you increase the default value to 32 GB for better performance. |
| Virtual CPU                | Two vCPUs required                                           |
| Storage space              | 30 GB                                                        |
| Virtual network interfaces | 1                                                            |
| Throughput                 | 1 Gbps                                                       |

The following figure shows Citrix ADC instances in two data centers and Citrix ADM high availability deployment using multisite agent-based architecture.

[![Citrix-ADM-Image-3](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_003.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_003.PNG)

The primary site has the Citrix ADM nodes deployed in a high availability configuration. The Citrix ADC instances in the primary site directly registered with the Citrix ADM.

In the secondary site, agents are deployed and registered with the Citrix ADM server in the primary site. These agents work in a cluster handling a continuous flow of traffic in case an agent failover — the Citrix ADC instances at the secondary site registered with the primary Citrix ADM server through agents. The instances send data directly to agents instead of primary Citrix ADM. The agents process the data received from the instances and send it to the primary Citrix ADM in a compressed format. Agents communicate with the Citrix ADM server over a secure channel, and the data sent over the channel compressed for bandwidth efficiency.

### Minimum Citrix ADC versions required for Citrix ADM feature

Diverse Citrix ADM features supported on different Citrix ADC software versions. Review the following table to make sure you have upgraded your Citrix ADC instances to the correct version.

| **Citrix ADM Features**                        | **Citrix ADC Software Version**                            |
| ----------------------------------------------- | ------------------------------------------------------------ |
| StyleBooks                                      | 10.5 and later                                               |
| OpenStack/CloudStack Support                    | 11.0 and later: If a partition is required 11.1 and later: If partition on shared virtual LAN is required. |
| NSX Support                                     | 11.1 Build 47.14 and later (VPX)                             |
| Mesos/Marathon Support                          | 10.5 and later                                               |
| Backup/Restore                                  | 10.1 and later OR for SDX 11.0 and later                     |
| Monitoring/Reporting & Configuration using Jobs | 10.1 and later                                               |
| **Citrix Analytics Features**                          | **Citrix ADC Software Version**                             |
| Web Insight                                     | 10.5 and later                                               |
| HDX Insight                                     | 10.1 and later                                               |
| Security Insight                                | 11.0.65.31 and later                                         |
| Gateway Insight                                 | 11.0.65.31 and later                                         |
| Cache Insight                                   | 10.5 and later*                                              |
| SSL Insight                                     | 12.0 and later                                               |

**Important Note:** Integrated Cache Metrics are not supported in Citrix ADM with Citrix instances running version 11.0 build 66.x.

## Environment Customizations and Sizing Recommendations

### Sizing Settings

Citrix Application Delivery Management (ADM) storage requirement is determined based on your Citrix ADM sizing estimation. By default, Citrix ADM provides you a storage capacity of 120 GB. If you need more than 120 GB for storing your data, you can attach an extra disk (Max extra disk per ADM is 3 TB).

Notes:

*  Estimate storage requirements and attach an extra disk to the server at the time of the initial deployment of Citrix ADM.
*  For a Citrix ADM single-server deployment, you can attach only one disk to the server in addition to the default disk.
*  For a Citrix ADM high availability deployment, you must attach an extra disk to each node. The size of both disks should be identical.
*  If you had earlier attached an external disk of lower capacity, you must remove the disk before attaching a new disk.
*  You can attach an extra disk of capacity higher than 2 terabytes. If necessary, the size of the disk can be smaller than 2 terabytes also.
*  Citrix recommends using solid-state drive (SSD) technology for Citrix ADM deployments.

### Prune Settings

To limit the amount of reporting data stored in your Citrix ADM server’s database, you can prune it. You can specify the interval for which you want Citrix ADM to retain network reporting data, events, audit logs, and task logs. By default, this data is pruned every 24 hours (at 00.00 hours). More details can found [here](/en-us/citrix-application-delivery-management-software/current-release/manage-system-settings/configure-system-prune-settings-mas.html).

### Backup Settings

Citrix devices can be backed up automatically to the Citrix ADM server. Also, those backed-up data forwarded to an external server for historical trending and auditing. Refer to the [link](/en-us/citrix-application-delivery-management-software/current-release/manage-system-settings/configure-system-backup-settings.html) for more details.

## ADM Deployment Scenarios

### Single-Server Deployment

In a Citrix ADM single server deployment, the database is deployed and integrated with the server, and a single server processes all the traffic. You can use Citrix ADM with Citrix Hypervisor, VMware ESXi, Microsoft Hyper-V, and Linux KVM.

### High Availability (HA) Deployment

An HA deployment of two Citrix ADM servers provides uninterrupted operations. In a high availability setup, both the Citrix ADM nodes must be deployed in active-passive mode, on the same subnet using the same software version and build and must have identical configurations. With HA deployment, the ability to configure the floating IP address on the Citrix ADM primary node eliminates the need for a separate Citrix ADC load balancer.

The following are the benefits of a high availability deployment with Citrix ADM:

*  An improved mechanism to monitor heartbeats between the primary and secondary nodes.
*  It provides physical streaming replication of the database instead of logical bi-directional replication.
*  High availability configuration provides the ability to configure the floating IP address on the primary node to eliminate the need for a separate Citrix load balancer.
*  It provides easy access to the Citrix ADM user interface using the floating IP address.
*  Citrix ADM user interface is provided only on the primary node. By using the primary node, you can eliminate the risk of accessing and making changes to the secondary node.
*  Configuring the floating IP address handles the failover situation, and reconfiguring the instances is not required.
*  Provides built-in ability to detect and handle the split-brain situation

The following diagram depicts the ADM HA deployment.

[![Citrix-ADM-Image-4](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_004.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_004.PNG)

#### Components of high availability architecture

In high availability deployment, one of the Citrix ADM nodes configured as the primary node (ADM HA Node 1) and the other as the secondary node (ADM HA Node 2). If the primary node goes down due to any reason, the secondary node takes over as the new primary node.

[![Citrix-ADM-Image-5](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_005.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_005.PNG)

## Disaster Recovery (DR) Mode - Reference Architecture

Disaster is a sudden disruption of business functions caused by natural calamities or human-caused events. Disasters affect data center operations, after which resources and the data loss at the disaster site must be fully rebuilt and restored. The loss of data or downtime in the data center is critical and collapses the business continuity.

The Citrix ADM disaster recovery (DR) feature provides full system backup and recovery capabilities for Citrix ADM deployed in high availability mode. At the time of recovery, certificates, configuration files, and a complete backup of the database are available in the recovery site.

The following table describes the terms used while configuring disaster recovery in Citric ADM

| **Terms**                     | **Description**                                              |
| ----------------------------- | ------------------------------------------------------------ |
| Primary site (Data Center A)  | The primary site has Citrix ADM nodes deployed in high availability mode. |
| Recovery site (Data Center B) | The recovery site has a disaster recovery node deployed in standalone mode. This node is in read-only mode and is not operational until the primary site is down. |
| Disaster recovery node        | The recovery node is a standalone node deployed in the recovery site. This node is made operational (to the new primary) in case a disaster select the primary site, and it is non-functional. |

**Note:** The primary site and DR site communicate with each other through ports 5454 and 22, which are enabled by default.

The following image shows the disaster recovery workflow, the initial setup before the disaster, and the workflow after the disaster.

[![Citrix-ADM-Image-6](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_006.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_006.PNG)

The image shows the disaster recovery setup before the disaster.

The primary site has Citrix ADM nodes deployed in the high availability mode, as shown in the previous section.

The recovery site has a standalone Citrix ADM disaster recovery node deployed remotely. The disaster recovery node is in read-only mode and receives data from the primary node to create data backup. Citrix ADC instances in the recovery site are also discovered, but they do not have any traffic flowing through them. During the backup process, all data, files, and configurations are sent and replicated on the disaster recovery node from the primary node.

[![Citrix-ADM-Image-7](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_007.PNG)](/en-us/tech-zone/design/media/reference-architectures_citrix-adm_007.PNG)

After the initiation of the script at the DR site, the DR site now becomes the new primary site. You can also access the DR user interface.

Full information about the disaster recovery (DR) feature is available in the Citrix ADM product documentation article [Configure disaster recovery for high availability](/en-us/citrix-application-delivery-management-software/current-release/deploy/disaster-recovery.html).

## ADM Agent Deployment

You can install and configure the agent, to enable communication between the primary Citrix ADM and the managed Citrix ADC instances in another data center.

You can install an agent on the following hypervisors in your enterprise data center:

*  Citrix Hypervisor
*  VMware ESXi
*  Microsoft Hyper-V
*  Linux KVM Server

The number of agents installed per site depends on the traffic being processed. Currently, Citrix has validated two agents per site for an agent failover scenario. Citrix recommends that you install at least two agents per site so that the traffic flows to another agent in case of an agent failover.

For communication purposes, the following ports must be open between the agent and Citrix ADM on-prem server.

| **Type** | **Port**      | **Details**                                                  |
| -------- | ------------- | ------------------------------------------------------------ |
| TCP      | 8443,7443,443 | For outbound and inbound communication between the agent and the Citrix ADM on-prem server |

The following ports must be open between the agent and Citrix ADC Instances.

| **Type** | **Port**          | **Details**                                                  |
| -------- | ----------------- | ------------------------------------------------------------ |
| TCP      | 80                | For NITRO communication between the agent and Citrix ADC or Citrix SD-WAN instance. |
| TCP      | 22                | For SSH communication between the agent and Citrix ADC or Citrix SD-WAN instance. For synchronization between Citrix ADM servers deployed in high availability mode. |
| UDP      | 4739              | For AppFlow communication between the agent and Citrix ADC or Citrix SD-WAN instance. |
| ICMP     | No reserved port | To detect network reachability between Citrix ADM and Citrix ADC instances, SD-WAN instances, or the secondary Citrix ADM server deployed in high availability mode. |
| SNMP     | 161,162           | To receive SNMP events from Citrix ADC instance to agent.    |
| Syslog   | 514               | To receive Syslog messages from Citrix ADC or Citrix SD-WAN instance to the agent. |
| TCP      | 5557              | For log stream communication between the agent and Citrix ADC instances. |

## References

[Citrix Application Delivery Management Product Documentation](/en-us/citrix-application-delivery-management-software/current-release/get-started.html)

 [Citrix ADC Product Documentation](/en-us/citrix-adc/current-release.html)

[Citrix Analytics Platform](/en-us/citrix-analytics.html)

[ADM Service Graph for cloud-native applications](/en-us/citrix-application-delivery-management-service/application-analytics-and-management/service-graph.html)

[Citrix ADM and Hybrid Multi-Cloud](https://www.youtube.com/watch?v=jq8PrwWZWtc)

[Citrix ADM Service in Citrix Cloud](/en-us/citrix-application-delivery-management-service)

## Appendix

### Citrix Networking Appliance & Functionality Overview

#### VPX Overview

Citrix ADC is an application delivery controller that performs application-specific traffic analysis to distribute intelligently, optimize, and secure Layer 4-Layer 7 (L4–L7) network traffic for web applications. For example, a Citrix ADC bases load balancing decisions on individual HTTP requests instead of on long-lived TCP connections, so that the failure or slowdown of a server is managed much more quickly and with less disruption to clients. Its feature set can be broadly consisting of switching features, security and protection features, and server-farm optimization features.

The Citrix ADC VPX is a software-based platform that provides industry-leading delivery of applications over the internet and private networks.  This virtual appliance can be deployed on hypervisors on-premises or cloud platforms. The VPX appliance is supported by Citrix Hypervisor, VMware ESXi, Microsoft Hyper-V, Linux KVM, Microsoft Azure, and Amazon Web Services (AWS).

The VPX provides the full functionality of the Citrix Networking product line, with throughput capabilities ranging from 10 Mbps to 100 Gbps. The performance is controlled by the platform license and can be increased on-demand by upgrading the platform license.

#### Citrix ADC VPX in Azure

Citrix ADC VPX in Azure is deployed in a Virtual Network (VNET) and is available from the Azure Marketplace in subscription-based, check-in/check-out, or Bring Your Own License (BYOL) editions. The recommended configuration includes three NICs: management, client-side, and server-side subnets. All on-premises Citrix Networking features are available in Azure, except for the following: clustering, IPv6, gratuitous ARP (GARP), L2 mode, tagged VLANs, dynamic routing, virtual MAC (VMAC), USIP, and jumbo frames.

More details on Citrix ADC VPX in Azure can be found [here](/en-us/citrix-adc/current-release/deploying-vpx/deploy-vpx-on-azure.html).

#### Citrix ADC VPX in AWS

Citrix ADC VPX in AWS is deployed in a Virtual Private Cloud (VPC) and is available as an Amazon Machine Image (AMI) from the AWS Marketplace in subscription-based or bring you to own license (BYOL) editions. The recommended configuration includes three Elastic Network Interfaces (ENIs): management, client-facing, and back-end subnets. All on-premises Citrix Networking features are available in AWS, except for the following: IPv6, gratuitous ARP (GARP), L2 mode, tagged VLAN, dynamic routing, virtual MAC (VMAC).

More details on Citrix ADC VPX in AWS can be found [here](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws.html).

#### MPX Overview

Citrix ADC MPX is a hardware-based, highly performant platform that provides industry-leading delivery of applications over the Internet and private networks, combining application-level security, optimization, and traffic management into a single, integrated appliance. All MPX appliances support Citrix nCore technology, which enables them to use their multi-core CPU systems for multi-gigabit performance and massive scalability for all application workloads.

An MPX can be integrated into any network as a complement to existing load balancers, servers, caches, and firewalls. It requires no additional client or server-side software and can be configured using the web-based GUI and CLI configuration utilities. Flexible Pay-As-You-Grow licensing helps customers protect their investment, avoid costly hardware upgrades, and reduce overall TCO.

#### SDX Overview

The Citrix ADC SDX platform delivers fully isolated network instances running on a single appliance. Each instance is a full-blown environment, which optimizes the delivery of applications over the internet and private networks. The SDX platform combines application-level security, optimization, and traffic management into a single, integrated appliance. The SDX appliance is architected such that each instance runs as a separate virtual machine with its own dedicated kernel, CPU resources, memory resources, address space, and bandwidth allocation. Network I/O is done in a way that not only maintains aggregate system performance but also enables complete segregation of each tenant's data and management-plane traffic.

A Citrix ADC can be connected to a network using various of methods such as one arm mode or two-arm mode. Citrix ADC requires multiple IP addresses to function on a network. The most important IP addresses are:

*  **NSIP (ADC IP):** There must be only one NSIP assigned to each instance, used for management. NSIP addresses are not shared between a High Availability (HA) Pair.
*  **VIP (Virtual Server IP):** Virtual Server IPs are used to host services on Citrix ADCs. Examples would be a Load Balancing Virtual Server, SSL VPN Virtual Server, and so on. VIP addresses are shared between a High Availability (HA) Pair.
*  **SNIP (Subnet IP):** This IP address is used to access a particular subnet. This IP is used as the source address on the network when accessing resources on the particular subnet configured for use with the subnet IP. SNIPs are shared between a High Availability (HA) pair.

## Citrix Application Delivery Management Analytics (Insight) Overview

### Web Insight

Provides visibility into enterprise web applications allowing IT administrators to monitor all web applications using the Citrix ADC by providing integrated and real-time monitoring of applications. Web Insight provides critical information such as user and server response time, enabling IT organizations to monitor and improve application performance.

### HDX Insight

Provides end-to-end visibility for ICA traffic passing through Citrix ADC. HDX Insight enables administrators to view real-time client and network latency metrics, historical reports, End-to-end performance data, and troubleshoot performance issues.

### Gateway Insight

It provides visibility into the failures that users encounter when logging on, regardless of the access mode. You can view a list of users logged on at a given time. Also the number of active users, number of active sessions, and bytes and licenses used by all users at any given time.

### Security Insight

It provides a single-pane solution to help you assess your application security status and take corrective actions to secure your applications.

### SSL Insight

SSL Insight provides visibility into secure web transactions (HTTPS). It allows IT administrators, to monitor all the secure web applications being served by the Citrix ADC by providing integrated and real-time and historical monitoring of secure web transactions.

### TCP Insight

TCP Insight provides an easy and scalable solution for monitoring the metrics of the optimization techniques and congestion control strategies (or algorithms) used in ADC instances to avoid network congestion in data transmission.

### Video Insight

The Video Insight feature provides a secure and scalable solution for monitoring the metrics of the video optimization techniques used by Citrix ADC instances to improve customer experience and operational efficiency.

### WAN Insight

WAN Insight analytics enables administrators to easily monitor the accelerated and unaccelerated WAN traffic that flows between the data center and branch WAN optimization appliances. WAN Insight also provides visibility into clients, applications, and branches on the network to help troubleshoot network issues effectively.
