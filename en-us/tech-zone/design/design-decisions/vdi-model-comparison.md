---
layout: doc
description: Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.
---
# Evaluating VDI Models

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

## Overview

Selecting the best VDI model starts with properly defining user groups and then aligning the requirements with the capabilities of the VDI models. Although there are multiple approaches towards defining user groups, it is often easiest to align user groups with departments. Typically most users within the same department or organizational unit consumes the same set of applications.

## User Segmentation

Depending on the size of the department, there might be a subset of users with unique requirements. Each defined user group is evaluated against the following criteria to determine if the departmental user group needs to be further divided into more specialized user groups.

*  Primary data center – Each user has a primary data center or cloud resource location assigned that hosts their virtual desktop, data, and application servers. Identify the data center that the user is assigned to rather than the data center they are currently using. Users are grouped based on their primary data center so that a unique design can be created for each one.
*  Personalization – Personalization requirements are used to help determine the appropriate VDI model for each user group. For example, if a user group requires complete personalization, a personal desktop is recommended as the optimal solution. There are three classifications available:

| Personalization | Requirement |
| :--- | :--- |
| None | User cannot modify any user or application settings, for example - a kiosk. |
| Basic | User can modify user-level settings of desktops and applications. |
| Complete | User can make any change, including installing applications. |

*  Security – Security requirements are used to help determine the appropriate desktop and policy (or policies) for each user group. For example, if a user group requires high security, a hosted pooled desktop or a local VM desktop is recommended as the optimal solution. There are three classifications available:

| Security Level | Description |
| :--- | :---|
| Low | Users are allowed to transfer data in and out of the virtualized environment. |
| Medium | All authentication and session traffic is be secured; users are not be able to install or modify their virtualized environment. |
| High | In addition to traffic encryption, no data leaves the data center (such as through printing or copy/paste); all user access to the environment is audited. |

*  Mobility – Mobility requirements are used to help determine the appropriate desktop model for each user group. For example, if a user group faces intermittent network connectivity, then any VDI model requiring an active network connection is not applicable. There are four classifications available:

| Mobility | Requirement |
| :--- | :--- |
| Local | Always uses the same device, connected to an internal, high-speed, and secured network. |
| Roaming Local | Connects from different locations on an internal, high-speed, secured network. |
| Remote | Sometimes connects from external variable-speed, unsecure networks. |

*  Desktop Loss Criticality – Desktop loss criticality is used to determine the level of high availability, load balancing, and fault tolerance measures required. For example, if there is a high risk to the business if the user’s resource is not available, the user is not allocated a local desktop. If that local desktop fails, the user is not able to access their resources. There are three classifications available:

| Desktop loss criticality | Requirement |
| :--- | :--- |
| Low | No major risk to products, projects, or revenue. |
| Medium | Potential risk to products, projects, or revenue. |
| High | Severe risk to products, projects, or revenue. |

*  Workload – Types and number of applications accessed by the user impacts overall density and the appropriate VDI model.  Users requiring high-quality graphics must utilize a local desktop implementation or a professional graphics desktop. There are three classifications available:

| User Type | Characteristics |
| :--- | :--- |
| Light | 1–2 office productivity apps or kiosk. |
| Medium | 2–10 office productivity apps with light multimedia use. |
| Heavy | Intense multimedia, data processing, or application development. |

**_Note:_** Performance thresholds are not identified based on processor, memory or disk utilization because these characteristics will change dramatically following the application rationalization and desktop optimization process. It is likely that the user’s management tools and operating system will change during the migration process. Instead, workload is gauged based on the number and type of applications the user runs.

| Type | Experience from the field |
| :--- | :--- |
|Utility Company | A large utility company collected data on every user in their organization. During the user segmentation process, it was realized that the organization’s existing role definitions were sufficiently well defined that all the users within a role shared the same requirements. This allowed a significant amount of time to be saved by reviewing a select number of users per group. |
| Government | A government organization discovered that there was significant deviation between user requirements within each role, particularly around security and desktop loss criticality. As such, each user needed to be carefully reviewed to ensure that they were grouped appropriately |

## Assign VDI Models

As with physical desktops, it is not possible to meet every user requirement with a single type of VDI. Different types of users need different types of resources. Some users may require simplicity and standardization, while others may require high levels of performance and personalization. Implementing a single VDI model across the entire organization inevitably leads to user frustration and reduced productivity.
Citrix offers a complete set of VDI technologies that have been combined into a single integrated solution. Because each model has different strengths, it is important that the right model is chosen for each user group within the organization.
The following list provides a brief explanation of each VDI model.

*  Hosted Apps – The hosted apps model delivers only the application interface to the user. This approach provides a seamless way for organizations to deliver a centrally managed and hosted application into the user’s local PC. The Hosted Apps model is often utilized when organizations must simplify management of a few line-of-business applications. Hosted apps include a few variants:
    *  Windows Apps – The Windows apps model utilizes a server-based Windows operating system, resulting in a many users accessing a single VM model.
    *  VM hosted apps – The VM hosted apps model utilizes a desktop-based Windows operating system, resulting in a single user accessing a single VM model. This model is often used to overcome application compatibility challenges with a multi-user operating system, like Windows 2008, Windows 2012 and Windows 2016.
    *  Linux Apps – The Linux apps model utilizes a server-based Linux operating system, resulting in a many users accessing a single VM model.
    *  Browser Apps – The browser apps model utilizes a server-based Windows operating system to deliver an app as a tab within the user’s local, preferred browser. This approach provides a seamless way for organizations to overcome browser compatibility challenges when users want to use their preferred browser (Internet Explorer, Microsoft Edge, Google Chrome, Mozilla Firefox and so on) but the web application is only compatible with a specific browser.
*  Shared Desktop – With the shared desktop model, multiple user desktops are hosted from a single, server-based operating system (Windows 2008, 2012, 2016, Red Hat, SUSE, CentOS, and Ubuntu). The shared desktop model provides a low-cost, high-density solution; however, applications must be compatible with a multi-user server based operating system. In addition, because multiple users share a single operating system instance, users are restricted from performing actions that negatively impact other users, for example installing applications, changing system settings, and restarting the operating system.
*  Pooled Desktop – The pooled desktop model provides each user with a random, temporary desktop operating system (Windows 7, Windows 8, and Windows 10). Because each user receives their own instance of an operating system, overall hypervisor density is lower when compared to the shared desktop model. However, pooled desktops remove the requirement that applications must be multi-user aware and support server based operating systems.
*  Personal Desktop – The personal desktop model provides each user with a statically assigned, customizable, persistent desktop operating system (Windows 7, Windows 8, Windows 10, Red Hat, SUSE, CentOS, and Ubuntu). Because each user receives their own instance of an operating system, overall hypervisor density is lower when compared to the shared desktop model. However, personal desktops remove the requirement that applications must be multi-user aware and support server based operating systems.
*  Pro Graphics Desktop – The pro graphics desktop model provides each user with a hardware-based graphics processing unit (GPU) allowing for higher-definition graphical content.
*  Local Streamed Desktop – The local streamed desktop model provides each user with a centrally managed desktop, running on local PC hardware
*  Local VM Desktop – The local VM desktop model provides each user with a centrally managed desktop, running on local PC hardware capable of functioning with no network connectivity.
*  Remote PC Access – The Remote PC Access desktop model provides a user with secure remote access to their statically assigned, traditional PC. The Remote PC Access desktop model is often the fastest and easiest VDI model to deploy as it utilizes already deployed desktop PCs.

Each user group is compared against the following table to determine which VDI model best matches the overall user group requirements. In many environments, a single user might utilize a desktop VDI model and an app VDI model simultaneously.

| User Segmentation Category | User Segmentation Characteristic | Hosted Apps | Hosted Shared Desktop | Hosted Pooled Desktop | Hosted Personal Desktop | Hosted Pro Graphics Desktop | Remote PC Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Workload**
| | **Light** | Best | Good  | Good  | Good  | Bad | Good |
| | **Medium** | Good | Best | Best | Good | Good | Good |
| | **Heavy** | Bad | Bad | Bad | Good | Best | Good |
| **Mobility**
| | **Local** | Best | Best | Best | Good | Good | Good |
| | **Roaming Local** | Best | Best | Best | Good | Good | Good |
| | **Remote** | Best | Best | Best | Good | Good | Best |
| **Personalization** |
| | **None** | Best | Best | Best | Bad | Good | Good |
| | **Basic** | Best | Best | Best | Bad | Good | Good |
| | **Complete** | Bad | Bad | Bad | Best |  | Best |
| **Security** |
| | **Low** | Good | Good | Good | Good | Good | Good |
| | **Medium** | Best | Best | Best | Good | Good | Good |
| | **High** | Good | Good | Best | Bad | Good | Bad |
| **Criticality** |
| | **Low** | Good | Good | Good | Good | Good | Good |
| | **Medium** | Best | Best | Best | Good | Good | Bad |
| | **High** | Best | Best | Best | Bad | Good | Bad |

Don’t forget to follow these top recommendations from Citrix Consulting based on years of experience:

Citrix Consulting Tips for Success

1.  Start with Windows apps, shared desktops, and pooled desktops – As you can see in the VDI capability table, the Windows apps, hosted shared desktop, and pooled desktop models can be used in most situations. By reducing the number of VDI models required, you help reduce deployment time and simplify management.
1.  Perfect match – It might not be possible to select a VDI model that is a perfect match for the user group. For example, you can’t provide users with a desktop that is highly secure and offers complete personalization at the same time. In these situations, select the VDI model which is the closest match to the organization’s highest priorities for the user group.
1.  Desktop loss criticality – There are only three VDI models that meet the needs of a high desktop loss criticality user group (backup desktops available) – none of which allow for complete personalization. If a high-desktop loss criticality user group also requires the ability to personalize their desktop they are provided with a pool of backup desktops (hosted shared, pooled) in addition to their primary desktop. Although these desktops would not include customizations made to their primary desktop, they would allow users to access core applications such as mail, Internet, and Microsoft Office.
1.  Consider Operations & Maintenance – The ongoing support of each VDI model should be factored in when deciding on a VDI model. For example, pooled desktops can be rebooted to a known good state which often leads to reduced maintenance versus a personal desktop where each desktop is unique.

## Other Tech Zone content

[Learn -> PoC Guides -> Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html) - Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and your on premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.

[Learn -> PoC Guides -> Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html) - Learn how to deliver the Citrix® HDX™ Optimization for Microsoft® Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops-service.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop deployment in Citrix Cloud.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops On-Prem](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop on-premises deployment.

[Learn -> Tech Insight -> App Layering - User Layers](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html) - User layers persist user profile settings, data, and user-installed applications in non-persistent VDI environments.

[Learn -> Tech Insight -> Remote PC Access](/en-us/tech-zone/learn/tech-insights/remote-pc-access.html) - Remote PC Access allows users to access their physical, office-based Windows PC from remote locations.

[Learn -> Tech Insight -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/tech-insights/virtual-apps-and-desktops-service.html) - Citrix Virtual Apps and Desktops Service provides a fast, low-impact deployment option for on-premises/cloud-hosted, Windows/Linux, desktops/apps.

[Learn -> Tech Insight -> Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) - Single Sign-on to Windows-based virtual apps and desktops when using a non-Active Directory based Citrix Workspace identity.

[Learn -> Tech Insight -> HDX - Display and Adaptive Technologies](/en-us/tech-zone/learn/tech-insights/hdx-adaptive-display.html) - Technologies that ensure an unparalleled user experience when connecting to remote Citrix resources.

[Design -> Reference Architectures -> Remote PC Access](/en-us/tech-zone/design/reference-architectures/remote-pc.html) - Discover the use cases and learn about the detailed architecture of Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.

[Design -> Reference Architectures -> Optimizing Unified Communications Solutions](/en-us/tech-zone/design/reference-architectures/optimizing-unified-communications-solutions.html) - Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Remote PC Access](/en-us/tech-zone/design/design-decisions/remote-pc-access.html) - When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> Single Server Scalability](/en-us/tech-zone/design/design-decisions/single-server-scalability.html) - Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Design Decisions -> Designing StoreFront and Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) - Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.

[Build -> Tech Papers -> Deploying Google Chrome](/en-us/tech-zone/build/tech-papers/google-chrome.html) - Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
