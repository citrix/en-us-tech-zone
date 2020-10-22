---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Allen Furmanski, Jaskirat Chauhan, Derek Thorslund
description: Learn about the deployment architecture and how Citrix SD-WAN WANOP helps to optimize Citrix Content Collaboration for customer-managed storage zones including relevant test data.
---
# Citrix SD-WAN WANOP for Content Collaboration (Customer-Managed on-premises Storage Zones)

## Audience

This document is intended for IT decision-makers, consultants, solution integrators and partners who are in search of a reference architecture on Citrix SD-WAN for Content Collaboration with on-premises storage zone controllers. Also, it describes how Citrix SD-WAN enables a high-performance user experience for branch users while accessing the files from the Content Collaboration environment.

## Objective of this document

The purpose of this document is to describe the Citrix SD-WAN WANOP reference architecture for Citrix Content Collaboration and the significance of integrating the Citrix SD-WAN WANOP solution with Customer-Managed (on-premises) Storage Zones. Also, it helps to understand how the WANOP solution addresses the file transfer operation issues pertaining to high latency, packet loss, and jitter due to long distances between the data center and branch offices.

## Overview of Citrix Content Collaboration

Enterprise organizations seek an alternative to traditional consumer-style file-sharing services that will give them full control over enterprise data and enable access for office and mobile workers who need their data from any device, anywhere and at any time.

Citrix Content Collaboration makes it possible for users to access and sync all their data from any device and securely share it with people both inside and outside the organization for easy collaboration and enhanced productivity. Support for large file sizes and integration with workflow tools such as Microsoft® Outlook® to provide an unmatched user experience. Citrix Content Collaboration allows IT to deliver a managed service with powerful security, reporting, and auditing features designed for enterprises.

There are three primary components of Citrix Content Collaboration:

*  **Management Plane** - A Citrix-managed component hosting the Content Collaboration services and business logic, hosted in either the United States or the European Union.
*  **Storage Zones** - The location where customer files are stored. Customers have a choice in where to store files, either hosted by Citrix or hosted by the customer in their own data center or on their own public cloud service subscriptions.
*  **Citrix Files Apps** - Native apps providing access to Content Collaboration services. Citrix Files apps are available for Windows, macOS, iOS, and Android, in addition to Outlook and Gmail.

Citrix Content Collaboration offers customizable solutions for every enterprise. Companies can host data in the cloud, store it on an existing data center network, or use a hybrid model. Citrix Content Collaboration also enables access to existing network shares from any device without having to migrate data.

![SDWAN-CC-RA-Image-1](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_001.png)

This reference architecture is focused on a customer-managed storage zone hosted inside the customer data center and how SD-WAN WANOP addresses the pain points.

## Customer Managed On-Premises Storage Zones

With customer-managed Storage Zones, organizations can store data within their data center to maintain complete control of enterprise content. Also, it enables access to existing network shares from any device without having to migrate data. The branch users access the content from the data center through content collaboration over the WAN or MPLS links.

It is important for every organization to provide data access to end-users with the best possible user experience. Administrators are required to maintain the environment with no downtime and not disrupt performance, scale, or capacity as usage grows.

![SDWAN-CC-RA-Image-2](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_002.png)

The Citrix Cloud Content Collaboration management plane authorizes transfers but is not involved in the actual transfer of bytes. SSL traffic is terminated on the Citrix ADC appliance. File transfers go directly from clients in the branch office to Storage Zone Controllers, via Citrix ADC. Persistent storage repositories can also be used for network share connectors at the data center. Storage Zone Controllers call back to the control plane to notify of uploads or downloads taking place.

## Storage Zone Connectors

Storage zone connectors provide access to documents and folders in:

*  SharePoint sites, site collections, and document libraries
*  Network file shares
*  Documentum ECM via a connector (requires Storage Zone Controller 4.1 or later)

Users who have permission to view a connected resource can browse connected SharePoint sites, SharePoint libraries, and network file shares from the ShareFile web interface and ShareFile clients. By default, connector browsing is disabled for the ShareFile web interface. It can be enabled with the help of Citrix Support.

## Pain Points

There are a few user experience issues for the branch users while accessing the Content Collaboration environment which needs to be addressed:

*  File transfers with ShareFile StorageZones can experience the suboptimal performance, specifically over high latency networks and with large files
*  Unlike cloud zones which are geo-distributed, it is not always possible for customers to set up on-premises zones near branch offices
*  Operations that users expect to be fast, such as saving and editing files online, may not always be
*  There is a delay when saving edits to a file, due to an increase in the cost of reuploading an edited file because the entire file must be uploaded again.

Anytime branch users are waiting for their computers to respond is lost time, resulting in lost productivity. When users work remotely or use off-site resources, their productivity depends on the responsiveness of their network connections. Safeguarding the responsiveness of their connections requires advanced network acceleration.

**Citrix SD-WAN WAN Optimization (WANOP)** appliance helps to solve those problems and optimizes file transfers through Adaptive TCP flow control, adaptive compression & de-duplication, and HTTP protocol acceleration.

## About Citrix SD-WAN WANOP

The Citrix SD-WAN WAN Optimization edition (WANOP) provides WAN optimization capability. It supports application acceleration, data reduction, and protocol control to optimize applications across the WAN. Optionally, it can include virtual Windows Server to simplify branch infrastructure and mobile PC plug-in capability.

The Citrix SD-WAN WANOP appliance optimizes WAN links, ensuring maximum responsiveness and throughput. Citrix SD-WAN WANOP provides robust usability even with undersized or degraded links. Citrix SD-WAN WANOP appliances support a full range of optimizations, including:

*  Multi-session compression with compression ratios of up to 10,000:1.
*  Protocol acceleration for Windows network file systems (CIFS), Citrix Virtual Apps (ICA and CGP, including the multi-session ICA standard), Microsoft Outlook (MAPI), and SSL.
*  Traffic shaping to ensure that high-priority and interactive traffic takes precedence over low-priority or bulk traffic.
*  Advanced TCP protocol acceleration, which reduces delays on congested or high-latency links.

## How Citrix SD-WAN WANOP works?

Citrix SD-WAN WANOP products work in pairs, one at each end of a link, to accelerate traffic over the link. Organizations typically can install one Citrix SD-WAN WANOP appliance per site. Enterprise companies with numerous branch offices might have multiple appliances for scalability. A WAN link from a site with a Citrix SD-WAN WANOP appliance to a site that does not have a Citrix SD-WAN WANOP appliance functions normally, but its traffic is not accelerated.

Citrix SD-WAN WANOP features include robust compression for brisk performance over bandwidth-constrained links and lossless flow control to deal with congestion. TCP optimizations overcome the main limitations of problematic links, and application optimization does away with the limitations of applications designed for high-speed, local networks. An autodetection feature makes deployment quick and easy.

## Citrix SD-WAN WANOP Architecture

Citrix SD-WAN WANOP appliances accelerate the traffic over WAN links. To accelerate, at least two appliances are needed, one for each site. The sender-side Citrix SD-WAN WANOP appliance applies a series of optimizations and transformations to traffic, such as compression. Many operations require that the receiver-side Citrix SD-WAN WANOP perform an inverse operation, such as decompression, to restore the traffic to its original state. Thus, most optimizations require that the traffic passes through two Citrix SD-WAN WANOP appliances.

A single Citrix SD-WAN WANOP appliance can communicate with any number of partner sites. Point-to-point hub-and-spoke and mesh networks are all supported. Citrix SD-WAN WANOP appliances can be added to the network at will because their auto-detection and auto-negotiation features ensure that a new appliance on the network is immediately detected by other appliances, and acceleration begins at once.

Refer to the [Product documentation](/en-us/citrix-sd-wan-wanop/11/about-sd-wan-wanop.html) for more information on WANOP Architecture and its features.

The following diagram represents the Citrix SD-WAN WANOP deployment architecture on a high level:

![SDWAN-CC-RA-Image-3](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_003.png)

## Deployment Modes

An SD-WAN appliance acts as a virtual gateway. It is neither a TCP endpoint nor a router. Like any gateway, its job is to buffer incoming packets and put them onto the outgoing link at the right speed. This packet forwarding can be done in different ways, such as inline mode, virtual inline mode, and WCCP mode.

A basic deployment consideration is whether the site has a single WAN router or multiple WAN routers. Network Administrators also need to consider which features can be used in which modes. A requirement to support VPNs affects the placement of the appliance in the network.

The most basic deployment criteria are:

*  All packets in the TCP connection must pass through a supported combination of two acceleration units (Citrix SD-WAN WANOP appliances or Plug-ins)
*  Traffic must pass through the two acceleration units in both directions

When these criteria are met, acceleration is automatic.

Refer to the [Product Documentation](/en-us/citrix-sd-wan-wanop/11/cb-deployment-modes-con.html) and the [Supported Features matrix](/en-us/citrix-sd-wan-wanop/11/get-started-with-sd-wan-wanop/supported-mode-and-feature-matrix.html) for more details.

## Citrix SD-WAN WANOP Features

There are various features within the Citrix SD-WAN WANOP product line which includes:

*  Compression
*  HDX Acceleration for Citrix Virtual Apps and Desktops
*  HTTP Acceleration
*  TCP Flow-Control Acceleration
*  Traffic Shaping
*  Traffic Classification
*  Link Definitions
*  Secure Traffic Acceleration
*  Video Caching
*  Monitoring with AppFlow
*  Internet Protocol Version 6 (IPv6) Acceleration
*  SCPS Support
*  WAN Insight
*  Office 365 Acceleration

## SD-WAN Integration with Content Collaboration

Citrix SD-WAN WANOP can be integrated with a Citrix Content Collaboration environment to solve those pain points discussed earlier in this document. Let’s review the SD-WAN WANOP integration methodology, deployment architecture, and features:

We begin by introducing the Citrix SD-WAN WANOP appliance at the data center where the Storage Zone Controllers were deployed and load-balanced by Citrix ADC. Also, we need to deploy another SD-WAN WANOP appliance in the branch office(s) from where the users are connecting to the Content Collaboration environment through ShareFile Clients. It is important to install an SSL certificate on WANOP from the Content Collaboration environment to decrypt HTTPS traffic for compression and de-duplication.

The appliance can be placed in-line with the WAN links. The appliance uses two bridged Ethernet ports for inline mode. Packets enter one Ethernet port and exit through the other. This mode puts the appliance between the WAN router and LAN networks.

![SDWAN-CC-RA-Image-4](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_004.png)

The above diagram depicts the SDWAN WANOP Integration with Customer-Managed Storage Zone Controllers for a Content Collaboration environment. Let’s review the features of SD-WAN WANOP and its benefits with this deployment.

The following WANOP features that help optimize the file transfer for Content Collaboration:

*  Adaptive TCP flow control
*  Adaptive compression and de-duplication
*  HTTP protocol acceleration

## What does "acceleration" mean?

In Citrix SD-WAN WANOP terminology, “acceleration” is the reduction of transaction time, which reduces the time users spend waiting. Since the time that users spend waiting represents a direct productivity loss, acceleration’s main benefit is increased productivity.

In network traffic, a transaction ranges from small to large, as with FTP transfers, which often exceed 1 GB in size. A practical accelerator must accelerate the entire range of transaction sizes, from interactive traffic to bulk traffic, giving the best performance and user experience across the board. Citrix SD-WAN WANOP technology achieves this in various ways:

### Compression overcomes low link speeds

The most obvious problem with the WAN links is their low bandwidth compared to LAN networks. How can we overcome low link bandwidth with compression?

Citrix SD-WAN WANOP compression uses breakthrough technology to provide transparent multilevel compression. It is true compression that acts on arbitrary byte streams. The compression engine is fast, allowing the speedup factor for compression to approach the compression ratio. For example, a bulk transfer monopolizing a 1.5 Mbps T1 link and achieving a 100:1 compression ratio can deliver a speedup ratio of almost 100x, or 150 Mbps, as long as the WAN bandwidth is the only bottleneck in the transfer.

Unlike with most compression methods, Citrix SD-WAN WANOP compression history is shared between all connections that pass between the same two appliances. Data sent hours, days, or even weeks earlier by connection-A can be referred to later by connection-B and receive the full speedup benefit of compression. The resulting performance is much higher than can be achieved by conventional methods.

### TCP protocol acceleration overcomes congestion

Any attempt to send traffic faster than the link speed results in congestion, which results in many problems caused by high packet losses and high queuing latency. TCP/IP has inbuilt flow control. When it’s a slow receiver TCP limits the number of packets being sent. Flow control in case of WANOP always tries to utilize the full capacity of the link.

A Citrix SD-WAN WANOP solves this problem by providing the flow control that was omitted from the TCP/IP protocol. Unlike ordinary Quality of Service (QoS) solutions, which simply reallocate packet loss, Citrix SD-WAN WANOP provides lossless flow control that controls the rate at which the endpoint senders transmit data, instead of allowing senders to transmit data at any speed they like and dropping packets when they send too much.

Each sender transmits only as much data as Citrix SD-WAN WANOP allows it to send, without ever dropping a packet, and this data is placed on the link at exactly the right rate to keep the link full without overflowing. By eliminating excess data, Citrix SD-WAN WANOP is not forced to discard it. Without Citrix SD-WAN WANOP, the dropped packets must be sent again, causing unnecessary delays. Lossless flow control also eliminates delays caused by excessive buffering. Lossless flow control is the key to maximum responsiveness on a busy link, enabling a link that was once congested to the point of unusability at 40% utilization to remain productive and responsive at 95% utilization.

### HTTP acceleration

HTTP is an ideal application for Citrix SD-WAN WANOP multi-level compression. Static content, including standard HTML pages, images, video, and binary files, receives variable amounts of the first-pass compression, typically 1:1 on pre-compressed binary content, and 2:1 or more on text-based content.

Starting with the second time the object is seen, the two largest compression engines (memory-based compression and disk-based compression) deliver extremely high compression ratios, with larger objects receiving compression ratios of 1,000:1 or more. With such high compression ratios, the WAN link stops being the limiting factor, and the server, the client, or the LAN becomes the bottleneck.

The appliance switches between compressors dynamically to give maximum performance. For example, the appliance uses a smaller compressor on the HTTP header and a larger one on the HTTP body.

### Eliminating distance-based unfairness

Links with high latency or packet losses are difficult to use at full bandwidth, especially with ordinary TCP variants such as TCP Reno. The consequences are excessive delays and difficulty in getting the bandwidth that you are paying for. The longer the link distance, the worse the problem becomes. Citrix SD-WAN WANOP TCP protocol acceleration minimizes these effects, allowing intercontinental and even satellite links to run at full speed.

Applications and protocols designed for LAN networks are notorious for poor performance over WAN because of the effects of long speed-of-light delays on their protocols. For example, a simple Windows file system (CIFS) operation can take up to 50 round trips as messages pass back and forth across the network. In a WAN with a 100 ms round-trip time, 50 round trips cause a delay of five seconds.

Although speed-of-light delays are a fundamental limitation, application optimizations can perform the same operations in a smaller number of round-trips, usually through speculative operations. Citrix SD-WAN WANOP optimizations are especially effective on CIFS/SMB (the Windows file system), MAPI (the Outlook/Exchange protocol), and HTTP.

### Traffic shaping

HTTP consists of a mix of interactive and bulk traffic. Every user’s traffic is a mix of both. The traffic shaper seamlessly and dynamically ensures that each HTTP connection gets its fair share of the link bandwidth, preventing bulk transfers from monopolizing the link at the expense of interactive users, while also ensuring that bulk transfers get any bandwidth that interactive connections do not use.

### Optimizing the HDX access for Citrix Virtual Apps and Desktops

Citrix SD-WAN WANOP multiple optimizations enhance Citrix HDX performance. Every aspect of Citrix SD-WAN WANOP acceleration comes into play with these protocols to make the remote user experience as productive as possible. Citrix SD-WAN WANOP appliances negotiate session options with Citrix Virtual Apps and Desktops servers. This allows the Citrix SD-WAN WANOP appliance to apply the following enhancements:

*  It replaces the server’s native compression with Citrix SD-WAN WANOP higher-performance compression.
*  The connection’s traffic-shaping priority on the priority bits embedded in every HDX connection. This allows the priority of the connection to vary according to the type of traffic. For example, interactive tasks are high-priority tasks and print jobs are low-priority tasks.
*  It maintains the end-to-end encryption of the original connection and it gathers and reports statistics based on the Citrix Virtual Apps and Desktops being used.

### Minimal configuration and Auto Detection

The WANOP solution is double-ended, requiring that a Citrix SD-WAN WANOP appliance is present at both ends of the link. Citrix SD-WAN WANOP is easy to install and maintain. A typical installation takes about 20 minutes. The only parameters needed are the usual network parameters (such as IP address and subnet mask), the address of a Citrix License Server, and speed of the link to send and receive.

Requiring only a minimal level of configuration is possible because of autodetection, through which a Citrix SD-WAN WANOP determines which connections can be accelerated (and which cannot), without any manual configuration. A Citrix SD-WAN WANOP at the other end of the link is automatically detected, and the connection is then accelerated.

A Citrix SD-WAN WANOP uses TCP header options to report its presence and to negotiate acceleration parameters with the remote Citrix SD-WAN WANOP because TCP header options are part of the TCP standard. This method works well, except in cases where firewalls are programmed to reject all but the most common options. Such firewalls exist, but they can be configured to allow the options used by Citrix SD-WAN WANOP to pass through.

### Transparent Operations

Citrix SD-WAN WANOP operations are transparent to both the sender and receiver. The other devices in the network are not aware that Citrix SD-WAN WANOP exists. They continue working just as they did before Citrix SD-WAN WANOP installation. This transparency also eliminates any need to install special software on servers or clients to benefit from Citrix SD-WAN WANOP acceleration. Everything works transparently.

## SD-WAN WANOP Test Results

### File Download

With Citrix SD-WAN WANOP enabled, results show a 60% reduction in single-file download time for files ~20 MB.

![SDWAN-CC-RA-Image-5](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_005.png)

With Citrix SD-WAN WANOP enabled, results show up to a 50% reduction in file download time when the same files are downloaded by multiple users.

![SDWAN-CC-RA-Image-6](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_006.png)

### File Upload

With Citrix SD-WAN WANOP enabled, results show up to 70% reduction in single-file upload time for files ~20 MB.

![SDWAN-CC-RA-Image-7](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_007.png)

### File Edits (Sync use case)

With Citrix SD-WAN WANOP enabled, results show up to 80% reduction in single-file upload time for file edits – when a file is modified slightly and saved.

![SDWAN-CC-RA-Image-8](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_008.png)

### File Previews

With Citrix SD-WAN WANOP enabled, results show up to 30% reduction in single-file preview time for files ~20 MB.

![SDWAN-CC-RA-Image-9](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_009.png)

### Network Share Connectors

With Citrix SD-WAN WANOP enabled, results show that there is a significant reduction in file download, upload, and previews with Network Share Connectors.

![SDWAN-CC-RA-Image-10](/en-us/tech-zone/design/media/reference-architectures_sdwan-content-collaboration_010.png)

## Overall Paybacks from Citrix SD-WAN WANOP

There was a significant advantage, sometimes dramatic, reduction in user wait time for common file transfer operations. It reduced the load on Storage Zone Controllers hence improved scalability.

Citrix SD-WAN WANOP deployment is a near plug-and-play component of your overall delivery solution. There are no code changes and no Storage Zone Controller upgrade is required.

Citrix SD-WAN WANOP maximizes existing investments in on-premises infrastructure. Data center WANOP virtual appliances can be hosted along with existing Storage Zone Controllers and Citrix ADC infrastructure.

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s10adc31c5d146148).

## References

[Reference Architecture on Customer-Managed Storage Zone](/en-us/tech-zone/design/reference-architectures/customer-storage-zones-on-premises.html)

[Storage Zone Controller Architecture](/en-us/storage-zones-controller/5-0/architecture-overview.html)

[Installation Process for Storage Zone Controller](/en-us/storage-zones-controller/5-0/install.html)

[Citrix Files Deployment Guide](/en-us/tech-zone/design/reference-architectures/customer-storage-zones-on-premises.html)

[Citrix Product Documentation](/en-us/citrix-sd-wan-wanop/11/about-sd-wan-wanop.html)

[Deployment Modes for Citrix SD-WAN WANOP](/en-us/citrix-sd-wan-wanop/11/cb-deployment-modes-con.html)

[FAQ for SD-WAN WANOP](/en-us/citrix-sd-wan-wanop/11/faqs.html)

[SD-WAN WANOP Supported Mode and Feature Matrix](/en-us/citrix-sd-wan-wanop/11/get-started-with-sd-wan-wanop/supported-mode-and-feature-matrix.html)
