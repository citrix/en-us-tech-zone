---
layout: doc
h3InToc: true
contributedBy: Martin Zugec, Mayank Singh
description: App protection policies protect application data from attacks based on keyloggers and screen capture tools. Allow companies to embrace BYOD, and extend resources to remote workers, contractors, and gig economy workers.
tz_title: App protection
tz_products: security;
---
# Tech Brief: App protection

In recent years, we have seen an increase in advance persistent threat (APT) attacks in all industries. Instead of a typical select & run attack (for example ransomware), the goal for attackers is to stay inside your network and remain undetected for an extended period. According to research from FireEye, mean dwell-time for 2018 in the Americas is 71 days, EMEA is 177 days, and APAC is 204 days. [2019 Data Breach Investigations Report](https://enterprise.verizon.com/resources/reports/dbir/) concludes that most security breaches took months or longer to discover. This gives attackers a significant time to proceed with their attack, propagate, harvest sensitive data and exfiltrate it before they are discovered (or if at all).

The end user is widely considered the weakest piece on the attack surface of an organization. It has become common practice for attackers to use sophisticated methods to fool users into installing malware on their endpoints. Once installed, the malware can silently collect and exfiltrate sensitive data such as user’s credentials, sensitive information, company’s intellectual property, or confidential data. With the increase in BYO devices and access of corporate resources from unmanaged endpoints, endpoints become an even more exposed threat surface. With many users working from home, the risk to the organizations is heightened due to the untrustworthiness of the endpoint device.

With the use of virtual apps and desktops, an attack surface of endpoints has been greatly reduced - data is stored centrally in a data center and it is much harder for the attacker to steal it. The virtual session is not running on the endpoint and users generally do not have permission to install apps within the virtual session. The data within the session is secure in the data center or cloud resource location. However, a compromised endpoint can capture session keystrokes and information displayed on the endpoint. Citrix provides administrators the ability prevent these attack vectors, using an add-on feature called App protection. The feature enables Citrix Virtual Apps and Desktops (CVAD) administrators to enforce policies specifically on one or more delivery groups. When users connect to sessions from these delivery groups, the user’s endpoint has either anti screen capture or anti-keylogging or both enforced on the endpoints.

## Anti-keylogging

A keylogger is one of the attacker’s favored tools of data exfiltration, as it can remain on an infected machine without doing any noticeable damage. All the keystrokes entered by the user are harvested, including user name / password combinations, credit cards numbers, and confidential data. The harvested data then silently exfiltrates later on. Spyware/keyloggers is commonly used by attackers - it is one of the top 3 malware varieties that are present in security breaches.

With encryption, the app protection’s anti-keylogging garbles the text the user is typing for both physical and on-screen keyboards. The anti-keylogging feature encrypts the text before any keylogging tool can access it from the kernel/OS level. A keylogger installed on the client endpoint, reading the data from the OS/driver, would capture gibberish instead of the keystrokes the user is typing.

App protection policies are active not only for published applications and desktops, but for Citrix Workspace authentication dialogs as well. Your Citrix Workspace is protected from the moment when your users open the first authentication dialog.

[![Citrix App protection policies anti keylogging](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_anti-keylogging.gif)](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_anti-keylogging.gif)

## Anti screen capture

Anti screen capture prevents an app from attempting to take a screenshot of or a recording of the screen within a virtual app or desktop session. The screen capture software would be unable to detect content within the capture region. The area selected by the app is grayed out or the app captures nothing instead of the screen section that it expects to copy. The anti screen capture feature applies to the snip and sketch, snipping tool, Shift+Ctrl+Print Screen on Windows.

[![Citrix App protection policies anti screen capture](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_2-anti-screen-capture-ss.png)](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_2-anti-screen-capture-ss.png)

The protection extends to files from [Citrix Files](/en-us/mobile-productivity-apps/citrix-files.html) or any other connectors like Google Drive or Microsoft OneDrive that are accessed from within Citrix Workspace app. The apps are protected from screen scrapers, as are all the microapps and their notifications from within Workspace.

Another use case for anti screen capture is preventing the sharing of sensitive data in a virtual meeting / web conferencing applications (such as GoToMeeting, Microsoft Teams or Zoom). Misdelivery (sharing data with the wrong recipient or publishing data to unintended audiences) is a common threat action variety that plagues many industries. In [2019](https://enterprise.verizon.com/resources/reports/dbir/), misdelivery has been a primary source of security incidents in the healthcare industry. Your data and applications are protected not only from external threats, but also from your own employees. Internal actors have been involved in 34% of security incidents in 2019, but this number is higher in some industries (45% in Education and a whopping 59% in Healthcare).

## How does it work?

App protection policies protect client endpoints running Windows and macOS operating systems. App protection policies work by controlling access to specific API calls of the underlying OS, required to capture screens or keyboard presses. App protection policies can therefore provide protection even against custom and purpose-built hacker tools. However, as OS's evolve, new ways of capturing screens and logging keys can emerge. While Citrix continues to identify and address, Citrix cannot guarantee full protection in specific configurations and deployments.

When a user logs into StoreFront, the security capabilities of the endpoint are assessed and matched against available resources. Applications and desktops protected by App protection policies are visible only if an endpoint meets the security requirements. One such requirement is a check if the App protection components are installed.

[![App protection diagram](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_diagram.png)](/en-us/tech-zone/learn/media/tech-briefs_app-protection-policies_diagram.png)

It is often assumed that you have to sacrifice user experience to get better security. App protection policies are implemented in a way that is seamless to the end users:

*  Protected resources are hidden from users if they cannot access them because they client does not support App protection policies
*  Anti screen capture is enabled only when a protected window is on screen (users can minimize it if they must take screenshot of the unprotected window)
*  Anti-keylogging protection is enabled only when the protected window is in focus

## Summary

App protection policies can help protect application data from attackers that have surreptitiously installed either a keylogger or screen capture tools or both on endpoints. App protection policies allow companies to embrace [BYOD](https://www.citrix.com/glossary/byod.html), and extend resources to [remote workers](/en-us/tech-zone/learn/tech-briefs/business-continuity.html), contractors, and gig economy workers. Read more about App protection policies configuration, specific requirements, and compatibility in our [App protection product documentation](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html)
