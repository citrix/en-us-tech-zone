---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
description: Zero Trust is the most important End User Computing movement since Mobile and Cloud. The Citrix Zero Trust Architecture enables the "any-any-any" vision that Citrix has been espousing for years and is secured by access policies that take trust into context.
tz_title: Zero Trust
tz_products: security;
---
# Tech Brief: Zero Trust

## Overview

  >**Note:**
  >
  >New to Zero Trust? Learn [what is Zero Trust Security](https://www.citrix.com/glossary/what-is-zero-trust-security.html) and about [Citrix solution for Zero Trust](https://www.citrix.com/digital-workspace/zero-trust.html).

Traditional, perimeter-based approaches to protecting information systems and data are inadequate.

For many years, the cybersecurity paradigm has emulated a familiar form of physical security from the middle ages: the castle and the moat. It’s an approach many enterprises have adopted and are finding insufficient in the world of cloud.

In this classic defense model, multiple layers of protection with tightly secured checkpoints and gateways surround and protect the crown jewels. All access is controlled and verified at the gateway where authentication and authorization are granted. However, once verified, people are effectively given free rein of the environment.

The model has shortcomings, mainly with granting excessive access from inadequate authentication and authorization. Attackers often use subterfuge to bypass the gateways and superior tools designed to break down the walls.

Likewise, in cybersecurity, valuable data is surrounded by multiple layers of firewalls, segmentation, authentication, and authorization. And while these components are necessary, they are insufficient due to the “deperimeterization” caused by the move to cloud and mobile.

A problem with this security model is the implied or implicit trust which is granted to persons or services within the walls of the network. The location or network on which a device or user resides must never implicitly grant a level of trust worthiness. The assumption must be made that neither the private network nor any device on it are inherently trustworthy.

How do organizations secure the crown jewels with a complex model incorporating the following variables?

*  Users: Located in corporate offices and public locations
*  Devices: Mobile, Bring Your Own Device (BYOD), Choose Your Own Device (CYOD), and Corporate Own Personally Enabled (COPE)
*  Applications: (Intranet/SaaS, browser, virtualized, mobile)
*  Data access and storage (on-prem and cloud)

Given all these facets of access, relying on a moat as the security perimeter becomes a liability.

VPNs have long been the traditional way to access enterprise applications and data when users are outside of corporate locations. This model has worked for use cases where end users gain access to the corporate network only from approved corporate-managed devices.

But this is why the VPN model doesn’t adequately meet the needs of the evolving use cases. Apps have been modernized for web-based access and deployed in multi-cloud environments. Apps, data, and services are not just inside the walls of the data center. Locations have moved, become dynamic, users are accessing resources everywhere, and organizations need a framework that allows easy access to all resources without slowing down productivity. Forcing user access through a corporate VPN en route to a cloud app falls short on end-user experience.

The requirement for a new strategy has been accelerated with the need for remote workers. How does an organization build a wall around a resource that exists in multiple locations simultaneously?

## Guiding Principles

Zero Trust works based on assuming “never trust and always verify” or an innate distrust ("default deny”). John Kindervag, coined the term “Zero Trust” as a way to solve the compounded problem of “deperimeterization” – the expansion and dissolving of the perimeter as it becomes more porous.

The purpose of a Zero Trust Architecture is to protect data. It is not a single network architecture but a set of guiding principles in network infrastructure design and operation. The Zero Trust approach provides a consistent security strategy for accessing data that resides anywhere, from anywhere, from any device and in any way.

With Zero Trust, there is no implicit trust granted to systems based on their physical or network location. The approach requires continuous authorization no matter what the originating request location; and increases visibility and analytics across the network.

Zero trust is achieved through an intentional implementation of the framework. Using a collection of products that are integrated and have Zero Trust principles built in provides a collective approach to achieve the desired business outcomes.

Citrix sees Zero Trust as a strategy that applies not only just to networking, but across the organization in users, devices, networks, applications – and how people work.

Citrix Zero Trust Architecture focuses on protecting resources and is designed and deployed adhering to tenets modeled after the National Institute of Standards and Technology (NIST) [Zero Trust tenets](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-207.pdf):

1.  All data sources and computing services are considered resources
2.  All communication is secured regardless of network location because every network, both enterprise and remote, is innately hostile and not trustworthy
3.  Access to individual enterprise resources is granted on a per-session basis
4.  Access to resources is enforced by dynamic policy that includes the observable state of identity, device, application, network and can include behavioral attributes
5.  As no device is inherently trusted, the enterprise monitors assets to ensure that they remain in the most secure state possible
6.  All resource authentication and authorization are dynamic and strictly enforced before access is allowed
7.  The enterprise collects as much information as possible about the current state of network infrastructure and communications. It uses the data to improve its security posture

## Pillars of Zero Trust

The journey from enterprise trust to Zero Trust requires a strategy for building a trust fabric. Organizations must identify current concerns and define their trust fabric to support digital transformation. Access must be aligned with the sensitivity of data and the situation in which the data is being requested and used. Zero Trust must granularly identify people, devices, data, and workloads on the network.

Citrix calls this Contextual Access. Access policies scrutinize trust elements across the 5 W’s of Access to grant specific usage entitlements. Contextual Access is a continuous process. It extends from the request event through specific data usage entitlements and dynamic policies that govern the data security lifecycle.

*  What data must be protected?
*  Where are requests for data coming from and where does it sit in the network?
*  Who has access to the data and for how long?
*  Why do these people need access privileges?
*  When do they need access to the data?

### People

Ongoing authentication and authorization of trusted users is paramount to Zero Trust. Identity is the “Who” that is requesting access to a resource. User authentication is dynamic and strictly enforced before access is allowed. It is a constant cycle of access:

*  Scanning and assessing threats
*  Adapting
*  Continuously authenticating
*  Monitoring
*  Validating user trustworthiness

Identity encompasses the use of technologies like Identity, Credential, and Access Management. Identity is the set of users of and attributes developed by the enterprise. The users and attributes form the basis for policies for resource access. User identities can include a mix of logical identity, biometric data, and behavior characteristics. Use identity attributes such as time and geolocation to derive trust scores to dynamically assess the risk and adjust access appropriately.

Authentication (both user and device) is performed before establishing a connection and access to resources is minimized to only those end users who are validated. People are continuously authenticated to determine the identity and security posture of each access request. After these requirements are met, access to data resources is granted only when the resource is required.

### Devices

Organizations need a consistent way of managing policies across devices and ownership models. Administrators need to be able to determine what level of confidence they can have in the device. Devices are one part of the “Where” question. Where is the request coming from and what is the device risk level? Real-time security posture and trustworthiness of devices are foundational attributes of the Zero Trust approach.

Traditionally, when looking at endpoint devices, there are three popular ownership models (corporate owned, BYOD/CYOD, COPE), each with differing inherent trust, trust factors and need for validation. With Zero Trust, that inherent trust is eliminated, and every device, regardless of ownership requires validation.

### Network

A Zero Trust Architecture focuses on protecting resources, not network segments. The network location of the user, device, or resource is no longer seen as the prime component of the security posture. However, the ability to segment, isolate, and control the network remains a pillar of security and essential for a Zero Trust network. And the ability to select the appropriate app and workspace delivery model is the second part of the “Where” question.

Zero Trust networks are sometimes described as “perimeter-less”. Some argue that perimeter protections are becoming less important for networks and operations. In reality, the perimeter is still there, but in a much more granular way. Zero Trust networks actually attempt to move perimeters in from the network edge and create segments to isolate critical data from other data. The perimeter must move closer to the data to strengthen protections and controls. Hence the reason why the traditional castle and moat approach is not sufficient.

During transitions internet-based technologies, it’s critical to consider how to:

1.  Control privileged network access
2.  Manage internal and external data flows
3.  Prevent lateral movement in the network
4.  Make dynamic policy and trust decision on network and data traffic

### Workload

Securing and properly managing the app layer and compute containers and virtual machines is central to Zero Trust adoption. Being able to identify and control the technology stack facilitates more granular and accurate access decisions. Unsurprisingly, MFA is an increasingly critical part of providing proper access control to apps in Zero Trust environments.

An important aspect of the Zero Trust model is granting access only to specific the apps required for end users to do their job. There’s no access provided into the network itself, significantly improving the organization’s security posture by reducing the attack surface.

### Data

Customers, employees, and partners are increasingly mobile, and consume apps and data other resources from every network to which they connect. When someone wants to access data, a Zero Trust model weighs the value of data against the assurance that the correct person is there and authorized to access the data while using a secure device.

The minimal requirements for access to the resource can include authenticator assurance levels, such as MFA and or requests for system configuration. Whether that person is inside or outside the corporate network is not a reliable indicator of any of those three assurances. However, flag obvious unusual or unsanctioned network locations such as deny access from overseas IP addresses or within unexpected time frames.

### Visibility, Analytics, and Orchestration

Zero Trust Architectures require increased access visibility. It is incomplete without tools like security information management, advanced security analytics platforms, and security user behavior analytics. These systems are continuously monitoring and logging access requests and policy changes over time. Security experts need these tools to observe in real time what is happening and orient defenses more intelligently.

Analytics is the system that is responsible for enabling, monitoring, and eventually terminating connections between a subject and an enterprise resource. The analytics policy engine is responsible for the ultimate decision to grant access to a resource for a given client or subject. It can use enterprise policy and input from external sources.

Analytics data can either be analyzed separately or combined with other security monitoring and logging data sets. Multiple services take data from multiple external sources and provide information about newly discovered attacks or vulnerabilities. The data includes DNS block lists, discovered malware, or command and control systems that the policy engine wants to deny access to from enterprise systems. By using threat intelligence feeds, the policy engine can help develop proactive security measures before an actual incident occurs. Analytics can use criteria-based scoring that assumes a set of qualified attributes that must be met before access is granted to a resource.

## Citrix Zero Trust Architecture

In moving from a boundary, perimeter-based security model to a resource based one, Citrix Workspace uses a holistic context-aware VPN-less approach.

![Zero Trust Diagram](/en-us/tech-zone/learn/media/tech-briefs_zero-trust_diagram4.png)

Citrix Workspace acts as the enforcement point to control access to applications and data. Access starts with a “default deny” instead of building on inherent trust. Access is only granted after verifying an entity by user and device credentials and other factors, including time, location, and device posture. Citrix Zero Trust mitigates the complexity around these factors by removing assumed trust and confirming it every step of the way.

### Citrix Endpoint Management

Citrix Endpoint Management provides data to assess device trust. It helps answer the question the device risk/trust level through continuous evaluation of device posture to support authorization. Pre and post-authentication device evaluation supports the authorization required to perform work securely. Also, more assessments are conducted for every access request. Citrix Endpoint Management examines whether the device has been compromised, its software version, protection status, and encryption enablement. Citrix Endpoint Management can be used to perform Endpoint Analysis scans to check for platform certificates and includes features for both domain-joined and non-domain-joined devices. Differing levels of trust can be assigned to full device management, app, or access from a browser with all three having different requirements.

### Citrix Gateway

Citrix Gateway provides an enhanced and flexible approach to security. IT is enabled to configure multiple authentication steps to access confidential data based on user role, location, device state, and more. Citrix Gateway determines the authentication mechanism to be used for each session. It uses factors like a user’s location and the risk profile of the user or device.

Citrix’s identity approach allows enterprises to preserve their investments. It allows them to use the native IdP security capabilities like MFA, biometrics to protect the user within the Workspace. It supports LDAP, RADIUS, TACACS, Diameter, and SAML2.0 authentication mechanisms, among others.

Citrix Gateway provides SmartAccess and SmartControl policies that provide flexibility to balance user convenience with risk. Based on the result of a SmartAccess scan, a user can be granted full access, reduced access, quarantine, or no access at all. For example, a user who fails a device compliance check can get access to a reduced set of applications. Sensitive applications can have restricted functionality like blocking printing and downloading. SmartControl centralizes policy management on Citrix Gateway, strengthening access control at the network layer before the user reaches the back-end resource.

### Citrix Secure Workspace Access

Citrix Workspace offers an integrated approach to secure access to the internet. In addition to managing user devices, Secure Workspace Access focuses on protecting a user’s workspace on both managed and unmanaged BYO devices. User information is always protected, whether accessing allow list or block list URLs or URL categories.

Citrix Secure Workspace Access offers a URL filtering engine and an integrated browser isolation service. Together, they give an admin the choice to completely block a URL or access any URL in a sandbox environment. Also, admins can take a cautious approach even for accessing allow list URLs. This approach ensures users get access to the information they need. It doesn’t impact on productivity while providing protection against any unforeseen threats or malicious content delivered from the internet.

A traditional URL filtering engine that assumes trust for a allow list URL. Secure Workspace Access does not implicitly trust an allow list URL since webpages, deemed safe by URL filtering engines, that can host malicious links. With Secure Workspace Access URLs within the trusted URL are also tested.

Secure Workspace Access also addresses “gray status URLs” by offering the isolated browser. Users can securely access sites that fall between URLs blacklisted or whitelisted. Secure Workspace Access doesn’t require a device to be managed making it ideal for BYO type environments. Most URL filtering engines push a PAC file on the end user’s device that is either managed or must be connected to a domain.

### Citrix Content Collaboration

Citrix Content Collaboration is a cloud-based Software-as-a-Service (SaaS) solution that enables the secure exchange of confidential business files. Content Collaboration safeguards both files in transit and at rest.

Content Collaboration employs TLS security protocols to protect authentication, authorization, and file transfers. Files are encrypted in transit with up to 256-bit encryption. A keyed-hashed message authentication code (HMAC) is employed to authenticate and ensure the integrity of intra-system communications.

Citrix Content Collaboration provides admins configurable controls. Admins protect corporate documents with access controls, audit logs, account lockout, and session timeout thresholds. With Citrix Security Analytics, customers can detect anomalies in file related activities, identify breaches, and take appropriate actions.

### Citrix Analytics

Citrix Security Analytics and its continuous monitoring and assessment of risk scores supports the Zero Trust philosophy of “never trust, always verify”. Before providing access to resources, the appropriate level of risk /security posture is calculated.

Citrix Security Analytics aggregates events from all Citrix services. Risk indicators from third party security solutions like Microsoft Security Graph are ingested to issue user risk scores. Citrix Security Analytics baselines, helps visualize, and maps trust relationships. It correlates events and activities to identify anomalies and provide insights by user, group, and app.

Citrix Security Analytics also offers continuous monitoring and insights into website access. Actions monitored include visiting malicious, dangerous, or unknown websites, bandwidth consumed, risky download, and upload activity. If a user is downloading excessive amounts of data, an action can be triggered to request a response from the user to validate their identity. Based on the user reply, a secondary action can be triggered.

Rules are configured to trigger specific actions on user accounts by continuously assessing risk score thresholds. For example, sessions authenticated into Citrix Workspace can be logged off on a change in the risk score.

Security admins can re-enable access once user risk levels are lower than acceptable levels. These capabilities are triggered based on contextual factors during the time of logon or on a continuous basis based on triggers from Citrix Security Analytics. It continuously monitors events and risk indicators from Citrix services and third-party security solutions such as Azure AD protection from Microsoft.

![Zero Trust Functions](/en-us/tech-zone/learn/media/tech-briefs_zero-trust_diagram3.png)

## Conclusion

Traditional security models assume all data and transactions are trusted based on assumptions. Incidents such as compromises, loss of data, malicious actors, or uncharacteristic user behavior would degrade that trust. Zero Trust flips the trust posture by assuming all data and transactions are untrusted from the outset.

For organizations, the promise of Zero Trust and mitigating excessive access has been a goal for many years. But its implementation has been elusive. All necessary elements have been inordinately difficult to construct and manage as an end-to-end security solution. These include multifactor authentication (MFA), dynamic identity management, endpoint analysis, encryption, information rights management (IRM), application-specific networking, and data usage policies.

Using Citrix Workspace, customers don’t need to deploy third-party products for SSO, MFA, SSL VPN, web proxies, and browser isolation. With Workspace, VPN-less access to sensitive resources is secured not only by providing access policies at the time of authentication, but throughout the session. Access is secured across all types of applications and resources deployed anywhere on a cloud, on-premises, or in a hybrid deployment model.
