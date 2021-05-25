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

|  **Success Criteria** | **Description** | **Solution**
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

| **Success Criteria** |  **Description** |  **Solution** |
|---|---|---|
| Protect Devices  |  Protecting devices and the underlying infrastructure from malware and Zero-Day threats |  Citrix Secure Internet Access |
| Protect Data  |  Protect data stored in sanctioned and unsanctioned apps |Citrix Secure Internet Access   |
|  Compliance  | Compliance and protecting users from malicious URLs   | Citrix Secure Internet Access |
| Unsecured Personal Devices  |  Users trying to access Workspace with an unsecured device must not gain access to any sanctioned resource. | Citrix Application Delivery Controller (ADC) - nFactor policies and endpoint analysis |
| Protection from keylogger  |  Protection from keylogger / enabling secure access from BYO | Citrix Secure Workspace Access   |
| Internet Security  | CompanyA must protect its users from potential internet threats hidden within emails, applications, and websites regardless of location. Internet protection must extend to all delivery models, including virtual apps/desktops, local apps, mobile apps, web apps, and SaaS apps.  | Citrix Secure Internet Access  |

### Protecting Apps and application APIs

CompanyA is investing in risk mitigation and threat prevention. It has determined that its attack surface has increased with apps moving to the cloud, SaaS, and the growing use of BYO and unmanaged devices to access corporate apps. Their on-prem secure web gateway and VPN deployments with rigid security policies cannot effectively protect applications in the cloud.

CompanyA must create a hybrid solution using both on-prem devices and cloud services for application security. On-premises devices will block app-layer and DDoS attacks on-premises, while a cloud-based protection service will prevent volumetric attacks and app-layer DDoS attacks in the cloud.

To be successful, CompanyA must protect and secure its resources while simultaneously providing a flexible work environment that aligns with the user requirements. CompanyA identified the following security-related criteria for a successful design.

|  **Success Criteria** | **Description**  | **Solution** |
|---|---|---|
| SaaS and Web App Security | The user's ability to download, print, or copy data from SaaS apps containing financial, personal, or other sensitive information must be restricted  | Citrix Secure Workspace Access – Security Policies with App Protection  |
|  Secure Access | CompanyA must protect internal corporate resources when accessed from untrusted and unsecured locations. Devices are not be allowed direct access to the internal network to help prevent malware intrusion.  | VPN-less access |
|  SaaS credential protection | The user's credentials to SaaS applications must include strong, multifactor authentication.  |  Citrix Secure Workspace Access – Single Sign-On with SAML-only authentication |
| Volumetric DDoS  |  CompanyA must stop volumetric DDoS attacks at the edge before they enter the network. CompanyA must protect both cloud apps and internal apps. 
CompanyA has apps deployed in multiple locations on cloud-hosted platforms. It must protect these apps from API-level threats like DDoS and Bot attacks, cross-site scripting, and SQL Injection attacks.
 | Citrix Web Application and API Protection  |
| SaaS DLP  | CompanyA requires their SaaS apps to use DLP controls inline. | Citrix Secure Internet Access  |
|  Protecting apps and application APIs |  Protection from volumetric DDoS, bot attacks, and other application-level attacks such as cross-site scripting | Citrix Web Application and API Protection
| API Protection  | Protection from API level attacks | Citrix Web Application and API Protection |
| Compromised User Protection | IT must be able to quickly identify and mitigate threats posed by a compromised user account.
IT must protect the entire threat surface and the centralized orchestration capabilities to provide the complete security that the business requires. | Citrix Security Analytics |

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the following high-level conceptual architecture. This architecture meets all the preceding requirements while giving CompanyA the foundation to expand to additional use cases in the future.

User needs to access an internal web app
Use ADC for contextual auth (can bring in CEM to allow/deny unmanaged devices?)
Use SWA to provide ZTNA
Use SIA to protect the internet traffic from the web app
Use WAAP to protect the app from the user's compromised endpoint

**diagram**
placeholder

CompanyA's architecture framework has multiple layers. The framework provides a foundation for understanding the technical architecture for protecting users and apps deployment scenarios. All layers flow together to create a complete, end-to-end solution.
At a high level:

**User Layer**: The user layer describes the end-user environment and endpoint devices used to connect to resources.

** Regardless of device, users access resources from the Workspace app, resulting in an experience that is identical across every form factor and device platform.

**Access Layer**: The access layer describes how users authenticate to their Workspace and secondary resources.

*  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.

*  Many of the authorized resources within the environment utilize a different set of credentials than those used for the primary Workspace identity. CompanyA will utilize the single sign-on capabilities of each service to protect these secondary identities better. The applications only allow SAML-based authentication for SaaS apps, which prevents users from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer**: The resource layer authorizes specific virtualized, SaaS, and web resources for defined users and groups while defining the security policies associated with the resource.

*  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the endpoint.

*  Due to the unknown nature of the endpoint security status, CompanyA requires VPN-less access to resources using isolated browsers or virtualized sessions.

*  Similarly, due to unknown endpoint security status, CompanyA requires protection against keylogging and screen scraping malware.

*  Since CompanyA allows access to internal web apps from unmanaged devices, it must protect the resource from attacks coming from potentially compromised endpoints.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  Even within a protected Workspace resource, users can interact with untrusted Internet resources. CompanyA utilizes Secure Internet Access to protect users from external threats from SaaS apps, web apps, virtual apps, mobile apps, and apps on endpoint devices.

*  Users might need to access personal items on their unmanaged endpoint devices. Appropriate policies are defined to protect users' privacy when accessing personal sites related to health and finance.

*  CompanyA requires a Security Analytics service to identify compromised users and automatically maintain a secure environment.
The subsequent sections provide greater detail into specific design decisions for CompanyA's BYOD protection reference architecture.

The subsequent sections provide greater detail into specific design decisions for CompanyA's BYOD protection reference architecture.

## Access Layer

### Authentication

Adaptive authentication
One of the challenges CompanyA experienced with previous acquisitions was how to integrate identity providers. The process of merging identity providers can take a significant amount of time. With the new strategy, CompanyA utilizes a Citrix Application Delivery Controller to handle all authentication requests.

**diagram**
placeholder

The authentication process works by the Application Delivery Controller evaluating the user's company and then applying the correct authentication request. As CompanyA has standardized on Okta for primary authentication, the Application Delivery Controller forwards the request to Okta. Once Okta completes user authentication, Okta replies to the Application Delivery Controller with a token identifying successful authentication.

Because strong authentication is a critical first step to security, CompanyA wants to have a solution ready for organizations that are not currently using multifactor authentication. In this instance, after the user authenticates with their Active Directory domain, the Application Delivery Controller uses the native time-based one-time password engine to provide multifactor authentication for the user. Once registered to the user's device, the user can either enter the code manually or use the push notification service. Push notifications simply require the user to select "Yes" from their registered mobile device to fulfill multifactor authentication.

**nFactor Policy**
For primary authentication, the Citrix Application Delivery Controller plays a critical part. To make the authentication decisions, the Application Delivery Controller utilizes an nFactor policy.

**diagram**
placeholder

The nFactor policy must know to what company the user belongs to process authentication requests properly. Once the correct company identifier is selected, nFactor forwards the request to the correct branch of the authentication policy.

Once the nFactor policy is defined, CompanyA can continue to expand it to incorporate different organizations it acquires in the future. The nFactor policy allows CompanyA to create additional flows utilizing authentication standards that include LDAP, RADIUS, SAML, client certificates, OAuth OpenID Connect, Kerberos, and more. The nFactor policy engine provides CompanyA with the flexibility to continue integrating additional acquisitions without a redesign.

**Zero Trust Network Access**
To provide access to internal resources like private web apps, virtual apps, and virtual desktops, CompanyA plans to use the Secure Workspace Access service and the Virtual Apps and Desktops service. These two services utilize a zero trust network access solution, which is a more secure alternative to traditional VPN.

**diagram**
placeholder

The Secure Workspace Access service and the Virtual Apps and Desktops service use the cloud connectors' outbound control channel connections. Those connections allow the user to access internal resources remotely. However, those connections are:

*  Limited in scope so that only the defined resource is accessible
*  Based on the user's primary, secured identity
*  Only for specific protocols, which disallow network traversal

## Resource Layer

### Resource Security Policies

CompanyA wants to limit the risk of data loss due to an insider threat. Within the different application types, CompanyA incorporates numerous restrictions to prevent users from copying, downloading, or printing data.

As a baseline policy, CompanyA defined the following (with the ability to relax policies as needed based on user and application).

|   |   |   |
|---|---|---|
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |

The App Protection Policies Tech Brief contains additional information regarding the keylogging and screenshot prevention policies.

## Control Layer

### Web App Firewall

When users authenticate to Citrix Workspace, they access public-facing web apps. To better protect the public web app, CompanyA uses the Citrix Application Delivery Controller solution's Bot Management and Web App Firewall components.

**diagram**
placeholder

The first line of defense is Bot Management. Bots can easily crash or slow a public web app by overwhelming the service with fraudulent requests. The bot management component of the Application Delivery Controller detects a bot request and prevents it from inundating the system.
The second line of defense is the Web App Firewall. The Web App Firewall protects public-facing apps from attacks. These types of attacks would typically be buffer overflow, SQL injection, and cross-site scripting. Web App Firewall detects and denies these attacks from impacting the data and the app.

### Secure Internet Access

As users interact with SaaS, web, virtual, local, and mobile apps, they often access public internet sites. Although CompanyA has an Internet Security Compliance class all users must complete yearly, it has not entirely prevented attacks, most often originating through phishing scams.
To help protect the users and the organization, CompanyA incorporates the Secure Internet Access service and Security Analytics into the flexible work design.

**diagram**
placeholder

Any internet traffic to/from the library of apps, desktops, and devices within the organization routes through the Secure Internet Access service. The service scans any URL to verify it is safe. Functionalities within specific public sites are denied or modified. Downloads are automatically scanned and verified.

As many websites are now encrypted, part of this security process is decrypting the traffic and inspecting. CompanyA wants to allow users the flexibility to access personal sites while on company-owned devices. The service will not decrypt specific categories of websites, such as financial and health-related sites, to ensure employee privacy. To have complete transparency, CompanyA plans to make the overall security policy plan available internally.

In designing the internet security policy, CompanyA wanted to start with a baseline policy. As CompanyA continues to assess risks within the organization, they will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA made the following modifications that are applied globally:

|   |   |
|---|---|
|   |   |
|   |   |
|   |   |
|   |   |
|   |   |
|   |   |

### Security Analytics

CompanyA needs to identify and stop insider threats to the environment before the impact is too significant.

To help protect the environment, CompanyA uses Citrix Security Analytics to identify insider threats, compromised users, and compromised endpoints. In many cases, a single instance of a threat does not warrant drastic action, but a series of threats can indicate a security breach.
CompanyA developed the following initial security policies:

|   |   |   |
|---|---|---|
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
|   |   |   |
