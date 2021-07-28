---
layout: doc
h3InToc: true
contributedBy: JP Alfaro
specialThanksTo: Darren Harding, Selma Wei, Manny Benitez, Tham Trejos, Amit Arora
description: The Citrix Workspace integration with nFactor and Multiple IDPs for CSPs provides guidance to design and implement authentication with multiple IDPs via Citrix ADC while leveraging the capabilities of Citrix Workspace.
tz_title: Citrix Workspace Integration with nFactor and Multiple IDPs for CSPs
tz_products: citrix-service-providers;
---
# Reference Architecture: Citrix Workspace Integration with nFactor and Multiple IDPs for CSPs

## Introduction

The purpose of this document is to guide Citrix Service Providers (CSPs) implementing the Citrix Virtual Apps and Desktops Service (CVADS) with Citrix Workspace and multiple Identity Providers (IDPs). Support for multiple IDPs with Citrix Workspace is achieved via the utilization of Citrix ADC nFactor Authentication.

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_001.png)

This document isn't intended to provide step-by-step guidance on how to deploy the Citrix Virtual Apps and Desktops service for CSPs. It assumes understanding of the [Virtual Apps and Desktops Reference Architecture for CSPs](/en-us/tech-zone/design/reference-architectures/csp-cvads.html), which provides in-depth design and deployment considerations for a CVADS environment for CSPs.

On the other hand, it assumes understanding of Citrix ADC, single sign-on (SSO), and the Citrix Federated Authentication Service. For further information on these technologies, visit docs.citrix.com.

This document starts by reviewing the most common elements you need to understand to comfortably deploy Citrix ADC nFactor authentication. Next, it reviews the authentication flow for the components that make up this solution.

Finally, it covers the steps required to deploy Citrix ADC nFactor authentication with multiple IDPs, and how to integrate it with Citrix Workspace.

## Overview

### Citrix Virtual Apps and Desktops Service

The Citrix Virtual Apps and Desktops Service provides secure access of centrally managed desktops and applications from any device or network. With the integration between the Federated Authentication Service (FAS) and Citrix Cloud, CSPs can deliver SSO to virtual apps and desktops workloads while supporting external SAML IDPs.

CVADS supports workloads hosted on multiple resource locations including traditional hypervisors and public cloud platforms like Microsoft Azure. The workloads hosted in these resource locations can be Windows or Linux based, supporting both multi-user and single user deployments.

Users can access their virtual resources via Windows, Mac, and Linux based clients, iOS and Android phones, and more. Citrix Gateway Service is usually in charge of handling external connections. It provides high availability with multiple points of presence around the world. Internal connections can use the new [Direct Workload Connection](/en-us/citrix-workspace/workspace-network-location.html) functionality.

CSPs can follow the [Virtual Apps and Desktops Reference Architecture for CSPs](/en-us/tech-zone/design/reference-architectures/csp-cvads.html) for guidance to architect a hosted DaaS solution powered by CVADS.

### Citrix ADC nFactor Authentication

Citrix ADC nFactor provides the actions and policies to deliver a scalable and flexible authentication experience to end customers. This use case is relevant to CSPs delivering desktops and applications to multiple customers, by allowing them to bring their own SAML IDP.

nFactor authentication uses a robust policy engine and allows CSPs to design complex authentication workflows. nFactor uses policy expressions as the mechanism to determine the authentication flow for users. This functionality is based on different details like user, or connection attributes.

For this architecture, OAUTH IDP policies are configured to allow Citrix ADC to handle the authentication for Citrix Workspace. Also, SAML (and potentially LDAP) policies are configured to connect to multiple IDPs.

### Citrix Workspace Experience

Workspace Experience is the cloud-based evolution of StoreFront. Through Workspace Experience, CSPs can deliver virtual apps and desktops, SSO to SaaS and on-prem web applications, microapp integrations and actions, and content collaboration. This advanced functionality allows CSPs to deliver their end customers with an integrated experience, from a single pane of glass, focused on employee experience, and increased productivity.

Currently, Citrix Workspace does not support the integration with multiple IDPs. Many CSPs, however, need to provide multiple IDP support while maintaining the advanced features provided by Citrix Workspace.

### OAUTH Authentication

Citrix ADC can be configured as an OAUTH Identity Provider (IDP) by using the Open ID Connect (OIDC) protocol. OAUTH is typically not referred to as an authentication protocol, but as an authorization framework instead. OIDC adds a user authentication portion to the typical OAUTH 2.0 flow. In this architecture, Citrix Workspace acts as the OAUTH Service Provider (SP) trusting Citrix ADC (OAUTH IDP).

For this configuration, Citrix Workspace requires for Active Directory shadow accounts to pass a set of "claims" for the authentication processs to be successful. Citrix Cloud requires these properties to establish the user context when subscribers sign in. If these properties aren't populated, subscribers can't sign into their workspace. The list of claims is reviewed in a later section of this document.

### SAML Authentication

In the context of this architecture, Citrix ADC becomes the Service Provider (SP), and each customer's authentication solution acts as the Identity Provider (IDP). The SP or the IDP can initiate the SAML authentication process. This architecture uses SP initiated SAML SSO.

The SAML communication flow does not imply direct communication between the SP and the IDP. The web browser handles all the communication and no firewall ports need to be opened between the SP and IDP. Only the web browser needs to be able to communicate with both the SP and IDP.

## Concepts and Terminology

Citrix ADC nFactor uses a set of entities that allow for the configuration of the different factors required by a specific deployment. The following concepts lay the foundation to understand the policy flows used by Citrix nFactor.

*  **Authentication server (action):** The authentication server (action) defines the specific configuration for a specific IDP, whether it's an on-prem Active Directory, Azure AD, Okta, ADFS, etc. It includes the required details for the Citrix ADC appliance to communicate with the IDP and authenticate the users.
*  **Authentication policy:** The authentication policies allow for users to be authenticated against the appliance. Policies use expressions under which they are applied. Expressions are used to let the ADC redirect the users to the appropriate IDP based on their UPN. An authentication policy must be linked to an authentication server (action).
    *  The most commonly used expression in these scenarios is **AAA.USER.NAME.SET_TEXT_MODE(IGNORECASE).AFTER_STR("@").EQ("domain.com")**. This expression evaluates the user's UPN suffix after the "@" sign and if it matches a policy, applies the configured SAML server (action) for authentication.
*  **Login schema:** The login schema is a logical representation of the logon form written in XML, in other words, they represent the user interface. It's an entity that defines what the user sees and specifies how to extract the data from the user. Different schemas (or no schema) can be used for the different authentication factors. Citrix ADC provides several out of the box schema templates for common use cases, which can be customized for other use cases.
*  **Policy label:** Policy labels specify the authentication policies for a particular factor. Each policy label corresponds to a single authentication factor. They are basically a collection of policies that can be linked together as a single entity. The result of policies in a policy label follows logical "OR" conditions. If the authentication specified by the first policy succeeds, other policies following it are skipped. Policy labels define their view through a login schema.
*  **"No-Auth" policy:** This is a special policy that always returns "success" as the authentication result. Their main purpose is to allow for flexibility when making logical decisions through the user authentication flow.
*  **Next factor:** It determines what is done after a given step if the authentication flow is successful. It can be an extra policy, or define that the authentication flow must stop.
*  **AAA vServer:** The authentication virtual server processes the associated authentication policies and provides access to the environment. For this architecture, the AAA vServer replaces the more common Gateway vServer and it's a fully addressable vServer. The Gateway vServer is only required if using Citrix ADC for HDX traffic, in which case the AAA vServer is configured as non-addressable. The Gateway vServer integration goes beyond the scope of this document.
*  **Authentication profile (optional):** The authentication profile allows for the AAA vServer, and thus all its policies, to be linked to a Gateway vServer. This profile is only required if handling HDX traffic through the Citrix ADC appliance.

## Architecture

CSPs are constantly growing and adding new customers to their CVADS based DaaS offering. This growth introduces the requirement to allow end customers to bring their own identity solutions and integrate them with Citrix Workspace, to use CVADS based workloads.

Citrix Workspace also provides advanced functionality that allows CSPs to integrate more services like SSO to SaaS and on-prem web applications, microapp integrations and actions, and content collaboration. It also provides integration with several IDPs like Azure AD, Okta, and SAML 2.0. However, multiple IDPs from a single Citrix Workspace are currently not supported.

Citrix Gateway Service handles the HDX proxy functionality when launching virtual apps and desktops resources from Citrix Workspace. This functionality might seem unnecessary since Citrix ADC is needed to provide multiple IDP support. However, offloading the HDX proxy significantly simplifies the network bandwidth and availability requirements on the CSP side.

This reference architecture focuses on the design decisions and considerations for integrating multiple IDPs, particularly SAML based, with Citrix Workspace and the CVAD service. This integration is achieved by configuring Citrix Workspace to delegate user authentication to Citrix ADC, which in turn forwards users to their respective IDPs.

### Considerations and Requirements

At the moment of this writing, the following details need to be considered before deciding to integrate multiple IDPs to a CSP managed CVADS deployment.

1.  A Citrix ADC Advanced or Premium license is required for AAA nFactor functionality.
2.  Citrix ADC 12.1 version 54.13 or later, or 13.0 version 41.20 or later are required.
3.  The CVADS multi-tenancy feature isn't compatible with this deployment due to limitations with FAS and reverse federation.
4.  Different SAML IDPs have different configuration steps. Those steps aren't covered in this document.
5.  Active Directory must be configured with the alternate UPN suffixes and shadow accounts for each specific customer.
6.  The following AD properties must be configured on the AD shadow accounts to be used as claims:
    *  Email address
    *  Display name
    *  Common name
    *  SAM account name
    *  UPN
    *  OID
    *  SID
7.  Active Directory Certificate Services must be configured and available before configuring FAS.
8.  While Citrix FAS is integrated with Citrix Workspace, it isn't a Cloud Service. FAS is deployed on the resource location.
9.  Initial ADC configuration steps, including IPs, certificates, and network details aren't covered in this document.
10.  Duplicate usernames across different UPN suffixes cannot be used. Even though the UPN suffix is different, the pre-Windows 2000 login name is the same for all suffixes.

### User Experience

The following diagram shows the authentication flow from a user experience perspective. To the end user, the experience is fairly similar to a traditional implementation without multiple IDPs. It's important, however, to educate end users on the need to use their UPN when signing in, as opposed to the more common SAM Account name.

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_002.png)

1.  End users from different customers log in to Citrix Workspace from any device / any network.
2.  Citrix Workspace automatically redirects the users to the Citrix ADC AAA vServer.
3.  Citrix ADC AAA vServer presents the user with a Username prompt. Users must enter their UPN.
4.  User authentication request is forwarded to the appropriate SAML IDP based on the user's UPN suffix.
5.  After the users authenticate via their IDP, they are redirected back to Citrix Workspace.
6.  When a user attempts to launch a virtual app or virtual desktop, the CVAD service handles the brokering process.
7.  Citrix Gateway Service establishes the HDX proxy to the virtual resources.
8.  Active Directory shadow accounts are used to request a smart card certificate to provide SSO to the user.
9.  The FAS rule is applied and the user sign-in request is satisfied via SSO.

### Authentication Flow

The following diagram shows the authentication flow for the different protocols in this architecture. Citrix Workspace acts as an OAUTH SP, Citrix ADC acts as both an OAUTH IDP and a SAML SP, and the customer IDP is the SAML IDP.

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_003.png)

1.  End user accesses Citrix Workspace (SP) via web browser or Citrix Workspace App.
2.  When reaching Citrix Workspace (OAUTH SP), the user is redirected to Citrix ADC AAA vServer (OAUTH IDP).
3.  User enters the UPN at the AAA vServer (SAML SP) login prompt and is redirected to the authentication service (SAML IDP).
4.  SAML IDP authenticates the user and generates a SAML assertion (XHTML form).
5.  The SAML assertion is sent back to the Citrix Workspace App.
6.  Citrix Workspace App redirects the SAML assertion to the Citrix ADC AAA vServer.
7.  Citrix ADC AAA vServer sends security context back to the user agent.
8.  Citrix Workspace App requests the resources from the Citrix ADC AAA vServer.
9.  Citrix ADC AAA vServer authenticates the user and claims are sent to Citrix Workspace.
10.  Resources are accessible by Citrix Workspace App.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|In the SAML flow, the web browser, or Workspace App is referred to as the "User Agent", which is part of the HTTP request header.|

### Policy Flow

The following diagram represents the nFactor policy flow for this architecture. Understanding the nFactor policy flow is vital to the success of the designed authentication architecture. In this diagram, an LDAP policy is used. While using an LDAP policy is optional, it's a common practice to provide access to administrators.

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_004.png)

1.  The user accesses Citrix Workspace via a web browser or the Citrix Workspace App. The authentication request is forwarded to the Citrix AAA vServer. There's an OAUTH IDP policy configured at Citrix ADC with the expression "TRUE", this means that this policy is applied to ALL requests.
2.  The authentication request hits a "NO_AUTHN" policy and is presented with the "Username Only" login schema. The NO_AUTHN policy acts as a place holder. NO_AUTHN policies always return "success" as the authentication result.
3.  When the user enters their username in UPN format, the UPN suffix determines which authentication policy is evaluated. In this architecture, domain1.com is forwarded to an LDAP server, domain2.com is redirected to an Azure AD tenant, and domain3.com is redirected to another Azure AD tenant. All of this functionality is based on the expressions used by each policy. Also notice that these authentication policies are bound to an "No Schema" login schema. This detail means that nothing is presented on the user's web browser.
4.  If the user enters a username under domain1.com, they are redirected to an LDAP policy (a traditional LDAP policy). This policy evaluates to "TRUE", which means that it affects all the users it evaluates. The login schema in this case is "Password Only", meaning users only enter their AD password in this step. Their username is captured in the previous factor.
5.  On the other hand, if the user enters a username under either domain2.com or domain3.com, they are redirected to their respective Azure AD tenant for login. In this case, Azure handles all the authentication, which is outside the realm of the Citrix ADC nFactor engine. When the user is authenticated, they are redirected back to the Citrix ADC AAA vServer.
6.  Once the authentication request is redirected back to Citrix Gateway, it flows through another authentication factor, which is tied to an LDAP policy. However, this policy does not perform authentication. The purpose of this policy is to extract the claims required to authenticate the user back to Citrix Workspace. All SAML policies are redirected to this single LDAP policy. Users aren't presented with a login schema and do not need to enter any information at this point, this process happens automatically.
7.  User claims are stored on the Citrix ADC and passed back to Citrix Workspace. These claims are required for Citrix Workspace to accept the authentication from Citrix ADC.
8.  The user is redirected back to Citrix Workspace, and they can now access their resources.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|The LDAP "no-authentication" policy is only used after a user is authenticated by another factor. It always evaluates to "Success" as the authentication result.|

### Scaling Considerations

The ability to scale this solution in an agile manner as new customers are onboarded is important to CSPs. This architecture allows for non-disruptive steps to be followed when onboarding new customers. The main steps to onboard a new customer to this solution are as follows.

1.  **Configure the end-customer IDP:** This might or might not fall under the CSP's responsibility. Most SAML IDPs provide extensive documentation to configure these types of solutions.
2.  **Add customer UPN suffix:** This is done via the Active Directory Domains and Trusts MMC console. UPN suffixes must be unique. Also, while the UPNs are different for each customer on the shared AD environment, all shadow accounts have the same NetBIOS suffix name.
3.  **Add shadow accounts:** Creating shadow accounts manually could become an extensive task and scripting is recommended to automate this process. End users don't need to know the password for these accounts.
4.  **Configure the SAML action and policy:** A new SAML action and policy must be configured for every new customer that is onboarded to this solution. The action contains all the SAML IDP details, and the policy contains the expression that is used to evaluate the policy.
5.  **Bind the SAML policy:** The new SAML authentication policy must be added to the SAML policy label that groups all the authentication policies for the different customers. Since all of these policies are mutually exclusive, adding new policies to the policy label does not cause any disruption with your current customers.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|Duplicate usernames across different UPN suffixes cannot be used. Even though the UPN suffix is different, the pre-Windows 2000 login name would be the same for duplicate users.|

## Implementation

This document covers the steps required to integrate Citrix Workspace with a Citrix ADC AAA vServer and multiple SAML IDPs. Cloud Connector configuration, Machine Catalog / Delivery Group creation, and FAS implementation fall beyond the scope of this document.

Successful configuration of the previously mentioned components can be achieved by following standard installation practices. No custom configuration steps are required to integrate them with this architecture. Visit the **Additional Configuration Resources** section of this document for configuration steps.

### Authentication Policies

#### LDAP Authentication Action (Optional)

1- Login to the Citrix ADC appliance and navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > Actions > LDAP** and click **Add**.

2- On the **Create Authentication LDAP Server** page enter the following information.

*  Name: LDAP authentication server entity name
*  Server Name / Server IP: Server name is recommended
*  Security Type: PLAINTEXT or SSL based on security requirements. (SSL Recommended)
*  Port: 389 or 636 based on security type.
*  Server Type: AD
*  Authentication: Checked
*  Base DN: AD based DN for user searches
*  Administrator Bind DN: AD service account UPN
*  Administrator Password: AD service account password
*  Confirm Administrator Password: AD service account password
*  Server Logon Name Attribute: userPrincipalName
*  Group Attribute: memberOf
*  Sub Attribute Name: cn
*  SSO Name Attribute: cn
*  Email: mail

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_005.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This step is optional, but highly recommended for support purposes. Create this action to allow environment administrators to log in against the environment with AD credentials.|

#### LDAP User Attributes Action

1- On the **LDAP Actions** page click **Add** to create a second LDAP action. Use the same information as the previous one, but this time, **UNCHECK** the **Authentication** box.

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_006.png)

2- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This step is NOT optional. This action is used to extract the user's claims from their shadow accounts in AD after they've been authenticated via their SAML IDP. These claims are necessary for the redirection back to Citrix Workspace to be successful.|

#### SAML Actions

1- Navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > Actions > SAML** and click **Add**.

2- On the **Create Authentication SAML Server** page enter the following information.

*  Name: SAML authentication server entity name
*  Redirect URL: Redirect URL provided by the IDP
*  Single Logout URL: Logout URL provided by the IDP
*  SAML Binding: POST / REDIRECT / ARTIFACT
*  Logout Binding: POST / REDIRECT
*  IDP Certificate Name: Click Add and import the certificate downloaded from your SAML IDP
*  Signing Certificate Name: Citrix ADC AAA vServer SSL certificate
*  Issue Name: Same as AAA vServer URL
*  Reject Unsigned Assertion: ON
*  Signature Algorithm: RSA-SHA256
*  Digest Method: SHA256

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_007.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|Repeat these steps to create a separate SAML action for each SAML IDP to be used.|
|The necessary fields to be completed in this page varies depending on the SAML IDP. Consult the specific SAML IDP documentation for additional information.|

### Login Schemas

#### Login Schema Profiles

1- Navigate to **Security > AAA – Application Traffic > Login Schema > Profiles** and click **Add**.

2- On the **Create Authentication Login Schema** page, enter the following information.

*  Name: Login schema profile entity name
*  Authentication Schema: noschema

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_008.png)

3- Click **Create**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|Repeat these steps to create the 2 remaining schema profiles with the **OnlyUsername.xml** and the **OnlyPassword.xml** files.|

#### Login Schema Policies

1- Navigate to **Security > AAA – Application Traffic > Login Schema > Policies** and click **Add**.

2- On the **Create Authentication Login Schema Policy** page, enter the following information.

*  Name: Login schema policy entity name
*  Profile: Previously created NoSchema profile
*  Rule: TRUE

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_009.png)

3- Click **Create**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|Repeat these steps to create the 2 remaining schema policies by linking them with the **"Username Only"** and **"Password Only"** schema profiles.|

### Authentication Policies

#### Baseline "No-Auth" Policy

1- Navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > Policy** and click **Add**.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy entity name
*  Authentication Type: NO_AUTHN
*  Expression: HTTP.REQ.URL.CONTAINS("/nf/auth/doAuthentication.do")

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_010.png)

3- Click **OK**.

#### LDAP "No-Auth" Policy (Optional)

1- On the **Authentication Policies** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy entity name
*  Authentication Type: NO_AUTHN
*  Expression: AAA.USER.LOGIN_NAME.SET_TEXT_MODE(IGNORECASE).AFTER_STR("@").EQ("domain1.com")

[![CSP-Image-011](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_011.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This step is optional, but highly recommended for support purposes. Create this policy to allow environment administrators to log in against the environment with AD credentials.|
|Replace "domain1.com" in the expression with the name of the internal AD domain UPN suffix.|
|The **NO_AUTHN** authentication type is used as a place holder to redirect users to the next authentication factor, which is their AD password, handled through another LDAP policy.|

#### LDAP Authentication Policy (Optional)

1- On the **Authentication Policies** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy entity name
*  Authentication Type: LDAP
*  Action: Previously created LDAP Authentication Action
*  Expression: TRUE

[![CSP-Image-012](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_012.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This step is optional, but highly recommended for support purposes. Create this policy to allow environment administrators to log in against the environment with AD credentials.|
|The **TRUE** expression in this policy means that this policy is evaluated against every user that is redirected to this authentication factor.|
|This policy is attached to the **Password Only** login schema previously created.|

#### LDAP User Attributes Policy

1- On the **Authentication Policies** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy entity name
*  Authentication Type: LDAP
*  Action: Previously created LDAP User Attributes Action
*  Expression: TRUE

[![CSP-Image-013](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_013.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This step is NOT optional. This policy is used to extract the user's claims from their shadow accounts in AD after they've been authenticated via their SAML IDP. These claims are necessary for the redirection back to Citrix Workspace to be successful.|
|The **TRUE** expression in this policy means that this policy is evaluated against every user that is redirected to this authentication factor.|
|This policy is attached to a **NO_SCHEMA** login schema.|

#### SAML Authentication Policies

1- On the **Authentication Policies** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy entity name
*  Authentication Type: SAML
*  Action: Previously created SAML Authentication Action
*  Expression: AAA.USER.LOGIN_NAME.SET_TEXT_MODE(IGNORECASE).AFTER_STR("@").EQ("domain2.com")

[![CSP-Image-014](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_014.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_014.png)

3- Click **OK**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|Replace "domain2.com" in the expression with the customer's domain name.|
|Create a SAML Authentication Policy for each SAML Authentication Action previously created and match them 1:1 with the appropriate domain name in the expression.|

### Authentication Policy Labels

#### LDAP Authentication Policy Label

1- Navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > Policy Label** and click **Add**.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy label entity name
*  Login Schema: "Password Only" login schema profile
*  Feature Type: AAATM_REQ
*  Click Continue

[![CSP-Image-015](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_015.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_015.png)

3- Bind the authentication policies with the following details.

*  **Policy 1**
    *  Select Policy: LDAP Authentication Policy
    *  Priority: 100
    *  Goto Expression: END
    *  Select Next Factor: N/A

[![CSP-Image-016](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_016.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_016.png)

4- Click **Done**.

#### LDAP User Attributes Policy Label

1- On the **Authentication Policies Labels** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy label entity name
*  Login Schema: "NO_SCHEMA" login schema profile
*  Feature Type: AAATM_REQ
*  Click Continue

[![CSP-Image-017](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_017.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_017.png)

3- Bind the authentication policies with the following details.

*  **Policy 1**
    *  Select Policy: LDAP User Attributes Policy
    *  Priority: 100
    *  Goto Expression: END
    *  Select Next Factor: N/A

[![CSP-Image-018](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_018.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_018.png)

4- Click **Done**.

#### Main Policy Label

1- On the **Authentication Policies Labels** page click **Add** to create another policy.

2- On the **Create Authentication Policy** page, enter the following information.

*  Name: Authentication policy label entity name
*  Login Schema: "NO_SCHEMA" login schema profile
*  Feature Type: AAATM_REQ
*  Click Continue

[![CSP-Image-019](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_019.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_019.png)

3- Bind the authentication policies with the following details.

*  **Policy 1**
    *  Select Policy: LDAP "No-Auth" Policy
    *  Priority: 100
    *  Goto Expression: NEXT
    *  Select Next Factor: LDAP Authentication Policy Label

*  **Policy 2**
    *  Select Policy: SAML Authentication Policy
    *  Priority: 110
    *  Goto Expression: NEXT
    *  Select Next Factor: LDAP User Attributes Policy Label

[![CSP-Image-020](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_020.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_020.png)

4- Click **Done**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|When onboarding new customers / IDPs, you need to add their respective SAML authentication policies to this policy label.|

### AAA vServer

1- Navigate to **Security > AAA – Application Traffic > Virtual Servers** and click **Add**.

2- On the **Authentication Virtual Server** page, enter the following information.

*  Name: Authentication virtual server entity name
*  IP Address Type: IP Address
*  IP Address: vServer assigned IP Address
*  Port: 443
*  Click OK.

[![CSP-Image-021](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_021.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_021.png)

3- On the **Certificate** pane, click **No Server Certificate** and bind an SSL certificate to the vServer. Then, click **Continue**.

4- On the **Advanced Authentication Policies** pane, click **No Authentication Policy** and enter the following information.

*  Select Policy: Baseline "No-Auth" Policy
*  Priority: 100
*  Goto Expression: NEXT
*  Select Next Factor: Main Policy Label

[![CSP-Image-022](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_022.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_022.png)

5- Click **Continue**.

6- On the **Advanced Settings** pane, click **Login Schemas**.

7- On the **Login Schemas** pane, click **No Login Schema** and enter the following information.

*  Select Policy: "Username Only" schema policy
*  Priority: 100
*  Goto Expression: END

[![CSP-Image-023](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_023.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_023.png)

8- Click **Bind**.

9- Back on the **AAA vServer** page, click **Done**.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|At this point, the AAA vServer URL is publicly reachable, and both LDAP and SAML authentication work.|

### Global Certificate Binding

1- Connect to the Citrix ADC via **SSH** and authenticate with the admin credentials.

2- Run the following command: **bind vpn global -certkeyName certname**.

3- Save the running configuration.

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|This command is used by ADC to sign the token that is sent to Citrix Workspace as part of the authentication process.|
|The -certkeyName flag refers to the same SSL certificate used on the AAA vServer.|

### Citrix Workspace Integration

#### Citrix Gateway IDP

1- On a web browser, go to [Citrix Cloud](https://citrix.cloud.com) and login with your Citrix credentials.

2- Once authenticated, navigate to **Identity and Access Management > Authentication > Citrix Gateway** and click **Connect**.

[![CSP-Image-024](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_024.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_024.png)

3- On the configuration pop-up screen, enter your publicly accessible AAA vServer FQDN and click **Detect**. Once detected, click **Continue**.

[![CSP-Image-025](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_025.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_025.png)

4- On the **Create a connection** screen, copy the **Client ID, Secret, and Redirect URL**.

[![CSP-Image-026](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_026.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_026.png)

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|DO NOT close this page, you must come back to finish the configuration.|

#### OAUTH IDP Profile

1- Back on Citrix ADC, navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > OAUTH IDP > Profiles** and click **Add**.

2- On the **Create Authentication OAUTH IDP Profile** page, enter the following information.

*  Name: OAUTH IDP authentication profile entity name
*  Client ID: Paste value from Citrix Cloud
*  Client Secret: Paste value from Citrix Cloud
*  Redirect URL: Paste value from Citrix Cloud
*  Issuer Name: ADC AAA vServer base URL
*  Audience: Same as Client ID
*  Send Password: Checked

[![CSP-Image-027](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_027.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_027.png)

3- Click **Create**.

#### OAUTH IDP Policy

1- Navigate to **Security > AAA – Application Traffic > Policies > Authentication > Advanced Policies > OAUTH IDP > Policies** and click **Add**.

2- On the **Create Authentication OAUTH IDP Policy** page, enter the following information.

*  Name: OAUTH IDP authentication policy entity name
*  Action: OAUTH IDP authentication profile
*  Expression: TRUE

[![CSP-Image-028](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_028.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_028.png)

3- Click **Create**.

#### OAUTH IDP Policy Binding

1- Navigate to **Security > AAA – Application Traffic > Virtual Servers** and click the previously created **AAA vServer**.

2- On the **Advanced Authentication Policies** pane, click **No OAuth IDP Policy** and bind the OAUTH IDP policy.

[![CSP-Image-029](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_029.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_029.png)

3- Back on the AAA vServer page, click **Done**.

#### Workspace Authentication

1- Back on Citrix Cloud, on the configuration page click **Test and Finish**.

2- Navigate to **Workspace Configuration > Authentication** and click **Citrix Gateway**.

3- On the configuration pop-up screen, check the box next to **"I understand the impact on the subscriber experience"** and click **Save**.

[![CSP-Image-030](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_030.png)](/en-us/tech-zone/design/media/reference-architectures_csp-ws-nfactor_030.png)

| NOTE:                                                                   |
| ----------------------------------------------------------------------- |
|At this point, navigating to the Citrix Workspace URL (customer.cloud.com) redirects users to the ADC AAA vServer. Once users log in with their respective IDP, they are redirected back to Citrix Workspace.|
|Single sign-on to virtual apps and desktops resources can be accomplished by integrating Citrix Workspace with Citrix FAS.|

### Other Configuration Resources

*  [Citrix ADC Configuration](/en-us/citrix-adc/current-release.html)
*  [Cloud Connector installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html)
*  [Hosting Connections (MCS)](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)
*  [VDA installation](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html)
*  [Machine Catalog creation](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html)
*  [Delivery Group creation](/en-us/citrix-virtual-apps-desktops-service/install-configure/delivery-groups-create.html)
*  [FAS Integration with Citrix Workspace](/en-us/citrix-workspace/workspace-federated-authentication.html)
