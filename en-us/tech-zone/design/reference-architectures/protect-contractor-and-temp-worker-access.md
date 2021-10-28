---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
description: This reference architecture explains CompanyA's Zero Trust Network Access plan to protect contractor and temp worker access to its data and apps.
tz_title: Zero Trust Network Access for Contractors and Temp Workers
tz_products: citrix-analytics;citrix-networking;citrix-secure-internet-access;citrix-secure-private-access;citrix-virtual-apps-and-desktops;citrix-workspace
---
# Reference Architecture: Zero Trust Network Access for Contractors and Temp Workers

## Overview

CompanyA is complementing its full-time staff with contractors and temp workers. Using Citrix, the company has realized several productivity benefits as contractors and temp workers can get on-boarded quickly and beginning work on business projects with minimal set up time. Also, since the company’s contractors and temp workers are typically mobile, they use Citrix to access resources from anywhere, at any time, and from any device.

CompanyA's contractor and temp worker policy has led to greater efficiency for its workforce. However, the policy has created a complex delivery model and introduced security concerns. As the line between the office and the outside world continues to blur, certain steps must be taken by CompanyA to protect sensitive company information.

CompanyA currently uses several unintegrated point products for remote access. It wants to consolidate and expand to a companywide Zero Trust Network Access (ZTNA) solution while protecting its resources. CompanyA needed to take an intelligent, contextual, and adaptive approach to workspace security that protects its contractors, temp workers, and data following the Zero Trust model. To that end, CompanyA is engaging in an initiative to update its app delivery architecture.

[![Overview](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_00.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_00.png)

CompanyA chose Citrix for its has expertise in foundational technologies like adaptive access, networking, least privilege and data protection. The company is implementing the integrated Citrix solution using Citrix Secure Private Access, Citrix Gateway, Citrix Secure Internet Access, and Citrix Web App and API Protection. Together this solution provides end-to-end protection of CompanyA resources accessed from contractor and temp worker endpoint devices.

This reference architecture explains CompanyA's plan to protect contractor and temp worker access to its data and apps.

## Success criteria

CompanyA wishes to enable all contractors and temp workers to work from home and remote locations. Although some contractors and temp workers currently have VPN access to web and SaaS apps, CompanyA has identified several security challenges that prevent a company-wide deployment. Therefore, CompanyA is implementing a VPN-less approach.

CompanyA must mitigate security threats against apps and the data in transit to, in use on, and at rest on the endpoint. After careful consideration of its security and IT requirements, CompanyA has decided to implement a managed endpoint deplyoment model for both contract and temp wokers. Since contractors and temp workers will be accessing sensitive apps and data, they will be provisioned a managed company endpoint requiring additional security measures. CompanyA's security policy requires that managed endpoints have security software agents installed to be granted access to company resources.

CompanyA has begun a threefold initiative to protect corporate resources accessed by contractor and temp worker endpoints. To be successful, CompanyA defined a list of success criteria for the initiative. These criteria form the basis for the overarching design.

### Protecting Access

CompanyA must protect contractor and temp worker access to their work environment. The company must create a safe mode of access to all apps and data that is seamless. Access must be secure, simple, and flexible to work from any location.

CompanyA has decided its security strategy is to move away from a traditional "castle and moat" approach to access and security. It is taking a Zero Trust approach instead of using a conventional appliance-based solution like a VPN that assumes contractors and temp workers are trusted.

In CompanyA's focus on protecting contractor and temp worker access, it has identified the following criteria for a successful design:

|  **Success Criteria** | **Description** | **Solution** |
|---|---|---|
| Block Unmanaged Devices | Contractors and temp workers trying to access Citrix Workspace with an unmanaged device must not gain access to any sanctioned resource | Citrix Application Delivery Controller (ADC) - nFactor policies and endpoint analysis |
| Adaptive access to web and SaaS apps | Adaptive access to web and SaaS apps using Citrix Secure Private Access to determine the correct level of access | Citrix Secure Private Access |
| Adaptive access to client-server (virtual) apps | Adaptive access to client-server (virtual) apps using Citrix Secure Private Access and Citrix Virtual Apps and Desktops to determine the correct level of access | Citrix Secure Private Access and Citrix Virtual Apps and Desktops |
| End-user monitoring | Continuous monitoring and continuous assessment to protect against potential threats. Apps are continuously monitored for data exfiltration, and abnormal access times and locations. | Citrix Analytics |
| SaaS App Access | Contractors and temp workers must access sanctioned SaaS applications with strong authentication that does not impact the user experience | Citrix Secure Private Access |
| Web App Access | Contractors and temp workers must access sanctioned internal Web applications with strong authentication that does not impact the user experience | Citrix Secure Private Access – Zero Trust Network Access |
| Personal Privacy | CompanyA must ensure contractor and temp worker privacy while still protecting the endpoints from potential threats when using unsanctioned websites | Citrix Secure Browser Service with Citrix Secure Internet Access (Using "do not decrypt" policies for sites with personal information) |

### Protecting Data

CompanyA must protect its data accessed by devices used by contractors and temp workers. The company has a highly complex infrastructure of layers of applications, systems, and networks in environments consisting of on-premises data centers, public, and private clouds. This sprawl has led to a complicated stack of different tools and technologies for protecting data.

CompanyA is designing a consolidated, cloud-delivered security stack to meet the demands of its modern workplace. By centralizing data security policy across the overall solution – it minimizes redundant tasks, removes overlapping policies, and allows IT to protect data and devices across all locations.

In CompanyA's focus on protecting data, it has identified the following criteria for a successful design:

| **Success Criteria** |  **Description** |  **Solution** |
|---|---|---|
| SaaS and Web App Security | The contractor's and temp worker's ability to download, print, or copy data from SaaS apps containing financial, personal, or other sensitive information must be restricted | Citrix Secure Private Access – Security Policies Enhanced Security |
| Protection from keyloggers | CompanyA must protect internal corporate resources when accessed from both contractor and temp worker endpoints. Endpoints can be compromised and have keylogging malware installed. Key logging must be blocked while using Citrix Workspace | Citrix Secure Private Access – Security Policies with App Protection |
| Protection from screen scrapers | CompanyA must protect internal corporate resources when accessed from contractor and temp worker endpoints. Endpoints can be compromised and have screen scraping malware installed. Screen scraping must be blocked while using Citrix Workspace | Citrix Secure Private Access – Security Policies with App Protection |
| Remote Browser Isolation | Social Media web site will be launched in a remote browser session | Citrix Secure Browser Service |
| Internet Security | Protect contractors and temp workers from potential internet threats hidden within emails, applications, and websites regardless of location. | Citrix Secure Internet Access - Security Policies with Malware Protection |
| Protect Devices | Protect endpoints and the underlying infrastructure from malware and Zero-Day threats | Citrix Secure Internet Access - Security Policies with Malware Protection |
| Protect Data | Protect data stored in sanctioned and unsanctioned apps | Citrix Secure Internet Access – Security Policies with Web Filtering |
| Compliance | Compliance and protecting contractors from malicious URLs | Citrix Secure Internet Access – Security Policies with Web Filtering |

### Protecting Apps

CompanyA must protect its apps accessed by contractor and temp worker endpoints. Its attack surface has increased due to the company moving apps to the cloud and using SaaS apps. CompanyA's current on-prem secure web gateway and VPN deployments with rigid security policies cannot effectively protect applications in the cloud.

CompanyA must create a hybrid solution using both on-prem devices and cloud services for application security. On-prem devices block app-layer and DDoS attacks on-premises, while a cloud-based protection service prevent volumetric attacks and app-layer DDoS attacks in the cloud.

In CompanyA's focus on protecting apps, it has identified the following criteria for a successful design:

|  **Success Criteria** | **Description**  | **Solution** |
|---|---|---|
| Secure Access | CompanyA must protect internal corporate resources when accessed from contractor and temp worker devices. Devices are not be allowed direct access to the internal network to help prevent malware intrusion. | Secure Private Access - VPN-less access |
| SaaS credential protection | The contractor's or temp worker's credentials to SaaS applications must include multifactor authentication. | Citrix Secure Private Access – Single Sign-On with SAML-only authentication |
| SaaS DLP | CompanyA requires its SaaS apps to use DLP controls inline. | Secure Browser Service with Citrix Secure Internet Access |
| Remote Browser Isolation | Social Media sites will be launched in a remote browser session | Citrix Secure Browser Service |
| Protect web apps | CompanyA must stop volumetric DDoS attacks at the edge before they enter the network. CompanyA must protect both cloud apps and internal apps. CompanyA has apps deployed in multiple locations on cloud-hosted platforms. It must protect these apps from API-level threats like DDoS and Bot attacks, cross-site scripting, and SQL Injection attacks. | Citrix Web App Firewall |
| Compromised Contractor Protection | IT must be able to quickly identify and mitigate threats posed by a compromised contractor or temp worker account. IT must protect the entire threat surface with centralized orchestration capabilities to provide the complete security that the business requires. | Citrix Security Analytics |

## Conceptual Architecture

This architecture meets all the preceding requirements while giving CompanyA the foundation to expand to more use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_01.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_01.png)

At a high level:

**User Layer**: The user layer describes the contractor and temp worker environment and endpoint devices used to connect to resources.

*  Contractor and temp worker endpoint devices are managed by CompanyA using Citrix Endpoint Management.
*  Contractor and temp worker endpoint devices require Citrix Workspace app to be installed.
*  Contractor and temp worker endpoint devices must have App Protection enabled in Workspace app.
*  The Citrix Secure internet Access agent is installed on the Virtual Apps and Desktops infrastructure.

**Access Layer**: The access layer describes how conctractors authenticate to their Workspace and secondary resources.

*  Citrix Gateway will verify that the contractor or temp worker device has a device certificate before the logon page appears.
*  Citrix Workspace provides the primary authentication broker for all subsequent resources.
*  CompanyA requires multifactor authentication to improve authentication security.
*  Many of the authorized resources within the environment utilize a different set of credentials than those credentials used for the primary Workspace identity. CompanyA will use the single sign-on capabilities of each service to protect these secondary identities better.
*  The applications only allow SAML-based authentication for SaaS apps. This prevents contractors and temp workers from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer**: The resource layer authorizes specific client-server (virtual), web, and SaaS resources for defined users and groups while defining the security policies associated with the resource.

*  CompanyA requires policies that disable the ability to print, download, copy and paste content from the managed resource to and from the contractor and temp worker endpoint device.
*  CompanyA requires VPN-less access to resources for contractors and temp workers.
*  Sensitive SaaS apps are given additional protection provided by the Citrix Workspace app. If the contractor or temp worker endpoint device does not have app protection available, adaptive access policies prevent them from launching the app.
*  Since CompanyA allows access to internal web apps from contractor and temp worker endpoint devices, Citrix Web App Firewall must protect the resource from attacks coming from potentially compromised endpoints.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of contractors and temp workers.

*  Even within a protected Workspace resource, contractors and temp workers can interact with untrusted internet resources. CompanyA uses Secure Internet Access to protect contractors and temp workers from external threats when using SaaS apps, web apps, and virtual apps and desktops.
*  Social Media sites will be launched in a remote browser session using the Citrix Secure Browser service.
*  If contractor or temp worker must access personal web sites such as health and finance on their devices through CompanyA resource, appropriate policies protect their privacy.
*  CompanyA requires a Security Analytics service to identify compromised contractors and temp workers and automatically maintain a secure environment.

The subsequent sections provide greater detail into specific design decisions for CompanyA's contractor and temp worker protection reference architecture.

## Access Layer

### Authentication

CompanyA's authentication policy denies access if the contractor or temp worker device does not pass an endpoint security scan. The scan verifies the device is managed by checking for a device certificate. When the device passes the scan, contractors and temp workers can then log on to the Citrix Gateway. CompanyA has determined that providing access to resources with a username and password does not provide adequate security and will require multifactor authenitication. Contractors and temp workers will use their Active Directory credentials and a TOTP token to authenticate.

Citrix Workspace incorporates a cloud-delivered Time-based One-Time Password (TOTP) providing multifactor authentication. Contractors and temp workers register with the TOTP service and create a pre-shared secret key within the authenticator app on a mobile device. Once the contractor or temp worker successfully registers with the TOTP micro-service, they must use the token, along with their Active Directory credentials, to successfully authenticate to Citrix Workspace.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_02.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_02.png)

Refer to the [Citrix Workspace Active Directory with TOTP Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp) to gain adequate knowledge on Active Directory with TOTP concepts and terminology.

### Zero Trust Network Access

CompanyA uses the Citrix Secure Private Access service and the Virtual Apps and Desktops service to provide access to SaaS and internal web apps, virtual apps, and virtual desktops. These services are a Zero Trust Network Access solution, which is a more secure alternative to a traditional VPN.

The Secure Private Access service and the Virtual Apps and Desktops service use the cloud connectors' outbound control channel connections. Those connections allow the contractor to access internal resources remotely. However, those connections are:

*  Limited in scope so that only the defined resource is accessible
*  Based on the contractor’s primary, secured identity
*  Only for specific protocols, which disallow network traversal

[![ZTNA](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_03.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_03.png)

## Resource Layer

### Resource Security Policies

CompanyA wants to limit the risk of data loss and data remanence on contractor and temp worker endpoint devices. Since CompanyA has both sensitive and regular virtual, SaaS, and web apps, it will apply adaptive access policies based on its security requirements. Within the different application types, CompanyA incorporates numerous restrictions to preventing the copying, downloading, or printing of data.

[![Lockdown](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_35.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_35.png)

*  Contractor and temp worker endpoint devices require Citrix Workspace app to be installed for access to company resources.
*  Contractor and temp worker endpoint devices **with** Citrix Workspace app use Secure Private Access to launch SaaS or web apps using Citrix Workspace Browser - a containerized browser local to the endpoint.
    *  Citrix Workspace Browser creates a connection to the SaaS app or a Zero Trust Network Access connection to the internal web app.
    *  Secure Private Access provides SSO and enforces adaptive access policies (download, print, copy, and paste restrictions).
*  As a fall back, contractor and temp worker endpoint devices **without** Citrix Workspace app use Secure Private Access to launch a virtua, SaaS, or web app through an isolated browser using the Citrix Secure Browser service.
    *  Secure Private Access provides SSO and enforces adaptive access policies such as download, print, copy, and paste restrictions to web and SaaS apps.
*  App Protection policies protect web and SaaS apps using screen scraping and key-logger restrictions.
    *  CompanyA will require App Protection polices for sensitive SaaS and Web apps.
    *  If the contractor or temp worker endpoint device does not have app protection available, adaptive access policies prevent the contractor from launching the app.
*  To further isolate potentially malicous web content and reduce resource consumption on the Virtual Apps and Desktops environment, Social Media sites will be launched in a remote browser session using the Citrix Secure Browser service.
*  When contractors and temp workers access virtual apps and desktops, the Virtual Apps and Desktops service provides SSO and enforces lockdown policies. The service restricts downloading, printing, and unidirectional and bidirectional copy & paste actions.

CompanyA has developed the following prescriptive access models to meet its contractor and temp worker security requirements:

| **Category**  | **SaaS Apps** | **Sensitive SaaS Apps** | **Web Apps** | **Sensitive Web Apps** | **Virtual Apps and Desktops** |
|---|---|---|---|---|---|
| Clipboard access | Allowed | Denied | Allowed | Denied | Denied |
| Printing | Allowed | Denied | Allowed | Denied | Denied |
| Navigation | Denied | Denied | Denied | Denied | Not Applicable |
| Downloads | Allowed | Denied | Allowed | Denied | Denied |
| Watermark | Disabled | Enabled | Disabled | Enabled | Enabled |
| Keylogging Prevention | Disabled | Enabled | Disabled | Enabled | Enabled |
| Screenshot Prevention | Disabled | Enabled | Disabled | Enabled | Enabled |

## Control Layer

### Web App and API Protection

When contractors and temp workers authenticate to Citrix Workspace, they access private internal web apps. To better protect the on-prem web apps, CompanyA uses the Citrix Application Delivery Controller Bot Management and Web App Firewall components.

The bot management component of the Application Delivery Controller detects a bot request and prevents it from inundating the system. The Web App Firewall protects public-facing apps from attacks. These types of attacks would typically be buffer overflow, SQL injection, and cross-site scripting. Web App Firewall detects and denies these attacks from impacting the data and the app.

CompanyA also uses the Citrix Web App and API protection service to prevent volumetric attacks and app-layer DDoS attacks against webs apps which are not on-prem.

[![Citrix Web App and API Protection](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_04.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_04.png)

### Secure Internet Access

As contractors and temp workers interact with SaaS, web, and virtual apps and desktops they often access non-CompanyA sanctioned internet sites. To help protect contractors and organization, CompanyA incorporates Citrix Secure Internet Access and Security Analytics into the design.

[![Citrix Secure Internet Access](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_05.png)](/en-us/tech-zone/design/media/reference-architectures_protect-contractor-and-temp-worker-access_05.png)

Any CompanyA related internet traffic to/from the library of apps, desktops, and devices within the organization routes through the Secure Internet Access service. The service scans any URL to verify it is safe. Functionalities within specific public sites are denied or modified. Downloads are automatically scanned and verified.

*  Contractor and temp worker endpoint devices are company managed and will use Citrix Secure Internet Access to proxy internet traffic when:
    *  Contractors and temp workers access virtual apps and desktops. The Virtual Apps and Desktops infrastructure has the Citrix Secure Internet Access agent installed to proxy and filter internet traffic.
    *  CompanyA has decided that Social Media sites will be redirected to the Citrix Secure Browser service to isolate and offload that internet traffic from the Virtual Apps and Desktops infrastracture. Citrix Secure Internet Access is used as its Secure Web Gateway.

As many websites are now encrypted, part of this security process is decrypting the traffic and inspecting. The service does not decrypt specific categories of websites, such as financial and health-related sites, to ensure employee privacy.

In designing the internet security policy, CompanyA wanted to start with a baseline policy. As CompanyA continues to assess risks within the organization, it will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA has the following policies applied globally:

| **Category** | **Change** | **Reason** |
|---|---|---|
| Financial and Investment | Do not decrypt | Employee privacy concerns |
| Health | Do not decrypt | Employee privacy concerns |
| Adult Content | Block | Company Policy |
| Drugs | Block | Company Policy |
| File Sharing | Block | Company Policy |
| Gambling | Block | Company Policy |
| Illegal Activity | Block | Company Policy |
| Malicious Sources | Block | Company Policy |
| Malware Content | Block | Company Policy |
| Porn/Nudity | Block | Company Policy |
| Virus & Malware | Block | Company Policy |
| Violence/Hate | Block | Company Policy |

Refer to the [Citrix Secure Internet Access Tech Brief](/en-us/tech-zone/learn/tech-briefs/secure-internet-access.html) to gain additional information regarding the web filtering and protection features.

## Summary

Based on the preceding requirements, CompanyA created the high-level conceptual architecture. The general flow and requirements are that contractors require:

*  Protected access to SaaS apps and VPN-less access to internal web apps via Citrix Secure Private Access
*  Adaptive authentication before being granted access to external or internal resources via Citrix Secure Private Access
*  Zero Trust access to specific resources via Citrix Secure Private Access
*  Protected access to internet traffic from the web apps, or virtual apps and desktops via Citrix Secure Internet Access and Citrix Secure Browser Service.
*  Protected access to web apps accessed from contractor and temp worker endpoint devices with Citrix Web Application Firewall

CompanyA is using Citrix to build a modern app delivery environment that enforces Zero Trust Network Access and provides end-to-end protection of its resources.
