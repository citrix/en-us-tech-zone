---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Secure Internet Access

## Overview

The continuous and exponential increase in bandwidth, expansive user mobility, and the shift of applications to the cloud has made it an absolute must for enterprises to secure user internet access. Since users and devices are the new network perimeter, it must be done in the cloud.

The demand for remote work has made appliance-based network security strategies unsustainable as the projected increases in bandwidth consumption combined with backhauled mobile traffic will quickly saturate the maximum capacity of any on-prem appliance architecture.

The erosion of the network perimeter due to increased use of cloud apps and services further accelerates this backhauling, resulting in a poor end-user experience and lower productivity due to latency. Citrix Secure Internet Access (Citrix SIA) shifts the focus from defending perimeters to following users to ensure internet access is secure regardless of location.

Citrix SIA uses a “Zero Trust” role-based policy model. One of the core goals of Zero Trust is to assign policies and manage access to resources based on a user’s role and identity. This foundational concept is built into the Citrix SIA platform’s policy engine.

Citrix SIA delivers comprehensive internet security to all users in all locations including:

*  Complete web & content filtering
*  Malware protection
*  Protection for outdated browsers & OS
*  SSL/TLS traffic management
*  CASB, cloud apps & social media controls
*  Advanced, real-time reporting
*  Flexible data traffic redirection for any device, anywhere

## Architecture

Citrix SIA is a Cloud Native technology operates in any cloud. The service operates in 100+ datacenters around the globe. The architecture provides not only a delivery mechanism for connections but can also attach to other solutions to improve performance and security. For example, Citrix SIA can take signatures from other threat intel technologies and route it through our cloud as well.

Through the use of containerized gateways, the data plane is separated from customer to customer and private keys for decrypting traffic are kept specific to the customer and not shared amongst others. Every work unit processing or storing data is fully dedicated to the customer who does not need to manage any of the components. Work units can scale horizontally elastically.

Citrix SIA allows administrators to explicitly define cloud zones directly within the cloud admin portal. These zones ensure users within a given region will be secured within the region and that log events generated within a given region to stay within the region. This is important for regulations such as GDPR, which require data to stay within national boundaries. Additionally, cloud zones can be used to define how users connect to the cloud, depending on location. For example, when users move from office to office, these admin-defined zones enable the dynamic bypassing of local printers and servers that apply to each office.

### Redirecting Flow

### Cloud Connector

### Authentication

## Security Features

### Malware Protection

#### Blocking or Allowing a website

#### Custom Block Pages

#### Keywords

### Data Loss Prevention

### Cloud Access Security Broker

### SSL/TLS Decryption

### Advanced Web Security

### Flow

## Citrix SDWAN

## Integration with CVAD

## Reporting