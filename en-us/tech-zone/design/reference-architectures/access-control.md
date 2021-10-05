---
layout: doc
h3InToc: true
contributedBy: Nagaraj Manoli
specialThanksTo: Praveen Raghuraman, Pradeep Vasu, Allen Furmanski, Jeff Wisgo
description: Gain knowledge about the Citrix Secure Private Access solution including key concepts, use cases, and strategies for implementing this comprehensive security solution for an organization's apps and data.
tz_title: Secure Private Access
tz_products: citrix-secure-private-access;
---
# Reference Architecture: Secure Private Access

## Audience

This document is for Citrix technical professionals, IT decision makers, partners, and security consultants who want to explore and adopt the Citrix Cloud service of Secure Private Access. The reader must have a basic understanding of Citrix products, security, and Citrix Cloud frameworks. For more information on Citrix Cloud and its services, see the official product documentation for [Citrix Cloud](/en-us/citrix-cloud.html).

## Objective of this document

This document provides a technical overview and architecture of Citrix Secure Private Access that provides conditional access to cloud apps and internet browsing, enhancing the organization's overall security and compliance posture. Citrix Secure Private Access combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators.

The document guides administrators to deliver a secure digital workspace with a consolidated solution using Citrix Secure Private Access integrated with multiple Citrix Cloud services, including Citrix Gateway service and Secure Browser service. To monitor user behavior and activity, Citrix Secure Private Access integrates with Citrix Analytics service.

## Security controls and mitigation

In today's business world, user data is one of the most treasured commodities, and organizations continue to find more value in it. Most of the breaches happening in organizations are because of inadequate security controls, excessive privileges granted to internal users, and over-reliance on network-based security controls. Many companies have unfortunately failed to truly adopt a secure in-depth approach to fortifying their environment.

Many IT departments within organizations do not have a solution for controlling access to users accessing an ever-increasing number of enterprise SaaS-based applications. In addition to the security risks posed by the potential for corporate data interacting with unmanaged SaaS apps, user productivity is lost due to logging into multiple apps. In addition, having to remember multiple passwords can often lead to bad password habits, such as the same password being used for multiple sites.

Browsing the internet poses another risk for enterprises today. According to security experts, most attacks exploit vulnerabilities in websites, browsers, and browser plug-ins. In response, some organizations even completely prohibit internet browsing, which can severely affect productivity.

Some web and SaaS apps require a certain browser version or plug-in to function properly. In the rapidly changing browser landscape, ensuring usability of corporate web and SaaS apps quickly amounts to a resource-intensive support task.

Many IT departments do not have a fully defined strategy for managing SaaS applications, resulting in significant security and compliance risks. Others have a fragmented approach that can result in multiple point solutions, such as:

*  **Deploying a single sign-on solution:** This solution helps to streamline the user experience and can include multifactor authentication, but these solutions do not typically have granular policy controls on apps once users sign in.

*  **Deploying web filtering to control or shut down external browsing:** This solution helps users to protect from visiting malicious websites. However, many features stop short of security policies for SaaS apps, leading many customers to deploy multiple solutions in addition to a web gateway.

*  **Publishing browsers for each SaaS application:** Publishing a web browser with Citrix Virtual Apps and Desktops is a time-tested method for controlling access to unsanctioned SaaS applications. However, it does require another IT resource to manage and is not the same user experience as directly connecting to a SaaS app via a browser.

*  **Relinquish management of SaaS applications to departmental control within the enterprise:** This approach opens up security and compliance gaps, and misses opportunities to understand user behavior and app usage, yet is often the default strategy for IT when faced with limited resources.

Citrix has come up with a better way to secure access to sanctioned SaaS and Web apps with a user-centric approach, as opposed to traditional ways that do not account for security. Citrix Workspace provides contextual and secure access of SaaS, web, and Virtual Desktops and Applications, reduces exposure to insider and outsider threats, secures content collaboration, and provides user behavior analytics and proactive security insights.

[![AC-Image-1](/en-us/tech-zone/design/media/reference-architectures_access-control_001.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_001.png)

The preceding diagram illustrates the traditional Citrix environment where endpoints are connecting through Citrix ADC to access their resources. These resources - including apps, desktops, and data - are being accessed from on-premises, and some of the external resources are accessed through the internet, including SaaS-based applications. Users access these external resources securely through Secure Gateway and web filtering services.

Workspace app is a single entry point to access resources from any device. It provides unique functionality to end users with enhanced security by employing an embedded browser engine. A networking engine within Workspace app optimizes network traffic and provides an SSL VPN in addition to Micro VPN capabilities for certain applications. End users are connected through Citrix ADC that provides Identity and Access Management capabilities.

Security monitoring provides end-user behavior monitoring and threat detection within the network. Data security includes DLP and IRM to keep control of data when users are working with or sharing files. Citrix Cloud web filtering, Citrix Analytics, and Citrix Gateway services offer a consolidated solution to deliver a secure digital workspace to end users.

## Citrix Secure Private Access

Citrix **Secure Private Access** combines the capabilities of instant secure access to SaaS and web applications through single sign-on (SSO), along with browser and cloud-based app controls, web-filtering policies, and integrated user behavior analytics. Citrix Secure Private Access goes beyond traditional SSO capabilities by introducing **cloud app control** – a set of enhanced security controls for SaaS and enterprise web apps providing conditional access to cloud apps and protecting user-actions based on admin-governed access policies. This solution enhances the organization's overall security and compliance posture. The user's experience remains seamless and integrated because the SaaS and web apps can be accessed alongside their mobile apps, virtual apps, and desktops as an integrated part of Citrix Workspace.

Citrix Secure Private Access combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators.

| **Functionality** | **Service/Component providing the functionality** |
| --- | --- |
| Consistent UI to access apps | Workspace Experience/WS App |
| SSO to SaaS and Web apps | Citrix Gateway service standard |
| Web filtering and categorization | Web filtering service |
| Enhanced security policies for SaaS | Cloud app control |
| Secure browsing | Secure Browser service |
| Visibility into Website access & risky behavior | Citrix Analytics |

## Why Citrix Secure Private Access?

Data security is delineated by the need to ensure data integrity, confidentiality, and protect the company's intellectual property. Today organizations face various challenges such as:

*  Multiple point products that are hard to manage and have different policy infrastructures
*  Providing secure remote access to end users to access company information
*  Protecting intellectual property stored in cloud and SaaS apps while staying compliant
*  Gaining visibility into cloud and SaaS apps after SSO and obtaining risk assessments

Citrix Secure Private Access helps IT and security admins govern authorized end-user access to sanctioned SaaS and enterprise-hosted web apps. User identities and attributes are used to determine access privileges and Secure Private Access policies determine the privileges that are required to perform operations.

[![AC-Image-2](/en-us/tech-zone/design/media/reference-architectures_access-control_002.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_002.png)

The integration of Citrix Secure Private Access results in several benefits:

*  **Seamless user experience with Single-sign on to SaaS and hosted applications:** This capability comes through a combination of the Workspace app and Gateway service

*  **Greater control for IT with organized delivery and access for SaaS applications:** Provides IT with a way to easily assign SaaS applications to the members of their workforce

*  **Enhanced security with policy controls for SaaS usage:** Referred to as cloud app control, a new capability that provides IT with a way to enforce security policies on the SaaS applications that they provide to employees

*  **Flexibility to enable controlled access to public internet content or unknown SaaS apps without sacrificing security:** This capability comes through a combination of Secure Browser service and web filtering functionality so that websites can be whitelisted/blacklisted or rendered in an isolated browser

*  **Visibility into SaaS usage and user web activity:** This capability provides a subset of Citrix Analytics for SaaS and web activity to provide more visibility for security and compliance

## Citrix Secure Private Access and Citrix Cloud

Citrix Secure Private Access is one of the services offered by Citrix Cloud. To access these services an administrator must have a Citrix account (also known as a Citrix.com or My Citrix account) to manage licenses and access the environment.

A Citrix account uses an organization ID (OrgID) as a unique identifier. The administrator can access their Citrix account by logging in at [citrix.com](https://www.citrix.com) with the user name or email address linked to the account. Initially, Citrix Cloud redirects to [https://citrix.cloud.com](https://citrix.cloud.com) to create an account or sign in with an existing Citrix account to activate a Cloud services trial.

A Citrix Cloud account allows admins to have broad administrative access on the services, therefore Citrix requires that the first admin who creates the Citrix Cloud account to explicitly give access to other admins as needed, even if the other admin is already a member of the existing MyCitrix account.

Citrix Secure Private Access is solution shown as a tile on the Citrix Cloud console, and includes integration across Citrix Gateway service, Web Filtering service, Secure Browser service, and Citrix Analytics service.

[![AC-Image-3](/en-us/tech-zone/design/media/reference-architectures_access-control_003.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_003.png)

**Secure Private Access:** The Citrix Secure Private Access service provides a unified experience integrating single sign-on, remote access, and content inspection into a single solution for end-to-end Secure Private Access.

Citrix Secure Private Access provides the following capabilities to administrators:

*  Configure multifactor authentication for end users
*  Configure a workspace to securely deliver access to apps from any device, manage, and add SaaS applications from the library
*  Configure web filtering to allow/block websites to end-users, and redirect them to Citrix Secure Browser service

**Analytics:** Citrix Analytics collects data across the Citrix portfolio of products and generates actionable insights, enabling administrators to proactively handle user and application security threats, improve app performance, and support continuous operations. Citrix Analytics gathers data and provides the following insights:

*  Security Analytics
*  Performance Analytics
*  Operations Analytics

**Gateway:** The Citrix Gateway service provides a secure remote access solution, a unified user experience for configured SaaS apps, heterogeneous virtual apps, and desktops. SaaS app delivery using Citrix Gateway service provides an easy, secure, robust, and scalable solution to manage the apps. SaaS apps delivered on the cloud have the following benefits:

*  Simple configuration: Easy to operate, update, and consume
*  Single sign-on: Hassle free logon with SSO
*  Standard template for different apps: Template-based configuration of popular apps

**Secure Browser:** Citrix Secure Browser service isolates web browsing to protect the corporate network from browser-based attacks. It delivers consistent, secure remote access to an internet-hosted web application with no need for user device configuration.

Administrators can rapidly roll out secure browsers, providing instant time-to-value. By isolating internet browsing, IT administrators can offer end-users safe internet access without compromising enterprise security.

## Identity and access Management

Identity and Access Management defines the identity providers and accounts used for administrators of and subscribers to Citrix Cloud and its offerings. Citrix Cloud uses the Citrix Identity provider to manage the identity information for users in the Citrix Cloud account. The administrator has the option to integrate Azure Active Directory or an on-premises Active Directory service.

To perform management activities and install the Citrix Cloud Connector on Citrix Cloud, Citrix administrators use their identity to access Citrix Cloud. This identity mechanism provides authentication for administrators using an email address and password. Also, administrators can use My Citrix credentials to sign in to Citrix Cloud.

[![AC-Image-4](/en-us/tech-zone/design/media/reference-architectures_access-control_004.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_004.png)

A subscriber's identity defines the services to which that subscriber has access to on Citrix Cloud. Identity comes from the Active Directory domain accounts provided from the domains within the resource location. Subscribers can authorize to access the offering from Citrix Cloud by assigning a subscriber to a library offering.

On-premises Active Directory communication with Citrix Cloud takes place through highly available Citrix Cloud Connectors. Organizations opting to use Azure Active Directory service have more flexibility in terms of managing the user accounts, audit control, and password policies. Also, the administrator can configure multifactor authentication for a higher level of security.

## Citrix Secure Private Access with enhanced security for SaaS apps

Citrix Cloud offers the single sign-on (SSO) capability to SaaS applications through the Citrix Gateway service. SSO delivers a unified user experience for web-based SaaS application SSO and publishing to the Citrix Workspace experience user interface. To enable a single sign-on user experience, a SaaS app trusts the SAML assertion provided by the Citrix Gateway service.

The process of enabling single sign-on to SaaS applications is now simplified to just a few web forms and clicking the configuration page. Citrix Gateway Connector provides a proxy to internal web-based SaaS application access for the Workspace app user.

Reference: [Citrix Gateway Connector](/en-us/citrix-gateway-service/gateway-connector.html)

### Content control

Protecting user data (SaaS app users) is a challenging task for most organizations. By using Citrix Secure Private Access, organizations can incorporate enhanced security policies within SaaS applications. Each policy enforces a restriction on the embedded browser when using the Workspace app for desktop, or on Secure Browser when using Workspace app for web or mobile.

*  **Preferred browser:** Disables local browser use and relies on the embedded browser engine (Workspace app - desktop) or Secure Browser service (Workspace app – mobile and web)
*  **Restrict clipboard access:** Disables cut/copy/paste operations between the app and endpoint clipboard
*  **Restrict printing:** Disables ability to print from within the app browser
*  **Restrict navigation:** Disables the next/back browser buttons
*  **Restrict downloads:** Disables the user's ability to download from within the SaaS app
*  **Display watermark:** Overlays a screen-based watermark showing the user name and IP address of the endpoint. If a user tries to print or take a screenshot, the watermark appears as displayed on the screen

## Citrix Secure Private Access single sign-on without enhanced security

[![AC-Image-5](/en-us/tech-zone/design/media/reference-architectures_access-control_005.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_005.png)

| **SL NO** | **Workspace app with sanctioned SaaS App** | **Local browser with sanctioned SaaS App** |
| --- | --- | --- |
| **1** | The user launches the Workspace app on the endpoint. | The user launches a web browser on the endpoint, connects to Workspace, and authenticates using on-premises Active Directory, or Azure Active Directory. |
| **2** | Workspace app connects to the workspace and authenticates using the on-premises Active Directory / Azure Active Directory. |  The local browser populates with approved resources. |
| **3** | Workspace app populates with approved resources. | When the user selects a SaaS app, the local browser sends the request to Workspace, which requests a one-time-use URL and forwards browser to the Gateway service. |
| **4** | Workspace app sends the request to Workspace, which requests a one-time-use URL from the Gateway service and a preferred browser. | The local browser initiates a connection to the Gateway service. |
| **5** | The local browser initiates a connection to the Gateway service. | The Gateway service requests an assertion from the single sign-on microservice. |
| **6** | The Gateway service requests an assertion from the single sign-on microservice. | The local browser is redirected the SaaS app login page where the assertion is presented. |
| **7** | The local browser is redirected to the SaaS app login page where the assertion is presented. | The SaaS app contacts the Gateway service to validate the assertion and authenticates the user. |
| **8** | The SaaS app contacts the Gateway service to validate the assertion and authenticates the user. | Once authenticated, communication occurs directly between the browser and the SaaS application. |
| **9** | Once authenticated, communication occurs directly between the browser and the SaaS application. | |

## Secure Private Access-single sign on with enhanced security

[![AC-Image-6](/en-us/tech-zone/design/media/reference-architectures_access-control_006.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_006.png)

| **SL NO** | **Workspace app with sanctioned SaaS app** | **Local browser with sanctioned SaaS App** |
| --- | ---| ---|
| **1** | The user launches the Workspace app on the endpoint. | The user launches a web browser on the endpoint, connects to Workspace, and authenticates using on-premises Active Directory or Azure Active Directory. |
| **2** | Workspace app connects to the workspace and authenticates using the on-premises Active Directory / Azure Active Directory. | Local browser populates with approved resources. |
| **3** | Workspace app populates with approved resources. | When the user selects a SaaS app, the local browser sends the request to Workspace, which requests a one-time-use URL and redirects browser to the Secure Browser service. |
| **4** | Workspace app sends the request to Workspace, which requests a one-time-use URL from the Gateway service. | The local browser initiates a Secure Browser connection. |
| **5** | The embedded browser initiates a connection to the Gateway service. | The Secure Browser initiates a connection to the Gateway service. |
| **6** | The Gateway service requests an assertion from the single sign-on microservice and enhanced security policies from the Secure Private Access service. | The Gateway service requests an assertion from the SSO microservice and enhanced security policies from the Secure Private Access service. |
| **7** | The embedded browser is redirected to the SaaS app login page where the assertion is presented. | Secure Browser is redirected to the SaaS app login page where the assertion is presented. |
| **8** | The SaaS app contacts the Gateway service to validate the assertion and authenticates the user. | The SaaS app contacts the Gateway service to validate the assertion and authenticates the user. |
| **9** | Once authenticated, communication occurs directly between the browser and SaaS application. | Once authenticated, communication occurs directly between the browser and SaaS application. |

Reference: [Citrix Secure Private Access tech brief](/en-us/tech-zone/learn/tech-briefs/access-control.html)

## Contextual access

Most SaaS apps are safe to use, although sometimes when a user clicks a hyperlink within a SaaS app, it can possess a security risk for an organization. The web filtering microservice allows, denies, or redirects the hyperlink request from the user.

[![AC-Image-7](/en-us/tech-zone/design/media/reference-architectures_access-control_007.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_007.png)

| **SL NO** | **Workspace App with Enhanced Security** | **Local Browser with Enhanced Security** |
| --- | --- | --- |
| **1** | The user selects a hyperlink from within the SaaS app. | The user selects a hyperlink from within the SaaS app. |
| **2** | The embedded browser within Workspace app sends the URL to the Secure Private Access service. | Secure Browser sends the URL to the Secure Private Access service. |
| **3** | The Secure Private Access service requests an analysis of the URL by the Web Filtering microservice. | The Secure Private Access service requests an analysis of the URL by the Web Filtering microservice. |
| **4** | For blocked links, the Web Filtering microservice denies access to the hyperlink. | For blocked links, the Web Filtering microservice denies access to the hyperlink. |
| **5** | For approved links, the Web Filtering microservice allows the user to access the link with the embedded browser. | For approved links, the Web Filtering microservice allows the user to access the link as a new tab within their current Secure Browser service session. |
| **6** | For redirected links, the Web Filtering microservice has a Workspace app send the link to Secure Browser service, which starts a new, virtual browser session for the end user. | For redirected links, the Web Filtering microservice allows the user to access the link as a new tab within their current Secure Browser service session. |

## Web filtering overview

Web filtering provides policy-based control of websites by using the information contained in URLs. This feature helps network administrators monitor and control user access to malicious websites on the network.

This service release enables web filtering for Citrix Workspace app accessing SaaS apps and the internet, and Citrix Secure Browser accessing SaaS apps and the internet.

[![AC-Image-8](/en-us/tech-zone/design/media/reference-architectures_access-control_008.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_008.png)

The Citrix Secure Private Access administrator can block and allow a list of URLs. The web filtering controller uses a categorization database and URL list. When the request comes to the web filtering controller, first it checks the global allow list which also contains critical Citrix Cloud URLs. Then it comes to Lists and Categorization and checks for blocked, allowed, and redirect to Secure Browser URLs. If none of the URLs match, then by default it falls back to the default list.

## Citrix Secure Private Access and Citrix Analytics

The Citrix Analytics service is a cloud-based service which facilitates pragmatic insights by collecting data across the Citrix portfolio of products. Citrix Secure Private Access organizes and produces information on user activities, such as websites visited and the bandwidth spent. Citrix Secure Private Access also monitors malware and phishing sites by looking into bandwidth consumption and reports on this. The administrator can take corrective actions by leveraging these key metrics to monitor the network.

Citrix Analytics easily integrates with Citrix Secure Private Access, Citrix Gateway service, and other Citrix portfolio products. Citrix Analytics provides comprehensive insights into user behavior. It uses machine learning algorithms to detect anomalous user behavior, troubleshoot user sessions, and view operational metrics for users in an organization that uses Citrix products.

Citrix services and products send data to Citrix Analytics, which are referred to as data sources. The data sources associated with the Citrix Cloud account are discovered by the Citrix Analytics service. Citrix Cloud logs are transmitted securely to Citrix Analytics. The logs are collected from Citrix Secure Private Access and maintained separately from the data sources.

[![AC-Image-9](/en-us/tech-zone/design/media/reference-architectures_access-control_009.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_009.png)

The Citrix Secure Private Access service delivers security and operations dashboards. The security dashboard provides user risk profiles and summary of access activity by the users, such as URL or domain visited and the bandwidth used. App access summarizes the details of domains, URLs, and apps accessed by the users.

Reference: [Citrix Secure Private Access and Analytics](/en-us/citrix-secure-workspace-access/monitor-user-activity-and-manage-settings-with-analytics.html)

[![AC-Image-10](/en-us/tech-zone/design/media/reference-architectures_access-control_010.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_010.png)

Citrix administrators can create rules on Citrix Analytics to perform actions on user accounts when unusual or suspicious activities occur. A rule is a set of conditions that must be met for an action to be run. A rule can contain a single condition and one or more actions. Conditions such as "risk score" and "risk score change" are global conditions. Global conditions can be applied to a specific user for a specific data source.

The administrator creates a rule based on the user's activity by applying conditions which are listed as follows:

*  **Attempt to access blacklisted URL** Indicates an attempt to access a blacklisted URL.
*  **Risky website access** Indicates that the user attempted to access a malicious, suspicious, or risky websites.
*  **Unusual download volume** Indicates that the volume of data downloaded by the user from applications or websites has exceeded the threshold defined implicitly by Citrix Analytics.
*  **Unusual upload volume** Indicates that the volume of data uploaded by the user from applications or websites has exceeded the threshold defined implicitly by Citrix Analytics.

Reference: [Secure Private Access and Analytics](/en-us/citrix-secure-workspace-access/monitor-user-activity-and-manage-settings-with-analytics.html)

## Citrix Secure Private Access end-user experience

The Citrix administrator has power to extend security control with the help of Citrix Secure Private Access. Citrix Workspace app is an entry point to access all resources securely, end users can access virtual apps, desktops, SaaS apps, and files through Citrix Workspace app. With Citrix Secure Private Access, administrators are able to control how a SaaS Application can be accessed by the end user via Citrix Workspace Experience web UI or native Citrix Workspace app client.

For more information refer to the Citrix Workspace app [Reference Architecture](/en-us/tech-zone/design/reference-architectures/workspace-app.html).

### End-user experience with Workspace App and web portal

[![AC-Image-11](/en-us/tech-zone/design/media/reference-architectures_access-control_011.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_011.png)

[![AC-Image-12](/en-us/tech-zone/design/media/reference-architectures_access-control_012.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_012.png)

When the user launches the Workspace app on the endpoint, they see their applications, desktops, files, and SaaS apps. If a user clicks the SaaS application when enhanced security is turned off, the application opens in a standard browser which is locally installed. If the administrator has turned on enhanced security then SaaS apps open on the embedded browser within Workspace app. Accessibility to hyperlinks within SaaS apps and web apps is controlled based on web filtering policies.

Similarly, with Workspace Web portal, when enhanced security it turned off SaaS applications are opened through a standard browser which is natively installed. When enhanced security is turned on, SaaS apps are opened through the Secure Browser. Users are able to access the websites within SaaS apps based on web filtering policies.

## Implementing Citrix Secure Private Access

The Citrix Secure Private Access service is a cloud-based service. To provide user-centric solutions to organizations and comply with policies, Citrix Secure Private Access plays a vital role. Along with Citrix Secure Private Access, there are other services that are enabled to provide consolidated solutions to end users. To get started with onboarding and setting up the Citrix Secure Private Access service the administrator must set up authentication, configure access to SaaS apps, and specify the content access settings in the Citrix Secure Private Access service. The end users can access the service from the Citrix Workspace app or the Workspace URL.

[![AC-Image-13](/en-us/tech-zone/design/media/reference-architectures_access-control_013.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_013.png)

The Citrix administrator logs in to Citrix Cloud using their credentials, and requests the Citrix Secure Private Access service. Citrix Secure Private Access is integrated with other Citrix portfolio products including Citrix Gateway, Secure Browser, and Citrix Analytics. This solution offers security for SaaS apps.

[![AC-Image-14](/en-us/tech-zone/design/media/reference-architectures_access-control_014.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_014.png)

**Step 1:** The Citrix administrator configures Identity and Access Management on Citrix Cloud. By default, Citrix Cloud uses the Citrix Identity provider to manage identity information for all users in the Citrix Cloud account. The administrator has flexibility to change and use Azure Active Directory or an on-premises Active Directory service.

**Step 2:** The Citrix administrator logs in to Citrix Gateway service to configure single sign-on for SaaS and web applications. The Citrix Gateway service offers a secure login to the company's environment where end-users sign in to applications using SAML single sign-on.

Add a web or SaaS app to the library from the template. To learn more about a list of SaaS applications supported by Citrix Secure Private Access, see [SaaS applications supported by the Citrix Secure Private Access service](/en-us/citrix-cloud/access-control/saas-apps-supported-by-acs.html)

**Step 3:** The administrator enters the app details like the name of the application, URL, and domain details. Enhanced security policies can be enabled to prevent data leaks. These policies within the SaaS applications enforce restrictions on the embedded browser when using a Workspace app for desktop or on Secure Browser when using Workspace app web or mobile.

[![AC-Image-15](/en-us/tech-zone/design/media/reference-architectures_access-control_015.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_015.png)

**Step 4:** **Enable Single sign-on for web/SaaS apps:** Citrix Gateway service (cloud services only) provides single sign-on for SaaS applications. Enabling single sign-on for SaaS applications is now simplified to just a few web forms and clicking the configuration page, which greatly simplifies the deployment process. It is possible for administrators to apply the Gateway Connector to proxy internal web-based SaaS application access to the external Workspace app user.

Under the **Library** section of Citrix Cloud, SaaS and web apps are published. The administrator has to add users after choosing the domain, and only subscribed users can access the application through Workspace app or Workspace web.

**Step 5:** **Filter website lists:** To protect the corporate network from browser-based attacks, Citrix Secure Private Access includes a web filtering service. Based on the policies, the web filtering service allows, denies, or redirects the hyperlink request from the user as defined:

**Allowed:** The request link is considered safe and access is permitted within the embedded browser of the Workspace app.

**Blocked:** The hyperlink is considered dangerous and access is denied.

**Redirected:** The administrator takes a precaution on websites which possess security threats by redirecting them through the Secure Browser service.

**Step 6:** Filter website Categories. The categorization database helps to filter web traffic controlling end-user access to specific websites, such as social networking, gambling, adult content, new media, and shopping. Categories restrict user access to specific websites and website categories.

| **Categorization presets provide for convenient out-of-the-box templates** |  |
| --- | --- |
| **Strict** | Minimizes the risk of accessing unsecured or malicious websites. End users can still access websites with low risk. Includes most business travel and social media websites. (133/192 categories) |
| **Moderate** | Minimizes risk while allowing other categories with a low probability of exposure from unsecured or malicious sites. Includes most business travel, leisure, and social media websites. (65/192 categories) |
| **Lenient** | Maximizes access while still controlling risk from illegal and malicious websites. (36/192 categories) |
| **None** | Allows all categories. |
| **Custom** | Configure custom filtering of categories. |

Reference: [Citrix Secure Private Access categories-list](/en-us/citrix-cloud/access-control/categories-list.html)

Citrix Workspace app gives users a great experience (secure, contextual, unified workspace) on any device. Users get seamless and secure access through single sign-on to all the apps they need to be productive.

To validate the configuration, end users can launch the SaaS app from Citrix Workspace app (or Citrix Workspace Web). Also, users can verify allowed/blocked websites added in the website filtering lists by visiting the URLs. The Citrix Workspace app's embedded browser allows administrators to provide native access to SaaS applications with enhanced security controls.

To learn more about Citrix Workspace app refer to the [Citrix Workspace app Reference Architecture](/en-us/tech-zone/design/reference-architectures/workspace-app.html).

**Step 7:** Citrix Secure Private Access service integrates with Citrix Analytics to fetch information on the activities of users, such as websites visited and bandwidth consumed. Citrix Analytics reports threats detected such as malware and phishing sites.

The administrator can log in to the Citrix Analytics cloud service and create rules for Citrix Secure Private Access and then apply the action plan when the conditions are met. To monitor and analyze user behavior activities, enable "Turn on Data processing" for Citrix Secure Private Access for analytics on Citrix Cloud (Data sources are Citrix services and products that send data to Citrix Analytics).

[![AC-Image-16](/en-us/tech-zone/design/media/reference-architectures_access-control_016.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_016.png)

The Citrix Analytics service monitors user activities and behavior using machine learning and artificial intelligence. Data sources for Citrix Analytics are gathered from Workspace app, Citrix Gateway, Citrix Secure Private Access, and Secure Browser service.

## Citrix Secure Private Access use cases

Today organizations are turning to Software as a Service (SaaS) solutions to address business requirements, yet often without taking the necessary security measures or properly maintaining the applications. Most SaaS application security failures are caused by users, not cloud providers. The organization must regulate its strategies to address these flaws.

Citrix Secure Private Access enforces enhanced security policies for SaaS apps (watermark, copy-paste restriction, prevent downloading/uploading sensitive data, and so on). It also defines access policies for website categories and websites to be blocked / allowed in the form of web filtering.

| Existing brownfield deployments | How Citrix Secure Private Access fits in |
| --- | --- |
| Organizations looking to adopt Citrix Cloud services. | 1) Citrix Secure Private Access provides the right fit into their environment, providing security for SaaS, SSO, and web filtering features to end users. 2) To organizations desiring an on-premises model Citrix Gateway helps in providing SSO and enhanced security to end users. |
| Organizations that have already adopted the Citrix Gateway service. | 1) Citrix Cloud offers several cloud-based services including Citrix Secure Private Access, Analytics, and Secure Browser service that provide security control for SaaS and internet-based apps. |
| Organizations that have already adopted third-party SSO | 1) Citrix Secure Private Access provides granular level security controls for SaaS apps, minimizes web-based threats and automatic policy enforcement through analytics based on user behavior. 2) ICA Proxy is supported for both on-premises and Citrix Virtual Apps and Desktops service. |

## Citrix Secure Private Access solution for enterprise web apps

Most of on-premises customers still use web apps, including SharePoint, Confluence, Microsoft Office, help desk applications, and so on Enterprise applications are delivered remotely using Citrix Gateway service and necessary security is added using Citrix Secure Private Access.

End-users access web apps using Citrix Workspace that leverages Citrix Gateway service. Citrix Gateway service, securely coupled with Citrix Workspace, delivers a unified user experience for configured web apps. Single sign-on and remote accessibility to internal web apps is available through different service packages.

[![AC-Image-20](/en-us/tech-zone/design/media/reference-architectures_access-control_020.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_020.png)

The preceding diagram shows the Citrix Secure Private Access solution applied to the on-premises customers using the Citrix Gateway Connector. Citrix Gateway connector acts as a bridge between Enterprise web apps and the Citrix Workspace service.

### Web app single-sign on with enhanced security

The user launches the Citrix Workspace app and connects with Citrix Workspace using an on-premises Active Directory service. The Citrix Workspace app is used to start a web app. Citrix Gateway service provides the recommended browser and the link. The embedded browser from the Citrix Workspace app makes an application connection with Citrix Gateway service. Also, enhanced security policies are enabled through Citrix Secure Private Access service. Furthermore, Citrix Gateway service establishes an outbound connection with Gateway Connector from the resource location. It verifies the login credentials, and Citrix Workspace app secures an end-to-end connection with the internal web app.

If the user is using a local browser, authentication happens through the Active Directory service, and the local browser makes a secure connection with Secure Browser service. Enhanced security policies are enabled through the Secure Private Access service. A secure outbound channel is then established between Gateway Connector and Citrix Gateway service. Next, user credentials are negotiated and single sign-on is established on behalf of the user. Finally, an end-to-end connection is found via the Secure browser for the user.

Reference: [Support for Enterprise web apps](/en-us/citrix-gateway-service/support-web-apps.html)

## Citrix Secure Private Access single sign-on for web apps

Citrix Gateway Connector and the Citrix Gateway service in the cloud secure the communication with on-premises applications. Web applications are accessed and delivered through Workspace using a VPN-less connection. The administrator must choose the single sign-on method during the web app's configuration.

These four types of SSO can be configured through Citrix Gateway service:

*  Form-based
*  Basic SSO
*  Kerberos
*  SAML (TP)

[![AC-Image-21](/en-us/tech-zone/design/media/reference-architectures_access-control_021.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_021.png)

Form-based Authentication

1)  Citrix Gateway service establishes a secure channel between Citrix Gateway Connector and sends the web app request with login credentials

2)  Citrix Gateway Connector submits login credentials to the respective web application

3)  An end-to-end secure connection is established between the web app and Citrix Workspace app via Citrix Gateway service using single sign-on to the web app

Basic SSO Authentication

1)  Citrix Gateway service creates a secure channel between Citrix Gateway Connector and sends the web app request with user name and password

2)  Citrix Gateway Connector submits login credentials to web application's login page

3)  The web app requests authentication using the NTLM protocol

4)  Citrix Gateway Connector responds to the NTLM request with user name and password

5)  An end-to-end secure connection is established between the web app and Citrix Workspace app via Citrix Gateway service using single sign-on to the web app

Kerberos Authentication

1)  Citrix Gateway service creates a secure channel between Citrix Gateway Connector and sends web app request with user name and password

2)  Citrix Gateway Connector submits login credentials to the web application's login page

3)  The web app requests authentication using Kerberos protocol

4)  Citrix Gateway Connector contacts the domain controller to verify the login credentials

5)  the Domain Controller validates the user credentials and acknowledges

6)  Citrix Gateway Connector forward the application back to the web app

7)  An end-to-end secure connection is established between the Citrix web app and Citrix Workspace app via Citrix Gateway service using single sign-on to the web app

To enable Kerberos single sign-on functionality, administrators configure Gateway Connector with credentials for a service account trusted to perform Kerberos Constrained Delegation.

Users may try to access malicious websites that cause severe damage to the enterprise. Also, they may violate enterprise regulations and policies. To overcome these problems, administrators can adopt Citrix Secure Private Access to filter risky websites that pose a risk to their organization. A watermark can also be added throughout the session that includes the user's name and IP address.

Reference: [Support for Enterprise web apps](/en-us/citrix-gateway-service/support-web-apps.html)

## Citrix Secure Private Access app protection policies

It is common for a user's login credentials to be stolen, and yet the user may not be aware of this. Cybercriminals use various techniques to apprehend end-user data, and one common technique is using keylogger malware to capture user data. These malware products can be easily installed on a user machine and immediately start trying to obtain user information. Leakage of user information can lead to significant damage to the organization and to the user. To overcome this problem, the organization must invest heavily in protecting user data and create a defensive shield against keyloggers.

Similar to keyloggers are applications that capture screenshots. These malicious programs operate by making screenshots of the user's desktop to capture information shown on the screen.

Various types of software can be installed on the user end point to overcome image grabbing of the user desktop. But this can lead to slower performance of user's desktop and environment.

Citrix Secure Private Access has advanced policies for protecting enterprise data. Endpoint security is an important security consideration for any organization because the majority of breaches occur at the user endpoint. App protection policies are rules that are applied when enabling enhanced security for a SaaS app. Customers can use two advanced security policies:

*  Anti-keylogging

*  Anti-screen capture

[![AC-Image-22](/en-us/tech-zone/design/media/reference-architectures_access-control_022.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_022.png)

Citrix App protection policies are enabled via the Citrix Secure Private Access solution. The benefits are as follows:

*  Protecting against keylogging and screen capture
*  Centralized management for Citrix administrators
*  Agnostic to device security posture

App protection policies are built in Citrix Workspace app beginning in version 1912 for Windows, but the administrator must enable this feature.

Reference: [App Protection policies](https://www.citrix.com/en-in/downloads/citrix-virtual-apps-and-desktops/components/app-protection-policies.html)

## Protect Users from risky internet browsing using Citrix Secure Browser

Web browsers are integral to an active production environment. They are powerful, data-rich tools exposed to the internet more than any other application in the work environment. During the past few years, cybercriminals have taken advantage of web browsers to fetch vast amounts of user information including credit card data, email IDs, and stored passwords.

Browser-based attacks have become prevalent not because they are strategically desirable hacking targets, but because browser-based attacks are difficult to detect. Conventional security controls fail to detect such attacks because these applications only scrutinize downloaded files and attachments. Hence, browser-based attacks tend to go unnoticed.

The Citrix Secure Browser service isolates web browsing to protect the customer production environment from browser-based attacks. Citrix Workspace app or local browsers are an entry point to the Citrix production environment. Citrix Secure Browser isolates internet browsing so that the website does not directly transfer any browsing data to or from the user device. By using this, security administrators can offer safe internet access without reducing enterprise security.

Citrix Secure Browser service is a SaaS product managed and operated by Citrix. It allows access to web applications via an intermediate web browser hosted in the cloud. When using Citrix Secure service, hosted web browsers track a user's browsing history and perform caching of HTTP/HTTPS requests. Citrix uses mandatory profiles and ensures that once the browsing session ends, that session's data is erased.

Secure Browser service can be accessed with an HTML5-compatible web browser. There is no client that users must download. All traffic between the end-user browser and Citrix Cloud service is encrypted using industry-standard TLS encryption and only TLS 1.2 is supported.

[![AC-Image-23](/en-us/tech-zone/design/media/reference-architectures_access-control_023.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_023.png)

The preceding diagram shows the integration of Citrix Secure Private Access solution, including Citrix Secure Browser service for both cloud and on-premises Citrix environments. Citrix Virtual Apps and Desktops customers with an on-premises StoreFront can easily integrate with the Secure Browser service.

To learn more about configuring Citrix StoreFront with the Secure Browser service, follow this [link](https://support.citrix.com/article/CTX230272?_ga=2.235378497.1770257565.1575439566-348774687.1571729438&_gac=1.241968438.1575297468.EAIaIQobChMI_eLFrJiX5gIV1AorCh1E7ggkEAAYASAAEgLlCPD_BwE).

## Citrix Secure Private Access and content collaboration

Most organizations have experienced some kind of ransomware or phishing attempts that have compromised their network. The root cause for such threats is often inadequate protection from web-based threats. There is a lack of visibility into what websites users are accessing on a day-to-day basis.

The Citrix Secure Private Access service enables an organization to protect its environment from browser-based attacks and data leaks. When employees access their apps from any device, whether they are in the office, home or traveling, Citrix Secure Private Access service provides a cohesive experience integrating SSO, two-factor authentication, remote access, and web filtering into a single solution for end-to-end Secure Private Access.

[![AC-Image-17](/en-us/tech-zone/design/media/reference-architectures_access-control_017.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_017.png)

Citrix Content Collaboration enables users to easily and securely exchange documents. There are many ways to work using Citrix Content Collaboration including a web-based interface, mobile clients, desktop apps, and integration with Microsoft Outlook and Gmail.

Citrix Files is a file manager that offers data sharing and storage, customizable usage and settings, and tools that allow users to collaborate more easily and get work done from any device, anytime and anywhere.

The preceding diagram depicts delivering the Citrix Files SaaS app to an end user in a hybrid cloud model scenario. The Citrix Cloud Connector provides a link to the Citrix Cloud account and resource location. The resource location contains the Active Directory for end users, allowing them to seamlessly sign on to their web application.

The Citrix Gateway service offers authentication, single sign-on, and enables fast and secure delivery of Citrix Virtual applications and SaaS applications. End users log in to the company's environment using their login credentials and can log in to web applications utilizing SAML single sign-on. On the Citrix Gateway service, administrators can choose the template of Web/SaaS app or define their own application parameters. For example:

**Name the application:** "Citrix Files"

**Enter the URL:** `https://xxxxx.sharefile.com/`

Similarly, on the SSO page administrators can ensure SAML is selected, assuming the SAML or SSO setting for Citrix Files has already been completed for the back-end.

**The Assertion URL is:** `https://xxxxx.sharefile.com/saml/xxxx`

**Audience:** `https://xxxxx.sharefile.com`

**Name ID format:** Email address

**Name ID:** Email

Once the SaaS app is added to the Citrix Cloud library, the administrator has to manage subscribers and provide the Workspace URL to users to access. With Active Directory, it can serve as an identity provider to allow SAML single sign-on to various web and SaaS applications.

### Content control and contextual access

On the Enhanced Security page, the administrator can set policies to protect the organization from sensitive data leaks. It provides the following options:

*  restricted clipboard access
*  restricted printing
*  restricted navigation
*  restricted downloads
*  displaying of watermarks

To block unwanted websites that possess security risks to organizations, the Citrix administrator has to block and allow URLs by adding URL lists or choosing category lists. The administrator can also take a cautious approach by redirecting URLs through a secure browser.

### User behavior and activities

To monitor user behavior and activities by an administrator, Citrix Secure Private Access service is easily integrated with the Citrix Analytics service. The administrator imposes predefined conditions and an action plan to mitigate the risks.

## Citrix Secure Private Access and G Suite

G Suite (also known as Google Workspace) is a set of SaaS applications that are accessible remotely over the internet. G Suite was formerly known as Google Apps developed by Google. G Suite is comprised of Gmail, Hangouts, and Calendar for communication; Google Drive for storage; Google Docs, Sheets, Slides, Forms, and Sites for collaboration. G Suite provides tremendous productivity advantages, but most networks are not designed properly to deliver the performance and security needed for mobility and cloud use cases.

Google Cloud and Citrix enable end users to expedite pioneering solutions delivering enhanced security with great user experience and much-desired flexibility in an app-centric, mobile-first, and hybrid cloud world.

G Suite, a set of market-leading productivity SaaS apps, can be integrated with Citrix Secure Private Access to allow the organization to get increased visibility and control of SaaS applications, which in turn prevents data leaks and unauthorized disclosure of sensitive information.

End users can access G Suite applications by simply entering the URL and login credentials through the Citrix Workspace app. Users will have an entire workspace including apps, desktops, and files, keeping in mind that users never have to enter another user name and password. The entire workspace is delivered through a single access point that improves productivity and streamlines common workflows for the end user.

[![AC-Image-18](/en-us/tech-zone/design/media/reference-architectures_access-control_018.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_018.png)

Citrix Secure Private Access not only provides a single sign-on feature, but also provides a layer of security controls which are not available in other solutions.

### Deployment

Citrix Cloud Connectors are used to handle all communications between resource locations and Citrix Cloud. For high availability, a minimum of two connectors are deployed at each resource location. The resource locations integrate with Active Directory, allowing end users to seamlessly sign on to their G Suite applications.

Citrix Gateway service with Active Directory can serve as an Identity provider to allow SAML single sign-on to G Suite SaaS applications. Citrix Secure Private Access allows end users to launch G Suite SaaS applications in Secure Browser sessions and also allows the administrator to apply five different control policies.

**Scenario 1:** Enhanced security is turned off. When a user launches Gmail within G Suite it opens in a standard browser. Similarly, URLs opened within their Gmail account use a standard browser without any additional security policies or controls. A user has a freedom to navigate from the page, cut, copy, and print the page.

**Scenario 2:** Enhanced security is turned on. When a user launches Gmail within the G Suite it opens in a Secure Browser. Now a layer of control capability is applied through Citrix Secure Private Access and the end user does not get a navigation bar to navigate away from the site. Furthermore cut, copy, and paste restrictions are imposed. Apart from this, Citrix Secure Private Access provides URL filtering capabilities that prevent users from visiting malicious websites. Alternately, users can be redirected to a Secure Browser based on the policies.

Benefits of using Citrix Secure Private Access with G Suite SaaS apps:

*  Single sign-on
*  Multifactor authentication
*  Enhanced security policies for G Suite
*  Web-filtering policies for G Suite
*  End-to-end visibility and analytics

## Advanced concepts

[Citrix Gateway SaaS and O365 Cloud Validated Reference Design](/en-us/advanced-concepts/design-guides/citrix-gateway-o365-saas.html)

[Citrix Gateway service SSO with Secure Private Access Validated Reference Design](/en-us/advanced-concepts/design-guides/citrix-gateway-service-sso-access-control.html)

## Summary

Citrix Secure Private Access is a consolidated solution for a secure digital workspace. Most organizations are adopting SaaS and web apps as the digital workspace is changing. Implementing a collaborative solution would significantly improve the security and provide benefits for enterprises, small businesses, and SaaS vendors, providing confidence that their data is protected.

The idea of only protecting a network is no longer sufficient. The organization must protect users and apps. This is why Citrix Secure Private Access delivers:

*  Consolidated access to SaaS, web, and virtual apps
*  Consistent end-user experience and flexibility to use any endpoint device
*  Application traffic visibility and threat detection using analytical services which helps in-app control for SaaS apps beyond single sign-on

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make your job easier, we would like to provide you with source diagrams that you can apply in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sa50da6ce47142da8).

## References

[Secure Private Access](/en-us/citrix-cloud/access-control.html)

[Tech Brief](/en-us/tech-zone/learn/tech-briefs/access-control.html)

[Tech Insights](/en-us/tech-zone/learn/tech-insights/access-control.html)

[Deliver secure, contextual user access on any device anywhere, without sacrificing IT control](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-secure-contextual-user-access-on-any-device.pdf)
