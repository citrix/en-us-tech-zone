---
layout: doc
h3InToc: true
contributedBy: First Last, First2 Last2
specialThanksTo: Frank Srp
description: Copy & paste description from TOC here
---
# Secure Workspace Access

## Contributors

**Author:** Florin Lazurca

## Overview

As people consume more SaaS-based applications, organizations must be able to unify access to all apps and simplify login operations while still enforcing authentication standards. It’s also crucial to monitor vendor SLAs and application utilization and security analytics.

Citrix Secure Workspace Access is a set of capabilities that delivers instant Single-Sign On (SSO) access to SaaS and web applications, granular and contextual security policies, app protection policies for all apps, browser isolation, and web filtering policies.

Citrix Secure Workspace Access goes beyond basic access and aggregation to provide IT with policy controls, known as cloud app control, that provide conditional access to cloud apps and internet browsing, enhancing the organization’s overall security and compliance posture. The user’s experience remains seamless and integrated because the SaaS and web apps can be accessed alongside their mobile and virtual apps and desktops as an integrated part of Citrix Workspace.

Secure Workspace Access combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators:

*  Web and SaaS SSO
*  Web filtering policies
*  Cloud app control
*  Secure Browser service
*  App protection policies
*  Contextual and granular security policies
*  Workspace app

Citrix Workspace aggregates all resources into a single, personalized user interface. End users can either opt for a locally installed Workspace App (desktop and mobile) or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the experience remains familiar and consistent.

## Preferred Primary Identity

Every organization can select its own unique identity provider for end users’ initial logons to Citrix Workspace. With Workspace, end users authenticate with strong authentication policies using their primary user identity.

Workspace uses the primary identity to authorize the user to a set of resources, each will most likely have additional identities. Accounts associated with the set of authorized resources are secondary identities.

Examples of primary identities include:

*  Azure Active Directory: The Workspace sign-in process is redirected to Azure Active Directory, which can incorporate custom branding, multifactor authentication, password policies, and auditing.
*  Windows Active Directory: Utilizes an organization’s on-premises Active Directory environment for initial sign-on to the user’s workspace experience. To federate the on-premises Active Directory with Citrix Cloud, the administrator deploys redundant cloud connectors in the same resource location as Active Directory.
*  Active Directory with Time-Based One Time Password: Active Directory-based authentication can also include multifactor authentication with a Time-based One Time Password (TOTP).
*  Citrix Gateway: Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace.
*  Okta: Organizations can use Okta as the primary user directory for Citrix Workspace.

Having primary and secondary identities allows Citrix Workspace to use a 3rd party to provide single sign-on to SaaS apps. Secure Workspace Access then treats the 3rd party as a service provider to provide an identity chain back to the primary user directory. This enables customers to stay on their current SSO provider without requiring major changes and layer on Secure Workspace Access enhanced security policies and analytics.

## Single Sign-On

Secure Workspace Access is able to create a connection to on-premises web apps without relying on a VPN. This VPN-less connection utilizes an on-prem deployed connector. The connector creates an outbound control channel to the organization’s Citrix Cloud subscription. From there, Workspace is able to tunnel connections to the internal web apps while providing SSO.

Some organizations might have already standardized on an SSO provider. Citrix Workspace Secure Access is able to utilize 3rd party SSO providers and still pull those resources into Workspace to maintain a single location for all resources.

Once the user is authenticated to Citrix Workspace Secure Access with a primary identity, subsequent authentication challenges to SaaS and web apps are automatically fulfilled by the single sign-on feature in the Citrix Cloud using SAML assertions.  There are 300+ SAML SSO templates available for quick configuration for web and SaaS apps. If the app doesn’t have a pre-existing template, it will only take a few more clicks to configure.

## Browser Isolation

Citrix Secure Workspace Access enables end users to safely browse the internet. When an end user launches a SaaS application from Citrix Workspace, several decisions are dynamically made to decide how best to serve this SaaS application. Citrix Secure Workspace Access provides three ways to serve this application to the end-user:

*  Launch the application in the local browser without enhanced security
*  Launch the application in the embedded browser within Citrix Workspace
*  Launch the application in a Secure Browser session — a virtualized browser

Beside using local web browser, Citrix offers two alternatives to accessing apps within Workspace.

First, the Citrix Workspace-embedded browser is a Chrome-based browser running on the client machine embedded in the Citrix Workspace security sandbox. Running locally gives end users the best performance for rendering web pages of SaaS applications and the secure sandbox protects the end user and the enterprise against malware, performance degradation, data loss and unintended end user behavior.

Second, Citrix Secure Browser service is a cloud hosted browser service that does not require any browser to be installed on end user device. The Citrix Secure Browser service is essentially a virtualized browser running in Citrix Cloud. This hosted browser service provides a secure way to access Internet and corporate browser-based applications. It creates an airgap between the browser and users, devices, and networks, protecting them from dangerous malware.

Citrix Secure Workspace Access has a very large database of URIs that are risk scored. On top of that, the administrator can set policies on specific domains to allow or block URIs. Administrators can also set policies on how applications need to be served to end-users.

Web links to unknown or risky websites can be automatically redirected to the Citrix Workspace embedded browser or the Secure Browser service to protect end-users from potentially malicious web sites. This transition is completely transparent for the end-user and keeps organizations safe while allowing employees to get their job done.

## Enhanced Security

To protect content, Secure Workspace Access incorporates enhanced security policies within the SaaS applications. Each policy enforces a restriction on the embedded browser when using Workspace app or on the Secure Browser service when using Workspace app web or mobile.

Referred to as cloud app control, this capability provides IT with a way to enforce security policies on both web and SaaS applications that they provision to employees. These policies protect data stored in these applications:

*  Preferred browser: Disables local browser use and relies on the embedded browser engine (Workspace app - desktop) or Secure Browser (Workspace app – mobile and web).
*  Restrict clipboard access: Disables cut/copy/paste operations between the app and endpoint clipboard.
*  Restrict printing: Disables ability to print from within the app browser.
*  Restrict navigation: Disables the next/back browser buttons.
*  Restrict downloads: Disables the user’s ability to download from within the SaaS app.
*  Display watermark: Overlays a screen-based watermark showing the user name and IP address of the endpoint. If a user tries to print or take a screenshot, the watermark appears as displayed on the screen.

## App Protection

One risk that must be mitigated when end users use their personal devices for work is malware. Two of the most dangerous types are keylogging and screen shot malware, which can be used to both exfiltrate and harvest sensitive information like user credentials or personally identifiable information.

Secure Workspace Access has advanced policies to protect user and organization data on web, SaaS and virtual apps. App Protection policies protect user sessions and application data from being hijacked by keyloggers and screen capturing malware.

App protection policies are rules applied while enabling enhanced security to an app.

*  Anti-Key Logging – protects the active window in focus against keyloggers by encrypting keystrokes
*  Anti-Screen Capture – protects the active window against screenshots by blanking the screen

A less malicious but equally dangerous risk is accidental screen sharing. The line between personal and work usage on devices has been blurred, so it’s become common for end users to move from working on a business app to a virtual hangout with friends or family on that device. In these scenarios, accidental screen sharing of sensitive data in the business app could result in significant issues, especially for those in highly regulated industries.

## Web Filtering

Citrix Secure Workspace Access includes a URL filtering engine. By using the information contained in URLs, this feature helps admins monitor and control user access to malicious websites on the internet. Together, with the aforementioned browser isolation options, web filtering gives admins the choice to completely block a URL or access a URL in the embedded browswer or in a Secure Browser session.

The web filtering controller uses a categorization database and URLs list. When the request comes to the web filtering controller, it first checks the global allow list which also contains critical Citrix Cloud URLs. Then it checks to Lists and Categorization and verifies with blocked and allowed and redirect to Secure Browser URLs. If none of the URLs match, then by default it falls back to the default list.

Admins can take a cautious approach even for accessing allow list URLs. This approach ensures users get access to the information they need. It doesn’t impact on productivity while providing protection against any unforeseen threats or malicious content delivered from the internet.

A traditional URL filtering engine that assumes trust for a allow list URL. Secure Workspace Access does not implicitly trust an allow list URL since webpages, deemed safe by URL filtering engines, that can host malicious links. With Secure Workspace Access, URLs on trusted sites are also tested.

## Security Analytics

Security and performance analytics - Users invariably access SaaS apps that have enhanced security inherent in them. Workspace app, the Citrix Gateway service, and the Secure Browser service provide the Security analytics service with information about the following user and application behaviors. These analytics impact the user’s overall risk score:

*  App launch time
*  App end time
*  Print action
*  Clipboard access
*  URL Access
*  Data upload
*  Data download

The web filtering feature evaluates the risk of each hyperlink selected within the SaaS application. Accessing these sites and monitoring changes in user behavior increases the user’s overall risk score because it signals the endpoint device is compromised and started to infect or encrypt data or the user and device are stealing intellectual property.

## Usage analytics

Usage Analytics provides insights into the basic usage data of Secure Workspace Access. Admins get the visibility into how users interact with the SaaS and Web applications that are being used in their organization.

The usage data helps them to understand the user adoption and engagement of a product. The following metrics to determine how the are being used and adding value for users:

*  Number of unique users using the SaaS and Web applications
*  Top SaaS and Web application users
*  Number of SaaS and Web applications launched
*  Top SaaS and Web applications
*  Top domains accessed by the users
*  Total amount of data uploaded and downloaded across users, applications, and domains

## Use Cases

### VPN-less access

Citrix Secure Workspace Access complements or replaces existing VPN solutions with a Zero Trust solution that allows access for remote users without a VPN. This solution solves many challenges with providing access to internal resources for external users. With Secure Workspace Access there is:

*  No network device to manage, maintain and secure – reducing appliance sprawl
*  No public IP address required as the cloud services are able to contact internal resources via the cloud connectors
*  No firewall rules required as the cloud connector and virtual app/desktop resources establish outbound connections to the cloud-based services (no inbound communication required)
*  A global deployment, organizations are automatically routed/rerouted to the optimal Gateway Service, greatly simplifying configurations required by the organization.
*  No change to the underlying data center infrastructure.

Workspace is able to create a connection to on-premises web apps without relying on a VPN.  This VPN-less connection utilizes an on-prem deployed connector. The connector creates an outbound control channel to the organization’s Citrix cloud subscription. From there, workspace is able to tunnel connections to the internal web apps while providing SSO. VPN-less access not only improves security and privacy but also improves end user experience.

### Protecting user and corporate data on BYO and unmanaged endpoints

Using Enhanced security policies, Secure Workspace Access gives admins the ability to protect their organizations from data loss and credential theft. Enhanced security policies are even more critical when corporate resources are being accessed by personal devices.

One set of policies is the App Protection feature. App Protection enables Citrix admins to enforce policies to protect endpoints from-screen capture and keylogging. This protects employees from dormant screen-grabbing malware or keyloggers that could potentially capture passwords or personal information.

App protection policies work by controlling access to specific API calls of the underlying OS required to capture screens or keyboard presses. These policies can protect against even the most customized and purpose-built hacker tools. It helps to secure any virtual or web application that employees use within Citrix Workspace, as well as authentication dialog boxes (preventing password leaks) within Workspace.

The App Protection feature makes the text entered by the user indecipherable by encrypting it before keylogging tool can access it. A keylogger installed on the client endpoint reading the data would capture gibberish characters instead of the keystrokes the user is typing.

### SSO and Security Controls for SaaS Apps

Secure Workspace Access offers single sign-on and contextual policies for access to web and SaaS apps. Using Citrix Gateway or Okta as your IdP provides support for all multi-factor authentication mechanisms and contextual controls for accessing for Secure Workspace Access. These integrations protect customers' existing identity ecosystem investment and ease their move to cloud without a rip and replace forklift upgrade.

Although an authorized SaaS app is considered safe, content in the SaaS app actually can be dangerous - constituting a security risk. When a user clicks a hyperlink within a SaaS app, the traffic is routed through the web filtering feature, which provides a risk assessment for the hyperlink. Based on the hyperlink’s risk assessment, and the customized list of URL categories, the web filtering feature allows, denies, or redirects the hyperlink request from the user as follows:

*  Approved: The hyperlink is considered safe and the embedded browser accesses within the Workspace app accesses the hyperlink.
*  Denied: The hyperlink is considered dangerous and access is denied.
*  Redirected: The hyperlink request is redirected to the embedded browser or Secure Browser service, where the user’s internet browsing activities are isolated from the endpoint device, the corporate network, and the SaaS app

Enhanced security with App Protection for web and SaaS apps provides IT with a way to enforce security policies on both web and SaaS applications that they provision to employees. These policies protect data stored in these applications by applying the following controls:

*  Watermarking
*  Restrict navigating
*  Restrict downloads
*  Restrict keylogging
*  Website filtering
*  Restrict screen capture
*  Restrict printing

More remote workers mean more remote meetings and web conferencing through a variety of applications. These meetings usually require employees to share their screens, which opens the possibility of exposing sensitive data by mistake. The App Protection feature protects against screenshot malware and web conference screen capturing by returning a blank screenshot instead of the information on a user’s screen. This also applies to the most common snipping tools, print-screen tools, screen capture and recording tools.

Browser isolation for internet traffic protects end users and enterprises from web-based threats. With the Embedded Browser and Secure Browser service, admins get a choice to access sites in a local Chrome based browser a cloud hosted virtual machine (VM). With the service, possible attacks are contained in the cloud. Browsers run in isolated environment where the VM is destroyed after use and a new instance is createed for each app access. Functions like "Copy and
Paste" are controlled by policy and no files or data will reach the corporate network.