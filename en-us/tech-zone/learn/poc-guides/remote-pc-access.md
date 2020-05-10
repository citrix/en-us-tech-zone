---
layout: doc
description: Learn how to remotely connect your users working from home to thier physical PCs in the office. Quickly connect your on-premises physical machines to to Citrix Cloud with Citrix Virtual Desktops service and allow remote access from any anywhere and on any device.
---
# Proof of Concept guide for Remote PC Access with Citrix Virtual Desktops service

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

## Overview

Citrix announced the general availability of Citrix Virtual Desktop service on 4th May 2020. This Proof of Concept guide is designed to help you quickly configure Citrix Virtual Desktops service to include Remote PC Access in your environment. At the end of this Proof of Concept guide you will be able to give users who are working from home access to the on-premises physical desktops using Citrix Virtual Desktops service. You will be able to let your users access their on-premises workstations on any device of their choice without having to connect over a VPN.

## Conceptual Architecture

[![Citrix Virtual Desktops service - Add Remote PC Access](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_virtual-desktops-service_ca.png)](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_virtual-desktops-service-ca.png)

## Scope

In this Proof of Concept guide, you will experience the role of a Citrix administrator and you will create a connection between your organization’s on-premises deployment of physical desktops and the Citrix Virtual Desktops service. You will provide access to those on-premises workstations to an end user with Citrix Virtual Desktops service using Citrix Workspace.

This guide will showcase how to perform the following actions:

1.  Create a Citrix Cloud Account (if you don’t have one already)
2.  Obtain a Citrix Virtual Desktops service account
3.  Create a new Resource Location (your office) and install the Citrix Cloud Connectors in it
4.  Install Citrix Virtual Delivery Agent on the Remote PC hosts that are to be made available remotely
5.  Create a machine catalog in Citrix Virtual Desktops service
6.  Create a delivery group
7.  Launch a session remotely from Citrix Workspace

## Prerequisites

### Domain requirements

The in-office workstations that you are looking to connect to are Windows machines that are joined to a Windows Active Directory (AD) domain.

### Citrix Cloud Connector

To install the Citrix Cloud Connectors in your environment, you will require (at least two) Windows Server 2012 R2 or later server machines/VMs. You will require static IPs for these two machines. Windows installation and domain join of these machines must have been done in advance. 
The system requirements for the Cloud Connectors are [here](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html). Review the guidance on the cloud connector installation [here](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#installation-considerations-and-guidance).
The machine the Citrix Cloud Connector runs on must have network access to all the physical machines that are to be made available on the internet via the Citrix Workspace.

Some requirements that could block Citrix Cloud Connector installation are:
The Citrix Cloud Connector machine must have outbound Internet access on port 80 and 443
Microsoft .NET Framework 4.7.2 or later must be pre-installed on the machine
Time on the machine must be synced with UTC

This guide provides detailed instructions on how to configure your environment including office workstations, connecting your on-premises setup up to Citrix Cloud. As a Citrix Cloud administrator, you enable your users to connect to thier office workstations remote with Citrix Virtual Desktops service

## Create a Citrix Cloud Account

1.  RDP to the Cloud Connector machine / VM and login as the AD admin.

1.  Go to the [Citrix Cloud](https://citrix.cloud.com) URL. If you are an existing Citrix Cloud customer skip to the next section [Create a new Resource Location](/en-us/tech-zone/learn/poc-guides/remote-pc-access.html#create-a-new-resource-location). Please ensure that you have an active Citrix cloud account. If your account has expired you will need to contact sales to enable it.

1.  If you are new to Citrix Cloud click **Sign up and try it free** in the bottom left

    ![Citrix Virtual Desktops service - Create Citrix Cloud account](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-1.png)

1.  **Enter your business details** and **check the “I’ve read, understand and agree to the Terms of Service” check box**, if you agree. Click **Continue**

    ![Citrix Virtual Desktops service - Enter company details and agree to TOS](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-2.png)
    
1.  **Select an appropriate region** to host your Citrix cloud deployment. Click **Continue**

    ![Citrix Virtual Desktops service - Select home region](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-3.png)
   
1.  If you chose the wrong region, then you can click **Change** to go back to the previous page and select the right region. Confirm that you have chosen the correct home region, **Check the “I acknowledge that my home region is set to *Region you chose*”** checkbox. Click **Continue**.

    ![Citrix Virtual Desktops service - Confirm home region](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-4.png)

1.  **Provide the name of your company** in the **Company Name** text box. If you agree then **check the “I’ve read, understand and agree to the Terms of Service”** checkbox and click **Sign Up**

    ![Citrix Virtual Desktops service - Enter company name and agree to TOS](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-5.png)

1.  Click **Sign In**

    ![Citrix Virtual Desktops service - Sign In](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-6.png)
    
1.  Verify your email id, by clicking **Sign in to Get Started** in the email you received from Citrix Cloud.

    ![Citrix Virtual Desktops service - Verify Email](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-7.png)
    
1.  Once the account is confirmed, **Enter and Confirm the password**. Click **Create Account**

    ![Citrix Virtual Desktops service - Create account finish](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-8.png)

## Subscribe to the Citrix Virtual Desktops service

1.  Enter **username and password**. Click **Sign In**. (If your account manages more than one customer select the appropriate one)

    ![Citrix Virtual Desktops service - Sign In](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-9.png)

1.  Search for the **Citrix Virtual Desktops** service tile. Click **Request Demo**

    ![Citrix Virtual Desktops service - Request Demo](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-10.png)

1.  Click **Request a call**

    ![Citrix Virtual Desktops service - Request a call](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-11.png)

1.  **Enter your details** and in comments specify **"Citrix Virtual Desktops service trial"**. Click **Submit**. Citrix Sales will contact you to give you access to the service.

    ![Citrix Virtual Desktops service - Request CVD service trial](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-12.png)

## Create a new Resource Location

1.  While the service is being provisioned, we can keep going. Return to the Citrix Cloud administration page. Scroll up, under Resource Locations Click **Edit or Add New**

    ![Citrix Virtual Desktops service - New Resource Location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-13.png)

1.  Click **Add a Resource Location** or **+ Resource Location** (if there is already a resource location)

    ![Citrix Virtual Desktops service - Add Resource Location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-14.png)

1.  Click the **ellipses** on the top right of the new resource location. Click **Manage Resource Location**.

    ![Citrix Virtual Desktops service - Manage Resource Location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-15.png)

1.  Enter a **new name** of the New Resource Location. Click **Confirm**.

    ![Citrix Virtual Desktops service - Name the Resource Location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-16.png)

1.  Under the newly created resource location click **+ Cloud Connectors**

    ![Citrix Virtual Desktops service - Add Citrix Cloud Connector](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-17.png)

1.  Click **Download**. Click **Run** once the donwlaod completes.

    ![Citrix Virtual Desktops service - Download and run Citrix Cloud Connector installer](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-18.png)

1.  Citrix Cloud connectivity test successful message should be displayed. Click **Close**. 
**Note**: If the test fails, check the following ![link to resolve the issue](https://support.citrix.com/article/CTX224133)

    ![Citrix Virtual Desktops service - Citrix Cloud connectivity check](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-19.png)
    
1.  Click **Sign In** and **sign in to Citrix Cloud** to authenticate the Cloud Conenctor.

    ![Citrix Virtual Desktops service - Authenticate to Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-20.png)

1.  From the drop downs **select the appropriate Customer and Resource Location** (Resource location dropdown will not be displayed if there is only one resource location). Click **Install**

    ![Citrix Virtual Desktops service - Select cusotmer and resource location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-21.png)

1.  Once the installation completes. A service connectivity test will run. Let it complete and you should again see a successful result. Click **Close**

    ![Citrix Virtual Desktops service - Cloud Connector installed](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-22.png)

1.  Click **Refresh all** to refresh the Resource Location page in Citrix Cloud

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-23.png)

1.  Click on **Cloud Connectors**

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-24.png)

1.  The newly added Cloud Connector is listed. Repeat the last 8 steps to install another Cloud Connector in the Resource Location on the second Windows server machine that you had prepared.

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-25.png)

## Install Virtual Delivery Agent on the Remote PC hosts

We now install the Citrix Virtual Desktops, virtual delivery agent on the physical machines that we are going to give users access to. If you wish to install the Citrix Virtual Delivery Agent using [scripts](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/vda-install-scripts.html) or a deployment tool like [SCCM](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/install-vdas-sccm.html) follow the appropriate links. Ensure to use the install command line parameters as shown in the instructions below.

1.  
