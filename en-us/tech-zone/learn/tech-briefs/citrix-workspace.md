---
layout: doc
h3InToc: true
contributedBy: Steven Beals
description: End users should be able to work where, when, and how they want.  Citrix Workspace has everything you need to keep people productive and data secure.
tz_title: Citrix Workspace
tz_products: citrix-workspace
---
# Tech Brief: Citrix Workspace

## Overview

Citrix Workspace is a complete digital workspace solution that delivers secure access to the information, apps, and other content that is relevant to a person’s role in your organization. Users subscribe to the services you make available and can access them from anywhere, on any device. Citrix Workspace allows you to deploy applications and desktops faster and helps you organize and automate the most important details that your users need to collaborate, make better decisions, and focus on their work.  Citrix Workspace contains many features and functionalities to provide users access to their digital workspace.

![[Citrix Workspace](/en-us/tech-zone/learn/media/tech-briefs_citrixworkspace-user-experience-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_citrixworkspace-user-experience-overview.png)

## Resource Aggregation

Citrix Workspace contains several aggregation features that enable users to access all the desktops, applications, and content available, from multiple resource or content locations to them from anywhere on any device.

![[Citrix Workspace](/en-us/tech-zone/learn/media/tech-briefs_citrixworkspace-workspaces-unified-resource-feed.png)](/en-us/tech-zone/learn/media/tech-briefs_citrixworkspace-workspaces-unified-resource-feed.png)

### Virtual Applications and Desktops Aggregation

Citrix Workspace provides the ability to aggregate a current on-premises deployment of [Citrix Virtual Apps and Desktops](/en-us/citrix-virtual-apps-desktops) alongside a [Citrix Virtual Apps and Desktops Service](/en-us/citrix-virtual-apps-desktops-service.html) resource location.

### Content Collaboration Aggregation

Citrix Workspace provides the ability to aggregate the [Citrix Content Collaboration Service](/en-us/citrix-content-collaboration.html) into the Citrix Workspace providing access to files located on-premises or within a cloud hosted solutions such as ShareFile or OneDrive.

### Web and SaaS Aggregation

Citrix Workspace aggregate’s web and SaaS applications into the Citrix Workspace which can be secured by the [Secure Workspace Access Service](https://www.citrix.com/products/citrix-secure-workspace-access/).

### Microapp Aggregation

Citrix Workspace aggregates microapp notifications, actions, and the intelligent feed delivered through the [Citrix Microapp Service](/en-us/citrix-microapps.html) into the Citrix Workspace.

### Secure Browser Service Aggregation

Citrix Workspace integrates the [Citrix Secure Browser Service](/en-us/citrix-cloud/secure-browser-service.html) into the Citrix Workspace to isolate web browsing and protect the corporate network from browser-based attacks.

### Citrix Analytics

Get insights into your environment with the [Citrix Analytics Service](/en-us/citrix-analytics.html) for all your Citrix Workspace users.

## User Interface

Citrix Workspace provides several features centered around the user interface to allow Citrix Workspace to be agnostic to device but continue to ensure consistency across these devices.

### Cross-platform Client Support

By leveraging [Citrix Workspace App](/en-us/citrix-workspace-app.html), Citrix Workspace provides a unified, consistent user experience across web, desktop, and mobile devices to end users.

### Favorites and Recommendations

Within Citrix Workspace, end users can leverage the [Favorites](/en-us/citrix-workspace/experience.html#workspace-features) feature to mark applications or desktops as a favorite.

### Recently Accessed Files and Applications

Highlight [recently accessed files, applications, and desktops](/en-us/citrix-workspace/experience.html#workspace-features) and show recommended files based on user and team activity.

### App Categories and Folders

Displays [application categories](/en-us/citrix-virtual-apps-desktops/install-configure/application-groups-create.html) and folders for applications.

### Search

Provides the ability to [search](/en-us/citrix-workspace/experience.html#workspace-features) the Citrix Workspace across all resources provided, including applications, desktops, and files.

### Single-Sign on to All Resources

Many authorized resources require another authentication, often with an identity different from the user’s primary workspace identity. Citrix Workspace provides users with a seamless experience by providing [single sign-on](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html) to secondary resources.

## Customizations

Citrix Workspace provides the ability to customize the appearance of end user workspaces.

### Basic Customizations

Citrix Workspace provides Citrix Admins the ability to [customize](/en-us/citrix-workspace/configure.html#customize-the-appearance-of-workspaces) the color and logo of your digital workspace.  

### Policy Driven Themes (Tech Preview)

Currently in Private Tech Preview, Citrix Workspace will provide Citrix Admins the ability to [create, customize and prioritize multiple themes](/en-us/citrix-workspace/configure.html#customize-the-appearance-of-workspaces) for the many business units of their organizations providing each employee a more personalized workspace experience.

### EULA for Login (Tech Preview)

Citrix Workspace enables Citrix Cloud admins who are managing Workspace account to personalize Citrix applications with their own pre-login custom message or license agreement for end users.

### Resource Filtering

Citrix Workspace provides the ability to allow desktops and applications to be filtered based on client properties, resource type or keyword through Contextual Access where Citrix ADC is used as the primary identity provider.

## Availability and Resiliency

Citrix Workspace is designed with industry best practices and provides several features to achieve a high degree of service availability and resiliency.  Citrix Cloud services, including Citrix Workspace, leverage a multi-regional global resource pool by operating in multiple regions and datacenters in multiple public cloud providers across the globe.

### Offline Reslience

[Service Continuity](/en-us/citrix-workspace/service-continuity.html()) removes or minimizes the dependence on the availability of components involved in the connection process to allow users to continue to launch their virtual applications and desktops regardless of the availability of the Citrix Cloud service.

### Service Monitoring

Citrix has a dedicated Site Reliability team that provides 24x7 service monitoring and includes rapid recovery and timely restoration of services along with maintaining business critical operations.  

### Auto-Upgrade

Citrix Workspace is an evergreen service meaning Citrix admins will never need to upgrade the Citrix management components and is continuously being updated with both feature and security updates.

## Configuration

### Workspace App Deployment Option

Provides administrators the ability to set the preference to the way users open applications and desktops whether it opens in the native application, the browser, or lets the user choose the method.

### Select View

Citrix Workspace view defaults to the home screen, however administrators have the option to set a user’s workspace to open either in Applications or Desktops

### Deploy Workspace App

Citrix Workspace provides the ability to deliver the native Citrix Workspace App installer to end users at logon.

### Start Menu Integration

Citrix Workspace provides administrators the ability to have published applications, SaaS applications, and Desktops appear as start menu or desktop shortcuts when using the Citrix Workspace App for Windows.

### StoreFront to Workspace Migration (Tech Preview)

The Citrix StoreFront to Workspace URL migration feature is available for those customers who are currently deploying Citrix StoreFront to deliver Citrix Virtual Applications and Desktops while you transition to Citrix Workspace.  Leveraging the [Citrix Global App Configuration Service](https://developer.cloud.com/citrixworkspace/server-integration/global-app-configuration-service/docs/getting-started) for Citrix Workspace, Citrix administrators can deliver the Workspace service URLs and Workspace app settings through a centrally managed service which will provide the option to configure [StoreFront URL to Workspace URL](/en-us/citrix-workspace-app-for-windows/configure.html#storefront-to-workspace-url-migration) mapping.

## Authentication and Access

### Long Lived Token and Password (Tech Preview)

Citrix Workspace provides the ability to use a secure token persisted on user device to reduce frequency of logon prompts.  By default, the token expiration date is setup for 30 days.

### SAML Authentication

Citrix Workspace supports using [SAML](/en-us/citrix-workspace/secure.html#saml-20) to manage end user authentication allowing you to choose the provider of your choice provided it supports SAML 2.0.

### Third Party IDP’s

Citrix Workspace supports using the both [Azure Active Directory](/en-us/citrix-workspace/secure.html#azure-active-directory) and [Okta](/en-us/citrix-workspace/secure.html#okta) to manage end user authentication to their workspace.

### Smart Card Authentication

Citrix Workspace supports the use of Smart Cards for end user authentication. Please note Smart Card authentication for Citrix Workspace requires a SAML configuration with an IDP that supports the requirement.

### Domain Pass-Through Authentication

When launching an application or desktop from a domain-joined Windows client, users are not prompted for their password launched from Citrix Workspace.

### Default Domain

Citrix Workspace provides the ability to have the Windows domain pre-populated within the Citrix Workspace logon form.

### Two-Factor Authentication OTP

Citrix Workspace provides a cloud-hosted Time-based One-Time Password (TOTP) option for organizations using a Windows Active Directory as the primary identity. The TOTP micro-service adds multifactor authentication to the user’s Workspace experience,

### Internal and External Detection

Citrix Workspace provides the ability on launch to detect if users are accessing the workspace via an internal or external connection and route them appropriately.

### On-premises Citrix ADC as the IDP

Citrix Workspace can leverage an on-premises [Citrix Gateway](/en-us/citrix-workspace/secure.html#citrix-gateway) as an identity provider to manage user authentication to end users’ workspaces allowing for [nFactor authentication](/en-us/tech-zone/learn/tech-briefs/citrix-nfactor-mfa.html).

### Federated Authentication Service (FAS)

With [FAS](/en-us/citrix-workspace/workspace-federated-authentication.html) enabled users can sign into their Citrix Workspace through a federated identify provided such as Okta, Azure AD, Google IDP) and enter their credentials only once to access their Citrix Virtual Applications and Desktops service hosted resources.

### Change Expired Password at Logon

Within Citrix Workspace users are prompted to change their expired password at logon.

### Change Password at Anytime

Citrix Workspace provides the ability for users to reset their password from the Citrix Workspace App interface after logon.

### Contextual Access (Tech Preview)

Citrix Workspace offers [contextual access](/en-us/citrix-gateway-service/contextual-access-to-web-and-saas-apps.html) via the Citrix Gateway Service to enterpricse Web and SaaS applictions based on the user’s network locations via an advanced policy infrastructure.
