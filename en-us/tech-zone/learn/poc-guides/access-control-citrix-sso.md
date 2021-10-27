---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Learn how to set up a Citrix Secure Private Access environment that uses Citrix as the single sign-on provider for SaaS applications.
tz_title: Secure Access to SaaS Applications with Citrix Secure Private Access
tz_products: citrix-secure-private-access;
---
# PoC Guide: Secure Access to SaaS Applications with Citrix Secure Private Access

## Overview

As users consume more SaaS-based applications, organizations must be able to unify all sanctioned apps, simplify user login operations while still enforcing authentication standards. Organizations must be able to secure these applications even though they exist beyond the confines of the data center. Citrix Workspace provides organizations with secure access to SaaS apps.

In this scenario, a user authenticates to Citrix Workspace using Active Directory, Azure Active Directory, Okta, Google, or Citrix Gateway as the primary user directory. Citrix Workspace provides single sign-on services for a defined set of SaaS applications.

[![Single Sign-on Overview](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_overview.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_overview.png)

 If the Citrix Secure Private Access service is assigned to the Citrix subscription, enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links are applied on top of the SaaS applications.

The following animation shows a user accessing a SaaS application with Citrix providing SSO and secured with Citrix Secure Private Access.

 [![Citrix SSO Demo](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_demo-video.gif)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_demo-video.gif)

This demonstration shows an IdP-initiated SSO flow where the user launches the application from within Citrix Workspace. This PoC guide also supports a SP-initiated SSO flow where the user tries to access the SaaS app directly from their preferred browser.

This proof of concept guide demonstrates how to:

1.  Setup Citrix Workspace
2.  Integrate a primary user directory
3.  Incorporate Single Sign-On for SaaS applications
4.  Define website filtering policies
5.  Validate the configuration

## Setup Citrix Workspace

The initial steps for setting up the environment is to get Citrix Workspace prepared for the organization, which includes

1.  Setting up the Workspace URL
1.  Enabling the appropriate services

### Set Workspace URL

1.  Connect to [Citrix Cloud](https://cloud.com) and log in as your administrator account
1.  Within Citrix Workspace, access **Workspace Configuration** from the upper-left menu
1.  From the **Access** tab, enter a unique URL for the organization and select Enabled

[![Workspace URL](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_workspace-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_workspace-config-001.png)

### Enable Services

From the Service Integration tab, enable the following services to support the secure access to SaaS apps use case

1.  Gateway
2.  Secure Browser

[![Workspace Services](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_workspace-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_workspace-config-002.png)

### Verify

Citrix Workspace takes a few moments to update services and URL settings. From a browser, verify the custom Workspace URL is active. However, logon is not be available until a primary user directory gets defined and configured.

## Integrate a Primary User Directory

Before users can authenticate to Workspace, a [primary user directory](/en-us/citrix-workspace/secure.html) must be configured. The primary user directory is the only identity the user requires as all requests for apps within Workspace utilizes single sign-on to secondary identities.

An organization can use any one of the following primary user directories

*  [Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guide.
*  [Active Directory with Time-Based One Time Password](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp): Active Directory-based authentication can also include multifactor authentication with a Time-based One Time Password (TOTP). This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html#active-directory-authentication) details the required steps to enable this authentication option.
*  [Azure Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#azure-active-directory): Users can authenticate to Citrix Workspace with an Azure Active Directory identity. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html) provides details on configuring this option.
*  [Citrix Gateway](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#citrix-gateway): Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html) provides details on the integration.
*  [Okta](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#okta): Organizations can use Okta as the primary user directory for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/okta-identity.html) provides instructions for configuring this option.

## Configure Single Sign-on

To successfully integrate SaaS apps with Citrix Workspace, the administrator needs to do the following

*  Configure SaaS app
*  Authorize SaaS app

### Configure SaaS App

*  Within Citrix Cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, search and correct template, which in this instance is **Humanity**
*  In the App details window, type in the organization's unique domain name for the SaaS application. The URL and Related Domains will automatically populate.

***Note**: Enhanced security policies uses the related domains field to determine the URLs to secure. One related domain is automatically added based on the URL in the previous step. Enhanced security policies require related domains for the application. If the application uses multiple domain names, the must be added into the related domains field, which is often `*.<companyID>.SaaSApp.com` (as an example `*.citrix.slack.com`)*

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-02.png)

*  In the **Enhanced Security** window, select the appropriate security policies for the environment
*  In the **Single Sign-On** window, copy the **Login URL**.
*  Select the link for **SAML Metadata** to identify the SAML settings needed for the SaaS application.

[![Setup SaaS App 03](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-03.png)

*  Within the SAML Metadata file, copy the X509 Certificate, represented as an alphanumeric string.

[![Setup SaaS App 04](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-04.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-04.png)

*  Within the Humanity SaaS app, use the gear icon in the upper right-hand corner to bring up settings. Select **Single Sign-On**

[![Setup SaaS App 05](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-05.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-05.png)

*  For the SAML Issuer URL, use the **Login URL** obtained from the Citrix Workspace configuration.
*  Past the x.509 Certificate string from the Citrix metadata file into the Humanity SaaS app.

[![Setup SaaS App 06](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-06.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-06.png)

*  Save the settings in Humanity.
*  Within Citrix Workspace, select **Save**
*  Select **Finish**

### Authorize SaaS App

*  Within Citrix Cloud, select **Library** from the menu

[![Authorize SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_authorize-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_authorize-saas-app-01.png)

*  Find the SaaS app and select **Manage Subscribers**
*  Add the appropriate users/groups who are authorized to launch the app

[![Authorize SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_authorize-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_authorize-saas-app-02.png)

### Validate

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured SaaS application
*  The SaaS App successfully launches

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser redirects to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app launches with Citrix providing single sign-on

## Define website filtering policies

Citrix Secure Private Access service provides website filtering within SaaS and Web apps to help protect the user from phishing attacks. The following shows how to setup website filtering policies.

*  From Citrix Cloud, **Manage** within the Secure Private Access tile

[![Citrix Secure Private Access 1](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_secure-workspace-access-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_secure-workspace-access-01.png)

*  If this guide was followed, the **Set up end user authentication** step and the **Configure end user access to SaaS, web and virtual applciations** steps are complete. Select **Configure Content Access**
*  Select **Edit**
*  **Enable** the **Filter website categories** option
*  Withint the **Blocked categories** box, select **Add**
*  Select the categories to block users from accessing

[![Citrix Secure Private Access 2](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_access-control-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_access-control-02.png)

*  When all applicable categories are selected, select **Add**

[![Citrix Secure Private Access 3](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_access-control-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_access-control-03.png)

*  Do the same for allowed categories
*  Do the same for redirected categories. These categories redirect to a Secure Browser instance
*  If needed, admins can filter denied, allowed and redirected actions for specific URLs following the same process that was used for defining categories. Website URLs takes precedence over categories.

## Validate the Configuration

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured SaaS application. If enhanced security is disabled, the app launches within the local browser, otherwise the embedded browser is used
*  The user automatically signs on to the app
*  The appropriate enhanced security policies are applied
*  If configured, select a URL within the SaaS app that is in the blocked, allowed and redirected categories
*  If configured, select a URL within the SaaS app that is in the blocked, allowed and redirected URLs
*  The SaaS App successfully launches

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser directs the browser to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app launches in the local browser if enhanced security is disabled. If enhanced security is enabled, a Secure Browser instance launches the SaaS app

## Troubleshooting

### Enhanced Security Policies Failing

Users might experience the enhanced security policies (watermark, printing, or cliboard access) fail. Typically, this happens because the SaaS application uses multiple domain names. Within the application configuration settings for the SaaS app, there was an entry for **Related Domains**.

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_add-saas-app-02.png)

The enhanced security policies are applied onto to those related domains. To identify missing domain names, an administrator can access the SaaS app with a local browser and do the following:

*  Navigate to the section of the app where the policies fail
*  In Google Chrome and Microsoft Edge (Chromium version), select the three dots in the upper right side of the browser to show a menu screen.
*  Select **More Tools**.
*  Select **Developer Tools**
*  Within the developer tools, select **Sources**. This provides a list of access domain names for that section of the application. In order to enable the enhanced security policies for this portion of the app, those domain names must be entered into the **related domains** field within the app configuration. Related domains are added like the following `*.domain.com`

[![Enhanced Security Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_enhanced-security-troubleshooting-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-citrix-sso_enhanced-security-troubleshooting-01.png)
