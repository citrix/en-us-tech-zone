---
layout: doc
description: Learn how to set up a Citrix Access Control environment that provides single sign-on and enhanced security to Office 365.
---
# Proof of Concept: Secure Access to Microsoft 365 with Citrix Access Control

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

**Special Thanks:** Krishna Kumar

## Overview

As users access confidential content within Microsoft 365 (Office 365), organizations must be able to simplify user login operations while still enforcing authentication standards. Organizations must be able to secure Microsoft 365 even though it exists beyond the confines of the data center. Citrix Workspace provides organizations with enhanced security controls for Microsoft 365.

In this scenario, a user authenticates to Citrix Workspace using either Active Directory as the primary user directory.

[![Active Directory and Okta SSO](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_ad-dir-azure-sso.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_ad-dir-azure-sso.png)

 If the Citrix Access Control Service is assigned to the Citrix subscription, enhanced security policies, ranging from applying screen-based watermarks, restricting printing/downloading actions, screen grabbing restrictions, keyboard obfuscation, and protecting users from untrustworthy links are applied on top of the Microsoft 365 applications.

The following animation shows a user accessing Microsoft 365 with SSO and secured with Citrix Access Control.

 [![Okta SSO Demo](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_demo-video.gif)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_demo-video.gif)

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

From the **Service Integration** tab, enable the following services to support the secure access to SaaS apps use case

1.  Gateway
2.  Secure Browser

[![Workspace Services](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_workspace-config-002.png)

### Verify

Citrix Workspace takes a few moments to update services and URL settings. From a browser, verify the custom Workspace URL is active. However, logon will not be available until a primary user directory gets defined and configured.

## Integrate a Primary User Directory

Before users can authenticate to Workspace, a [primary user directory](/en-us/citrix-workspace/secure.html) must be configured. The primary user directory is the only identity the user requires as all requests for apps within Workspace utilizes single sign-on to secondary identities.

An organization can use any one of the following primary user directories with Microsoft 365:

*  [Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) guide.
*  [Active Directory with Time-Based One Time Password](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp): Active Directory-based authentication can also include multifactor authentication with a Time-based One Time Password (TOTP). This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html#active-directory-authentication) details the required steps to enable this authentication option.
*  [Citrix Gateway](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#citrix-gateway): Organizations can utilize an on-premises Citrix Gateway to act as an identity provider for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html) provides details on the integration.
*  [Okta](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#okta): Organizations can use Okta as the primary user directory for Citrix Workspace. This [guide](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/okta-identity.html) provides instructions for configuring this option.

***Note**: At this time, utilizing Azure Active Directory as the user's primary identity will not function properly.*

## Federate Azure Authentication to Citrix Workspace

To successfully federate Microsoft 365 with Citrix Workspace, the administrator needs to do the following

*  Configure SaaS App
*  Authorize SaaS App
*  Verify Authentication Domain
*  Configure Domain Federation

### Configure SaaS App

With the domain verified within Azure, a Microsoft 365 SaaS app can get configured within Citrix Workspace.

*  Within Citrix Cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Office 365**

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)

*  Select **Next**
*  In the **App details** screen, change the **Name**, **Icon**, and **Description** as needed while leaving all remaining entries unchanged.

[![Setup SaaS App 03](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)

*  Select **Next**
*  Enhanced security policies uses the related domains field to determine which URLs to secure. One related domain is automatically added based on the default URL. Enhanced security policies require related domains for any URL associated with the application. Microsoft 365 includes many URLs, which can be found in the section [Microsoft 365 Related Domains](/en-us/tech-zone/learn/poc-guides/access-control-azuresso-o365.html#office-365-related-domains).

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
*  Select **Finish** to complete the configuration of the Microsoft 365 SaaS apps.

### Authorize SaaS App

*  Within Citrix Cloud, select **Library** from the menu

[![Authorize SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-01.png)

*  Find the Microsoft 365 app and select **Manage Subscribers**
*  Add the appropriate users/groups who are authorized to launch the app

[![Authorize SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_authorize-saas-app-02.png)

### Verify Authentication Domain

To federate authentication to Citrix Workspace, Azure must verify the fully qualified domain name. Within the Azure Portal, do the following:

*  Access Azure Active Directory
*  Select **Custom domain names** in the navigation window
*  Select **Add custom domain**
*  Enter the fully qualified domain name

[![Domain Verification 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-01.png)

*  Select **Add domain**
*  Azure provides records to incorporate into your domain name registrar. Once done, select **Verify**.

[![Domain Verification 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_verify-domain-02.png)

*  Once complete, the domain contains a verified mark

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
 $uri = "https://app.netscalergateway.net/ngs/dhhn4j3mf1kc/saml/login?APPID=8dd87428-460b-4358-a3c2-609451e8f5be" # The Login URL from the Citrix Workspace Microsoft 365 app configuration
 $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2("e:\CitrixCloud.crt") # Path to the downloaded certificate file from Citrix Workspace
 $certData = [system.convert]::tobase64string($cert.rawdata)

 Set-MsolDomainAuthentication `
     -DomainName $dom `
     –federationBrandName $fedBrandName `
     -Authentication Federated `
     -PassiveLogOnUri $uri `
     -LogOffUri $logoffuri `
     -SigningCertificate $certData `
     -IssuerUri $IssuerUri `
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

***Note: If the federation settings need to be removed, run the following PowerShell command*

```
Set-MsolDomainAuthentication -DomainName $dom -Authentication Managed
```

### Validate

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select the Microsoft 365 application
*  Observe the URL to see it briefly redirect through Azure
*  The Microsoft 365 portal successfully launches

SP-Initiated Validation

*  Launch a browser
*  Go to the company-defined URL for the SaaS application
*  The browser redirects to Okta then to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, the SaaS app launches with Okta providing single sign-on

## Define website filtering policies

Citrix Access Control service provides website filtering within SaaS and Web apps to help protect the user from phishing attacks. The following shows how to set up website filtering policies.

*  From Citrix Cloud, **Manage** within the Access Control tile

[![Citrix Access Control 1](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-01.png)

*  If this guide was followed, the **Set up end user authentication** step and the **Configure end user access to SaaS, web and virtual applciations** steps are complete. Select **Configure Content Access**
*  Select **Edit**
*  **Enable** the **Filter website categories** option
*  Within the **Blocked categories** box, select **Add**
*  Select the categories to block users from accessing

[![Citrix Access Control 2](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-02.png)

*  When all applicable categories are selected, select **Add**

[![Citrix Access Control 3](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_access-control-03.png)

*  Do the same for allowed categories
*  Do the same for redirected categories. These categories redirect to a Secure Browser instance
*  If needed, admins can filter denied, allowed, and redirected actions for specific URLs following the same process that was used for defining categories. Website URLs take precedence over categories.

## Validate the Configuration

IdP-Initiated Validation

*  Log into to Citrix Workspace as a user
*  Select Microsoft 365. If enhanced security is disabled, the app launches within the local browser, otherwise the embedded browser is used
*  The user automatically signs on to the app
*  The appropriate enhanced security policies are applied
*  If configured, select a URL within the SaaS app that is in the blocked, allowed, and redirected categories
*  If configured, select a URL within the SaaS app that is in the blocked, allowed, and redirected URLs
*  The SaaS App successfully launches

SP-Initiated Validation

*  Launch a browser
*  Go to the [Office 365](https://www.office365.com) website and select **Sign In**
*  Enter the user name.
*  The browser redirects the browser to Citrix Workspace for authentication
*  Once the user authenticates with the primary user directory, Microsoft 365 launches in the local browser if enhanced security is disabled. If enhanced security is enabled, a Secure Browser instance launches Microsoft 365

## Microsoft 365 Related Domains

When creating a new app within Citrix Workspace, a related domain field is available. The enhanced security policies utilize these related domains to determine when to enforce the policy.

The following list are the current domains associated with Microsoft 365.

***Note:** These domains can change at any time*

*  *.office.com
*  *.office365.com
*  *.sharepoint.com
*  *.live.com
*  *.onenote.com
*  *.microsoft.com
*  *.powerbi.com
*  *.dynamics.com
*  *.microsoftstream.com
*  *.powerapps.com
*  *.yammer.com
*  *.windowsazure.com
*  *.msauth.net
*  *.msauthimages.net
*  *.msocdn.com
*  *.microsoftonline.com
*  *.windows.net
*  *.microsoftonline-p.com
*  *.akamaihd.net
*  *.sharepointonline.com
*  *.officescriptsservice.com
*  *.live.net
*  *.office.net
*  *.msftauth.net

## Microsoft 365 Apps

If it is preferable to launch a specific Microsoft 365 app (Word, PowerPoint, or Excel) instead of the Microsoft 365 portal, the administrator needs to create a separate application instance within Citrix Cloud for each app. Each app has a unique URL, which must include the correct value for federated domain, which was configured in this guide. The federated domain entry informs Azure to redirect to the correct federated domain configuration.

*  Word: `https://login.microsoftonline.com/login.srf?wa=wsignin1%2E0&rver=6%2E1%2E6206%2E0&wreply=https%3A%2F%2Foffice.live.com%2Fstart%2FWord.aspx%3Fauth%3D2&whr=`**federated domain**
*  PowerPoint: `https://login.microsoftonline.com/login.srf?wa=wsignin1%2E0&rver=6%2E1%2E6206%2E0&wreply=https%3A%2F%2Fwww.office.com%2Flaunch%2Fpowerpoint%3Fauth%3D2&whr=`**federated domain**
*  Excel: `https://login.microsoftonline.com/login.srf?wa=wsignin1%2E0&rver=6%2E1%2E6206%2E0&wreply=https%3A%2F%2Foffice.live.com%2Fstart%2FExcel.aspx%3Fauth%3D2&whr=`**federated domain**
*  CRM/Dynamics Online: `https://<tenant>.crm.dynamics.com/?whr=`**federated domain**
*  OneDrive for Business: `https://login.microsoftonline.com/login.srf?wa=wsignin1%2E0&rver=6%2E1%2E6206%2E0&wreply=https%3A%2F%2F<tenant>-my.sharepoint.com%2F&whr=`**federated domain**
*  Outlook Calendar: `https://outlook.office.com/owa/?realm=`**federated domain**`&path=/calendar/view/Month`
*  Outlook Web Access to Exchange Online: `https://outlook.com/owa/`**federated domain**
*  SharePoint Online: `https://login.microsoftonline.com/login.srf?wa=wsignin1%2E0&rver=6%2E1%2E6206%2E0&wreply=https%3A%2F%2F<tenant>.sharepoint.com%2F&whr=`**federated domain**

*  Within Citrix Cloud, select **Manage** from the Gateway tile.

[![Setup SaaS App 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-01.png)

*  Select **Add a Web/SaaS app**
*  In the Choose a template wizard, select **Office 365**

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-02.png)

*  Select **Next**
*  In the **App details** screen, use the application-specific URL from above and place it in the **URL** field.
*  Change the **Name**, **Icon**, and **Description** as needed while leaving all remaining entries unchanged.

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-individual-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-individual-saas-app-03.png)

*  Select **Next**
*  Enhanced security policies uses the related domains field to determine which URLs to secure. One related domain is automatically added based on the default URL. Enhanced security policies require related domains for any URL associated with the application. Microsoft 365 includes many URLs, which can be found in the section [Microsoft 365 Related Domains](/en-us/tech-zone/learn/poc-guides/access-control-azuresso-o365.html#Microsoft-365-related-domains).

*  In the **Enhanced Security** window, select the appropriate security policies for the environment

[![Setup SaaS App 04](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-04.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-04.png)

*  In the **Single Sign-On** window, verify the **Relay State URL** is aligned with the application-specific URL from above
*  Verify the **Name ID Format=Persistent** and **Name ID=Active Directory GUID**
*  Enable the setting **Launch the app using the specified URL**. When the user launches an application from Workspace (IdP-initiated flow) the user will always end on the Azure portal page. This setting includes the Relay State URL within the SAML assertion, directing Azure to show the URL instead of the Azure portal page.
*  Under Advanced attributes, verify **Attribute Name=IDPEmail**, **Attribute Format=Unspecified**, and **Attribute Value=Email**

[![Setup SaaS App 05](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-individual-saas-app-05.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-individual-saas-app-05.png)

*  Select **Save**
*  Select **Finish** to complete the configuration of the Microsoft 365 SaaS apps.

## Stay Signed In

In the default configuration, Azure Active Directory displays a dialog box during the logon process allowing the users to remain signed in.

[![Persistent Sign In 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_persistent-signin-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_persistent-signin-01.png)

This is an Azure setting that can be easily changed by doing the following:

*  Within Azure, select **Azure Active Directory**
*  Select **Company Branding**
*  Select the enabled Locale
*  In the Edit company branding pane, select **No** in the **Show option to remain signed in**

[![Persistent Sign In 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_persistent-signin-02.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_persistent-signin-02.png)

*  Select **Save**

## Troubleshooting

### User Account Does Not Exist in the Directory

When trying to launch Microsoft 365, the user might receive the following error: The user account "account name" does not exist in the "GUID" directory.

[![User Account Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_troubleshooting-no-user-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_troubleshooting-no-user-01.png)

The following are suggestions on how to solve this issue:

*  Verify the user is licensed to use Microsoft 365 within the Microsoft 365 administrator console
*  Verify the identified email address within the error matches between the primary user directory, Azure Active Directory and Microsoft 365.

### Federation Realm Object

During validation, a user might receive the following error:

[![Federation Realm Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_troubleshooting-federation-realm-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_troubleshooting-federation-realm-01.png)

This is often caused by the domain not being verified or properly federated. Review the following sections of the PoC guide:

*  [Verify Authentication Domain](/en-us/tech-zone/learn/poc-guides/access-control-azuresso-o365.html#verify-authentication-domain)
*  [Configure Domain Federation](/en-us/tech-zone/learn/poc-guides/access-control-azuresso-o365.html#configure-domain-federation)

### Enhanced Security Policies Failing

Users might experience the enhanced security policies (watermark, printing, or clipboard access) fail. Typically, this happens because the SaaS application uses multiple domain names. Within the application configuration settings for the SaaS app, there was an entry for **Related Domains**.

[![Setup SaaS App 02](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_add-saas-app-03.png)

The enhanced security policies are applied onto to those related domains. The [Microsoft 365 Related Domains](/en-us/tech-zone/learn/poc-guides/access-control-azuresso-o365.html#Microsoft-365-related-domains) section of this PoC guide contains the initial set of related domains, which Microsoft can change at any time.

If the enhanced security policies fail to function within certain sections of the app, a related domain is still missing. To identify missing domain names, an administrator can access the SaaS app with a local browser and do the following:

*  Navigate to the section of the app where the policies fail
*  In Google Chrome and Microsoft Edge (Chromium version), select the three dots in the upper right side of the browser to show a menu screen.
*  Select **More Tools**.
*  Select **Developer Tools**
*  Within the developer tools, select **Sources**. This provides a list of access domain names for that section of the application. To enable the enhanced security policies for this portion of the app, those domain names must be entered into the **related domains** field within the app configuration. Related domains are added like the following `*.domain.com`

[![Enhanced Security Troubleshooting 01](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_enhanced-security-troubleshooting-01.png)](/en-us/tech-zone/learn/media/poc-guides_access-control-azuresso-o365_enhanced-security-troubleshooting-01.png)
