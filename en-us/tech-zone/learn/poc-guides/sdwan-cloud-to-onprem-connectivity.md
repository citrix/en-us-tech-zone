---
layout: doc
description: Copy & paste description from TOC here
---
# Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity

## Contributors

**Author:** [Matt Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Karthick Srivatsan

## Overview

Use of the Cloud to deliver Enterprise services continues to grow. The recent need for significant portions of the workforce to work from home is driving demand for more scalable resources available from anywhere on demand.  However, those Cloud services typically still need to make backend calls to Data Centers systems such as; service account authenticating to Active Directory, applications pulling records from databases, virtual desktops copying files from shares.

Networking options like Express Routes, or Vnet Peering, available in Microsoft Azure for example, have disadvantages including complexity, cost, or scalability.  Also, they are limited to passing point-to-point traffic without providing performance benefits, require integration with complex routing to expand their use, and are not portable between public cloud platforms.

Citrix SD-WAN virtual appliances (VPX) may be deployed in Microsoft Azure, Amazon Web Services (AWS), or Google Cloud Platform (GCP) and integrate with Citrix SD-WAN appliances hosted in Data Centers or Private Clouds. The “virtual path” or secure tunnel that is established between SD-WAN instances seamlessly routes traffic between the Cloud and Data Center, across the Internet, while enhancing performance in transit.

Citrix SD-WAN uniquely transfers flows on a per packet basis across a secure UDP tunnel.  Using the proprietary header attached to each packet it measures in real-time the connectivity quality including latency, loss, or jitter. Therefore, when multiple links are available it steers flows to find the best path according to quality requirements.

[![Cloud-to-Data Center Connectivity Overview](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)](/en-us/tech-zone/learn/media/poc-guides_sdwan-cloud-to-onprem-connectivity-overview.png)
