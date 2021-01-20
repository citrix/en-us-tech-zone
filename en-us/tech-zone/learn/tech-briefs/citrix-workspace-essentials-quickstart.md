---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: Daniel Jim, Andre Leibovici
description: Copy & paste description from TOC here
---
# Citrix Workspace Essentials and Secure Workspace Access Getting Started Guide

## Introduction

This guide demonstrates how to configure SaaS and internal web apps with single sign-on (SSO) in Citrix Workspace. The following is what the reader will be able to accomplish using this deployment guide:

*  Learn step-by-step process in configuring VPN-less access to internal web apps in Citrix Workspace
*  Learn step-by-step process in configuring secure access to SaaS apps in Citrix Workspace
*  Learn configurations needed to enable second factor authentication to Citrix Workspace
*  Learn set of configurations needed to make SSO to SaaS/web apps work with 3rd party SAML IdP like Okta, Azure AD, etc.

## Solution Overview

Citrix Workspace Essentials provides end users with simplified, secure, and VPN-less access to SaaS, Web Apps, and data that IT delivers, using single sign-on and multi-factor authentication.

![Solution Overview](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_over.png)

Figure 1: Citrix Workspace Essentials overview

### Single Sign-on to all applications

A user’s primary Workspace identity authorizes them to access SaaS and on premises web apps. Many authorized resources require another authentication, often with an identity different from the user’s primary workspace identity. Citrix Workspace provides users with a seamless experience by providing single sign-on to these secondary resources.

![Single Sign-On](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_sso.png)

Figure 2: Access to SaaS and on premises web apps

For additional information on primary identity options for Citrix Workspace, refer to the [Workspace - Primary Identity](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html) Tech Brief.

The first thing you will notice is the ease of use. There are 300+ SAML SSO templates available for quick configuration of your web and SaaS apps. If you have an app that doesn’t have a pre-existing template, it will only take you a few more clicks to configure.

![SaaS Templates](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_template.png)

Figure 3: SAML SSO templates available for quick configuration

### Multifactor authentication (MFA) using one-time password (TOTP)

Citrix Workspace Essentials provides a cloud-hosted Time-based One-Time Password (TOTP) option for organizations using a Windows Active Directory as the primary identity. TOTP is a simple two factor auth inside workspace experience, using a time-sensitive One-Time-Password generated for the user on their registered device. Watch this [video](https://www.youtube.com/watch?v=R8xwG_k2v78) to learn more.

Users are able to request and install a new token and admins can enable and disable TOTP multifactor authentication with minimal effort. When Two Factor Auth is enabled inside workspace experience, it will be enforced for all users on all access points.

*  Natively generated One-Time Password (OTP)
*  Supports on-premises AD
*  User device / app needs to be registered with Citrix Cloud
*  OTP token generation and login
*  Self-service
*  Citrix SSO app, Google authenticator, Microsoft authenticator app

![Multifactor Authentication](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_mfa.png)

Figure 4: Natively generated One-Time Password

### Support for Workspace experience (Workspace app)

Citrix Workspace Essentials leverages all the benefits provided by Workspace experience through Workspace app including:

![Workspace App](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_app.png)

Figure 5: Benefits provided by Workspace experience

Read this Workspace App [Tech Insight](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-app.html) to learn more.

### Global POP network

Citrix Workspace Essentials operates across the globe in multiple Points of Presence (POPs) in different countries and in different regions and expanding continuously. With PoPs in both Microsoft Azure POPs and in Amazon AWS, customers are never too far away from a service node.

Every PoP is highly available and have redundant services running within a PoP in case of a failover. If in a catastrophic event an entire POP goes down which is very low probability, users can still be serviced through next closest POP. Citrix Workspace Essentials has high built-in resiliency both within a PoP and across PoPs.

### Usage Analytics for SaaS and Web Apps

Citrix Workspace Essentials includes Usage Analytics to provide end-to-end usage visibility across your SaaS and web app infrastructure. IT can track user login failures and gain insight into user adoption of applications, high usage periods, and data uploaded and downloaded by each user and application. This empowers IT to scale demand and reduce costs by deprecating low usage applications.

![Usage Analytics](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_analyt.png)

Figure 6: Usage Analytics provides end-to-end usage visibility

The SaaS and Web Apps dashboard provides Citrix Analytics administrators insight into SaaS and web apps published in Citrix Workspace. The following information can be found within the SaaS and Web Apps Dashboard:

*  Number of unique users using the SaaS and Web applications
*  Top SaaS and Web application users
*  Number of SaaS and Web applications launched
*  Top SaaS and Web applications
*  Top domains accessed by the users
*  Total amount of data uploaded and downloaded across users, applications, and domains

## Recommended Architecture

The recommended architecture below has the following components:

*  Citrix Workspace as the preferred end-user portal
*  Microsoft Active Directory is the user directory
*  Web apps are in corporate data center
*  Citrix Analytics provides usage metrics of SaaS/web apps
*  Okta and Azure AD are the SAML IdP for SaaS & Web apps (optional)
*  Access to Content Collaboration (when licensed)

![Architecture](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_arch.png)

Figure 7: Recommended architecture and components

Pre-requisites for configuring app access

*  Getting Started with [Citrix Cloud](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-app.html)
*  Make sure you have the right entitlements and / or trial enabled
*  Workspace Platform configurations required to setup the solution
    *  Setting up the Workspace
    *  User Authentication
    *  [Tech Brief](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html) with all the different types of authentication methods to Workspace

## Use case 1: Secure access to internal web apps

Citrix Workspace Essentials provides secure access to intranet web apps with SSO. Follow the [SSO to Web Configuration Steps](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/access-control-web-citrix-sso.html) in the article to deploy this feature.

![Single Sign-On to Web](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_ssoweb.png)

Figure 8: Secure access to intranet web apps with SSO

## Use case 2: Secure access to SaaS apps

Citrix Workspace Essentials provides secure access to sanctioned SaaS apps with SSO. Follow the [SSO to SaaS Configuration Steps](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/access-control-citrix-sso.html) in the article to deploy this feature.

![Single Sign-On to SaaS](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_ssosaas.png)

Figure 9: Secure access to sanctioned SaaS apps with SSO

## Use case 3: Configuring second factor authentication to Citrix Workspace

Citrix Workspace Essentials enables second factor authentication during user login to Citrix Workspace. Follow the steps in the [Configuring AD + token in Identity and Access Management](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html) and [Deploying Citrix Cloud connector](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guides to deploy this feature.

![Time-based One-time Password](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_totp.png)

Figure 10: Second factor authentication during user login to Citrix Workspace

## Citrix Secure Workspace Access

Citrix Secure Workspace Access introduces additional security features on top of the instant Single-Sign On (SSO) access to SaaS and web applications provided by Citrix Workspace Essentials. Unlike a traditional VPN, Secure Workspace Access provides a zero trust approach to securely access corporate web, SaaS, and virtual applications. With advanced security controls for managed, unmanaged, and BYO devices, it’s ideal for IT and employees alike.

![Secure Workspace Access](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace-essentials-quickstart_diag.png)

Figure 11: Citrix Secure Workspace Access Overview

Secure Workspace Access combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators. These include granular and contextual security policies, app protection policies for all apps, web browser isolation, and web filtering policies.

For more information about Citrix Secure Workspace Access, see [Citrix Secure Workspace Access](/en-us/citrix-secure-workspace-access.html) in Citrix product documentation and [Citrix Secure Workspace Access Tech Brief](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/secure-workspace-access.html)

### Secure Workspace Access Use Case 1 - Applying Enhanced Security to SaaS and Web Apps

SaaS apps have grown in popularity because of their simplicity and zero dependencies on managed infrastructure. However, many lack the security controls and governance that IT needs to meet corporate security standards. Additionally, many organizations welcome the opportunity to add layers of security to their on-premises web apps.

Citrix Secure Workspace Access enables IT to apply these added security controls to both SaaS and web apps to prevent data exfiltration. This includes policies to restrict copying and pasting, printing, downloads, navigation, watermarking (overlays a screen-based watermark showing the username and IP address of the endpoint), and more.

Each policy enforces a restriction on an embedded browser when using the Citrix Workspace app for desktop or Citrix Secure Browser service when using the Workspace app web or mobile.

Enhanced security with App Protection provides IT with a way to enforce security policies on both web and SaaS applications that they provision to employees. These policies protect data stored in these applications by applying security controls detailed in [Enhanced Security](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/secure-workspace-access.html#enhanced-security).

### Secure Workspace Access Use Case 2 - Protecting BYO and Unmanaged Devices

The increase in flexible work and the use of personal devices for work have created new security challenges. IT cannot defend against devices infected with malware with no insight into device health, especially those with keyloggers or screenshot malware that can enable attackers to exfiltrate sensitive corporate data.

Corporate-managed devices go through regular health checks to ensure that devices meet safety requirements. However, most end users won’t take the same care with personal devices. So if a user accesses corporate resources like apps or document repositories, the malware can exfiltrate login information and any data presented on the user’s screen.

App protection secures unmanaged devices by scrambling keystrokes and returning screenshots as blank screens, protecting corporate data from keyloggers, or screenshot malware.

Using Enhanced security policies, Secure Workspace Access gives admins the ability to protect their organizations from data loss and credential theft. Enhanced security policies are even more critical when employees use personal devices to access corporate resources. Read more here: [Protecting BYO](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/secure-workspace-access.html#protecting-user-and-corporate-data-on-byo-and-unmanaged-endpoints)

### Secure Workspace Access Use Case 3 - Using Secure Browser Policy and Isolation

Browsing the internet poses another risk to enterprises, exposing them to vulnerabilities in websites, browsers, and browser plug-ins. Malware that might live on employees’ devices can also pose a serious risk to corporate resources.

While most users understand they shouldn’t visit potentially risky websites on their corporate-issued devices, they may not take the same care with their personal ones. In response, some organizations even completely disallow internet browsing, severely affecting productivity.

Citrix Secure Workspace Access includes a secure embedded browser capable of applying enhanced security policies, and whenever enhanced security policies are enabled, the embedded browser is used. But suppose the user is not using Citrix Workspace, but rather a native browser. Then a more secure mechanism is required.

Citrix Secure Browser service, a Chromium-based browser hosted in Microsoft Azure, enables users to navigate the web and apps securely without introducing risk to the corporate environment. Threats that may be introduced by visiting malicious websites are isolated off the corporate network and devices. The browser is stateless and discarded at the end of each session, ensuring that any malicious software encountered while browsing the web never reaches your corporate infrastructure.

Secure Workspace Access enables end users to safely browse the internet. When an end user launches a SaaS application from Citrix Workspace, several decisions are dynamically made to decide how best to serve this SaaS application. Secure Workspace Access provides three ways to serve this application to the end-user. Read more here: [Browser Isolation](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/secure-workspace-access.html#browser-isolation)

### Secure Workspace Access Use Case 4 - Security Analytics for SaaS and Web Apps

The Citrix product portfolio is extensive, and the cloud services are designed from the ground up to interact and amplify each other. As part of this portfolio, you will find Citrix Analytics for Security. Citrix Analytics for Security natively integrates with Citrix Secure Workspace Access to provide continuous monitoring, risk assessment, and mitigation to protect organizations during and after the initial user log in, across different applications and clouds, for both compliance and governance.

Using this continuous monitoring, the system can identify inconsistent and suspicious activities, providing actionable insights into user behavior across Identity, Devices, Locations, Networks, Apps, and Files.

For example, assume a user is downloading excessive amounts of data via the VPN-less connection. In that case, an action can be triggered to request a user’s response to validate his/her identity or email the user to verify their activity and put them on a watch list. And based on the user’s reply response, a secondary action could be initiated.

These rules can be configured to trigger user accounts’ specific actions based on continuously assessed user risk score thresholds. For example, an end-user session authenticated into Citrix Workspace can be logged off based on a change in risk score in real-time.

End users invariably access SaaS apps that have enhanced security enabled. Workspace app, the Citrix Gateway service, and the Secure Browser service provide the Security analytics service with information about the following user and application behaviors. Usage Analytics provides insights into the basic usage data of Secure Workspace Access. Admins get the visibility into how users interact with the SaaS and Web applications that are being used in their organization. Read more here: [Security Analytics](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/secure-workspace-access.html#security-analytics)

### Secure Workspace Access Enhanced Security Demo

See how Citrix Secure Workspaces Access provides Enhanced Security for Web and SaaS Applications and the end user experience.

**Watch this video to [see demo](https://youtu.be/9rT-0IIKw6M):**

[![Citrix Tech Insight - Citrix Secure Workspace Access Enhanced Security](/en-us/tech-zone/learn/media/tech-insights_secure-workspace-access-user-experience_enhanced-security.png)](https://youtu.be/9rT-0IIKw6M)

### Citrix Secure Workspace Access SSO to SaaS Apps Demo

See how Citrix Secure Workspaces Access provides Single Sign On to SaaS Applications and the end user experience.

**Watch this video to [see demo](https://youtu.be/F0x26hN7ZOM):**

[![Citrix Tech Insight - Citrix Secure Workspace Access SSO to SaaS Apps](/en-us/tech-zone/learn/media/tech-insights_secure-workspace-access-user-experience_saas-sso.png)](https://youtu.be/F0x26hN7ZOM)

### Citrix Secure Workspace Access VPN-less access Demo

See how Citrix Secure Workspaces Access provides VPN-less access to Web Applications and the end user experience.

**Watch this video to [see demo](https://youtu.be/pIqfoUwsbwY):**

[![Citrix Tech Insight - Citrix Secure Workspace Access VPN-less access](/en-us/tech-zone/learn/media/tech-insights_secure-workspace-access-user-experience_vpn-less.png)](https://youtu.be/pIqfoUwsbwY)

### Citrix Secure Workspace Access Website Filtering Demo

See how Citrix Secure Workspaces Access provides Website Filtering for Web and SaaS Applications and the end user experience.

**Watch this video to [see demo](https://youtu.be/KxxNA8Efuh0):**

[![Citrix Tech Insight - Citrix Secure Workspace Access Website Filtering](/en-us/tech-zone/learn/media/tech-insights_secure-workspace-access-user-experience_website-filtering.png)](https://youtu.be/KxxNA8Efuh0)
