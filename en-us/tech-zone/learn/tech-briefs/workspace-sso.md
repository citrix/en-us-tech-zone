---
layout: doc
---
# Workspace - Single Sign-On

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

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

Many identity providers include strong authentication policy options, helping to secure the user’s primary authentication to Citrix Workspace. In cases where the identity provider only includes a single user name and password, like Active Directory, Citrix Workspace includes extra capabilities to improve primary authentication security, like [Time-based One Time Password](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory-with-totp).

To gain a deeper understanding of the primary identity for Citrix Workspace, refer to the Workspace Identity Tech Brief.

## Secondary Identities

Many of the applications, desktops, and resources a user accesses within Citrix Workspace are secured with an another set of user credentials, referred to as secondary identities. Many of the secondary identities are different than the user’s primary identity.

[![Workspace Secondary Identities](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_secondary-identities.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_secondary-identities.png)

The single sign-on µ-service translates the user’s primary workspace identity into a resource-specific identity using appropriate approaches like:

*  SAML
*  Kerberos
*  Forms
*  Virtual Smartcards

With Citrix Workspace, users authenticate once with their primary identity and all subsequent authentication challenges to secondary resources are automatically satisfied.

How Citrix Workspace provides single sign-on to different resources is based on the type of resource accessed. To better understand the different approaches, it is best to break it down into the following topics:

*  SaaS apps
*  Web apps (section coming soon)
*  Mobile apps (section coming soon)
*  Virtual apps and desktops
*  IdP Chaining (section coming soon)

## SSO: SaaS Apps

From a Citrix Workspace perspective, a SaaS application is a browser-based application hosted in the cloud by a third party. To access the application, users must authenticate with a set of credentials associated with the SaaS application, referred to as a secondary identity.

To achieve single sign-on to a SaaS application, Citrix Workspace utilizes industry standard SAML-based authentication.

SAML-based authentication typically focuses on three main entities:

*  Identity Provider: the entity that proves the user’s primary identity (Active Directory, Azure Active Directory, Okta, and so on)
*  Service Provider: the entity that delivers a service (SaaS app) and contains a secondary identity
*  Assertion: a package of data that indicates the user was authenticated (XML)

    SAML-based authentication works by associating two different user accounts (primary and secondary) with common attribute(s), typically a user principal name (UPN) or email address.

    [![SAML Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-overview.png)

    The user identity can be different between the primary identity from the identity provider and secondary identity from the service provider.

    With single sign-on, the user does not need to know their secondary identity’s user
name or password. In addition, many SaaS applications have the ability to disable the password (and direct password access) from user accounts when authentication uses SAML. This forces user authentication to always use the primary identity from the identity provider and not the secondary identity from the service provider.

    Citrix Workspace adds a fourth component to the SAML process

*  Identity Broker: the entity that links multiple identity providers to multiple service providers (Citrix Workspace)

[![Brokering Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-overview.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-overview.png)

Citrix Workspace takes claims about the user’s primary identity and translates them to secondary identities.

Adding an identity broker (IdB) into the SAML authentication flow still requires a common attribute linking the user’s primary identity (IdP) to the secondary identity (SP).

[![Brokering Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-broker-detail.png)

For SAML authentication to work, the identity broker associates the request with a SAML specific logon URL for each SaaS application. This URL receives the user assertion. When the service provider receives the assertion, it must validate the assertion against the entity that generated the assertion, which is the identity broker's SAML issuer URL.

[![SAML URLs](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-urls.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso_saml-urls.png)

Within Citrix Workspace, the overall process for single sign-on to SaaS applications is as follows:

[![SAML URLs](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso-saas-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-sso-saas-flow.png)

SSO to SaaS apps within Citrix Workspace helps solve a few user and admin experience challenges:

1.  Users do not have to remember a user name and password for each secondary identity
2.  Users do not have to create complex passwords for each secondary identity
3.  Users do not have to setup/configure MFA keys/tokens for each secondary identity
4.  Admins can disable access to all SaaS apps by disabling the user’s primary identity
5.  Admins can base the user’s primary identity on one of the growing list of supported identity providers

To keep access secure, organizations must

1.  Implement strong authentication policies for the primary Workspace identity
2.  Disable direct access to SaaS apps with the secondary identity

## SSO: Web Apps (section coming soon)

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
*  Virtual Smartcard Security: It is important to ensure that the federated authentication service infrastructure is managed and secured appropriately. The following article provides [security recommendations for the federated authentication service](https://docs.citrix.com/en-us/federated-authentication-service/config-manage/security.html).
*  Redundancy: In a production deployment, the overall architecture must incorporate fault tolerance as part of the design. This includes redundant federated authentication services servers, certificate authorities, and so on. Depending on the scale of the environment, organizations might need to dedicate subordinate certificate authorities instead of pointing to the root certificate authority.
*  Certificate Authority: For a production deployment, organizations must design the certificate authority to handle the scale. Also, organizations must properly design the associated certificate revocation list (CRL) infrastructure to overcome potential service disruptions.
*  Tertiary Authentication: Within a virtual desktop session, many internal websites require users to authenticate with an Active Directory identity. The virtual smartcard used to provide single sign-on to the virtual desktop can be used to provide single sign-on to the internal website. The federated authentication service allows (via a group policy object) in-session certificates where the virtual smartcard is placed within the user certificate store. This capability provides single sign-on to these tertiary resources.

## SSO: IdP Chaining (section coming soon)
