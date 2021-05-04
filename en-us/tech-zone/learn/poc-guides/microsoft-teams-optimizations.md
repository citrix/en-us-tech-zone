---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Fernando Klurfan
description: Learn how to deliver the Citrix HDX Optimization for Microsoft Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.
tz_title: Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments
tz_products: citrix-virtual-apps-and-desktops;
---
# PoC Guide: Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments

## Overview

This document serves as a guide to prepare an IT organization for successfully evaluating Unified Communications (UC) in desktop and application virtualization environments using Microsoft Teams. Over 500,000 organizations, including 91 of the Fortune 100 (as of Mar 2019) use Teams in 44 languages across 181 markets. Without proper consideration and design for optimization, virtual desktop and virtual application users will likely find the Microsoft Teams experience to be subpar. Citrix provides technologies to optimize this experience, to make Teams more responsive with crisp video and audio, even when working remotely in a virtual desktop. However, with multiple combinations of Teams infrastructures, clients, endpoint types, and user locations one must find the right “recipe” to deliver Teams optimally.

The **Citrix® HDX™ Optimization for Microsoft® Teams** offers clear, crisp 720p high-definition video calls @30 fps, in an optimized architecture. Users can seamlessly participate in audio-video or audio-only calls to and from other Teams users, Optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.
This document guides administrators in evaluating the Teams delivery solution in their Citrix environment. It contains best practices, tips, and tricks to ensure that the deployment is the most robust.

## Optimized versus Generic delivery of Microsoft Teams

This choice is often what causes the most confusion about delivering a Microsoft Teams experience in a Citrix environment. The main reason is that without optimization the media must “hairpin” from your client to the server in the data center and then back to the endpoint. This additional traffic can put significant load on the server (especially for video) and can cause delay and an overall degraded experience, especially if the other party in a Teams call is originating from a user in a similar virtualized experience. This method for delivering a Microsoft Teams experience is referred to as “Generic” delivery.

The preferred method of delivery is the “Optimized” method. In this case, the architect or administrator uses Optimization for Microsoft Teams in their environment. The “Optimized” method is like splitting the Teams client in two, as illustrated in the following comparison diagram. The user interface lives inside the virtual host, and is seen completely in the virtual desktop or application display. However, the media rendering, or media engine is separated off to run on the endpoint. This method allows for an exquisite rendering of the audio and video and a great desktop sharing experience.

![Optimized vs Fallback mode of delivery for Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_microsoft-teams-optimizations_1.png)

### Choosing the right Teams optimization for your environment

Optimization for Teams is not a “one size fits all” technology. For the Teams desktop app, with Windows clients, the Citrix HDX Optimization for Microsoft Teams with Citrix Workspace app is the way to go. With Linux and Mac clients being on the roadmap.
For Web based teams, with Windows and Linux clients using a Chrome browser, the Citrix HDX Optimization for Microsoft Teams with Browser Content Redirection would be the right solution.
For the remaining OS and Teams delivery format combinations, the generic delivery with the Fallback to Media over HDX is the option. Optimization for mobile OSs is not available right now. Typically, mobile users who desire access to Teams on their devices use Teams native apps from the appropriate app store.

### Pros of using Citrix HDX Optimization for Microsoft Teams

-  Richest experience, all media rendered on endpoint
-  No hair-pinning effect, media communications go point to point between clients and the Teams conferencing service homed in Office 365
-  Less resource impact on the Citrix Virtual Apps and Desktops hosts
-  Less HDX bandwidth consumed over “generic” approach
-  Allows for use of high tech Teams optimized headsets and handsets
-  Supports delivery with Citrix Virtual Apps using Windows Server OSs
-  Simple installation on client devices, minimal prerequisites
-  Can be used remotely from the enterprise network with Office 365
-  Wide choice of supported HDX Premium thin client devices (see [Citrix Ready list](https://citrixready.citrix.com/category-results.html?&f1=endpoints&f2=thin-clients&f3=unique-proposition/ms-teams-optimized&lang=en_us))
-  Support provided by both Microsoft and Citrix support
-  No requirement for both the sides of the optimized architecture to authenticate to the back-end
-  Requires no modification to the Teams back end

## Citrix HDX Optimization for Microsoft Teams

These components are by default, bundled into Citrix Workspace app and the Virtual Delivery Agent (VDA)

### Conceptual Architecture

[![Teams Optimization for Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/media/poc-guides_microsoft-teams-optimizations_2.png)](/en-us/tech-zone/learn/media/poc-guides_microsoft-teams-optimizations_2.png)

### Call Flow

1.  Launch Microsoft Teams.
1.  Teams authenticates to O365. Tenant policies are pushed down to the Teams client, and relevant TURN and signaling channel information is relayed to the app.
1.  Teams detects that it is running in a VDA and makes API calls to the Citrix JavaScript API.
1.  Citrix JavaScript in Teams opens a secure WebSocket connection to WebSocketService.exe running on the VDA (127.0.0.1:9002). WebSocketService.exe runs as a Local System account on session 0. WebSocketService.exe performs TLS termination and user session mapping, and spawns WebSocketAgent.exe, which now runs inside the user session.
1.  WebSocketAgent.exe instantiates a generic virtual channel by calling into the Citrix HDX Browser Redirection Service (CtxSvcHost.exe).
1.  Citrix Workspace app’s wfica32.exe (HDX engine) spawns a new process called HdxTeams.exe, which is the new WebRTC engine used for Teams optimization.
1.  HdxTeams.exe and Teams.exe have a 2-way virtual channel path and can start processing multimedia requests.

    —–User calls——

1.  Peer A clicks the call button. Teams.exe communicates with the Teams services in Azure establishing an end-to-end signaling path with Peer B. Teams asks HdxTeams for a series of supported call parameters (codecs, resolutions, and so forth, which is known as a Session Description Protocol (SDP) offer). These call parameters are then relayed using the signaling path to the Teams services in Azure and from there to the other peer.
1.  The SDP offer/answer (single-pass negotiation) and the Interactive Connectivity Establishment (ICE) connectivity checks (NAT and Firewall traversal using Session Traversal Utilities for NAT (STUN) bind requests) complete. Then, Secure Real-time Transport Protocol (SRTP) media flows directly between HdxTeams.exe and the other peer (or O365 conference servers if it is a Meeting).

### OS versions supported by the Optimization for Teams with Microsoft Teams desktop app

-  VM hosting Teams client – Install Citrix Virtual Delivery Agent (VDA) version 1906.2 or higher
    -  Single session OS - Microsoft Windows 10 64-bit, minimum version 1607 up to 1909
    -  Multi-session OS - Microsoft Windows Server 2019, 2016, 2012 R2 (Standard and Datacenter Editions)

-  Windows client machine - Install Citrix Workspace app 1907 for Windows or higher
    -  Microsoft Windows 10, 8, 7 (32-bit & 64-bit editions, including Embedded editions 2016 LTSR or 2019 LTSC)

### Supported Teams headsets and handsets

The list of devices that are supported by Microsoft for [Teams](https://products.office.com/en-us/microsoft-teams/across-devices/devices) and [Skype for Business](https://docs.microsoft.com/en-us/skypeforbusiness/certification/devices-usb-devices)

Note: Microsoft Teams does not support iPhone headsets

## Installation Steps

### Prerequisites

Note - The Optimization for Teams at GA only applies to Windows Endpoints

1.  Download the latest Citrix Virtual Apps and Desktops VDA installer. On Citrix.com, select the Downloads Tab. Select Citrix Virtual Apps and Desktops as the product and select Product Software as the download type. Select Citrix Virtual Apps and Desktops 1906 or later, it is under Components
1.  Ensure that the Teams service is reachable, from the client in addition to the VDA
1.  Ensure Microsoft Teams client version 1.2.00.31357 or higher is installed in the Virtual Delivery Agent hosts or base image or on Citrix Virtual Apps servers, which will be used to deliver Microsoft Teams or on both. See instructions on how to install it below
1.  Download the latest Citrix Workspace app. [Link](https://www.citrix.com/downloads/workspace-app/windows/workspace-app-for-windows-latest.html)

The installation procedures are simple

## Microsoft Teams install

The installation must be done on the golden image of your catalog or in the office layer (if you are using App Layering). We recommend you follow the Microsoft Teams installation guidelines. Avoid installing Teams under AppData. Instead, install in **C:\Program Files** by using the **ALLUSER=1** flag.
For more information, see [Install Microsoft Teams using MSI](https://docs.microsoft.com/en-us/MicrosoftTeams/msi-deployment#vdi-installation)

If Teams was installed in user mode before on the image:

-  Users from EXE installer:
    -  Have all users in the environment manually uninstall from **Control Panel > Programs & Features**
-  Admin from MSI:
    -  Admin uninstalls in the normal way
    -  All users in the environment must sign in for uninstallation to be completed
-  Admin from Office Pro Plus:
    -  Admin may need to uninstall as if MSI were directly installed (above)
    -  Office Pro Plus must be configured to not include Teams

## Citrix Virtual Apps and Desktops VDA install on the host virtual machines

The HDX Optimization for Teams is bundled as part of VDA in Citrix Virtual Apps and Desktops. It is installed on the hosts or base image of the catalog and Citrix Virtual Apps servers, which may be used to deliver Teams.

### Application requirements

The VDA installer automatically installs the following items, which are available on the Citrix installation media in the Support folders

-  Microsoft .NET Framework 4.7.1 or later, if it is not already installed
-  Microsoft Visual C++ 2013 and 2015 Runtimes, 32-bit and 64-bit
-  BCR_x64.msi - the MSI that contains the Microsoft Teams optimization code and starts automatically from the GUI. If you’re using the command line interface for the VDA installation, don’t exclude it

For Windows Server, if you did not install and enable the Remote Desktop Services roles, the installer automatically installs and enables those roles.

3 GB of free disk space for each user profile (recommended by Microsoft)

Ensure that the Microsoft Teams client application is installed in per-machine mode on the VDA

**Install the Citrix Virtual Delivery Agent** on the host or base image, following the instructions [here](/en-us/citrix-virtual-apps-desktops/install-configure/install-vdas.html)

Using this image, create the appropriate machine catalogs and delivery groups in **Citrix Studio / Citrix Cloud Manage** tab before trying to establish sessions and accessing the Teams client.

## Windows client device – Citrix Workspace app 1909 for Windows install

The Citrix Workspace app 1909 for Windows has the optimization components built into it. When you install the application on your client, the components are already present.

### System Requirements

-  Approximately 1.8–2.0 GHz quad core CPU required for 720p HD resolution during a peer-to-peer video conference call. Quad core CPUs with lower speeds (~1.5 GHz) but equipped with Intel Turbo Boost or AMD Turbo Core that can boost up to 2.0 GHz are also supported
-  Citrix Workspace app requires a minimum of 600 MB free disk space and 1 GB RAM.
-  Microsoft .NET Framework version 4.6.2 or later is installed automatically, if it is not already installed.

Follow the instructions, to install the Citrix Workspace app for Windows [here](/en-us/citrix-workspace-app-for-windows/install.html#using-a-windows-based-installer)

## Policy Settings

To enable optimization, ensure the **Microsoft Teams redirection** Studio policy is set to **Allowed**
The policy is enabled by default

![Studio Policy to enable Teams optimization](/en-us/tech-zone/learn/media/poc-guides_microsoft-teams-optimizations_3.png)

**Note**: In addition to this policy being enabled, HDX checks to verify that the version of Citrix Workspace app is equal to or greater than the minimum required version. If both conditions are met, the following registry key is set to 1 on the VDA. The Microsoft Teams application reads the key to load in VDI mode

Key: **HKEY_CURRENT_USER\Software\Citrix\HDXMediaStream**

Name: **MSTeamsRedirSupport**

Value: DWORD (1 - on, 0 - off)

## Network Requirements

Microsoft Teams relies on Media Processor servers in Microsoft Azure for meetings or multiparty calls. Microsoft Teams relies on Azure Transport Relays for scenarios where two peers in a point-to-point call do not have direct connectivity or where a participant does not have direct connectivity to the Media Processor. Therefore, the network health between the peer and the Office 365 cloud determines the performance of the call.

We recommend evaluating your environment to identify any risks and requirements that can influence your overall cloud voice and video deployment. Use the [Prepare your organization’s network for Microsoft Teams](https://aka.ms/PerformanceRequirements) page to evaluate if your network is ready for Microsoft Teams.

### Port / Firewall settings

Teams traffic flows via Transport Relay on UDP 3478-3481, TCP 443 (fallback) and the clients need access to these address ranges: 13.107.64.0/18, 52.112.0.0/14, 52.120.0.0/14.

Optimized traffic for peer to peer connections is routed on higher ports (40 K+ UDP) at random, if they are open. For more info [read](https://docs.microsoft.com/en-us/office365/enterprise/urls-and-ip-address-ranges#skype-for-business-online-and-microsoft-teams).

Be sure that all computers running the Workspace app client with Teams optimization can resolve external DNS queries to discover the TURN/STUN services provided by Microsoft 365 (for example, `worldaz.turn.teams.microsoft.com`) and that your firewalls are not preventing access.

For support information, see [Support](/en-us/citrix-virtual-apps-desktops/multimedia/opt-for-ms-teams/teams-monitor-ts-support.html#support) section of our documentation.

### Summary of key network recommendations for Real Time Protocol (RTP) traffic

Connect to the Office 365 network as directly as possible from the branch office. Bypass proxy servers, network SSL intercept, deep packet inspection devices, and VPN hairpins (use split tunneling if possible) at the branch office. If you must use them, make sure RTP/UDP Teams traffic is unhindered. Plan and provide enough bandwidth. Check each branch office for network connectivity and quality.
The WebRTC media engine in Workspace app (HdxTeams.exe) uses the Secure RTP protocol for multimedia streams that are offloaded to the client. The following metrics are recommended for guaranteeing a great user experience

-  Latency (one way) < 50 milliseconds
-  Latency (RTT) < 100 milliseconds
-  Packet Loss < 1% during any 15 second interval
-  Packet inter-arrival jitter < 30 ms during any 15 second interval

In terms of bandwidth requirements, optimization for Microsoft Teams can use a wide variety of codecs for audio (OPUS/G.722/PCM/G711) and video (H264/VP9). The peers negotiate these codecs during the call establishment process using the Session Description Protocol (SDP) Offer/Answer.

Citrix minimum recommendations for bandwidth and codes for specific type of content are:

-  Audio (each way) ~90 kbps using G.722
-  Audio (each way) ~60 kbps using Opus*
-  Video (each way) ~700 kbps using H264 360p @ 30 fps and 16:9
-  Video (each way) ~2500 kbps using H264 720p @ 30 fps and 16:9
-  Screen sharing ~300 kbps using H264 1080p @ 15 fps

(*) Opus supports constant and variable bitrate encoding from 6 kbps up to 510 kbps, and it is the preferred codec for Peer to Peer calls between two VDI users

## Common deployment related tips and questions

### Team Tips

To update the Teams desktop client, Uninstall the currently installed version, then install the new version.

To uninstall the Teams desktop client MSI, if it was first installed using the per-machine mode, use one of the following commands:

msiexec /passive /x Teams_windows_x64.msi /l*v msi_uninstall_x64.log

msiexec /passive /x Teams_windows.msi /l*v msi_uninstall.log

### Screen sharing

Microsoft Teams relies on video-based screen sharing (VBSS), effectively encoding the desktop being shared with video codecs like H264 and creating a high-definition stream.
With HDX optimization, incoming screen sharing is treated as a video stream, therefore if you are in the middle of a video call and the other peer starts to share their desktop, their camera video feed is paused and instead the screen sharing video feed is displayed. The peer must then manually resume their camera sharing.

### Multi-monitor

In cases where CDViewer is in full screen mode and spanning across multi-monitor setups, only the primary monitor is shared. Users must drag the application of interest inside the virtual desktop to the primary monitor for it to be seen by the other peer on the call.

## Troubleshooting

Here are a few ways to resolve the issues users may face:

**Symptom**: Installation Failure

**Cause**: Inconsistent state of Citrix redirection services

**Resolution**: Validate the following:

1.  Teams automatically launches for all users after sign-in to Windows
1.  Existence of directories and files
    -  Program Files (x86) or Program Files
        -  Microsoft\Teams\current folder with Teams.exe, which is main application
        -  Teams Installer folder with Teams.exe, which is EXE installer (do not ever run this manually!)
    -  %LOCALAPPDATA%
        -  Microsoft\Teams is either not there, or mostly empty (only a couple of files)
1.  Existence of shortcuts:
    -  Teams desktop client shortcut, pointing to Program Files…, in following places:
        -  On desktop
        -  In Start menu
1.  Existence of Windows Registry information:
    -  A value named Teams, of type REG_SZ, in one of the following key paths in registry:
        -  Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion\Run
        -  Computer\HKEY_LOCAL_MACHINE\Microsoft\Windows\CurrentVersion\Run

**Symptom**: Failure while placing an audio/video call and cannot find the audio/video devices connected

**Cause**: Inconsistent state of Citrix redirection services

**Resolution**: Validate that the HDXTeams.exe process is running on the VDA. If the process is not running then we need to restart Citrix Redirection Services, do the following - in this order - to check if HdxTeams.exe is getting launched

-  Exit Teams on VDA
-  Start services.msc on VDA
-  Stop "Citrix HDX Teams Redirection Service"
-  Disconnect the HDX session
-  Reconnect to the HDX session
-  Start "Citrix HDX Teams Redirection Service"
-  Restart "Citrix HDX HTML5 Video Redirection Service"
-  Launch Teams on VDA

**Symptom**: No incoming ring notification tone on a Citrix Session

**Cause**: Audio being played on the VDA host

**Resolution**: No audio devices on Citrix session / incorrect local default audio device

-  Make sure that a remote audio device is present on the Citrix session.
-  Make sure that Citrix Redirection service is running on remote host. Restart it (solves most problems).
-  In case multiple audio sources are available, make sure that the default playback device on the client machine is selected to the device where the user expects to hear the ring notification.

## Summary

We support Microsoft Teams infrastructures: whether on-prem or Office 365 (cloud) as long as configuration allows for successful internal and external client communication.

We have walked through the way to go about evaluating the Citrix Optimization for Teams and pointed you to the resources for deploying the rest. The Optimization for Microsoft Teams greatly increases server scalability and offers zero degradation in audio-video quality and optimal network bandwidth efficiency. It is the Microsoft recommended solution for a VDI deployment. If there are Linux clients in your environment, then it’s the only solution, that Microsoft and Citrix, jointly support.
