---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Sameer Mehta, Mathew Varghese, Blake Connell
description: Provide visibility into your environment to protect it from malicious users and to improve the end user experience proactively.
tz_title: Analytics
tz_products: citrix-analytics;
---
# Tech Brief: Analytics

Organizations’ IT environments are becoming more complex as they begin to adopt SaaS, cloud, and mobile applications. Administrators need the visibility into their environment not only to protect it from malicious users, but also to improve the user experience proactively. Citrix Analytics pulls together the entire Citrix portfolio to provide visibility into the status and context of individual users. Unlike some of the other monitoring tools that Citrix provides, Citrix Analytics gives you **proactive and prescriptive** insights into your environment to resolve issues before they become a problem. Citrix Analytics is driven through machine-learning to provide you with the necessary insights without information overload.

## Overview

Citrix Analytics generates actionable insights, enabling administrators to proactively handle user and application security threats, improve app performance, and support continuous operations. Citrix Analytics is available as a cloud service delivered through Citrix Cloud. Citrix Analytics can be broken up into three categories: Security, Performance, and Usage. Citrix Analytics for Security allows you to monitor and identify inconsistent or suspicious activity within your environment. Usage Analytics gives you visibility to how users interact with various Citrix products. Citrix Analytics for Performance provides user-centric experience scores, application, and infrastructure performance scores through advanced analytics.

## Citrix Analytics for Security

Citrix Analytics for Security collects data across Citrix and third-party products and generates actionable insights. It supports integration with the following:

-  Citrix Secure Private Access
-  Citrix Content Collaboration
-  Citrix Endpoint Management
-  Citrix Gateway
-  Citrix Virtual Apps and Desktops (both on-premises and as a service)
-  Citrix Secure Browser Service
-  Microsoft Graph Security API
-  Microsoft Active Directory (on-prem)

Citrix Analytics for Security detects anomalous user behavior through its machine learning μ-service. It assigns users a risk score, a value that indicates the aggregate level of risk a user poses through its risk scoring μ-service. This score is a dynamic value that is based on User Behavior Analytics (UBA). Administrators can create policies to automate processes and apply actions based on risk indicators. Citrix Analytics for Security retains data for 13 months. If the administrator turns off data processing for a specific data source, the data that was already captured remains stored for 13 months. More information on what specific logs per data source are collected [here](/en-us/security-analytics/data-governance.html).

Citrix Analytics for Security receives the information in the following manner. For the Citrix Secure Private Access service, Citrix Content Collaboration, Citrix Endpoint Management, and Citrix Gateway Service (cloud) it receives its information directly from the control plane of the specific data source. For Citrix Gateway on-prem, it receives the data from the Application Delivery Management agent. For Citrix Virtual Apps and Desktop (cloud & on-prem) it receives its information through Citrix Workspace app. To get active directory data, Citrix Analytics communicates with the Cloud connectors. For Microsoft Graph security, we can get information from Azure AD identity protection and Windows Defender ATP through graph APIs.

To get started you must have a Citrix Cloud account. Once, you have access to Citrix Cloud you can request access to a Citrix Analytics for Security trial. Then have the options to enable data processing and beginning receiving information. An in-depth guide on how to get started can be found [here](/en-us/security-analytics/getting-started-security.html).

[![Architecture High Level](/en-us/tech-zone/learn/media/tech-briefs_analytics_architecture-high-level.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_architecture-high-level.png)

### Users Dashboard

 The users dashboard allows you to get a holistic view of any users that are deemed risky within your organization. The users are categorized between high, medium, and low risk users. Administrators can change views from users with the highest scores, highest score change users, risk indicator users, or risk indicator change. Also, it shows the risk categories—essentially giving you a comprehensive list of risk exposure and what requires immediate attention. More information on the user dashboard can be found [here](/en-us/security-analytics/users-dashboard.html).

[![User Dashboard](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-dashboard.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-dashboard.png)

 With all of these dashboards, you can click and get more granular information. For example, if you click *see more* under the **Risk Categories dashboard**, you get a summary of risk indicator occurrences under each category.

[![Risk Report](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-report.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-report.png)

Also, if under the risky users dashboard, you click a specific user—it will redirect you to the User risk timeline. This timeline allows you to gain deeper insights into what actions the user has done that are risky. You will also see if any automated actions were taken against that specific user. By clicking each event, you can get additional information as to when an event happened and where the source of that event is. Within the user risk dashboard, you can find user information such as AD information (phone, email, title) and information on what application, devices, and locations they are using. More information on the risk timeline can be found [here](/en-us/security-analytics/risk-timeline.html).

[![Risk Timeline](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-timeline.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-timeline.png)

The risk scores are calculated through policy-based violations (set by administrators), user behavior modeling over time, AI/ML anomaly behavior detection, and peer group normalization.  The risk scores are values that indicate the aggregate level of risk a user poses. The risk indicators are user activities that look suspicious or can pose a security threat to your organization. There are default risk indicators that are used by the system, but administrator can create custom risk indicators as well.

[![Risk Score](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-score.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_risk-score.png)

### Policies and Actions

Policies are defined so once a condition is met, the action is run. A policy contains one or more conditions, and a single action. There are default policies available—these policies have pre-defined conditions and have a corresponding action. These default policies can be used as is or modified based on your requirements. The default policies are the following:

-  Successful credential exploit
-  Potential data exfiltration
-  Unusual access from a suspicious IP
-  Unusual app access from an unusual location
-  Low risk user—first time access from a new IP
-  First time access from device

Actions are the responses to the suspicious events that prevent future anomalous events from occurring. Actions can be invoked at will by the Citrix Analytics administrator or automatically by the system based on rules defined by the administrator.
Currently the following actions are available:

-  Global
    -  Request End User response
    -  Add to watchlist
    -  Notify administrator(s)
    -  Remove from watchlist
-  Citrix Content Collaboration
    -  Disable user
    -  Expire all links
-  Citrix Virtual Apps and Desktops
    -  Log off user
    -  Start session recording
    -  Stop session recording
-  Citrix Endpoint Management
    -  Lock device
    -  Notify admin
    -  Notify User

Currently the following conditions are available when creating a policy:

-  Risk score
    -  Risk score (equals, is greater than, is less than)
-  Citrix Gateway
    -  Excessive authorization failures
    -  EPA scan failure
    -  Excessive authentication failures
    -  Logon from suspicious IP
    -  Access from an unusual location
    -  First time access from a new IP
-  Citrix Virtual Apps and Desktops
    -  First time access from a new device
    -  Potential data exfiltration
    -  Access from device with unsupported OS
    -  Unusual time of application usage (SaaS)
    -  Unusual time of application usage (virtual)
-  Citrix Content Collaboration
    -  Excessive file downloads
    -  Excessive file/folder deletion
    -  Excessive file sharing
    -  Excessive file uploads
    -  Excessive authentication failures
    -  Ransomware Activity Suspected (Files Replaced)
    -  Ransomware Activity Suspected (DLP alert)
    -  Excessive downloads
    -  First time access from a new location
    -  Ransomware activity suspected (files updated)
-  Citrix Secure Private Access
    -  Attempt to Access Blacklisted URL
    -  Risky website access
    -  Excessive data download
    -  Unusual Upload Volume
-  Citrix Endpoint Management
    -  Device with blacklisted apps detected
    -  Jailbroken/rooted device detected
    -  Unmanaged device detected

[![Policy](/en-us/tech-zone/learn/media/tech-briefs_analytics_policy.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_policy.png)

More information on how to set up policies and actions can be found [here](/en-us/security-analytics/policies-and-actions.html).

### User Access Dashboard

The User Access dashboard summarizes the number of risky domains accessed and the volume of data uploaded and downloaded by the users in your network. It provides the following metrics:

-  Number of malicious domains accessed by the users
-  Number of dangerous domains accessed by the users
-  Number of unknown domains accessed by the users
-  Number of clean domains accessed by the users
-  Number of blocked URLs accessed by the users

[![User Access](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-access.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-access.png)

### App Access Dashboard

The App Access dashboard summarizes the details of domains, URLs, and apps accessed by the users in your network. The **App Access Summary** section provides an overview of the following metrics in your network:

-  Number of malicious domains accessed by users
-  Number of dangerous domains accessed by users
-  Number of unknown domains accessed by users
-  Number of clean domains accessed by users
-  Volume of data uploaded or downloaded from the risky domains

[![App Access](/en-us/tech-zone/learn/media/tech-briefs_analytics_app-access.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_app-access.png)

### Shared Links Dashboard

The Share Links dashboard is the launching point into share event analysis and threat prevention. It shows the visibility into the share link’s patterns across an organization.

[![Share Links](/en-us/tech-zone/learn/media/tech-briefs_analytics_share-links.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_share-links.png)

### Reporting

Administrators can create custom reports from the events received in your data sources. Currently, the supported data sources for custom reports include Secure Private Access, Content Collaboration, and Virtual Apps and Desktops. More information on how to create custom reports can be found [here](/en-us/security-analytics/create-custom-report.html).

[![Report](/en-us/tech-zone/learn/media/tech-briefs_analytics_report.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_report.png)

## Citrix Analytics for Performance

Citrix Analytics for Performance quantifies user experience and gives customers end-to-end visibility on what the root cause for end user experience is. It also provides multi-site aggregation and reporting so customers who have multiple sites can consume data from a single pane of glass instead of having to log into multiple Director consoles. Finally, it provides the infrastructure performance score to give administrators a cohesive view of their infrastructure health.

The user experience score is calculated by considering different factors affecting the end user experience such as: session resiliency, session availability, session logon duration, and session responsiveness. Administrators are then able to divide deeper and look at subfactors to be able to determine the exact root cause of the problem. For example, sub factors for session logon duration include GPOs, Profile Load, Interactive Session, Brokering, VM Start, HDX Connection, Authentication, and Logon Scripts. Dynamic thresholds are used to benchmark the Session Logon Duration and the Session Responsiveness factors and subfactors. These calculations are done on a per customer basis and calculated based on the past 30 days. The thresholds are recalibrated every seven days to reflect changes made in the environment. More information on how the user experience score is calculated can be found [here](/en-us/performance-analytics/user-analytics/ux-score.html).

[![UX Score](/en-us/tech-zone/learn/media/tech-briefs_analytics_ux-score.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_ux-score.png)

Citrix Analytics for Performance can be used for both on-prem and cloud customers and does not require customers to be on Citrix Workspace. Citrix Analytics gets data directly from the Director’s monitoring data base. The data is pushed securely from Director to Citrix Analytics through https port 443. Citrix Analytics for Performance also captures HDX data from Citrix Gateway. For an on-prem gateway, a customer is required to use the ADM service. For gateway service, HDX data is sent directly to Citrix Analytics. There is no data going from Citrix Cloud to your on-prem environment. Data communication is outbound which means that no ports need to be open or any inbound traffic allowed. For the customer on Citrix Virtual Apps & Desktop Service, Citrix Analytics gets data directly from the Director platform—all of which is hosted within the Citrix Cloud.

[![CASP Architecture](/en-us/tech-zone/learn/media/tech-briefs_analytics_casp-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_casp-architecture.png)

### User Experience Score Dashboard

The User Experience Score dashboard gives a holistic view of which users are experiencing “excellent”, “fair”, or “poor” experience. Citrix Analytics for Performance has multi-site aggregation to give you a holistic view of all of your environments (cloud or on-prem). Multi-site aggregation gives the administrator the flexibility to look at their environment holistically or filter out per a specific site.

[![UX Dashboard](/en-us/tech-zone/learn/media/tech-briefs_analytics_ux-dashboard.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_ux-dashboard.png)

Citrix Analytics administrators can drill down to see which factors are causing the user to get that specific end user experience score. Citrix Analytics for Performance provides administrators insights into possible root causes of what can be causing the user experience issue. More information on the user experience subfactors can be found [here](/en-us/performance-analytics/user-analytics/intermediate-drilldown.html).

[![Subfactors](/en-us/tech-zone/learn/media/tech-briefs_analytics_subfactors.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_subfactors.png)

Also, within this User Experience dashboard, the administrators are able to see the User session trends which show the total sessions vs the total unique users and the number of session failures. The total sessions indicate the total number of user sessions when an app or a desktop is launched from Workspace App. The total unique users are the number of unique users who have launched a session or have an active session during the specified time period.

[![User Sessions](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-sessions.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_user-sessions.png)

### Infrastructure Dashboard

The infrastructure dashboard provides administrators with an overview of their environment’s infrastructure health. The dashboard provides the VDA information across all sites. For multi-session OS VDAs, administrators are able to see which VDAs are in unusable state based on the load evaluator index. For single-session OS VDAs, administrators can see the number of VDAs that are in use and available. More information on the metrics available in the Infrastructure dashboard can be found [here](/en-us/performance-analytics/infrastructure-analytics.html).

[![Infrastructure](/en-us/tech-zone/learn/media/tech-briefs_analytics_infrastructure.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_infrastructure.png)

## Usage Analytics

Usage Analytics provides insights into how users interact with various Citrix products to help understand user adoption and engagement. Usage analytics currently supports Secure Private Access service, Content Collaboration Service, and Microapps Service.

### Content Collaboration Dashboard

The Content Collaboration dashboard provides the following information:

-  Number of unique users using the Content Collaboration service
-  Top Content Collaboration users
-  Amount of data uploaded to the Content Collaboration service
-  Amount of data downloaded to the Content Collaboration service
-  Number of files uploaded to the Content Collaboration service
-  Number of files downloaded to the Content Collaboration service
-  Number of actions performed by users on the files
-  Number of share events created by the user

[![Usage](/en-us/tech-zone/learn/media/tech-briefs_analytics_usage.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_usage.png)

### Microapps Dashboard

The Microapp dashboard provides insights into how users are interacting with the Microapp service. The following information can be found in the dashboard:

-  Number of unique users using the microapps
-  Top microapp users
-  Most used microapps
-  Number of actions initiated by the users using the microapps
-  Number of actions taken by the users on the notifications delivered by the microapps

[![Microapps](/en-us/tech-zone/learn/media/tech-briefs_analytics_microapps.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_microapps.png)

### SaaS and Web Apps Dashboard

The SaaS and Web Apps dashboard provides Citrix Analytics administrators insight into SaaS and web apps published in Citrix Workspace. The following information can be found within the SaaS and Web Apps Dashboard:

-  Number of unique users using the SaaS and Web applications
-  Top SaaS and Web application users
-  Number of SaaS and Web applications launched
-  Top SaaS and Web applications
-  Top domains accessed by the users
-  Total amount of data uploaded and downloaded across users, applications, and domains.

[![SaaS](/en-us/tech-zone/learn/media/tech-briefs_analytics_saas.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_saas.png)

## Architecture and Process Flow

Below you can find the conceptual architecture and process flow of Citrix Analytics.

[![Analytics Process Flow](/en-us/tech-zone/learn/media/tech-briefs_analytics_process-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_process-flow.png)
