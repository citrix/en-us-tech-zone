---
layout: doc
description: App protection policies protect application data from attackers that have surreptitiously installed key loggers and/or screen capture tools while allowing companies to embrace BYOD, and extend their desktops or apps to remote workers, contractors, and gig economy workers.
---
# Title

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** [Martin Zugec](https://twitter.com/MartinZugec) and [Arvind SankaraSubramaniam]()

The end user is widely considered the weakest piece on the attack surface of an organization. It has become common practice for attackers to use sophisticated methods to fool users into installing malware on their endpoints. Once installed, the malware can silently collect and exfiltrate sensitive data such as user’s credentials, sensitive information, company’s intellectual property or confidential data. With the proliferation of BYO devices and access of corporate resources from unmanaged endpoints, the client endpoint becomes an even more exposed threat surface. With many users working from home, the risk to the organizations is heightened due to the untrustworthiness of the endpoint device.

When accessing a virtual app or virtual desktop session, the attack surface is drastically reduced, as the session is not running on the endpoint and users generally do not have permission to install apps within the virtual session. The data within the session is secure in the data center or cloud resource location. However, a compromised endpoint can capture session keystrokes and information displayed on the endpoint. Citrix provides administrators the ability prevent these attack vectors, using an add-on feature called App protection. The feature enables Citrix Virtual Apps and Desktops (CVAD) administrators to enforce policies on specific delivery group(s), such that when users connect to sessions from these delivery group(s), the user’s endpoint has either anti screen capture or anti-keylogging ot both enforced on the endpoints.

## Features

App protection policies, at the time of writing this brief (March 2020), protect client endpoints running Windows Desktop OSes (Windows 10, Windows 8.1 and Windows 7) and all versions of macOS that the Citrix Workspace app for Mac supports.

App protection policies work by controlling access to specific API calls of the underlying OS, required to capture screens or keyboard presses. App protection policies can therefore provide protection even against custom and purpose-built hacker tools. However, as OSes evolve, new ways of capturing screens and logging keys can emerge. While Citrix continues to identify and address, Citrix cannot guarantee full protection in specific configurations and deployments.

When a user logs into StoreFront, security capabilities of the endpoint are assessed and matched against available resources. Applications and desktops protected by App protection policies are visible only if an endpoint meets the security requirements (including a check if the App protection components are installed).



Since these features are based on the Citrix Workspace app, they protect not only the in session screens for Windows or MAC desktops/apps but also the Citrix Workspace app dialogs, which includes authentication screens.

## Anti-keylogging

Keyloggers are one of attackers’ favored ways of data exfiltration as they can remain on an infected machine without doing any noticeable damage. All keystrokes entered by the user are harvested, including username/password combinations, credit cards numbers, and confidential data. The harvested data is then silently exfiltrated later on.

With encryption, the app protection’s anti-keylogging garbles the text the user is typing for both physical and on-screen keyboards. The anti-keylogging feature encrypts the text before any keylogging tool can access it from the kernel/OS level. A keylogger installed on the client endpoint, reading the data from the OS/driver, would capture gibberish instead of the keystrokes the user is typing.

[![App_protection_policies_Anti_Keylogging](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_1-anti-keylogging-ss.png)](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_1-anti-keylogging-ss.png)

## Anti screen capture

Anti screen capture prevents an app from attempting to take a screenshot of or a recording of the screen within a virtual app or desktop session. The screen capture software would be unable to detect content within the capture region. The area selected by the app is grayed out or nothing is captured by the app instead of the screen section that it expects to copy. This applies to snip and sketch, snipping tool, Shift+Ctrl+Print Screen on Windows, screen capture / recording tools and meeting / web conferencing applications such as GoToMeeting and Teams, on both Windows and macOS.

[![App_protection_policies_Anti_screen_capture](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_2-anti-screen-capture-ss.png)](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_2-anti-screen-capture-ss.png)

The anti screen capture protection is active only when any of the protected apps are on screen. If any part of the window displaying the app is on screen then the policies disable screen captures.
If the user wants to take a screenshot of the client endpoint’s screen / other apps, while a protected app is running on the endpoint, then the user must minimize the protected app.

The protection extends to files from [Citrix Files](https://docs.citrix.com/en-us/mobile-productivity-apps/citrix-files.html) or any other connectors like Google Drive or Microsoft OneDrive that are accessed from within Citrix Workspace app. The apps will be protected from screen scrapers, as will be all the microapps and their notifications from within Workspace.

## Considerations

*  App protection policies are configured on the Desktop Delivery Controller. Administrators identify the delivery groups to be protected by applying the policy on them. Read more about configuration and compatibility in our [technical docs](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/secure/app-protection.html)

*  App protection policies require a minimum of Citrix Virtual Apps and Desktops version 1912 installed on Desktop Delivery Controller and StoreFront. App protection policies are implemented on the client end point and control data being passed from the Citrix Workspace app to the client’s OS, therefore there is no dependency on the state or version of the VDA installed in the session.

*  The required versions of the Citrix Workspace app are: Citrix Workspace app 1912 for Windows (LTSR) and later, Citrix Workspace app 2001 for Mac and later.  The HTML5 client is not supported.

*  If users connect from an older version than Citrix Workspace app 1912 for Windows or Citrix Workspace app 2001 for Mac, or from Citrix Receiver, then the tagged resources (to be protected) are not enumerated in the StoreFront.
App protection policies don’t work in conjunction with the Client Clipboard Redirection policies and the policies need to be disabled to ensure that App protection works.

As of writing this document (March 2020), app protection policies are unsupported in a double hop (nested) scenario or if a user is using Remote Desktop Protocol to access the Citrix session. For nested sessions, the first hop must be to a Windows desktop operating system and then the user must launch a second Citrix session from within it for the protections to apply.

## Summary

App protection policies can help protect application data from attackers that have surreptitiously installed either keyloggers or screen capture tools or both while allowing companies to embrace [BYOD](https://www.citrix.com/glossary/byod.html), and extend their desktops or apps to remote workers, contractors and gig economy workers.
