---
layout: doc
h3InToc: true
contributedBy: Florin Lazurca
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Citrix Workspace Essentials Getting Started Guide

## Introduction

This guide demonstrates how to configure SaaS and internal web apps with single sign-on (SSO) in Citrix Workspace. The following is what the reader will be able to accomplish using this deployment guide:

*  Learn step-by-step process in configuring VPN-less access to internal web apps in Citrix Workspace
*  Learn step-by-step process in configuring secure access to SaaS apps in Citrix Workspace
*  Learn configurations needed to enable second factor authentication to Citrix Workspace
*  Learn set of configurations needed to make SSO to SaaS/web apps work with 3rd party SAML IdP like Okta, Azure AD, etc.

## Solution Overview

Citrix Workspace Essentials provides end users with simplified, secure, and VPN-less access to SaaS, Web Apps, and data that IT delivers, using single sign-on and multi-factor authentication.

Figure 1:

### Single Sign-on to all applications

A user’s primary Workspace identity authorizes them to access SaaS and on premises web apps. Many authorized resources require another authentication, often with an identity different from the user’s primary workspace identity. Citrix Workspace provides users with a seamless experience by providing single sign-on to these secondary resources.

Figure 2:

For additional information on primary identity options for Citrix Workspace, refer to the [Workspace - Primary Identity](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html) Tech Brief.

The first thing you will notice is the ease of use. There are 300+ SAML SSO templates available for quick configuration of your web and SaaS apps. If you have an app that doesn’t have a pre-existing template, it will only take you a few more clicks to configure.

Figure 3:

### Multifactor authentication (MFA) using one-time password (TOTP)

Citrix Workspace Essentials provides a cloud-hosted Time-based One-Time Password (TOTP) option for organizations using a Windows Active Directory as the primary identity. TOTP is a simple two factor auth inside workspace experience, using a time-sensitive One-Time-Password generated for the user on their registered device. Watch this [video](https://www.youtube.com/watch?v=R8xwG_k2v78) to learn more.

Users are able to request and install a new token and admins can enable and disable TOTP multifactor authentication with minimal effort. When Two Factor Auth is enabled inside workspace experience, it will be enforced for all users on all access points.

*  Natively generated One-Time Password (OTP)
*  Supports on-premises AD
*  User device / app needs to be registered with Citrix Cloud
*  OTP token generation and login
*  Self-service
*  Citrix SSO app, Google authenticator, Microsoft authenticator app

Figure 4:

### Support for Workspace experience (Workspace app)

Citrix Workspace Essentials leverages all the benefits provided by Workspace experience through Workspace app including:

Figure 5:

Read this Workspace App [Tech Insight](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-app.html) to learn more.

### Global POP network

Citrix Workspace Essentials operates across the globe in multiple Points of Presence (POPs) in different countries and in different regions and expanding continuously. With PoPs in both Microsoft Azure POPs and in Amazon AWS, customers are never too far away from a service node.

Every PoP is highly available and have redundant services running within a PoP in case of a failover. If in a catastrophic event an entire POP goes down which is very low probability, users can still be serviced through next closest POP. Citrix Workspace Essentials has high built-in resiliency both within a PoP and across PoPs.

### Usage Analytics for SaaS and Web Apps

Citrix Workspace Essentials includes Usage Analytics to provide end-to-end usage visibility across your SaaS and web app infrastructure. IT can track user login failures and gain insight into user adoption of applications, high usage periods, and data uploaded and downloaded by each user and application. This empowers IT to scale demand and reduce costs by deprecating low usage applications.

Figure 6:

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

Figure 7:

Pre-requisites for configuring app access

*  Getting Started with [Citrix Cloud](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-app.html)
*  Make sure you have the right entitlements and / or trial enabled
*  Workspace Platform configurations required to setup the solution
    *  Setting up the Workspace
    *  User Authentication
    *  [Tech Brief](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html) with all the different types of authentication methods to Workspace

## Use case 1: Secure access to internal web apps

Citrix Workspace Essentials provides secure access to intranet web apps with SSO. Follow the [SSO to Web Configuration Steps](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/access-control-web-citrix-sso.html) in the article to deploy this feature.

Figure 8:

## Use case 2: Secure access to SaaS apps

Citrix Workspace Essentials provides secure access to sanctioned SaaS apps with SSO. Follow the [SSO to SaaS Configuration Steps](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/access-control-citrix-sso.html) in the article to deploy this feature.

Figure 9:

## Use case 3: Configuring second factor authentication to Citrix Workspace

Citrix Workspace Essentials enables second factor authentication during user login to Citrix Workspace. Follow the steps in the [Configuring AD + token in Identity and Access Management](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html) and [Deploying Citrix Cloud connector](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guides to deploy this feature.

Figure 10:
