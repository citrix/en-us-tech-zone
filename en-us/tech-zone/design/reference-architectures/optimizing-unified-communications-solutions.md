---
layout: doc
description: Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.
---
# Optimizing Unified Communications Solutions with Citrix Technologies

## Contributors

**Author:** [Allen Furmanski](mailto:allen.furmanski@citrix.com)

**Special Thanks:** [Derek Thorslund](https://twitter.com/derektcitrix), [Fernando Klurfan](https://twitter.com/citrixferk)

## Audience

This document is intended for Citrix technical professionals, IT decision-makers, partners, and consultants who want to deliver unified communication solutions in a Citrix virtualized environment. The content is relevant with both on-premises and public cloud architectures. The reader should have a basic understanding of Citrix app and desktop virtualization offerings in addition to unified communications solutions. For more information on Citrix Virtual Apps and Desktops, refer to the [Citrix Virtual Apps and Desktops official documentation](/en-us/citrix-virtual-apps-desktops).

## Objective of this Document

The purpose of this document is to describe how to best deploy unified communication solutions with Citrix Virtual Apps and Desktops. The overall goal is to deliver an optimal user experience, improve security, and maximize server scalability.

## Introduction

Real-time collaboration is at the heart of the modern workplace. It’s how employees remain productive and business gets done. Whether a two-person internal voice call or an international video conference hosting dozens of attendees, today’s unified communication solutions meet the demanding needs of any organization. Citrix virtualization solutions complement these offerings by:

-  Keeping sensitive data like chat logs, file transfers, and SIP signaling secure within the data center as opposed to be distributed across hundreds or thousands of endpoint devices.
-  Providing a consistent user experience across various device types and platforms – even helping to enable capabilities on platforms without native client support.
-  Easing the administrative burden. Instead of managing unified communications clients and versions across endpoints simply deploy Workspace app, embrace auto-updates, and BYO initiatives
-  And much more…

Citrix has worked with the vendors of the following unified communications solutions to offer Optimization Packs. These packs help offload voice and video content to the endpoint device wherever possible. The unified communication client remains secure in the data center with this approach (see the architecture section for more details).

-  [Optimization for Microsoft Teams](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html)
-  [HDX RealTime Optimization Pack for Skype for Business](/en-us/hdx-optimization/current-release.html)
-  [Cisco Jabber Softphone for VDI](https://citrixready.citrix.com/cisco-systems-inc/cisco-jabber-vdi.html)
-  [Avaya Equinox VDI](https://support.avaya.com/products/P1706/avaya-equinox-vdi)
-  [Zoom Meetings optimization](https://citrixready.citrix.com/zoom-video-communications/zoom-meetings.html)

The preceding solutions ensure the best possible user experience and server scalability when used with supported Citrix versions and endpoint client devices. When the requirements are not met (such as connecting from an unsupported platform or client device) or using a different unified communications solution, a generic fallback approach can be used. This provides optimization of audio and video for sessions. We discuss this approach later in the document.

## Architecture

Most optimized unified communication solutions for Citrix environments employ an agent on the Citrix server/desktop to handle business logic, signaling, and other capabilities. A decoupled media engine resides on the endpoint device to process the audio and video. This approach reduces the hops that data packets would normally travel through in a virtualized environment.

[![Architecture of Unified Communications Solutions with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/media/reference-architectures_optimizing-unified-communications-solutions_001.png)](/en-us/tech-zone/design/media/reference-architectures_optimizing-unified-communications-solutions_001.png)

The following table provides the details of each officially supported unified communications solution including platform support and versions.
Acronyms used:
Browser Content Redirection (BCR)
Virtual Delivery Agent (VDA)
Citrix Workspace app (CWA)
RealTime Optimization Pack (RTOP)

|                                | Citrix Minimum Versions | Virtual App/Desktop Optimization | Web App Optimization              | Windows Support | Mac Support | Linux Support |
|--------------------------------|-------------------------|----------------------------------|-----------------------------------|-----------------|-------------|---------------|
| Microsoft Teams                | VDA 1906.2+  CWA 1907+    | Yes                              | Yes, with BCR (Windows CWA Only) | Yes             | Roadmap     | CWA 2004+       |
| Skype for Business             | RTOP 2.4+               | Yes                              | n/a                               | Yes             | Yes         | Yes           |
| [Cisco Jabber Softphone for VDI](https://software.cisco.com/download/home/284585947) | 7.15+                   | Yes. Cisco Plug-in 12.6+         | n/a                               | Yes             | No          | Yes           |
| [Cisco Webex Meetings](https://software.cisco.com/download/home/286304684/type/283802941/release/39.3(0))           | 7.15+                   | [Yes. Cisco Plug-in 40.4+](https://www.cisco.com/c/en/us/td/docs/collaboration/meeting_center/wvdi/wvdi-b-admin-guide/wvdi-b-admin-guide_chapter_010.html)         | [Yes with BCR](https://www.cisco.com/c/en/us/td/docs/collaboration/meeting_center/wvdi/wvdi-b-admin-guide/wvdi-b-admin-guide_chapter_01.html)                      | Yes             | No          | Yes           |
| [Cisco Webex Teams](https://www.cisco.com/c/en/us/td/docs/voice_ip_comm/cloudCollaboration/wbxt/vdi/wbx-teams-vdi-deployment-guide.html)              | 7.15+                   | [Yes. Cisco Plug-in.](https://www.webex.com/downloads/teams-vdi.html)                               | Yes with BCR                      | [Yes](https://binaries.webex.com/WebexTeamsDesktop-Windows-VDI-gold-Production/WebexTeamsVDIClient.msi)             | n/a         | n/a           |
| Zoom                           | 7.15+                   | [Yes. Zoom Plug-in](https://support.zoom.us/hc/en-us/articles/360031096531-Getting-Started-with-VDI)                | n/a                               | Yes             | No          | Yes           |
| Avaya One-X                    | 7.15+                   | Yes. One-X Plug-in               | n/a                               | Yes             | No          | No            |

The preceding table is subject to change at any time and may not necessarily contain the latest data. It also reflects the solutions that have been tested for optimization within Citrix environments. Other solutions can be optimized in a generic fashion as described in the following sections.

The Microsoft Teams and Skype for Business solutions use a media engine which is co-developed and co-supported between Citrix and Microsoft. For Teams it’s built into the VDA and Workspace app so no further components are required. For Skype for Business, there are separate components for agent (RealTime Connector on the VDA) and engine (RTME on the endpoint). They must be installed as part of the HDX RealTime Optimization Pack.

Jabber, WebEx, Zoom, and Avaya solutions use a similar agent/engine architecture as Microsoft solutions. However, those solutions are owned by their respective vendors. Consult the respective vendor’s website or Citrix Ready for more details on these solutions.

Cisco WebEx offerings in particular use Citrix’s Browser Content Redirection (BCR) for web app optimization. BCR redirects the viewport area of a web browser running on a Citrix VDA to the endpoint client device for rendering to improve user experience and server scalability. For more information on BCR, see the following section or refer to the [product documentation](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html).

When designing an optimized unified communications solution with Citrix, it is important to understand basic hardware and software requirements including potential extra loads presented to the environment. These are just a few of the questions that must be considered:

•  How many users use the unified communications solution with Citrix?
•  How the users connect to the environment?
•  Is the unified communications software available through published apps, desktops, or VDI?
•  What endpoint telephony hardware is used? (see [Citrix Ready](https://citrixready.citrix.com/) offerings)

## Optimizing Video

This section covers optimizing video for unified communications solutions. It typically applies in generic fallback scenarios such as unsupported endpoint client device or platform or unsupported unified communication solution. One such example is running the GoToMeeting collaboration offering within a virtual desktop.

Fallback scenarios result in server-side rendered video. In this case, Citrix recommends the following configuration for best performance:

-  The H.264 video codec should be used for video playback and it is enabled by default.
-  A Citrix endpoint client that supports GPU hardware acceleration is recommended. That includes the Citrix for Windows, Linux, Mac, and Chrome-OS. When using thin-client devices confirm with vendor if H.264 hardware decoding is supported, and which Citrix client version is used (if integrated with vendor image).  
-  Installing a GPU that supports H.264 hardware encoding/decoding on the Citrix server or VDI can also improve performance and save CPU cycles by offloading this process. There are scalability considerations when using a GPU in a multi-session server (Virtual Apps).
-  Webcam optimization - [CTX132764](https://support.citrix.com/article/CTX132764)

## Optimizing Audio

In this section we cover how to optimize audio for fallback scenarios. The Citrix recommendation is to enable UDP audio for the best overall experience. For audio quality, VOIP services work best with the “medium” setting. Playback is ideal at the “high” setting. For more details, refer to the [7.15 LTSR Audio Documentation](/en-us/xenapp-and-xendesktop/7-15-ltsr/multimedia/audio.html#audio-over-udp-real-time-transport-and-audio-udp-port-range). For additional information on optimizing audio, refer to the KB article [Delivering Softphones with Virtual Apps and Desktops](https://support.citrix.com/article/CTX133024).

## Generic USB

Generic USB redirection offers support for a wide range of USB devices within virtual sessions. However, given the nature of the USB standard and bandwidth it requires, it is typically only suited for LAN situations. For webcams, the recommendation is to not use this generic USB redirection capability because it consumes too much bandwidth in almost all situations. Unified communications hardware with specialty buttons can take advantage of composite USB redirection. This is also known as hybrid mode and is used to optimize multimedia virtual channels for voice and video while using generic USB redirection for specific functions. The functions are configurable by the admin. Refer to the **Composite USB Redirection** section at [https://support.citrix.com/article/CTX133024](https://support.citrix.com/article/CTX133024) for more details.

## Browser Content Redirection

Browser Content Redirection offloads the viewable “viewport” area of a VDA-based web browser to the endpoint device for rendering. This solution is ideal for optimizing any web-based unified communication offering, but especially true for those utilizing WebRTC. Refer to [Browser Content Redirection Documentation](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html) and [https://support.citrix.com/article/CTX230052](https://support.citrix.com/article/CTX230052) for more details on the feature including configuration and troubleshooting.

## Network Connectivity

Citrix SD-WAN is recommended to ensure optimal network connectivity and audio/video quality between office locations and the unified communications server. The [Citrix SD-WAN Cloud Direct service](/en-us/tech-zone/learn/tech-briefs/sdwan-cloud-direct.html) provides a great solution for connectivity to UCaaS solutions such as RingCentral, Cisco WebEx, GoToMeeting and Microsoft Teams. Citrix customers who run their workloads in public clouds can use the Citrix SD-WAN virtual appliance which is supported on Azure, AWS, GCP, and (in Tech Preview) Oracle Cloud. Microsoft Teams customers should refer to the documentation section [Citrix SD-WAN Optimized Network Connectivity for Microsoft Teams](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html#citrix-sd-wan-optimized-network-connectivity-for-microsoft-teams). For more information on SD-WAN, please refer to the [SD-WAN Documentation](/en-us/netscaler-sd-wan.html) and [SD-WAN Reference Architecture](/en-us/tech-zone/design/reference-architectures/sdwan.html).

## Monitoring

Ongoing monitoring of the optimized unified communications solution in a Citrix environment is important. Administrators should first understand if optimization is taking place, and what graphics modes and virtual channels are in use, bandwidth consumption, and so forth. Help desk staff should also have an understanding of tools and processes available to assess and troubleshoot as required.
For solutions with an Optimization Pack, the easiest way to check for optimization during a voice or video call is to observe resource utilization in task manager. With optimization enabled, the running process for the unified communication solution consumes less CPU when compared to running in a non-optimized state. For Browser Content Redirection, the processes are called HdxBrowserCef.exe and HdxBrowser.exe. More details can be found in the Browser Content Redirection troubleshooting guide at [https://support.citrix.com/article/CTX230052](https://support.citrix.com/article/CTX230052).

## Summary

Whichever unified communication solution customers decide to utilize within their organization, Citrix has a way to secure and optimize it with the Citrix Virtual Apps and Desktops family. Many popular solutions have specific Optimization Packs while others can use Citrix’s innovative Browser Content Redirection or generic fallback optimization techniques. Understanding the architecture and implementation specifics for a given environment helps to ensure the best possible user experience and scalability. Lastly, Citrix Director provides the necessary visibility for administrators and help desk staff to proactively analyze and troubleshoot unified communication optimization.

## Resources

-  [Optimization for Microsoft Teams](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html)
-  [https://support.citrix.com/article/CTX133024](https://support.citrix.com/article/CTX133024)
-  [Delivering Microsoft Skype for Business to XenApp and XenDesktop Users](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/delivering-microsoft-lync-to-xenapp-and-xendesktop-users.pdf)
-  [HDX RealTime Optimization Pack - What's New](/en-us/hdx-optimization/current-release/whats-new.html)
-  [https://support.citrix.com/article/CTX200279](https://support.citrix.com/article/CTX200279)
-  [https://support.zoom.us/hc/en-us/articles/360031096531-Getting-Started-with-VDI](https://support.zoom.us/hc/en-us/articles/360031096531-Getting-Started-with-VDI)
-  [https://help.webex.com/en-us/tx7gq6/The-Webex-Meetings-Virtual-Desktop-App](https://help.webex.com/en-us/tx7gq6/The-Webex-Meetings-Virtual-Desktop-App)
-  [https://help.webex.com/en-us/j3p7bp/Cisco-Jabber-and-Virtual-Desktop-Infrastructure](https://help.webex.com/en-us/j3p7bp/Cisco-Jabber-and-Virtual-Desktop-Infrastructure)
-  [Skype for Business Lab Guide from Synergy 2016](https://citrix.sharefile.com/share/view/7a7f3b27e2984e2a)

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

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Remote PC Access](/en-us/tech-zone/design/design-decisions/remote-pc-access.html) - When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> Single Server Scalability](/en-us/tech-zone/design/design-decisions/single-server-scalability.html) - Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Design Decisions -> VDI Model Comparison](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html) - Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Design Decisions -> Designing StoreFront and Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) - Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.

[Build -> Tech Papers -> Deploying Google Chrome](/en-us/tech-zone/build/tech-papers/google-chrome.html) - Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
