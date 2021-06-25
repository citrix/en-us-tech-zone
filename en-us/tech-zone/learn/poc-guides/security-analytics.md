---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: First Last, First2 Last2
description: Learn how to setup Citrix Analytics for Security.
tz_title: POC Guide Citrix Analytics for Security
tz_products: citrix-analytics;citrix-content-collaboration;citrix-endpoint-management;citrix-networking;citrix-secure-internet-access;citrix-secure-workspace-access;citrix-service-providers;citrix-virtual-apps-and-desktops-standard-for-azure;citrix-virtual-apps-and-desktops;citrix-workspace;google-cloud-platform;other;security;third-party-content
---
# PoC Guide: POC Guide Citrix Analytics for Security

## Overview

Citrix Analytics for Security continuously assesses the behavior of Citrix Virtual Apps and Desktops users and Citrix Workspace users and applies actions to protect sensitive corporate information. The aggregation and correlation of data across networks, virtualized applications and desktops, and content collaboration tools enables the generation of valuable insights and more focused actions to address user security threats. More information on Citrix Analytics for Security can be found [here](/en-us/tech-zone/learn/tech-briefs/analytics.html) and videos demonstrating the Citrix Analytics for Security can be found [here](/en-us/tech-zone/learn/tech-insights/security-analytics.html).

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_1.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_1.png)

## Pre-requistes

### On-premises Citrix Virtual Apps and Desktops Sites

-  Delivery Controller 7.16 or later
-  Director 7.16 or later
-  Citrix Cloud account with Citrix Analytics entitlements
-  If you are using StoreFront, StoreFront 1906 or later is required

## Deployment Steps

### Citrix Virtual Apps and Desktops on-premises using Workspace

#### Connecting to on-premises StoreFront

Log into Citrix Cloud and click *Manage* under the Analytics console from your StoreFront server

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_2.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_2.png)

Click **Manage**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_3.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_3.png)

Click **settings** and then click *data sources*

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_4.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_4.png)

Click the ellipses next to Virtual Apps and Desktops and select **Connect to StoreFront Deployment**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_5.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_5.png)

Click **download file**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_6.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_6.png)

Open powershell and run the following command: Import-STFCasConfiguration -Path "configuration file path"

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_7.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_7.png)

You can see that the StoreFront database has been added

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_8.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_8.png)

#### Connecting to on-premises sites using Workspace

Site needs to be added to Citrix Workspace using [Site Aggregation](/en-us/citrix-workspace/add-on-premises-site) beforehand

Log into Citrix Cloud from one of your delivery controllers

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_9.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_9.png)

Select **manage** under Security Analytics

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_10.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_10.png)

Select **Data sources** under **Settings**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_11.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_11.png)

click **Policy Incomplete** under Virtual Apps and Desktops

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_12.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_12.png)

click the drop down under your site name and then click **continue**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_13.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_13.png)

Select **download agent**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_14.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_14.png)

Complete the installation

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_15.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_15.png)

click **Connect to Installed Agent**. This process can take a few minutes.

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_16.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_16.png)

Enter the information for your site administrator

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_17.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_17.png)

Enter your Director’s URL

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_18.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_18.png)

Click done after reviewing your information

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_19.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_19.png)

## Risk Indicators

User risk indicators are user activities that look suspicious or can pose a security threat to your organization. User risk indicators span across all Citrix products used in your deployment. The indicators are based on user behavior and are triggered where the user’s behavior deviates from the normal. User risk indicators help in determining the user’s risk score.

Click **Custom Risk Indicators and Policies** under Settings

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_20.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_20.png)

Turn on the risk indicators by clicking the toggle. Then click **Create Indicator**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_21.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_21.png)

Here you can create custom indicators

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_22.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_22.png)

Click **policies**. A policy is a set of conditions that must be met to apply an action. A policy contains one or more conditions and a single action. You can create a policy with multiple conditions and one action that can be applied to a user’s account.

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_23.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_23.png)

Click **Create policy**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_24.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_24.png)

Select the condition and then the action you want

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_25.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_25.png)

Make sure that the policy is enabled and click **Create policy**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_26.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_26.png)

## Dashboards

The user dashboard provides visibility into user-behavior patterns across an organization. Using this data, you can proactively monitor, detect, and flag behavior that fall outside the norm, such as phishing or ransomware attacks.
click a specific user

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_27.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_27.png)

This dashboard provides a risk timeline of what the user is doing and what source it is coming from.

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_28.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_28.png)

click **Access assurance**

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_29.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_29.png)

The Access Assurance Location dashboard provides an overview of the locations from where your users are accessing their Citrix Virtual Apps and Desktops environment.

[![Citrix Security Analytics](/en-us/tech-zone/learn/media/poc-guides_security-analytics_30.png)](/en-us/tech-zone/learn/media/poc-guides_security-analytics_30.png)
