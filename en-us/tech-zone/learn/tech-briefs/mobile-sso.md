---
layout: doc
description: Native mobile app single sign-on for iOS and Android SaaS applications.
---
# Mobile SSO

## Contributors

**Author:** [Frank Srp](https://twitter.com/UEMSRP)

**Special Thanks:** Alex Rubio

As users consume more SaaS-based applications, organizations must be able to unify all sanctioned apps and this is true on the traditional mobile device. With many SaaS-based applications originally written for a web browser, this does not offer the best user experience. So, many of these SaaS service providers decided to go with native applications on the iOS and Android platform. Mobile SSO provides organizations the ability to simplify login operations while still enforcing authentication standards through the Citrix Workspace.

## Unified Experience

### Citrix Workspace

Citrix Workspace aggregates all resources into a single, personalized user interface. Users can either opt for a locally installed Workspace App (desktop and mobile) or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the experience remains familiar and consistent.

![Citrix Workspace App Overview](/en-us/tech-zone/learn/media/tech-briefs_access-control_workspaceapp-overview.png)

### Citrix Endpoint Management Enrollment via the Citrix Workspace

With the Citrix Endpoint Management Service and Workspace integration turned on, the enrollment process is slightly different. The user will now start off enrolling in or authenticating to the Workspace app. Once the user has authenticated, Secure Hub will pull the enrollment URL and use the existing authentication to enroll the device. Secure Hub will still be used to install any required certificates, policies and application on to the device. However, the unified app store is now located within the Workspace app.

[![Citrix Endpoint Management Enrollment via the Citrix Workspace](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_citrix-endpoint-management-enrollment-via-citrix-workspace.png)](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_citrix-endpoint-management-enrollment-via-citrix-workspace.png)

### Mobile Single Sign-On

Once the user is authenticated to Citrix Workspace with a primary identity, subsequent authentication challenges to native mobile SaaS apps are automatically fulfilled by the Single Sign-On µ-service in Citrix Cloud using SAML assertions. Traffic from the native mobile SaaS app to the Single Sign-On µ-service is accomplished by invoking a per app VPN from the native mobile SaaS app to the Citrix Gateway Service.

By default, the SAML assertion utilizes the email address associated with the user’s Active Directory account (identity provider) with the email address associated with the user's SaaS or web app account (service provider).

[![Mobile Single Sign-On](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_mobile-single-sign-on.png)](/en-us/tech-zone/learn/media/tech-briefs_mobile-sso_mobile-single-sign-on.png)

## Other Tech Zone content

[Learn -> Tech Insight -> Micro VPN](/en-us/tech-zone/learn/tech-insights/micro-vpn.html) - On-demand, per-app VPN that gives access to a specific app back end resource without the risk of opening a full tunnel to your data center.

[Learn -> Tech Insight -> MDX Containers for iOS](/en-us/tech-zone/learn/tech-insights/mdx-containers.html) - Citrix Endpoint Management MDX containers protect mobile apps and control their access to device resources through policy mitigating the risk of unwanted enterprise data loss.

[Learn -> Tech Insight -> Google Chrome OS Management](/en-us/tech-zone/learn/tech-insights/google-chrome-os-management.html) - Manage Chrome OS devices with Citrix Endpoint Management.
