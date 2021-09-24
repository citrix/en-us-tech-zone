---
layout: doc
h3InToc: true
contributedBy: Steven Beals
description: End users should be able to work where, when, and how they want. Citrix Workspace has everything you need to keep people productive and data secure.
tz_title: Citrix Workspace
tz_products: citrix-workspace
---
# Tech Brief: Citrix Workspace

## Overview

Citrix Workspace is a complete digital workspace solution that delivers secure access to the information, apps, and other content that is relevant to an end user's role in your organization. Users subscribe to the services you make available and can access them from anywhere, on any device. Citrix Workspace allows you to deploy applications and desktops faster and helps you organize and automate the most important details that your users need to collaborate, make better decisions, and focus on their work.

Citrix Workspace includes many features to provide users access to their digital workspace.

![[Citrix Workspace User Experience](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace_user-experience-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace_user-experience-overview.png)

## Resource Aggregation

Citrix Workspace integrates Citrix Cloud Services that enables users to access all the desktops, applications, and content available to them. Citrix Workspace provides access from multiple resource or content locations to users from anywhere on any device.

![[Citrix Workspace Unified Resource Feed](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace_workspaces-unified-resource-feed.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-workspace_workspaces-unified-resource-feed.png)

### Virtual Applications and Desktops Aggregation

Citrix Workspace aggregates current on-premises deployments of [Citrix Virtual Apps and Desktops](/en-us/citrix-virtual-apps-desktops) alongside a [Citrix Virtual Apps and Desktops Service](/en-us/citrix-virtual-apps-desktops-service.html) resource location.

### Content Collaboration Aggregation

Citrix Workspace aggregates the [Citrix Content Collaboration Service](/en-us/citrix-content-collaboration.html) providing access to files located on-premises or within a cloud hosted solutions such as ShareFile or OneDrive.

### Web and SaaS Aggregation

Web and SaaS applications are integrated into the Citrix Workspace and are secured by the [Secure Workspace Access Service](https://www.citrix.com/products/citrix-secure-workspace-access/).

### Microapp Aggregation

Citrix Workspace aggregates microapp notifications, actions, and the intelligent feed delivered through the [Citrix Microapp Service](/en-us/citrix-microapps.html) into the Citrix Workspace.

### Secure Browser Service Aggregation

Citrix Workspace integrates the [Citrix Secure Browser Service](/en-us/citrix-cloud/secure-browser-service.html) to isolate web browsing and protect the corporate network from browser-based attacks.

### Citrix Analytics

Get insights into your environment with the [Citrix Analytics Service](/en-us/citrix-analytics.html) for all your Citrix Workspace users.

## User Interface

Citrix Workspace provides several features centered on the user interface to allow Citrix Workspace to be agnostic to a device but continue to ensure consistency across multiple devices.

### Cross-platform Client Support

Using the [Citrix Workspace App](/en-us/citrix-workspace-app.html), Citrix Workspace provides a unified and consistent user experience across web, desktop, and mobile devices to end users.

### Favorites and Recommendations

Within Citrix Workspace, end users can use the [Favorites](/en-us/citrix-workspace/experience.html#workspace-features) feature to mark applications or desktops as a favorite.

### Recently Accessed Files and Applications

Highlight [recently accessed files, applications, and desktops](/en-us/citrix-workspace/experience.html#workspace-features) and show recommended files based on user and team activity.

### App Categories and Folders

Displays [application categories](/en-us/citrix-virtual-apps-desktops/install-configure/application-groups-create.html) and folders for applications.

### Search

Provide [search](/en-us/citrix-workspace/experience.html#workspace-features) abilities to the Citrix Workspace across all resources provided.

### Single-Sign on to All Resources

Many resources require another form of authentication, often with an identity different from the users primary workspace identity. Citrix Workspace provides users with a seamless experience with [single sign-on](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html) to secondary resources.

## Customizations

Citrix Workspace allows admins to customize the appearance of end user workspaces.

### Basic Customizations

Citrix Admins can [customize](/en-us/citrix-workspace/configure.html#customize-the-appearance-of-workspaces) the color and logo of the digital workspace.

### Policy Driven Themes (preview)

Citrix Admins can [create, customize, and prioritize multiple themes](/en-us/citrix-workspace/configure.html#customize-the-appearance-of-workspaces) for the many business units of their organizations. Citrix Admins can provide each employee a more personalized workspace experience within the workspace.

### EULA for Login (preview)

Citrix Workspace enables Citrix Cloud admins to personalize Citrix applications with their own pre-login custom message or license agreement for end users.

### Resource Filtering

Virtual Desktops and applications can be filtered based on client properties, resource type, or keyword through Contextual Access where Citrix ADC is used as the primary identity provider.

## Availability and Resiliency

Citrix Cloud services are designed with industry leading practices and offer several features to achieve a high degree of service availability and resiliency. Services are built to operate in multiple regions and data centers in multiple public cloud providers across the globe.

### Offline Resilience

[Service Continuity](/en-us/citrix-workspace/service-continuity.html()) removes or minimizes the dependence on the availability of components involved in the connection process. This featureallows users to continue to launch their virtual applications and desktops whatever the availability of the Citrix Cloud service.

### Service Monitoring

Citrix has a dedicated Site Reliability team that provides 24x7 service monitoring. This team provides rapid recovery and timely restoration of services along with maintaining business critical operations.

### Auto-Upgrade

Citrix Workspace is an evergreen service. Citrix admins never need to upgrade the Citrix management components including feature and security updates.

## Configuration

### Workspace App Deployment Option

Administrators can set the preference to the way users open applications and desktops whether it opens in the native application, the browser, or lets the user choose the method.

### Select View

Citrix Workspace view defaults to the home screen, however administrators can set a users workspace to open either in Applications or Desktops

### Deploy Workspace App

Citrix Workspace can deliver the native Citrix Workspace App installer to end users at logon.

### Start Menu Integration

Citrix administrators can publish applications, SaaS applications, and desktops in the Start Menu or as desktop shortcuts when using the Citrix Workspace App for Windows.

### StoreFront to Workspace Migration (preview)

The Citrix StoreFront to Workspace URL migration feature is available to customers who currently deploy Citrix StoreFront to deliver Citrix Virtual Applications and Desktops. Using the [Citrix Global App Configuration Service](https://developer.cloud.com/citrixworkspace/server-integration/global-app-configuration-service/docs/getting-started) for Citrix Workspace, Citrix administrators can deliver the Workspace service URLs and Workspace app settings through a centrally managed service which provides the option to configure [StoreFront URL to Workspace URL](/en-us/citrix-workspace-app-for-windows/configure.html#storefront-to-workspace-url-migration) mapping.

## Authentication and Access

### Long Lived Token and Password (preview)

Citrix Workspace can use a secure token persisted on an end users device to reduce the frequency of logon prompts. By default, the token expiration date is set for 30 days.

### SAML Authentication

Citrix Workspace supports using [SAML](/en-us/citrix-workspace/secure.html#saml-20) to manage end user authentication allowing you to choose the provider of your choice provided it supports SAML 2.0.

### Third Party IdPs

Citrix Workspace supports using both [Azure Active Directory](/en-us/citrix-workspace/secure.html#azure-active-directory) and [Okta](/en-us/citrix-workspace/secure.html#okta) to manage end user authentication to their workspace.

### Smart Card Authentication

Citrix Workspace supports the use of Smart Cards for end user authentication. Note that Smart Card authentication for Citrix Workspace requires a SAML configuration with an IdP that supports the requirement.

### Domain Pass-Through Authentication

When launching an application or desktop from a domain-joined Windows client, users aren't prompted for their password launched from Citrix Workspace.

### Default Domain

Citrix Workspace pre-populates the Windows domain within the Citrix Workspace logon form.

### Two-Factor Authentication OTP

Citrix Workspace provides a cloud-hosted Time-based One-Time Password (TOTP) option for organizations using a Windows Active Directory as the primary identity. The TOTP micro-service adds multifactor authentication to the Workspace experience,

### Internal and External Detection

Citrix Workspace, on launch, can detect if users are accessing the workspace via an internal or external connection and route them appropriately.

### On-premises Citrix ADC as the IdP

Citrix Workspace can use an on-premises [Citrix Gateway](/en-us/citrix-workspace/secure.html#citrix-gateway) as an identity provider to manage user authentication to end users workspaces allowing for [nFactor authentication](/en-us/tech-zone/learn/tech-briefs/citrix-nfactor-mfa.html).

### Federated Authentication Service (FAS)

With [FAS](/en-us/citrix-workspace/workspace-federated-authentication.html) enabled, users can sign into their Citrix Workspace through a federated identify provided such as Okta, Azure AD, Google IdP). Users enter their credentials only once to access their Citrix Virtual Applications and Desktops service hosted resources.

### Change Expired Password at Logon

Within Citrix Workspace users are prompted to change their expired password at logon.

### Change Password at Anytime

Citrix Workspace provides the ability for users to reset their password from the Citrix Workspace App interface after logon.

### Contextual Access (preview)

Citrix Workspace offers [contextual access](/en-us/citrix-gateway-service/contextual-access-to-web-and-saas-apps.html) via the Citrix Gateway Service to enterprise Web and SaaS applications which are based on the end user network location via an advanced policy infrastructure.
