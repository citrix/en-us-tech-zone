---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Learn how to set up VPN-less access to an internal web application with Citrix Secure Private Access, utilizing Citrix-provided SSO.
tz_title: Secure Access to Internal Web Applications with Citrix Secure Private Access
tz_products: citrix-secure-private-access;
---
# PoC Guide: Secure Access to Internal Web Applications with Citrix Secure Private Access

## Overview

With remote work, users need access to internal web-based applications. Providing a better experience means avoiding a VPN deployment model, which often results in the following challenges:

*  VPN Risk 1: Are difficult to install and configure
*  VPN Risk 2: Require users install VPN software on endpoint devices, which might utilize an unsupported operating system
*  VPN Risk 3: Require the configuration of complex policies to prevent an untrusted endpoint device from having unrestricted access to the corporate network, resources, and data.
*  VPN Risk 4: Difficult to keep security policies synchronized between VPN infrastructure and on-premises infrastructure.

To improve the overall user experience, organizations must be able to unify all sanctioned apps, simplify user login operations while still enforcing authentication standards.

[![Single sign-on Overview](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_vpn-less-web-apps.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_vpn-less-web-apps.png)

Organizations must be able to deliver and secure SaaS, web, Windows, Linux applications, and desktops even though some of these resources exist beyond the confines of the data center and are able to access resources outside of the data center. Citrix Workspace provides organizations with secure, vpn-less access to user-authorized resources.

In this proof of concept scenario, a user authenticates to Citrix Workspace using Active Directory, Azure Active Directory, Okta, Google, or Citrix Gateway as the primary user directory. Citrix Workspace provides single sign-on services for a defined set of enterprise web applications.

[![Single sign-on Overview](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_identity-brokering-web-app.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_identity-brokering-web-app.png)

 If the Citrix Secure Private Access service is assigned to the Citrix subscription, enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links are applied on top of the web applications.

The animation shows a user accessing a web application with Citrix-provided SSO and secured with Citrix Secure Private Access service.

 [![Citrix SSO Demo](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_demo-video.gif)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_demo-video.gif)

This demonstration shows a flow where the user launches the application from within Citrix Workspace, which uses the vpn-less connection to the data center. Because the user accesses an internal web app from an external device, the access request must come from within Citrix Workspace.

This proof of concept guide demonstrates how to:

1.  Setup Citrix Workspace
2.  Integrate a primary user directory
3.  Incorporate Single sign-on for Outlook Web Access, which is located within the data center
4.  Define website filtering policies
5.  Validate the configuration

## Setup Citrix Workspace

The initial steps for setting up the environment is to get Citrix Workspace prepared for the organization, which includes

1.  Setting up the Workspace URL
1.  Enabling the appropriate services

### Set Workspace URL

1.  Connect to [Citrix cloud](https://cloud.com) and log in as your administrator account
1.  Within Citrix Workspace, access **Workspace Configuration** from the upper-left menu
1.  From the **Access** tab, enter a unique URL for the organization and select Enabled

[![Workspace URL](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_workspace-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_workspace-config-001.png)

### Enable Services

From the **Service Integration** tab, enable the following services to support the secure access to web apps use case

1.  Gateway
2.  Secure Browser

[![Workspace Services](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_workspace-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_workspace-config-002.png)

### Verify

Citrix Workspace takes a few moments to update services and URL settings. From a browser, verify the custom Workspace URL is active. However, logon is not available until a primary user directory gets defined and configured.

## Integrate a Primary User Directory

Before users can authenticate to Workspace, a [primary user directory](/en-us/citrix-workspace/secure.html) must be configured. The primary user directory is the only identity the user requires as all requests for apps within Workspace utilizes single sign-on to secondary identities.

An organization can use any one of the following primary user directories

*  [Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guide.
*  [Active Directory with Time-Based One Time Password](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp): Active Directory-based authentication can also include multifactor authentication with a Time-based One Time Password (TOTP). This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html#active-directory-authentication) details the required steps to enable this authentication option.
*  [Azure Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#azure-active-directory): Users can authenticate to Citrix Workspace with an Azure Active Directory identity. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html) provides details on configuring this option.
*  [Citrix Gateway](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#citrix-gateway): Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html) provides details on the integration.
*  [Okta](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#okta): Organizations can use Okta as the primary user directory for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/okta-identity.html) provides instructions for configuring this option.

## Configure Single Sign-on

To successfully integrate web apps with Citrix Workspace, the administrator needs to do the following

*  Deploy Gateway Connector
*  Configure Web app
*  Authorize Web app

### Deploy Gateway Connector

*  Within Citrix cloud, select **Resource Locations** from the menu bar.

[![Gateway Connector 01](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-01.png)

*  Within the resource location associated with the site containing the web app, select **Gateway Connectors**
*  Select **Add a Gateway Connector**
*  Download the image associated with the appropriate hypervisor. Leave this browser window open
*  Once downloaded, import the image into the hypervisor
*  When the image starts, it will provide the URL to use to access the console

[![Gateway Connector 02](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-02.png)

*  Log into the Connector and change the admin password and set the network IP address
*  To provide single sign-on for certain on-prem applications (SharePoint for example), configure an account to perform Kerberos Constrained Delegation
*  Return to the browser window that downloaded the Connector image. Select **Get Activation Code**

[![Gateway Connector 03](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_connector-03.png)

*  Add the activation code to the on-premises Gateway Connector configuration screen. If done correctly, the Gateway Connector shows an established connection with the Citrix Gateway Service.

### Configure Web App

*  Within Citrix cloud, select **Manage** from the Gateway tile.

[![Setup Web App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Skip**

[![Setup Web App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-02.png)

*  In the App details window, select **Inside my corporate network**
*  Provide a name for the application
*  Enter the URL for the web application
*  Add additional related domains as necessary for the web application
*  Add an application specific icon if necessary

***Note**: Enhanced security policies use the related domains field to determine the URLs to secure. One related domain is automatically added based on the URL in the previous step. Enhanced security policies require related domains for the application. If the application uses multiple domain names, each must be added into the related domains field, which is often `*.<companyID>.company.com` (as an example `*.mail.citrix.com`)*

[![Setup Web App 03](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-03.png)

*  In the **Enhanced Security** window, select the appropriate security policies for the environment
*  Select **Next**

[![Setup Web App 04](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-04.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-04.png)

*  Select the appropriate resource location hosting the web application. If a Gateway Connector was not previously installed and configured, it must be done now.
*  Select **Next**

[![Setup Web App 05](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-05.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-05.png)

*  In the **Single Sign-On** window, select the appropriate SSO option for the web application.  This often requires help from the web app owner.  For OWA, select **Form-Based**
*  Enter the appropriate information for the web app's logon form. This is app-specific

[![Setup Web App 06](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-06.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-06.png)

*  Select **Save**
*  Select **Finish**

### Authorize Web App

*  Within Citrix cloud, select **Library** from the menu

[![Authorize Web App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_authorize-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_authorize-saas-app-01.png)

*  Find the web app and select **Manage Subscribers**
*  Add the appropriate users/groups who are authorized to launch the app

[![Authorize Web App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_authorize-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_authorize-saas-app-02.png)

### Validate

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured web application
*  The web App successfully launches

SP-Initiated Validation

*  Access to internal web apps must be initiated from within Citrix Workspace

## Define website filtering policies

Citrix Secure Private Access service provides website filtering within SaaS and Web apps to help protect the user from phishing attacks. The following shows how to set up website filtering policies.

*  From Citrix cloud, **Manage** within the Secure Private Access tile

[![Citrix Secure Private Access 1](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_secure-workspace-access-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_secure-workspace-access-01.png)

*  If this guide was followed, the **Set up end user authentication** step and the **Configure end user access to SaaS, web and virtual applciations** steps are complete. Select **Configure Content Access**
*  Select **Edit**
*  **Enable** the **Filter website categories** option
*  Within the **Blocked categories** box, select **Add**
*  Select the categories to block users from accessing

[![Citrix Secure Private Access 2](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_access-control-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_access-control-02.png)

*  When all applicable categories are selected, select **Add**

[![Citrix Secure Private Access 3](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_access-control-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_access-control-03.png)

*  Do the same for allowed categories
*  Do the same for redirected categories. These categories redirect to a Secure Browser instance
*  If needed, admins can filter denied, allowed and redirected actions for specific URLs following the same process that was used for defining categories. Website URLs take precedence over categories.

## Validate the Configuration

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured web application. The app launches within the embedded browser.
*  The user automatically signs on to the app
*  The appropriate enhanced security policies are applied
*  If configured, select a URL within the web app that is in the blocked, allowed and redirected categories
*  If configured, select a URL within the web app that is in the blocked, allowed and redirected URLs

SP-Initiated Validation

*  Access to internal web apps must be initiated from within Citrix Workspace

## Web Application Configuration Examples

### OWA

The SSO configuration for Outlook Web Access is as follows:

[![Setup Web App 06](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-06.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-06.png)

### SharePoint

An on-premises deployment for SharePoint can support different types of authentication (Kerberos, Forms-based, and SAML). The SSO configuration for an internal SharePoint site using Kerberos is as follows:

[![Authorize Web App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_sharepoint-config-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_sharepoint-config-01.png)

## Troubleshooting

### Enhanced Security Policies Failing

Users might experience the enhanced security policies (watermark, printing, or clipboard access) fail. Typically, this happens because the web application uses multiple domain names. Within the application configuration settings for the web app, there was an entry for **Related Domains**.

[![Setup Web App 03](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_add-web-app-03.png)

The enhanced security policies are applied onto to those related domains. To identify missing domain names, an administrator can access the web app with a local browser and do the following:

*  Navigate to the section of the app where the policies fail
*  In Google Chrome and Microsoft Edge (Chromium version), select the three dots in the upper right side of the browser to show a menu screen.
*  Select **More Tools**.
*  Select **Developer Tools**
*  Within the developer tools, select **Sources**. This provides a list of access domain names for that section of the application. In order to enable the enhanced security policies for this portion of the app, those domain names must be entered into the **related domains** field within the app configuration. Related domains are added like the following `*.domain.com`

[![Enhanced Security Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_enhanced-security-troubleshooting-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-web-citrix-sso_enhanced-security-troubleshooting-01.png)
