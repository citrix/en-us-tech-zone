---
layout: doc
description: Learn how to setup a Citrix Workspace environment able to use Citrix or Okta as the single sign-on provider for SaaS applications.
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
1.  Provide enhanced security and website filtering

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

Citrix Workspace may take a few moments to update services and URL settings.  From a browser, verify the custom Workspace URL is active.

## Integrate a Primary User Directory

Before users can authenticate to Workspace, a [primary user directory](https://docs.citrix.com/en-us/citrix-workspace/secure.html) must be configured. The primary user directory is the only identity the user will require as all requests for apps within Workspace will utilize single sign-on to secondary identities.

An organization can use any one of the following primary user directories

*  [Active Directory](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guide.
*  [Active Directory with Time-Based One Time Password](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp): Active Directory-based authentication can also include multi-factor authentication with a Time-based One Time Password (TOTP). This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html#active-directory-authentication) details the required steps to enable this authentication option.
*  [Azure Active Directory](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#azure-active-directory): Users can authenticate to Citrix Workspace with an Azure Active Directory identity.  This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html) provides details on configuring this option.
*  [Citrix Gateway](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#citrix-gateway): Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace. This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html) provides details on the integration.
*  [Okta](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#okta): Organizations can use Okta as the primary user directory for Citrix Workspace.  This [guide](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/okta-identity.html) provides instructions for configuring this option.

## Option 1: Citrix-Provided Single Sign-On to SaaS Apps

## Option 2: Okta-Provided Single Sign-On to SaaS Apps

In this scenario, a user authenticates to Citrix Workspace using either Active Directory or Okta as their primary user directory. Defined SaaS applications leverage Okta to provide single sign-on services.

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_ad-dir-okta-sso.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_ad-dir-okta-sso.png)

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-dir-okta-sso.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-dir-okta-sso.png)

 Citrix Workspace incorporates enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links.

Assumptions:

*  Okta is already configured to provide SSO to Office 365 and other SaaS apps
*  User can successfully sign into the Okta portal and launch Office 365 and other SaaS apps
*  Citrix Workspaces is already configured with Active Directory set as the directory for the user’s primary identity.

To successfully integrate Okta apps with Citrix Workspace, the administrator needs to do the following

*  Identify SAML Login URL
*  Identify IdP Issuer URI
*  Setup SAML Identity Provider
*  Configure SaaS app
*  Authorize SaaS app

### Identify SAML Login URL

*  Log into Okta as an administrator
*  Select **Applications**
*  Select the application to add into Citrix Workspace. In this example, Microsoft Office 365 is used.
*  Under **General**, scroll down until the correct **App Embed Link** is located. This will be used as the SAML Login URL for Citrix Workspace.

[![SAML Login URL](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_app-embed-link.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_app-embed-link.png)

### Identify IdP Issuer URI

*  Log into Citrix cloud as an administrator
*  Under the **Identity and Access Management** section, select **API Access**
*  Capture the **customer ID** parameter.  This is used to create the IdP Issuer URI in the format: https://cloud.com/<customerID>

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
*  For the IdP Issuer URI, use the URL https://cloud.com/<customerID>. CustomerID is from the IdP Issuer URI section
*  For the IdP Single Sign-On URL, use the following URL https://app.netscalergateway.net/ngs/<customerid>/saml/login substituting customer ID, which was obtained from the IdP Issuer URI section
[![Setup SAML IdP 02](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-02.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_okta-add-saml-idp-02.png)
*  Leave this part of the process open until we are able to obtain an SSL certificate from Citrix Cloud.

### Configure SaaS App

*  Within Citrix cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_secure-access-saas-apps_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Skip**
*  In the App details window, provide a **name** for the application
*  For the URL, use the **App Embed Link** from the Identity SAML Login URL section
*  For related domains, one is automatically added based on the entered URL added in the previous step. That specific related domain is associated with the Okta application link. Citrix Workspace requires additional related domains for the actual application, which will often be *.<companyID>.SaaSApp.com (as an example *.citrix.slack.com).  
