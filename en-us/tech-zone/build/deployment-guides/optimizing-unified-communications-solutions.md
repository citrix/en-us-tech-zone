# Optimizing Unified Communications Solutions with Citrix Technologies

## Contributors

**Author:** [Allen Furmanski](mailto:allen.furmanski@citrix.com)

## Audience

This document is intended for Citrix technical professionals, IT decision-makers, partners, and consultants who wish to deliver unified communication solutions in a Citrix virtualized environment – whether on-premises or from a public cloud. The reader should have a basic understanding of Citrix app and desktop virtualization offerings as well as unified communications solutions. For more information on Citrix Virtual Apps and Desktops, refer to the [Citrix Virtual Apps and Desktops official documentation](/en-us/citrix-virtual-apps-desktops).

## Objective of this Document

The purpose of this document is to describe how to best deploy unified communication solutions with Citrix Virtual Apps and Desktops to deliver optimal user experience, improve security, and maximize server scalability.

## Introduction

Real-time collaboration is at the heart of the modern workplace. It’s how employees remain productive and business gets done. Whether a two-person internal voice call across town or an international video conference with a prospective customer hosting dozens of attendees, today’s unified communication solutions meet the demanding needs of any organization. Citrix virtual app and desktop solutions complement these offerings by:

-  Keeping sensitive data like chat logs, file transfers, and SIP signaling secure within the confines of the data center as opposed to distributed across hundreds or thousands of endpoint devices.
-  Providing a consistent user experience across various device types and platforms – even helping to enable capabilities on platforms without native client support.
-  Easing the administrative burden. Instead of managing unified communications clients and versions across endpoints simply deploy Workspace app, embrace auto-updates, and BYO initiatives
-  And much more…

Citrix has worked with the vendors of the below unified communications solutions to offer optimization packs. These packs help offload voice and video content to the endpoint device wherever possible while keeping the unified communication client secure in the data center (see the architecture section for more details).

-  [Optimization for Microsoft Teams](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html)
-  [HDX RealTime Optimization Pack for Skype for Business](/en-us/hdx-optimization/current-release.html)
-  [Cisco Jabber Softphone for VDI](https://citrixready.citrix.com/cisco-systems-inc/cisco-jabber-vdi.html)
-  [Avaya VDI Communicator](https://support.avaya.com/products/P0994/vdi-communicator/)
-  [Zoom Meetings optimization](https://citrixready.citrix.com/zoom-video-communications/zoom-meetings.html)

The above solutions ensure the best possible user experience and server scalability when used with supported Citrix versions and endpoint client devices. When the requirements are not met with these solutions (e.g. connecting from an unsupported platform or client device) or a different unified communications solution is in use, a generic fallback approach may be used to optimize audio and video. We will discuss this later in the document.

## Architecture

Most optimized unified communication solutions for Citrix environments employ an agent on the Citrix server/desktop to handle business logic, signaling, etc. and a decoupled media engine on the endpoint device to process the audio and video. This approach reduces the hops that data packets would normally travel through in a virtualized environment.

(diagram placeholder)

The below table provides the details of each officially supported unified communications solution including platform support and versions.
Acronyms used:
BCR = Browser Content Redirection
VDA = Virtual Delivery Agent
CWA = Citrix Workspace App
RTOP = RealTime Optimization Pack

|                                | Citrix Minimum Versions | Virtual App/Desktop Optimization | Web App Optimization              | Windows Support | Mac Support | Linux Support |
|--------------------------------|-------------------------|----------------------------------|-----------------------------------|-----------------|-------------|---------------|
| Microsoft Teams                | VDA 1906+  CWA 1909+    | Yes                              | Yes,with BCR   (Windows CWA Only) | Yes             | Roadmap     | Roadmap       |
| Skype for Business             | RTOP 2.4+               | Yes                              | n/a                               | Yes             | Yes         | Yes           |
| Cisco Jabber Softphone for VDI | 7.15+                   | Yes. Cisco plug-in 12.6+         | n/a                               | Yes             | No          | Yes           |
| Cisco Webex Meetings           | 7.15+                   | Yes. Cisco Plug-in 39.3+         | Yes with BCR                      | Yes             | No          | Yes           |
| Cisco Webex Teams              | 7.15+                   | No                               | Yes with BCR                      | n/a             | n/a         | n/a           |
| Zoom                           | 7.15+                   | Yes. Zoom plug-in                | n/a                               | Yes             | No          | Yes           |
| Avaya One-X                    | 7.15+                   | Yes. One-X plug-in               | n/a                               | Yes             | No          | No            |

Note that the table above is subject to change at any time and may not necessarily contain the latest data. It also reflects the solutions that have been tested for optimization within Citrix environments. Other solutions may be optimized in a generic fashion as described in the sections below.

The Microsoft Teams and Skype for Business solutions use a media engine which is co-developed and co-supported between Citrix and Microsoft. For Teams it’s built into the VDA and Workspace app so no additional components are required. For Skype for Business, there are separate agent (RealTime Connector on the VDA) and engine (RTME on the endpoint) components that must be installed as part of the HDX RealTime Optimization Pack.

Jabber, WebEx, Zoom, and Avaya solutions use a similar agent/engine architecture as Microsoft solutions; however, those solutions are owned by their respective vendors. Consult the respective vendor’s web site and/or Citrix Ready for additional details on these solutions.

Cisco WebEx offerings in particular make use of Citrix’s Browser Content Redirection (BCR) functionality for web app optimization. BCR redirects the viewport area of a web browser running on a Citrix VDA to the endpoint client device for rendering to improve user experience and server scalability. For more information on BCR, see the section below and/or refer to the [product documentation](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html).

When designing an optimized unified communications solution with Citrix, it is important to understand basic hardware and software requirements including potential additional loads presented to the environment. These are just a few of the questions that must be considered:

•  How many users will leverage the unified communications solution with Citrix?
•  How will the users connect to the environment?
•  Will the unified communications software be available through published apps, desktops, and/or VDI?
•  What endpoint telephony hardware will be used? (see [Citrix Ready](https://citrixready.citrix.com/) offerings)

## Optimizing Video

This section will cover optimizing video for unified communications solutions and typically applies in generic fallback scenarios (e.g. Unsupported endpoint client device or platform and/or unsupported unified communication solution). One such example is running the GoToMeeting collaboration offering within a virtual desktop.

Fallback scenarios will result in server-side rendered video. In this case, Citrix recommends the following configuration for best performance:

-  The H.264 video codec should be used for video playback and it is enabled by default.
-  A Citrix endpoint client that supports GPU hardware acceleration is recommended. That includes the Citrix Workspace App for Windows, Linux, Mac, and Chrome-OS. When using thin-client devices confirm with vendor if H.264 hardware decoding is supported, and which Citrix client version is used (if integrated with vendor image).  
-  Installing a GPU that supports H.264 hardware encoding/decoding on the Citrix server or VDI can also improve performance and save CPU cycles by offloading this process. There are scalability considerations when using a GPU in a multi-session server (Virtual Apps).
-  Webcam optimization - [CTX132764](https://support.citrix.com/article/CTX132764)

## Optimizing Audio

In this section we will cover how to optimize audio for fallback scenarios. The Citrix recommendation is to enable UDP audio for the best overall experience. For audio quality, VOIP services work best with the “medium” setting. Playback is ideal at the “high” setting. For more details, refer to the [7.15 LTSR Audio Documentation](/en-us/xenapp-and-xendesktop/7-15-ltsr/multimedia/audio.html#audio-over-udp-real-time-transport-and-audio-udp-port-range).

## Generic USB

Generic USB redirection offers support for a wide range of USB devices within virtual sessions; however, given the nature of the USB standard and bandwidth it requires, it is typically only suited for LAN situations. For webcams, the recommendation is to not use this generic USB redirection capability because it consumes too much bandwidth in almost all situations. Unified communications hardware with specialty buttons can take advantage of composite USB redirection (also known as hybrid mode) to leverage optimized multimedia virtual channels for voice and video while using generic USB redirection only for specific functions configured by the admin. Refer to the Composite USB Redirection section at [https://support.citrix.com/article/CTX133024](https://support.citrix.com/article/CTX133024) for additional details.

## Browser Content Redirection

Browser Content Redirection offloads the viewable “viewport” area of a VDA-based web browser to the endpoint device for rendering. This solution is ideal for optimizing any web-based unified communication offering, but especially true for those utilizing WebRTC. Refer to [Browser Content Redirection Documentation](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html) and [https://support.citrix.com/article/CTX230052](https://support.citrix.com/article/CTX230052) for additional details on the feature including configuration and troubleshooting.

## Monitoring

Ongoing monitoring of the optimized unified communications solution in a Citrix environment is important. Administrators should first understand if optimization is taking place, as well as what graphics modes and virtual channels are in use, bandwidth consumption, etc. Help desk staff should also have an understanding of tools and processes available to assess and troubleshoot as required.
For solutions that offer an optimization pack, the easiest way to check if optimization is taking place during a voice or video call is to simply observe the resource utilization in task manager. With optimization enabled, the running process for the unified communication solution will consume significantly less CPU when compared to running in a non-optimized state.   For Browser Content Redirection, the processes are called HdxBrowserCef.exe and HdxBrowser.exe. Additional details can be found in the Browser Content Redirection troubleshooting guide at [https://support.citrix.com/article/CTX230052](https://support.citrix.com/article/CTX230052).

(additional monitoring content)

## Summary

Whichever unified communication solution customers decide to utilize within their organization, Citrix has a way to secure and optimize it with the Citrix Virtual Apps and Desktops family. Many popular solutions have specific optimization packs while others leverage Citrix’s innovative Browser Content Redirection or generic fallback optimization techniques. Understanding the architecture and implementation specifics for a given environment will help to ensure the best possible user experience and scalability. Lastly, Citrix Director provides the necessary visibility for administrators and help desk staff to proactively analyze and troubleshoot unified communication optimization.

## Resources

-  [https://support.citrix.com/article/CTX133024](https://support.citrix.com/article/CTX133024)
-  [https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/delivering-microsoft-lync-to-xenapp-and-xendesktop-users.pdf](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/delivering-microsoft-lync-to-xenapp-and-xendesktop-users.pdf)
-  [/en-us/hdx-optimization/current-release/whats-new.html](/en-us/hdx-optimization/current-release/whats-new.html)
-  [https://support.citrix.com/article/CTX200279](https://support.citrix.com/article/CTX200279)
-  [https://support.zoom.us/hc/en-us/articles/360031096531-Getting-Started-with-VDI](https://support.zoom.us/hc/en-us/articles/360031096531-Getting-Started-with-VDI)
-  [https://help.webex.com/en-us/tx7gq6/The-Webex-Meetings-Virtual-Desktop-App](https://help.webex.com/en-us/tx7gq6/The-Webex-Meetings-Virtual-Desktop-App)
-  [https://help.webex.com/en-us/j3p7bp/Cisco-Jabber-and-Virtual-Desktop-Infrastructure](https://help.webex.com/en-us/j3p7bp/Cisco-Jabber-and-Virtual-Desktop-Infrastructure)
-  [Skype for Business Lab Guide from Synergy 2016](https://citrix.sharefile.com/share/view/7a7f3b27e2984e2a)
