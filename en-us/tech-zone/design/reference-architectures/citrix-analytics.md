---
layout: doc
h3InToc: true
contributedBy: Nagaraj Manoli
specialThanksTo: George Tsolis, Vikramjeet Singh, Arun Pons
description: Learn about analytics services offered by Citrix Cloud including security analytics, performance analytics, and integration with other Citrix portfolio products.
tz_title: Citrix Analytics
tz_products: citrix-analytics;
---
# Reference Architecture: Citrix Analytics

## Audience

This document is intended for technical professionals, IT decision-makers, partners, and system-integrators. This document also allows the administrator to explore and adopt the Citrix Analytics service with other Citrix portfolio products. Citrix Analytics enhances the security of an organization’s Citrix environment by efficiently monitoring and managing the risk factors. The reader need to have a basic understanding of Citrix portfolio products and solutions.

## Objective of this document

This document covers a technical overview, architectural concepts, and capabilities of the Citrix Analytics service. This document includes a tailor-made solution with other Citrix solutions that help administrators and users to understand and adopt in their Citrix environment.

## Intelligent threat detection and mitigation

In today’s era, organizations are more concerned about the security and privacy of information, and many organizations are betting on best-in-class defensive security solution in their environment. One of the emerging technologies is an intelligent threat detection platform. Such a platform helps many organizations to aggregate, correlate, and analyze threat data from different sources to take relevant defensive actions.

In the dynamic environment of IT, the threat factors keep changing, advanced threats focused on public and private organizations are growing at a faster rate. The organization needs a complete security solution that mitigates and encounters any dangerous acts. The solution safeguards the organization's intellectual property, sensitive information, and financial data.

Machine learning and Artificial intelligence

Many organizations have faced (and continue to face today) various cyber-attacks. Intruders are adopting automation and scripts in their attacks and increase their speed and scale. The organization must mitigate and be able to respond in real-time at CPU speed to counter these kinds of aggressive attacks. Machine learning and artificial intelligence can help and enables the organization to counter the attacks and build defensive walls effectively.

Adoption of machine learning and artificial intelligence improves the security of the IT environment. Workforce errors can be mitigated and reduced. Machine learning and artificial intelligence can help in areas such as risk analysis, anti-malware, and anomaly detection.

Artificial intelligence can be applied to differentiate between normal and abnormal behaviors in the environment. Machine learning can be used to recognize these behaviors and provide a layer of security to network and software applications in the background. Machine learning uses stored logs/records and learns from the analyses to predict the data in the future.

[![CAS-1](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_001.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_001.png)

The preceding diagram depicts a conceptual machine learning platform. In general, a machine learning platform caters for analyzing input data from data collection points (data sources). In later stages, it segregates data based on the type of applications. The platform keeps updating the models and profiles based on the data and the results. Machine learning techniques are applied in multiple ways that are specific to the requirement.

By applying machine learning techniques on more massive sets of data, an organization can be more efficient in their threat intelligence compilation and threat investigation. In this way, organizations can be more proactive in their approach to security threats and concerns.

## Introduction to Citrix Analytics

Many organizations are facing cyber threats from all over the world. In real-time, it is challenging to identify insider threats as it may be even more damaging than external threats. Standard analytics often fails to expose those threats before severe damage to the system. The organization has to adopt user behavior analytics delivering proactive, secure insights. Standard analytics solutions primarily focus on security, and resolution does not provide visibility into the user session and information on user activities. Eventually, the IT team loses control over the performance and operations of the IT environment.

Citrix has developed a turnkey solution that works across the Citrix product portfolio. Citrix Analytics collects data across Citrix portfolio products and third-party products. Citrix Analytics allows administrators to detect, analyze, and proactively respond to security threats across Citrix environments.

Citrix Analytics enables administrators to handle user and application security threats, improve app performance, and support continuous operations. Citrix Analytics is available as a cloud service delivered through Citrix Cloud.

### Citrix Analytics offerings

Security Analytics

Security Analytics provides visibility into user and application behavior. The administrator can distinguish between normal behavior and a malicious attacker. An inbuilt machine learning platform that proactively identifies and manages internal and external threats.

Performance Analytics

Performance Analytics provides visibility into user session details across an organization. Metrics collected by analytics engines help to identify issues that arise during a user’s login session.

Operations Analytics

Operations Analytics provides information on user activities such as websites visited and bandwidth consumption. Metrics that are received from data sources help to monitor networks and take corrective actions.

[![CAS-2](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_002.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_002.png)

The preceding diagram depicts the Citrix Analytics service that is a cloud-based service which works across Citrix portfolio products and third-party products. It collects data from different data sources and detects abnormal behaviors of a user or any other entity. This process uses Machine Learning (ML) algorithms that continuously monitor the customer environment.

## Data Governance and Data Sources

Data governance provides information regarding the collection, storage, retention of logs, and protects the gathered data by Analytics service. Security administrators can choose the logs that have to be monitored and take representative action based on the logged activity.

[![CAS-3](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_003.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_003.png)

Most of the organizations have some form of data governance for individual applications or sections. Data governance is an essential task to perform during the project implementation. Few of the obligatory topics encompassed by data governance are:

*  Data sources
*  Data privacy
*  Data transmission
*  Data control
*  Data retention
*  Data storage
*  Data quality and types

Reference: [Data governance](/en-us/citrix-analytics/data-governance.html)

### Data Sources

Data sources are the services that send data to Citrix Analytics. Services that are running on cloud or in the on-premises locations that become a data source to Citrix Analytics by enabling certain functions within the product.

Services that are running on Citrix Cloud, including Content Collaboration, Endpoint Management associated with the Citrix Cloud account, are automatically discovered by Citrix Analytics. Other on-premises services such as Citrix Gateway and Citrix Virtual Apps and Desktops can be added as data sources to Citrix Analytics.

The preceding diagrams show external data sources such as Microsoft® Graph Security and Microsoft® Active Directory are part of Citrix Analytics data sources. Citrix Analytics captures data from these external data sources after successful integration.

Reference: [Data Sources](/en-us/citrix-analytics/data-sources.html)

## Tech Concepts

This section discusses the architecture and services offered by Citrix Analytics.

## Citrix Analytics for Citrix portfolio products

Citrix Analytics can be integrated with multiple Citrix and Microsoft® products. It collects metrics on users, applications, endpoints, networks, and data to deliver comprehensive insights into user behavior. Citrix Analytics, as of today, supports the following products:

*  Citrix Secure Workspace Access
*  Citrix Content Collaboration
*  Citrix Gateway
*  Citrix Virtual Apps and Desktops
*  Citrix Endpoint Management

Citrix Analytics creates profiles of the users and applications across the network. Profile creation is only possible with information/data collected from the data source (user behavior information). This profile contains information about the devices, files, locations, and so on. To mitigate the threats in the network, the pattern generated by analytics provides high visibility and take necessary action. This service provides complete visibility over user behavior in the environment.

[![CAS-4](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_004.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_004.png)

The preceding picture depicts the integration of Citrix products with the Citrix Analytics cloud service. Typically, end-users use their own devices to connect to Citrix Workspace and access required resources. Those services communicate with the Citrix Analytics Service hosted in Citrix Cloud. Also, customers can tie back on-premises Citrix Virtual Apps and Desktops environment to communicate with the Citrix Analytics service. Connecting on-premises resources requires either site aggregation or an on-premises StoreFront and Citrix Analytics agent installed on the Delivery Controller.

Citrix Analytics service receives logs directly from the data sources. Captured data remain in the databases for 13 months. Citrix Analytics uses these metrics for analysis through a machine learning platform and can perform actions when deviant or suspicious activities occur.

## Citrix Analytics and Microsoft® products integration

Most organizations today rely on a diverse portfolio of security solutions that include endpoint protection, network firewalls, identity, access controls, cloud security, and so on. In the end, it tends to increase cost and complexity. Along with that, connecting multiple security tools and workflows becomes a challenging task for the IT team. To overcome these challenges integration process is simplified. Citrix Analytics integration with Microsoft® products helps unification of security and incident management which results in simplified reporting and analytics.

Citrix Analytics supports the integration of Microsoft® products that include Microsoft® Graph Security and Microsoft® Active Directory. Currently, it supports Azure AD Identity Protection and Windows Defender ATP from Microsoft® Graph Security. To enable this service on Microsoft® products, the customer must have enabled Citrix Analytics Service from Citrix Cloud. For more information, refer to the following [link](/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/signing-up-for-citrix-cloud).

### Enable Citrix Analytics on Microsoft® Graph Security

Microsoft® Graph Security API provides a standard interface and uniform schema to integrate security alerts, unlock contextual information, and simplify security automation. Microsoft® Graph Security aggregates data from multiple security providers, including Microsoft® Defender ATP, Office 365 ATP, Azure ATP, Microsoft® Intune, Azure Sentinel, and so on.

Microsoft® Graph Security API can easily be coupled with Citrix Analytics. The Microsoft® Graph Security acts as a data source for the Analytics service that transmits data from Security Providers. Currently, this solution supports the following security providers from Microsoft® Graph Security:

*  Azure AD Identity Protection
*  Windows Defender ATP

[![CAS-5](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_005.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_005.png)

The preceding diagram depicts the Microsoft® Security Graph solution with Citrix Analytics to enhance overall analytics capabilities. The administrator can have clear insights of user behavior during the resource utilization that includes applications and end-user desktops. Citrix Analytics pulls the data from Microsoft® Graph Security hosted on Azure. Both the products work on the same platform that even makes it easier for integration.

Citrix Analytics UI provides processed data based on risk score indicators from high, medium, and low. Based on the risk score, value analytics can perform actions on that particular user. With the external feeds from Microsoft® Graph Security to Citrix, Analytics administrators can start to have a holistic picture of the access to resources by a user.

Reference: [Integration of Microsoft® Graph Security](/en-us/citrix-analytics/getting-started/microsoft-security-graph.html)

### Integrate Analytics with Microsoft® Active Directory

The organization can connect Microsoft® Active Directory service with Citrix Analytics, resulting in an enhancement in the user profile in Citrix Analytics. User profile in Citrix Analytics encompasses imported information such as user information and user groups data. In case risky users are identified by Citrix Analytics, imported information like job title, organization, office location, email, and contact details help in gaining visibility of the user profile.

Security Analytics has provisions for monitoring privileged users. This functionality enables the administrator to monitor behavior anomalies of privileged users closely. For example, if a privileged user starts deleting files and folders excessively, the machine learning platform detects unusual behavior and triggers an alarm.

Reference: [Privileged users in Citrix Analytics](/en-us/citrix-analytics/security-analytics/users-dashboard.html#privileged-users)

[![CAS-6](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_006.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_006.png)

The preceding diagram depicts the integration of Microsoft® Active Directory with Citrix Analytics. Before enabling this service by the administrator, Cloud Connectors are installed on-premises to pull the information. On successful integration, the Citrix administrator has to turn on data processing on the Citrix Analytics UI so that they can monitor and troubleshoot risks identified in the environment.

To learn more about integration with Microsoft® Active Directory, refer to this [link](/en-us/citrix-analytics/getting-started/active-directory-integration.html).

## Analytics

Countless use cases exist by adopting Citrix Analytics in a Citrix environment. The Analytics Service is divided into three areas:

*  Security Analytics
*  Performance Analytics
*  Operations Analytics

Each offering is discussed in the following sections:

## Security Analytics

Security Analytics aggregates data from endpoints for security monitoring and threat detection. Many deployments have a different set of tools that incorporate large and diverse data sets into the algorithm. As technology changes security analytics include adaptive learning skills. That helps to calibrate detection models, based on learnings and logic behind anomaly detection.

Citrix Security Analytics receives data from multiple data sources and displays actionable insights. Machine learning algorithms within Security Analytics detect and takes predefined actions on user behavior. Risk score indicators that are dynamic based on users, user behavior, endpoints, network traffic, and files.

Security Analytics supports integration with the following:

*  Citrix Content Collaboration
*  Citrix Endpoint Management
*  Citrix Secure Workspace Access
*  Citrix Gateway
*  Citrix Virtual Apps and Desktops
*  Microsoft® Graph Security
*  Microsoft® Active Directory

To learn more about Security Analytics, refer to the following [link](/en-us/citrix-analytics/security-analytics.html).

### User risk indicators

User risk indicators are user activities that look suspicious or can pose a security threat to the organization. User risk indicators span across all Citrix Products used in the deployment. The signs are based on user behavior and are triggered where the user’s behavior deviates from the norm. User risk indicators help in determining the user’s risk score.

User risk indicators occur based on the following categories:

*  Access-based
*  Data based
*  Application-based

[![CAS-7](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_007.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_007.png)

The preceding diagram depicts user risk score indicators. The indicators are based on user behavior and are triggered where the user’s behavior deviates from the norm.

Reference: [Citrix user risk indicators](/en-us/citrix-analytics/security-analytics/risk-indicators.html)

### Policies and actions

A policy is defined as a set of conditions that must be met for an action to execute. A policy contains a single condition and one or more actions. The administrator can create single policy with multiple actions that can be applied to a user’s account.

Policies on Citrix Analytics help to perform actions on user accounts when unusual or suspicious activities occur. Once policies are applied, the action is triggered immediately after an unexpected event occurs.

[![CAS-8](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_008.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_008.png)

As mentioned, the policy is a set of conditions. The risk score and risk score change are global conditions applied to a specific user for a particular data source.

Reference: [Policies](/en-us/citrix-analytics/security-analytics/policies-and-actions.html#what-are-policies)

Actions help to respond to suspicious events and prevent future anomalous events from occurring. The administrator can take action on user accounts that display unusual or suspicious behavior. It is up to the admin to apply action automatically, or manually depending on the conditions.

Reference: [Actions](/en-us/citrix-analytics/security-analytics/policies-and-actions.html#what-are-actions)

## Performance Analytics

Performance Analytics is a powerful tool to find the root cause for end-user experience issues. Also, it quantifies user experience, and app performance gives users end-to-end visibility. Performance Analytics supports multi-site aggregation and reporting so that data represented from multiple sites or multiple sources in a unified display.

The user experience score is calculated based on latency, logon duration, reconnections, and failures. The exact root cause of the problem identified by keenly looking into the metrics. For example, logon duration includes: brokering, VM Start, HDX Connection, Authentication, GPOs, Profile Load, and so on.

[![CAS-9](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_009.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_009.png)

The preceding diagram shows the UX score compilation by getting information from an end user’s latency, logon duration, failures, and reconnections. It allows the administrator to drill down further to find the root cause of the issues that users are experiencing. Performance analytics is available for both on-premises and cloud-based Citrix Virtual Apps and Desktops environments.

To learn more about user experience, refer to the following [link](/en-us/citrix-analytics/performance-analytics/user-analytics.html#what-is-user-experience-analytics).

## Operations Analytics

Operations Analytics is a more specific term oriented to business analytics that focuses on improving existing operations. Operations Analytics involves the use of various data aggregation to get more transparent information for day to day IT services. Citrix infrastructure consists of multiple products that are an amalgamation of different sets of workloads, and the administrator needs to have insight into the environment. Also, many administrators fail to focus on improving and optimization of the Citrix environment.

To overcome such problems, Citrix has embedded Operations Analytics into an analytics solution. The analytics solution contains machine learning algorithms and delivers actionable insights into the operational data of customers Citrix environments.

For example, if an enterprise is using the Citrix Secure Workspace Access service, admins can use the operations dashboards to get insights into user operations and application operation data. The administrator has thorough visibility of the data consumption (download & upload), domains accessed, and other available metrics according to the data sources. These metrics help in procuring and providing resources and quickly responding to any operations issues.

In another way, operations analytics support the idea of enterprise resource planning. Operation Analytics aggregates information to enhance the proactive approach towards resource management.

[![CAS-10](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_010.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_010.png)

The preceding diagram depicts Operations Analytics. It has two dashboards that are:

User Operations: Provides an overview of the user operations data based on transactions and data usage volume.

App Operations: Provides an overview of app operations data based on domains, categories, and download volume.

Reference: [Operations Analytics](/en-us/citrix-analytics/operations-analytics.html)

## Citrix Analytics integration with Citrix Products

Citrix Analytics integrated with the other Citrix components labeled “data sources.” The following products supported by Citrix Analytics service and provides insights about user behavior in the Citrix environment.

*  Citrix Secure Workspace Access
*  Citrix Content Collaboration
*  Citrix Endpoint Management
*  Citrix Gateway
*  Citrix Virtual Apps and Desktops

In this section, the integration of Citrix Analytics with other Citrix products discussed.

### Citrix Analytics and Citrix Secure Workspace Access

Citrix Secure Workspace Access service enables the administrators to provide a cohesive experience integrating single sign-on, remote access, and content inspection into a unique solution for end-to-end Secure Workspace Access. Administrators can protect the organization’s network and end-user devices from malware and data leaks by filtering access to specific websites and website categories.

Citrix Secure Workspace Access and Citrix Analytics solutions gives clear insights into user behavior and monitors the entire network. The inbuilt capability of Citrix Analytics that uses machine learning helps to take corrective actions. Citrix Analytics uses similar metrics and collected by the Secure Workspace Access service. The parameters of activities of users, such as websites visited, and the bandwidth spent. It also detects malware and phishing sites.

[![CAS-11](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_011.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_011.png)

The preceding diagram depicts a holistic view of Citrix Secure Workspace Access integration with the Citrix Analytics service. Citrix Secure Workspace Access and Citrix Analytics Services are hosted on Citrix Cloud. Data processing can be enabled by administrator with few clicks on the Analytics UI. Then Citrix Analytics starts capturing the data from Secure Workspace Access.

Security Analytics

The following are policies that an administrator can create to take actions based on a user’s activity.

*  Attempt to Access Blacklisted URL: Policy that can enable to indicate when a user attempts to access a blacklisted URL

*  Risky Website Access: This policy suggests that the user tried to access malicious, suspicious, or unsafe websites with high reputation ratings. (URL reputation rating is given for websites. The values range from 1 to 4. 4 is the riskiest website, and 1 is a clean site

*  Unusual Download Volume: The volume of data downloaded by the user from an application or websites has exceeded the threshold defined implicitly by Citrix Analytics

*  Unusual Upload Volume: The amount of data uploaded by the user from an app or sites has surpassed the limit set implicitly by Citrix Analytics

To learn more about Citrix Analytics integration with Citrix Secure Workspace Access, refer to the following [link](/en-us/citrix-secure-workspace-access/monitor-user-activity-and-manage-settings-with-analytics.html).

### Citrix Analytics and Citrix Content Collaboration

Citrix Content Collaboration enables us to quickly and securely exchange documents, send extensive material by email, and safely handle document transfers to third parties. In the Citrix Workspace environment, the user can access all of their files from the Citrix Workspace app. To track the user’s behavior and activity, it is cumbersome. In a small scale or large-scale Citrix environment, threats hidden behind the screen are tough to discern and take precautionary measures.

Citrix Content Collaboration can be monitored and troubleshot with the help of Citrix Analytics. Citrix Content Collaboration service hosted on Citrix Cloud nimbly integrates with Analytics service by enabling the data sources.

[![CAS-12](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_012.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_012.png)

The preceding diagram depicts the integration of Citrix Content Collaboration and Citrix Analytics service. Security Analytics as part Analytics service support monitoring and troubleshooting of user behavior and activities.

Security Analytics

The following are policies that an administrator can create to take actions based on the user’s activity.

*  Excessive file downloads: This policy indicates an attempt to download data that exceeds the AI-based threshold for this period

*  Excessive file/folder deletion: This policy indicates an attempt to delete a disproportionate number of files or folders, which exceed the AI-based threshold for the user

*  Excessive file sharing: This policy indicates that the share links created and shared have exceeded the threshold set by the machine learning algorithms for this period

*  Excessive file uploads: This policy indicates an attempt to upload data that exceeds the AI-based threshold for this period

*  Excessive login failures: the user makes multiple failed login attempts

*  Ransomware activity suspected (File Replaced): This policy indicates an effort to replace existing files with encrypted versions, resembling a ransomware attack

*  Excessive access to sensitive files (DLP alert): indicates an attempt to access files deemed confidential more than the threshold defined in the Citrix Content Collaboration Data Loss Prevention (DLP) policy

*  Unusual login access: This indicator is triggered when the user has suspicious access to Citrix Content Collaboration account identified by the Analytics AI engine, based on user’s usage locations and behavior patterns

*  Ransomware activity suspected (Files Updated): This policy indicates an attempt to update existing files with encrypted versions, resembling a ransomware attack

In case any unusual behavior or suspicious activity detected by Citrix Analytics service, then it can protect the data by disabling the user and expire all links.

### Citrix Analytics and Citrix Endpoint Management

Citrix Endpoint Management is a solution for managing endpoints, offering mobile device management (MDM) and mobile application management (MAM) capabilities. With Endpoint Management, the administrator can manage device and app policies and deliver apps to users.

Usually, the end-user might change device or install one of the blacklisted apps. Such incidents trigger an alarm in the Endpoint Management environments. Most of the time, it may go unnoticed to Citrix administrators or the possibility of manual errors.

In such cases, when an administrator has to manage thousands of endpoints, it becomes a burden. This process tends to increase much working hours managing the devices. To overcome these problems, Citrix Endpoints Management can be integrated with Citrix Analytics service by the administrator. That helps in detection of unmanaged device, blacklisted app installed device detection, and applying actions on such device can be automated.

[![CAS-13](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_013.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_013.png)

The preceding diagram depicts Citrix Analytics integration with Citrix Endpoint Management (Citrix Cloud service). From the Analytics UI with just a few clicks, the Endpoint Management service can be monitored and detect any suspicious activities on endpoints. This monitoring service starts on any device Android, iOS, managed, or unmanaged, so that the administrator will have a clear view of devices such as jailbroken, blacklisted apps.

Security Analytics

The following policies that an administrator can create to take actions based on the devices and applications.

*  A device with blacklisted apps detected: This policy indicates the detection of a device with blacklisted applications on the network using Citrix Endpoint Management service

*  Jailbroken or rooted device detected: This policy indicates the detection of a jailbroken or rooted device on the network using Citrix Endpoint Management service

*  Unmanaged device detected: This policy indicates that an unmanaged device discovered by Citrix Endpoint Management service in your network

When any of the conditions are met, the administrator has an option to give a notification to the administrator and user. The administrator can also lock that particular device.

### Citrix Analytics and Citrix Gateway

Citrix Gateway service provides secure remote access solutions with a diverse Identity and Access Management (IdAM) capabilities. Gateway service helps in delivering a unified experience into SaaS apps, heterogeneous Virtual Apps and Desktops, and so forth.

Citrix Gateway’s End Point Analysis (EPA) scan policies help to detect user access-based threats and reports in the UI. Similarly, Citrix Gateway detects all the user logon failures, which can be primary, secondary, or tertiary authentication failures. Also, authorization failures distinguished from the Citrix Gateway.

Citrix Gateway metrics on user logon activity are collected by Citrix Analytics to perform an automated task that would not be practical for an administrator to do manually.

[![CAS-14](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_014.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_014.png)

The preceding diagram depicts Citrix Analytics integration with Citrix Gateway Service. The administrator has to enable data processing so that Citrix Analytics starts capturing the data from the Gateway Service.

Security Analytics

The following policies can be created by the administrator to automate the monitoring activity.

*  Authorization failures: This policy indicates that the Analytics AI engine identified that the user has attempted to access a resource without sufficient permissions

*  EPA scan failures: This policy shows an attempt to access the network using a device that has failed Citrix Gateway’s End Point Analysis (EPA) scan before or after authentication

*  Logon failures: This policy indicates that the Analytics AI engine identified multiple primary authentication failures based on the user’s usage and behavior patterns

*  Unusual login access: This policy suggests an attempt to logon to Citrix Gateway from significant locations that deviated from the user’s access pattern

With the help of Citrix Analytics, when any of the conditions crop up, then the administrator can take action on that user by enabling “Log off user” from the Analytics UI. Else administrators can examine that user’s activity with the help of selecting “Notify administrator.”

## Citrix Analytics and Citrix Virtual Apps and Desktops

Citrix Virtual Apps and Desktops are virtualization solutions that give IT control of virtual machines, applications, licensing, and security, while providing anywhere access for any device. Citrix Virtual Apps and Desktops allow end-users to run applications and desktops independently of the device’s operating system and interface. Similarly, administrators have the advantage of managing the network and control access from selected devices or all devices.

Citrix Analytics for Virtual Apps and Desktops framework gives an insight into user activities. Based on user behavior, alarm, or notifications are triggered when the user’s behavior deviates from the normal behavior. App-based risk indicators are triggered when users attempt to access an unauthorized application over a specific period. Data based risk indicators are generated for unusual data upload and download with a large volume.

[![CAS-15](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_015.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_015.png)

The preceding diagram depicts Citrix Analytics integration with Citrix Virtual Apps and Desktops. Based on the user's attempt to access resources with a device that has unsupported operating systems, new equipment, an alarm triggered with notification. The inbuilt machine learning platform keeps feeding the latest update into its database, and profiles are updated. In the future, any anomaly in the environment can be easily detected and mitigated without admin intervention.

Security Analytics

The following policies can be created by the administrator to automate the monitoring activity.

*  Potential Data Exfiltration: This policy indicates an attempt to exfiltrate data from Citrix Workspace to an external device or location

*  Access from New Device(s): This policy indicates an attempt to log on to Citrix Workspace from a new device

*  Access from a device with unsupported OS: This policy indicates an attempt to access the network launching receiver from a device with an unsupported OS version or unsupported browser version

*  Unusual Application usage (SaaS): This policy indicates that the user used applications at achieved times that deviate from the user’s usage and behavior patterns identified by Citrix Analytics

*  Unusual Application usage (Virtual): This policy indicates that the Analytics AI engine identified usage of applications at great times based on the user’s usage and behavior patterns

Citrix Analytics can perform actions on the user’s account when the preceding conditions are encountered. During the policy creation the administrator can enable the action when any conditions are met.

Reference: [CVAD Risk Indicators](/en-us/citrix-analytics/security-analytics/risk-indicators/citrix-virtual-apps-and-desktops-risk-indicators.html)

Performance Analytics

Performance Analytics gives an insight into user session details in the Citrix environment. The data collected by Citrix Analytics helps to monitor and troubleshoot issues that arise during a user’s login session.

The following are metrics that Citrix Performance Analytics provides to the administrator.

*  User Experience: This offering provides insights into the user and session performance parameters — a profound view of all sites in the organization within a concise dashboard. The user experience score helps to segregate the users based on their experience namely Excellent, Fair, or Weak

*  User Sessions: The User Session section of the User Experience dashboard displays an important session metric for the chosen period and Site. The administrator can view Total Sessions, Total unique users, and Session failures data

*  Session Responsiveness: This data represents the ICA Round Trip time. The information used to quantify user experience. The Session Responsiveness has Active Session, Session classification, and Session classification trend. If any session is facing network issues, the session responsiveness trend data helps to identify such problems in the network

*  Session Logon Duration: Logon duration is the app or desktop availability after the user clicks in the Citrix Workspace app. Total logon time includes phases such as Brokering, VM Start, HDX Connection, Authentication, Profile Load, Logon Script, GPO, and Shell Launch. This section has Total logons, Session classification, and sorted by Delivery Groups

Reference: [Performance Analytics](/en-us/citrix-analytics/performance-analytics/user-analytics.html#user-experience-score)

## Citrix Analytics and on-premises Citrix Virtual Apps and Desktops

Many organizations having on-premises Citrix Virtual Apps and Desktops environments can take advantage of the Citrix Analytics Cloud Service. Citrix Analytics supports and discovers data sources automatically.

To enable Analytics on Virtual Apps and Desktops Sites administrator can adopt one of the following methods to onboard on-premises Virtual Apps and Desktops Sites to Citrix Analytics:

*  Onboard Sites using Workspace
*  Onboard Sites using StoreFront

[![CAS-16](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_016.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-analytics_016.png)

### Onboard Virtual Apps and Desktops Sites using Workspace

Citrix Analytics automatically discovers the sites once added to Citrix Workspace. The Virtual Apps and Desktops site card displays the number of discovered Sites and users. To add Sites in Citrix Cloud the customer needs to have a Workspace subscription. The administrator has to do Site aggregation before proceeding with onboarding on Citrix Analytics.

The administrator has to turn on data processing on the Site card. Once the Citrix Analytics start receiving the events, collected data can be viewed based on the selected time. A policy agent installed to configure the policies and this step is not associated with data transmission from the data sources. To know more about policy agent and installation, refer to the following [link](/en-us/citrix-analytics/getting-started/virtual-apps-desktops-data-source.html#onboard-virtual-apps-and-desktops-sites-using-workspace).

### Onboard Virtual Apps and Desktops Sites using StoreFront

Another method that StoreFront can become a data source to Citrix Analytics is by aggregating applications and desktops from Citrix Virtual Apps and Desktops Sites into a single store for users. User events captured by Citrix Analytics process through to get actionable insights into user behaviors. There are a few prerequisites that the administrator has to configure before enabling this method. On the networking front, StoreFront deployments must have TCP port 443 open for outbound internet connection. In case environment uses proxy servers on the network, administrator has to allow a particular port of communication.

From the Citrix Analytics service, the administrator has to connect to an on-premises StoreFront deployment. To enable this feature, import the configuration settings. That makes Citrix Analytics receive the data from StoreFront.

To learn more on Citrix Virtual Apps and Desktops as a data source, refer to the following [link](/en-us/citrix-analytics/getting-started/virtual-apps-desktops-data-source.html#onboard-virtual-apps-and-desktops-sites-using-workspace).

Onboarding Citrix Performance Analytics

Performance Analytics is a comprehensive monitoring solution. Performance analytics helps in monitoring and viewing the usage of Citrix Virtual Apps and Desktops Sites in the organization. To enable these capabilities, the administrator has to configure on-premises sites with Citrix Analytics from the Citrix Director console. This feature requires Director version 1909 or later, Delivery Controller and VDA version 1906 or later.

Reference: [Configuring on-premises Sites with Citrix Analytics for performance](/en-us/citrix-virtual-apps-desktops/director/install-and-configure/onboarding.html)

## Key Benefits of Citrix Analytics

Citrix Analytics is an intuitive analytics service that allows administrators to monitor and identify inconsistent or suspicious activity on the networks. A turn-key machine learning platform that provides actionable insights into user behavior based on indicators across users, endpoints, network traffic, and files.

An organization can solve business challenges and performance-related issues by adopting analytics solutions built-on a machine learning platform. There are various benefits that organizations and administrators can realize by selecting Analytics Service in Citrix brownfield deployment or green field deployment.

*  Detect and analyze threats based on user behavior
Detect and prevent ransomware attacks by taking security measures. Monitor and analyze user access, and authentication behavior helps to stop malicious activity and prevent any data loss. Automated platform with the help of ML and AI algorithms, any anomalies are detected

*  Health visibility
Advanced analytics helps to identify issues proactively. Application Performance Analytics distills app data and many real-time performance metrics into a single-digit App Score that shows responsiveness of an app. Admins and app owners can drill down into the infrastructure associated with specific apps to troubleshoot issues

*  Tighter external security
Citrix Analytics generates a threat index based on violation type, rate of attack, location, and client details

*  Improved internal security
Citrix Analytics collects a wide variety of data from many sources surrounding user activity, access behaviors, and network and data usage patterns. The automated machine learning platform helps to mitigate any unusual occurrences. Information on UI helps administrators to understand the activities/events on the environment

*  Central management and automation
Citrix Analytics collects different metrics from multiple resources, that help an administrator to monitor all Citrix portfolio products from a single UI

*  Cloud service
Citrix Analytics is a SaaS offering from Citrix Cloud that has security, performance, and the operational dashboard. The UI provides a summary and categorization of the user risk profiles, user experience details, and so on in the Citrix environment

## Summary

Many of the global security challenges are not addressed by existing traditional threat protection tools alone. Most of the attacks come from external actors that possibly defended, but strikes from within the perimeter are even more menacing. While defending external threats, many organizations fail to recognize the impact on performance and identifying the root cause.

Citrix Analytics that comes with built-in machine learning enhanced with artificial intelligence holds great promise in addressing many security challenges in a Citrix environment. Citrix Analytics is not just about the security of a Citrix environment. It gives visibility into performance with user experience score and user operations visibility in terms of domains visited, data consumption, and so on. Integration of Citrix Analytics in green field or brownfield deployments helps the administrator to have greater control of the Citrix environment and reduce IT costs.

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s19562044ed447c2b).

## References

The following resources referenced for a better understanding of Citrix Analytics:

[Citrix Analytics](/en-us/citrix-analytics)

[Security Analytics](/en-us/citrix-analytics/security-analytics.html)

[Operations Analytics](/en-us/citrix-analytics/operations-analytics.html)

[FAQ](/en-us/citrix-analytics/faqs.html)
