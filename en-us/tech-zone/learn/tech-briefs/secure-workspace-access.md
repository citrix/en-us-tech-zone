---
layout: doc
h3InToc: true
contributedBy: First Last, First2 Last2
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Secure Workspace Access

## Contributors

**Author:** Florin Lazurca

## Overview

As people consume more SaaS-based applications, organizations must be able to unify access to all apps and simplify login operations while still enforcing authentication standards. It’s also crucial to monitor vendor SLAs and application utilization and security analytics.
Citrix Secure Workspace Access is a set of capabilities that delivers instant Single-Sign On (SSO) access to SaaS and web applications, granular and contextual security policies, app protection policies for all apps, browser isolation and web filtering policies.

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