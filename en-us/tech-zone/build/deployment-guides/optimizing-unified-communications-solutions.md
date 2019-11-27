# Optimizing Unified Communications Solutions with Citrix Technologies

## Contributors

**Author:** [Allen Furmanski](mailto:allen.furmanski@citrix.com)


## Audience

This document is intended for Citrix technical professionals, IT decision-makers, partners, and consultants who wish to deliver unified communication solutions in a Citrix virtualized environment – whether on-premises or from a public cloud. The reader should have a basic understanding of Citrix app and desktop virtualization offerings as well as unified communications solutions. For more information on Citrix Virtual Apps and Desktops, refer to the [Citrix Virtual Apps and Desktops official documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops).


## Objective of this Document

The purpose of this document is to describe how to best deploy unified communication solutions with Citrix Virtual Apps and Desktops to deliver optimal user experience, improve security, and maximize server scalability.


## Introduction

Real-time collaboration is at the heart of the modern workplace. It’s how employees remain productive and business gets done. Whether a two-person internal voice call across town or an international video conference with a prospective customer hosting dozens of attendees, today’s unified communication solutions meet the demanding needs of any organization. Citrix virtual app and desktop solutions complement these offerings by:

- Keeping sensitive data like chat logs, file transfers, and SIP signaling secure within the confines of the data center as opposed to distributed across hundreds or thousands of endpoint devices.
- Providing a consistent user experience across various device types and platforms – even helping to enable capabilities on platforms without native client support.
- Easing the administrative burden. Instead of managing unified communications clients and versions across endpoints simply deploy Workspace app, embrace auto-updates, and BYO initiatives
- And much more…


Citrix has worked with the vendors of the below unified communications solutions to offer optimization packs. These packs help offload voice and video content to the endpoint device wherever possible while keeping the unified communication client secure in the data center (see the architecture section for more details).

- [Optimization for Microsoft Teams](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html)
- [HDX RealTime Optimization Pack for Skype for Business](https://docs.citrix.com/en-us/hdx-optimization/current-release.html)
- [Cisco Jabber Softphone for VDI](https://citrixready.citrix.com/cisco-systems-inc/cisco-jabber-vdi.html)
- [Avaya VDI Communicator](https://support.avaya.com/products/P0994/vdi-communicator/)
- [Zoom Meetings optimization](https://citrixready.citrix.com/zoom-video-communications/zoom-meetings.html)

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
