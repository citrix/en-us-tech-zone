---
layout: doc
description: Learn how to set up a Citrix Access Control environment that is able to use Okta as the single sign-on provider for SaaS applications.
---
# Proof of Concept: Secure Access to Office 365 with Azure and Citrix Access Control

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

**Special Thanks:** Krishna Kumar

## Overview

As users access confidential content within Office 365, organizations must be able to simplify user login operations while still enforcing authentication standards. Organizations must be able to secure Office 365 even though it exist beyond the confines of the data center. Citrix Workspace provides organizations with enhanced security controls for Office 365.

In this scenario, a user authenticates to Citrix Workspace using either Active Directory or Azure Active Directory as the primary user directory.

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_ad-dir-azure-sso.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_ad-dir-azure-sso.png)

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_aad-dir-azure-sso.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_aad-dir-azure-sso.png)

 If the Citrix Access Control Service is assigned to the Citrix subscription, enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links are applied on top of the Azure-based SaaS applications.

The following animation shows a user accessing a SaaS application with Azure providing SSO and secured with Citrix Access Control.

 [![Okta SSO Demo](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_demo-video.gif)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_demo-video.gif)

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

[![Workspace URL](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-001.png)

### Enable Services

From the Service Integration tab, enable the following services to support the secure access to SaaS apps use case

1.  Gateway
2.  Secure Browser

[![Workspace Services](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-002.png)

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

## Federate Azure Authentication to Citrix Workspace

To successfully federate Office 365 with Citrix Workspace, the administrator needs to do the following

*  Configure SaaS App
*  Authorize SaaS App
*  Verify Authentication Domain
*  Configure Domain Federation

### Configure SaaS App

With the domain verified within Azure, an Office 365 SaaS app can get configured within Citrix Workspace.

*  Within Citrix cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Office 365**

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)

*  Select **Next**
*  In the **App details** screen, change the **Name**, **Icon**, and **Description** as needed while leaving all remaining entries unchanged.

[![Setup SaaS App 03](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)

*  Select **Next**
*  Enhanced security policies uses the related domains field to determine which URLs to secure. One related domain is automatically added based on the default URL. Enhanced security policies require related domains for any URL associated with the application. Office 365 includes many URLs, which can be found in the section Office 365 Related Domains.

*  In the **Enhanced Security** window, select the appropriate security policies for the environment

[![Setup SaaS App 04](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-04.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-04.png)

*  In the **Single Sign-On** window, verify the **Name ID Format=Persistent** and **Name ID=Active Directory GUID**
*  Under Advanced attributes, verify **Attribute Name=IDPEmail**, **Attribute Format=Unspecified**, and **Attribute Value=Email**
*  Select **Download** to capture the **CRT-based** certificate.
*  Next to the **Login URL**, select the **Copy** button to capture the Login URL. This URL gets used later.
*  Select the **SAML Metadata** link

[![Setup SaaS App 05](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-05.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-05.png)

*  Within the SAML Metadata file, look for **EntityID**. Copy the entire URL and store for later use. Once captured, the SAML Metadata file can be closed.

[![Setup SaaS App 06](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-06.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-06.png)

*  Select **Save**
*  Select **Finish** to complete the configuration of the Office 365 SaaS apps.

### Authorize SaaS App

*  Within Citrix Cloud, select **Library** from the menu

[![Authorize SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-01.png)

*  Find the Office 365 app and select **Manage Subscribers**
*  Add the appropriate users/groups who are authorized to launch the app

[![Authorize SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-02.png)

### Verify Authentication Domain

To federate authentication to Citrix Workspace, Azure must verify the fully qualified domain name.  Within the Azure Portal, do the following:

*  Access Azure Active Directory
*  Select **Custom domain names** in the navigation window
*  Select **Add custom domain**
*  Enter the fully qualified domain name

[![Domain Verification 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-01.png)

*  Select **Add domain**
*  Azure provides records to incorporate into your domain name registrar. Once done, select **Verify**.

[![Domain Verification 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-02.png)

*  Once complete, the domain should contain a verified mark

[![Domain Verification 03](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-03.png)

### Configure Domain Federation

The final configuration is to have Azure use Citrix Workspace as the federated authority for the verified domain. Configuring federation must be done with PowerShell.

*  Launch PowerShell
*  Add the appropriate modules with the following commands

```
Install-Module AzureAD -Force
Import-Module AzureAD -Force
Install-Module MSOnline -Force
Import-module MSOnline -Force
```

*  Connect to Microsoft Online via PowerShell and authenticate

```
Connect-MSOLService
```

*  Verify the domain is currently set to **Managed** within Azure by running the following PowerShell command

```
Get-MsolDomain
```

[![Domain Federation 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-01.png)

*  Use the following code in a PowerShell script to make this domain **Federated** by changing the variables to align with your environment

```
 $dom = "workspaces.wwco.net" # The fully qualified domain name verified within Azure
 $fedBrandName = "CitrixWorkspaceSAMLIdP" # A name to help remember the configuration purpose
 $IssuerUri = "https://citrix.com/fdafdjk4" # The entityID taken from the Citrix Worksapce SAML Metadata file
 $logoffuri = "https://app.netscalergateway.net/cgi/logout" # Standard entry for all. Do not change
 $uri = "https://app.netscalergateway.net/ngs/dhhn4j3mf1kc/saml/login?APPID=8dd87428-460b-4358-a3c2-609451e8f5be" # The Login URL from the Citrix Workspace Office 365 app configuration
 $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("e:\CitrixCloud.crt") # Path to the downloaded certificate file from Citrix Workspace
 $certData = [system.convert]::tobase64string($cert.rawdata)
 Set-MsolDomainAuthentication |
    -DomainName $dom |
    –federationBrandName $fedBrandName |
    -Authentication Federated |
    -PassiveLogOnUri $uri |
    -LogOffUri $logoffuri |
    -SigningCertificate $certData |
    -IssuerUri $IssuerUri |
    -PreferredAuthenticationProtocol SAMLP
```

*  Verify the domain is currently set to **Federated** within Azure by running the following PowerShell command

```
Get-MsolDomain
```

[![Domain Federation 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-02.png)

*  Verify the federation settings in Azure by running the following PowerShell command

```
Get-MsolDomainFederationSettings -DomainName $dom
```

[![Domain Federation 03](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_domain-federation-03.png)

### Validate

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the configured SaaS application
*  Observe the Okta sign-on process briefly appearing
*  The SaaS App successfully launches

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser redirects to Okta then to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app launches with Okta providing single sign-on

## Define website filtering policies

Citrix Access Control service provides website filtering within SaaS and Web apps to help protect the user from phishing attacks. The following shows how to setup website filtering policies.

*  From Citrix Cloud, **Manage** within the Access Control tile

[![Citrix Access Control 1](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-01.png)

*  If this guide was followed the **Set up end user authentication** step and the **Configure end user access to SaaS, web and virtual applciations** steps are complete. Select **Configure Content Access**
*  Select **Edit**
*  **Enable** the **Filter website categories** option
*  Withint the **Blocked categories** box, select **Add**
*  Select the categories to block users from accessing

[![Citrix Access Control 2](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-02.png)

*  When all applicable categories are selected, select **Add**

[![Citrix Access Control 3](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_access-control-03.png)

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

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_add-saas-app-02.png)

The enhanced security policies are applied onto to those related domains. To identify missing domain names, an administrator can access the SaaS app with a local browser and do the following:

*  Navigate to the section of the app where the policies fail
*  In Google Chrome and Microsoft Edge (Chromium version), select the three dots in the upper right side of the browser to show a menu screen.
*  Select **More Tools**.
*  Select **Developer Tools**
*  Within the developer tools, select **Sources**. This provides a list of access domain names for that section of the application. In order to enable the enhanced security policies for this portion of the app, those domain names must be entered into the **related domains** field within the app configuration. Related domains should be added like the following `*.domain.com`

[![Enhanced Security Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_enhanced-security-troubleshooting-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-okta-sso_enhanced-security-troubleshooting-01.png)

## Office 365 Related Domains