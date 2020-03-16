---
layout: doc
description: When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.
---
# Remote PC Access Design Decisions

## Contributors

**Author:** Matthew Greenbaum, Simon Jackson, Nick Rintalan, [Daniel Feller](https://twitter.com/djfeller)

## Overview

Remote PC Access is an easy and effective way to allow users access to their office-based, physical Windows PC. Using any endpoint device, users can remain productive regardless of their location. However, organizations want to consider the following when implementing Remote PC Access.

## Authentication

Users continue to authenticate with Active Directory, but this authentication happens when the user initiates a connection to the organization's public fully qualified domain (FQDN). The FQDN requests authentication from Citrix Gateway.

Because the site is external, organizations require stronger authentication than a simple Active Directory user name and password. Incorporating multifactor authentication, like a time-based one-time password token, can greatly improve authentication security.

Citrix Gateway provides organizations with numerous multifactor authentication options, which include:

*  [Time-based One Time Password](/en-us/citrix-gateway/13/authentication-authorization/configure-onetime-passwords.html)
*  [RADIUS](/en-us/citrix-gateway/13/authentication-authorization/configure-radius.html)
*  [TACACS+](/en-us/citrix-gateway/13/authentication-authorization/configure-tacacs.html)
*  [SAML](/en-us/citrix-gateway/13/authentication-authorization/configure-saml.html)

Some of these options, like the time-based one time password, requires the user to initially register for a new token. As this option requires access to the user's email, it would need to be done before the user attempts to work remotely.

## Session Security

Users are able to remotely access the work PC with an untrusted, personal device. Organizations can use integrated Citrix Virtual Apps and Desktops policies to protect against:

*  Endpoint Risks: Key loggers secretly installed on the endpoint device can easily capture a user's user name and password. Anti-keylogging capabilities protect the organization from stolen credentials by obfuscating keystrokes.
*  Inbound Risks: Untrusted endpoints can contain malware, spyware, and other dangerous content. Denying access to the endpoint device's drives prevents transmission of dangerous content to the corporate network.
*  Outbound Risks: Organizations must maintain control over content. Allowing users to copy content to local, untrusted endpoint devices places extra risks on the organization. These capabilities can be denied by blocking access to the endpoint's drives, printers, clipboard, and anti-screen-capturing policies.

## Infrastructure Sizing

***Note:** The following sizing recommendations are a good starting point, but each environment is unique, resulting in unique results. Please monitor the infrastructure and size appropriately.*

In many implementations, organizations enable thousands of physical PCs for Remote PC Access capabilities. Each PC must register with the Citrix Virtual Apps and Desktop controller. The controller must be able to accommodate the increase load. Also, Citrix Gateway must be sized appropriately to handle the ICA traffic for the new users.

As a general recommendation with regards to sizing

### Citrix Virtual Apps and Desktops

When using an on-premises deployment of Citrix Virtual Apps and Desktops, start with the following recommendations:

*  A 4 vCPU controller can handle 5,000 new sessions
*  A 4 vCPU StoreFront server supports 55,000 requests per hour

### Gateway

 When using an on-premises Citrix ADC for Citrix Gateway, consult the data sheet for the particular model and refer to the SSL VPN/ICA proxy concurrent users line item as a starting point. If the ADC is handling other workloads, validate that current throughput and CPU usage are not approaching any upper limits

### Gateway Service

When using the Gateway Service from Citrix Cloud, sizing is irrelevant as it is managed by Citrix. However, this type of implementation requires on-premises Citrix Cloud Connectors.

The rendezvous protocol enables the Virtual Delivery Agent installed on each physical PC to communicate directly with the Gateway Service instead of tunneling the session through the Cloud Connector. The rendezvous protocol requires the 1912 version of the Virtual Delivery Agent.

[![Rendezvous Protocol Policy](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)

It is disabled by default and can be enabled with a Virtual Apps and Desktops policy.

For Remote PC Access, the following can be used for initial planning estimations:

*  A 4 vCPU Citrix Cloud Connector can handle 1,000 concurrent sessions without using the rendezvous protocol. Without the rendezvous protocol, traffic funnels through the Cloud Connector on the way to the Gateway Service.
*  A 4 vCPU Citrix Cloud Connector sizing becomes irrelevant when using the rendezvous protocol as the ICA traffic flows directly between the Gateway Service and the PC.

## Availability

Remote PC Access supports Wake-on-LAN operations to enable powering on Windows PC that are currently powered off. However, this option requires the use of Microsoft Endpoint Configuration Manager.

If Configuration Manager is not an option, administrators can configure an Active Directory Group Policy object to remove the "Shut Down" option from the Windows PC. This prevents the user from powering down the physical PC, allowing them to connect remotely.

***Note:** Wake on LAN functionality is not available when using the cloud service - Citrix Virtual Apps and Desktops Service.*

If there is a power failure, the physical PC will be powered off. If available, modify the PC's BIOS setting to automatically power on in the event of a power failure.

## User Assignments

To have the remote experience match the local experience, users need to connect to their personal work PC. The user must get assigned to the correct physical PC.

User assignments can be automated. Once the Virtual Delivery Agent installs on the physical PC, the next user who logs on will get assigned to that PC within Citrix Virtual Apps and Desktops. This is an effective method for assigning thousands of users.

By default, multiple users can be assigned to a desktop if they have all logged into the physical PC, but this can be disabled via a [registry edit](https://support.citrix.com/article/CTX137805) on the Delivery Controllers.

The Citrix Virtual Apps and Desktops administrator can modify the assignments as needed within Citrix Studio or via PowerShell.

## Agent Deployment

To deploy the Virtual Delivery Agent to thousands of physical PCs, automated processes are required.

The install media for Citrix Virtual Apps and Desktops include a deployment script (InstallMedia\Support\ADDeploy\InstallVDA.bat) that can be leveraged by Active Directory Group Policy Objects.

The script can be used as a baseline for PowerShell scripts and Enterprise Software Deployments (ESD) tools. These approaches allow organizations to quickly deploy the agent to thousands of physical endpoints.

## VDA Registration

Depending on the network topology, the subnet containing the Virtual Apps and Desktops delivery controllers might not allow communication from the physical PCs. To properly register with the delivery controller, the VDA on the PC must be able to communicate with the delivery controller over ports:

*  TCP port 80 if communication unsecured
*  TCP port 443 if communication secured (preferred configuration)

## Other Tech Zone content

[Learn -> PoC Guides -> Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html) - Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and your on premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.

[Learn -> PoC Guides -> Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html) - Learn how to deliver the Citrix® HDX™ Optimization for Microsoft® Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops-service.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop deployment in Citrix Cloud.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops On-Prem](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop on-premises deployment.

[Learn -> Tech Insight -> HDX - Display and Adaptive Technologies](/en-us/tech-zone/learn/tech-insights/hdx-adaptive-display.html) - Technologies that ensure an unparalleled user experience when connecting to remote Citrix resources.

[Learn -> Tech Insight -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/tech-insights/virtual-apps-and-desktops-service.html) - Citrix Virtual Apps and Desktops Service provides a fast, low-impact deployment option for on-premises/cloud-hosted, Windows/Linux, desktops/apps.

[Learn -> Tech Insight -> App Layering - User Layers](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html) - User layers persist user profile settings, data, and user-installed applications in non-persistent VDI environments.

[Learn -> Tech Insight -> Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) - Single Sign-on to Windows-based virtual apps and desktops when using a non-Active Directory based Citrix Workspace identity.

[Design -> Reference Architectures -> Optimizing Unified Communications Solutions](/en-us/tech-zone/design/reference-architectures/optimizing-unified-communications-solutions.html) - Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Reference Architectures -> Remote PC Access](/en-us/tech-zone/design/reference-architectures/remote-pc.html) - Discover the use cases and learn about the detailed architecture of Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Single Server Scalability](/en-us/tech-zone/design/design-decisions/single-server-scalability.html) - Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Design Decisions -> Designing StoreFront and Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) - Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.

[Design -> Design Decisions -> VDI Model Comparison](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html) - Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.

[Build -> Tech Papers -> Deploying Google Chrome](/en-us/tech-zone/build/tech-papers/google-chrome.html) - Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
