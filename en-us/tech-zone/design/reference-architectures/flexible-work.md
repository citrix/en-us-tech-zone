---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Learn how to design an environment to support a flexible work style without compromising IT security. The reference architecture incorporates Secure Internet Access, Secure Workspace Access, Virtual Apps and Desktops, SD-WAN, Endpoint Management and Security Analytics.
---
# Flexible Work

## Overview

For years, CompanyA supported remote work for a small percentage of the overall user population. To hire the best people from any geography, CompanyA is investigating expanding remote work to be a company wide policy. This policy not only opens up the pool of potential employee candidates, but also provides current employees with better work/life flexibility.

However, the global pandemic of 2020-2021 brought this initiative to the forefront. Although CompanyA was able to weather the severity of the pandemic, there is a heightened awareness to devise and implement a solution in a timely fashion.

This reference architecture explains how CompanyA is planning their Citrix Workspace solution to support a flexible work style without compromising IT security.

## Success Criteria

Although many users have a primary work environment, the solution needs to support a flexible work style allowing users to work from anywhere, as needed. To be successful, CompanyA defined a list of success criteria that forms the basis for the overarching design.

### User Experience

The most important aspect of a flexible work solution is to meet the needs of the user. CompanyA identified the following user-related criteria for a successful design.

| **Success Criteria** | **Description** | **Solution**
---|---|---|
|**Unified Experience**|Users might access the resources with numerous devices, including PCs, tablets, and smartphones. Having a unified experience across all platforms helps avoid end user confusion.|Citrix Workspace|
|**Personal Mobile Devices**|Users are able to select from a list of corporate managed mobile devices. With mobile devices, CompanyA allows the user to modify the device for personal use while keeping corporate security policies in place.|Citrix Endpoint Management – Corporate Owned, Personally Enabled (COPE)|
|**Personal PC Devices**|Users are able to select from a list of corporate managed endpoint devices that fits their usage requirements. These devices are fully managed and secured by CompanyA.|Citrix Endpoint Management – Choose Your Own Device (CYOD) |
|**Redundant Home Network**|Users working from home in a mission critical scenario, must have redundant, highly-available network connections.|Citrix SD-WAN 110 home appliance|
|**Personal Privacy**|When using unsanctioned websites, the user’s privacy must be ensured while still protecting the user and endpoint from potential threats.|Citrix Secure Internet Access with do not decrypt policies for sites with personal information|
|**SaaS App Access**|Users must be able to access sanctioned SaaS applications, with strong authentication that does not impact the experience.|Citrix Secure Workspace Access|
|**Web App Access**|Users must be able to access sanctioned, internal Web applications on any approved device.|Citrix Secure Workspace Access – Zero Trust Network Access|
|**Virtual App and Desktop Access**|Users must be able to access authorized Windows apps and desktops on any approved device.|Citrix Virtual Apps and Desktops Service|
|**Direct Virtual App and Desktop routing**|Users working from the local, office environment are be able to utilize a direct route to the resource without compromising security policies.|Citrix Virtual Apps and Desktops Service with Direct Workload Connection|
|**Virtual Meeting Support**|As users are no longer constrained to working within the same location, the use of virtual meeting solutions, like Microsoft Teams, is a necessity. Every user is able to access the application and use local webcams/microphones.|Citrix Virtual Apps and Desktops Service with multimedia conferencing policies|

### Security

To be successful, CompanyA must be able to protect and secure their resources while simultaneously providing a flexible work environment that aligns with the user requirements. CompanyA identified the following security-related criteria for a successful design.

| **Success Criteria** | **Description** | **Solution**
---|---|---|
|**Unsecured Personal Devices**|Users trying to access Workspace with an unsecured device must be unable to gain access to any sanctioned resource.|Citrix Application Delivery Controller (ADC) - nFactor policies and endpoint analysis|
|**Secure Home Network** |Users working from home with sensitive financial and personal data must do so from a separate, secured network. |Citrix SD-WAN 110 home appliance|
|**Internet Security**|Regardless of location, users must be protected from potential Internet threats hidden within emails, applications, and websites. Internet protection must extend to all delivery models, including virtual apps/desktops, local apps, mobile apps, web apps, and SaaS apps. |Citrix Secure Internet Access|
|**SaaS and Web App Security**|The user’s ability to download, print or copy data from SaaS apps containing financial, personal or other sensitive information must be restricted. |Citrix Secure Workspace Access – Security Policies with App Protection|
|**Virtual App & Desktop Security**|Users accessing virtual apps and desktops must not be able to copy, download or print financial, personal or other sensitive information. |Citrix Virtual Apps and Desktops – Security Policies with App Protection|
|**Secure Access**|Internal, corporate resources must be protected from untrusted and unsecured locations. To help prevent malware intrusion, devices are not be allowed direct access to the internal network. |VPN-less access|
|**Multifactor authentication**|With security being top of mind, MFA is required to ensure another layer of authentication protection of corporate resources.|Citrix ADC - Time-Based One-Time Password with Push|
|**SaaS credential protection**|The user’s credentials to SaaS applications must include strong, multifactor authentication.|Citrix Secure Workspace Access – Single Sign-On with SAML-only authentication|
|**Compromised User Protection**|IT must be able to quickly identify and mitigate threats posed by a compromised user account.|Citrix Security Analytics|

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the following high-level, conceptual architecture. This architecture meets all of the preceding requirements while giving CompanyA the foundation  to expand to additional use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_flexible-work_conceptual.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_conceptual.png)

The architecture framework is divided into multiple layers. The framework provides a foundation for understanding the technical architecture for the flexible work deployment scenarios. All layers flow together to create a complete, end-to-end solution.

At a high-level:

**User Layer**: The user layer describes the end-user environment and end-point devices that are used to connect to resources.

*  Regardless of device, users access resources from Workspace app, resulting in an experience that is identical across every form factor and device platform.
*  Every endpoint device must be secured. Users who prefer to use a personal mobile device must still enroll the device with the organization.
*  Depending on the role, certain users might require a separate, secure home network or fault tolerant network connections.
*  In a flexible work environment, CompanyA provides everyone with access to a virtualized Microsoft Teams, which includes Workspace App integrated optimizations.

**Access Layer**: The access layer describes details surrounding how users authenticate to their Workspace and secondary resources.

*  Before a user is able to access the logon page for Citrix Workspace, the Endpoint Management service scanned the endpoint to verify it is a corporate managed device. To better protect resources, CompanyA requires some level of management for all endpoints devices (device level management or Windows and application level management for mobile).
*  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.
*  Many of the authorized resources within the environment utilize a different set of credentials than the ones used for the primary Workspace identity. CompanyA will utilize the single sign-on capabilities of each service to better protect these secondary identities. For SaaS apps, the applications only allow SAML-based authentication, which prevents users from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer**: The resource layer authorizes specific SaaS, web, and virtual resources for defined users and groups while defining the security policies associated with the resource.

*  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the endpoint.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  Even within a protected Workspace resource, users have the ability to interact with untrusted Internet resources. CompanyA utilizes Secure Internet Access to protect the user from external threats from SaaS apps, web apps, virtual apps, mobile apps, and apps on endpoint devices.
*  Users might need to access personal items on the managed endpoint devices. Appropriate policies are defined to protect the user’s privacy when accessing personal sites related to health and finance.
*  With all of the policies in place to protect the users when working in a flexible environment, there are still risks. CompanyA uses the Security Analytics service to identified compromised users and automatically take actions to maintain a secure environment.

The subsequent sections provide greater detail into specific design decisions for CompanyA’s flexible work reference architecture.

## User Layer

### Home Network Connectivity

For many users, accessing their Workspace is simply a matter of connecting to their home network, which will be shared between work and personal use. However, based on certain end user requirements, CompanyA  requires either redundant connections or a secured home network.

Based on these requirements, CompanyA uses a combination of the following three options based on user requirements.

[![Home Network Connectivity](/en-us/tech-zone/design/media/reference-architectures_flexible-work_home-network-connectivity.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_home-network-connectivity.png)

From a connectivity perspective, CompanyA has four main types of users, which relate to the defined home connectivity options:

|**Users**|**Requirements**|**Solution**|
---|---|---|
|Executives|Secure home network with redundant connections|Option 3: Secured, Redundant Network - Citrix SD-WAN 110 with LTE|
|Finance|Secure home network|Option 2: Secured Network - Citrix SD-WAN 110|
|Call Center|Redundant connections|Option 3: Secured, Redundant Network - Citrix SD-WAN 110 with LTE|
|Everyone|Baseline|Option 1: Shared Network|

Certain users within the organization deal with sensitive information, like financial data. To better protect the network communication, those users require a separate, protected home network. This user group deploys an SD-WAN 110 appliance into their home network. Although they still share the primary ISP connection, the work devices use the protected home network. The separate network allows CompanyA to apply security policies to the remote work network without impacting the rest of the home network.

Certain user groups require fault tolerant connections. Those users deploy an SD-WAN 110 appliance with a backup LTE connection. When the SD-WAN 110 appliance detects reliability issues with the primary ISP, SD-WAN automatically fails over to the LTE connection.

Although the SD-WAN 110 appliances are deployed within the user’s home, they are centrally managed. Centrally managing the devices allows CompanyA to institute a zero touch deployment approach for home users following these guides:

*  [SD-WAN 110 Zero Touch Deployment via LTE](https://support.citrix.com/article/CTX272228)
*  [SD-WAN 110 Zero Touch Deployment with Broadband](https://support.citrix.com/article/CTX272229)

Learn more about [SD-WAN 110 for Home Office Users](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-home-office.html).

### Endpoint Security

CompanyA has two different strategies for endpoint devices based on the form factor.

1.  Traditional Devices: For traditional devices like desktops, PCs, and laptops, CompanyA uses Mobile Device Management (MDM) with a Choose Your Own Device (CYOD) strategy. With this approach, CompanyA is responsible for purchasing, securing, and maintaining the device. To make the overall management easier while still allowing end user satisfaction, CompanyA has a list of approved devices ranging from Windows to Mac. Users are able to access personal resources from the device, but the device is managed, secured, monitored, and audited by CompanyA.
2.  Mobile Devices: For mobile devices like iPhones and Android phones, CompanyA uses Mobile App Management (MAM) with a Company Owned, Personally Enabled (COPE) strategy. Just like the Choose Your Own Device strategy, the Company Owned, Personally Enabled strategy provides the user with a list of approved mobile devices. The device is purchased and secured by CompanyA, while still allowing users to access personal apps that are not protected or audited by the organization. Work-related applications are containerized, helping to protect work-related apps and content from personal apps installed locally.

Learn more about [Endpoint Management Ownership Models](/en-us/tech-zone/learn/tech-briefs/citrix-endpoint-management.html#unified-endpoint-management-ownership-models)

Enrollment of the devices uses the following enrollment policies

|Operating System|Policy|Value|
---|---|---|
|Windows|Device Management|Fully Managed|
||Allow manual MDM unenrollment|Disabled|
|iOS|Device Management|Apple Device enrollment|
||Citrix MAM|Enabled|
|Android|Management|Android Enterprise|
||Device owner mode|Fully managed with work profile|
||BYOD work profile|Disabled|
||Citrix MAM|Enabled|

### Endpoint Security Policies

To secure the endpoints, CompanyA is starting with the following baseline device policies:

|Policy|Value|Endpoints|
---|---|---|
|Passcode: Minimum length|6|All OS types (iOS, macOS, Android, Android Enterprise, Windows)|
|App Inventory|Enabled|All OS types (iOS, macOS, Android, Android Enterprise, Windows)|
|Device Encryption|Require device to be encrypted|Windows – BitLocker|
|||macOS – FileVault|
|Secure Mail|App deployment policy|iOS and Android|
|Secure Web|App deployment policy|iOS and Android|
|OS Update|Automatically install and notify to schedule reboot|Windows, macOS, iOS, Android|

### Microsoft Teams Optimization

With a distributed workforce, CompanyA relies more heavily on virtual conferencing solutions and standardized on Microsoft Teams.  By optimizing the way Microsoft Teams voice and video communication packets cross the wire, Citrix Virtual Apps and Desktops delivers a virtual meeting experience identical to that of a traditional PC.

To learn more about Microsoft Teams integration and optimization, review the following:

*  [Microsoft Teams Tech Insight Video](/en-us/tech-zone/learn/tech-insights/hdx.html#microsoft-teams-optimization)
*  [Microsoft Teams Proof of Concept Guide for Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html)

## Access Layer

### Authentication

Due to security concerns, CompanyA requires a strong authentication policy. CompanyA uses a 2-staged approach.

Stage 1 is focused on securing the user’s primary identity into Citrix Workspace with a contextual, multi-factor approach.

[![Primary Authentication](/en-us/tech-zone/design/media/reference-architectures_flexible-work_primary-authentication.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_primary-authentication.png)

The authentication policy denies access if the device does not pass an endpoint security scan. The scan verifies the device is managed and secured with corporate security policies. Once the scan succeeds, the user is able to use their Active Directory credentials and a TOTP token to authenticate.  The TOTP token can either be entered manually or automatically with push notifications.

The stage 2 authentication scheme focuses on the secondary resources (SaaS apps, web apps, virtual apps and desktops). Almost every secondary resource requires authentication. Some use the same identity provider as the user’s primary identity, while others use an independent identity provider, most common with SaaS apps.

*  SaaS Apps: For SaaS applications, CompanyA uses SAML-based authentication with Citrix Workspace acting as an identity broker for Active Directory. Once configured, the SaaS applications only allow SAML-based authentication. Any attempt to log on with a username/password specific to the SaaS app will fail. This policy allows CompanyA to improve the strength of the authentication while making it easier to disable access due to a compromised user account.
*  Web Apps: The inventory of web applications in CompanyA all use the user’s Active Directory credentials. For web applications, CompanyA uses a combination of forms, Kerberos, and SAML-based authentication to provide single sign-on. The choice between the options is based on the unique aspects of each Web application.
*  Virtual Apps/Desktops: For the virtual apps and desktops, CompanyA uses pass-through authentication from Citrix Workspace, eliminating the secondary authentication challenge.

The [Workspace Single Sign-On Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html) contains additional information regarding single sign-on for SaaS, web, virtual apps, virtual desktops, and IdP chaining options.

### Resource Access

From a security perspective, all users are considered external. CompanyA needs to consider how external users are able to access internal resources. Internal, corporate resources must be protected from untrusted and unsecured locations. To help prevent malware intrusion, devices are not be allowed direct access to the internal network.

To provide access to internal resources like private web apps, virtual apps, and virtual desktops, CompanyA plans to use the Secure Workspace Access service and the Virtual Apps and Desktops service. These two services utilize a zero trust network access solution, which is a more secure alternative to traditional VPNs.

[![Zero Trust Network Access](/en-us/tech-zone/design/media/reference-architectures_flexible-work_zero-trust-network-access.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_zero-trust-network-access.png)

The Secure Workspace Service and the Virtual Apps and Desktops Service uses the outbound control channel connections established by the cloud connectors. Those connections allow the user to remotely access internal resources. However, those connections are

*  Limited in scope so that only the defined resource is accessible
*  Based on the user’s primary, secured identity
*  Only for specific protocols, which disallow network traversal

However, when users are working from a local office, in what is considered local access, the routing for virtual apps and desktops is optimized without compromising security policies. To do this, CompanyA implements the direct workload connection capability within the Citrix Virtual Apps and Desktops service.

[![Direct Workload Connection](/en-us/tech-zone/design/media/reference-architectures_flexible-work_direct-workload-connection.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_direct-workload-connection.png)

When a user is remote, Citrix Workspace has the Gateway Service establish a secure connection between the virtual resource and the remote user. However, when the user in physically located on the local network, following the same flow adds latency to the user’s connection. If the user is able to make a local connection to the virtual resource, latency decreases and the user experience increases.

Direct Workload Connection allows organizations to define a set of public IP addresses associated with the local sites. If the user’s IP address originates from one of the defined IP addresses, Workspace determines that the user is a local user and uses a direct connection procedure.

## Resource Layer

### Resource Security Policies

CompanyA wants to limit the risk of data loss due to theft of an endpoint device or a compromised endpoint device. Within the different application types, CompanyA incorporates numerous restrictions to prevent users from copying, downloading, or printing data.

As a baseline policy, CompanyA defined the following (with the ability to relax policies as needed based on user and application).

|Category|SaaS Apps|Web Apps|Virtual Apps/Desktops|Mobile Apps|
---|---|---|---|---|
|Clipboard access|Denied|Denied|Client to Server only|Only enabled between protected apps|
|Printing|Denied|Denied|Denied|Denied|
|Navigation|Denied|Denied|Not Applicable|Not Applicable|
|Downloads|Denied|Denied|Denied|Denied|
|Watermark|Enabled|Enabled|Enabled|Not Applicable|
|Keylogging Prevention|Enabled|Enabled|Enabled|Not Applicable|
|Screenshot Prevention|Enabled|Enabled|Enabled|Not Applicable|

The [App Protection Policies Tech Brief](/en-us/tech-zone/learn/tech-briefs/app-protection-policies.html) contains additional information regarding the keylogging and screenshot prevention policies.

## Control Layer

The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  Even within a protected Workspace resource, users have the ability to interact with untrusted resources on the Internet.CompanyA utilizes Secure Internet Access to protect the user from external threats from within SaaS apps, web apps, virtual apps, mobile apps, and apps on endpoint devices.
*  Users might need to access personal items on the managed endpoint devices. Appropriate policies are defined to protect the user’s privacy when accessing personal sites related to health and finance.
*  With all of the policies in place to protect the users when working in a flexible environment, there are still risks. CompanyA uses the Security Analytics service to identified compromised users and automatically take actions to maintain a secure environment.
*  CompanyA must be able to manage the remote SD-WAN 110 devices, deployed in numerous households. By utilizing SD-WAN Orchestrator, CompanyA can centrally managed a distributed deployment.

### Secure Internet Access

As users interact with SaaS, web, virtual, local, and mobile apps, they are often accessing public internet sites. Although CompanyA has an Internet Security Compliance class all users must complete on a yearly basis, it has not completely prevented attacks, most often originating through phishing scams.

To help protect the users and the organization, CompanyA incorporates the Secure Internet Access service and Security Analytics into the flexible work design.

[![Internet Security](/en-us/tech-zone/design/media/reference-architectures_flexible-work_internet-security.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_internet-security.png)

Any internet traffic to/from the library of apps, desktops, and devices within the organization are routed through the Secure Internet Access service. Within this service, any URL is scanned to verify it is safe. Functionalities within certain public sites are denied or modified. Downloads are automatically scanned and verified.

As many websites are now encrypted, part of this security process is to decrypt the traffic and inspect. CompanyA wants to allow users with the flexibility to access personal sites while on company-owned devices. To ensure employee privacy, certain categories of websites will not be decrypted, like financial and health related sites. To have complete transparency, CompanyA plans to make the overall security policy plan available internally.

In designing the internet security policy, CompanyA wanted to start out with a baseline policy. As CompanyA continues to assess risks within the organization, they will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA made the following modifications that are applied globally:

|Category|Change|Reason|
---|---|---|
|Financial and Investment|Do not decrypt|Employee privacy concerns|
|Health|Do not decrypt|Employee privacy concerns|
|Adult Content|Block|
|Drugs|Block|
|File Sharing|Block|
|Gambling|Block|
|Illegal Activity|Block|
|Malicious Sources|Block|
|Malware Content|Block|
|Porn/Nudity|Block|
|Virus & Malware|Block|
|Violence/Hate|Block|

In addition to global security controls, CompanyA also has a need to implement additional internet controls for a subset of the environment.

|Category|Group|Reason|
---|---|---|
|YouTube – Restrict HD Video|Virtual Apps and Desktops hosts|Allowing HD video within virtual desktops often results in an increased resource load, potentially impacting performance and scalability.|

### Security Analytics

CompanyA needs to identify and stop threats to the environment before the impact is too great.

To help protect the environment, CompanyA uses Citrix Security Analytics to identity insider threats, compromised users, and compromised endpoints. In many cases, a single instance of a threat does not warrant drastic action, but a series of threats can indicate a security breach.

CompanyA developed the following initial security policies:

|Name|Conditions|Action|Reason|
---|---|---|---|
|Jailbroken device|Jailbroken/rooted device detected|Lock Device|Mobile devices are owned and managed by CompanyA. Jailbroken devices can introduce malware, capable of stealing data.
|Unmanaged endpoint|Unmanaged device detected|Notify admin|CompanyA requires every device be managed. If an unmanaged device is detected, there is a possibility the environment was breached and the user’s account must be disabled.|
|Endpoint risk|EPA Scan Failure|Add to watchlist|Only managed devices are allowed access. If a user tries to use an unmanaged device, the user is monitored to verify they are not a threat. If additional risks arise, additional actions are warranted.|
|Unusual access|Log on from suspicious IP and access from an unusual location|Lock user|If a user logs in from an unusual location and a suspicious IP, there is a strong indication the user was compromised.|
|Unusual app behavior|Unusual time of app usage and access from unusual location|Start session recording|If a user accesses a virtual app at a strange time and location, there is the potential the user is compromised. Security analytics records the session to have the admin verify legitimacy.|
|Potential credential exploits|Excessive authentication failures and access from an unusual location|Add to watchlist|If a user has many authentication failures from an unusual location, it can indicate someone is trying to break into the system. However, the attacker has yet to succeed. Only need to add the user to the watchlist.|

The [Citrix User Risk Indicators](/en-us/security-analytics/risk-indicators.html) document contains additional information regarding the various risk indicators provided to Citrix Security Analytics.

The [Citrix Policies and Actions](/en-us/security-analytics/policies-and-actions.html) page contains information about the remediation steps Citrix Security Analytics can perform.

### Citrix SD-WAN Orchestrator

A subset of the user population, based on requirements, will deploy the Citrix SD-WAN 110 appliance within their home network. This appliance will either provide redundant network connections or create a separate, secure home network.

To manage management easier for the distributed SD-WAN deployment, CompanyA will utilize the Citrix SD-WAN Orchestrator.

[![SD-WAN Orchestrator](/en-us/tech-zone/design/media/reference-architectures_flexible-work_sd-wan-orchestrator.png)](/en-us/tech-zone/design/media/reference-architectures_flexible-work_sd-wan-orchestrator.png)

With Orchestrator, the administrator makes all necessary configuration settings within the cloud service. Once connected to the network, the SD-WAN 110 appliances registers with the cloud service and receives the configuration information.

The cloud-based service makes managing the distributed SD-WAN environment easier for the administrator.

## Sources

To make it easier for you to plan a flexible work solution, we would like to provide you with [source diagrams](https://citrix.sharefile.com/d-sa8ea1c024deb43109baca85856907c64) that you can adapt.
