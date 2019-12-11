---
layout: doc
description: Learn how Citrix Workspace utilizes a secure primary identity to broker authorization to SaaS, web, mobile and virtual apps.
---
# Workspace Identity

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

A user’s primary workspace identity authorizes access to all workspace resource feeds including microapps, mobile apps, virtual apps, virtual desktops, and content. Citrix Workspace provides organizations with a choice in selecting the user’s primary identity provider.

## Identity Provider (IdP)

An identity provider is the final authority of the user’s identity. The identity provider is responsible for the policies to protect and secure the user’s identity, which includes password policies, multifactor authentication policies and lockout policies.

Before the cloud era, organizations had a single option for an identity provider: Windows Active Directory. But now, almost every system or service requiring a unique user account is acting like an identity provider. Facebook, LinkedIn, Twitter, Workday, and Google are all identity providers as each is the final authority on a user’s identity within their service. In addition, a common security recommendation, which is rarely followed, is to use different passwords for each identity to limit the impact of a stolen password.

The traditional approach to identity provides one of the worst user experiences imaginable. Users are constantly challenged to authenticate. Users are forced to remember unique, complex passwords for each service. Users are spending valuable time having passwords reset and accounts unlocked due to forgotten credentials.

Citrix Workspace provides a better alternative to the status quo. Citrix Workspace allows each organization to choose a primary identity from a growing list of options, which currently includes.

*  Windows Active Directory
*  Azure Active Directory
*  Okta
*  Citrix Gateway

[![Workspace Identity](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)

Citrix Workspace relies on the identity broker micro-service to manage authentication to the configured identity provider. A successful workspace authentication allows the resource feed µ-service to create a list of authorized resources available to the user.

However, many of these services have, or require an identity different from the user’s primary workspace identity as they utilize a different identity provider. The single sign-on µ-service translates the user’s primary workspace identity into a resource-specific identity using appropriate approaches like:

*  SAML
*  Kerberos
*  Forms
*  Virtual Smartcards

The Citrix Workspace approach of primary and secondary identities creates an experience where users physically authenticate once and all subsequent authentication challenges are automatically satisfied.

## Identity Brokering

An identity provider, and the related identity database, is the final authority for a user’s identity. However, each identity provider is different. Each identity provider incorporates different parameters, policies, and criteria for a user’s identity.

Within Citrix Workspace, the identity broker µ-service redirects the primary authentication request to the configured identity provider. Once the authentication request is transferred to the identity provider, authentication policies, within the identity provider, dictate how the user must authenticate, which often includes multifactor authentication policies.

Once the identity provider successfully authenticates the user, the identity broker µ-service receives a successful authentication response. The benefit of this approach is that organizations can change authentication policies within the identity provider without impacting Citrix Workspace.

In addition to a successful authentication response from the identity provider, the identity broker µ-service receives and translates the unique information from the identity provider into a standard set of claims about the user.

Claims about a user are simply information identifying the user, which can be a UPN, SID, GUID, email, or anything else contained within the identity provider’s database. The claims allow Citrix Workspace to generate a list of resources and services the user is authorized to access. For example, if the primary identity for the user is a Google ID, Citrix Workspace uses the Google ID to control authorization to other resources and services not linked to Google ID, like Workday, SAP, Windows, Slack, and others. Said another way, with Citrix Workspace, users can use a single Google ID to log in to every authorized resource, including Windows.

Within Citrix Workspace, the identity broker µ-service continues to expand to include additional options for the primary identity provider. The identity broker µ-service includes identity providers that can be available from an on-premises location or the identity broker µ-service can utilize cloud-based identity providers. The following diagram provides an overview of the Citrix Workspace identity platform and all current identity provider options, which are later discussed in more detail.

[![Workspace Identity Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_identity-brokering.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_identity-brokering.png)

Each of the identity providers is unique; but in the end, each identity provider tells Citrix Workspace a few things about the user:

1.  The user name
1.  The user successfully authenticated
1.  Claims about the user

To better understand the details of each identity provider, review the following sections on primary identity providers

*  Active Directory
*  Active Directory with TOTP
*  Azure Active Directory
*  Citrix Gateway
*  Okta

## Active Directory

When configured, users are able to authenticate to Citrix Workspace using Active Directory credentials.

[![Active Directory IdP](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-idp.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-idp.png)

To integrate Citrix Workspace with an on-premises Active Directory domain, Workspace must be able to communicate with a domain controller. By deploying a highly available set of cloud connectors within the on-premises environment, the cloud connectors establish an outbound control channel with the organization’s Citrix Cloud subscription. The outbound control channel allows Citrix Workspace to securely tunnel communication, over port 443, with on-premises components without requiring inbound firewall port modifications.

The cloud connector includes an AD Provider service that allows Citrix Workspace to read user and group information from the Active Directory domain. When a user authenticates with Citrix Workspace, the credentials are sent to the on-premises domain controller via the cloud connector’s AD Provider service.

[![Active Directory Ports](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-ports.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-ports.png)

## Active Directory with TOTP

For many organizations, providing access to application and desktop services with a user name and password does not provide adequate security. Incorporating a [Time-based One-Time Password](/en-us/tech-zone/learn/tech-insights/authentication-totp.html) (TOTP) provides multifactor authentication by introducing a “something you have”, which is the TOTP token, with the “something you know”, which is the password.

TOTP generates a random 6 digit code that changes every 30 seconds. This code is based on a secret key that is shared between the user’s mobile app and the backend infrastructure. The secret key is the “something you have” factor for multifactor authentication. To generate the random code, an industry standard, secure-hash algorithm gets applied to the secret key and the current time. To authenticate, the code in the mobile app is compared against the code from the backend infrastructure.

To register with the TOTP service, each user creates and installs a pre-shared secret key within the authenticator app on a mobile device.

[![Active Directory with TOTP Registration](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-totp-registration.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-totp-registration.png)

Once the user successfully registers with the TOTP micro-service, the user must use the token, along with their Active Directory credentials, to successfully authenticate to Citrix Workspace.

[![Active Directory with TOTP Authentication](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-totp-authentication.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_active-directory-totp-authentication.png)

Because TOTP is incorporated as a capability within Citrix Workspace, the complexity of setting up and maintaining a TOTP-type solution within an on-premises environment is removed. With this capability within Citrix Workspace, admins enable the service and users register devices.

There are a few items to consider when enabling TOTP-based multifactor authentication:

*  Authenticator Apps: TOTP uses an industry standard algorithm to generate the time-based token. Users can use any number of mobile apps to generate the tokens, including: Citrix SSO, Microsoft Authenticator, Google Authenticator and others.
*  Token Count: Users are allowed one token (key) per user account.
*  Device Count: Although users are limited to a single token (key), users can install the token across multiple devices. However, the install must happen during the registration phase as users are unable to reveal the QR code or secret key after registration completes.
*  Device Replacement: Whenever a user replaces their mobile device, they must register the device with the TOTP service. When the user goes through the TOTP registration process again, the old secret key is deleted. Any device using the old secret key fails to generate the correct token, resulting in a failed Workspace authentication.
*  Token Reset: Administrators are able to manually reset a user’s token. Once reset, users are unable to complete authentication without reregistering with the TOTP service.
*  Deployment: Like all identity and access management configuration changes, enabling TOTP on a Workspace subscription impacts all users. When enabled, any new authentication attempts fail until the user successfully registers with the TOTP service.

The [TOTP Tech Insight video](/en-us/tech-zone/learn/tech-insights/authentication-totp.html) provides additional details on the user and admin experience.

## Azure Active Directory

Citrix Workspace allows users to authenticate with an Azure Active Directory account. The authentication can be as simple as a user name and password or utilize any multifactor authentication policies available within Azure Active Directory. The integration between Citrix Workspace and Azure Active Directory results in Azure Active Directory handling the authentication process while returning an identity token for the user.

[![Azure Active Directory IdP](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_azure-active-directory-idp.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_azure-active-directory-idp.png)

To integrate Citrix Workspace and Azure Active Directory, Citrix Workspace automatically creates an enterprise application within Azure and sets the correct permissions. These permissions include the following (read-only capabilities):

*  Sign in and read user profile
*  Read all users’ basic profiles
*  Read all users’ full profiles
*  Read directory data
*  Read all groups

Azure Active Directory authenticates the user. Once the user is successfully authenticated, Azure provides Citrix Workspace with an Azure identity token including claims about the user to uniquely identify them within the correct directory.

Citrix Workspace utilizes the Azure Active Directory claims to authorize the user to resources and services within Citrix Workspace.

For Citrix Workspace to complete the authentication process and to generate a list of authorized resources, each Azure Active Directory user account must have the following parameters defined, which can be added manually or synchronized from an on-premises Windows Active Directory domain:

*  Email address
*  User Principal Name
*  Security Identifier (SID)

The [Azure Active Directory Tech Insight video](https://www.youtube.com/watch?v=H1Z9OOWSEGA) provides additional detail on the admin configuration and user experience when using a FIDO2 YubiKey.

## Citrix Gateway

Users are able to authenticate to Citrix Workspace using an on-premises Citrix Gateway. Citrix Gateway authentication accommodates simple authentication policies that use a single source for user authentication, like Active Directory, as well as more complex, cascaded authentication policies that rely upon multiple authentication providers and policies.

The integration between Citrix Workspace and Citrix Gateway results in Citrix Gateway handling the initial authentication process. Once Citrix Gateway validates the user’s authentication, Workspace validates the Active Directory credentials before generating a list of authorized services and resources.

[![Citrix Gateway IdP](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-idp.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-idp.png)

To integrate Citrix Workspace and Citrix Gateway, an OAuth IdP policy must get created within the on-premises Citrix Gateway, which is an authentication protocol based on the OAuth 2.0 specification. Citrix Workspace connects to the organization’s unique Citrix Gateway URL to access the OpenID Connect application by referencing the application’s client ID, which is protected with a shared secret key.

The OpenID Connect application, configured on Citrix Gateway, uses the advanced authentication policies bound to the authentication virtual server to authenticate the user. Once the user is successfully authenticated to Citrix Gateway, Citrix Gateway returns the user’s Active Directory credentials to Citrix Workspace.

The [Citrix Gateway Tech insight video](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) provides additional details on the admin configuration and the user experience.

With the use of [nFactor](https://support.citrix.com/article/CTX264698), Citrix Gateway allows organizations to create a more dynamic authentication flow, taking into account characteristics like user group membership, device ownership and user location.

In one example, using the most basic configuration, organizations can integrate a Citrix Gateway with Citrix Workspace to provide authentication to Active Directory.

[![Citrix Gateway - Active Directory](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-active-directory.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-active-directory.png)

This type of configuration requires an OAuth IdP policy configured with Citrix Workspace and an LDAP policy configured for an on-premises Active Directory domain. However, this basic authentication policy can be accomplished without utilizing a Citrix Gateway.

In many organizations, users must authenticate against a RADIUS deployment, like DUO, before authenticating to Active Directory, helping to protect Active Directory credentials.

[![Citrix Gateway - DUO](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-duo.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-duo.png)

This configuration requires the authentication policy to first validate a RADIUS authentication. If successful, the authentication flow continues to the next authentication factor, which is LDAP authentication.

With Citrix Gateway, organizations can implement contextual authentication, where the user’s authentication flow depends on the current user context. For example, an organization can implement different authentication policies for corporate-owned devices vs user-owned.

[![Citrix Gateway - Certificates](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-certificates.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-certificates.png)

In this configuration, Citrix Workspace sends the authentication request to Citrix Gateway. Citrix Gateway requests a client-based certificate, which is only available on corporate-owned devices. If the certificate is available and valid, the user simply provides an Active Directory password. However, if the certificate is invalid or does not exist, which would be the case for a user-owned device, Citrix Gateway challenges the user to provide a TOTP code followed by Active Directory credentials.

Another example of contextual authentication provides different authentication policies based on Active Directory group membership.

[![Citrix Gateway - User Groups](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-user-groups.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_citrix-gateway-user-groups.png)

Users interacting with financial data, personal data or intellectual property data should encounter stricter authentication policies as shown with Group2 in the diagram.

When using an on-premises Citrix Gateway as the identity provider, users are able to utilize push-based authentication with Citrix Workspace, as detailed in the [Push Authentication Tech Insight video.](/en-us/tech-zone/learn/tech-insights/authentication-push.html)

Regardless of the type of authentication policy configured, once the user successfully validates their identity, Citrix Gateway must respond to the initial Citrix Workspace request with the user’s Active Directory credentials. For Citrix Workspace to complete the authentication process and to generate a list of authorized resources, each Active Directory user account must have the following parameters defined:

*  Email address
*  Display name
*  Common name
*  sAMAccountName
*  User Principal Name
*  Object Identifier (OID)
*  Security Identifier (SID)

## Okta

When configured, users are able to authenticate to Citrix Workspace using Okta credentials. The authentication can be as simple as a user name and password or utilize any multifactor authentication policies available within Okta. The integration between Citrix Workspace and Okta results in Okta handling the authentication process while returning an identity and access token for the user.

[![Okta IdP](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-idp.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-idp.png)

To integrate Citrix Workspace and Okta, an OpenID Connect application must get created within Okta. Citrix Workspace connects to the organization’s unique Okta URL to access the OpenID Connect application by referencing the application’s client ID, which is protected with a shared secret key.

The OpenID Connect application authenticates the user. Once the user is successfully authenticated to Okta, Okta provides Citrix Workspace with two security tokens:

*  Access Token: A token proving the user has authorization to access the Okta resource (OpenID Connect application), eliminating the need to continuously reauthenticate.
*  Identity Token: A token proving the user’s identity. The identity token contains claims (information) about the authenticated user. The token is encoded to prove that it has not been tampered.

These tokens allow Citrix Workspace to access the OpenID Connect application and to generate a list of authorized resources based on the user’s Okta identity.

The type of resource feeds within Citrix Workspace dictates the requirements for the user’s Okta identity.

*  Native Okta identity: A native Okta identity is a stand-alone identity within the Okta identity provider. SaaS, web, and microapp application resource feeds within Citrix Workspace are able to directly use the default set of claims within a native Okta identity token.
*  Active Directory linked Okta identity: Microsoft Windows-based resources (VDI) require an Active Directory-based identity. If Citrix Workspace must authorize access to Citrix Virtual Apps and Desktops resources, the Okta identity used to authenticate to Workspace must contain claims (Active Directory SID, UPN, and GUID). These claims enable Citrix Workspace to authorize Active Directory-based resources to an Okta identity. The process for linking the Active Directory parameters with an Okta ID is greatly simplified with the use of the Okta Active Directory synchronization tool.

[![Okta - Parameters](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-parameters.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-parameters.png)

The setup and configuration of Okta as an identity provider is detailed in the [Okta Tech Insight video.](/en-us/tech-zone/learn/tech-insights/okta.html)
