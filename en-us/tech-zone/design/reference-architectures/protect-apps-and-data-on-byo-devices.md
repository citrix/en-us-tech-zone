---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca, First2 Last2
specialThanksTo: First Last, First2 Last2
description: TODO insert description of article
tz_title: TODO insert title of article
tz_products: citrix-analytics;citrix-content-collaboration;citrix-endpoint-management;citrix-networking;citrix-secure-internet-access;citrix-secure-workspace-access;citrix-virtual-apps-and-desktops
---
# Reference Architecture: Protect apps and data on BYO devices

## Overview

CompanyA provides remote access to a small subset of its overall user base. These end users, who are part of a hybrid and distributed workforce, utilize Bring Your Own (BYO) devices to access internal and cloud resources. Resources include client-server apps (virtual apps and desktops), web, and SaaS apps that must be protected when accessed from untrusted devices.

CompanyA's remote access policy has led to greater efficiency for its hybrid and distributed workforce; however, the policy has created a complex delivery model and introduced security concerns. Since the end user BYO devices are unmanaged, CompanyA must mitigate security threats against apps and the data in transit to, in use on, and at rest on the BYO devices.

CompanyA currently uses several unintegrated point products for remote access. It wants to consolidate and expand to a company-wide Zero Trust Network Access (ZTNA)solution while protecting its resources. To that end, CompanyA is engaging in an initiative to update its app delivery architecture to implement the integrated Citrix solution using Citrix Secure Workspace Access, Citrix Gateway, Citrix Secure Internet Access, and Citrix Web App and API Protection. Together this solution provides end-to-end protection of CompanyA resources accessed from BYO devices.

[![Overview](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_00.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_00.png)

This reference architecture will explain CompanyA's plan to protect user access, protect data and devices, and protect apps.

## Success Criteria

CompanyA wishes to enable all users to work from home and remote locations. Post pandemic, employees will continue benefiting from a BYOD enabled hybrid and distributed workforce. Although some users currently have VPN access to web and SaaS apps, CompanyA has identified several security challenges that prevent a company-wide deployment. Therefore, CompanyA is implementing a VPN-less approach.

Since CompanyA does not manage end users's devices, it has no way to understand if the devices transfer any malicious content to their application infrastructure. Moreover, CompanyA's security policy does not require BYO devices to have agents installed in order to provide access to company resources.

Consequently, CompanyA has begun a threefold initiative to protect corporate resources accessed by BYO devices. To be successful, CompanyA defined a list of success criteria for the initiative. These criteria form the basis for the overarching design.

### Protecting User Access

CompanyA must protect BYOD user access to their work environment. Access must be secure, simple, and flexible to use any device and work from any location. CompanyA must create a simple and safe mode of access to all apps and data that is seamless for end-users.

CompanyA has decided its security strategy is to move away from a traditional "castle and moat" approach to access and security. Instead of using a conventional appliance-based solution like VPNs that assume remote users are trusted and may allow complete access to all network resources, CompanyA is taking a Zero Trust approach.

In CompanyA's focus on protecting user access, it has identified the following criteria for a successful design:

|  **Success Criteria** | **Description** | **Solution** |
|---|---|---|
| Contextual access for web and SaaS apps | Contextual access for web and SaaS apps using Citrix Secure Workspace Access to determine correct level of access | Citrix Secure Workspace Access |
| Contextual access for client-server (virtual) apps | Contextual access for client-server (virtual) apps using Citrix Secure Workspace Access and Citrix Virtual Apps and Desktops to determine correct level of access | Citrix Secure Workspace Access and Citrix Virtual Apps and Desktops |
| End-user monitoring | Continuous monitoring and continuous assessment to protect against potential threats. Apps are continously monitored for data exfiltration, abnormal access times and locations. | Citrix Analytics |
| SaaS App Access | Users must access sanctioned SaaS applications with strong authentication that does not impact the experience | Citrix Secure Workspace Access |
| Web App Access | Users must be able to access sanctioned internal Web applications on any approved device  with strong authentication that does not impact the experience | Citrix Secure Workspace Access – Zero Trust Network Access |
| Personal Privacy | CompanyA must ensure user privacy while still protecting the user and endpoint from potential threats when using unsanctioned websites | Citrix Secure Browser Service with Citrix Secure Internet Access (Using "do not decrypt" policies for sites with personal information) |

### Protecting Data

CompanyA must protect its data accessed by BYO devices. Like many companies today, CompanyA has a highly complex infrastructure of layers of applications, systems, and networks in environments consisting of on-premises data centers, public, and private clouds. This sprawl has led to a complicated stack of different tools and technologies for protecting data.

CompanyA is designing a consolidated, cloud-delivered security stack to meet the demands of their modern workplace. By centralizing data security policy across the overall solution – it will minimize redundant tasks, remove overlapping policies, and allow IT to protect data and devices across all locations from a centralized dashboard.

In CompanyA's focus on protecting data, it has identified the following criteria for a successful design:

| **Success Criteria** |  **Description** |  **Solution** |
|---|---|---|
| BYO Devices | Users access Workspace with a BYO device and must not gain unfettered access to sanctioned resources. | Citrix Secure Workspace Access |
| SaaS and Web App Security | The user's ability to download, print, or copy data from SaaS apps containing financial, personal, or other sensitive information must be restricted | Citrix Secure Workspace Access – Security Policies Enhanced Security |
| Protection from keylogger | CompanyA must protect internal corporate resources when accessed from BYO devices. Devices may be compromised and have keylogging malware installed. Key logging must be blocked while using Citrix Workspace | Citrix Secure Workspace Access – Security Policies with App Protection |
| Protection from screen scrapers | CompanyA must protect internal corporate resources when accessed from BYO devices. Devices may be compromised and have screen scraping malware installed. Screen scraping must be blocked while using Citrix Workspace screen scraping malware | Citrix Secure Workspace Access – Security Policies with App Protection |
| Internet Security | Protect users from potential internet threats hidden within emails, applications, and websites regardless of location. | Citrix Secure Browser Service with Citrix Secure Internet Access - Security Policies with Malware Protection |
| Protect Devices | Protect devices and the underlying infrastructure from malware and Zero-Day threats | Citrix Secure Browser Service with Citrix Secure Internet Access - Security Policies with Malware Protection |
| Protect Data | Protect data stored in sanctioned and unsanctioned apps | Citrix Secure Browser Service with Citrix Secure Internet Access – Security Policies with Web Filtering |
| Compliance | Compliance and protecting users from malicious URLs | Citrix Secure Browser Service with Citrix Secure Internet Access – Security Policies with Web Filtering |

### Protecting Apps

CompanyA must protect its apps accessed by BYO devices. The company's use of BYO devices has increased the risk of compromised devices accessing corporate apps. Also, its attack surface has increased due to the company moving apps to the cloud and using SaaS apps. Its current on-prem secure web gateway and VPN deployments with rigid security policies cannot effectively protect applications in the cloud.

CompanyA must create a hybrid solution using both on-prem devices and cloud services for application security. On-premises devices will block app-layer and DDoS attacks on-premises, while a cloud-based protection service will prevent volumetric attacks and app-layer DDoS attacks in the cloud.

In CompanyA's focus on protecting apps, it has identified the following criteria for a successful design:

|  **Success Criteria** | **Description**  | **Solution** |
|---|---|---|
| Secure Access | CompanyA must protect internal corporate resources when accessed from untrusted and unsecured locations. Devices are not be allowed direct access to the internal network to help prevent malware intrusion. | Secure Workspace Access - VPN-less access |
| SaaS credential protection | The user's credentials to SaaS applications must include strong, multifactor authentication. | Citrix Secure Workspace Access – Single Sign-On with SAML-only authentication |
| SaaS DLP | CompanyA requires their SaaS apps to use DLP controls inline. | Secure Browser Service with Citrix Secure Internet Access |
| Protect web apps | CompanyA must stop volumetric DDoS attacks at the edge before they enter the network. CompanyA must protect both cloud apps and internal apps. CompanyA has apps deployed in multiple locations on cloud-hosted platforms. It must protect these apps from API-level threats like DDoS and Bot attacks, cross-site scripting, and SQL Injection attacks. | Citrix Web App Firewall |
| Compromised User Protection | IT must be able to quickly identify and mitigate threats posed by a compromised user account. IT must protect the entire threat surface and the centralized orchestration capabilities to provide the complete security that the business requires. | Citrix Security Analytics |

## Conceptual Architecture

This architecture meets all the preceding requirements while giving CompanyA the foundation to expand to additional use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_01.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_01.png)

At a high level:

**User Layer**: The user layer describes the end-user environment and endpoint devices used to connect to resources.

*  End user devices are BYOD. The devices are unmanaged, and CompanyA does not require any agent to be installed on the device.

*  End users access resources from the Cirtrix Workspace web, resulting in an experience that is protected even on BYO devices.

*  End users may install Citrix Workspace App for additional capabilities but are not required to.

**Access Layer**: The access layer describes how users authenticate to their Workspace and secondary resources.

*  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.

*  Many of the authorized resources within the environment utilize a different set of credentials than those used for the primary Workspace identity. CompanyA will utilize the single sign-on capabilities of each service to protect these secondary identities better.

*  The applications only allow SAML-based authentication for SaaS apps, which prevents users from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer**: The resource layer authorizes specific client-server (virtual), web, and SaaS resources for defined users and groups while defining the security policies associated with the resource.

*  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the BYO device.

*  Due to the unknown nature of the endpoint security status, CompanyA requires VPN-less access to resources using isolated browsers or virtualized sessions.

*  Similarly, due to unknown endpoint security status, CompanyA requires protection against keylogging and screen scraping malware. (*Requires Workspace App)

*  Since CompanyA allows access to internal web apps from BYO devices, Citrix Web App Firewall must protect the resource from attacks coming from potentially compromised endpoints.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  Even within a protected Workspace resource, users can interact with untrusted Internet resources. CompanyA utilizes Secure Internet Access to protect users from external threats when using SaaS apps, web apps, and virtual apps/desktops.

*  Should users need to access personal web sites (e.g. health and finance) on their BYO devices through CompanyA resource, appropriate policies are defined to protect users' privacy.

*  CompanyA requires a Security Analytics service to identify compromised users and automatically maintain a secure environment.

The subsequent sections provide greater detail into specific design decisions for CompanyA's BYOD protection reference architecture.

## Access Layer

### Authentication

CompanyA has determined that providing access to resources with a user name and password does not provide adequate security. Multifactor authentication is required for all users. CompanyA will leverage Active Directory + Token for its mulitfactor method and the Citrix Gateway service to handle all authentication requests.

Citrix Workspace incorporates a cloud-delivered Time-based One-Time Password (TOTP) providing multifactor authentication. Users will register with the TOTP service and create a pre-shared secret key within the authenticator app on a mobile device.

Once the user successfully registers with the TOTP micro-service, the user must use the token, along with their Active Directory credentials, to successfully authenticate to Citrix Workspace.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_02.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_02.png)

Refer to the [Citrix Workspace Active Directory with TOTP Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp) to gain adequate knowledge on Active Directory with TOTP concepts and terminology.

### Zero Trust Network Access

To provide access to resources such as SaaS apps and internal web apps, virtual apps, and virtual desktops, CompanyA plans to use the Citrix Secure Workspace Access service and Virtual Apps and Desktops service. These services are a Zero Trust Network Access solution, which is a more secure alternative to a traditional VPN.

The Secure Workspace Access service and the Virtual Apps and Desktops service use the cloud connectors' outbound control channel connections. Those connections allow the user to access internal resources remotely. However, those connections are:

*  Limited in scope so that only the defined resource is accessible
*  Based on the user's primary, secured identity
*  Only for specific protocols, which disallow network traversal

[![ZTNA](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_03.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_03.png)

## Resource Layer

### Resource Security Policies

CompanyA wants to limit the risk of data loss and data remanence on BYO devices. Within the different application types, CompanyA incorporates numerous restrictions to prevent users from copying, downloading, or printing data.

CompanyA has developed prescriptive access models to meet its security requirements:

*  BYO devices **without** Workspace app use Secure Workspace Access to launch a SaaS or web app through an isolated browser using the Citrix Secure Browser service. Secure Workspace Access provides SSO and enforces contextual access policies (download, print, copy and paste restrictsions) to web and SaaS apps.

*  BYO devices **with** Workspace app use Secure Workspace Access to launch a Saas or web app using a local, containerized browser. The embedded browser creates a connection to the SaaS app or a Zero Trust Network Access connection to the internal web app. Secure Workspace Access provides SSO and enforces contextual access policies (download, print, copy and paste restrictions) and App Protection policies (screen scraping and key-logger restrictions) to web and SaaS apps.

*  When users access virtual apps and desktops, the Virtual Apps and Desktops service provides SSO and enforces lockdown policies to restrict downloading, printing, and unidirectional and bidirectional copy/paste actions.

As a baseline policy, CompanyA has defined the following policies (with the ability to relax policies as needed based on user and application).

| **Category**  | **SaaS Apps**  | **Web Apps** | **Virtual Apps and Desktops** |
|---|---|---|---|
| Clipboard access | Denied | Denied | Denied |
| Printing | Denied | Denied | Denied |
| Navigation | Denied | Denied | Not Applicable |
| Downloads | Denied | Denied | Denied |
| Watermark | Enabled | Enabled | Enabled |
| Keylogging Prevention* | Enabled | Enabled | Enabled |
| Screenshot Prevention* | Enabled | Enabled | Enabled |

*Citrix Workspace App required

[![Lockdown](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_35.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_35.png)

## Control Layer

### Web App and API Protection

When users authenticate to Citrix Workspace, they access private web apps on BYO devices. To better protect the on prem private web app, CompanyA uses the Citrix Application Delivery Controller Bot Management and Web App Firewall components.

The bot management component of the Application Delivery Controller detects a bot request and prevents it from inundating the system. The Web App Firewall protects public-facing apps from attacks. These types of attacks would typically be buffer overflow, SQL injection, and cross-site scripting. Web App Firewall detects and denies these attacks from impacting the data and the app.

CompanyA also uses the Citrix Web App and API protection service to prevent volumetric attacks and app-layer DDoS attacks against webs apps that are not on prem.

[![Citrix Web App and API Protection](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_04.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_04.png)

### Secure Internet Access

As users interact with SaaS, web, and virtual apps they often access non-CompanyA sanctioned internet sites. Although CompanyA has an Internet Security Compliance class that all users must complete yearly, it has not entirely prevented attacks, most often originating through phishing scams.

To help protect the users and the organization, CompanyA incorporates the Citrix Secure Browser service with Citrix Secure Internet Access and Security Analytics into the BYO devices design to protect apps and data.

[![Citrix Secure Internet Access](/en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_05.png)](en-us/tech-zone/design/media/reference-architectures_protect-apps-and-data-on-byo-devices_05.png)

Any CompanyA related internet traffic to/from the library of apps, desktops, and devices within the organization routes through the Secure Internet Access service. The service scans any URL to verify it is safe. Functionalities within specific public sites are denied or modified. Downloads are automatically scanned and verified.

*  When users access virtual apps and desktops, the Virtual Apps and Desktops infrastructure will have the Citrix Secure Internet Access agent installed to proxy traffic.
*  When users access web and SaaS resources with the Citrix Secure Browser service, Citrix Secure Internet Access is used as it's Secure Web Gateway.

As many websites are now encrypted, part of this security process is decrypting the traffic and inspecting. The service will not decrypt specific categories of websites, such as financial and health-related sites, to ensure employee privacy. To have complete transparency, CompanyA plans to make the overall security policy plan available internally.

In designing the internet security policy, CompanyA wanted to start with a baseline policy. As CompanyA continues to assess risks within the organization, it will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA made the following modifications that are applied globally:

| **Category** | **Change** | **Reason** |
|---|---|---|
| Financial and Investment | Do not decrypt | Employee privacy concerns |
| Health | Do not decrypt | Employee privacy concerns |
| Adult Content | Block | |
| Drugs | Block | |
| File Sharing | Block | |
| Gambling | Block | |
| Illegal Activity | Block | |
| Malicious Sources | Block | |
| Malware Content | Block | |
| Porn/Nudity | Block | |
| Virus & Malware | Block | |
| Violence/Hate | Block | |

Refer to the [Citrix Secure Internet Access Tech Brief](/en-us/tech-zone/learn/tech-briefs/secure-internet-access.html) to gain additional information regarding the web filtering and protection features.

## Summary

Based on the preceding requirements, CompanyA created the high-level conceptual architecture mentioned above. The general flow and requirements are that end users will require:

*  Protected access to SaaS apps and VPN-less access to internal web apps via Citrix Secure Workspace Access
*  Contextual authentication before being granted access to external or internal resources via Citrix Secure Workspace Access
*  Zero Trust access to specific resources via Citrix Secure Workspace Access
*  Protected access to internet traffic from the web apps, or vitual apps and desktops via using Citrix Secure Browser Service with Citrix Secure Internet Access
*  Protected access to web apps accessed from BYO devices with Citrix Web Application Firewall

 CompanyA is using Citrix to build a modern app delivery environment that enforces a Zero Trust Network Access approach and provides end-to-end protection of its resources accessed from BYO devices.
