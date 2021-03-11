---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Mayank Singh
description: Learn how to get started with Citrix Virtual Apps and Desktop Service to deliver virtual apps and desktops to your end users while having the management plane hosted on Citrix Cloud.
---
# Proof of Concept guide for Getting Started with Citrix Virtual Apps and Desktop Service

## Overview

The Citrix Virtual Apps and Desktop service allows you to provide employees with a full workspace from any device while leaving most of the setup, upgrades, and monitoring to Citrix. IT will continue to manage and control the virtual machines, applications, and policies on the back end, while Citrix manages all the key infrastructure, installation, setup, and upgrades needed to maintain a Citrix environment. This reduces administrative overload and ensures you have all the latest features and functionality.

## Conceptual Architecture

[![Citrix Virtual Apps and Desktops Service Architecture](/en-us/tech-zone/learn/media/poc-guides_cvads_conceptual-architecture.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_conceptual-architecture.png)

### Components

**Hosted by Citrix:**

*  [Workspace](/en-us/citrix-workspace): complete digital workspace that lets you securely deliver apps, desktops, files, & microapps to your end-users
*  [Gateway Service](/en-us/citrix-gateway-service): allows secure, contextual remote access to apps and data
*  [Virtual Apps and Desktop Service](/en-us/citrix-virtual-apps-desktops-service): allows IT to provide virtual apps and desktops to their end users

**Hosted by Customer:**

*  [Cloud Connector](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html): serves as a channel for communication between Citrix Cloud and your resource location. Although this is hosted within the customer's resource location, it is managed by Citrix and kept evergreen. A minimum of two Cloud Connectors per resource location is recommended so that there is no loss in communication with Citrix Cloud.
*  [Virtual Delivery Agent](/en-us/citrix-virtual-apps-desktops-service): The VDA is used to register with the Cloud connector so connections can be brokered between the end-user's device and the virtual machine

## Scope

This proof-of-concept guide describes the following:

1.  Creating a Citrix Cloud account
2.  Requesting a Citrix Virtual Apps and Desktops service trial
3.  Creating a Resource Location and installing the Citrix Cloud Connectors
4.  Installing Citrix Virtual Delivery Agent on the virtual machines
5.  Creating a Machine Catalog in the Citrix Virtual Apps & Desktops service
6.  Creating a Delivery Group
7.  Launching a session from Citrix Workspace

## Prerequisites

### VM Requirements

Prior to starting this POC, you need to create an image and install any application you would like to deliver to your end users.

Virtual Delivery Agents (VDAs) are required on each physical or virtual machine that delivers applications and desktops. These register with the cloud connectors. VDAs can be installed in either single-session or multi-session OS. The following operating systems are supported:

*  Windows 10 (see [CTX224843](https://support.citrix.com/article/CTX224843) for edition support) multi-session and single session
*  Windows 7
*  Windows Server 2012 R2 and newer

### Cloud Connector Requirements

The cloud connectors act as the communication channel between the components hosted in Citrix Cloud and the components hosted in the resource location. The cloud connectors act as a proxy for the delivery controller in Citrix Cloud.
To install the Citrix Cloud Connectors in your environment, you require (at least two) Windows Server 2012 R2 or later server machines/VMs. You require static IPs for these two machines. Windows installation and domain join of these machines must have been done in advance.
The system requirements for the Cloud Connectors are [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html). Review the guidance on the cloud connector installation [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#installation-considerations-and-guidance).
The machine the Citrix Cloud Connector runs on must have network access to all the virtual machines that are to be made available on the internet via the Citrix Workspace.

Some requirements Citrix Cloud Connector installation (installer performs checks for these) is:

The Citrix Cloud Connector machine must have outbound Internet access on port 443, and port 80 to only **\*.digicert.com**. The port 80 requirement is for X.509 certificate validation. See more info [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#certificate-validation-requirements)

Microsoft .NET Framework 4.7.2 or later must be pre-installed on the machine

Time on the machine must be synced with UTC

The [Cloud Connector Connectivity Check Utility](https://support.citrix.com/article/CTX260337) tool can be used to test the reachability of the Citrix Cloud and its related services

## Deployment Steps

### Create Citrix Cloud Account

If you are an existing Citrix Cloud customer, skip to the next section: [Obtain Citrix Virtual Apps and Desktops Trial](#obtain-a-citrix-virtual-apps-and-desktops-trial-account). Ensure that you have an active Citrix Cloud account. If your account has expired, contact your account manager to enable it.

1.  Go to the [Citrix Cloud](https://citrix.cloud.com).
2.  If you are new to Citrix Cloud click **Sign up and try it free** in the bottom left.
[![Citrix Virtual Apps and Desktops Create Cloud Account](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc1.png)
3.  **Enter your business details** and Check the **“I’ve read, understand and agree to the Terms of Service”** check box, if you agree. Click **Continue**.
[![Citrix Virtual Apps and Desktops Enter Business Details](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc2.png)
4.  **Select an appropriate region** to host your Citrix Cloud deployment. Click **Continue**
[![Citrix Virtual Apps and Desktops Select Region](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc3.png)
5.  If you chose the wrong region, then you can click **Change** to go back to the previous page and select the right region. Confirm that you have chosen the correct home region, Check the **“I acknowledge that my home region is set to < Region you chose >”** check box.
Click **Continue**.
[![Citrix Virtual Apps and Desktops Confirm Region](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc4.png)
6.  Provide the name of your company in the **Company Name** text box.
If you agree then check the **“I’ve read, understand and agree to the Terms of Service”** check box and click **Sign Up**
[![Citrix Virtual Apps and Desktops Verify Information](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc5.png)
7.  Click **Sign In**
[![Citrix Virtual Apps and Desktops Sign In](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc6.png)
8.  Verify your email id, by clicking **Sign in to Get Started** in the email you received from Citrix Cloud.
[![Citrix Virtual Apps and Desktops Email](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc7.png)
*Note: If you are unable to locate this email, search your spam folder. The subject of the email is: Complete your Account Setup from: donotreplynotifications@citrix.com*
9.  Once the account is confirmed, **Enter and Confirm the password.** Click **Create Account**.
[![Citrix Virtual Apps and Desktops Password](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_ccc8.png)

### Enable Multifactor Authentication for Citrix Cloud Administrators

1.  Click the admin name in the top right corner of Citrix Cloud. Click **My Profile**
[![Citrix Virtual Apps and Desktops Citrix Cloud Profile](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa1.png)
2.  Click **Set up an authenticator app** on your device to start the device enrollment process. This app can be on your mobile device, laptop, or desktop.
[![Citrix Virtual Apps and Desktops Authenticator App](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa2.png)
3.  You will receive an email with a verification code.
[![Citrix Virtual Apps and Desktops Device Registration Email](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa3.png)
4.  Enter this code and your account password. Click **Verify**.
[![Citrix Virtual Apps and Desktops Code Verification](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa4.png)
5.  **Download an authenticator app** that supports time-based, one-time passwords (TOTP). Options include Citrix SSO, Google Authenticator, Microsoft Authenticator, and more.
6.  **Scan the QR code** or enter the key into your authenticator app. An entry will show up for Citrix and will start generating six-digit TOTP codes.
[![Citrix Virtual Apps and Desktops QR code](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa5.png)
7.  To verify proper configuration, enter the six-digit code from the app and click **Verify**.
[![Citrix Virtual Apps and Desktops QR code verification](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa6.png)
8.  If you lose your device or delete your authenticator app, you’ll need to use a recovery method to regain access to your account. Citrix will require two recovery methods to ensure you’ll always be able to securely access your Citrix Cloud account. First, choose **“Recovery Phone”**.
[![Citrix Virtual Apps and Desktops recovery phone](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa7.png)
9.  **Enter your recovery phone information**. In the case that you lose or replace your device, lose your backup codes, or delete your authenticator app, a Citrix representative will call you to verify your identity. We will never ask you for your password or back up codes.
[![Citrix Virtual Apps and Desktops phone info](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa8.png)
10.  Get your backup codes by clicking **“Generate backup codes”**.
[![Citrix Virtual Apps and Desktops backup codes](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa9.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa9.png)
11.  Click **Download Codes** to download a text file with your backup codes. Store these securely as you’ll need them if you lose your device or delete the app that was generating your one-time passwords.
[![Citrix Virtual Apps and Desktops download codes](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa10.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa10.png)
12.  Acknowledge the backup codes have been downloaded and click **Finish**.
[![Citrix Virtual Apps and Desktops MFA finish](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa11.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_mfa11.png)

### Obtain a Citrix Virtual Apps and Desktops Trial Account

1.  Enter **Username and Password** Click **Sign In**. (If your account manages more than one customer select the appropriate one)
[![Citrix Virtual Apps and Desktops Cloud Sign In](/en-us/tech-zone/learn/media/poc-guides_cvads_trial1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_trial1.png)
2.  Click **Citrix Cloud** on top of the page. Search for the Citrix Virtual Desktops Service Tile. Click **Request Trial**.
[![Citrix Virtual Apps and Desktops Cloud Request Demo](/en-us/tech-zone/learn/media/poc-guides_cvads_trial2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_trial2.png)
3.  If you know who your account team is, contact them to get the trial approved. If you are unsure who your account team is, continue to the next step.
4.  Click **Request a Call**
[![Citrix Virtual Apps and Desktops Request Call](/en-us/tech-zone/learn/media/poc-guides_cvads_trial3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_trial3.png)
5.  **Enter your details** and in Comments section specify **“Citrix Virtual Desktops service trial”**. Click **Submit**.
Citrix Sales contact you to give you access to the service. *Note: This is not immediate, a Citrix sales rep will reach out*
[![Citrix Virtual Apps and Desktops Request Call Info](/en-us/tech-zone/learn/media/poc-guides_cvads_trial4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_trial4.png)

### Create a Resource Location

Resource locations contain the resources required to deliver applications and desktops to your users. Your resource locations include your active delivery domain controller, your hypervisor or cloud services (hosts), your session hosts, and your cloud connectors.

1.  Log in to your Citrix Cloud Console from your cloud connector *(this can be done via RDP)*.
2.  Under Resource Locations **Click Edit or Add New**
[![Citrix Virtual Apps and Desktops Add or Edit Resource Location](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location1.png)
3.  Click **Add a Resource Location**
[![Citrix Virtual Apps and Desktops Add Resource Location](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location2.png)
4.  Click the **ellipses** on the top right of the new resource location. Click **Manage Resource Location.**
 [![Citrix Virtual Apps and Desktops Manage Resource Location](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location3.png)
5.  Enter a new name of the New Resource Location. Click **Confirm**
[![Citrix Virtual Apps and Desktops Name Resource Location](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location4.png)
6.  Under the newly created resource location click **+ Cloud Connectors**
[![Citrix Virtual Apps and Desktops Cloud Connectors](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location5.png)
7.  Click **Download**. Click **Run** once the download finishes
[![Citrix Virtual Apps and Desktops Run](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location6.png)
8.  Citrix Cloud connectivity test successful message will be displayed. Click Close.
If the test fails, check the following [link](https://support.citrix.com/article/CTX224133) to resolve the issue. You may also use the [Cloud Connector Connectivity Check Utility](https://support.citrix.com/article/CTX260337) tool to verify all addresses are reachable
[![Citrix Virtual Apps and Desktops Connection Test](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location7.png)
9.  Click **Sign In** and **Sign in to Citrix Cloud**
[![Citrix Virtual Apps and Desktops Sign In Connector](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location8.png)
10.  From the drop-down menu select the appropriate **Customer and Resource Location** (Resource location drop-down menu will not be displayed if there is only one resource location). Click **Install**
[![Citrix Virtual Apps and Desktops Chose Region](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location9.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location9.png)
11.  Once the installation completes, a service connectivity test runs. Let it complete and you will see a successful result. Click **Close**
[![Citrix Virtual Apps and Desktops Test Connectivity](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location10.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location10.png)
12.  Refresh the Resource Location page in Citrix Cloud.
[![Citrix Virtual Apps and Desktops Refresh](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location11.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location11.png)
13.  Click **Cloud Connectors**
[![Citrix Virtual Apps and Desktops Cloud Connector](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location12.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location12.png)
14.  The newly added Cloud Connector is listed. Repeat to install another Cloud Connector in the Resource Location on the second Windows server machine that you had prepared.
[![Citrix Virtual Apps and Desktops Cloud Connector Warning](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location14.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_resource-location14.png)

### Install Citrix Virtual Delivery Agent

The Virtual Delivery Agent must be installed on all physical or virtual machines that are used to deliver applications or desktops. In this POC, we are going to be installing it on virtual machines. These can be single-session OS or multi-session OS depending on your organization’s needs.

1.  Connect to the **virtual machine the Domain admin**. *(for this POC, we are connecting via RDP)* This will serve as the master image.
[![Citrix Virtual Apps and Desktops RDP](/en-us/tech-zone/learn/media/poc-guides_cvads_vda1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda1.png)
2.  Open **Citrix.com** on a browser. Hover over Sign In and click **My Account**
[![Citrix Virtual Apps and Desktops My Account](/en-us/tech-zone/learn/media/poc-guides_cvads_vda2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda2.png)
3.  Sign in with your **username and password**.
[![Citrix Virtual Apps and Desktops Sign In](/en-us/tech-zone/learn/media/poc-guides_cvads_vda3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda3.png)
4.  Click **Downloads** in the top menu. From the Select a product drop-down menu, select Citrix Virtual Apps and Desktops
[![Citrix Virtual Apps and Desktops menu](/en-us/tech-zone/learn/media/poc-guides_cvads_vda4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda4.png)
5.  In the page that opens, select the **latest version of Citrix Virtual Apps and Desktops 7**
[![Citrix Virtual Apps and Desktops Product Download](/en-us/tech-zone/learn/media/poc-guides_cvads_vda5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda5.png)
6.  Scroll down to **Components that are on the product ISO but also packaged separately**. Click the **chevron** to expand the section. Click **Download File** on the appropriate iso (depending on if you’re installing in a single or multi-session OS)
[![Citrix Virtual Apps and Desktops Product iso](/en-us/tech-zone/learn/media/poc-guides_cvads_vda6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda6.png)
7.  Check **“I have read and certify that I comply with the above Export Control Laws”** check box, if you agree. Click **Accept**. The download will begin.
[![Citrix Virtual Apps and Desktops Product terms and conditions](/en-us/tech-zone/learn/media/poc-guides_cvads_vda7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda7.png)
8.  **Save** the file. When the download completes, Click **Open Folder**
[![Citrix Virtual Apps and Desktops Product Open Folder](/en-us/tech-zone/learn/media/poc-guides_cvads_vda8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda8.png)
9.  Click **Run**
[![Citrix Virtual Apps and Desktops Product Run](/en-us/tech-zone/learn/media/poc-guides_cvads_vda9.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda9.png)
10.  Select **Create a master MCS image** and click **Next**
[![Citrix Virtual Apps and Desktops MCS image](/en-us/tech-zone/learn/media/poc-guides_cvads_vda10.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda10.png)
11.  Click **Next**
[![Citrix Virtual Apps and Desktops Core Components](/en-us/tech-zone/learn/media/poc-guides_cvads_vda11.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda11.png)
12.  Choose the additional components that you need and click **Next**
[![Citrix Virtual Apps and Desktops extra Components](/en-us/tech-zone/learn/media/poc-guides_cvads_vda12.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda12.png)
13.  Type in the name of your cloud connector. Click **Test connection** and then click **Add**
[![Citrix Virtual Apps and Desktops Delivery Controller](/en-us/tech-zone/learn/media/poc-guides_cvads_vda13.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda13.png)
14.  Add the second cloud connector and select **Next**
15.  Choose the appropriate features and click **Next**
[![Citrix Virtual Apps and Desktops Features](/en-us/tech-zone/learn/media/poc-guides_cvads_vda15.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda15.png)
16.  Click the firewall and select **Next**
[![Citrix Virtual Apps and Desktops Firewall](/en-us/tech-zone/learn/media/poc-guides_cvads_vda16.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda16.png)
17.  Review the components and click **Install**
[![Citrix Virtual Apps and Desktops Summary](/en-us/tech-zone/learn/media/poc-guides_cvads_vda17.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda17.png)
18.  If you want to collect diagnostic information click **Connect**, otherwise clear and click **Next**
 [![Citrix Virtual Apps and Desktops Diagnostic](/en-us/tech-zone/learn/media/poc-guides_cvads_vda18.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda18.png)
19.  Type in your **Citrix Cloud credentials** and click **Ok**
[![Citrix Virtual Apps and Desktops Insight Service](/en-us/tech-zone/learn/media/poc-guides_cvads_vda19.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda19.png)
20.  Click **Next**
[![Citrix Virtual Apps and Desktops Diagnostic2](/en-us/tech-zone/learn/media/poc-guides_cvads_vda20.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda20.png)
21.  Once the installation is completed click **Finish** (this might require the machine to restart)
[![Citrix Virtual Apps and Desktops Finish](/en-us/tech-zone/learn/media/poc-guides_cvads_vda21.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_vda21.png)
**Repeat the procedure for all the master images you want to make. The same process applies for multi-session OS, except you would download the multi-session OS iso.**

### Create a Hosting Connection to the Citrix Cloud

Specific supported hypervisors can be found [here](/en-us/citrix-virtual-apps-desktops-service/system-requirements.html#hosts--virtualization-resources)

1.  Once the trial is approved, **Login to Citrix Cloud** from your local machine. Scroll to **My Services**, and locate **Virtual Apps and Desktops** service tile, click **Manage**
[![Citrix Virtual Apps and Desktops Manage](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting1.png)
2.  The service overview page is displayed. **Scroll further** down and find the **Workspace Experience URL. Bookmark it.**
[![Citrix Virtual Apps and Desktops Workspace URL](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting2.png)
3.  Click the **chevron next to Manage** on the top left. Click **Full Configuration**
[![Citrix Virtual Apps and Desktops Full Configuration](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting3.png)
4.  In the Left drop-down list under Configuration. Click **Hosting** and then click **Add Connection and Resources**
[![Citrix Virtual Apps and Desktops Hosting](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting4.png)
5.  Select your connection type and input the necessary information
[![Citrix Virtual Apps and Desktops Connection](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting5.png)
6.  Decide whether you are going to use shared storage or local storage and click **Next**
[![Citrix Virtual Apps and Desktops Storage](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting6.png)
7.  Pick with where you want each storage type to be saved and click **Next**
[![Citrix Virtual Apps and Desktops Manage](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting7.png)
8.  Choose which network you want your virtual machines to use in addition to a name for the resource and click **Next**
[![Citrix Virtual Apps and Desktops Network](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting8.png)
9.  Review the summary and if everything is accurate click **Finish**
[![Citrix Virtual Apps and Desktops Finish](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting9.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_hosting9.png)

### Create a Machine Catalog in Citrix Virtual Apps & Desktops service

Use Citrix Virtual Apps & Desktop service to create a catalog of the virtual machines. We are going to be creating copies of the machine we installed the VDA agent.

1.  In the left menu under Citrix Studio. Click **Machine Catalogs**
[![Citrix Virtual Apps and Desktops Machine Catalog](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog1.png)
2.  In the Actions menu (right side). Click **Create Machine Catalog**
[![Citrix Virtual Apps and Desktops Create Machine Catalog](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog2.png)
3.  In the Machine Catalog Setup dialog, click **Next**
[![Citrix Virtual Apps and Desktops Intro](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog3.png)
4.  Select the **appropriate operating system for your machine catalog**. This needs to match the operating system of the virtual machine where you installed the VDA
[![Citrix Virtual Apps and Desktops Operating System](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog4.png)
5.  Choose how you want to deploy your machines. In this POC guide, we are going to have **Machines that are powered managed** and deployed through **Citrix Machine Creation Services**. Click **Next**
[![Citrix Virtual Apps and Desktops Machine Management](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog5.png)
6.  Choose how you want users to connect. Click **Next**
[![Citrix Virtual Apps and Desktops-Desktop Experience](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog6.png)
7.  Choose the VM in which you installed the VDA and click **Next**
[![Citrix Virtual Apps and Desktops Master Image](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog7.png)
8.  Specify the number of machines you want created and click **Next**
[![Citrix Virtual Apps and Desktops VM](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog8.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog8.png)
9.  Active Directory accounts for the new VM can be created beforehand or can be created automatically. Specify which OU you want these new VMs to be a part of. Determine the naming scheme that you want these new machines to have and click **Next**
[![Citrix Virtual Apps and Desktops Computer Accounts](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog9.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog9.png)
10.  **Enter credentials** that have sufficient permissions to create machine accounts in active directory and click **Ok**
[![Citrix Virtual Apps and Desktops Credentials](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog10.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog10.png)
11.  Enter a **Machine Catalog name**. Review—and click **Finish**
[![Citrix Virtual Apps and Desktops Summary](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog11.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog11.png)
12.  This will now create a copy of the master image and create your machine catalog
[![Citrix Virtual Apps and Desktops Creating Machine Catalog](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog12.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog12.png)
13.  Once completed you see your machine catalog appear in your Citrix Cloud console
[![Citrix Virtual Apps and Desktops Confirmation](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog13.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_machine-catalog13.png)

### Create Delivery Group

1.  From the left side menu click **Delivery Groups** to start creating your delivery group.
[![Citrix Virtual Apps and Desktops Delivery Group](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group1.png)
2.  Click **Create Delivery Group** from the Actions menu.
[![Citrix Virtual Apps and Desktops Create Delivery Group](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group2.png)
3.  In the Create Delivery Group dialog that opens click **Next**
[![Citrix Virtual Apps and Desktops Introduction](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group3.png)
4.  Select the catalog you created earlier and choose the number of machines from the machine catalog that will be assigned to this delivery group. Click **Next**
[![Citrix Virtual Apps and Desktops Machines](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group4.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group4.png)
5.  You can choose to assign users at this stage, or leave the user management to Citrix Cloud and assign users through the **Citrix Cloud library**. Click **Next**
[![Citrix Virtual Apps and Desktops Users](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group5.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group5.png)
6.  You can specify applications that appear to the end users under their workspace or if you want to deliver only the desktop, leave this blank and click **Next**
[![Citrix Virtual Apps and Desktops Applications](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group6.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group6.png)
7.  Choose the name of your **Delivery group** and the display name that your users will see and click **Finish**
[![Citrix Virtual Apps and Desktops Finish](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group7.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_delivery-group7.png)

### Add Users

1.  Go back to main menu of Citrix Cloud and click **View Library**
[![Citrix Virtual Apps and Desktops Library Offering](/en-us/tech-zone/learn/media/poc-guides_cvads_users1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_users1.png)
2.  You will now see your offerings available. Click the ellipses and click **Manage Subscribers**
[![Citrix Virtual Apps and Desktops Manage Subscribers](/en-us/tech-zone/learn/media/poc-guides_cvads_users2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_users2.png)
3.  Choose the user or group that you want to assign to those resources
[![Citrix Virtual Apps and Desktops Resources](/en-us/tech-zone/learn/media/poc-guides_cvads_users3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_users3.png)

### Launch a Session from Citrix Workspace

1.  **Open the Workspace URL** you had saved earlier (from Citrix Cloud) to the gain access to the Citrix Workspace.
**Login as a domain user** you assigned the resources in the previous section
[![Citrix Virtual Apps and Desktops Log in](/en-us/tech-zone/learn/media/poc-guides_cvads_launch1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_launch1.png)
2.  If this is the first time you are launching a session from the browser, you may get the following pop-up. **Ensure Citrix Workspace App** is installed and click **Detect Workspace**
[![Citrix Virtual Apps and Desktops Detect Workspace](/en-us/tech-zone/learn/media/poc-guides_cvads_launch2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_launch2.png)
3.  Click **View all Desktops**. You will now see the desktop that we assigned in the previous section
[![Citrix Virtual Apps and Desktops Start Session](/en-us/tech-zone/learn/media/poc-guides_cvads_launch3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_launch3.png)
4.  The session launches giving the user access to their virtual desktop

## Additional Information

The Citrix Cloud Resource Center can help you learn more about features, and search to resolve issues.

1.  Click the **blue compass** arrow at the bottom of the Citrix Cloud page
[![Citrix Virtual Apps and Desktops Help](/en-us/tech-zone/learn/media/poc-guides_cvads_info1.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_info1.png)
2.  Click **Search Articles**. This allows you to search through a list of product documentation and Knowledge Center Articles
[![Citrix Virtual Apps and Desktops Search](/en-us/tech-zone/learn/media/poc-guides_cvads_info2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_info2.png)
3.  Enter a search query
[![Citrix Virtual Apps and Desktops How To](/en-us/tech-zone/learn/media/poc-guides_cvads_info3.png)](/en-us/tech-zone/learn/media/poc-guides_cvads_info3.png)

You have access to a list of product documentation and Knowledge Center articles for common tasks without leaving Citrix Cloud.
