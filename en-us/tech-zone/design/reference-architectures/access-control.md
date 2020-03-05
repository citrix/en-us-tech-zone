---
layout: doc
description: Gain knowledge about the Citrix Access Control solution including key concepts, use cases, and strategies for implementing this comprehensive security solution for an organization’s apps and data.
---
# Citrix Access Control

## Contributors

**Author:** [Nagaraj Manoli](mailto:nagaraj.manoli@citrix.com)

**Special thanks:** [Praveen Raghuraman](mailto:praveen.raghuraman@citrix.com), [Allen Furmanski](https://twitter.com/tekguyallen)

## Audience

This document is intended for Citrix technical professionals, IT decision makers, partners, and security consultants who want to explore and adopt the Citrix Cloud service of Access Control. The reader must have a basic understanding of Citrix products, security, and Citrix Cloud frameworks. For more information on Citrix Cloud and its services, see the official product documentation for [Citrix Cloud](/en-us/citrix-cloud.html).

## Objective of this document

This document provides a technical overview of Citrix Access Control and architecture that provides conditional access to cloud apps and internet browsing-enhancing the organization’s overall security and compliance posture. Citrix Access Control combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators.

The document guides admins on an approach to deliver a secure digital workspace with a consolidated solution using Citrix Access Control integrated with multiple Citrix Cloud services including Citrix Gateway Service and Secure Browser. To monitor user behavior and activities Citrix Access Control integrates with Citrix Analytics.

## Security Controls and Mitigation

In today’s business world, user data is one of the most treasured commodities, and organizations continue to find more and more value in it. Most of the breaches happening in organizations are because of inadequate security controls, excessive privileges granted to internal users, and an over-reliance on network-based security controls. Many companies have unfortunately failed to truly adopt a secure in-depth approach to fortifying their environment.

Many IT departments within organizations do not have a solution for controlling access to users accessing an ever-increasing number of enterprise SaaS-based applications. In addition to the security risks posed by the potential for corporate data interacting with unmanaged SaaS apps, user productivity is lost due to logging into multiple apps. In addition, having to remember multiple passwords can often lead to bad password habits or the same passwords being used for multiple sites.

Browsing the internet poses another risk to enterprises today. Most of the attacks are exploiting vulnerabilities in websites, browsers and browser plug-ins according to security experts. In response, some organizations even completely prohibit internet browsing, severely affecting productivity.

Some web and SaaS apps require a certain browser version or plug-in to function properly. With the rapid changes to the browser landscape, ensuring the usability of the corporate web and SaaS apps quickly mounts to a resource consuming support task.

Many IT departments do not have a fully defined strategy for managing SaaS applications, resulting in significant security and compliance risk. Others have a fragmented approach resulting in multiple point solutions, such as:

*  **Deploying a Single Sign-On solution:** This helps streamline the user experience and can include multifactor authentication, but these solutions do not typically have granular policy controls on apps once users sign in.

*  **Deploying web filtering to control or shut down external browsing:** This is a point solution to protect from visiting malicious websites. However, features stop short of security policies for SaaS apps, leading many customers to deploy multiple solutions in addition to a web gateway.

*  **Publishing Browsers for each SaaS application:** Publishing a web browser with Citrix Virtual Apps and Desktops is a time-tested method for controlling access to unsanctioned SaaS applications, the “unknown” category of websites on the internet, and presenting a user with a single portal for SaaS and virtual apps. However, it does require additional IT resources to manage and is not the same user experience as directly connecting to a SaaS app via a browser.

*  **Relinquish management of SaaS applications to departmental control within the enterprise:** This opens up security and compliance gaps and misses the opportunity to understand user behavior and app usage, yet but it is often the default strategy for IT when faced with limited resources.

Citrix has come up with a better way to secure access to sanctioned SaaS and Web apps with a user-centric approach as opposed to traditional ways that do not account for security. Citrix Workspace provides contextual and secure access of SaaS, Web and Virtual Desktops and Applications, reduces exposure to insider and outsider threats, secures content collaboration and provides user behavior analytics and proactive security insights.

[![AC-Image-1](/en-us/tech-zone/design/media/reference-architectures_access-control_001.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_001.png)

The above diagram illustrates the traditional Citrix environment, endpoints are connecting through Citrix ADC and accessing their resources. These resources including apps, desktops, and data are being accessed from on-premises, and some of the external resources are accessed through internet that includes SaaS-based applications. Users are accessing these external resources securely through Secure Gateway and web filtering services.

Workspace app is a single entry point to access resources from any device. Workspace app provides a unique functionality to end users with enhanced security with embedded browser engine. Networking engine within Workspace app optimizes network traffic provides SSL VPN as well Micro VPN capabilities for certain applications. End users are connected through Citrix ADC provides Identity and Access Management capability.

Security monitoring provides end-user behavior and threat detection within the network. Data security includes DLP and IRM when users are working with files or sharing the files to keep control of data. In Citrix Cloud Web filtering, Analytics, and Citrix Gateway services offer a consolidated solution to deliver a secure digital workspace to end users.

## Citrix Access Control

Citrix **Access Control** combines the capabilities of instant secure access to SaaS and web applications through Single Sign-On (SSO), along with browser and cloud-based app controls web-filtering policies and integrated user-behavior analytics. Citrix Access Control goes beyond traditional SSO capabilities by introducing **cloud app control** – a set of enhanced security controls for SaaS and enterprise web apps providing conditional access to cloud apps and protecting user-actions based on admin governed access policies. The solution enhances the organization’s overall security and compliance posture. The user’s experience remains seamless and integrated because the SaaS and web apps can be accessed alongside their mobile, virtual apps and desktops as an integrated part of Citrix Workspace.

Citrix Access Control combines elements of several Citrix Cloud services to deliver an integrated experience for end users and administrators.

| **Functionality** | **Service/Component providing the functionality** |
| --- | --- |
| Consistent UI to access apps | Workspace Experience/WS App |
| SSO to SaaS and Web apps | Citrix Gateway Service Standard |
| Web filtering and categorization | Web filtering service |
| Enhanced security policies for SaaS | Cloud app control |
| Secure browsing | Secure Browser service |
| Visibility into Website access & risky behavior | Citrix Analytics |

## Why Citrix Access Control?

Data security is often characterized by the needs to ensure data integrity, confidentiality, and protecting company intellectual property. Today organizations are facing various challenges such as:

*  Multiple point products that are hard to manage and have different policy infrastructures
*  Providing secure remote access to end-users to access company information
*  Protecting intellectual property stored in cloud and SaaS apps and staying compliant
*  Gaining visibility into cloud and SaaS apps after SSO and obtaining risk assessments

Citrix Access Control helps IT and security admins to govern authorized end-user access to sanctioned SaaS and enterprise hosted web apps. User identities and attributes are used to determine access privileges and access control policies determine privileges that are required to perform operations.

[![AC-Image-2](/en-us/tech-zone/design/media/reference-architectures_access-control_002.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_002.png)

The integration of Citrix Access Control results in several benefits:

*  **Seamless user experience with Single-sign on to SaaS apps and hosted apps:** This capability comes through a combination of the Workspace app and Gateway Service

*  **Greater control for IT with organized delivery and access for SaaS applications:** Provides IT with a way to easily assign SaaS applications to their workforce

*  **Enhanced security with policy controls for SaaS usage:** Referred to as cloud app control, this is a new capability that provides IT with a way to enforce security policies on the SaaS applications that they provide to employees

*  **Flexibility to enable controlled access to public internet content or unknown SaaS apps without sacrificing security:** This capability comes through a combination of Secure Browser and web filtering functionality so that websites can be whitelisted/blacklisted or rendered in an isolated browser

*  **Visibility into SaaS usage and user web activity:** This capability that provides a subset of Citrix Analytics for SaaS and web activity to provide more visibility for security and compliance

## Citrix Access Control and Citrix Cloud

Citrix Access Control is one of the services offered by Citrix Cloud. To access these services an administrator must have a Citrix account (also known as Citrix.com or My Citrix account) to manage licenses and access the environment.

A Citrix account uses an organization ID(OrgID) as a unique identifier. The administrator can access their Citrix account by logging in at <https://www.citrix.com> with the user name or email address linked to the account. To begin with, Citrix Cloud navigates to <https://citrix.cloud.com> and create an account or sign in with an existing Citrix account to activate the trail of the Cloud Services.

A Citrix Cloud account allows admins to have broad administrative access on the services, therefore Citrix expects that the first admin who creates the Citrix Cloud account has to explicitly give access to other admins as required even if the other admin is already a member of the existing MyCitrix account.

Citrix Access Control is a tile and a solution which includes the integration across Citrix Gateway Service, Web Filtering Service, Secure Browser Service, and Citrix Analytics.

[![AC-Image-3](/en-us/tech-zone/design/media/reference-architectures_access-control_003.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_003.png)

**Access Control:** The Citrix Access Control service provides a unified experience integrating single sign-on, remote access, and content inspection into a single solution for end-to-end access control.

Access Control provides the following capabilities to administrators:

*  Configure multifactor authentication for end users
*  Configure a workspace to securely deliver access to apps from any device, manage and add SaaS applications from the library
*  Configure web filtering to allow/block websites to end-user, and redirect them to Citrix Secure Browser Service

**Analytics:** Citrix Analytics collects data across the Citrix portfolio of products and generates actionable insights, enabling administrators to proactively handle user and application security threats, improve app performance, and support continuous operations. Citrix Analytics gathers data and provides the following insights:

*  Security Analytics
*  Performance Analytics
*  Operations Analytics

**Gateway:** The Citrix Gateway Service provides a secure remote access solution, provides a unified user experience for the configured SaaS apps, heterogeneous virtual apps, and desktops. SaaS apps delivery using Citrix Gateway service provides an easy, secure, robust, and scalable solution to manage the apps. SaaS apps delivered on the cloud have the following benefits:

*  Simple configuration- Easy to operate, update, and consume
*  Single sign-on- Hassle free logon with SSO
*  Standard template for different apps- Template based configuration of popular apps

**Secure Browser:** Citrix Secure Browser Service isolates web browsing to protect the corporate network from browser-based attacks. It delivers consistent, secure remote access to an internet hosted web application with no need for user device configuration.

Administrators can rapidly roll out secure browsers, providing instant time-to-value. By isolating internet browsing, IT administrators can offer end users safe internet access without compromising enterprise security.

## Identity and Access Management

Identity and Access Management defines the identity providers and accounts used for administrators of and subscribers to Citrix Cloud and its offerings. Citrix Cloud uses the Citrix Identity provider to manage the identity information for users in the Citrix Cloud account. The administrator has the option to use Azure Active Directory or an on-premises Active Directory service.

To perform management activities, and install the Citrix Cloud Connector on Citrix Cloud, Citrix administrators use their identity to access Citrix Cloud. This identity mechanism provides authentication for administrators using an email address and password. Additionally, administrators can also use My Citrix credentials to sign in to Citrix Cloud.

[![AC-Image-4](/en-us/tech-zone/design/media/reference-architectures_access-control_004.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_004.png)

A subscriber’s identity defines the services to which they have access to in Citrix Cloud. Identity comes from Active Directory domain accounts provided from the domains within the resource location. Subscribers can authorize to access the offering from Citrix Cloud by assigning a subscriber to a library offering.

On-premises Active Directory communication with Citrix Cloud takes place through highly available Citrix Cloud Connectors. Organizations opting to use Azure Active Directory service have more flexibility in terms of managing the user accounts, audit control, and password policies. Also, the administrator can configure multifactor authentication for a higher level of security.

## Citrix Access Control with Enhanced Security for SaaS Apps

Citrix Cloud offers the single-sign-on capability to SaaS applications through the Citrix Gateway Service. SSO delivers a unified user experience for Web-based SaaS application SSO and publishing to the Citrix Workspace experience user interface. To enable a single sign-on user experience, a SaaS app trusts the SAML assertion provided by the Citrix Gateway Service.

The process of enabling single sign-on to SaaS applications is now simplified in just a few web forms and by clicking on the configuration page. Citrix Gateway Connector provides a proxy to the internal web-based SaaS application access to the Workspace app user.

Reference: [Citrix Gateway Connector](/en-us/citrix-gateway-service/gateway-connector.html)

### Content Control

Protecting user data (SaaS app users) is a challenging task for most of the organizations. Using Citrix Access Control organizations incorporate enhanced security policies within the SaaS application. Each policy enforces a restriction on the embedded browser when using the Workspace app for desktop or on Secure Browser when using Workspace app web or mobile.

*  **Preferred browser:** Disables local browser use and relies on the embedded browser engine (Workspace app - desktop) or Secure Browser service (Workspace app – mobile and web)
*  **Restrict clipboard access:** Disables cut/copy/paste operations between the app and endpoint clipboard
*  **Restrict printing:** Disables ability to print from within the app browser
*  **Restrict navigation:** Disables the next/back browser buttons
*  **Restrict downloads:** Disables the user’s ability to download from within the SaaS app
*  **Display watermark:** Overlays a screen-based watermark showing the user name and IP address of the endpoint. If a user tries to print or take a screenshot, the watermark will appear as displayed on the screen

## Citrix Access Control Single Sign-On without Enhanced Security

[![AC-Image-5](/en-us/tech-zone/design/media/reference-architectures_access-control_005.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_005.png)

| **SL NO** | **Workspace App with Sanctioned SaaS App** | **Local Browser with sanctioned SaaS App** |
| --- | --- | --- |
| **1** | The user launches the Workspace app on the endpoint. | The user launches a web browser on the endpoint, connects to Workspace and authenticates using on-premises Active Directory or Azure Active Directory. |
| **2** | Workspace app connects to the workspace and authenticates using the on-premises Active Directory/ Azure Active Directory. | Local browser populates with approved resources. |
| **3** | Workspace app populates with approved resources. | When the user selects a SaaS app, the local browser sends the request to Workspace, which requests a one-time-use URL and recommended browser from the Gateway Service. |
| **4** | Workspace app sends the request to Workspace, which requests a one-time-use URL from the Gateway Service and a preferred browser. | The local browser initiates a connection to the Gateway Service. |
| **5** | The local browser initiates a connection to the Gateway Service. | The Gateway Service requests an assertion from the Single Sign-on micro-service. |
| **6** | The Gateway Service requests an assertion from the Single Sign-on micro-service. | The local browser is redirected the SaaS app login page where the assertion is presented. |
| **7** | The local browser is redirected to the SaaS app login page where the assertion is presented. | The SaaS app contacts the Gateway Service to validate the assertion and authenticates the user. |
| **8** | SaaS app contacts the Gateway Service to validate the assertion and authenticates the user. | Once authenticated, communication occurs directly between the browser and SaaS application. |
| **9** | Once authenticated, communication occurs directly between the browser and SaaS application. | |

## Access Control- Single Sign-On with Enhanced Security

[![AC-Image-6](/en-us/tech-zone/design/media/reference-architectures_access-control_006.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_006.png)

| **SL NO** | **Workspace App with Sanctioned SaaS App** | **Local Browser with sanctioned SaaS App** |
| --- | ---| ---|
| **1** | The user launches the Workspace app on the endpoint. | The user launches a web browser on the endpoint, connects to Workspace and authenticates using on-premises Active Directory or Azure Active Directory. |
| **2** | Workspace app connects to the workspace and authenticates using the on-premises Active Directory/Azure Active Directory. | Local browser populates with approved resources. |
| **3** | Workspace app populates with approved resources. | When the user selects a SaaS app, the local browser sends the request to Workspace, which requests a one-time-use URL and recommended browser from the Gateway Service. |
| **4** | Workspace app sends the request to Workspace, which requests a one-time-use URL from the Gateway Service. | The local browser initiates a Secure Browser connection. |
| **5** | The embedded browser initiates a connection to the Gateway Service. | The Secure Browser initiates a connection to the Gateway Service. |
| **6** | The Gateway Service requests an assertion from the Single Sign-on micro-service and enhanced security policies from the Access Control service. | The Gateway Service requests an assertion from the SSO micro-service and enhanced security policies from the Access Control service. |
| **7** | The embedded browser is redirected to the SaaS app login page where the assertion is presented. | Secure Browser is redirected to the SaaS app login page where the assertion is presented. |
| **8** | The SaaS app contacts the Gateway Service to validate the assertion and authenticates the user. | The SaaS app contacts the Gateway Service to validate the assertion and authenticates the user. |
| **9** | Once authenticated, communication occurs directly between the browser and SaaS application. | Once authenticated, communication occurs directly between the browser and SaaS application. |

Reference: [Citrix Access Control tech brief](/en-us/tech-zone/learn/tech-briefs/access-control.html)

## Contextual Access

Most SaaS apps are safe to use though sometimes when a user clicks a hyperlink within a SaaS app, it can possess a security risk for an organization. The web filtering micro-service allows, denies or redirects the hyperlink request from the user.

[![AC-Image-7](/en-us/tech-zone/design/media/reference-architectures_access-control_007.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_007.png)

| **SL NO** | **Workspace App with Enhanced Security** | **Local Browser with Enhanced Security** |
| --- | --- | --- |
| **1** | The user selects a hyperlink from within the SaaS app. | The user selects a hyperlink from within the SaaS app. |
| **2** | The embedded browser within Workspace app sends the URL to the Access Control service | Secure Browser sends the URL to the Access Control service |
| **3** | The Access Control service requests an analysis of the URL by the Web Filtering micro-service | The Access Control service requests an analysis of the URL by the Web Filtering micro-service. |
| **4** | For blocked links, the Web Filtering micro-service denies access to the hyperlink. | For blocked links, the Web Filtering micro-service denies access to the hyperlink. |
| **5** | For approved links, the Web Filtering micro-service allows the user to access the link with the embedded browser. | For approved links, the Web Filtering micro-service allows the user to access the link as a new tab within their current Secure Browser service session. |
| **6** | For redirected links, the Web Filtering micro-service has Workspace app send the link to Secure Browser service, which starts a new, virtual browser session for the end user. | For redirected links, the Web Filtering micro-service allow the user to access the link as a new tab within their current Secure Browser service session. |

## Web Filtering Overview

Web filtering provides policy-based control of websites by using the information contained in URLs. This helps network administrators monitor and control user access to malicious websites on the network.

This service release enables web filtering for Citrix Workspace app accessing SaaS apps and the internet and Citrix Secure Browser accessing to SaaS and the internet.

[![AC-Image-8](/en-us/tech-zone/design/media/reference-architectures_access-control_008.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_008.png)

The Citrix Access Control administrator can block and allow a list of URLs. Web filtering controller uses a categorization database and URLs list. When the request comes to the web filtering controller, first it checks the global allow list which also contains critical Citrix Cloud URLs. Then it comes to Lists and Categorization and verifies with blocked and allowed and redirect to Secure Browser URLs. If none of the URLs match, then by default it falls back to the default list.

## Citrix Access Control and Citrix Analytics

The Citrix Analytics service is a cloud-based service which facilitates pragmatic insights by collecting data across the Citrix portfolio of products. Citrix Access Control organizes and produces information on activities of the users, such as websites visited, and the bandwidth spent. Citrix Access Control also monitors malware and phishing sites by looking into bandwidth consumption and then reports. The administrator can take corrective actions by making use of these key metrics to monitor the network.

Citrix Analytics easily integrates with Citrix Access Control, Citrix Gateway service, and other Citrix portfolio products. Citrix Analytics provides comprehensive insights into user behavior. It uses machine learning algorithms to detect anomalous user behavior, troubleshoot user sessions and view operational metrics for users in an organization using Citrix products.

Citrix services and products send data to Citrix Analytics-referred to as data sources. These data sources associated with the Citrix Cloud account are automatically discovered by the Citrix Analytics service. Citrix Cloud logs are transmitted securely to Citrix Analytics. The logs are collected from Citrix Access Control and maintained separately from the data sources.

[![AC-Image-9](/en-us/tech-zone/design/media/reference-architectures_access-control_009.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_009.png)

The Citrix Access Control service delivers security and operations dashboards. Under security, it provides user’s risk profiles, user access summary activity performed by the users, such as URL or domain visited and the bandwidth used. App access summarizes the details of domains, URLs and apps accessed by the users.

Reference: [Citrix Access Control and Analytics](/en-us/citrix-cloud/access-control/analytics.html)

[![AC-Image-10](/en-us/tech-zone/design/media/reference-architectures_access-control_010.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_010.png)

Citrix administrators can create rules on Citrix Analytics to perform actions on user accounts when unusual or suspicious activities occur. A rule is a set of conditions that must be met for an action to be executed. A rule can contain a single condition and one or more actions. Conditions such as “risk score” and “risk score change” are global conditions. Global conditions can be applied to a specific user for a specific data source.

The administrator creates a rule to take action based on the user’s activity by applying conditions which are listed below:

*  **Attempt to Access Blacklisted URL.** Indicates an attempt to access a blacklisted URL.
*  **Risky Website Access.** Indicates that the user attempted to access malicious, suspicious or risky websites.
*  **Unusual download volume.** Indicates that the volume of data downloaded by the user from applications or websites has exceeded the threshold defined implicitly by Citrix Analytics.
*  **Unusual upload volume.** Indicates that the volume of data uploaded by the user from applications or websites has exceeded the threshold defined implicitly by Citrix Analytics.

Reference: [Access Control and Analytics](/en-us/citrix-cloud/access-control/analytics.html)

## Citrix Access Control End-User experience

The Citrix administrator has power to extend the security control with the help of Citrix Access Control. Citrix Workspace app is an entry point to access all resources securely, end-users can access virtual apps, desktops, SaaS apps, files through Citrix Workspace app. With Citrix Access Control administrators will be able to control how a SaaS Application can be accessed by the end user via Citrix Workspace Experience web UI or native Citrix Workspace app client.

For more information please refer, Citrix Workspace app [Reference Architecture](/en-us/tech-zone/design/reference-architectures/workspace-app.html).

### End-user experience with Workspace App and Web portal

[![AC-Image-11](/en-us/tech-zone/design/media/reference-architectures_access-control_011.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_011.png)

[![AC-Image-12](/en-us/tech-zone/design/media/reference-architectures_access-control_012.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_012.png)

When the user launches the Workspace app on the endpoint, users will see their applications, desktops, files, and SaaS apps. If the user clicks on the SaaS application when the enhanced security is turned off, the application opens in a standard browser which is locally installed. If the administrator has turned on enhanced security then SaaS apps open on the embedded browser within Workspace app. Accessibility to hyperlinks within SaaS apps and web apps are controlled based on web filtering policies.

Similarly, with Workspace Web portal, when enhanced security it turned off SaaS applications are opened through the standard browser which is natively installed. When enhanced security is turned on, SaaS apps are opened through the Secure Browser. Users will be able to access the websites within SaaS apps based on web filtering policies.

## Implementing Citrix Access Control

The Citrix Access Control service is a cloud-based service. To provide user-centric solutions to organizations, and comply with policies Citrix Access Control plays a vital role. Along with Citrix Access Control, there are other services that are enabled to provide consolidated solutions to end users. To get started with onboarding and setting up the Citrix Access Control service, the administrator must set up authentication, configure access to SaaS apps, and specify the content access settings in the Citrix Access Control service. The end users can access the service from the Citrix Workspace app or the Workspace URL.

[![AC-Image-13](/en-us/tech-zone/design/media/reference-architectures_access-control_013.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_013.png)

The Citrix administrator logs in to Citrix Cloud using their credentials, and request Citrix Access Control service. The Citrix Access Control is integrated with other Citrix portfolio products including Citrix Gateway, Secure Browser, and Citrix Analytics. This solution offers security for SaaS apps.

[![AC-Image-14](/en-us/tech-zone/design/media/reference-architectures_access-control_014.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_014.png)

**Step1:** The Citrix administrator configures Identity and Access Management on Citrix Cloud. By default, Citrix Cloud uses the Citrix Identity provider to manage the identity information for all users in the Citrix Cloud account. The administrator has the option to change this to use Azure Active Directory or an on-premises Active Directory service.

**Step2:** The Citrix administrator logs in to Citrix Gateway service to configure single sign-on to SaaS and web applications. The Citrix Gateway service offers a secure login to the company’s environment, end user’s sign in to applications utilizing SAML single sign-on.

Add a web or SaaS app to the library from the template. To learn more about a list of SaaS applications supported by Citrix Access Control follow the link below

Reference: [SaaS applications supported by the Citrix Access Control Service](/en-us/citrix-cloud/access-control/saas-apps-supported-by-acs.html)

**Step3:** The administrator enters the app details like name of the application, URL and domain details. Enhanced security policies are enabled to prevent data leaks. These policies within the SaaS applications enforce a restriction on the embedded browser when using Workspace app for desktop or on Secure Browser when using Workspace app web or mobile.

[![AC-Image-15](/en-us/tech-zone/design/media/reference-architectures_access-control_015.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_015.png)

**Step4:** **Enable Single sign on to web/SaaS apps for publishing:** Citrix Gateway Service (cloud services only) provides single sign-on to SaaS application. To enable single sign on to SaaS application is now simplified in just a few web forms and clicking on the configuration page greatly simplifies the deployment process. It is possible for administrators to leverage the Gateway Connector to proxy the internal web-based SaaS application access to the external Workspace app user.

Under the Library section of Citrix Cloud, SaaS and web apps are published. The administrator has to add the users from choosing the domain, only subscribed users will get to access the application through Workspace app or Workspace web.

**Step5:** **Filter website lists:** To protect the corporate network from browser-based attacks, Citrix Access Control includes a web filtering service. Based on the policies the web filtering service allows, denies or redirects the hyperlink request from the user as seen below:

**Allowed:** Request link is considered safe and it is access is permitted within the embedded browser within the Workspace app.

**Blocked:** The hyperlink is considered dangerous and access is denied.

**Redirected:** The administrator takes a precaution on websites which may possess security threats by redirecting through the Secure Browser Service.

**Step6:** Filter website Categories The categorization database helps to filter web traffic controlling end-user access to specific websites, such as social networking, gambling, adult content, new media, and shopping. Categories restrict user access to specific websites and website categories.

| **Categorization Preset provide for convenient out-of-the-box templates** |  |
| --- | --- |
| **Strict** | Minimizes the risk of accessing unsecured or malicious websites. End users can still access websites with very low risk. Includes most business travel and social media websites. (133/192 categories) |
| **Moderate** | Minimizes risk while allowing additional categories with a low probability of exposure from unsecured or malicious sites. Includes most business travel, leisure, and social media websites. (65/192 categories) |
| **Lenient** | Maximizes access while still controlling risk from illegal and malicious websites. (36/192 categories) |
| **None** | Allows all categories. |
| **Custom** | Configure custom filtering of categories. |

Reference: [Citrix Access Control categories-list](/en-us/citrix-cloud/access-control/categories-list.html)

Citrix Workspace app gives users a great experience (secure, contextual, unified workspace) on any device. Users get seamless and secure access through single sign-on, to all the apps they need to be productive.

To validate the configuration, end users can launch the SaaS app from Citrix Workspace app (or Citrix Workspace Web). Also, users can verify allowed/blocked websites added in the website filtering lists by visiting the URLs. The Citrix Workspace app embedded browser allows administrators to provide native access to SaaS applications with enhanced security control.

To learn more on Citrix Workspace app refer to the [Citrix Workspace app Reference Architecture](/en-us/tech-zone/design/reference-architectures/workspace-app.html).

**Steps7:** Citrix Access Control service integrates with Citrix Analytics to fetch information on the activities of users, such as websites visited, and the bandwidth consumed. Citrix Analytics reports threat detection such as malware and phishing sites.

The administrator can log in to the Citrix Analytics cloud service and create rules for Citrix Access Control and they apply the action plan when the conditions are met. To monitor and analyze user behavior activities enable “Turn on Data processing” for Citrix Access Control for analytics on Citrix Cloud (Data sources are Citrix services and products that send data to Citrix Analytics).

[![AC-Image-16](/en-us/tech-zone/design/media/reference-architectures_access-control_016.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_016.png)

The Citrix Analytics Service monitors user activities and behavior using machine learning and artificial intelligence. Data sources for Citrix Analytics gathered from Workspace app, Citrix Gateway, Citrix Access Control, and Secure Browser service.

## Citrix Access Control Use Cases

Today organizations are turning towards Software as a Service (SaaS) solutions to address business requirements though often without taking the necessary security measures or maintaining the applications. Most SaaS application security failures are caused by users, not cloud providers. The organization must regulate its strategies to address these flaws.

Citrix Access Control enforces enhanced security policies for SaaS apps (watermark, copy-paste restriction, prevent downloading/uploading sensitive data). It also defines access policies for website categories and websites to be blocked/ allowed in the form of web filtering.

| Existing Brownfield deployments | How Citrix Access Control fits in |
| --- | --- |
| Organizations looking to adopt Citrix Cloud services. | 1) Citrix Access Control provides the right fit into their environment, providing security for SaaS, SSO, and web filtering feature to end users. 2) Organization looking towards on-premises model Citrix Gateway helps in providing SSO and enhanced security to end users. |
| Organizations that have already adopted the Citrix Gateway service. | 1) Citrix Cloud offers several cloud-based services including Citrix Access Control, Analytics, and Secure browser providing security control for SaaS and internet. |
| Organizations that have already adopted third-party SSO | 1) Citrix Access Control provides granular level security control for SaaS apps, minimizes web-based threats and automatic policy enforcement through analytics, based on user behavior. 2) ICA Proxy for both on-premises and Citrix Virtual Apps and Desktops Service. |

## Citrix Access Control and Content Collaboration

Most organizations have experienced some kind of ransomware or multiple phishing attempts that have compromised their network. The root causes for such threats are often due to inadequate protection from web-based threats. Lack of visibility into what websites users are accessing on a day-to-day basis.

The Citrix Access Control service enables an organization to protect its environment from browser-based attacks and data leaks. When employees access their apps from any device whether they are in the office, home or traveling Citrix Access Control service provides a cohesive experience integrating SSO, two-factor authentication, remote access, and web filtering into a single solution for end-to-end access control.

[![AC-Image-17](/en-us/tech-zone/design/media/reference-architectures_access-control_017.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_017.png)

Citrix Content Collaboration enables users to easily and securely exchange documents. There are many ways to work using Citrix Content Collaboration, including a web-based interface, mobile clients, desktop apps, and integration with Microsoft Outlook and Gmail.

Citrix Files is a file manager that offers data sharing and storage, customizable usage and settings, and tools that allow users to collaborate more easily and get the work done from any device, anytime and anywhere.

The above diagram depicts delivering the Citrix Files SaaS app to end user in a hybrid cloud model scenario. The Citrix Cloud Connector provides a link to the Citrix Cloud account and resource location. The resource location will contain the Active Directory for end users, allowing users to seamlessly sign on to their web application.

The Citrix Gateway Service offers authentication, single sign-on, and enables fast and secure delivery of Citrix Virtual Apps and SaaS applications. End user’s login to the company’s environment using their login credentials and will then be able to sign on to a web application utilizing SAML single sign-on. On the Citrix Gateway Service administrators choose the template of Web/SaaS app or define their own application parameters. For example:

**Name the application:** “Citrix Files”

**Enter the URL:** `<https://xxxxx.sharefile.com/>`

Similarly, on the SSO page ensuring SAML is selected, assuming SAML/SSO setting for Citrix Files has already been completed for the backend.

**The Assertion URL is:** `<https://xxxxx.sharefile.com/saml/xxxx>`

**Audience:** `<https://xxxxx.sharefile.com>`

**Name ID format:** Email address

**Name ID:** Email

Once the SaaS app is added to the Citrix Cloud library, the administrator has to manage subscribers and provide the Workspace URL to users to access. In conjunction with Active Directory, it can serve as an identity provider to allow SAML single sign on to various web and SaaS applications.

### Content Control and Contextual Access

On the Enhanced Security page, the administrator restricts and enforces policies to protect the organization from sensitive data leaks. It provides the following options to apply:

*  restricted clipboard access
*  restricted printing
*  restricted navigation
*  restricted downloads
*  displaying of watermarks

To block unwanted websites which possess security risks to organizations the Citrix administrator has to block and allow URLs by adding URL lists or by choosing category lists. The administrator can also take caution to redirect URLs through a secure browser.

### User behavior and activities

To monitor user behavior and activities, Citrix Access Control service is easily integrated with the Citrix Analytics service. The administrator imposes predefined conditions and also takes an action plan to mitigate the risks.

## Citrix Access Control and Google G Suite

G Suite is a SaaS application, these apps are accessible remotely over the internet. G Suite is formerly known as Google Apps developed by Google. G Suite comprises Gmail, Hangouts, Calendar, and Google+ for communication; Drive for storage; Docs, Sheets, Slides, Forms, and Sites for collaboration. It also comprises an app development platform, App Maker. G Suite provides tremendous productivity advantages, but most of the networks are not designed properly to deliver the performance and security needed for mobility and cloud.

Google Cloud and Citrix enable end users to work with pioneering solutions delivering enhanced security with great user experience, and the flexibility of choice in an app-centric, mobile-first, multi and hybrid cloud world.

G Suite, a market-leading productivity SaaS app, integrated with Citrix Access Control which allows the organization to get increased visibility and control for SaaS application which in turn prevents data leaks and unauthorized disclosure of sensitive information.

End users are accessing G Suite application by just entering URL and login credentials through the Citrix Workspace app. Users will have entire workspace including apps, desktops, and files and keeping in mind users will never have to enter another user name and password. Entire workspace delivered through a single access point which will improve productivity and streamline common workflows for the end-user.

[![AC-Image-18](/en-us/tech-zone/design/media/reference-architectures_access-control_018.png)](/en-us/tech-zone/design/media/reference-architectures_access-control_018.png)

Citrix Access Control not only provides a single sign-on feature but also provides a layer of security control which is not available anywhere.

### Deployment

Citrix Cloud Connectors are used to handle all communications between resource location and Citrix Cloud. To provide high availability, a minimum of two connectors are deployed at each resource location. The resource location will serve as Active Directory for end users allowing users to seamlessly sign on to their G Suite applications.

Citrix Cloud Gateway service in conjunction with Active Directory can serve as an Identity provider to allow SAML single sign-on to G Suite SaaS application. Citrix Access Control allows the end users to launch G Suite SaaS application in Secure Browser sessions and allow the administrator to apply five different control policies.

**Scenario 1:** Enhanced security is turned off. When a user launches Gmail within the G Suite it opens it in a standard browser and similarly on the URL within their Gmail account makes use of a standard browser without any additional security policies or controls. A user has the option to navigate from the page, cut, copy and print the page.

**Scenario 2:** Enhanced security is turned on. When a user launches Gmail within the G Suite it opens in a Secure Browser. Now a layer of control capability is applied through Citrix Access Control and the end user will not have a navigation bar to navigate away from the site and cut, copy and paste restrictions are imposed. Apart from this Citrix Access Control provides URL filtering capability which prevents from visiting malicious websites. Also, users can be redirected to a Secure Browser based on the policies.

Benefits of using Citrix Access Control with G Suite SaaS apps:

*  Single sign-on experience
*  Multifactor authentication
*  Enhanced security policies for G Suite
*  Web-filtering policies for G Suite
*  End-to-end visibility and analytics

## Advanced Concepts

[Citrix Gateway SaaS and O365 Cloud Validated Reference Design](/en-us/advanced-concepts/design-guides/citrix-gateway-o365-saas.html)

[Citrix Gateway Service SSO with Access Control Validated Reference Design](/en-us/advanced-concepts/design-guides/citrix-gateway-service-sso-access-control.html)

## Summary

Citrix Access Control is a consolidated solution to deliver a secure digital workspace. Most organizations are adopting SaaS and web apps as the digital workspace is changing. Implementing a collaborative solution would significantly improve the security and provide benefits for enterprises, small businesses, and SaaS vendors with confidence knowing that their data is protected.

The idea of solely protecting a network is no longer enough. The organization must protect users and apps so Citrix Access Control delivers:

*  Consolidated access to SaaS, web and virtual apps
*  Consistent end-user experience and flexibility to use any endpoint device
*  Application traffic visibility and threat detection using analytical services which helps in-app control for SaaS apps beyond single sign-on

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sa50da6ce47142da8).

## References

[Access Control](/en-us/citrix-cloud/access-control.html)

[Tech Brief](/en-us/tech-zone/learn/tech-briefs/access-control.html)

[Tech Insights](/en-us/tech-zone/learn/tech-insights/access-control.html)

[Deliver secure, contextual user access on any device anywhere, without sacrificing IT control](https://www.citrix.com/content/dam/citrix/en_us/documents/solution-brief/deliver-secure-contextual-user-access-on-any-device.pdf)
