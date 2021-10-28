---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Dan Feller, Manjunatha Gali, Shashidhar Reddy, John Panagulias
description: Learn how to design an environment that uses Desktop-as-a-Service and Chrome OS for Contact Centers. This reference architecture incorporates Citrix Virtual Apps and Desktops service, SD-WAN, Citrix Workspace, Citrix Secure Internet Access, Citrix Endpoint Management and Security Analytics.
tz_title: DaaS for Contact Centers Reference Architectures
tz_products: citrix-analytics;citrix-endpoint-management;citrix-networking;citrix-secure-internet-access;citrix-secure-private-access;citrix-virtual-apps-and-desktops;citrix-workspace
---
# Reference Architecture: DaaS for Contact Centers Reference Architectures

## Overview

Contact Centers are vital when it comes to customer interaction and satisfaction. Often customer service agents are the “face” of a company and represent the only interface between a company and its customers. Contact centers during the pandemic saw average handle, queue, and hold times increasing which made it frustrating for customers. Meanwhile, contact centers face high employee turn-over. Employee turn-over causes an increase in cost as each new agent needs to be set up and trained before being able to work productively. Contact centers need to redefine the agent experience to retain existing employees while attracting new talent. Therefore, the entire contact center environment, including call quality and application performance, can play an important role for the agent in providing the best customer experience and satisfaction.

CompanyA is a contact center company. As COVID-19 occurred, CompanyA became even more vital to its customers. CompanyA knew it had to rethink its IT strategy going forward to reduce operational expenses, reduce the risk of outages and downtime, increase security, and build an environment focused on customer and agent experience that would lead to revenue growth. During COVID-19 over 75% of CompanyA's agents had to work from home. CompanyA wants to continue allowing agents to work from home in some capacity even after restrictions are lifted. They decided this because according to Harvard Business Review, at-home agents can answer 13.5% more calls than their office-bound peers. Also, a flexible WFH strategy allows them to expand their talent pool and retain existing talent, while reducing their overall cost. CompanyA decided to migrate to Citrix Cloud services and standardize on Google Chromebooks to have a reliable, flexible, and secure environment.

This reference architecture explains how CompanyA is planning their environment that allows them to maintain a WFH strategy, scale quickly and easily, and reduce costs.

## Success Criteria

Company A has defined a list of success criteria that formed the basis for the overarching design.

### User Experience

| Success Criteria                                         | Description                                                                                       | Solution                                                                |
|----------------------------------------------------------|---------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------|
| Seamless experience                                      | To reduce user disruption, end users have a similar look and feel                                 | Citrix Workspace                                                        |
| Easy onboarding                                          | New agents must be able to onboard quickly and efficiently without requiring third party assistance | Citrix Workspace + Google Chromebooks                                   |
| Flexibility of remote work                               | Work from anywhere, anytime, and on any device                                                    | Citrix Workspace                                                        |
| Single sign-on                                           | Secure access to all apps (Windows, SaaS, and Web apps) without reauthentication                  | Citrix Virtual Apps and Desktops service |
| Contact Center apps, peripherals, and endpoints supported | Support for the needed contact center applications, endpoints, and peripherals                    | Chromebooks                                                             |
| Optimized end-user experience                            | Equal or better user experience on virtual apps than they do on local apps                        | HDX                                                                     |

### Admin Experience

| Success Criteria                    | Description                                                                          | Solution                                                         |
|-------------------------------------|--------------------------------------------------------------------------------------|------------------------------------------------------------------|
| Expense reduction                   | Standardize on company-owned devices that can be sent to agents at a low cost        | Google Chromebooks                                               |
| Protect from internet-based threats | Increase security to protect their IP                                                | Citrix Secure Internet Access                                    |
| Network resiliency                  | Ensure network resiliency and call quality even while connecting from a home network | Citrix SD-WAN                                                    |
| Network Optimization                | Intelligently optimize and prioritize web traffic                                    | Citrix SD-WAN                                                    |
| Reduce on-premises footprint        | Reduce on-going costs to maintain on-prem environments                               | Citrix Cloud services                                            |
| Zero Trust Network Access           | Remove VPN dependencies to allow agents to work remotely                             | Citrix Virtual Apps and Desktops service |
| Protect from insider threats        | Protect customers information from zero-day attacks and malicious insiders           | Citrix Analytics for Security                                    |
| Surge protection                    | Scale quickly and efficiently when surges occur                                      | Citrix Cloud services Autoscale                                  |
| Managed endpoints                   | Be able to manage the endpoints given to the agents                                  | Citrix Endpoint Management                                       |

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the following high-level, conceptual architecture. This architecture meets all of the preceding requirements while giving CompanyA the foundation to expand to other use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_01.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_01.png)

The architecture framework is divided into multiple layers. The framework provides a foundation for understanding the technical architecture for the mergers and acquisitions scenario. All layers flow together to create a complete, end-to-end solution.

At a high-level:

**User Layer:** The user layer describes the end-user environment and endpoint devices that are used to connect to resources.

-  Regardless of device, users access resources from Workspace app, resulting in an experience that is identical across every form factor and device platform.
-  CompanyA provides their agents with Google Chromebooks for a fast and easy onboarding experience. The Chromebooks are shipped directly to the agents and automatically configured on first logon.
-  End users are able to use peripherals such as headsets and webcams.
-  CompanyA wants to ensure that no data is stored on the device and can be removed in case the endpoint gets lost, stolen, or the agent leaves the company.

**Access Layer:** The access layer describes details surrounding how users authenticate to their Workspace and secondary resources.

-  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.
-  Many of the authorized resources within the environment use a different set of credentials than the ones used for the primary Workspace identity. CompanyA uses the single sign-on capabilities of each service to better protect these secondary identities. For SaaS apps, the applications only allow SAML-based authentication, which prevents users from accessing the SaaS apps directly and bypassing the security policies.

**Resource Layer:** The resource layer authorizes specific SaaS, web, and virtual resources for defined users and groups while defining the security policies associated with the resource.

-  User has access to apps and desktops that are pertinent to their role.
-  CompanyA provides the necessary contact center applications through Citrix Workspace to their agents.
-  To better protect data, CompanyA requires policies that disable the ability to print, download, and copy/paste content from the managed resource to and from the endpoint.
-  CompanyA requires zero trust network access to resources with the use of isolated browsers or virtualized sessions.
-  HDX technology allows agents to have optimal user experience critical for voice and video communication.

**Control Layer:** The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

-  Even within a protected Workspace resource, users can interact with untrusted Internet resources. CompanyA uses Secure Internet Access to protect the user from external threats from SaaS apps, web apps, virtual apps, and apps on endpoint devices.
-  With all of the policies in place to protect the users when working in a flexible environment, there are still risks. CompanyA uses the Security Analytics service to identified compromised users and automatically take actions to maintain a secure environment.
-  Citrix Virtual Apps and Desktops service manages the authorization and brokering of the virtual apps and desktops.
-  Citrix Endpoint Management ensures that the administrators can manage the Chromebooks that are sent to the agents.

The subsequent sections provide greater detail into specific design decisions for CompanyA's contact center reference architecture.

## User Layer

### Ensuring network resiliency while optimizing web traffic

[![Home Network](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_02.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_02.png)

For many users, accessing their Workspace is simply a matter of connecting to their home network, which is shared between work and personal use. However, based on certain end user requirements, CompanyA requires a secure home network.
CompanyA uses an SD-WAN 110 appliance to create a secure WiFi network for agent's work devices. If the SD-WAN 110 appliance detects reliability issues with the ISP link, it will automatically reroute the remote work network through the standby LTE connection. This ensures that the agent always has connectivity to be able to communicate with customers.

Although the SD-WAN 110 appliances are deployed within the user's home, they are centrally managed. Centrally managing the devices allows CompanyA to institute a zero touch deployment approach for home users following these guides:

-  [SD-WAN 110 Zero Touch Deployment via LTE](https://support.citrix.com/article/CTX272228)
-  [SD-WAN 110 Zero Touch Deployment with Broadband](https://support.citrix.com/article/CTX272229)

Learn more about [SD-WAN 110 for Home Office Users](/en-us/tech-zone/learn/tech-briefs/citrix-sdwan-home-office.html).

High availability and great user experience are important for CompanyA, and SD-WAN can help in both areas. Citrix SD-WAN does per-packet QOS and analyzes traffic in real-time on a per-packet basis, including latency, loss, and jitter. By bonding two relatively inexpensive internet connections, Citrix SD-WAN can deliver higher bandwidth, lowest possible latency, and various [Quality-of-Service features](/en-us/citrix-sd-wan/current-release/quality-of-service.html).

Citrix SD-WAN sets up single-port multi-stream HDX AutoQoS automatically by default (including for voice & video). Citrix SD-WAN prioritizes real-time and interactive traffic ahead of bulk and background traffic. Reliable, secure, high-performance network connectivity with QoS is critical to providing a great user experience with virtual apps and desktops. Citrix SD-WAN masks network glitches with multi-link packet-level processing so users enjoy a continuous, high-performance connection. Direct workload connection also enables Citrix SD-WAN to provide HDX AutoQoS, a valuable user experience feature that automates QoS through cooperative processing with Citrix Virtual Apps and Desktops.
More information on SD-WAN can be found [here](/en-us/tech-zone/learn/tech-briefs/sdwan-workspace.html).

### User Endpoints and Peripherals

CompanyA has decided to provision Google Chromebooks to their agents. This allows agents to standardize on one device.  CompanyA uses Citrix Endpoint Management to manage the Chromebooks and to push the latest version of Citrix Workspace app for Chrome OS onto the Chromebooks.

Users only have to enroll the device on initial set-up. During enrollment, the appropriate applications and security policies are automatically applied and maintained. After that, agents access all their applications and desktops through Citrix Workspace.

A demo on how the agents enroll the devices can be found [here](/en-us/tech-zone/learn/tech-insights/google-chrome-os-management.html).

End-users use approved Citrix Ready peripherals. CompanyA provides Sennheiser and Poly headsets to their agents.

### Microsoft Teams Optimization

With a distributed workforce, CompanyA uses a Contact Center application that relies heavily on virtual conferencing using Microsoft Teams integration. By optimizing the way Microsoft Teams voice and video communication packets cross the wire, Citrix Virtual Apps and Desktops delivers a virtual meeting experience identical to that of a traditional PC.
To learn more about Microsoft Teams integration and optimization, review the following:

-  [Microsoft Teams Tech Insight Video](/en-us/tech-zone/learn/tech-insights/hdx.html#microsoft-teams-optimization)
-  [Microsoft Teams Proof of Concept Guide for Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html)

## Resource Layer

### Contact Center Applications

When determining which contact center application they should standardize on, CompanyA wanted to ensure that the application was tested and validated for use with the Citrix and Google solution. The Citrix Ready Program provides technical support and resources to help 3rd-party partners complete their integration and earn the Citrix Ready validation designation. Below is a list of partners that have completed the validation process, along with some of those that we plan to compete testing soon.

-  [Vonage Contact Center (Citrix Ready)](https://citrixready.citrix.com/vonage/vonage-business-communications-vbc.html)
-  [Ring Central (Citrix Ready)](https://citrixready.citrix.com/ringcentral/ringcentral.html)
-  Amazon Connect
-  Lifesize
-  OneContactCC
-  Genesys
-  Five9
-  Twilio
-  EvolveIP
-  Worldline

*The above is not a comprehensive list of contact center applications that work with Citrix.*

## Access Layer

### Authentication

Due to security concerns, CompanyA requires a strong authentication policy. CompanyA uses a 2-staged approach.
Stage 1 is focused on securing the user's primary identity into Citrix Workspace with a contextual, multi-factor approach.

[![Authentication](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_03.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_03.png)

The authentication policy denies access if the device does not pass an endpoint security scan. The scan verifies that the device is managed and secured with corporate security policies. Once the scan succeeds, the user is able to use their Active Directory credentials and a TOTP token to authenticate.
The stage 2 authentication scheme focuses on the secondary resources (SaaS apps, web apps, virtual apps and desktops). Almost every secondary resource requires authentication. Some use the same identity provider as the user's primary identity, while others use an independent identity provider, most common with SaaS apps.

-  SaaS Apps: For SaaS applications, CompanyA uses SAML-based authentication with Citrix Workspace acting as an identity broker for Active Directory. Once configured, the SaaS applications only allow SAML-based authentication. Any attempt to log on with a username/password specific to the SaaS app fails. This policy allows CompanyA to improve the strength of the authentication while making it easier to disable access due to a compromised user account.
-  Web Apps: The inventory of web applications in CompanyA all use the user's Active Directory credentials. For web applications, CompanyA uses a combination of forms, Kerberos, and SAML-based authentication to provide single sign-on. The choice between the options is based on the unique aspects of each Web application.
-  Virtual Apps/Desktops: For the virtual apps and desktops, CompanyA uses pass-through authentication from Citrix Workspace, eliminating the secondary authentication challenge.

The [Workspace Single Sign-On Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html) contains additional information regarding single sign-on for SaaS, web, virtual apps, virtual desktops, and IdP chaining options.

### Resource Access

CompanyA needs to consider how agents can access internal resources. Internal, corporate resources must be protected from untrusted and unsecured locations. To help prevent malware intrusion, devices are not allowed direct access to the internal network.
To provide access to internal resources like private web apps, virtual apps, and virtual desktops, CompanyA plans to use the Virtual Apps and Desktops service. These two services use a zero-trust network access solution, which is a more secure alternative to traditional VPNs.
The Virtual Apps and Desktops Service use the outbound control channel connections established by the cloud connectors. Those connections allow the user to remotely access internal resources. However, those connections are

-  Limited in scope so that only the defined resource is accessible
-  Based on the user's primary, secured identity
-  Only for specific protocols, which disallow network traversal

## Control Layer

### Citrix Virtual Apps and Desktops Service

CompanyA has chosen to use Citrix Virtual Apps and Desktops service because it gives them the flexibility, they need to deploy resources from multiple resource locations from a unified management console. It also reduces the administrative overhead to deploy and manage their virtual apps and desktop environment. It allows them to minimize hardware costs and deploy DaaS resources. With Citrix Virtual Apps and Desktops service, the Delivery Controllers, SQL Database, Studio, Director, and Licensing are the core components in the Control layer. These components are provisioned on Citrix Cloud by Citrix during the activation of the Virtual Apps and Desktop service. Citrix handles the redundancy, the updates, and the installation of these components.

More in-depth information on Citrix Virtual Apps and Desktops service can be found [here](/en-us/tech-zone/learn/tech-briefs/cvads.html).

### Service Continuity

It is important for CompanyA that their users don't experience lost productivity due to outages or cloud issues. Therefore, they have turned on service continuity within Citrix Cloud. Service continuity allows users to connect to resources that are reachable during outages or when Citrix Cloud components are unreachable. This functionality has given CompanyA peace of mind to ensure that even in the rare occasion of a cloud outage, their users are still productive. Service continuity improves the visual representation of published resources during outages by using Progressive Web Apps service worker technology to cache resources in the user interface. Service continuity indicates which resources are available during an outage.
Service continuity uses Workspace connection leases to allow users to access apps and desktops during outages. Workspace connection leases are long-lived authorization tokens.

More information on how Service continuity works can be found [here](/en-us/tech-zone/learn/tech-briefs/citrix-cloud-resiliency.html).

### Autoscale

CompanyA chose to deploy Autoscale to optimize cloud costs. Autoscale allows you to intelligently use, allocate, and deallocate resources.

CompanyA initially uses the following schedule based Autoscale parameters based on the typical workday:

| Day      | Peak Times | Off-Peak Times | Machines Active         |
|----------|------------|----------------|-------------------------|
| Weekdays | 7am-5pm    | 5pm-7am        | Peak: 50% Off-Peak: 10% |
| Weekends | 9am-6pm    | 6pm-9am        | Peak: 50% Off-Peak: 10% |

To accommodate more users, CompanyA also enabled load-based scaling with the following parameters:

| Day      | Capacity Buffer (Peak) | Capacity Buffer (Off-peak) |
|----------|------------------------|----------------------------|
| Weekdays | 20%                    | 5%                         |
| Weekend  | 20%                    | 5%                         |

More information about Autoscale can be found [here](/en-us/tech-zone/learn/tech-briefs/autoscale.html).

### Secure Internet Access

As users interact with SaaS, web, virtual, local, and mobile apps, they are often accessing public internet sites. Although CompanyA has an Internet Security Compliance class all agents must complete on a yearly basis, it has not completely prevented attacks, most often originating through phishing scams.
To help protect the agents and the organization, CompanyA incorporates the Secure Internet Access service and Security Analytics into their architecture.

[![Secure Internet Access](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_04.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_04.png)

Any internet traffic to/from the library of apps, desktops, and devices within the organization are routed through the Secure Internet Access service. Within this service, any URL is scanned to verify it is safe. Functionalities within certain public sites are denied or modified. Downloads are automatically scanned and verified.

As many websites are now encrypted, part of this security process is to decrypt the traffic and inspect. CompanyA wants to allow users with the flexibility to access personal sites while on company-owned devices. To ensure employee privacy, certain categories of websites will not be decrypted, like financial, and health related sites. To have complete transparency, CompanyA plans to make the overall security policy plan available internally.

In designing the internet security policy, CompanyA wanted to start with a baseline policy. As CompanyA continues to assess risks within the organization, they will relax/strengthen the policies as appropriate.

By default, all categories are decrypted and allowed. CompanyA made the following modifications that are applied globally:

| Category                 | Change         | Reason                 |
|--------------------------|----------------|------------------------|
| Financial and Investment | Do not decrypt | Agent privacy concerns |
| Health                   | Do not decrypt | Agent Privacy concerns |
| Adult Content            | Block          |                        |
| Drugs                    | Block          |                        |
| File Sharing             | Block          |                        |
| Gambling                 | Block          |                        |
| Illegal Activity         | Block          |                        |
| Malicious Sources        | Block          |                        |
| Malware Content          | Block          |                        |
| Porn/Nudity              | Block          |                        |
| Virus/Malware            | Block          |                        |
| Violence/Hate            | Block          |                        |

CompanyA has the following two chrome extensions installed:

-  Chromebook Connector: This connects Chromebooks to the cloud agent to extend all cloud security capabilities across Chromebook devices
-  Chromebook App Compatibility Plugin: This is required to ensure compatibility with apps running outside the Chrome browser

### Citrix Endpoint Management

[![CEM](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_05.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-contact-centers_05.png)

CompanyA uses Citrix Endpoint Management to manage the Chromebooks that are sent to the agents. CompanyA pushes Citrix Workspace to the Chromebooks which is the primary way that agents access their apps. The Secure Internet Access agent is also be pushed via Citrix Endpoint Management.

CompanyA also uses Citrix Endpoint Management to push the following device policies:

-  [App Restrictions Policy](/en-us/citrix-endpoint-management/policies/app-restrictions-policy.html#chrome-app-settings): CompanyA uses this to restrict any applications that they don't want to allow on the company-owned Chromebooks
-  [Control OS updates](/en-us/citrix-endpoint-management/policies/control-os-updates.html#chrome-os-settings): CompanyA uses this to ensure that all endpoints are on a specific OS version. They use this to delay updates if needed.
-  [Content device policy](/en-us/citrix-endpoint-management/policies/content-policy.html): CompanyA uses this policy to set their browser's home page
-  [Restrictions policy](/en-us/citrix-endpoint-management/policies/restrictions-policy.html#chrome-os-settings): CompanyA uses this policy to set the following restrictions:
    -  Disable printing-on
    -  Disable proceeding from the safe browsing warning page-on
    -  Safe browsing mode-on
    -  External storage accessibility- Disabled

### Security Analytics

CompanyA uses Security Analytics to mitigate and stop threats.

To help protect the environment, CompanyA uses Citrix Security Analytics to identity insider threats, and compromised users. Often, a single instance of a threat does not warrant drastic action, but a series of threats can indicate a security breach.

CompanyA developed the following initial security policies:

| Name                          | Conditions                                                            | Action                | Reason                                                                                                                                                                                                                 |
|-------------------------------|-----------------------------------------------------------------------|-----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unusual Access                | Log on from suspicious IP and access from an unusual location         | Lock user             | If a user logs in from an unusual location and a suspicious IP, there is a strong indication the user was compromised.                                                                                                 |
| Unusual app behavior          | Unusual time of app usage and access from unusual location            | Request user response | If a user accesses a virtual app at a strange time and location, there is the potential the user is compromised. Security analytics notifies the user to confirm if the user identifies the activity.               |
| Potential credential exploits | Excessive authentication failures and access from an unusual location | Add to watchlist      | If a user has many authentication failures from an unusual location, it can indicate that someone is trying to break into the system. However, the attacker has yet to succeed. Only need to add the user to the watchlist. |
