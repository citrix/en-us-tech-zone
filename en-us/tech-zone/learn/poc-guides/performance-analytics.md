---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Allen Furmanski
description: Learn how to get started with Citrix Analytics for Performance.
tz_title: Proof of Concept-Performance Analytics
tz_products: citrix-analytics
---
# PoC Guide: Proof of Concept-Performance Analytics

## Overview

Citrix Analytics for Performance allows you to track, aggregate, and visualize key performance indicators of your Citrix Virtual Apps and Desktops environment. It quantifies user experience and gives customers end-to-end visibility on what the root cause for end user experience is. It also provides multi-site aggregation and reporting. Customers who have multiple sites can consume data from a single pane of glass instead of having to log into multiple consoles. More information on Citrix Analytics for Performance can be found [here](/en-us/tech-zone/learn/tech-briefs/analytics.html) and videos demonstrating the Citrix Analytics for Performance can be found [here](/en-us/tech-zone/learn/tech-insights/performance-analytics.html).

[![Citrix Performance Analytics Architecture](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_1.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_1.png)

## Pre-requisites

### On-premises Citrix Virtual Apps and Desktops Sites

-  Delivery Controller and Director 1909 or later
-  Citrix Cloud account with Citrix Analytics entitlements
    -  Your Citrix Cloud account is an Administrator account with rights to the Product Registration Experience.
-  Full details on pre-requisites can be found [here](/en-us/performance-analytics/onboarding.html)

### On-premises StoreFront

-  Your StoreFront version must be 1906 or later.
-  The StoreFront deployment must be able to connect to the following address:
    -  *https://*.cloud.com
    -  *https:// api.analytics.cloud.com
-  The StoreFront deployment must have port 443 open for outbound internet connections. Any proxy servers on the network must allow this communication with Citrix Analytics.

## Deployment Steps

### Connecting Citrix Analytics with on-premises Citrix Virtual Apps and Desktop sites

1.  Log into Director as a full administrator and pick which Site you want to configure with Performance Analytics
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_2a.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_2a.png)
2.  Click the Analytics tab
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_3.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_3.png)
3.  Select the terms and click *Get Started*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_4.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_4.png)
4.  Click *Connect Site*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_5.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_5.png)
5.  Click *Copy Code* and then click *Register on Citrix Cloud*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_6.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_6.png)
6.  Log in to Citrix Cloud
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_7.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_7.png)
7.  Paste the 8 digit code you copied in Director and click *continue*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_8.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_8.png)

### Connecting to on-premises StoreFront

1.  Log into Citrix Cloud and click *Manage* under the Analytics console from your StoreFront server
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_9.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_9.png)
2.  Click *Manage*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_10.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_10.png)
3.  Click on *settings* and then click on *data sources*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_11.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_11.png)
4.  Click the ellipses next to Virtual Apps and Desktops and select *Connect to StoreFront Deployment*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_12.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_12.png)
5.  Click *download file*
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_13.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_13.png)
6.  Open powershell and run the following command: Import-STFCasConfiguration -Path "configuration file path"
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_14.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_14.png)
7.  You will now be able to see that the StoreFront database has been added
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_15.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_15.png)

### Citrix Virtual Apps and Desktop Service

Citrix Analytics automatically detects Citrix Virtual Apps and Desktops service sites and no action by the administrator is needed.

## Relevant Data

Once data is flowing into the environment some things to keep in mind:

### Multi-site aggregation

Allows you to get insights into multi-sites regardless of whether they are hosted on-premises, cloud, or hybrid.
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_16.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_16.png)

### User-Experience Score

The User Experience score is a comprehensive measurement of the quality of the session established by the user. Click on the number of poor users
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_17.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_17.png)

User experience score are based on:

-  Session availability
-  Session responsiveness
-  Session logon duration
-  Session resiliency
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_18.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_18.png)

You can click on each factor to get corresponding subfactors and insights into what might be causing the issue and proactively resolve those issues.
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_19.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_19.png)

### Failure Insights

Failure Insights provide insights into the root causes for session failures in your environment.
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_20.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_20.png)

Click on the number of machines to see which machines are black hole machines
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_21.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_21.png)

Click on a specific machine to see further details on the black-hole machine
[![Citrix Performance Analytics](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_22.png)](/en-us/tech-zone/learn/media/poc-guides_performance-analytics_22.png)
