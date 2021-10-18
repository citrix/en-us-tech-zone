---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: Frank Srp
description: With Secure Private Access, organizations go beyond access and aggregation to provide IT with policy controls that provide conditional access to cloud apps and internet browsing, enhancing the organization's overall security and compliance posture.
tz_title: Secure Private Access
tz_products: citrix-secure-private-access;
---
# Tech Brief: Secure Private Access

## Overview

As people consume more SaaS-based applications, organizations must be able to unify access to all apps and simplify end user authentication while still enforcing security and privacy standards. It’s also crucial to monitor vendor SLAs, application utilization, and security analytics. Citrix Secure Private Access delivers on those requirements and more.

Secure Private Access provides instant Single-Sign On (SSO) access to SaaS and web applications, granular and adaptive security policies, app protection policies for all apps, and web browser isolation. Secure Private Access combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators:

*  MFA and Device Trust
*  Web and SaaS SSO
*  Gateway
*  Cloud App Control
*  Secure Browser
*  App protection
*  Analytics

![Secure Private Access High level](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_diag.png)

Figure 1: Secure Private Access capabilities

## Preferred Primary Identity

Every organization can select its own unique identity provider for end users’ initial authentication to Citrix Workspace. With Workspace, end users login with strong authentication policies using their primary user identity.

Workspace uses the primary identity to authorize the user to a set of resources, each most likely have additional identities. Accounts associated with the set of authorized resources are secondary identities.

Examples of primary identities include:

*  Azure Active Directory: The Workspace sign-in process is redirected to Azure Active Directory, which can incorporate custom branding, multifactor authentication, password policies, and auditing.
*  Windows Active Directory: Utilizes an organization’s on-premises Active Directory environment for initial sign-on to the user’s workspace experience. To federate the on-premises Active Directory with Citrix Cloud, the administrator deploys redundant cloud connectors in the same resource location as Active Directory.
*  Active Directory with Time-Based One Time Password: Active Directory-based authentication can also include multifactor authentication with a Time-based One Time Password (TOTP).
*  Citrix Gateway: Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace.
*  Okta: Organizations can use Okta as the primary user directory for Citrix Workspace.

![Authentication](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_auth.png)

Figure 2: Multiple identity providers

Having primary and secondary identities allows Secure Private Access to use a third party to provide single sign-on to SaaS apps. Secure Private Access treats the third party as a service provider to provide an identity chain back to the primary user directory. This identity chaining enables customers to stay on their current SSO provider without requiring major changes and layer on enhanced security policies and analytics.

![Any Identity](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_anyidentity.gif)

Figure 3: Using multiple identity providers

Learn more [here](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html)

## Single Sign-On

Secure Private Access is able to create a connection to on-premises web apps without relying on a VPN. This VPN-less connection utilizes an on-prem deployed connector. The connector creates an outbound control channel to the organization’s Citrix Cloud subscription. From there, Secure Private Access is able to tunnel connections to the internal web apps while providing SSO.

Some organizations might have already standardized on an SSO provider. Secure Private Access is able to utilize third party SSO providers and still pull those resources into Workspace to maintain a single location for all resources.

Once the user is authenticated with a primary identity, the single sign-on feature in Citrix Cloud uses  SAML assertions to automatically fulfill subsequent authentication challenges to SaaS and web apps. There are 300+ SAML SSO templates available for quick configuration for web and SaaS apps.

![Single Sign-On](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_saassso.gif)

Figure 4: SSO to SaaS using SAML

Learn more [here](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html)

## Browser Isolation

 Secure Private Access enables end users to safely browse the internet. When an end user launches a SaaS application from Citrix Workspace, several decisions are dynamically made to decide how best to serve this SaaS application. Secure Private Access provides three ways to serve this application to the end-user:

*  Launch the application in the local browser without enhanced security
*  Launch the application in the Citrix Workspace Browser within Citrix Workspace
*  Launch the application in a Secure Browser session — a virtualized browser

![Browser Options ](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_enhsec.png)

Figure 5: Multiple browser options

Beside using the local web browser, Citrix offers two alternatives to accessing apps within Workspace.

First, the Citrix Workspace Browser is a Chrome-based browser running on the client machine embedded in the Citrix Workspace security sandbox. Running locally gives end users the best performance for rendering webpages of SaaS applications. The secure sandbox protects the end user and the enterprise against malware, performance degradation, data loss, and unintended end user behavior.

Second, the Citrix Secure Browser Service is a cloud hosted browser service that does not require any browser to be installed on end user devices. The Secure Browser service is essentially a virtualized browser running in Citrix Cloud. This hosted browser service provides a secure way to access internet and corporate browser-based applications. It creates an air gap between the browser and users, devices, and networks, protecting them from dangerous malware.

Web links can be automatically redirected to the Citrix Workspace Browser or the Secure Browser service to protect end-users from potentially malicious websites. This transition is completely transparent for the end-user and keeps organizations safe while allowing employees to get their job done.

Secure Private Access has a large database of URIs that are risk scored and administrators can set policies on specific domains to allow or block URIs. Administrators can also set policies on how applications need to be served to end-users.

![Browser Isolation](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_browseiso.gif)

Figure 6: Web browser isolation with the Citrix Workspace Browser and Secure Browser Service

The following diagram contrasts the communication flow to sanctioned SaaS apps when using Workspace App with Workspace Browser versus a local browser.

![Enhanced Security Flow Diagram](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_enhsecdiag.png)

Figure 7: Enhanced Security Flow Diagram

## Enhanced Security

To protect content, Secure Private Access incorporates enhanced security policies within SaaS applications. Each policy enforces a restriction on the Workspace Browser when using the Workspace app or on the Secure Browser service when using the Workspace on web or mobile.

Referred to as Cloud App Control, this capability provides IT with a way to enforce security policies on both web and SaaS applications that they provision to employees. These policies protect data stored in these applications:

*  Preferred browser: Disables local browser use and relies on the Workspace Browser engine (Workspace app - desktop) or Secure Browser (Workspace app – mobile and web).
*  Restrict clipboard access: Disables cut/copy/paste operations between the app and endpoint clipboard.
*  Restrict printing: Disables ability to print from within the app browser.
*  Restrict navigation: Disables the next/back browser buttons.
*  Restrict downloads: Disables the user’s ability to download from within the SaaS app.
*  Display watermark: Overlays a screen-based watermark showing the user name and IP address of the endpoint. If a user tries to print or take a screenshot, the watermark appears as displayed on the screen.

![Enhanced Security](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_enhsecweb.gif)

Figure 8: Enhanced Security policies with an internal web app

## App Protection

One risk that must be mitigated when end users use their personal devices for work is malware. Two of the most dangerous types are keylogging and screenshot malware. Both which can be used to both exfiltrate and harvest sensitive information like user credentials or personally identifiable information.

Secure Private Access has advanced policies to protect user and organization data on web, SaaS, and virtual apps. App Protection policies protect user sessions and application data hijacking by keyloggers and screen capturing malware.

App protection policies are rules applied while enabling enhanced security to an app.

*  Anti-Keylogging – protects the active window in focus against keyloggers by encrypting keystrokes
*  Anti-Screen Capture – protects the active window against screenshots by blanking the screen

A less malicious but equally dangerous risk is accidental screen sharing. The line between personal and work usage on devices has been blurred, so it’s become common for end users to move from working on a business app to a virtual hangout with friends or family on that device. In these scenarios, accidental screen sharing of sensitive data in the business app can result in significant issues, especially for end users in highly regulated industries.

![App Protection](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_appprotslack.gif)

Figure 9: App Protection policies protecting Citrix Workspace privacy while using a screen sharing app

## Security Analytics

End users invariably access SaaS apps that have enhanced security enabled. Workspace app, the Citrix Gateway service, and the Secure Browser service provide the Security analytics service with information about the following user and application behaviors. These analytics impact the user’s overall risk score:

*  App launch time
*  App end time
*  Print action
*  Clipboard access
*  URL Access
*  Data upload
*  Data download

## Usage analytics

Usage Analytics provides insights into the basic usage data of Secure Private Access. Admins get the visibility into how users interact with the SaaS and Web applications that are being used in their organization.

The usage data helps them to understand the user adoption and engagement of a product. The following metrics to determine how the are being used and adding value for users:

*  Number of unique users using the SaaS and Web applications
*  Top SaaS and Web application users
*  Number of SaaS and Web applications launched
*  Top SaaS and Web applications
*  Top domains accessed by the users
*  Total amount of data uploaded and downloaded across users, applications, and domains

Learn more [here](/en-us/tech-zone/learn/tech-briefs/analytics.html)

## Use Cases

### VPN-less Access

Citrix Secure Private Access complements or replaces existing VPN solutions with a Zero Trust solution that allows access for remote users without a VPN. This solution solves many challenges with providing access to internal resources for external users. With Secure Private Access there is:

*  No network device to manage, maintain, and secure – reducing appliance sprawl
*  No public IP address required as the cloud services are able to contact internal resources via the cloud connectors
*  No firewall rules required as the cloud connector and virtual app/desktop resources establish outbound connections to the cloud-based services (no inbound communication required)
*  A global deployment, organizations are automatically routed/rerouted to the optimal Gateway Service, greatly simplifying any configurations required by the organization.
*  No change to the underlying data center infrastructure.

Workspace is able to create a connection to on-premises web apps without relying on a VPN. This VPN-less connection utilizes an on-prem deployed connector. The connector creates an outbound control channel to the organization’s Citrix Cloud subscription. From there, Workspace is able to tunnel connections to the internal web apps while providing SSO. VPN-less access not only improves security and privacy but also improves end user experience.

![VPN-less Access](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_usecase1.png)

Figure 10: Use Case 1: VPN-less Access

### SSO and Security Controls for SaaS Apps

Secure Private Access offers single sign-on and contextual policies for access to web and SaaS apps. Using Citrix Gateway or Okta as the IdP provides support for all multifactor authentication mechanisms and contextual controls. These integrations protect customers' existing identity ecosystem investment and ease their move to cloud without a rip and replace forklift upgrade.

Although an authorized SaaS app is considered safe, content in the SaaS app actually can be dangerous - constituting a security risk. Enhanced security with App Protection provides IT with a way to enforce security policies on both web and SaaS applications that they provision to employees. These policies protect data stored in these applications by applying the following controls:

*  Watermarking
*  Restrict navigating
*  Restrict downloads
*  Restrict keylogging
*  Restrict screen capture
*  Restrict printing

More remote workers mean more remote meetings and web conferencing through various applications. These meetings usually require employees to share their screen, which opens the possibility of exposing sensitive data by mistake. The App Protection feature protects against screenshot malware and web conference screen capturing by returning a blank screenshot instead of the information on a user’s screen. This protection also applies to the most common snipping tools, print-screen tools, screen capture, and recording tools.

Browser isolation for internet traffic protects end users and enterprises from web-based threats. With Workspace Browser and Secure Browser service, admins get a choice to access sites in a local Chrome based browser a cloud hosted virtual machine (VM). With the service, possible attacks are contained in the cloud. Browsers run in an isolated environment where the VM is destroyed after use and a new instance is created for each app access. Policies control functions like "Copy and Paste" so that no files or data can reach the corporate network.

![SSO and Security Controls for SaaS Apps](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_usecase2.png)

Figure 11: Use Case 2: SSO and Security Controls for SaaS Apps

### Protecting User and Corporate Data on BYO and Unmanaged Endpoints

Using Enhanced security policies, Secure Private Access gives admins the ability to protect their organizations from data loss and credential theft. Enhanced security policies are even more critical when employees use personal devices to access corporate resources.

One set of policies is the App Protection feature. App Protection enables Citrix admins to enforce policies to protect endpoints from-screen capture and keylogging. The feature protects employees from dormant screen-grabbing malware or keyloggers that can potentially capture passwords or personal information.

App protection policies work by controlling access to specific API calls of the underlying OS required to capture screens or keyboard presses. These policies can protect against even the most customized and purpose-built hacker tools. It helps to secure any virtual or web application that employees use within Citrix Workspace and authentication dialog boxes (preventing password leaks) within Workspace.

The App Protection feature makes the text entered by the user indecipherable by encrypting it before a keylogging tool can access it. A keylogger installed on the client endpoint reading the data would capture gibberish characters instead of the keystrokes the user is typing.

![Protecting User and Corporate Data on BYO and Unmanaged Devices](/en-us/tech-zone/learn/media/tech-briefs_secure-workspace-access_usecase3.png)

Figure 12: Use Case 3: Protecting user and Corporate Data on BYO and Unmanaged Devices

## Summary

In conclusion, Citrix Workspace aggregates all resources into a single, personalized user interface. End users can either opt for a locally installed Workspace App (desktop and mobile) or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the experience remains familiar and consistent.

With Secure Private Access, organizations go beyond access and aggregation. Secure Private Access gives IT policy controls that enable conditional access to SaaS apps and internet browsing. It enhances an organization’s overall security and compliance posture. The user’s experience remains seamless and integrated as part of Citrix Workspace. SaaS and web apps can be accessed alongside their mobile and virtual apps and desktops.
