---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca, First2 Last2
specialThanksTo: First Last, First2 Last2
description: TODO insert description of article
tz_title: TODO insert title of article
tz_products: citrix-analytics;citrix-content-collaboration;citrix-endpoint-management;citrix-networking;citrix-secure-internet-access;citrix-secure-workspace-access;citrix-virtual-apps-and-desktops
---
# Reference Architecture : Protect apps and data on BYO devices

## Overview

CompanyA provides secure remote access for a small subset of its overall user base. End users utilize Bring Your Own (BYO) devices to access internal and cloud resources which include virtual apps/desktops, web and SaaS apps. CompanyA's secure remote access policy has led to greater efficiency for its hybrid and distributed workforce; however, the policy has created a complex delivery model and introduced security concerns. Since end user devices are unmanaged, CompanyA must mitigate security threats against  apps and the data in transit to, in use on, and at rest on the BYO devices.

CompanyA currently uses several unintegrated point products for remote access. It wants to consolidate and expand to a company-wide solution while protecting its resources which include internal web apps. To that end, CompanyA is engaging in an initiative to update its app delivery architecture to implement the integrated Citrix solution using Citrix Secure Workspace Access, Citrix Secure Internet Access, and Citrix Web App Firewall. Together they provide end-to-end protection of CompanyA resources.

This reference architecture explains CompanyA's plan to protect user access, protect data and devices, and protect apps.

[![Overview](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_00.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_00.png)

## Success Criteria

CompanyA has a deep investment in its existing infrastructure and wants to avoid a rip-and-replace approach. The company is concerned about the complexity of replacing everything and losing its existing investments. However, it must build a modern app delivery environment that allows access to and protects its resources.

CompanyA wishes to enable all users to work from home and remote locations. Post pandemic, employees will continue benefiting from a hybrid and BYOD workforce model. Although some users have VPN access to web and SaaS apps, CompanyA has identified several security challenges that prevent a company-wide deployment. Therefore, CompanyA is implementing a VPN-less approach.

CompanyA does not manage end users's devices and has no way to understand if the devices transfer any malicious content to their application infrastructure. However, CompanyA's security policy does require endpoints to have some agents installed in order to provide secure access to company resources. Consequently, CompanyA has begun a threefold initiative to protect corporate resources accessed by unmanaged devices. To be successful, CompanyA defined a list of success criteria for the initiative. These criteria form the basis for the overarching design.

### Protecting User Access

CompanyA must protect user access to their work environment. Access must be secure, simple, and flexible to use any device and work from any location. CompanyA must create a simple and safe mode of access to all apps and data that is seamless for end-users.

CompanyA has decided its security strategy is to move away from a traditional "castle and moat" approach to access and security. Instead of using a conventional appliance-based solution like VPNs that assume remote users are trusted and may allow complete access to all network resources, CompanyA is taking a Zero Trust approach.

In CompanyA's focus on protecting user access, it has identified the following criteria for a successful design:

|  **Success Criteria** | **Description** | **Solution** |
|---|---|---|
| Contextual access | Contextual access for web, SaaS, and CVAD using Citrix Secure Workspace Access to determine correct level of access | Citrix Secure Workspace Access |
| Contextual authentication | Contextual authentication for web, SaaS, and CVAD using Citrix ADC identify if unmanaged devices meet requirments to login | Citrix ADC |
| End-user monitoring  | Continuous monitoring and continuous assessment to protect against potential threats. Web apps are continously monitored for data exfiltration, abnormal access times and locations.  | Citrix Secure Workspace Access |
| Personal Mobile Devices | Users can select an endpoint device that fits their usage requirements. These devices are not managed and enrollement is not required, however business related apps and data must be secured. | Citrix Endpoint Management |
| Personal Privacy | CompanyA must ensure user privacy while still protecting the user and endpoint from potential threats when using unsanctioned websites | Citrix Secure Internet Access with "do not decrypt" policies for sites with personal information |
| SaaS App Access | Users must access sanctioned SaaS applications with strong authentication that does not impact the experience | Citrix Secure Workspace Access |
| Web App Access | Users must be able to access sanctioned internal Web applications on any approved device | Citrix Secure Workspace Access – Zero Trust Network Access |

### Protecting Data

CompanyA, like many companies today, has a highly complex infrastructure of layers of applications, systems, and networks in environments consisting of on-premises data centers, public and private clouds. This sprawl has led to a complicated stack of different tools and technologies for protecting data and devices.

CompanyA is designing a consolidated, cloud-delivered security stack to meet the demands of their modern workplace. By centralizing device and data security policy across the overall solution – it will minimize redundant tasks, remove overlapping policies, and allow IT to protect data and devices across all locations from a centralized dashboard.

In CompanyA's focus on protecting data and devices, it has identified the following user-related criteria for a successful design:

| **Success Criteria** |  **Description** |  **Solution** |
|---|---|---|
| Protect Devices | Protect devices and the underlying infrastructure from malware and Zero-Day threats | Citrix Secure Internet Access - Security Policies with Malware Protection|
| Protect Data | Protect data stored in sanctioned and unsanctioned apps | Citrix Secure Internet Access – Security Policies with Web Filtering |
| Compliance | Compliance and protecting users from malicious URLs | Citrix Secure Internet Access – Security Policies with Web Filtering |
| Unsecured Personal Devices | Users trying to access Workspace with an unsecured device must not gain unfettered access to sanctioned resources. | Citrix Secure Workspace Access |
| Protection from keylogger | Protection from keylogger / enabling secure access from BYO | Citrix Secure Workspace Access – Security Policies with App Protection  |
| Protection from screen scrapers | Protection from screen scraping malware / enabling secure access from BYO | Citrix Secure Workspace Access – Security Policies with App Protection |
| Internet Security | Protect users from potential internet threats hidden within emails, applications, and websites regardless of location. |  Citrix Secure Internet Access - Security Policies with Malware Protection |

### Protecting Apps

CompanyA is investing in risk mitigation and threat prevention to protect its web apps. The company's use of BYO devices has increased the risk of compromised devices accessing corporate apps. CompanyA has also determined that its attack surface has increased due to the company moving apps to the cloud and using SaaS apps. Its on-prem secure web gateway and VPN deployments with rigid security policies cannot effectively protect applications in the cloud.

CompanyA must create a hybrid solution using both on-prem devices and cloud services for application security. On-premises devices will block app-layer and DDoS attacks on-premises, while a cloud-based protection service will prevent volumetric attacks and app-layer DDoS attacks in the cloud.

To be successful, CompanyA must protect and secure its resources while simultaneously providing a work environment that aligns with the user requirements. CompanyA identified the following security-related criteria for a successful design:

|  **Success Criteria** | **Description**  | **Solution** |
|---|---|---|
| SaaS and Web App Security | The user's ability to download, print, or copy data from SaaS apps containing financial, personal, or other sensitive information must be restricted  | Citrix Secure Workspace Access – Security Policies Enhanced Security |
| Secure Access | CompanyA must protect internal corporate resources when accessed from untrusted and unsecured locations. Devices are not be allowed direct access to the internal network to help prevent malware intrusion.  | Secure Workspace Access - VPN-less access |
| SaaS credential protection | The user's credentials to SaaS applications must include strong, multifactor authentication. | Citrix Secure Workspace Access – Single Sign-On with SAML-only authentication |
| Volumetric DDoS | CompanyA must stop volumetric DDoS attacks at the edge before they enter the network. CompanyA must protect both cloud apps and internal apps. CompanyA has apps deployed in multiple locations on cloud-hosted platforms. It must protect these apps from API-level threats like DDoS and Bot attacks, cross-site scripting, and SQL Injection attacks. | Citrix Web App Firewall |
| SaaS DLP | CompanyA requires their SaaS apps to use DLP controls inline. | Citrix Secure Internet Access  |
| Protecting web apps | Protection from volumetric DDoS, bot attacks, and other application-level attacks such as cross-site scripting | Citrix Web App Firewall |
| Compromised User Protection | IT must be able to quickly identify and mitigate threats posed by a compromised user account. IT must protect the entire threat surface and the centralized orchestration capabilities to provide the complete security that the business requires. | Citrix Security Analytics |

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the high-level conceptual architecture below. The general flow and requirements are that end users will require:

*  Protected access to SaaS apps and VPN-less access to internal web apps via Citrix Secure Workspace Access
*  Contextual authentication before being granted access to external or internal resources via Citrix Secure Workspace Access
*  Zero Trust access to specific resources via Citrix Secure Workspace Access
*  Protected access to internet traffic from the web apps, or vitual apps and desktops via using Citrix Secure Internet Access
*  Protected access to back end apps accessed from unmananged devices with Citrix Web Application Firewall

This architecture meets all the preceding requirements while giving CompanyA the foundation to expand to additional use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_01.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_01.png)

CompanyA's architecture framework has multiple layers. The framework provides a foundation for understanding the technical architecture required to protect users and apps. All layers flow together to create a complete and end-to-end solution.
At a high level:

**User Layer**: The user layer describes the end-user environment and endpoint devices used to connect to resources.

*  End user devices are BYOD. The device is unmanaged, and CompanyA does not require any agent to be installed on the device.

*  End users may install Citrix Workspace App for additional capabilities but are not required to.

*  Regardless of device, users access resources from the Cirtrix Workspace web, resulting in an experience that is protected even on unmanaged devices.

**Access Layer**: The access layer describes how users authenticate to their Workspace and secondary resources.

*  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.

*  Many of the authorized resources within the environment utilize a different set of credentials than those used for the primary Workspace identity. CompanyA will utilize the single sign-on capabilities of each service to protect these secondary identities better.

*  The applications only allow SAML-based authentication for SaaS apps, which prevents users from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer**: The resource layer authorizes specific virtualized, SaaS, and web resources for defined users and groups while defining the security policies associated with the resource.

*  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the BYOD endpoint.

*  Due to the unknown nature of the endpoint security status, CompanyA requires VPN-less access to resources using isolated browsers or virtualized sessions.

*  Similarly, due to unknown endpoint security status, CompanyA requires protection against keylogging and screen scraping malware.

*  Since CompanyA allows access to internal web apps from BYDO devices, Citrix Web App Firewall must protect the resource from attacks coming from potentially compromised endpoints.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  Even within a protected Workspace resource, users can interact with untrusted Internet resources. CompanyA utilizes Secure Internet Access to protect users from external threats from SaaS apps, web apps, and virtual apps/desktops.

*  Users might need to access personal items on their unmanaged endpoint devices. Appropriate policies are defined to protect users' privacy when accessing personal sites related to health and finance.

*  CompanyA requires a Security Analytics service to identify compromised users and automatically maintain a secure environment.

The subsequent sections provide greater detail into specific design decisions for CompanyA's BYOD protection reference architecture.

## Access Layer

### Authentication

CompanyA has determined that providing access to resources with a user name and password does not provide adequate security.
Multifactor authentication is required for all users. CompanyA will leverage Active Directory + Token for its mulitfactor method and the Citrix Gateway service to handle all authentication requests.

Citrix Workspace incorporates a cloud-delivered Time-based One-Time Password (TOTP) providing multifactor authentication. Users will register with the TOTP service, create and install a pre-shared secret key within the authenticator app on a mobile device.

Once the user successfully registers with the TOTP micro-service, the user must use the token, along with their Active Directory credentials, to successfully authenticate to Citrix Workspace.

Refer to the [Citrix Workspace Active Directory with TOTP Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp) to gain adequate knowledge on Active Directory with TOTP concepts and terminology.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_02.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_02.png)

**Zero Trust Network Access**
To provide access to internal resources like private web apps, virtual apps, and virtual desktops, CompanyA plans to use the Secure Workspace Access service and Virtual Apps and Desktops service. These two services utilize a zero trust network access solution, which is a more secure alternative to traditional VPN.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_03.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_03.png)

The Secure Workspace Access service and the Virtual Apps and Desktops service use the cloud connectors' outbound control channel connections. Those connections allow the user to access internal resources remotely. However, those connections are:

*  Limited in scope so that only the defined resource is accessible
*  Based on the user's primary, secured identity
*  Only for specific protocols, which disallow network traversal

## Resource Layer

### Resource Security Policies

CompanyA wants to limit the risk of data loss due to an insider threat. Within the different application types, CompanyA incorporates numerous restrictions to prevent users from copying, downloading, or printing data.

As a baseline policy, CompanyA has defined the following policies (with the ability to relax policies as needed based on user and application).

| **Category**  | **SaaS Apps**  | **Web Apps** | **Virtual Apps and Desktops** |
|---|---|---|---|
| Clipboard access | Denied | Denied | Denied |
| Printing | Denied | Denied | Denied |
| Navigation | Denied | Denied  | Not Applicable |
| Downloads  | Denied   | Denied  | Denied |
| Watermark  | Enabled  | Enabled | Enabled |
| Keylogging Prevention* | Enabled | Enabled | Enabled |
| Screenshot Prevention* | Enabled  | Enabled  | Enabled |

*Keylogging and Screenshot preventtion enabled with Workspace App

## Control Layer

### Web App Firewall

When users authenticate to Citrix Workspace, they access public-facing web apps. To better protect the public web app, CompanyA uses the Citrix Application Delivery Controller solution's Bot Management and Web App Firewall components.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_04.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_04.png)

The first line of defense is Bot Management. Bots can easily crash or slow a public web app by overwhelming the service with fraudulent requests. The bot management component of the Application Delivery Controller detects a bot request and prevents it from inundating the system.

The second line of defense is the Web App Firewall. The Web App Firewall protects public-facing apps from attacks. These types of attacks would typically be buffer overflow, SQL injection, and cross-site scripting. Web App Firewall detects and denies these attacks from impacting the data and the app.

### Secure Internet Access

As users interact with SaaS, web, virtual, local, and mobile apps, they often access public internet sites. Although CompanyA has an Internet Security Compliance class that all users must complete yearly, it has not entirely prevented attacks, most often originating through phishing scams.

To help protect the users and the organization, CompanyA incorporates the Secure Internet Access service and Security Analytics into the protecting apps and data on BYO devices design.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_05.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_05.png)

Any internet traffic to/from the library of apps, desktops, and devices within the organization routes through the Secure Internet Access service. The service scans any URL to verify it is safe. Functionalities within specific public sites are denied or modified. Downloads are automatically scanned and verified.

As many websites are now encrypted, part of this security process is decrypting the traffic and inspecting. CompanyA wants to allow users the flexibility to access personal sites while on company-owned devices. The service will not decrypt specific categories of websites, such as financial and health-related sites, to ensure employee privacy. To have complete transparency, CompanyA plans to make the overall security policy plan available internally.

In designing the internet security policy, CompanyA wanted to start with a baseline policy. As CompanyA continues to assess risks within the organization, it will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA made the following modifications that are applied globally:

| **Category**  | **Change**  |  **Reason** |
|---|---|---|
| Financial and Investment | Do not decrypt  | Employee privacy concerns |
| Health  |  Do not decrypt  | Employee privacy concerns |
| Adult Content | Block | |
| Drugs  | Block | |
| File Sharing  |  Block | |
| Gambling  |  Block | |
| Illegal Activity  | Block | |
| Malicious Sources  |  Block | |
| Malware Content  | Block | |
| Porn/Nudity | Block | |
| Virus & Malware | Block | |
| Violence/Hate | Block | |

The Secure Internet Access Tech Brief contains additional information regarding the web filtering and protection features.
