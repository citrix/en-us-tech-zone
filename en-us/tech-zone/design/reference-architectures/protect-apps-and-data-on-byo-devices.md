---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca, First2 Last2
specialThanksTo: First Last, First2 Last2
description: TODO insert description of article
tz_title: TODO insert title of article
tz_products: citrix-analytics;citrix-content-collaboration;citrix-endpoint-management;citrix-networking;citrix-secure-internet-access;citrix-secure-workspace-access;citrix-service-providers;citrix-virtual-apps-and-desktops-standard-for-azure;citrix-virtual-apps-and-desktops;citrix-workspace;google-cloud-platform;other;security;third-party-content
---
# Reference Architecture : Protect apps and data on BYO devices

## Overview

CompanyA, as a policy, provides access to internal and cloud resources to a small subset of the overall user base. The users use BYOD and unmanaged devices to access virtual apps and desktops, SaaS, and internal web applications. This policy has led to greater efficiency for their hybrid and distributed workforce; however, it has created a complex delivery model that has also introduced security concerns. Since user devices are unmanaged, CompanyA must mitigate security threats against the apps and data in transit to, in use on, and at rest on the BYOD devices.

CompanyA currently uses several unintegrated point products but wants to simplify and provide company-wide access to their virtualized apps and desktops, SaaS, and web internal apps service. Also, CompanyA must protect its internal web services.  To that end, CompanyA is engaging in an initiative to update its app delivery architecture to implement the integrated Citrix solution using Citrix Secure Workspace Access, Citrix Secure Internet Access, and Citrix Web App Firewall. Together they provide end-to-end protection of CompanyA resources.

This reference architecture explains CompanyA's plan to protect access, protect data and devices, and protect apps.

**diagram1**
placeholder

## Success Criteria

CompanyA has a deep investment in its existing infrastructure and wants to avoid a rip-and-replace approach. The company is concerned about the complexity of replacing everything and losing its existing investments. Instead, it is building a modern environment that allows access to its current virtual and web resources and protects access to SaaS apps.

CompanyA wishes to enable more users to work from home and remote locations. Post pandemic, employees (contractors and partners included) will continue benefiting from a hybrid and BYOD workforce model. Although some users have VPN access to web and SaaS apps, CompanyA has identified several security challenges that prevent a company-wide deployment.

Typically, CompanyA does not manage contractors' and partners' devices. Hence, there is no way to understand if the devices transfer any malicious content to their application infrastructure. Consequently, CompanyA has begun a threefold initiative to protect corporate resources accessed by unmanaged devices. To be successful, CompanyA defined a list of success criteria for the initiative. These criteria form the basis for the overarching design.

### Protecting User Access
CompanyA must protect user access with a secure, simple, but productive work environment with flexibility to use any device and work from any location. CompanyA must also create a simplified and safe mode of access to all apps and data, makes it seamless for end-users to access all apps and data from using any device.
CompanyA has decided its security strategy is to move away from a traditional "castle and moat" approach to access and security. Instead of using a conventional appliance-based solution like VPNs that assume remote users are trusted and allow complete access to all network resources, CompanyA is taking a Zero Trust approach.
In CompanyA's focus on protecting user access, it has identified the following criteria for a successful design:

|  Success Criteria | Description | Solution
|---|---|---|
|  Contextual access and contextual auth |  Contextual access and contextual auth for web, SaaS, and CVAD service | Citrix Secure Workspace Access  |
| End-user monitoring  |  Continuous monitoring and continuous assessment |  Citrix Secure Workspace Access  |
| Personal Mobile Devices  | Users can select an unmanaged endpoint devices that fits their usage requirements. These devices are fully are not managed CompanyA.  | xxx |
|Personal Privacy   |  CompanyA must ensure user privacy while still protecting the user and endpoint from potential threats when using unsanctioned websites. | Citrix Secure Internet Access does not decrypt policies for sites with personal information |
|  SaaS App Access | Users must access sanctioned SaaS applications with strong authentication that does not impact the experience.  | Citrix Secure Workspace Access  |
| Web App Access  | Users must be able to access sanctioned internal Web applications on any approved device  | Citrix Secure Workspace Access – Zero Trust Network Access |

### Protecting Data
CompanyA, like many companies today, has a highly complex infrastructure of layers of applications, systems, and networks in environments consisting of on-premises data centers, public and private clouds. This sprawl has led to a complicated stack of different tools and technologies for protecting data and devices. 
CompanyA is designing a consolidated, cloud-delivered security stack to meet the demands of their modern workplace. By centralizing device and data security policy across the overall solution – it will minimize redundant tasks, remove overlapping policies, and allow IT to protect data and devices across all locations from a centralized dashboard.
In CompanyA's focus on protecting data and devices, it has identified the following user-related criteria for a successful design.