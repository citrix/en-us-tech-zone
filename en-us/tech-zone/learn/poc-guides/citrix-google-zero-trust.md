---
layout: doc
h3InToc: true
author: Rick Dehlinger
contributedBy: 
---

# Proof of Concept Guide: Delivering workspace security and zero trust with Citrix and Google Cloud

## About this PoC Guide

This Proof of Concept (PoC) guide is the ‘hands on’ partner to [Deliver workspace security and zero trust with Citrix and Google Cloud](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-workspace-security-and-zero-trust-with-citrix-and-google-cloud.pdf), aka the “[Solution Brief](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-workspace-security-and-zero-trust-with-citrix-and-google-cloud.pdf)”. The Solution Brief explores how the combination of Citrix Workspace and Google BeyondCorp provides unified, secure, and intelligent zero trust access to SaaS and web apps, plus Citrix Virtual Apps and Desktops. It also explores the architecture of the solution, and outlines how customers benefit from the integrated solution. As such, you want to review the [Solution Brief](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-workspace-security-and-zero-trust-with-citrix-and-google-cloud.pdf) before continuing.

This guide walks you through how to build and test the solution. It also provides you with insights into the known issues and workarounds that exist today. We lay out the prerequisites you need to have in place, then progress through the various steps you need to complete to build and test the solution effectively.

As mentioned in the [Solution Brief](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-workspace-security-and-zero-trust-with-citrix-and-google-cloud.pdf) - we’re anxious to hear **what opportunities emerge for you** after exploring and experiencing this solution for yourself. We’d love to know **how we can evolve** the solution to help you make an even bigger difference for what’s important to you! Drop us a line and let us know what’s on your mind? You can reach us via email to [the Citrix on Google SME working group](mailto:caceb231.citrix.onmicrosoft.com@amer.teams.ms).

## Solution Details

As you start to dig more deeply into how this solution or any zero trust solution works, you can quickly get overwhelmed with technical details. This is especially true with regard to authentication, configuration options, and possible access scenarios. To better understand how this unique ZeroTrust Access solution works, let’s look at a few different options or scenarios. These will help us better understand how and where this solution works.

Before we dive in, we need to establish some common ground on terminology. Specifically, we’ll cover Google Cloud Identity and its relationship with Google Workspace (G Suite), and Google Cloud. Google Cloud Identity, as a technology stack, has been around for many years. It’s most recognized as the identity ‘engine’ behind Gmail, G Suite or Google Workspace, where it’s been used for years. This was well before it ever became a standalone product offering can be used independently of G Suite (now called Google Workspace). It’s also used with Google Cloud, regardless of whether a Google Cloud customer also uses Google Workspace.

Consider the following diagram, which represents the structure/relationship between Google Cloud Identity, Google Workspace, and Google Cloud:

![google-cloud-identity](/en-us/tech-zone/learn/media/citrix-google-zero-trust_google-cloud-identity.png)

Google Cloud Identity, as a set of services, underpins all the services shown above. It’s a multitenant cloud service, and the primary domain uniquely represents each tenant. In the case of the lab we used to build this guide, that primary domain is *silvertonhcl.citrix.com*.

The Google Admin Console manages the GCID (Google Cloud Identity) tenant. It also manages numerous other services. This includes Google Workspace, Chrome Enterprise device management, and management of the users, groups, and OUs inside the identity tree.

GCID also provides you with authentication/authorization for Google Cloud Platform. In the context of Google Cloud Platform, the GCID primary domain becomes the ‘organization’ name. This is the top level object in the tree structure that represents a customer’s Google Cloud ‘estate’. All Google Cloud Platform resources are contained inside this tree hierarchy, including folders, sub folders, and projects.

Google Cloud Platform is managed from the Google Cloud Console, which is different than the Google Admin Console. But users and administrators log into either or both consoles using a Google Cloud Identity account. Furthermore, a Google Cloud Identity domain can be configured to trust a third party identity provider for authentication. Users, groups, or OUs are often synchronized with the Google Cloud Identity domain using tools such as Google Cloud Directory Sync.

### Authentication and Authorization Scenarios

The choice of authentication provider flow and identity federation architecture is key to providing the best possible user experience. It’s also important from a policy application and analytics perspective. Citrix Workspace and Google Cloud Identity (GCID) can both operate as Service Providers (SP) and Identity Providers (IdP) in federated authentication architecture. Under the right conditions, they can both facilitate SSO to web/SaaS apps. Both can also be inserted alongside each other in a federated identity chain (when using a capable third party IdP such as Okta). Being in the authentication flow is important to both services as it allows them to ‘see’ the authentication event and optionally apply access policy controls.

Cloud IAP requires Google Cloud Identity, and applies users/groups defined in Cloud Identity for authorization. In many environments today, Google Identity operates as a Service Provider. That is, it’s configured to trust a third party SAML IdP. The users or groups are typically synchronized using Google Cloud Directory Sync or third party tools.

It’s important to note that while GCID can be configured to trust a third party SAML IdP, **you can only configure one provider**. That provider is then used as the IdP for the entire GCID domain. It is also used for all Google services which rely on GCID. These include Google Workspace, IAP, IAM, and third party applications or services which are configured to trust GCID as SAML IdP.

It’s also important to note that a single Citrix Workspace (Citrix Cloud tenant) supports **one active IdP at a time**. However, for solution proofing you can toggle between configured IdPs. Each Workspace has a unique, customer configurable FQDN (https://< customer-FQDN >.cloud.com). This toggle impacts all users who use the Citrix Workspace instance on that unique FQDN. If you’re evaluating different IdP’s for Citrix Workspace, make sure you use a non-production Citrix Cloud tenant so you don’t adversely impact production users.

This PoC Guide introduces the following identity federation architectures:

#### Scenario 1 - Active Directory as primary IdP

In this common scenario, the customer chooses to use their own instance of Microsoft Active Directory as the IdP for Citrix Workspace. Google Cloud Identity is configured to trust Citrix Workspace as it’s IdP. Google Cloud Directory Sync is used to replicate users, groups, and attributes from Active Directory into Google Cloud Identity. Citrix Workspace provides the authentication services for Google Cloud Identity. This scenario allows users to authenticate against Active Directory for logging into Chrome, ChromeOS, Android, Google Workspace, and more. It also allows Citrix Workspace to provide single sign-on to Google Workspace applications launched through Citrix Workspace. Citrix Workspace uses the users’ Active Directory credentials to enable SSO to virtualized apps and desktops.

Citrix Workspace also provides you with integrated, time-based one-time password functionality. The functionality can be enabled for this scenario. This functionality provides customers with a two-factor authentication solution without purchasing any additional software, providing an extra layer of security on top of this industry leading solution. For more information on how to enable Active Directory plus token authentication in Citrix Workspace, see [Connect Active Directory to Citrix Cloud](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad.html).

This Proof of Concept Guide focuses on the Active Directory as IdP authentication scenario, providing detailed guidance on setup and configuration. It’s simple to set up, does not require any additional, third party software, and provides the best user experience for most access scenarios. It allows Citrix Workspace users to single sign-on to Google products and services (such as Google Workspace and the Google Cloud console). It also provides rich signals to Citrix Analytics, further strengthening the security posture and manageability of the solution.

#### Scenario 2 - Okta as primary IdP

In this scenario, the customer chooses to use Okta as their primary IdP. They use Okta’s tooling to synchronize users/groups/attributes between Active Directory, Okta directory, and Google Cloud Identity. Google Cloud ID, Active Directory, and Citrix Workspace are configured in appropriate Okta identity chains. Citrix Federated Identity Services is utilized to single sign-on users to virtualized applications and desktops.

While this scenario is the most complex to set up and requires an Okta subscription, it provides the richest user experience. Not to mention the most threat intelligence signals to both Google Security Center and Citrix Analytics. Okta provides a mechanism by which identity providers can be flexibly ‘chained’ together. This allows multiple IdPs to take part in authentication flows, thus receiving threat intelligence signals as users access the system.

While this authentication scenario is beyond the scope of this PoC Guide, further information on this solution can be found in the following Proof of Concept Guide: [Secure Access to SaaS Applications with Okta and Citrix Access Control](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/access-control-okta-sso.html).

#### Scenario 3 - Google Cloud Identity as primary IdP

In this scenario, the customer chooses to use [Google Cloud Identity](https://cloud.google.com/identity) as their primary IdP. Google Cloud Identity (GCID) is used for access to Google Workspace, Chrome, Chrome OS, Android, and potentially other SaaS applications which are configured to use Google Cloud Identity as the IdP. Citrix Workspace is also configured to use Google Cloud Identity as the IdP. If the customer also deploys virtualized applications via Citrix Workspace, Google Cloud Directory Sync ensures appropriate user objects exist in Active Directory. It also synchronizes specific active directory user attributes back to GCID, enabling Citrix Federated Authentication Services to single sign-on users to virtualized applications and desktops.

This scenario allows the customer to take advantage of Google Cloud Identity’s advanced features, such as multifactor authentication. It also provides access to Google’s library of pre-integrated SAML 2.0 and OpenID Connect apps, and the ability to look into threats with Security Center.

This scenario does not currently allow Citrix Workspace to provide you with single sign-on to Google products and services. Although with Google Chrome on the endpoint, Chrome handles the SSO directly. This scenario also currently reduces the quantity of threat intelligence signals fed to Citrix Analytics. It requires the use of Citrix Federated Authentication Services to provide SSO to virtualized applications and desktops.

### Policy Definition/Enforcement Options (Configuration Scenarios)

Both Google BeyondCorp and Citrix Workspace provide several advanced mechanisms which can be used to define/enforce corporate access policy. This PoC Guide walks you through setting up and testing the basics, creating a functional, integrated system. It then moves on to some advanced topics. It explores how we can add different types of policy controls and observe the user experience while we’re validating that policy is being applied appropriately.

Policy Definition (with BeyondCorp)  
Both platforms control who has access to web apps by authorizing (or subscribing) users based on group membership, though individual user accounts can also be used. In the integrated solution, they also use a common identity provider for authentication and the same groups for authorization.

BeyondCorp allows us to further restrict access to an IAP-secured app by using [IAM Conditions](https://cloud.google.com/iam/docs/conditions-overview). IAM Conditions are implemented by binding them to an IAM Role assignment. IAM conditions can be created based on the following ‘out of the box”:

-  [resource attributes](https://cloud.google.com/iam/docs/conditions-resource-attributes) (that is, the resource being accessed. Type, Service, or name). See [Configuring resource-based access](https://cloud.google.com/iam/docs/configuring-resource-based-access) for more information.
-  [request attributes](https://cloud.google.com/iam/docs/conditions-overview#request_attributes) (that is, date/time, host/path).  See [Configuring temporary access](https://cloud.google.com/iam/docs/configuring-temporary-access) and [Using host name and path conditions](https://cloud.google.com/iap/docs/cloud-iap-context-aware-access-howto#using_hostname_and_path_conditions) to learn more.

BeyondCorp can also use Access levels which are created and managed at the GCP Organizational level under Security/Access Context Manager. These can be flexibly applied to IAP-protected resources using IAM Conditions. Access levels can be set up based on a number of different [Access level attributes](https://cloud.google.com/access-context-manager/docs/access-level-attributes) including IP subnetwork, request origin (region), user identity, and device attributes. Access levels can also be combined or nested together to create granularly enforce corporate policy across a variety of access scenarios.

As we pointed out earlier, the Authentication and Authorization configuration has a direct impact upon the user experience. It also affects the quantity and richness of threat signals that are captured. Regardless of the AA configuration chosen, users get a streamlined login experience including single sign-on. Users also get a curated access experience, with all the applications and information being presented through the Citrix Workspace and Google Chrome.

Both platforms provide mechanisms that allow customers to define and enforce corporate access policy.

We explore:

-  Applying DLP controls using Citrix Enhanced Security on IAP-protected web apps
-  Restricting access with BeyondCorp using IAM conditions and VPC Service Network Controls
-  Restricting access with BeyondCorp using the Endpoint Verification extension and Device Attributes

### Access Scenarios

The BeyondCorp/Citrix Workspace Zero Trust Access solution supports several different access scenarios which may have an impact on the user experience. Other factors which may impact the user experience include:

-  Type of device or operating system
-  Locally installed browser
-  Citrix Workspace access method - Workspace web or Workspace app

Modern Windows OS with Chrome browser and Citrix Workspace app for Windows installed:

-  Launch IAP-protected app from Citrix Workspace web (via Chrome browser)
-  Launch IAP-protected app from Citrix Workspace app (latest version, Chrome browser set as local browser default)
-  Launch IAP-protected app directly from Chrome browser

Initially, we’re going to focus our Chrome browser initiated access scenarios on illuminating the single sign-on experience. As such, we perform launches in a Chrome instance before a user is logged into Chrome. In later scenarios, we shift to launching from Chrome WITH our test user logged in. This practice allows us to explore the UX with Google’s Endpoint Verification extension installed.

## Solution Configuration

Now let’s explore how to build the BeyondCorp/Citrix Workspace Zero Trust Access solution. Let’s start with the prerequisites you need to gather or create.

### Prerequisites

To build this solution, you need the following:

-  A Google Cloud Identity tenant, and a user account with super-admin equivalence in the Google Admin Console ([https://admin.google.com](https://admin.google.com))
-  One or more test user accounts which are NOT super-admins.
Why? Super-admins always bypass third party IdP configurations and authenticate directly to Google Cloud ID. As such, they’re not so useful for evaluating the user experience.
-  A [Google Cloud organization resource](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations) (created automatically when a Google Cloud Identity user logs into the [Google Cloud Console](https://console.cloud.google.com)). You also need an account that has the rights to administer Access Context Manager. See [IAM Roles for Administering Access Context Manager](https://cloud.google.com/access-context-manager/docs/access-control) for more information.
-  A GCP Project associated with the Google Cloud organization, with billing enabled. You also need a user with Project Owner rights. The IAP-protected resource resides in this project, as do some IAP specific settings such as OAuth consent/credentials.
-  A Citrix Cloud tenant, with full Administrator rights. You’ll also need an active trial or paid subscription for Citrix Workspace Premium or Citrix Workspace Premium Plus.
-  For the Authentication and Authorization scenario we’re going to cover here, you need:
    -  An online Citrix Cloud resource location, with at least one Cloud Connector installed. The Windows Server(s) with the Cloud Connector installed must be a member of an Active Directory domain. This is how Citrix Cloud communicates with the AD we use for federation. For an overview of how to set up a Citrix Cloud ‘Resource Location’, see [Set up resource locations](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html).
    -  Google Cloud Directory Sync installed/configured/run, with sample users and groups synchronized between Active Directory and Google Cloud Identity. For a walk-through of configuring/using GCDS, see [Federating Google Cloud with Active Directory: Provisioning user accounts](https://cloud.google.com/architecture/identity/federating-gcp-with-active-directory-synchronizing-user-accounts). To examine a functional GCDS configuration, see [Appendix 1: Sample Google Cloud Directory Sync configuration](#Appendix-1:-Sample-Google-Cloud-Directory-Sync-configuration).

### Basic Setup

Once you’ve collected or created the solution prerequisites, you can move on to configuring the basic integrations needed for the BeyondCorp/Citrix Workspace Zero Trust Access solution. These steps lay the foundation for exploring the advanced capabilities in the **Advanced Setup** section that follows. Before you begin the Advanced Setup, you need to complete the following steps:

#### Step 1: Create a basic Google Cloud IAP sample application

In this step, you create an IAP-protected sample application using Google’s tooling and tutorial. You then test access to the sample app via Chrome.

##### 1a: Create IAP-protected sample app

To get started building this solution, we’re going to start by creating an IAP protected sample application. In this solution guide we’ve used a simple, reproducible sample web application that we deployed on Compute Engine using this [excellent tutorial](https://cloud.google.com/iap/docs/tutorial-gce).

You can also follow this same tutorial, provide your own, or use one of the other getting started tutorials. These include [IAP with App Engine](https://cloud.google.com/iap/docs/app-engine-quickstart), [IAP for GKE](https://cloud.google.com/iap/docs/enabling-kubernetes-howto), and [IAP for on-premises apps](https://cloud.google.com/iap/docs/enabling-on-prem-howto).

##### 1b: Test access to IAP-protected sample app

Once you’ve got the “IAP sample app” created, let’s check our work! Check the following:

-  In the Google Cloud Console, select your IAP project and navigate to the Security/Identity-Aware Proxy topic. Make sure that you’ve got an HTTPS resource listed. Also, ensure that it has a sample user or group attached with the “IAP-secured Web App User” role assigned:

    ![“IAP-secured Web App User](/en-us/tech-zone/learn/media/citrix-google-zero-trust_iap-secured-web-app-use.png)

    > **For context:**
    >
    > Screenshots are taken from our lab project, which is called iap-example-1. Our test user is a member of _iapusers@silvertonhcl.com. This GCID user group was replicated from Active Directory, via GCDS. This was done along with our test user bcuser@silvertonhcl.com:
    >
    > ![iap-example-1](/en-us/tech-zone/learn/media/citrix-google-zero-trust_iap-example-1.png)

-  Launch an incognito window in Chrome, connect to the FQDN on your load balancer (that is, [https://app.domain](https://app.domain)). You will be redirected to and greeted by the Google sign-in dialog:

    ![“sign in dialog](/en-us/tech-zone/learn/media/citrix-google-zero-trust_sign-in-dialog.png)

    > **For context:**
    >
    > In our lab environment, the FQDN we configured for our IAP sample app was <https://iapsample1.silvertonhcl.com>. We used “BeyondCorp-Citrix IAP Validation” as the OAUTH consent screen title:![consent-screen-title](/en-us/tech-zone/learn/media/citrix-google-zero-trust_consent-screen-title.png)

-  From that sign-in dialog, you will be able to log in with a user that’s a member of the group you assigned the “IAP-secured Web App User” role to. It will log you into your sample app successfully:

    ![consent-screen-title](/en-us/tech-zone/learn/media/citrix-google-zero-trust_sample-app.png)

If all three of the validation steps passed, you’ve got a functional deployment of BeyondCorp! There’s more to it (as you’ll find out later) but this basic configuration - with a functioning sample IAP-secured web app - is enough to move on to the next step.

#### Step 2: Configure Citrix Workspace as the IdP for Google Cloud ID

In this step, you create a ‘special’ application in the Citrix Cloud Library. This is ‘special’ because we’re going to use it to configure Google Cloud ID to use a 3rd party (Citrix Workspace) as the identity provider (IdP). It’s also special because we’re going to use it to authorize users to log into Google Identity.

Once it’s configured, we’re going to use the ‘special’ application’s SSO properties to configure Google Cloud ID to use Citrix Workspace as it’s IdP.

Finally, we test our work! After completing this step, our test user will be able to log directly into our sample IAP-secured web app. This is done by providing valid Active Directory credentials to Citrix Workspace’s authentication flow. Our test user will also be able to log into Citrix Workspace and see/launch the ‘special’ application we created in the Citrix Cloud Library.

##### 2a: Verify prerequisites are in place for the Active Directory as IdP authentication scenario

We called these prerequisites out in the previous section, but it’s worth calling out again as you need them to complete this build. Those prerequisites are:

-  An online Citrix Cloud resource location, with at least one Cloud Connector installed. The Windows Server(s) with the Cloud Connector installed must be a member of an Active Directory domain.
-  Google Cloud Directory Sync installed/configured/run, with sample users and groups synchronized between Active Directory and Google Cloud Identity. For a walk-through of configuring/using GCDS, [see Federating Google Cloud with Active Directory: Provisioning user accounts](https://cloud.google.com/architecture/identity/federating-gcp-with-active-directory-synchronizing-user-accounts). For more details on the GCDS sync profile we used for this build, see [Appendix 1: Sample Google Cloud Directory Sync configuration](#Appendix-1:-Sample-Google-Cloud-Directory-Sync-configuration).
-  To verify that you’ve got a resource location up, and that AD is available to your Workspace tenant, log into the Citrix Cloud console. Click the hamburger icon in the top left corner, and navigate to Identity and Access Management/Domains. You will see at least 1 resource location and one forest/domain.

    > **For context:**
    >
    > Our lab forest/domain is called silvertonhcl.com, and we’ve got 1 resource location deployed called SilvertonHCL us-west1.
    >
    > ![resource location](/en-us/tech-zone/learn/media/citrix-google-zero-trust_resource-location.png)

-  To verify that GCDS has synchronized AD with Google Cloud ID, inspect your test users and the groups you are using side by side between the two identity stores. They must both exist in both identity stores, and share common attributes.

    > **For context:**
    >
    > Our test user is bcuser@silvertonhcl.com. The user has both **email** and **UPN** attributes set, giving us flexibility in what attribute we’re keying off of to trigger GCDS to synchronize the user object. We’re expecting users to apply UPN/email address for login.
    >
    > ![verify account synchronization](/en-us/tech-zone/learn/media/citrix-google-zero-trust_verify-that-gcds.png)
    >
    > We’re using two groups for this system build - Citrix Workspace Users and IAP BeyondCorp Users. The groups also have an email attribute set - we’re keying off this attribute to trigger GCDS synchronization. Google Cloud also uses the group email address when assigning IAM permissions in the Google Cloud Console:
    >
    > ![Google Cloud Console](/en-us/tech-zone/learn/media/citrix-google-zero-trust_google-cloud-console.png)
    >
    > We can check the Google Admin Console ([https://admin.google.com](https://admin.google.com)) and verify that our test user bcuser@silvertonhcl.com is synchronized to GCID. The groups we’re using are synchronized as well. Our groups contain our test user, and the group has an email address.
    >
    >![synchronized to GCID](/en-us/tech-zone/learn/media/citrix-google-zero-trust_synchronized-to-gcid.png)

Did your AD forest or domain show up as expected in the Citrix Cloud console? Do your test users and groups exist in both Active Directory and GCID? Now you’re ready to move on!

##### 2b: Create the ‘special’ app in the Citrix Cloud Library

In this step, you create a ‘special’ application in the Citrix Cloud Library. It’s ‘special’ because **we’re going to use it to configure Google Cloud ID to use a third party (Citrix Workspace) as the identity provider (IdP)**. It’s also special because **we’re going to use it to authorize users to log into Google Identity**.

To start, in the Citrix Cloud console ([https://citrix.cloud.com](https://citrix.cloud.com)), navigate to the Library using either the link on the **Home** screen or clicking the hamburger button and selecting Library:

![selecting Library](/en-us/tech-zone/learn/media/citrix-google-zero-trust_selecting-library.png)

The library is where you define and configure the web and SaaS applications that Citrix Workspace presents to users. The library is also where you authorize users for each app. Create an app by clicking the floating blue crosshair icon and selecting ‘Add a Web/SaaS App’ to start the workflow.

> To learn more about the Citrix Cloud Library and configuring web/SaaS apps, see [Access Control on Citrix TechZone](https://docs.citrix.com/en-us/tech-zone.html#citrix-access-control).

We’re going to use the Google Workspace Hub as our ‘special’ app. That gives us a chance to use it to test SSO through Workspace before we publish an IAP-protected web app. We use the **G Suite template** (in Citrix Cloud’s template Library) as our starting point:

![G Suite template](/en-us/tech-zone/learn/media/citrix-google-zero-trust_g-suite-template.png)

> **Note:**
>
> Your ‘special’ Library app may be different, but make sure it’s something that all of your users/accounts are authorized to access. In this scenario we’re using a Google Workspace app (the Hub), and we’re assigning a Google Workspace license to every user on the system.

Then we configure the appropriate **App details** and save them:

![App details](/en-us/tech-zone/learn/media/citrix-google-zero-trust_app-details.png)
We leave **Enhanced security** disabled for now, and move on to the **Single sign on** configuration. Note the **Assertion URL** and **Audience** fields have placeholders inserted from the G Suite template. Insert your top level GCID domain name into the URL path. The GCID domain name in our lab is silvertonhcl.citrix.com, so the correct Assertion URL is <https://www.google.com/a/silvertonhcl.citrix.com/acs>, and the Audience URL is <https://www.google.com/a/silvertonhcl.citrix.com> - yours will obviously be different:

![Enhanced security](/en-us/tech-zone/learn/media/citrix-google-zero-trust_enhanced-security.png)

Note the section outlined in red on the right side - this information is what you need for the GCID third part IdP configuration. You can get back to this information by editing the Library app later. Alternatively, you can download the Certificate and record the Login URL for use in the next task.

Click **Save**, and Finish to create our ‘special’ app.

##### 2c: Authorize users/groups to use the ‘special’ app

In the Citrix Cloud console, navigate back to the Library, find your special app, and **authorize users** for access. This authorization is done by clicking the ellipsis (three dots) and choosing ‘Manage Subscribers’:

![G Suite](/en-us/tech-zone/learn/media/citrix-google-zero-trust_gsuit.png)

Make sure you select your domain, and pick your users or group(s) to authorize them for this app. Pick carefully! We’re using the SAML properties/auth point from this Library app to configure GCID to trust Workspace as IdP. **Workspace won’t authorize users to log into other Google services (such as Google Workspace) without being subscribed to this ‘special’ Library app**.

![G Suite hub](/en-us/tech-zone/learn/media/citrix-google-zero-trust_g-suit-hub.png)

> **Note**
>
> The third Party IdP settings we configure shortly do not apply to GCID superadmin users - Super Admins always authenticate directly to GCID. Keep this in mind as you’re testing login experience/flow later in this guide!

##### 2d: Configure Google Cloud ID to trust Workspace as third party IdP

We have our ‘special’ Library app configured, and we’ve got the SAML configuration information - Login URL and Certificate. We can now configure GCID to trust Workspace as third Party IdP. To accomplish this, log into the Google Admin console ([https://admin.google.com](https://admin.google.com)) as a superadmin for your GCID domain. Navigate to Security, Settings, then find and click the Set-up single sign-on (SSO) with a third party IdP topic. Configure the URL values appropriately, upload the certificate, tick 'Use a domain specific issuer', and click Save (bottom right corner of the page):

![3rd party IdP](/en-us/tech-zone/learn/media/citrix-google-zero-trust_party-idp.png)

> **Note**
>
> The sign-in page URL in the screenshot above is truncated - use the whole URL copied from the Library app SSO properties. It includes an APPID=< guid > at the end, and your Citrix Cloud OrgID in the middle.
>
> Also: the Sign-out page URL is specific to your Workspace URL. It defaults to your OrgID, but can be changed/customized to your preferences. You can find it in the Citrix Cloud console, under Workspace Configuration/Access:

![Workspace Configuration](/en-us/tech-zone/learn/media/citrix-google-zero-trust_workspace-configuration.png)

>**Note**
>
>Citrix Workspace, when operating as an IdP, does not currently provide a change password URL. See [404 error on Chrome sign out](#issue:-404-error-on-chrome-logout) for details.

Once you’ve got GCID setup to trust Workspace as third party IdP, you’re ready to move on!

##### 2e: Verify Federated Identity Configuration and Google Workspace SSO (if applicable)

Time to test our work! SSO to web apps can be a little tricky to grasp. This is especially true when you’re working with a browser that you can log into with multiple identities. Let’s start with a Chrome incognito window, and work our way up from there.

To start verifying, open a Chrome incognito window and connect to our ‘special’ application URL. This works for our verification purpose since it’s ‘behind’ GCID which is using Workspace as its IdP. So the authentication flow uses Citrix Workspace. For our ‘special’ app, that URL is [http://apps.google.com/user/hub](http://apps.google.com/user/hub). You’re likely to get the redirection ‘speed bump’ as seen below – this allows the Google service to ‘find’ the right GCID domain to authenticate to. Enter the test user credentials here and click **Next**:

![special-application url](/en-us/tech-zone/learn/media/citrix-google-zero-trust_special-application-url.png)

>**Tip**
>
>Want to simplify the logon experience for IAP-protected resources? Consider setting access_settings.oauth_settings.login_hint - it can save some clicks and improve the access experience for your users. See [Authenticating using a Google Workspace domain](https://cloud.google.com/iap/docs/customizing#authenticating_using_a_domain) in the IAP documentation.

As long as your test user is NOT a GCID super admin, you will be directed into the Citrix Workspace authentication flow. (Remember that [GCID ‘super admins’ are ‘exempt’](https://support.google.com/a/answer/6341409?hl=en) from using the third party IdP authentication flow!) Click **Sign In** for Employee Users:

![Employee Users](/en-us/tech-zone/learn/media/citrix-google-zero-trust_employee-users.png)

You will be directed to the Citrix Workspace authentication form. Here’ we’ve applied [custom branding to our Workspace](https://docs.citrix.com/en-us/citrix-workspace/configure.html#customize-the-appearance-of-workspaces) - the Google Cloud logo is a good visual indication that we’re on the right track! We’ll log in with our test user account:

![authentication form](/en-us/tech-zone/learn/media/citrix-google-zero-trust_authentication-form.png)

If all goes to plan, our ‘special’ app comes up, and our test user is logged in successfully! [https://docs.citrix.com](https://docs.citrix.com)

![Dashboard](/en-us/tech-zone/learn/media/citrix-google-zero-trust_dashboard.png)

Assuming your first test was successful, we can now test the same app launch through the Citrix Workspace app. If you don’t have the Citrix Workspace app installed, you can download it from the [Workspace app download page](https://www.citrix.com/downloads/workspace-app/). Configure it to use your test Workspace URL (<https://silvertonhcl.cloud.com> in our case) on first use. Then you will see the same (possibly customized) login dialog. Go ahead and sign in.

![login dialog](/en-us/tech-zone/learn/media/citrix-google-zero-trust_login-dialog.png)

After a successful login, we will be able to find and click to launch our ‘special’ app:

![click to launch](/en-us/tech-zone/learn/media/citrix-google-zero-trust_click-to-launch.png)

Assuming we’re not already logged into Chrome, we will see a verification dialog similar to the one below:
![verification dialog](/en-us/tech-zone/learn/media/citrix-google-zero-trust_verification-dialog.png)

>**Note**
>
>The user sign-on experience can get more complicated if you are already logged into Chrome as a different user. This is also true if you’ve got multiple users logged into Chrome. For simplicity/clarity, we test with no users logged in.

After clicking **Continue** at the ‘speed bump’, we will be logged into both our ‘special’ app and Chrome as our test user:

![speed bump](/en-us/tech-zone/learn/media/citrix-google-zero-trust_speed-bump.png)

Success? Good! Now we can move on to publishing our IAP-protected sample app.

#### Step 3: Publish IAP-protected sample app to Citrix Workspace with SSO

Now that you’ve got Google Cloud ID successfully set up to use Citrix Workspace as the IdP, it’s time to bring BeyondCorp and Citrix Workspace together. In this step, we publish the IAP-secured sample app to Citrix Workspace by creating a test Citrix Cloud Library app and configuring it for SAML SSO. We subscribe our test user/group to our Library app, then we’re ready to run through our access scenarios, checking/validating the user experience.

Publishing an IAP-protected app to Citrix Workspace, with SSO, is similar to the process we went through to [configure our ‘special’ app](#2b:-create-the-‘special’-app-in-the-citrix-cloud-library), with one critical exception: **IAP-protected apps must launch using a Service Provider initiated SSO flow** - they don’t support the default IdP initiated flow. Keep reading to learn more.

##### 3a: Create a Citrix Cloud Library app in the Citrix Cloud console

To complete this step, initiate the same Add Web/SaaS App workflow we used [earlier](#2b:-create-the-‘special’-app-in-the-citrix-cloud-library), also using the G Suite template as your starting point. We give it a unique name so we can create a second Library app later with Enhanced security enabled:

![Enhanced security enabled](/en-us/tech-zone/learn/media/citrix-google-zero-trust_enhanced-security-enabled.png)

> We gave our IAP-protected sample app a URL of <https://iapsample1.silvertonhcl.com>, so we define it as shown above. The Related Domains field is populated automatically, and we gave it a custom icon and description which users see.

Save the App details, leave Enhanced security disabled, and move on to the Single sign on configuration. Note that the **SSO** properties we’re going to set are ALMOST identical to the G Suite template based ‘special’ app we created earlier. There is one critical difference which we introduced earlier (highlighted below):

![Single sign on configuration](/en-us/tech-zone/learn/media/citrix-google-zero-trust_single-sign-on-configuration.png)

> **Note**
>
> The highlighted tick box above: you need to **enable SP initiated SSO** as IAP-protected applications do not support the G Suite template default, which is IdP initiated. The differences between SP and IdP initiated SSO flows are beyond the scope of this guide . But the SSO flow option does have an impact on the user experience, particularly when you have multiple users that are logged into Chrome. If you’d like to learn more, [this document](https://developer.okta.com/docs/concepts/saml/) provides a good primer on SAML. The Relay State is inherited from the App details we defined earlier.

After you’ve configured the **SSO** properties correctly (including enabling SP initiated SSO), Save the Single sign on options. Click **Finish** to save our first IAP application definition.

##### 3b: Authorize users/groups to use the IAP-protected Library application

This step uses the same flow as we ran in [this earlier step](#2c:-authorize-users/groups-to-use-the-‘special’-app), though we’re going to use a different user group this time. In our lab, we’re using this group specifically to authorize users to access IAP-protected applications, but your authorization strategy may differ:

![manage subscribers](/en-us/tech-zone/learn/media/citrix-google-zero-trust_manage-subscribers.png)

> In our scenario, we’re selecting a group from Active Directory as our Workspace tenant is using AD as it’s IdP. Since we’ve also configured GCDS to sync this group to GCID, the authorization is functional. This is despite authorizing our IAP-protected app by assigning the group from GCID.

##### 3c: Retest access to the IAP-protected app

Now that we’ve published our IAP-protected app as a Citrix Library app, we can test our work! We start with a Chrome incognito window and work our way up the stack. Open a fresh Chrome incognito window and direct it to the IAP-protected app URL. In our lab, that’s <https://iapsample1.silvertonhcl.com>. You will see the same ‘speed bump’ dialog we saw when we tested the IAP-protected app earlier. It will show the title you configured for your **OAUTH consent** screen. In our case, we used “BeyondCorp-Citrix IAP Validation” as the **OAUTH consent** screen title:

![“BeyondCorp-Citrix IAP Validation](/en-us/tech-zone/learn/media/citrix-google-zero-trust_beyondcorp-citrix-iap-validation.png)

This time, however, we will be redirected through the Citrix Workspace authentication flow, ultimately entering our test user credentials into the Workspace login form:

![Workspace login form](/en-us/tech-zone/learn/media/citrix-google-zero-trust_workspace-login-form.png)

![Workspace login form](/en-us/tech-zone/learn/media/citrix-google-zero-trust_workspace-login-form-next.png)

We will now be connected to the IAP-protected sample application, authenticated as our test user:

![test user](/en-us/tech-zone/learn/media/citrix-google-zero-trust_test-user.png)

Now let’s proceed with testing the launch from the **Citrix Workspace app**. Log into the Citrix Workspace app as your sample user, if you haven’t already done so. Find the IAP-protected sample app and launch it. You may need to refresh your Workspace to see the IAP-protected sample app, but once you find it, click to launch it. You will see a similar ‘speed bump’ dialog we saw earlier:

![test the launch](/en-us/tech-zone/learn/media/citrix-google-zero-trust_test-the-launch.png)

>**Remember**
>
>The access experience can get more complicated if you’re logged into Chrome as a different user. This is also true if you’ve got multiple users logged in. For simplicity while testing, remove all cookies and sign out of Chrome before launching the IAP-protected app from Citrix Workspace.

After a few authentication redirects, you will be logged into both Chrome and the IAP-protected app - without signing in again:

![without signing in again](/en-us/tech-zone/learn/media/citrix-google-zero-trust_without-signing-in-again.png)

Finally, let’s test accessing the IAP-protected app from **Citrix Workspace web**. For this test, open up a Chrome tab and direct it to your Citrix Workspace URL. Log in to Citrix Workspace web with your test user:

![Citrix Workspace web](/en-us/tech-zone/learn/media/citrix-google-zero-trust_citrix-workspace-web.png)

> The URL for our lab Citrix Workspace is <https://silvertonhcl.cloud.com>. Yours will be different.

Upon login, your test user is presented with its authorized apps. Click your IAP-protected sample app to launch it. You will see the same results you saw when launching your IAP-protected app directly from Chrome earlier.

### Advanced Setup

You’ve successfully built and tested a basic Zero Trust Access solution with BeyondCorp/Citrix Workspace. Now it’s time to get into some more advanced topics. Both platforms provide mechanisms for defining and enforcing corporate access policies in this Zero Trust Access solution. In this section, we’re going to explore these policy mechanisms by building upon the foundation we’ve already created. We’ll then run through our access scenarios, checking/validating the user experience.

#### Step 4: Applying DLP controls using Citrix Enhanced Security on IAP-protected web apps

Citrix Workspace provides extra security features which can be optionally enabled for **web and SaaS apps** on an application by application basis. The Enhanced security features are a property of the app in the Library, and include options to restrict clipboard access, printing, navigation, and downloads. It can also display a watermark of the user name and IP address to help deter users from collecting screenshots of sensitive data. In this step, you enable Enhanced security on your Library app, then test the user experience.

##### 4a: Enable Enhanced Security

To complete this step, you can simply toggle Enhanced security and the specific settings of your choice on the Library app you created earlier. We’ve selected all the Enhanced security options below, but you can play with different combinations of restrictions if you’d like. Once you’ve made the changes, don’t forget to Save the **Enhanced security** settings and Finish to complete the editing process:

![editing process](/en-us/tech-zone/learn/media/citrix-google-zero-trust_editing-process.png)

##### 4b: Test the user access experience with Citrix Enhanced Security enabled

Start by testing the user access experience from the Citrix Workspace app. Refresh your app list or logoff and logon if you created a second Library application and don’t see it. Find and launch your IAP-protected app with SSO and Enhanced Security enabled:

![IAP-protected app with SSO](/en-us/tech-zone/learn/media/citrix-google-zero-trust_iap-protected-app-with-sso.png)

Consider that a user launches a web app which was published with Enhanced security enabled. If the launch comes from the Citrix Workspace app on Windows or OSX, the web app is launched using Citrix Workspace’s embedded browser directly on the endpoint device. The embedded browser signs the user on to the app and enforces the Enhanced Security options you selected:

![Enhanced Security options you selected](/en-us/tech-zone/learn/media/citrix-google-zero-trust_enhanced-security-options-selected.png)

We enabled all the Enhanced security controls earlier, and have highlighted the modified UI elements in the screenshot above.

Now let’s try launching the same app from Citrix Workspace web. From Chrome, navigate to your Citrix Workspace URL and login:

![Citrix Workspace URL and login](/en-us/tech-zone/learn/media/citrix-google-zero-trust_citrix-workspace-url-login.png)

For clarity of the UX, notice that our test user is not signed into Chrome, even after they’ve logged into Citrix Workspace web. Now we can launch our IAP sample app with a click:

![clarity of the UX](/en-us/tech-zone/learn/media/citrix-google-zero-trust_clarity-of-the-ux.png)

You notice a slightly different user experience with this test - we’ve highlighted some of the differences in UX above. The UX differs when the web application with Citrix Enhanced Security enabled launches from Workspace web. It might also be from Workspace app on end user devices that run something other than Windows or OSX. This is because the app launches in a separate browser tab using Citrix’s Secure Browser Service. The Secure Browser Service provides a virtualized browser instance that runs inside the local browser tab. It enforces the Enhanced Security options in the virtualized browser.

When we click through the ‘speed bump’ logon flow, our IAP-protected app comes up and the same DLP controls we saw/enabled earlier are applied:

![DLP controls](/en-us/tech-zone/learn/media/citrix-google-zero-trust_dlp-controls.png)

You’ve worked with Citrix Workspace’s Enhanced Security options. Now let’s explore some of the more advanced options for access control restriction available through BeyondCorp.

#### Step 5: Explore advanced access policy controls in BeyondCorp

In [Step 1 of this guide](#step-1:-Create-a-basic-Google-Cloud-IAP-sample-application), we created a basic IAP-protected sample web application. We saw how IAP uses GCID to authorize access based on group membership, using an IAM policy and the “IAP-secured Web App User” IAM role. To help understand what we built earlier, consider the following screenshot. It shows the properties of the IAP-secured sample app we created earlier:

![Step 1 of this guide](/en-us/tech-zone/learn/media/citrix-google-zero-trust_step-1-of-this-guide.png)

If we click the edit icon next to our authorized group (bottom right corner above) we can view and configure the permissions assigned to the IAP resource:

![permissions assigned to the IAP resource](/en-us/tech-zone/learn/media/citrix-google-zero-trust_permissions-assigned-to-iap-resource.png)

Here we see that the “IAP-secured Web App User” IAM role has been assigned to our IAP user group. Notice that no additional conditions have been bound to the role assignment - we come back to that shortly! This IAM policy binding effectively grants any successfully authenticated member of the [_iapusers@silvertonhcl.com](mailto:_iapusers@silvertonhcl.com) group access to our IAP-protected resource.

BeyondCorp allows us to further restrict access to an IAP-secured app by using [IAM Conditions](https://cloud.google.com/iam/docs/conditions-overview). Think of IAM Conditions as a flexible way to define conditional, attribute-based access control for Google Cloud resources (such as IAP-secured resources).

IAM Conditions are implemented by binding them to an IAM Role assignment. IAM conditions can be created based on the following ‘out of the box”:

-  [resource attributes](https://cloud.google.com/iam/docs/conditions-resource-attributes) (that is, the resource being accessed. Type, Service, or name). See [Configuring resource-based access](https://cloud.google.com/iam/docs/configuring-resource-based-access) for more information.
-  [request attributes](https://cloud.google.com/iam/docs/conditions-overview#request_attributes) (that is, date/time, host/path).  See [Configuring temporary access](https://cloud.google.com/iam/docs/configuring-temporary-access) and [Using host name and path conditions](https://cloud.google.com/iap/docs/cloud-iap-context-aware-access-howto#using_hostname_and_path_conditions) to learn more.

BeyondCorp can also utilize [Access levels](https://cloud.google.com/access-context-manager/docs/overview#access-levels) as IAM conditions. Access levels are created/managed at the GCP Organizational level under Security/[Access Context Manager](https://cloud.google.com/access-context-manager/docs/overview). Once created, they can be applied to resources inside the GCP Organization, such as IAP-protected resources, using IAM Conditions.

Access levels can be set up based on a broad variety of [access level attributes](https://cloud.google.com/access-context-manager/docs/access-level-attributes), including:

-  [IP subnetworks](https://cloud.google.com/access-context-manager/docs/create-access-level#corporate-network-example) (CIDR blocks)
-  Regions (identified by [ISO 3166-1 alpha 2 codes](https://www.iso.org/obp/ui/#search/code/))
-  Other access levels (that is, nested dependencies)
-  Members (specific users or service accounts, via gcloud or Access Context Manager API only)
-  Device policy (one or more [device attributes](https://cloud.google.com/access-context-manager/docs/create-access-level#device-example) collected by the [Endpoint Verification](https://cloud.google.com/endpoint-verification/docs/overview) browser extension)

Access levels can be created by defining one or more conditions, which can be made up of one or more attributes. Multiple conditions can be combined to create sophisticated access levels, and multiple access levels can be bound to an IAP-protected resource. In short - BeyondCorp policies are flexible! They can also be a bit complicated to understand initially, but we guide you through a couple exercises to get you started.

>**Tip**
>
> Consider enabling Cloud Audit Logs on the GCP project where your IAP protected resource resides. This helps you with troubleshooting as you explore the results of your work here. See [Enabling Cloud Audit Logs](https://cloud.google.com/iap/docs/audit-log-howto) for more information.

##### 5a: Create Test Access levels in Access Context Manager

To explore BeyondCorp advanced policy controls, we create a couple simple access levels, then we bind them to our IAP-protected resource and explore the outcomes. To begin, change to the Organization level in the Google Cloud Console, and navigate to Security/Access Context Manager.

![navigate to Security/Access Context Manager](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5a-navigate-to-security-access-context-manager.png)

From here, we’ll create two simple Access levels, which we’ll apply to our IAP-protected resource in a future step. Get started by clicking **New**, and then create the first Access level as shown below:

[first Access levels](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5a-first-access-level.png)

We call this first Access level **on_authorized_network**. This basic mode policy uses the IP Subnetworks condition, which returns TRUE if the user is coming from a known CIDR block. Here we’ve identified our test device’s public IP address using a tool such as [https://whatsmyip.org](https://whatsmyip.org)). We’ve also defined this public IP address as “..*/32”. We’ve got a couple devices to test from, and they’ve got different public IP addresses. So we can verify the policy binding is working by switching devices. If you don’t have a second device, you can simply modify the CIDR block here and retest. Click Save once you’re done.

Now let’s create a second Access level using the properties shown below:

![second Access levels](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5a-second-access-level.png)

We call this one **on_authorized_device**. We configure this access level to use a Device Policy, which means we are deploying and using the Endpoint Verification extension with Chrome. We use a [simple policy attribute](https://cloud.google.com/endpoint-verification/docs/setting-up-device-approvals) which requires Admin intervention, providing us with an opportunity to explore the user experience end to end. Click save when you’re done.

You will now see the two access levels in Access Context Manager:

![Access Context Manager](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5a-access-context-manager.png)

##### 5b: Bind Access levels to IAP-protected resource

Now that we’ve got a couple access levels defined, it’s time to bind them to our IAP-protected resource. Start by selecting the project that contains your IAP-protected resource, navigating down to Security/Identity-Aware Proxy. You will see your IAP resource object. It is likely to have the unconditional IAM policy binding we created in Step 1.

>**Note**
>
> Due to a UI bug identified during testing, we delete and re-create the IAM policy binding. This allows us to test different combinations of access level evaluation and enforcement.

To remove an existing IAM policy binding, click the recycle bin icon and confirm the action. We use the Add Member button to recreate the binding:

![recreate the binding](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5b-recreate-binding.png)

After clicking **Add Member**, re-create the policy binding by identifying your test user group and selecting the “IAP-secured Web App User” role.

![selecting role](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5b-selecting-role.png)

After selecting the IAP-secured Web App User role, the **Access Levels** drop-down list will appear. Click the down arrow to see the access levels we created earlier. You will be able to select one or more access levels to restrict the policy binding further.

>**Note**
>
> Not seeing the Access Levels drop down list? Check your admin user’s IAM role bindings in the project. Select your IAP Project, then navigate to Security/IAM. Your admin user must have have the **Access Context Manager Admin** role and the **IAP Policy Admin** roles assigned either directly or via policy inheritance.

Start by binding **on_authorized_network** and save the binding. This process is repeated later to validate our second access level policy.

![validate our second access level](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5b-validate-second-access-level.png)

>**Note**
>
> Policy bindings are usually replicated and active in 60 seconds or less. Although certain policies can take up to 7 minutes to replicate and become active.

##### 5c: Test on_authorized_network access level binding

Now that we have our first access level bound to the IAP-protected app, let’s proceed with testing the UX.

First we launch the IAP-protected app from a device we know is NOT on what we defined as an ‘authorized network’. We start with a direct launch from Chrome:

![launch from Chrome](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-launch-from-chrome.png)

As expected, the user is denied access. Now we try it from a different browser, just for giggles:

![different browser](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-different-browser.png)

Same result! This makes sense as we’ve not implemented any controls that require a specific browser configuration.

Now let’s launch our IAP-protected app from Citrix Workspace app:

![launch our IAP-protected app](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-launch-iap-protected-app.png)

As we’d expect, our IAP sample app launches in Citrix Workspace embedded browser, and access is denied. Also, we notice that Citrix Enhanced Security DLP controls are activated on the browser window, including the watermark with my endpoint’s local IP address.

Now we try the launch from Citrix Workspace web:

![launch from Citrix Workspace web](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-launch-from-citrix-workspace-web.png)

As we’d expect, access is denied again. Our IAP-protected app was launched in a separate tab using Citrix’s Secure Browser Service. Notice also the Citrix Enhance Security DLP controls activated. There’s a watermark over the top of the UI. You see the IP address identified is different: why? In the previous screenshot, the IAP-protected app is processed and presented using Citrix Workspace’s embedded browser that runs on the client device. This has a local IP address of 192.168.42.x. In the screenshot above, the IAP-protected app is processed and presented from a virtualized browser in the cloud. That browser has both a different private and public IP address.

Now let’s test from our device that IS on the IP network we’d previously defined as ‘authorized’. First up - a direct launch from Chrome:

![direct launch from Chrome](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-direct-launch-from-chrome.png)

No surprises there. Now let’s try launching from Citrix Workspace app:

![try launching from Citrix Workspace app](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-try-launching-from-citrix-workspace-app.png)

Again no surprises. ...or is there one? Since Enhanced security is still enabled, we’d expect it to launch using the Workspace embedded browser, but notice the IP address in the watermark: it doesn’t match our ‘authorized network’, so what’s up? The IP address in the watermark is the local private IP address on the client, but outbound Internet access goes through Cloud NAT in this case. The NAT’d external IP address IS in the ‘authorized’ CIDR block we used when defining our currently bound access level. Since IAP ‘sees’ our traffic coming from the NAT’d external IP, it’s allowed through.

Finally, a launch through Citrix Workspace web:

![launch through Citrix Workspace web](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5c-launch-through-citrix-workspace-web.png)

Since we launched the IAP-protected app through Workspace web, we’d expect it to launch in a separate tab and use the Secure Browser Service. Since the virtualized browser is actually running in the cloud, we’d expect it to have a different IP address. This isn’t going to match our ‘authorized’ CIDR block.

##### 5d: Enable Endpoint Verification extension (and optionally deploy with Chrome Enterprise policy)

For our last scenario, we’re going to apply an access level (policy) that incorporates device state in the mix. The Endpoint Verification extension captures device state signals. The extension captures and logs device state details to the Google Admin console ([https://admin.google.com](https://admin.google.com)).

To get started, let’s first make sure that Endpoint Verification is enabled. Turn endpoint verification on or off describes how to enable EV if needed, though it’s enabled by default. Here’s a quick snip of the instructions:

![Endpoint Verification](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5d-endpoint-verification.png)

A quick check in our Admin console shows that Endpoint Verification is indeed already enabled:

![Admin console](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5d-admin-console.png)

[Turn endpoint verification on or off](https://support.google.com/a/answer/9007320) also describes a couple ways to get the Endpoint Verification extension installed. We can allow users to install the extension manually, or we force-install the extension. We go for option 2, and force install the extension - you can choose instead to manually install the extension if you’d like

![Verification extension installed](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5d-verification-extension-installed.png)

Next, let’s configure a ‘speed bump’ of our own. This allows us to step through and explore the device state information flow. To do this, we’re going to set up device approvals using the [Setting up device approvals](https://cloud.google.com/endpoint-verification/docs/setting-up-device-approvals) How-to guide.

Following the guide, we found and enabled device approvals. We also added an admin email address to send approval requests to, and saved the setting:

![enabled device approvals](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5d-enabled-device-approvals.png)

Finally, we need to reconfigure our IAM policy binding on the IAP-protected resource so that it’s using the **on_authorized_device** condition. This process is the same as we used in step 5b of this guide, but we’re choosing a different access level. Once complete, it will look something like this:

![reconfigure our IAM policy](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5d-reconfigure-iam-policy.png)

##### 5e: Test on_authorized_device access level binding

For our first test, we try launching our IAP-protected resource directly from Chrome in Incognito mode:

![IAP-protected resource directly from Chrome](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-iap-protected-resource-directly-from-chrome.png)

This launch was denied for a couple reasons. For starters - we haven’t yet approved this device for accessing corporate resources. Also, we tried the launch from an Incognito window. But BeyondCorp doesn’t allow access from Incognito windows when we’re enforcing access policy that is based on device attributes

Let’s try that again, but this time we don’t use Incognito mode. Also, we log into Chrome as our test user first. After logging into Chrome, we can click the **Extensions** icon and see that Endpoint Verification was automatically installed for us:

![Endpoint Verification was automatically installed](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-endpoint-verification-automatically-installed.png)

If we now navigate to our IAP-protected app, we see that access again denied. We expected this, however, because we set up the ‘speed bump’ by requiring Admin approval for new devices. When we return to the Google Admin console and navigate to Devices/Mobile and endpoints, we see our new device listed with a status of “Pending approval”:

![“Pending approval](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-pending-approval.png)

We approve this device by clicking the far right button and selecting APPROVE DEVICE:

![selecting approve device](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-selecting-approve-device.png)

After approving the device, we go back and refresh the page. Success!

![refresh the page](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-refresh-page.png)

Now let’s try launching our IAP-protected app from Citrix Workspace app. Remember that we’ve still got Advanced security enabled on our IAP sample app:

![launching our IAP-protected app](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-launching-iap-protected-app .png)

Denied. Why? The Citrix Workspace embedded browser doesn’t have the Endpoint Verification extension installed, so it doesn’t come back as an ‘Approved’ device. We see the same behavior if we try launching the IAP-protected app from Citrix Workspace web. This is because the Secure Browser Service also doesn’t have the Endpoint Verification installed.

For one last test, let’s go back to our IAP sample app in the Citrix Cloud Library and disable Advanced security. Once our work is saved, let’s rerun our Workspace App and Workspace web launch tests.

First up, Workspace app. With Advanced security disabled, our IAP-protected sample app launches in the local Chrome browser, which has the Endpoint Verification extension installed. The device has also been Approved in the Google Admin console, so we’d expect it to work:

![advanced security disabled](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-advanced-security-disabled.png)

Finally, we try the launch again, this time from Workspace web:

![from Workspace web](/en-us/tech-zone/learn/media/citrix-google-zero-trust_5e-from-workspace-web.png)

Our app launches successfully in a new tab in the local Chrome browser. This has the Endpoint Verification extension installed, and the device has been Approved in the Google Admin console).

## Observations and Conclusions

Now that we’ve set up and tested the Citrix Workspace + BeyondCorp zero trust access solution, let’s distill out some observations and conclusions:

-  With the right authentication and authorization scenario, IAP-protected web apps can be defined, presented, and launched from Citrix Workspace. It can provide the user with a single sign-on experience.
-  When configuring IAP-protected apps for SSO in Citrix Workspace, you must enable the service provider initiated SSO flow.
-  Most of BeyondCorp’s access policies can be used to further restrict access when used with Citrix Workspace, even with Citrix’s Advanced security features (DLP controls) enabled.
-  When using BeyondCorp’s device state policies to restrict access, Citrix’s Advanced security cannot be enabled. This is because the BeyondCorp device state policies require the Endpoint Verification extension. This extension is not currently there inside either Citrix Workspace embedded browser or Citrix Secure Browser Service.

## Known Issues and Workarounds

### Issue: 404 error on Chrome logout

**Description**: When signing out of Chrome when the Google Cloud ID domain is using Citrix Workspace as IdP, the user is effectively logged out, but receives a 404 error message:

![404 error message](/en-us/tech-zone/learn/media/citrix-google-zero-trust_404-error-message.png)

![server error](/en-us/tech-zone/learn/media/citrix-google-zero-trust_server-error.png)

This error message appears because Citrix Workspace, when used as an IdP, doesn’t currently redirect Chrome to a functional login dialog.

**Workaround**: None currently identified. This is a usability issue and does not compromise the security or functionality of the solution. Citrix Engineering is aware of the issue and is evaluating ways to improve the UX in this scenario.

## FAQ

Q: What IAP/BC features are available to apply to Google applications (such as Google Workspace)?

-  Context-Aware Access: [https://support.google.com/a/answer/9275380](https://support.google.com/a/answer/9275380)
    -  Continuous policy evaluation for browser accessed apps/services
    -  Not enforced on Google Workspace mobile apps, Apple Mail app, desktop apps (like Drive File Stream)

Q: What IAP/BC features are available to apply to third party SAML apps?

-  Context-Aware Access: [https://support.google.com/a/answer/9275380](https://support.google.com/a/answer/9275380)
    -  ...applied at sign in/session start vs. continuously.

## Additional Resources

Deploying Endpoint Verification with Google Admin Console: [https://cloud.google.com/endpoint-verification/docs/deploying-with-admin-console](https://cloud.google.com/endpoint-verification/docs/deploying-with-admin-console)

[Configure SAML single sign-on for Chrome devices - Google Chrome Enterprise Help](https://support.google.com/chrome/a/answer/6060880?hl=en)

[*Overview of IAM Conditions > Cloud IAM Documentation*](https://cloud.google.com/iam/docs/conditions-overview)

[Setting up context-aware access with Identity-Aware Proxy](https://cloud.google.com/iap/docs/cloud-iap-context-aware-access-howto)

[Identity-Aware Proxy overview](https://cloud.google.com/iap/docs/concepts-overview)

[https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-sso.html#sso-web-apps](https://docs.citrix.com/en-us/tech-zone/learn/tech-briefs/workspace-sso.html#sso-web-apps)

[Citrix Access Control](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/access-control.html)

[Enable single sign-on for workspaces with Citrix Federated Authentication Service](https://docs.citrix.com/en-us/citrix-workspace/workspace-federated-authentication.html)

Configuring G Suite SSO with [NetScaler with Unified Gateway
G Suite Microapp integration documentation](https://docs.citrix.com/en-us/netscaler-gateway/downloads/G-Suite-single-sign-on-configuration.pdf)

Configure G Suite SSO using [this snippet in our product docs](https://docs.citrix.com/en-us/citrix-gateway-service/saas-apps-templates/citrix-gateway-gsuite-saas.html),

[https://cloud.google.com/endpoint-verification/docs/overview](https://cloud.google.com/endpoint-verification/docs/overview)

[https://cloud.google.com/iap/docs/tutorial-gce](https://cloud.google.com/iap/docs/tutorial-gce)

[https://cloud.google.com/iap/docs/concepts-overview?hl=en](hthttps://cloud.google.com/iap/docs/concepts-overview?hl=en)

[https://support.google.com/a/answer/7390066?hl=en](https://support.google.com/a/answer/7390066?hl=en)

[https://cloud.google.com/resource-manager/docs/quickstart-organizations](https://cloud.google.com/resource-manager/docs/quickstart-organizations)

[https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy#organizations)

## Appendix 1: Sample Google Cloud Directory Sync configuration

GCDS provides immense flexibility in setting up sync profiles to serve a broad variety of use cases. Our lab GCDS sync profile is simple but functional. Yours is likely to be different, but here are the high points to help you understand how we made it function for this system build.

As a refresher - our test user is bcuser@silvertonhcl.com. The user has both **email** and **UPN** attributes set, giving us flexibility in what attribute we’re keying off of to trigger GCDS to synchronize the user object. We’re expecting users to use UPN/email address for login.

![As a refresher](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-refresher.png)

We’re using two groups for this system build - Citrix Workspace Users and IAP BeyondCorp Users. The groups also have an email attribute set - we’re keying off this attribute to trigger GCDS synchronization.

![GCDS synchronization](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-gcds-synchronization.png)

Now let’s explore our GCDS configuration. For starters, we’re only synchronizing User Accounts and Groups (under General Settings). On the User Accounts, we’re using Active Directory standard attributes for the four key GCID attributes we want synchronized:

![GCDS configuration](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-gcds-configuration.png)

We’re using a sync rule that excludes users/groups without email addresses:

![sync rule](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-sync-rule.png)

We’re using exclusion rules to keep our **groups** from being seen and sync’d as **users**:

![exclusion rules](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-exclusion-rules.png)

We’re using name filters in search rules for Groups. We’re using mail, displayName, and member attributes from AD to synchronize to GCID:

![search rules](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-search-rule.png)

![edit group search rule](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-edit-group-search-rule.png)

This configuration yields the basic results we need for this build. Our test user bcuser@silvertonhcl.com is synchronized to GCID, as are the groups we’re using. Our groups also contain our test user.

![our test user](/en-us/tech-zone/learn/media/citrix-google-zero-trust_appendix1-our-test-user.png)
