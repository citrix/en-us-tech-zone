---
layout: doc
h3InToc: true
contributedBy: Frank Srp
specialThanksTo: Alex Rubio
description: Native mobile app single sign-on for iOS and Android SaaS applications.
tz_title: Mobile SSO
tz_products: citrix-endpoint-management;
---
# Tech Brief: Mobile SSO

  >**Tech Preview Disclaimer**
  >
  >**This feature is currently in tech preview.** The information in this article is for information purposes only, provided “AS IS” without warranty, and is subject to change at Citrix’s discretion without notice. Any terms governing Citrix products or services shall be contained in a written agreement ran by the parties.

As users consume more SaaS-based applications, organizations must be able to unify all sanctioned apps and this fact is true on the traditional mobile device. With many SaaS-based applications originally written for a web browser, this unification does not offer the best user experience. So, many of these SaaS service providers decided to go with native applications on the iOS and Android platform. Mobile SSO provides organizations the ability to simplify login operations while still enforcing authentication standards through the Citrix Workspace.

## Unified Experience

### Citrix Workspace

Citrix Workspace aggregates all resources into a single, personalized user interface. Users can either opt for a locally installed Workspace App (desktop and mobile) or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the experience remains familiar and consistent.

![Citrix Workspace app overview](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_workspaceapp-overview.png)

### Citrix Endpoint Management Enrollment via the Citrix Workspace

With the Citrix Endpoint Management Service and Workspace integration turned on, the enrollment process is slightly different. The user can now start off enrolling in or authenticating to the Workspace app. Once the user has authenticated, Secure Hub pulls the enrollment URL and use the existing authentication to enroll the device. Secure Hub is still used to install any required certificates, policies, and application on to the device. However, the unified app store is now located within the Workspace app.

[![Citrix Endpoint Management Enrollment via the Citrix Workspace](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_citrix-endpoint-management-enrollment-via-citrix-workspace.png)](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_citrix-endpoint-management-enrollment-via-citrix-workspace.png)

### Mobile Single Sign-On

Once the user is authenticated to Citrix Workspace with a primary identity, subsequent authentication challenges to native mobile SaaS apps are automatically fulfilled by the Single Sign-On µ-service. This action happens in Citrix Cloud using SAML assertions. Traffic from the native mobile SaaS app to the Single Sign-On µ-service is accomplished by invoking a per app VPN from the native mobile SaaS app to the Citrix Gateway Service.

By default, the SAML assertion utilizes the email address associated with the user’s Active Directory account (identity provider) with the email address associated with the user's SaaS or web app account (service provider).

[![Mobile Single Sign-On](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_mobile-single-sign-on.png)](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_mobile-single-sign-on.png)
