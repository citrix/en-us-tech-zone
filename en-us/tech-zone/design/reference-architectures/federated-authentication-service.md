---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Roger LaMarca
description: Learn about Federated Authentication Service, authentication delegation and how to use seamless web authentication methods to log in to Windows environments for both Citrix Cloud and on-premises deployments.
tz_title: Federated Authentication Service
tz_products: security;
---
# Reference Architecture: Federated Authentication Service

## Overview

The IT industry has already started moving beyond legacy single-factor authentication to increase security through better credential methods for enabling remote access to internal resources. The organizations are adopting modern authentication approaches, mostly SAML (Security Assertion Markup Language) based, to enable secure access to the internal services.

Modern authentication is a framework of identity management that offers more secure user authentication and authorization. Managing user identities with modern authentication gives administrators many different tools that offer more secure systems of identity management. These authentication methods include services such as ADFS, Azure Active Directory, Okta, Google, Ping-Federate, and others. These methods offers a broader range of multi-factor options (text, call, pin) than the traditional password and security token.

In addition to eradicating the password weakness when using legacy single-factor authentication, SAML authentication enables administrators to manage a single credential set per user for all the apps which they need to access. When a user leaves the organization, IT administrators must revoke just one credential set. Credentials can be revoked without logging into each separate application.

## What is SAML

SAML is an XML-based industry-standard framework for exchanging authentication and authorization data between an identity provider and a service provider.

[![Federated-Authentication-Service-Image-1](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_001.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_001.png)

To recognize the SAML providers:

*  A Service Provider (SP) is an entity providing the service, typically in the form of an application
*  An Identity Provider (IdP) is an entity providing the user identities, including the ability to authenticate and authorize a user.

**Authentication** is the process of verifying the user's identity and credentials (password, two-factor authentication, and multi-factor).

**Authorization** is the process that tells the service provider what access to grant for the authenticated user.

## SAML Assertion

A SAML assertion is a cryptographically signed XML document issued by the Identity Provider sends to the Service Provider that contains the user authorization. There are three different types of SAML Assertions:

*  **Authentication** - Authentication assertions prove the identification of the user and provide the time the user logged in and what method of authentication they used (Kerberos, multi-factor, and more).

*  **Attribute** - The attribution assertion passes the SAML user attributes (specific pieces of data that provide information about the user like UPN).

*  **Authorization decision** - An authorization decision assertion says if the user is authorized to use the service or if the identity provider denied their request due to a password failure or lack of rights to the service.

SAML completely changes the authentication method by which a user signs in to access a service. Once an application or service configured to authenticate via SAML, the authentication exchange between the Service Provider and the configured Identity Provider occurs. The authentication process verify the user's identity and permissions, and then grant or deny that user's access to the services. Each identity provider and service provider need to agree upon a similar and exact configuration for SAML authentication to function correctly.

It is essential to highlight that SAML does not support sending the user's password between the identity provider and the service provider. SAML works by passing information about users, login assertions, and attributes between the identity provider and service providers. A user logs in once with the identity provider. The identity provider then passes the SAML assertion to the service provider when the user attempts to access those internal application services.

## Federated Authentication Service

The modern authentication framework carries a technical challenge to the Citrix environment. Citrix Workspace that has various Identity Providers to choose from, and at the same time, windows VDAs do not natively support SAML. The Virtual Delivery Agents(VDAs) accepts username/password, Kerberos, and certificates as authentication methods for logon.

With SAML authentication, Citrix Gateway and StoreFront do not have access to the user's password. It has only SAML assertion thus cannot perform single sign-on to the VDA during the session launch. With the SAML token, it breaks the Single Sign-On(SSO) to the VDA and prompts the users again for their credentials.

Citrix introduced the **Federated Authentication Service(FAS)** to achieve the Single Sign-On during the session launch when using SAML authentication by issuing virtual smart card user certificates to log on to the VDA. Citrix FAS is integrated with the Microsoft Active Directory and Certificate Services to issue smart card class certificates automatically on behalf of Active Directory users. Citrix FAS uses similar APIs that allow administrators to provision physical smart cards to issue the virtual smart card class user certificates.

[![Federated-Authentication-Service-Image-2](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_002.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_002.png)

Citrix FAS is to integrate with Workspace/StoreFront and the VDA to swap the SAML assertion out for a user certificate effectively. That issued certificate inserted as part of the session launch process, thus achieving Single Sign-On to the VDA and avoiding extra authentication prompts presented to the user. Citrix FAS deployment is supported for both Windows and Linux VDA's workloads.

## FAS Installation and considerations

### Citrix Cloud Connectors

For Citrix Cloud deployments, Cloud Connector enables the communication between resource location (where the FAS server resides) and Citrix Cloud. It is recommended to have two or more Cloud Connectors for each resource location. Ensure that the Cloud connectors can communicate with Active Directory Domain controllers and Virtual Delivery Agents at the respective resource location.

### FAS Server

Citrix FAS is supported to install on all the latest Windows Server versions. However, Citrix FAS deployment is supported for both Windows and Linux VDA's workloads. It is recommended to install the FAS services on a dedicated server that does not contain any other Citrix components. Install two or more FAS servers for each data center or resource location. Refer to [Citrix FAS installation and configuration](/en-us/federated-authentication-service/install-configure.html) document.

For scalability and high availability, refer to the document [Citrix Federated Authentication Service Scalability](/en-us/tech-zone/design/downloads/citrix-federated-authentication-service-scalability.pdf) and Citrix KB Article [CTX225721](https://support.citrix.com/article/CTX225721). When we are migrating from On-Premises to Citrix Cloud, the existing FAS servers deployed at the On-Premises environment can be utilized and configured to communicate with Citrix Cloud through Cloud Connectors.

The FAS servers should be installed within the secured internal network segment as it needs to access to Active Directory domain controllers, Certificate Services and registration authority certificate, and private key. Refer to the [Advanced configuration](/en-us/federated-authentication-service/config-manage.html) documentation to review the certificate, network, and other security considerations.

### Certificate Services

If it is not already deployed, you must design and deploy the Microsoft Certification Authority (CA) services in Enterprise mode as per your organization's security norms. To avoid interoperability issues with other software, FAS provides three Citrix FAS certificate templates for its own use. One of the Certificate Templates is for Smart Card logon to Citrix VDA. The other two Certificate Templates are to authorize FAS as a certificate registration authority. These templates must be deployed and registered with Active Directory with the help of an admin account that has permissions to administer your Enterprise forest.

### Active Directory

It is recommended to have Server 2012 functional level for the Active Directory. Domain Controllers must be installed with Domain Controller Authentication certificates and templates ([CTX218941](https://support.citrix.com/article/CTX218941)). The certificates on the Domain Controllers must support smart card authentication.

Each Active Directory deployment is different from another deployment, so extra steps may be required to get the FAS solution working in your environment. Refer to the [Citrix Blog](https://www.citrix.com/blogs/2019/11/20/your-guide-to-citrix-fas-multi-forest-selective-authentication/) for multi-forest selective authentication to choose the appropriate architecture for the deployment. It is advised to carefully test your solution in a lab environment before implementing it in a production environment.

Upload all three FAS Certificate templates to the Active Directory and configure a CA server to issue certificates using the new templates. One of the Certificate templates is for Smart Card logon to Citrix VDA. The other two Certificate templates are to authorize FAS as a certificate registration authority.

Install the Citrix FAS group policy ADMX templates into the **Policy Definitions** folder on the domain controller. Create a group policy object (GPO) and configure the GPO with the DNS addresses of the FAS servers. This GPO must apply to FAS servers, StoreFront servers, and every VDA with the respective domain. Ensure that the FAS Group Policy configuration has been applied correctly to the StoreFront and VDAs before creating the Machine Catalog and Delivery Groups.

The order of DNS addresses of your FAS servers in the GPO list must be consistent for all the VDAs, StoreFront servers (if present), and FAS servers. The GPO list is used by the VDA to locate the FAS server chosen for a virtual app or desktop launch.

### In-session certificate support

By default, VDAs do not allow access to certificates after logon. If necessary, use the Group Policy template to configure the system for in-session certificates. The in-session certificates option in the GPO controls whether a certificate can be used after login to the VDA. Only select this option if users need to have access to the certificate after authenticating.

If you choose the in-session option, it places the certificate in the user's personal certificate store after logon for application use. For example, TLS authentication to web servers within the VDA session, the certificate is used by the browser. If this option is not selected, the certificate is only used for logon or reconnection, and users do not have access to the certificate after authenticating.

### Citrix Delivery Controller

The Citrix Delivery Controllers must be a minimum of version 7.15, and the VDAs must be a minimum of version 7.15. It is essential that enabling the trust between the Delivery Controller and the StoreFront servers by running the `Set-BrokerSite -TrustRequestsSentToTheXmlServicePort $true` PowerShell cmdlet on the Delivery Controllers.

### Citrix StoreFront

Citrix strongly recommends installing the latest version of the StoreFront server for on-premises deployments. The StoreFront server must be a minimum of version 3.12. Ensure that the StoreFront servers requesting tickets and the Virtual Delivery Agents (VDAs) redeeming tickets have the identical configuration of FAS DNS addresses.

## Conceptual Architecture of Citrix FAS

The Federated Authentication Service (FAS) is a Citrix component that integrates with Microsoft Active Directory and Certificate Authority (CA), allowing users to seamlessly authenticate within a Citrix environment. It brings more comfortable alternative sign-in methods; hence users no longer have to provide credentials during the VDA session launch, thus achieving Single Sign-On.

For the On-Premises environment, starting from StoreFront 3.6, it is possible to use SAML authentication along with several external identity providers, and that integrates with Citrix FAS enabling users to authenticate from Citrix Gateway or either directly through StoreFront.

Citrix achieved the public GA release of Federated Authentication Service with Citrix Cloud in June 2020. Citrix FAS is now fully supported with Citrix Workspace to achieve Single Sign-On to VDAs when using a federated identity provider such as Azure AD and Okta Identity Providers.

### Citrix FAS for On-Premises Deployment

The conceptual architecture for the Citrix FAS deployment with the On-premises environment is the following. Let's review the design framework of each layer on this deployment to understand how it delivers a complete solution for your organization.

[![Federated-Authentication-Service-Image-3](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_003.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_003.png)

**User Layer:** Users access the Citrix environment through Citrix Gateway using Citrix Workspace app or browser. Citrix Workspace app is available for Windows, Mac, Android, and iOS. External users can also utilize the HTML5 version of the Workspace app through web browsers, where they cannot install the Workspace app on the devices.

**Access Layer:** This layer explains the deployment of Citrix Gateway and StoreFront. Citrix Gateway is deployed in the DMZ network to enable access to remote users, and the StoreFront servers are deployed within the corporate network for internal users. Citrix administrator has configured the SAML authentication policies on Citrix Gateway.

To enable Federated Authentication Service integration on a StoreFront Store, run the PowerShell cmdlets as an Administrator account on a new store created, and this step is required if users are accessing through StoreFront and there is no gateway involved. Refer to [Citrix Docs](/en-us/federated-authentication-service/install-configure.html#enable-the-fas-plug-in-on-storefront-stores) for more information on PowerShell cmdlets. Configuring the **VDA Logon provider** setting tells StoreFront to request a certificate from FAS and the VDA to retrieve it during session launch for authentication to the VDA. Once configured, this configuration applies to all session launches against that Store.

When using Citrix Gateway, configure StoreFront with the details of the Citrix Gateway and Callback URL, because logon evidence is transmitted via the Callback. Configuring Logon type as "Smart card" helps native clients perform SAML authentication. The setting "Fully delegate credential validation to Gateway" enables the StoreFront servers to delegate user authentication to the Gateway, which allows pass-through SAML authentication from the Citrix Gateway. IT admin has created the FAS Group Policy Object that containing the trusted FAS servers list and applied to the OU where the FAS Servers, VDAs, and StoreFront Servers reside in the AD domain.

When using SAML authentication, the actual authentication takes place at the Identity Provider. SAML Identity Provider can be anything such as ADFS, Azure AD, Okta, Google, or Ping Identity. Let's consider Active Directory Federation Services (ADFS) as an Identity Provider for this conceptual architecture to move on.

ADFS provides simplified, secured identity federation and single sign-on (SSO) capabilities for end-users who want to access applications within the secured enterprise and federation partner organizations. The ADFS tightly integrated with the organization's Active Directory domain and certificate services.

From now, it is denoted that the SAML Service Provider is the Citrix Gateway or StoreFront. The SAML Identity Provider is the Microsoft ADFS that exists on the domain and configured to access it from both internal and external networks for authentication.

Let's discuss on resource enumeration and session launch processes along with the FAS authentication workflow at the end of all layers.

**Control Layer:** Delivery controllers, SQL Database, Studio, and Licensing are the core components deployed and managed in the Control Layer. The administrator has configured hosting connections to communicate with hypervisors to provision and manage the virtual machines. Machine Catalog and Delivery groups are created and enabled access to the required user groups using Citrix Studio. Citrix Director helps the administrators to monitor the complete Citrix environment.

For Citrix FAS deployment, On the Delivery Controller, set the TrustRequestsSentToTheXmlServicePort to "true" which trust the XML requests sent by StoreFront Servers. It enables support for "Pass-through" authentication, and connections routed through Citrix Gateway. StoreFront Server communicates with the Delivery Controller using XML and enumerates the resources for the authenticated user. The user is presented with the StoreFront page listing the Apps and Desktops, which they are entitled to.

**Resource Layer:** The Resource layer is where all the user workloads (VDAs) reside in the Citrix environment. With the help of Citrix Studio and using the master image templates, the admin has deployed virtual apps servers using Windows Server OS. Using Citrix Provisioning (PVS), the admin has created Servers VDAs for task workers. Using Machine Create Services (MCS), the admin has deployed virtual desktops using Windows 10 for Power workers, and virtual Linux desktops using Red Hat Enterprise distribution for Linux users on the hypervisors.

Virtual Delivery Agents installed with these VMs are registered with Delivery Controllers; the admin created Machine Catalogs and Delivery Groups with the Desktops and Apps to enable access for the users using an AD security group. Citrix HDX Policies are created and assigned using Citrix Studio for the delivery groups to optimize and secure the HDX connections.

For Citrix FAS deployment, the Network Administrator has configured the [firewall rules](/en-us/federated-authentication-service/config-manage/security.html#firewall-and-network-security) for VDAs to communicate with Citrix FAS Servers to obtain the user certificates during the session launch. Citrix admin validated the Group Policy containing the trusted FAS Servers are linked and enabled with the Active Directory OUs where the VDA's and StoreFront Servers are residing in AD.

**Platform Layer:** This layer discusses the hardware platform required to host Control layer components and user workloads for On-Premises deployment. The Citrix admin has installed the hypervisor software on the server hardware and deployed the required virtual machines for Control infrastructure and user workloads. The network admin has enabled firewall rules for all the Citrix components to communicate with each other in the environment. The storage admin helped to configure and assign adequate storage to the Citrix environment.

**Operations Layer**: For this conceptual deployment, Let us focus on the FAS Server deployment under the Operations Layer.

For Citrix FAS Server deployment, the Citrix administrator has deployed two new windows Virtual Machines on the hypervisor and installed the FAS components. The FAS administration console is installed as part of the FAS installation. The first time the administration console is used, it guides you through a process that deploys certificate templates, sets up the certificate authority, and authorizes FAS to use the certificate authority.

Going next to the Certificate Authority, FAS uses DCOM calls that are specific to Windows Certificate Authorities. Third-party or public Certificate Authorities cannot be used. Multiple CA details can be specified within the FAS console for high availability. A central Certificate Authority can support multiple domains via cross-forest enrollment. Along the same lines, StoreFront, FAS, and the VDAs all mutually authenticate via Kerberos and, therefore, must either be in the same domain or domains that have a two-way trust between them.

Also, it is necessary to configure the user rule, which authorizes the issuance of certificates for VDA logon and in-session use, as directed by StoreFront. Each rule specifies the StoreFront servers that are trusted to request certificates, the set of users for which they can be requested, and the set of VDA machines permitted to use them. Refer to [Citrix Docs](/en-us/federated-authentication-service/install-configure.html#using-the-federated-authentication-service-administration-console), which explain the process more in detail.

FAS has a registration authority certificate that allows it to issue certificates autonomously on behalf of your domain users. As such, it is vital to develop and implement a security policy to protect FAS servers, and to constrain their permissions.

During the Session launch, the generation of user certificates is the most expensive part of the process. The FAS must generate a new certificate for a user dynamically. It can extend the logon process slightly and add to the CPU load on the FAS server. However, FAS servers can cache certificates that allow a user to log in almost as fast as using explicit password authentication. The login time for users significantly improves when user certificates are pre-generated within the FAS server. Refer to [Citrix Documentation](/en-us/federated-authentication-service/config-manage/ca-configuration.html#pre-generate-user-certificates) for detailed steps to be followed.

#### Session launch process with Citrix FAS

Let's discuss the authentication flow when using FAS with Citrix Gateway and StoreFront to VDAs:

[![Federated-Authentication-Service-Image-4](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_004.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_004.png)

When a user logs on to Citrix Gateway (Service Provider), it responds by generating a SAML login request and redirects to the ADFS (Identity Provider) login Page. Users enter the credentials on the ADFS Single Sign-On page. ADFS authenticates the user with the Active Directory and parses the SAML request as a SAML response. The encoded SAML response with the signed token handed over back to the Service Provider (Citrix Gateway).

Citrix Gateway validates the SAML response using the IdP certificate. It extracts the SAML assertion to look up the user's identity (User Principal Name) and the authorization to grant them access. Then it passes to the StoreFront for validation and resource enumeration. StoreFront servers again validate and verify the assertion with a trusted gateway, which uses the callback URL configuration. The FAS logon evidence feature provides logon evidence passed to FAS by Citrix Gateway and StoreFront. FAS can validate the evidence to ensure that token was issued by a trusted Identity Provider (IdP). For more detailed information on Logon evidence, refer to [Citrix documentation](/en-us/advanced-concepts/implementation-guides/citrix-federated-authentication-service-logon-evidence-overview.html).

Now, the StoreFront's Logon Data Provider service contacts the Federated Authentication Service and asks to generate a certificate for the authenticated user. The FAS connects to Active Directory to verify the user and then speaks to Active Directory Certificate Services(ADCS) and submits a certificate request for the user.

The Certificate Authority issues a valid certificate for the authenticated user. The FAS holds the user certificate and its attributes. This certificate is not shared with StoreFront. However, StoreFront is made aware that the user is valid, and FAS has enrolled a certificate for this authenticating user.

Now, the StoreFront Server communicates with the Delivery Controller using XML and enumerates the resources for the authenticated user. The user is presented with the StoreFront page listing all the Apps and Desktops, which they are entitled to.

When a user launches a virtual app or desktop in their workspace app, the request is sent to StoreFront to obtain the ICA file. StoreFront contacts the delivery controller, and asking for the VDA details for this session. The delivery controller validates the request and selects the VDA, which can take this user session and share the VDA and Secure Ticket Authority(STA) details with StoreFront to generate the ICA file. In addition to that, the StoreFront server selects a FAS server from the GPO list and contacts the selected FAS server to obtain a ticket that grants access to a user certificate, which is now stored on the FAS server. StoreFront appends this FAS token into the ICA file and sent back to the workspace app.

[![Federated-Authentication-Service-Image-5](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_005.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_005.png)

Workspace app establishes the launch session with Citrix Gateway by providing the STA ticket to validate and grant the communication with VDA. STA ticket is validated with Delivery Controllers, and then it passes to VDA for session launch. The FAS ticket is presented with VDA during this time to validate with the FAS Servers. The VDA Credential Plugin contacts the FAS Server selected from the GPO list. The FAS server validates the token and issues the valid user certificate. Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched and presented to the user.

### Citrix FAS for Citrix Cloud Deployment

The conceptual architecture for the FAS deployment with the Citrix Cloud environment is shown below. Let's review the design framework of each layer and the FAS workflow of this deployment to understand how it delivers a complete solution for an organization.

[![Federated-Authentication-Service-Image-6](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_006.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_006.png)

**User Layer:** The users, also called subscribers in Citrix Cloud, can access the Citrix environment using the Cloud Workspace URL. Users are provided with the Workspace URL from the Citrix Cloud to connect from the browser or workspace app installed on their endpoints.

**Access Layer:** Citrix Workspace or Gateway Service is the front end or entry point for users to access the Citrix environment. The administrator can change the Workspace URL and authentication options using the cloud portal. Installation of the Cloud Connectors enables extending the Customer's Active Directory domain to Citrix Cloud. Authentication configuration in the Workspace allows the admin to select the authentication source for users to sign in and access the Citrix resources.

Let's consider Microsoft Azure Active Directory (AAD) as an authentication source for this conceptual architecture to move on. Citrix Cloud includes an Azure AD app that allows customers to connect their Citrix Cloud Subscription with Azure AD. For more information on how to connect Azure Active Directory with Citrix Cloud, refer to the [Citrix documentation](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html). Upon enabling Azure AD authentication, providing access to users and groups should be managed using Libraries in the Citrix Cloud.

The Workspace Authentication configuration page enabled with the new option "Configure Authentication with the Federated Authentication Service." The FAS is disabled by default. The administrator enables the FAS authentication by using the "Enable FAS button."

[![Federated-Authentication-Service-Image-7](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_007.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_007.png)

When users start accessing the environment using the Workspace URL, users get redirected to the corresponding Identity providers based on the configuration. In this case, the user gets redirected to the Azure provided sign-in page. The user enters valid Azure AD credentials, and then the browser is redirected back to Citrix Workspace and presented with the resources page where it shows the Apps and Desktops, which are assigned to the user.

The session launch processes, along with the FAS authentication workflow, are discussed at the end of all layers.

**Control Layer:** For the Citrix Cloud environment, the Delivery controllers, SQL database, Studio, Director, and Licensing are the core components in the control layer which are deployed by Citrix on the Cloud during the activation of the Virtual Apps and Desktops Service. The administrator has configured the Resource Locations and hosting connections to communicate with the on-premises hypervisors. Machine Catalogs are created using the Cloud Studio Portal, and the Citrix Director helps to monitor the environment.

When the administrator enables the FAS authentication on Citrix Workspace, it enables extra FAS µ-services on Citrix Cloud to communicate with the on-premises FAS Servers. The FAS Servers deployed at resource location connecting the Cloud FAS µ-service over the outbound SSL connection.

**Resource Layer:** This layer is referring to a resource location where all the user workloads reside in this deployment. As a starting point for integrating the on-premises user workloads with Citrix Cloud, the administrator installed Citrix Cloud Connectors, which allows communication between on-premises components and Citrix Cloud services.

The VDAs installed at the resource locations are registered with Cloud Delivery Controllers. The admin creates Machine Catalogs and Delivery Groups. Citrix HDX Policies are created and assigned using Citrix Studio for the delivery groups to optimize and secure the HDX connections.

**Platform Layer:** This layer discusses the hardware platform required to host the user workloads and other components required for resource location. The Citrix admin has installed the hypervisor software on the server hardware and deployed the required virtual machines for user workloads and other components like FAS Servers. The network admin has enabled firewall rules for all the Citrix components to communicate with each other in the environment. The storage admin helped to configure and assign adequate storage to the Citrix environment.

**Operations Layer**: The tools and components like File Servers, Citrix WEM Servers, RDS License Servers, Citrix App Layering Servers, and FAS Servers which are required to manage the user workloads, are covered under the Operations Layer.

The administrator has deployed a file server cluster and Workspace Environment Manager Service to configure the user profiles for the VDAs. The IT Admin has created dedicated file shares and NFS shares for the user profiles and folder redirection. The Citrix admin also applied resource management policies to optimize the CPU and memory utilization on VDA agents. The IT admin has configured the Remote Desktop Services (RDS) Client Access License Server to issue the RDS licenses for Virtual Apps workloads.

To synchronize the Active Directory users and groups with Azure AD, the administrator has installed the required number of Azure AD Connect servers and configured with their Azure Subscription. For Citrix FAS deployment, the Citrix administrator has deployed two new windows VMs and followed the installation process to install and configure the Citrix FAS services.

Using the FAS administration console with the elevated permissions, the administrator has configured FAS Servers through a process that deploys certificate templates, sets up the certificate authority, and authorizes FAS to use the certificate authority. For detailed steps, refer to [Citrix documentation](/en-us/federated-authentication-service/install-configure.html#using-the-federated-authentication-service-administration-console).

[![Federated-Authentication-Service-Image-8](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_008.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_008.png)

Also, it is necessary to configure the user rule, which authorizes the issuance of certificates for VDA logon. Each rule specifies the StoreFront servers that are trusted to request certificates, the set of users for which they can be requested, and the set of VDA machines permitted to use them. However, when a rule is used with Citrix Cloud, the StoreFront access permissions are ignored. When we are using the existing FAS Servers, the FAS installer detects the existing setup and marks green next to these options.

The same rule can be used with Citrix Cloud and the on-premises StoreFront deployment. StoreFront access permissions are still applied when an on-premises StoreFront uses the rule. Refer to [Citrix Docs](/en-us/federated-authentication-service/install-configure.html#using-the-federated-authentication-service-administration-console), which explain the process more in detail.

Install the Citrix FAS group policy ADMX templates into the **Policy Definitions** folder on the domain controller. Create and configure the GPO with the DNS addresses of the FAS servers. This GPO must apply to FAS servers, StoreFront servers, and every VDA with the respective domain. Ensure that the FAS Group Policy configuration has been applied correctly to the StoreFront and VDAs before creating the Machine Catalog and Delivery Groups.

As a final step of configuring the FAS Servers on Citrix Cloud and connecting the On-Premises FAS Servers with Citrix Cloud, the administrator has chosen the option "Connect to Citrix Cloud." Once signed in with Citrix Cloud, the administrators select the customer account and the resource location. Now, Citrix Cloud registers the FAS server and displays it on the Resource Locations page.

[![Federated-Authentication-Service-Image-9](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_009.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_009.png)

#### Session launch process when using Citrix FAS with Citrix Cloud

When a user starts accessing the environment using the Workspace URL, the user gets redirected to the Azure-based Sign-in page. The user enters valid Azure credentials on the Azure Single Sign-On page. Azure authenticates the user and redirects the user back to the Citrix Workspace page. Citrix Workspace enumerated the resources and presented it to the resources page to the user.

When a user launches a virtual application or desktop in their Workspace, the request is sent to obtain the ICA file. Controllers select the available VDA from the resource location. Citrix Cloud selects a FAS server in the same resource location as the VDA to obtain a ticket that grants access to the VDA using a user certificate provided by the Certificate Authority, which is now stored on the FAS Server. Citrix Workspace appends this FAS token into the ICA file and sent back to the user system.

[![Federated-Authentication-Service-Image-10](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_010.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_010.png)

Workspace app establishes the connection using the ICA file to the VDA, and to authenticate the subscriber, the VDA connects to FAS and presents the ticket. The VDA Credential Plugin contacts the FAS Server and validates the token. Now, the FAS Server provides a valid user certificate to the VDA. Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched for the user.

### Use-Case #1

A Citrix Service Provider (CSP) designing a hybrid Citrix Virtual Apps and Desktops environment that would allow multiple customers to access. Considering future growth, CSP also needs to consider onboarding more new customers. The design specification suggests providing different Identity provider for each Customer and avoids creating domain trusts.

Citrix Service Provider onboards customers to their Citrix Virtual Apps and Desktops environment frequently, and the solution required Citrix Gateway to selectively provide a different SAML identity provider based on the UPN suffix. For example, Customer-A is configured to use Azure AD, and the customer-B is configured to use Active Directory Federation Services (ADFS), and so on. Configuring different identity providers for each Customer, the environment needed to provide single sign-on to the Citrix VDAs using Citrix FAS.

[![Federated-Authentication-Service-Image-11](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_011.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_011.png)

Refer to the [Citrix Blog](https://www.citrix.com/blogs/2020/04/23/multi-domain-citrix-gateway-nfactor-authentication-and-citrix-fas/), which contains detailed steps to configure the environment. Let's discuss the workflow and session launch process for each Customer. The significant configuration happens at the Citrix Gateway with the utilization of Citrix ADC AAA, nFactor, and Citrix Gateway session policies to accommodate multiple customer user requests.

1.  When a user logs in to Citrix Gateway (Service Provider), the user is identified based on the UPN suffix, and it redirects to the respective Identity Provider login page. Users enter the credentials, and now the Identity provider authenticates the user and responds with the SAML token as a response. The encoded SAML response with the signed token handed over back to the Service Provider Citrix Gateway, where the Root CA certificates from Identity providers were installed to validate the tokens.

2.  Citrix Gateway validates the SAML response using the IdP certificate. It extracts the SAML assertion to look up the user's identity (User Principal Name) and the authorization to grant them access. Then it passes to the StoreFront for validation and resource enumeration.

3.  Now, the StoreFront's Logon Data Provider service contacts the Federated Authentication Service and asks to generate a certificate for the authenticated user. The FAS connects to the Active Directory to verify the user and their shadow accounts. Then it speaks to Active Directory Certificate Services (AD CS) and submits a certificate request for the user. The Certificate Authority issues a valid certificate for the authenticated user.

4.  When a user launches a virtual app or desktop in their workspace app, the request is sent to StoreFront to obtain the ICA file. StoreFront validates the request and contacts the delivery controller asking for VDA details for this session. The delivery controller validates the request and shares the VDA and Secure Ticket Authority(STA) details with StoreFront to generate the ICA file. In addition to that, the StoreFront server selects a FAS server from the GPO list and contacts the selected FAS server to obtain a ticket that grants access to a user certificate, which is now stored on the FAS server. StoreFront appends this FAS token into the ICA file and sent back to the workspace app.

5.  The workspace app initiates a connection to start the session with Citrix Gateway by providing the STA ticket to validate and grant the communication. Upon validation, the connection is passed to the VDA for the session launch. The FAS ticket is presented with VDA during the time of authentication to validate against the FAS Servers. The VDA Credential Plugin contacts the FAS Server selected from the GPO list. The FAS server validates the token and issues the valid user certificate. Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched.

### Use-Case #2

Company-A recently acquired Company-B. Hence Company-A wanted to grant access to Company-B employees on their existing Citrix Virtual Apps and Desktops environment. To simplify the authentication between both company domains, administrators established a federated connection between the domains by implementing External SAML Identity Providers solution like Azure Active Directory. SAML authentication allows the users to seamlessly log in to the AD environment of the other company to access the resources. In both the companies, users can use their company-specific credentials, wherein a shadow account is used and mapped at the Company-A to access the resources.

[![Federated-Authentication-Service-Image-12](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_012.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_012.png)

To log in to the Citrix VDA, every user must have an Active Directory account in a domain trusted by the VDA. For Federated Users, we need to create shadow accounts for (Company-B) each federated user in the Company-A domain. These shadow accounts need a UPN that matches the SAML attribute (usually email address) provided by the SAML IdP. If the email address provided by the SAML IdP does not match the UPN suffix for the company domain, we need to add the UPN suffix that matches the email suffix provided by the SAML IdP on the Active Directory Domains and Trusts snap-in.

1.  The Company-B user accesses the Gateway URL, and the user gets redirected to the SAML Identity Provider. In this case, it is an Azure Active Directory. Both the Company-A and Company-B Domains synced to a single Azure AD tenant.

2.  The user enters the Company-B credentials to authenticate against the AAD and redirected back to the Gateway with the SAML Token.

3.  Gateway parses the SAML token and then uses this SAML Token to verify the identity of the user using the shadow account. Then it passes to the StoreFront for resource enumeration.

4.  Now, the StoreFront's Logon Data Provider service contacts the Federated Authentication Service and asks to generate a certificate for the authenticated shadow account user. The FAS connects to the Active Directory to verify the user(shadow account). Then it speaks to Active Directory Certificate Services (AD CS) and submits a certificate request for the user. The Certificate Authority issues a valid certificate for the authenticated user.

5.  StoreFront created the resources page, sent back to the user.

6.  When a user launches a virtual app or desktop in their workspace app, the request is sent to StoreFront to obtain the ICA file. StoreFront validates the request and contacts the delivery controller asking for VDA details for this session. The delivery controller validates the request and shares the VDA and Secure Ticket Authority(STA) details with StoreFront to generate the ICA file. In addition to that, the StoreFront server selects a FAS server from the GPO list and contacts the selected FAS server to obtain a ticket that grants access to a user certificate, which is now stored on the FAS server. StoreFront appends this FAS token into the ICA file and sent back to the workspace app.

7.  The workspace app initiates a connection to start the session with Citrix Gateway by providing the STA ticket to validate and grant the communication. Upon validation, the connection is passed to the VDA for the session launch. The FAS ticket is presented with VDA during the time of authentication to validate against the FAS Servers. The VDA Credential Plugin contacts the FAS Server selected from the GPO list. The FAS server validates the token and issues the valid user certificate. Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched.

### Use-Case #3

An existing enterprise Citrix customer wants to migrate their existing legacy Citrix environment as part of a tech refresh and upgrade plan. The migration plan dictates that the control infrastructure of a Citrix environment has to be moved to Citrix Cloud. To deploy user workloads, they planned to utilize the existing hardware available at the regional data center and Azure Cloud, which helps them better user management and resource allocation. Also, the Customer chosen to utilize the existing Citrix Gateway deployed at each regional data center for optimal HDX connection. Finally, for the authentication, the Customer opted to go with Azure Active Directory as they planned to migrate the Domain Controllers to Azure Active Directory Domain Services.

According to the migration plan, the Domain Administrator has installed and configured the AD Connect to sync the users and groups with Azure Active Directory and then to Azure AD Domain Services. The Customer purchased Citrix Cloud subscription. Hence the control infrastructure components are deployed and managed by the Citrix. The Citrix Admin has configured Azure AD as an authentication method for the users and created resource locations by installing the Cloud Connectors on each region data center.

To utilize the existing Citrix Gateways installed at each region, the administrator has configured the "Gateway" option on the Citrix Cloud, pointing to an on-premises gateway, which helps in launching the user's HDX connection via the on-premises Gateway. The administrators deployed the required user workloads on the existing hardware at the respective data center, and the VDAs are now registered with Citrix Cloud controllers. Domain administrator has created region-wise AD security groups for user provisioning to the Citrix resources. Citrix administrator enabled access to resources on the library page on the Citrix Cloud portal using region-specific AD security groups, which allows the users to access their published desktops and applications from their same region.

To achieve single sign-on to the VDA when using Azure AD for authentication with Citrix Workspace, the Customer decided to go with the Citrix FAS solution. Citrix admin has Installed the required number of FAS Servers and configured them to communicate with Citrix Cloud FAS services on each resource location at Citrix Cloud. Group policy containing the list of FAS Servers to the respective region is linked at the OU level; hence the VDA fetch the list of FAS servers from the same data center.

[![Federated-Authentication-Service-Image-13](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_013.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_013.png)

Now, let us review the session launch workflow for the users:

1.  When a user starts accessing the environment using the Workspace URL, the user gets redirected to the Azure-based sign-in page. The user enters valid Azure credentials on the Azure Single Sign-On page. Azure authenticates the user and redirects the user back to the Citrix Workspace.

2.  Citrix Workspace connects with cloud controllers and enumerates the resources assigned for the user and presented it to the user.

3.  The user launches a virtual application or desktop in their Workspace, and the request is sent to obtain the ICA file. Cloud Controllers select the available VDA from the resource location where the resources are mapped for this user. Citrix Cloud selects a FAS server in the same resource location to obtain a ticket that grants access to the VDA. The FAS Server is requesting a user certificate from the Certificate Authority, which is now stored on the FAS Server. Citrix Workspace appends this FAS token and Cloud Connector as STA into the ICA file and sent back to the user system.

4.  Workspace app parses the ICA file and establishes the connection to the on-premises Gateway for the HDX connection to VDA.

5.  Citrix Gateway validates the connection using the STA ticket with Cloud Connector and passes the connection to VDA.

6.  To authenticate the user, the VDA connects to the FAS server from the GPO list. The VDA Credential Plugin contacts the FAS Server and validates the token. Now, the FAS Server provides a valid user certificate to the VDA.

7.  Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched for the user.

### Use-Case #4

A new Citrix customer wants to deploy the Citrix Virtual Apps environment to enable access to internal resources that are not exposed over the internet like the Intranet portal and Exchange mailboxes. To ensure the environment is highly available, the Customer has chosen to deploy the environment in two locations in active/active design. The critical servers are available as redundant at each location to avoid failures at the component level. The Customer wants to use Microsoft 365 for multifactor authentication and Conditional Access with the Citrix environment for authentication. The main goal for the Customer is to provide access to a user who can access the virtual apps from any data center.

As per the design, the Customer has deployed a dedicated XenDesktop (Virtual Apps and Desktops) site at each location consisting of 3 x StoreFront Servers, 3 x Delivery Controllers, Always-On SQL Servers, 3 x PVS Servers, License, and Director Servers. Each location has a pair of Citrix ADC for GSLB, Gateway, and Load balancer configuration. Since the Customer wants to use Azure AD authentication, a pair of Citrix FAS servers needs to be deployed at each location.

The environment was configured as per the design and the customer requirement.

1.  Global Server Load Balancing(GSLB) has been configured between the sites to load balance and route the users to the available site.

2.  StoreFront servers are load balanced using the Citrix ADC Gateway.

3.  StoreFront servers are configured to communicate with both locations' delivery controllers; hence users can access resources in either Data Center 1 or 2.

4.  It is recommended to configure the GPO for the StoreFront servers to point only to FAS servers in the local data center to optimize the certificate request process.

In this customer environment, where StoreFront enumerates resources from both the data center, it is required to configure the VDAs to be aware of all FAS servers from both the data centers. The VDA can retrieve the certificate from the FAS server that received the request from StoreFront (which may be in a different data center). StoreFront servers would only communicate with the two FAS servers in the same data center. GPO Policies listing the FAS servers would use blanks to ensure that the StoreFront server fetches the list of FAS Servers in the right index, as shown in the following image.

[![Federated-Authentication-Service-Image-14](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_014.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_014.png)

When configuring this, these "Index" alignment of the GPO must be considered. The lists of FAS servers on StoreFront and the VDAs must align, like the list of Delivery Controllers, a VDA does not accept a launch request from a Delivery Controller that it is unaware.

With Citrix FAS, the VDA can retrieve a certificate from the listed FAS server. The additional consideration is the fact that the order of the FAS servers in the StoreFront and VDA registries must match because the FAS servers are assigned an Index number based on the order they are listed in the registry. Sometimes, it is required to leave blank entries in the registry/policy applied to StoreFront server groups to ensure that the index matches between StoreFront and VDA.

[![Federated-Authentication-Service-Image-15](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_015.png)](/en-us/tech-zone/design/media/reference-architectures_federated-authentication-service_015.png)

Now, let us review the session launch workflow for the users:

1.  When a user starts accessing the environment, the user request lands to either Datacenter-1 or Datacenter-2 based on the GSLB ADNS.

2.  The Gateway redirects the user to the Azure-based sign-in page. The user enters valid Azure credentials on the Azure Single Sign-On page. Azure authenticates the user and redirects the user back to the Gateway.

3.  Citrix Gateway connects with StoreFront Servers and enumerates the resources assigned for the user and presented it to the user.

4.  The user launches a virtual application, and the request is sent to obtain the ICA file. StoreFront connects with the delivery controller, and it selects the available VDA from the delivery group where the user mapped to access the resources. StoreFront selects a FAS server in the same location through the GPO to obtain a ticket that grants access to the VDA. The FAS Server is requesting a user certificate from the Certificate Authority, which is now stored on the FAS Server. StoreFront appends this FAS token and Delivery Controller as STA into the ICA file and sent back to the user system.

5.  The workspace app parses the ICA file and establishes the connection to the On-Premises Gateway for the HDX connection to VDA.

6.  Citrix Gateway validates the connection using the STA ticket with the delivery controller and passes the connection to VDA.

7.  To authenticate the user, the VDA connects to the FAS server from the GPO list. The VDA Credential Plugin contacts the FAS Server and validates the token. Now, the FAS Server provides a valid user certificate to the VDA. Upon successful validation of the user certificate, the single sign-on is achieved, and the VDA session is launched for the user.

### Summary

Citrix Federated Authentication Service helps in all the deployments where the customers want to eliminate the legacy password credential method and to move towards the modern authentication methods like SAML and others. As a final note, Citrix FAS is a vital service. Therefore, it is something approved by a company's security team before being deployed. Review and implement the required security controls for the FAS Services.

### Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s32442d04daf4eeb8).

### References

[Citrix FAS installation and configuration](/en-us/federated-authentication-service/install-configure.html)

[Citrix FAS scalability and HA document](/en-us/advanced-concepts/downloads/citrix-federated-authentication-service-scalability.pdf)

[Federated Authentication Service High Availability and Scalability - CTX225721](https://support.citrix.com/article/CTX225721)

[Advanced FAS configuration](/en-us/federated-authentication-service/config-manage.html)

[Domain Controller Authentication certificates and templates](https://support.citrix.com/article/CTX218941)

[Multi-forest selective authentication](https://www.citrix.com/blogs/2019/11/20/your-guide-to-citrix-fas-multi-forest-selective-authentication/)

[PowerShell Cmdlets for Citrix FAS](/en-us/federated-authentication-service/install-configure.html#enable-the-fas-plug-in-on-storefront-stores)

[Firewall rules for Citrix FAS](/en-us/federated-authentication-service/config-manage/security.html#firewall-and-network-security)

[User Rule configuration](/en-us/federated-authentication-service/install-configure.html#using-the-federated-authentication-service-administration-console)

[Pre-Generate User Certificates](/en-us/federated-authentication-service/config-manage/ca-configuration.html#pre-generate-user-certificates)

[Logon evidence](/en-us/advanced-concepts/implementation-guides/citrix-federated-authentication-service-logon-evidence-overview.html)

[Connect Azure Active Directory with Citrix Cloud](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-azure-ad.html)

[Deploys FAS certificate templates](/en-us/federated-authentication-service/install-configure.html#using-the-federated-authentication-service-administration-console)
