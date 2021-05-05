---
layout: doc
h3InToc: true
contributedBy: Sasa Petrovic
specialThanksTo: Holger Marggrander, Jeff Qiu, Maria Chang, Martin Latteier, Martin Zugec, Rico Projer, Robert Wolfer, Sarah Steinhoff
description: Learn about the different decision factors involved in choosing the optimal application delivery method.
tz_title: Evaluating Application Delivery Methods
tz_products: other;
---
# Design Decision: Evaluating Application Delivery Methods

## Overview

Evaluating which is the best application delivery method is an activity as old as Citrix and has grown more complex as more application delivery technologies have been developed. Although it is a question asked frequently, the answer isn't always straightforward. Circumstances like different user demands, various application types, and new or changing delivery technologies can strongly impact the evaluation.

This article is meant to be a guideline to help you identify the best application delivery method for the use case at hand based on its requirements. As the ecosystem of applications today has dramatically changed, it is expected to evolve even more in the upcoming years with the incorporation of SaaS-based applications. Hence, different aspects have to be considered during an evaluation process to identify the best delivery method. To simplify this complex process, decision tree diagrams have been created to guide you through the various scenarios. The diagrams are separated into the following segments:

1.  Modern vs. Traditional
2.  Endpoint vs. Citrix Virtual Apps and Desktops
3.  Hosted Shared vs. VDI Desktop
4.  Hosted Shared Desktop vs. Hosted Shared Application

The four segments depict different tiers of the application delivery methods and some of the outcomes of a segment lead to a subsequent flowchart. The following overview shows how the tiers relate to each other:

![Explicit Decision Factors](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_000.png)

The first and second tiers are most relevant to Solution Architects and Application Business Owners as their outcome is a technology stack rather than a delivery method. Tier three and four are technically driven and related to Citrix Virtual Apps and Desktops delivery methods. Therefore, they are geared towards Engineers and Administrators.

Different requirements, needs, and circumstances lead to different outcomes and therefore a “one size fits all” method does not exist. There is no right or wrong in the evaluation process either, as every environment has its own unique characteristics. Companies with a large user base and complex change management process spread across many branches come to different conclusions than small businesses with one data center and a simple change management process.

While you can run the flowchart for every application, the diagrams are primarily meant to give you a general guidance on a delivery strategy and can also be used to challenge the current install base. The diagrams furthermore contain explanations to almost every decision and its implications and give recommendations for the particular use case.

**Please note**: The unique characteristics of your environment require all setups and combinations to be thoroughly tested before an implementation to avoid any unforeseen results.

For more resources on the different delivery methods, see [Citrix Docs](/en-us/citrix-virtual-apps-desktops-service/delivery-methods.html).

## Modern vs. Traditional Overview

**Modern**: For this article, we consider web-based applications, delivered as a Software as a Service (SaaS), as modern. These applications are typically hosted in a cloud computing environment. Web applications located in an on-premises data center can also be considered as modern, as long as the code execution is done on the web server and no client components are needed (except a web browser).

**Traditional**: Traditional means an application is installed directly on the user’s endpoint and / or Citrix Virtual Apps and Desktops workload. This type is also referred to as classic application. Calling them legacy application would not be accurate since most of the applications today still have to be installed and are not available as SaaS applications.

From a technical perspective, SaaS applications are preferred. The code is run on a web server hosted in a cloud environment, which typically causes less resource usage on the client / front-end side. In addition, scalability and maintenance of the back-end system is no longer your concern as it is taken care of by the application provider. In this model, the application is also kept in an "evergreen" state without major impacts to your environment. On the client / front-end only a browser is needed to access the application. Hence, little to no maintenance work related to the application is needed here either. This setup also enables you to use any device of your choosing since there is no dependency on the operating system. [Citrix Workspace](https://www.citrix.com/products/citrix-workspace/) is the ideal platform to deliver and manage SaaS applications in a secure manner. Features and solutions like [Secure Workspace Access](/en-us/tech-zone/design/reference-architectures/access-control.html), [Security Analytics](/en-us/citrix-analytics/security-analytics/about.html), Single-Sign-On to SaaS applications and the incorporation of [microapps](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html) through [Workspace intelligence](https://www.citrix.com/digital-workspace/intelligent-workspace-solution.html) provide a unified and therefore best user experience with the highest security possible.

However, there are reasons why SaaS applications cannot be used. For instance, if technical, legal and / or security requirements can’t be met, a traditional approach needs to be considered. In such a scenario, it’s best to identify the exact reasons why the usage of SaaS is not possible. Once identified, it's recommended to clarify if a partial integration or a transition in stages is possible, to benefit from the advantages offered by SaaS technologies.

[![Modern vs Traditional](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_001.png)](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_001.png)

## Endpoint vs. Citrix Virtual Apps and Desktops Overview

**Endpoint:** Installation on the physical client device.

**Citrix Virtual Apps and Desktops:** Application virtualization via Citrix Virtual Apps and Desktops, where the applications are installed on a Hosted Shared server or VDI Desktop. The exact Citrix Virtual Apps and Desktops delivery method will be determined in the subsequent segments.

### Device Diversity

The ever-increasing number of digital natives joining the workforce compels companies to expand their endpoint portfolio with non-Windows devices as well. Also, SaaS applications are further enabling users to access applications regardless of the type of device and operating system in use. The demand to allow devices which are not Windows-based has drastically increased over the last couple of years. To allow a bring-your-own-device (BYOD) or chose-your-own-device (CYOD) approach, Citrix Virtual Apps and Desktops can be used to deliver Windows-based applications to non-windows devices as well.

### Security

Moving applications to Citrix Virtual Apps and Desktops lowers the client footprint and allows a zero-trust architecture. Citrix virtualization and networking technologies provide robust methods for segmenting users, applications, and data while still providing a seamless user experience. This way, the network traffic can be streamlined. The endpoint to server network communication will be reduced to a minimum, which in turn reduces the exposure of your server network. The application data traffic between the front-end and back-end will solely reside within the confines of your server network.

### Contractors

Usually contactors already own devices. Instead of handing out corporate devices, Citrix Virtual Apps and Desktops along with [Citrix Gateway](https://www.citrix.com/products/citrix-gateway/) can be used to allow a secure access to applications, desktops, and other resources. This approach lowers the endpoint costs and maintenance efforts.

### Time-to-Market

Installation of applications on numerous endpoints can be a tedious, time consuming and error-prone task, as the installation needs to be ran on every device. This circumstance especially applies to large enterprises, with thousands of devices spread across the globe. In such use cases, an application release can take weeks or even months until it has been distributed to every device. If issues occur, a rollback might be an even more complex and time-consuming endeavor.

Citrix Virtual Apps and Desktops allows you to centralize the application management. Application releases are independent of the client device, since the update is done on Hosted Shared servers or VDI Desktops in a corporate data center. Furthermore, it’s highly recommended to use Citrix Provisioning Services or Machine Creation Services to benefit from Citrix’s market leading [image management](/en-us/tech-zone/design/design-decisions/image-management.html) capabilities. Both image management solutions allow a consistent installation baseline across all virtual machines and provide the fastest rollout and rollback methods. Releases can be rolled-out or rolled-back with a simple reboot of the virtual machine which reduces the time-to-market of new application deployments to a minimum.

### Mobile Workforce

Mobile users are often traveling and need to access applications offline as well. While offline, editing documents or writing emails are the most common tasks performed. In such a scenario the application has to be installed on the endpoint. Most of today's business applications however, require a back-end connectivity to work. Which in turn means, that the mobile user has to be online to use the application. The Citrix [HDX](https://www.citrix.com/digital-workspace/hdx/) protocol enables mobile workers to access applications with great user experience even on a low bandwidth or high latency connection.

[![Endpoint vs CVAD](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_002.png)](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_002.png)

## Hosted Shared vs. VDI Desktop Overview

**Hosted Shared (multi-user):** Hosted Shared systems are VDAs based on a Windows server operating system with the Remote Desktop Session Host role (formerly known as Terminal Server) installed. This type is referred to as Multi-session OS / Server OS VDAs and is shared among multiple users simultaneously.

**VDI Desktop (single-user):** In this article, VDI refers to Single-session OS / Desktop OS VDAs. This delivery type is based on a client operating system and used exclusively by a single user at a time.

Generally, Hosted Shared Desktops tend to be more cost effective as multiple users are hosted on a single machine. Nevertheless, there are use cases where a VDI Desktop is preferred, such as to support resource (CPU, memory, disk) intensive applications. Also, users who need administrative privileges to work, require a VDI due to security considerations and to have the ability to install and change the desktop according to their needs (without impacting others). There are also customers using VDI because the operational and processual synergies with other solutions outweigh the additional cost overhead.

[![Hosted Shared vs VDI](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_003.png)](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_003.png)

## Hosted Shared Desktop vs. Hosted Shared Application Overview

**Hosted Shared Desktop:** This method is a desktop published to multiple users on a single Multi-session OS.

**Hosted Shared Application (multi use):** With the Host Shared Application model (multi use), multiple applications are installed on the same server and shared among some users. It’s based on a Multi-session OS as well and sometimes referred as a siloed approach. In this model, applications are delivered virtually and displayed seamlessly in high definition on user devices.

**Hosted Shared Application (single use):** The only difference between multi and single use is, that Host Shared Application single use has only **a single** business application installed. This application can still be used by multiple users at once. **Important:** This type of solution can be avoided as much as possible, as it is inefficient from a resource (costs) and maintenance (efforts) point of view.

The approach in this segment is slightly different to the others. There are many different combinations how these three delivery methods can be utilized. Because of this, we tried to identify the optimal delivery method based on the challenges our customers and partners face the most. It is important to work closely with the appropriate Business Application Owners to understand the application's characteristics in detail and therefore to better assess which delivery models can be used.

From the operational perspective, placing as many applications as possible on a single image can often lead to a reduction of maintenance efforts. Fewer images mean less work. However, this requires that there are no technical conflicts between these applications. Sometimes changes on one application require testing all other applications on the image as well. Hence, it’s important to reflect on the change and release management process of every application in detail, to avoid organizational conflicts. Hosting the application on a file share (if at all possible) or via App-V (Shared Content Store) can even more simplify the release process, as changes can be applied without going through an imaging process. Both options can't be used for all use cases and require extra and appropriately sized infrastructure. Regardless, these methods should be at least considered as it can help to reduce the number of image changes.

Other factors such as security requirements and performance utilization can also have an impact on the decision-making process. Especially applications with unpredictable resource utilization and regular CPU-bursts have a negative impact on other applications and its users. Such bottlenecks have to be avoided at any cost, because then all users on that system suffer from a bad user experience. Workspace Environment Management can help to mitigate such performance bottlenecks. Applications, where bottlenecks can’t even be handled by [Workspace Environment Management](/en-us/workspace-environment-management), can be placed on dedicated servers (Hosted Shared Application single use). This type of setup ensures that the necessary resources are available and avoids negative impact on other applications.

[![Hosted Shared Desktop vs Hosted Shared Apps](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_004.png)](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_004.png)

## Summary

In this article, we have reflected the most common decision factors when choosing an application delivery method. This guide can help you to identify the optimal method for your own unique environment.

## Sources

The goal of this article is to assist you with planning your own implementation. To make this task easier, we would like to provide you with source diagrams that you can adapt for your own needs: [source diagrams](https://citrix.sharefile.com/d-sc3b40c906354b5ab).
