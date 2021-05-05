---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Learn how Citrix Workspace provides single sign-on capabilities to SaaS apps, web apps, mobile apps, Windows virtual apps and Windows virtual desktops. In addition, learn how Workspace single sign-on can support IdP chaining configurations.
tz_title: Workspace Single Sign-On
tz_products: citrix-workspace;
---
# Tech Brief: Workspace Single Sign-On

A user’s primary Workspace identity authorizes them to access SaaS, mobile, web, virtual apps, and virtual desktops. Many authorized resources require another authentication, often with an identity different from the user’s primary workspace identity. Citrix Workspace provides users with a seamless experience by providing single sign-on to secondary resources.

## Primary Identity

Understanding the differences between a user’s primary and secondary identities provides a foundation for understanding Workspace single sign-on.

[![Workspace Identity](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_primary-identity.png)

To start, Citrix Workspace allows each organization to choose a primary identity from a growing list of options, which currently includes:

*  Windows Active Directory
*  Azure Active Directory
*  Okta
*  Citrix Gateway
*  Google

Within Citrix Workspace, the user’s primary identity serves two purposes:

1.  Authenticates the user to Citrix Workspace
1.  Authorizes the user to access a set of resources within Citrix Workspace

Once the user successfully authenticates to Citrix Workspace with the primary identity, they have authorization to all secondary resources. It is critical for organizations to set up strong authentication policies for the user’s primary identity.

Many identity providers include strong authentication policy options, helping to secure the user’s primary authentication to Citrix Workspace. In cases where the identity provider only includes a single user name and password, like Active Directory, Citrix Workspace includes extra capabilities to improve primary authentication security, like [Time-based One Time Password](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp).

To gain a deeper understanding of the primary identity for Citrix Workspace, refer to the [Workspace Identity Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html).

## Secondary Identities

Many of the applications, desktops, and resources a user accesses within Citrix Workspace are secured with another set of user credentials, referred to as secondary identities. Many of the secondary identities are different than the user’s primary identity.

[![Workspace Secondary Identities](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_secondary-identities.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_secondary-identities.png)

The single sign-on µ-service translates the user’s primary workspace identity into a resource-specific identity using appropriate approaches like:

*  SAML
*  Kerberos
*  Forms
*  Virtual Smartcards

With Citrix Workspace, users authenticate once with their primary identity and all subsequent authentication challenges to secondary resources are automatically satisfied.

How Citrix Workspace provides single sign-on to different resources is based on the type of resource accessed. To better understand the different approaches, it is best to break it down into the following topics:

*  SaaS apps
*  Web apps
*  Mobile apps (section coming soon)
*  Virtual apps and desktops
*  IdP Chaining

## SSO: SaaS Apps

From a Citrix Workspace perspective, a SaaS application is a browser-based application hosted in the cloud by a third party. To access the application, users must authenticate with a set of credentials associated with the SaaS application, referred to as a secondary identity.

To achieve single sign-on to a SaaS application, Citrix Workspace federates identity between the primary identity and secondary identity. The SSO process utilizes industry standard SAML-based authentication.

SAML-based authentication typically focuses on three main entities:

*  Identity Provider: the entity in a SAML link providing proof the user’s identity is valid
*  Service Provider: the entity in a SAML link delivering a service (SaaS app) based on a secondary identity
*  Assertion: a package of data providing

SAML-based authentication works by associating two different user accounts (primary and secondary) with common attribute(s), typically a user principal name (UPN) or email address.

[![SAML Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-overview.png)

The user identity can be different between the primary identity from the identity provider and secondary identity from the service provider.

With single sign-on, the user does not need to know their secondary identity’s user
name or password. In addition, many SaaS applications have the ability to disable the password (and direct password access) from user accounts when authentication uses SAML. This forces user authentication to always use the primary identity from the identity provider and not the secondary identity from the service provider.

 Citrix Workspace introduces a fourth component to the SAML process

*  Identity Broker: the entity that links multiple identity providers to multiple service providers (Citrix Workspace)

[![Brokering Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-overview.png)

Within a SAML link, there needs to be an entity acting as the service provider (SP) and identity provider (IdP). The IdP does not have to contain the primary user account identities. In this example, the primary user identity is contained within the primary user directory (Dir).

When acting as an identity broker (IdB), Citrix Workspace takes claims about the user’s primary identity and translates them to secondary identities.

Adding an identity broker (IdB) into the SAML authentication flow still requires a common attribute linking the user’s primary identity (IdP) to the secondary identity (SP).

[![Brokering Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)

For SAML authentication to work, the identity broker associates the request with a SAML specific logon URL for each SaaS application. This URL receives the user assertion. When the service provider receives the assertion, it must validate the assertion against the entity that generated the assertion, which is the identity broker's SAML issuer URL.

[![SAML URLs](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-urls.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-urls.png)

Within Citrix Workspace, the overall process for single sign-on to SaaS applications is as follows:

[![SAML URLs](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saas-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saas-flow.png)

SSO to SaaS apps within Citrix Workspace helps solve a few user and admin experience challenges:

1.  Users do not have to remember a user name and password for each secondary identity
2.  Users do not have to create complex passwords for each secondary identity
3.  Users do not have to setup/configure MFA keys/tokens for each secondary identity
4.  Admins can disable access to all SaaS apps by disabling the user’s primary identity
5.  Admins can base the user’s primary identity on one of the growing list of supported identity providers

To keep access secure, organizations must

1.  Implement strong authentication policies for the primary Workspace identity
2.  Disable direct access to SaaS apps with the secondary identity

## SSO: Web Apps

A web application is a browser-based application hosted and managed by the organization.  The web application is hosted within the on-premises data center. To access a web application, users must establish a secure connection to the host and authenticate with a set of credentials associated with the web application, referred to as a secondary identity.  

[![Web App SSO Flow](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_webapp-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_webapp-flow.png)

Based on the conceptual architecture, the Gateway Connector establishes an outbound control channel connection to the organization's Citrix Cloud subscription. Once established authentication and access requests to the on-premises web application travel across the Gateway Connector's control channel, eliminating the need for a VPN connection.

Depending on the web application, the secondary identity could be the same identity as the primary identity used to authenticate to Citrix Workspace or a unique identity managed by a different identity provider.

To achieve single sign-on to a web application, Citrix Workspace federates identity between the primary identity (used to sign into Citrix Workspace) and the secondary identity (used to sign into the web app). The SSO process for web application utilizes multiple approaches to be able to support a larger number of web applications.  These approaches can be

*  Basic - used when the web application server presents users with a basic-401 challenge. Basic authentication utilizes the credentials used to authenticate to Citrix Workspace.
*  Kerberos - used when the web application server presents users with a negotiate-401 challenge. Kerberos utilizes the credentials used to authenticate to Citrix Workspace.
*  Forms - used when the web application server presents users with an HTML authentication form. Forms-based authentication requires the administrator to identify the appropriate fields on the authentication page for the user name and password.
*  SAML - used when the web application server is able to use SAML-based authentication, where the web app acts as the service provider and Citrix Workspace as the identity provider. The user's primary identity used to log into Citrix workspace must have a parameter (UPN or email) aligned with the credentials in the web application. To learn more about SAML-based single sign-on, review the [SSO to SaaS Apps](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html#sso-saas-apps) section.
*  No SSO - used when the web application server does not require user authentication or when the administrator wants the user to sign in manually.

Single sign-on to web applications within Citrix Workspace helps solve a few user and admin experience challenges:

*  Users do not have to remember a user name and password for each web application
*  Users do not have to create complex passwords for each web application
*  Users do not have to setup/configure MFA keys/tokens for each web application
*  Users do not have to start a VPN connection to access an internal web application
*  Admins can disable access to all web applications by disabling the user’s primary identity
*  Admins can base the user’s primary identity on one of the growing list of supported identity providers
*  Once deployed, admins do not have to update the Gateway Connectors. Citrix Cloud services automates the updates as needed.

To keep access secure, organizations must

*  Implement strong authentication policies for the primary Workspace identity
*  Disable VPN access to web applications

Deploy redundant Gateway Connectors to maintain availability when a connector is being updated. Only one connector gets updated at a time and the process does not continue until a success result is received.

## SSO: Mobile Apps (section coming soon)

## SSO: Virtual Apps and Desktops

Citrix Virtual Apps and Desktops allows users to remotely access Windows and Linux-based applications and desktops. To access a Windows-based virtual application or desktop requires users to authenticate with an Active Directory identity.

When a user’s primary Workspace identity is Active Directory, a virtual app and desktop session utilizes pass-through authentication to provide single sign-on to the secondary resource. However, if an organization wants to use a non-Active Directory-based identity provider for the user’s primary identity, single sign-on capabilities of Citrix Workspace must translate the primary identity to an Active Directory secondary identity.

To achieve single sign-on to a virtual app and desktop, Citrix Workspace utilizes the federated authentication service, which dynamically generates an Active Directory-based virtual smartcard for the user.

Before a virtual smartcard can be generated, Workspace must be able to link the user’s primary identity with the Active Directory-based secondary identity via a set of common attributes.

For example, when Okta is the primary identity for Citrix Workspace, the user’s Okta identity must include three extra parameters (cip_sid, cip_upn, and cip_oid). The parameters associate an Active Directory identity with an Okta identity.

[![SAML URLs](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-parameters.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-identity_okta-parameters.png)

Once the user successfully authenticates to the primary identity, the single sign-on capability within Citrix Workspace uses the parameters to request a virtual smartcard.

Within Citrix Workspace, the overall process for single sign-on to a Windows-based virtual application and desktop is as follows:

[![SSO to Citrix Virtual Apps and Desktop](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_cvad-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_cvad-flow.png)

This example assumes the Citrix Virtual Apps and Desktop is an on-premises deployment.

If organizations use the cloud-based Citrix Virtual Apps and Desktops Service, the architecture resembles the following:

[![SSO to Citrix Virtual Apps and Desktop Service](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_cvads-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_cvads-flow.png)

SSO to Windows-based virtual applications and desktops within Citrix Workspace helps solve a few user and admin experience challenges:

1.  Users do not receive an authentication prompt when accessing a virtual application or desktop
1.  Users do not have to create, update, and remember complex passwords for their Active Directory identity
1.  Admins can disable access to all virtual applications and desktops by disabling the user’s primary identity

To properly integrate the federated authentication service, consider the following:

*  Synchronization: The primary Workspace identity must maintain synchronization with the Active Directory-based secondary identity. Many primary identity providers include an Active Directory synchronization tool to help maintain synchronization between primary and secondary identities. Without proper synchronization, the federated authentication service is unable to associate an Active Directory identity to the virtual smartcard.
*  Smartcard Only Authentication: With a group policy object, administrators can force smartcard only authentication. This eliminates the possibility of a user trying to bypass the secured primary identity by using secondary identity’s user name/password credential. However, if a user must ever use the username/password to interactively log on to a service, the authentication fails with this policy setting enabled.
*  Virtual Smartcard Security: It is important to ensure that the federated authentication service infrastructure is managed and secured appropriately. The following article provides [security recommendations for the federated authentication service](/en-us/federated-authentication-service/config-manage/security.html).
*  Redundancy: In a production deployment, the overall architecture must incorporate fault tolerance as part of the design. This includes redundant federated authentication services servers, certificate authorities, and so on. Depending on the scale of the environment, organizations might need to dedicate subordinate certificate authorities instead of pointing to the root certificate authority.
*  Certificate Authority: For a production deployment, organizations must design the certificate authority to handle the scale. Also, organizations must properly design the associated certificate revocation list (CRL) infrastructure to overcome potential service disruptions.
*  Tertiary Authentication: Within a virtual desktop session, many internal websites require users to authenticate with an Active Directory identity. The virtual smartcard used to provide single sign-on to the virtual desktop can be used to provide single sign-on to the internal website. The federated authentication service allows (via a group policy object) in-session certificates where the virtual smartcard is placed within the user certificate store. This capability provides single sign-on to these tertiary resources.

## SSO: IdP Chaining

Many organizations currently rely on a 3rd party solution (Okta, Ping, Azure, and so on) to provide single sign-on to SaaS applications. Citrix Workspace can integrate the SSO-enabled SaaS applications into the user’s resource feed through a process called IdP chaining. IdP chaining essentially converts one SAML assertion to another SAML assertion. IdP chaining allows organizations to maintain their current SSO provider while fully integrating with Citrix Workspace, including the implementation of enhanced security policies.

When Citrix Workspace provides SSO to SaaS applications, it uses SAML authentication. SAML-based authentication works by associating two different user accounts (primary and secondary) with common attribute(s), typically a user principal name (UPN) or email address.

With SAML-based single sign-on, there are two partners:

1.  Service Provider (SP): the entity that delivers a service and contains a secondary identity
1.  Identity Provider (IdP): the entity that provides validation for the user’s primary identity. The identity provider in a SAML group does not have to be the final authority on the user’s identity.

[![Brokering Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)

The primary user directory is the final authority on the user’s identity. Citrix Workspace, acting as an identity broker (IdB), obtains claims about the user from the primary user directory (Dir) to create a SAML assertion. The assertion proves the user’s identity to the service provider (SP) and fulfills the single sign-on process.

When an organization already utilizes another SSO provider, IdP chaining adds an additional SAML authentication link into the authentication chain.

[![IdP Chaining Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-overview.png)

In this IdP chaining example, Citrix Workspace, acting as an identity broker authenticates the user to the primary user directory. Within the first SAML link, Citrix Workspace utilizes claims about the user to create a SAML assertion for an Okta-specific resource, which acts as a service provider. Within the second SAML link, Okta utilizes claims about the user to create a SAML assertion for a specific SaaS app, which is the service provider.

IdP chaining adds additional links between the user’s primary identity and the requested service. Within each SAML link, the common attribute between identity provider and service provider must be the same. As the authentication passes across different links in the chain, the common attribute can change.

[![IdP Chaining Common Attribute](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-common-attribute.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-common-attribute.png)

Within each SAML link, the identity provider associates the authentication request with a SAML specific logon URL for each SaaS application. This URL receives the user assertion, which includes the common attribute. When the service provider receives the assertion, it must validate the assertion against the entity that generated the assertion, which is the identity provider’s SAML issuer URL.

[![IdP Chaining URL Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-url-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-url-overview.png)

In an IdP chain, the process is identical except each SSO-enabled application within the SSO provider has an app-specific URL. The app-specific URL acts as a service provider. The app-specific URL is used as the SAML login URL for when the SSO provider takes on the service provider role.

[![IdP Chaining URL Details](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-url-detail.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-url-detail.png)

In this example, when a user selects an Okta-enabled application from within Citrix Workspace, Citrix Workspace presents the assertion to the SAML login URL for the requested Okta application. Okta validates the assertion with the SAML issuer URL from Citrix Workspace. Once successful, Okta presents an assertion to the SaaS application’s SAML login URL, which validates the assertion with the Okta SAML issuer URL.

The easiest way to think of IdP chaining is to focus on each link in the chain individually, which includes one identity provider and one service provider. In this example, the links in the chain are:

1.  Citrix-to-Okta
2.  Okta-to-Workday

This results in the following authentication flow:

[![IdP Chaining Flow](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_idp-chaining-flow.png)

Creating an IdP chain allows organizations to maintain their current SSO provider while still unifying all resources within Citrix Workspace.
