---
layout: doc
description: Learn how to setup a Citrix Access Control environment that is able to use Citrix, Okta or Microsoft as the single sign-on provider for SaaS applications.
---
# Proof of Concept: Secure Access to SaaS Applications

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

## Overview

As users consume more SaaS-based applications, organizations must be able to unify all sanctioned apps, simplify user login operations while still enforcing authentication standards. Organizations must be able to secure these applications even though they exist beyond the confines of the data center. Citrix Workspace provides organizations with secure access to SaaS apps.

This proof of concept guide demonstrates how to:

1.  Setup Citrix Workspace
1.  Integrate a primary user directory
1.  Incorporate Single Sign-On for SaaS applications
1.  Define website filtering policies
1.  Validate the configuration

## Setup Citrix Workspace

The initial steps for setting up the environment is to get Citrix Workspace prepared for the organization. This involves

1.  Setting up the Workspace URL
1.  Enabling the appropriate services

### Set Workspace URL

1.  Connect to [Citrix Cloud](https://cloud.com) and log in as your administrator account
1.  Within Citrix Workspace, access **Workspace Configuration** from the upper-left menu
1.  From the **Access** tab, enter a unique URL for the organization and select Enabled

[![Workspace URL](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_workspace-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_workspace-config-001.png)

### Enable Services

From the Service Integration tab, enable the following services to support the secure access to SaaS apps use case

1.  Gateway
2.  Secure Browser

[![Workspace Services](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_workspace-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_workspace-config-002.png)

### Verify

Citrix Workspace may take a few moments to update services and URL settings.  From a browser, verify the custom Workspace URL is active. However, logon will not be available until a primary user directory gets defined and configured.

## Integrate a Primary User Directory

Before users can authenticate to Workspace, a [primary user directory](https://docs.citrix.com/en-us/citrix-workspace/secure.html) must be configured. The primary user directory is the only identity the user requires as all requests for apps within Workspace utilizes single sign-on to secondary identities.

An organization can use any one of the following primary user directories

*  [Active Directory](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guide.
*  [Active Directory with Time-Based One Time Password](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp): Active Directory-based authentication can also include multi-factor authentication with a Time-based One Time Password (TOTP). This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html#active-directory-authentication) details the required steps to enable this authentication option.
*  [Azure Active Directory](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#azure-active-directory): Users can authenticate to Citrix Workspace with an Azure Active Directory identity.  This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html) provides details on configuring this option.
*  [Citrix Gateway](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#citrix-gateway): Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace. This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html) provides details on the integration.
*  [Okta](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#okta): Organizations can use Okta as the primary user directory for Citrix Workspace.  This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/okta-identity.html) provides instructions for configuring this option.

## Option 1: Citrix-Provided Single Sign-On to SaaS Apps (Coming Soon)

## Option 2: Okta-Provided Single Sign-On to SaaS Apps

In this scenario, a user authenticates to Citrix Workspace using either Active Directory or Okta as the primary user directory. Okta also provides single sign-on services for a defined set of SaaS applications.

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_ad-dir-okta-sso.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_ad-dir-okta-sso.png)

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-dir-okta-sso.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-dir-okta-sso.png)

 If the Citrix Access Control Service is assigned to the Citrix subscription, enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links are applied on top of the Okta-based SaaS applications.

 Assumptions:

*  Okta is already configured to provide SSO to Office 365 and other SaaS apps
*  Users can successfully sign into the Okta portal and launch Office 365 and other SaaS apps
*  Citrix Workspaces is already configured with Active Directory or Okta as the user’s primary identity directory.

To successfully integrate Okta apps with Citrix Workspace, the administrator needs to do the following

*  Identify SAML Login URL
*  Identify IdP Issuer URI
*  Setup SAML Identity Provider
*  Configure SaaS app
*  Authorize SaaS app
*  Setup IdP Routing

### Identify SAML Login URL

*  Log into Okta as an administrator
*  Select **Applications**
*  Select the application to add into Citrix Workspace. In this example, Microsoft Office 365 is used.
*  Under **General**, scroll down until the correct **App Embed Link** is located. This will be used as the SAML Login URL for Citrix Workspace.

[![SAML Login URL](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_app-embed-link.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_app-embed-link.png)

### Identify IdP Issuer URI

*  Log into Citrix cloud as an administrator
*  Under the **Identity and Access Management** section, select **API Access**
*  Capture the **customer ID** parameter.  This is used to create the IdP Issuer URI in the format: `https://cloud.com/<customerID>`

[![IdP Issuer URI](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_idp-issuer-uri.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_idp-issuer-uri.png)

### Setup SAML Identity Provider

Okta needs to use Citrix Workspace as a SAML identity provider, resulting in Okta becoming a service provider in the SAML configuration.

*  Log into Okta as an administrator
*  Select **Security** -> **Identity Providers**
*  Select **Add Identity Provider** -> **Add SAML 2.0 IdP**
[![Setup SAML IdP 01](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-01.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-01.png)
*  Provide a **Name**
*  For the IdP Username, use the following expression: **idpuser.userName** (this is case sensitive)
*  Match against should be **Okta Username or email**
*  If no match is found, select **Redirect to Okta sign-in page**
*  For the IdP Issuer URI, use the URL `https://cloud.com/<customerID>`. CustomerID is from the IdP Issuer URI section

[![Setup SAML IdP 02](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-02.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-02.png)

*  Leave this part of the process open until we are able to obtain the single sign-on URL and SSL certificate from Citrix Cloud.

### Configure SaaS App

*  Within Citrix cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Skip**
*  In the App details window, provide a **name** for the application
*  For the URL, use the **App Embed Link** from the Identity SAML Login URL section
*  For related domains, one is automatically added based on the entered URL added in the previous step. That specific related domain is associated with the Okta application link. Citrix Workspace requires additional related domains for the actual application, which will often be `*.<companyID>.SaaSApp.com` (as an example *.citrix.slack.com)

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-02.png)

*  In the **Enhanced Security** window, select the appropriate security policies for the environment
*  In the **Single Sign-On** window, select **Download** to capture the PEM-based certificate.
*  Select the **Copy** button to capture the Login URL

[![Setup SaaS App 03](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-03.png)

*  Switch back to the Okta configuration
*  For the **IdP Single Sign-On URL**, use the Citrix Login URL copied from the previous step. It should resemble `https://app.netscalergateway.net/ngs/<customerid>/saml/login?APPID=2347894324327`
*  Add the certificate to the SAML-based IdP configuration.

[![Setup SaaS App 04](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-04.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-04.png)

*  Once the wizard completes, copy the **Assertion Consumer Service URL** and the **Audience URI**.

[![Setup SaaS App 05](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-05.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-05.png)

*  Switch back to the Citrix configuration.
*  In the **Single Sign-On** window, for the **Assertion URL**, use the **Assertion Consumer Service URL** item obtained from the SAML Identity Provider section
*  For the **Audience**, use the **Audience URI** item obtained from the SAML Identity Provider section.
*  The Name ID Format and Name ID can remain as email. Okta uses the Email address to associate with an Okta user.

[![Setup SaaS App 06](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-06.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-06.png)

### Authorize SaaS App

*  Within Citrix cloud, select **Library** from the menu

[![Authorize SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_authorize-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-01.png)

*  Find the SaaS app and select **Manage Subscribers**
*  Add the appropriate users/groups who are authorized to launch the app

[![Authorize SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_authorize-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-02.png)

### Setup IdP Routing

So far, the configuration supports an IdP-initiated launch process, where the user launches the app from within Citrix Workspace. In order to enable an SP-initiated process, where the user launches the app with direct URL, Okta needs an IdP routing rule defined.

*  Within the Okta admin console, select **Security** - **Identity Providers**
*  Select **Routing Rules**
*  Select **Add Routing Rule**
*  Provide a name for the rule
*  For the Use this **identity provider option**, select the Citrix identity provider created earlier

[![Okta Identity Provider Routing Rule](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-idp-routing.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-idp-routing.png)

***Note**: During the configuration, the Okta admin might be unable to sign into the Okta admin console because the inbound SAML configuration is incomplete.  If this happens, the admin can bypass the IdP routing rule by accessing the Okta enviornment witht he following address: `http://companyname.oka.com/login/default`*

### Validate

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured SaaS application
*  Observe the Okta sign-on process briefly appearing
*  The SaaS App should successfully launch

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser should redirect to Okta then to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app should launch with Okta providing single sign-on

## Option 3: Microsoft-Provided Single Sign-On to SaaS Apps (Coming Soon)

## Define website filtering policies

Citrix Access Control service provides website filtering within SaaS and Web apps to help protect the user from phishing attacks. The following shows how to setup website filtering policies.

*  From Citrix Cloud, **Manage** within the Access Control tile

[![Citrix Access Control 1](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-01.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-01.png)

*  If this guide was followed the **Set up end user authentication** step and the **Configure end user access to SaaS, web and virtual applciations** steps are complete. Select **Configure Content Access**
*  Select **Edit**
*  **Enable** the **Filter website categories** option
*  Withint the **Blocked categories** box, select **Add**
*  Select the categories to block users from accessing

[![Citrix Access Control 2](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-02.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-02.png)

*  When all applicable categories are selected, select **Add**

[![Citrix Access Control 3](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-03.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_access-control-03.png)

*  Do the same for allowed categories
*  Do the same for redirected categories. These categories redirect to a Secure Browser instance
*  If needed, admins can filter denied, allowed and redirected actions for specific URLs following the same process that was used for defining categories.  Website URLs takes precedence over categories.

## Validate the Configuration

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured SaaS application. If enhanced security is disabled, the app should launch within the local browser, otherwise the embedded browser is used
*  The user should automatically sign on to the app
*  The appropriate enhanced security policies are applied
*  If configured, select a URL within the SaaS app that is in the blocked, allowed and redirected categories
*  If configured, select a URL within the SaaS app that is in the blocked, allowed and redirected URLs
*  The SaaS App should successfully launch

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser should direct the browser to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app should launch in the local browser if enhanced security is disabled. If enhanced security is enabled, a Secure Browser instance launches the SaaS app
