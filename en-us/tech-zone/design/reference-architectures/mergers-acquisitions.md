---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Learn how to design an environment to support a mergers and acquisition strategy without compromising IT security. The reference architecture incorporates Citrix Workspace, Secure Workspace Access, Virtual Apps and Desktops, Application Delivery Controller, Federated Authentication Service and Security Analytics.
---
# Mergers and Acquisitions

## Overview

CompanyA is a food manufacturer located in the northern plains of the United States. To continue growing and expand into new types of food, CompanyA plans to acquire additional food manufacturers across different climates. As part of the acquisition process, CompanyA needs a repeatable strategy to integrate acquired company systems into a single, unified experience.

The integration across multiple independent organizations introduces many technical challenges. The challenges mostly focused on granting access to external users (CompanyA users) to private resources (CompanyB applications) that are authorized by separate, independent identity providers.

CompanyA decided to use Citrix Workspace as the cornerstone for their mergers & acquisition strategy. They are looking to use the same strategy for acquiring CompanyC, CompanyD and CompanyE.

## Success Criteria

As part of the acquisition strategy, CompanyA needs a solution that can quickly and securely allow users access to CompanyA and CompanyB resources. To be successful, CompanyA defined a list of success criteria that forms the basis for the overarching design.

### User Experience

The first aspect of a mergers and acquisitions solution is to meet the needs of the user. CompanyA identified the following user-related criteria for a successful design.

| **Success Criteria** | **Description** | **Solution**
---|---|---|
|**Application Library**|Users from CompanyA and CompanyB need a centralized way to access resources from the other company.|Citrix Workspace|
|**Web App Single Sign-On**|When accessing private web resources from the other company, users are not  required to remember and enter another user accounts or passwords.|Citrix Secure Workspace Access service|
|**Virtual App Single Sign-On**|When accessing virtual Windows apps from the other company, users are not  required to remember and enter another user accounts or passwords.|Citrix Virtual Apps and Desktops service – Federated Authentications Service|
|**Unified Experience**|Regardless of the user’s original company, all users have the same authentication experience.|Citrix Application Delivery Controller – nFactor authentication policies|

### Security

The first aspect of a mergers and acquisitions solution is to meet the needs of the user. CompanyA identified the following user-related criteria for a successful design.

| **Success Criteria** | **Description** | **Solution**
---|---|---|
|**Identity Providers**|Each acquired organization maintains a separate identity provider until such a time when it can be integrated with CompanyA’s primary identity provider.|Citrix Application Delivery Controller|
|**Multifactor authentication**|With security being top of mind, MFA is required to ensure another layer of authentication protection of corporate resources.|Integrate currently deployed solution or require Time-Based One-Time Password with Push|
|**VPN-less Access**|Corporate resources must be protected from untrusted and unsecured locations. To help prevent malware intrusion, devices are not be allowed direct access to the internal network. |Citrix Secure Workspace Access aervice and Citrix Virtual Apps and Desktops service|
|**Internal Threats**|There are documented cases where internal users who are unhappy with the acquisition steal customer data and intellectual property. Capturing and storing data must be restricted|Enhanced Security Policies, App Protection Policies, and Security Analytics|
|**External Threats**|To handle multi-directory authentication, the Citrix Application Delivery Controller presents Workspace with an authentication web app. CompanyA must add extra layers of protection for public facing web apps.|Citrix Application Delivery Controller with Bot Management and Web App Firewall|

## Conceptual Architecture

Based on their defined requirements, CompanyA created the following high-level, conceptual architecture for their acquisition strategy. The conceptual architecture not only meets all of the requirements, but it gives CompanyA the foundation they need to expand to additional use cases as identified in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_conceptual.png)](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_conceptual.png)

The architecture framework is divided into multiple layers. The framework provides a foundation for understanding the technical architecture for the mergers and acquisitions scenario. All layers flow together to create a complete, end-to-end solution.

At a high-level:

**User Layer**: The user layer describes the end-user environment and end-point devices that are used to connect to resources.

*  Regardless of device, users access resources from Workspace app, resulting in an experience that is identical across every form factor and device platform.

**Access Layer**: The access layer describes details surrounding how users authenticate to their Workspace and secondary resources.

*  Users continue to authenticate with their pre-acquisition primary identity.
*  Users continue to use their pre-acquisition multifactor authentication solution. If the company does not currently utilize multifactor authentication, CompanyA provides phone-based, TOTP tokens that support push-based authentication.

**Resource Layer**: The resource layer authorizes specific SaaS, web and virtual resources for defined users and groups and the security policies associated with the resource.

*  Regardless of a user’s originating company, the user must be allowed seamless access to any authorized resource hosted by other companies.
*  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the endpoint. CompanyA also requires restricting screen scraping\capturing applications and keylogging malware.
*  Due to the unknown nature of the endpoint security status, CompanyA requires VPN-less access to resources with the use of isolated browsers or virtualized sessions.

**Control Layer**: The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

*  With the policies in place to protect the users and company data, there are still risks. CompanyA uses the Security Analytics service to identify compromised users or insider threats and automatically take actions to maintain a secure environment.
*  The platform unifying multi-directory authentication must be secured from external threats and attacks. CompanyA enables the integrated Web App Firewall and Bot Management to protect the authentication point into the environment.

Hosting Layer: The hosting layer details how components are deployed on hardware, whether that is on-premises, cloud, or hybrid cloud.

*  Citrix Workspace must be able to access every company’s identity provider, whether that is an on-premises Active Directory domain or a cloud-based offering from Okta, via a single Workspace site.

The subsequent sections provide greater detail into specific design decisions for CompanyA’s mergers and acquisitions strategy reference architecture.

## Access Layer

### Authentication

One of the challenges CompanyA experienced with previous acquisitions was how to integrate identity providers. The process of merging identity providers can take a significant amount of time. With the new strategy, CompanyA utilizes a Citrix Application Delivery Controller to handle all authentication requests.

[![Primary Authentication](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_primary-authentication.png)](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_primary-authentication.png)

The authentication process works by the Application Delivery Controller evaluating the user’s company and then applying the correct authentication request. As CompanyA has standardized on Okta for primary authentication, the request is forwarded to Okta. Once Okta completes user authentication, Okta replies to the Application Delivery Controller with a token identifying successful authentication.

But not every company that CompanyA acquires uses Okta. Instead, if the Application Delivery Controller identifies the user is from CompanyB, the user is asked for their Active Directory user name and password. Those credentials are validated against CompanyB’s Active Directory domain. If CompanyB already integrated a multifactor authentication solution, users continue to user their registered tokens.

Because strong authentication is a critical first step to security, CompanyA wants to have a solution ready for organizations that are not currently using multifactor authentication. In this instance, after the user authenticates with their company’s Active Directory domain, the Application Delivery Controller uses the native time-based one-time password engine to provide multifactor authentication for the user. Once registered to the user’s device, the user can either enter the code manually or use the push notification service. Push notifications simply require the user to select “Yes” from their registered mobile device to fulfill multifactor authentication.

### nFactor Policy

For primary authentication, the Citrix Application Delivery Controller plays a critical part. To make the authentication decisions, the Application Delivery Controller utilizes an nFactor policy.

[![nFactor Policy](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_nfactor-policy.png)](/en-us/tech-zone/design/media/reference-architectures_mergers--acquisitions_nfactor-policy.png)

To process authentication requests properly, the nFactor policy must know what company the user belongs to. Once the correct company identifier is selected, nFactor forwards the request to the correct branch of the authentication policy.

Once the nFactor policy is defined, CompanyA can continue to expand it to incorporate additional organizations it acquires in the future. The nFactor policy allows CompanyA to create additional flows utilizing authentication standards that include LDAP, RADIUS, SAML, client certificates, OAuth OpenID Connect, Kerberos, and more. The nFactor policy engine provides CompanyA with the flexibility to continue integrating additional acquisitions without a redesign.

### Zero Trust Network Access

To provide access to internal resources like private web apps, virtual apps, and virtual desktops, CompanyA plans to use the Secure Workspace Access service and the Virtual Apps and Desktops service. These two services utilize a zero trust network access solution, which is a more secure alternative to traditional VPNs.

[![Zero Trust Network Access](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_zero-trust-network-access.png)](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_zero-trust-network-access.png)

The Secure Workspace Access service and the Virtual Apps and Desktops service uses the outbound control channel connections established by the cloud connectors. Those connections allow the user to remotely access internal resources. However, those connections are

*  Limited in scope so that only the defined resource is accessible
*  Based on the user’s primary, secured identity
*  Only for specific protocols, which disallow network traversal

## Resource Layer

### Federated Authentication Services

Users authenticate to Citrix Workspace with a primary identity. The primary identity is based on their company’s identity provider. One of the challenges with mergers and acquisitions is access to secondary resources that are based on a secondary identity. For example, CompanyA allows certain users from CompanyB and CompanyC to access virtual Windows applications. To access a virtual Windows application, the user must have a user account (secondary identity) within the domain containing the virtual resource. A CompanyB user’s account (primary identity) will not authenticate to a CompanyA resource (secondary identity). To translate credentials between a primary and secondary identity and provide single sign-on to virtual Windows applications, CompanyA uses the Federated Authentication Service within Citrix Cloud.

The [Workspace Single Sign-On Tech Brief](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-sso.html#sso-virtual-apps-and-desktops) contains additional information related to the Federated Authentication Service.

### Resource Security Policies

CompanyA wants to limit the risk of data loss due to an insider threat. Within the different application types, CompanyA incorporates numerous restrictions to prevent users from copying, downloading, or printing data.

As a baseline policy, CompanyA defined the following (with the ability to relax policies as needed based on user and application).

|Category|SaaS Apps|Web Apps|Virtual Apps/Desktops|
---|---|---|---|
|Clipboard access|Denied|Denied|Client to Server only|
|Printing|Denied|Denied|Denied|
|Navigation|Denied|Denied|Not Applicable|
|Downloads|Denied|Denied|Denied|
|Watermark|Enabled|Enabled|Enabled|
|Keylogging Prevention|Enabled|Enabled|Enabled|
|Screenshot Prevention|Enabled|Enabled|Enabled|

The [App Protection Policies Tech Brief](/en-us/tech-zone/learn/tech-briefs/app-protection-policies.html) contains additional information regarding the keylogging and screenshot prevention policies.

## Control Layer

### Web App Firewall

When users authenticate to Citrix Workspace, they access a custom authentication form that supports the mergers and acquisitions strategy. The authentication form, hosted on the Citrix Application Delivery Controller, is a public webpage that must be protected from bots and attacks. To better protect the public web app, CompanyA uses the Bot Management and Web App Firewall components of the Citrix Application Delivery Controller solution.

[![Web App Firewall](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_web-app-firewall.png)](/en-us/tech-zone/design/media/reference-architectures_mergers-acquisitions_web-app-firewall.png)

The first line of defense is Bot Management. Bots can easily crash or slow a public web app by overwhelming the service with fraudulent requests. The bot management component of the Application Delivery Controller is able to detect a bot request and prevent it from inundating the system.

The second line of defense is the Web App Firewall. With the Web App Firewall, the policy engine that handles the submitted credentials is protected from attack. These types of attacks would typically be buffer overflow, SQL injection, and cross site scripting. Web App Firewall detects and deny these attacks from impacting the authentication policy engine.

### Security Analytics

CompanyA needs to identify and stop insider threats to the environment before the impact is too great.

To help protect the environment, CompanyA uses Citrix Security Analytics to identity insider threats, compromised users and compromised endpoints. In many cases, a single instance of a threat does not warrant drastic action, but a series of threats can indicate a security breach.

CompanyA developed the following initial security policies:

|Name|Conditions|Action|Reason|
---|---|---|---|
|Unusual access|Log on from suspicious IP and access from an unusual location|Lock user|If a user logs in from an unusual location and a suspicious IP, there is a strong indication the user was compromised.|
|Unusual app behavior|Unusual time of app usage and access from unusual location|Start session recording|If a user accesses a virtual app at a strange time and location, there is the potential the user is compromised. Security analytics records the session to have the admin verify legitimacy.|
|Potential credential exploits|Excessive authentication failures and access from an unusual location|Add to watchlist|If a user has many authentication failures from an unusual location, it can indicate someone is trying to break into the system. However, the attacker has yet to succeed. Only need to add the user to the watchlist.|

The [Citrix User Risk Indicators](/en-us/security-analytics/risk-indicators.html) document contains additional information regarding the various risk indicators provided to Citrix Security Analytics.

The [Citrix Policies and Actions](/en-us/security-analytics/policies-and-actions.html) page contains information about the remediation steps Citrix Security Analytics can perform.
