---
layout: doc
description: Provide visibility into your environment to protect it from malicious users and to improve the end user experience proactively.
---
# Citrix Analytics

## Contributors

**Author:** [Ana Ruiz](https://twitter.com/mobileruiz)

**Special Thanks:** Sameer Mehta & Mathew Varghese

Organizations’ IT environments are becoming more complex as they begin to adopt SaaS, cloud, and mobile applications. Administrators need the visibility into their environment not only to protect it from malicious users, but also to improve the user experience proactively. Citrix Analytics pulls together the entire Citrix portfolio to provide visibility into the status and context of individual users.

## Overview

Citrix Analytics generates actionable insights, enabling administrators to proactively handle user and application security threats, improve app performance, and support continuous operations. Citrix Analytics is available as a cloud service delivered through Citrix Cloud. Citrix Analytics can be broken up into three categories: Security, Performance, and Operations. Security Analytics allows you to monitor and identify inconsistent or suspicious activity within your environment. Operations Analytics gives you visibility to the domains your users are accessing within your environment. Performance Analytics provides user-centric experience scores, application, and infrastructure performance scores through advanced analytics.

## Security Analytics

Citrix Security Analytics collects data across Citrix and third-party products and generates actionable insights. It supports integration with the following:

-  Citrix Access Control
-  Citrix Content Collaboration
-  Citrix Endpoint Management
-  Citrix Gateway
-  Citrix Virtual Apps and Desktops
-  Microsoft Graph Security
-  Microsoft Active Directory (on-prem)

Security analytics detects anomalous user behavior through its machine learning μ-service. It assigns users a risk score, a value that indicates the aggregate level of risk a user poses through its risk scoring μ-service. This is a dynamic value that is based on User Behavior Analytics (UBA). Administrators have the ability to create policies to automate processes and apply actions based on risk indicators. Security Analytics retains data for 13 months. If the administrator turns off data processing for a specific data source, the data that was already captured remains stored for 13 months.

Security Analytics receives the information in the following manner. For Citrix Access Control service, Citrix Content Collaboration, Citrix Endpoint Management, and Citrix Gateway Service (cloud) it receives its information directly from the control plane of the specific data source. For Citrix Gateway on-prem, it receives the data from the Application Delivery Management agent. For Citrix Virtual Apps and Desktop (cloud & on-prem) it receives its information through Citrix Workspace app. To get active directory data, Citrix Analytics communicates with the Cloud connectors. For Microsoft Graph security, we can get information from Azure AD identity protection and Windows Defender ATP through graph APIs.

## Performance Analytics

Performance Analytics quantifies user experience and gives customers end-to-end visibility on what the root cause for end user experience is. It also provides multi-site aggregation and reporting so customers who have multiple sites can consume data from a single pane of glass instead of having to log into multiple Director consoles. Finally, it provides infrastructure performance score to give administrators a cohesive view of their infrastructure health.

The user experience score is calculated by considering different factors affecting the end user experience such as: latency, logon duration, failures, and reconnections. Administrators are then able to divide deeper and look at subfactors to be able to determine the exact root cause of the problem. For example, sub factors for logon duration include: GPOs, Profile Load, Interactive Session, Brokering, VM Start, HDX Connection, Authentication, and Logon Scripts.

Performance Analytics can be used for both on-prem as well as cloud customers and does not require customers to be on Citrix Workspace. Citrix Analytics gets data directly from Director’s monitoring data base. The data is pushed securely from Director to Citrix Analytics through https port 443. Performance Analytics also captures HDX data from Citrix Gateway. For on-prem gateway, a customer is required to use the ADM service. For gateway service, HDX data is sent directly to Citrix Analytics. There is no data going from Citrix Cloud to your on-prem environment. Data communication is outbound which means that no ports need to be open or any inbound traffic allowed. For customer on Citrix Virtual Apps & Desktop Service, Citrix Analytics gets data directly from Director—all of which is hosted within Citrix Cloud.

## Operations Analytics

Operations Analytics provides an overview of the total number of domains that have been accessed in your network. This can be either filtered through the user operations dashboard or the app operations dashboard. The user dashboard shows the data based on which users have the top number of transactions and data downloaded. Whereas the app dashboard shows the top domains that have been accessed and the top domains based on data downloaded. Citrix Analytics is getting this information directly from Citrix Access Control Service.

## Architecture and Process Flow

Below you can find the conceptual architecture and process flow of Citrix Analytics.

[![Analytics Process Flow](/en-us/tech-zone/learn/media/tech-briefs_analytics_process-flow.png)](/en-us/tech-zone/learn/media/tech-briefs_analytics_process-flow.png)

## Other Tech Zone content

[Learn -> Tech Insight -> Security Analytics](/en-us/tech-zone/learn/tech-insights/security-analytics.html) - Generate actionable insights about your environment, enabling administrators to proactively handle user and application security threats.

[Learn -> Tech Insight -> Performance Analytics](/en-us/tech-zone/learn/tech-insights/performance-analytics.html) - Gain visibility into your environment through user-centric experience scores, application & infrastructure performance scores with Performance Analytics.

[Design -> Reference Architectures -> Citrix Analytics](/en-us/tech-zone/design/reference-architectures/citrix-analytics.html) - Learn about analytics services offered by Citrix Cloud including security analytics, performance analytics, and integration with other Citrix portfolio products.
