---
layout: doc
---
# Microsoft Teams optimization for Citrix Virtual Apps and Desktops environments

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** Fernando Klurfan

## Overview

This document serves as a guide to prepare an IT organization for successfully evaluating Unified Communications (UC) in desktop and application virtualization environments using Microsoft Teams. Over 500,000 organizations, including 91 of the Fortune 100 (as of Mar 2019)use Teams in 44 languages across 181 markets. Without proper consideration and design for optimization, virtual desktop and virtual application users will very likely find the Microsoft Teams experience to be subpar. Citrix provides technologies to optimize this experience, to make Teams more responsive with crisp video and audio, even when working remotely in a virtual desktop. However, with multiple combinations of Teams infrastructures, clients, endpoint types, and user locations one must find the right “recipe” to deliver Teams optimally.

The **Citrix® HDX™ Optimization for Microsoft® Teams** offers clear, crisp 720p high-definition video calls @30 fps, in an optimized architecture. Users can seamlessly participate in audio-video or audio-only calls to and from other Teams users, Optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.
This document will guide administrators in evaluating the Teams delivery solution in their Citrix environment. It contains best practices, tips and tricks to ensure that the deployment is the most robust.

## Optimized versus Generic delivery of Microsoft Teams

This is often what causes the most confusion about delivering a Microsoft Teams experience in a Citrix environment. The main reason is that without optimization the media must “hairpin” from your client to the server in the data center and then back to the endpoint. This can put significant load on the server (especially for video) and can cause delay and an overall degraded experience, especially if the other party in a Teams call is originating from a user in a similar virtualized experience. This method for delivering a Microsoft Teams experience is referred to as “Generic” delivery.

The preferred method of delivery is the “Optimized” method. In this case, the architect and/or administrator uses Optimization for Microsoft Teams in their environment. The “Optimized” method is like splitting the Teams client in two, as illustrated in the following comparison diagram. The user interface lives inside the virtual host, and is seen completely in the virtual desktop or application display. However, the media rendering, or media engine is separated off to run on the endpoint. This allows for a very rich rendering of the audio and video and a great desktop sharing experience.

[Optimized vs Fallback mode of delivery for Microsoft Teams](/en-us/tech-zone/build/media/deployment-guides_microsoft-teams-optimizations_1.png)

### Choosing the right Teams optimization for your environment

Optimization for Teams is not a “one size fits all” technology. For the Teams desktop app, with Windows clients, the Citrix HDX Optimization for Microsoft Teams with Citrix Workspace App is the way to go. With Linux and Mac clients being on the roadmap.
For Web based teams, with Windows and Linux clients using a Chrome browser, the Citrix HDX Optimization for Microsoft Teams with Browser Content Redirection would be the right solution.
For the remaining OS and Teams delivery format combinations, the generic delivery with the Fallback to Media over HDX is the option. Optimization for mobile OS’s is not available right now. Typically, mobile users who desire access to Teams on their devices will leverage Teams native apps from the appropriate app store. 

### Pros of using Citrix HDX Optimization for Microsoft Teams

-  Richest experience, all media rendered on endpoint
-  No hair-pinning effect, media communications go point to point between clients and the Teams conferencing service homed in Office 365
-  Less resource impact on the Citrix Virtual Apps and Desktops hosts
-  Less HDX bandwidth consumed over “generic” approach
-  Allows for use of high tech Teams optimized headsets and handsets
-  Supports delivery with Citrix Virtual Apps using Windows Server OS’s
-  Simple installation on client devices, minimal prerequisites
-  Can be used remotely from the enterprise network in conjunction with Office 365 
-  Wide choice of supported HDX Premium thin client devices (see [Citrix Ready list](https://citrixready.citrix.com/category-results.html?&f1=endpoints&f2=thin-clients&f3=unique-proposition/ms-teams-optimized&lang=en_us))
-  Support provided by both Microsoft and Citrix support
-  No requirement for both the sides of the optimized architecture to authenticate to the backend
-  Requires no modification to the Teams back end


## Citrix HDX Optimization for Microsoft Teams

These components are by default, bundled into Citrix Workspace app and the Virtual Delivery Agent (VDA)

### Architecture





