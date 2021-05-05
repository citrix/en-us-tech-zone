---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
description: Learn how to remotely connect your users working from home to their physical PCs in the office. Quickly connect your on-premises physical machines to Citrix Cloud with Citrix Virtual Desktops service and allow remote access from anywhere and on any device.
tz_title: Remote PC Access with Citrix Virtual Desktops service
tz_products: citrix-virtual-apps-and-desktops;
---
# PoC Guide: Remote PC Access with Citrix Virtual Desktops service

## Overview

Citrix announced the addition of Remote PC Access within Citrix Virtual Desktops service on April 30, 2020. This Proof of Concept guide is designed to help you quickly configure Citrix Virtual Desktops service to include Remote PC Access in your environment. At the end of this Proof of Concept guide you will be able to give users who are working from home access to the on-premises physical desktops using Citrix Virtual Desktops service. You will be able to let your users access their on-premises workstations on any device of their choice without having to connect over a VPN.

## Conceptual Architecture

[![Citrix Virtual Desktops service - Add Remote PC Access](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_virtual-desktops-service-ca.png)](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_virtual-desktops-service-ca.png)

## Scope

In this Proof of Concept guide, you will experience the role of a Citrix administrator and you will create a connection between your organization’s on-premises deployment of physical desktops and the Citrix Virtual Desktops service. You will provide access to those on-premises workstations to an end user with Citrix Virtual Desktops service using Citrix Workspace.

This guide will showcase how to perform the following actions:

1.  Create a Citrix Cloud account (if you don’t have one already)
2.  Obtain a Citrix Virtual Desktops service account
3.  Create a new Resource Location (your office) and install the Citrix Cloud Connectors in it
4.  Install Citrix Virtual Delivery Agent on the Remote PC Access hosts
5.  Create a Machine Catalog in Citrix Virtual Desktops service
6.  Create a Delivery Group
7.  Launch a session from Citrix Workspace

## Prerequisites

### Host machine requirements

The in-office workstations that your users must connect to are Windows single-session operating system machines, and are joined to a Windows Active Directory (AD) domain.

### Citrix Cloud Connector

To install the Citrix Cloud Connectors in your environment, you require (at least two) Windows Server 2012 R2 or later server machines/VMs. You require static IPs for these two machines. Windows installation and domain join of these machines must have been done in advance.
The system requirements for the Cloud Connectors are [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html). Review the guidance on the cloud connector installation [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#installation-considerations-and-guidance).
The machine the Citrix Cloud Connector runs on must have network access to all the physical machines that are to be made available on the internet via the Citrix Workspace.

Some requirements Citrix Cloud Connector installation (installer performs checks for these) are:

The Citrix Cloud Connector machine must have outbound Internet access on port 443, and port 80 to only **\*.digicert.com**. The port 80 requirement is for X.509 certificate validation. See more info [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#certificate-validation-requirements)

Microsoft .NET Framework 4.7.2 or later must be pre-installed on the machine

Time on the machine must be synced with UTC

This guide provides detailed instructions on how to configure your environment including office workstations, connecting your on-premises setup up to Citrix Cloud. As a Citrix Cloud administrator, you enable your users to connect to their office workstations remote with Citrix Virtual Desktops service

## Create a Citrix Cloud Account

1.  RDP to the Cloud Connector machine / VM and login as the local admin.

1.  Go to the [Citrix Cloud](https://citrix.cloud.com) URL. If you are an existing Citrix Cloud customer skip to the next section: [Subscribe to the Citrix Virtual Desktops service](/en-us/tech-zone/learn/poc-guides/remote-pc-access.html#subscribe-to-the-citrix-virtual-desktops-service). Ensure that you have an active Citrix Cloud account. If your account has expired you must contact sales to enable it.

1.  If you are new to Citrix Cloud, click **Sign up and try it free** in the bottom left

    ![Citrix Virtual Desktops service - Create Citrix Cloud account](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-1.png)

1.  **Enter your business details** and **check the “I’ve read, understand and agree to the Terms of Service” check box**, if you agree. Click **Continue**

    ![Citrix Virtual Desktops service - Enter company details and agree to TOS](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-2.png)

1.  **Select an appropriate region** to host your Citrix Cloud deployment. Click **Continue**

    ![Citrix Virtual Desktops service - Select home region](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-3.png)

1.  If you chose the wrong region, then you can click **Change** to go back to the previous page and select the right region. Confirm that you have chosen the correct home region, **Check the “I acknowledge that my home region is set to *Region you chose*”** check box. Click **Continue**.

    ![Citrix Virtual Desktops service - Confirm home region](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-4.png)

1.  **Provide the name of your company** in the **Company Name** text box. If you agree then **check the “I’ve read, understand and agree to the Terms of Service”** check box and click **Sign Up**

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

1.  Click **Download**. Click **Run** once the download completes.

    ![Citrix Virtual Desktops service - Download and run Citrix Cloud Connector installer](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-18.png)

1.  Citrix Cloud connectivity test successful message is displayed. Click **Close**. **Note**: If the test fails, check the following [link to resolve the issue](https://support.citrix.com/article/CTX224133)

    ![Citrix Virtual Desktops service - Citrix Cloud connectivity check](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-19.png)

1.  Click **Sign In** and **sign in to Citrix Cloud** to authenticate the Citrix Cloud Connector.

    ![Citrix Virtual Desktops service - Authenticate to Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-20.png)

1.  From the drop-down lists **select the appropriate Customer and Resource Location** (Resource location drop-down list is not displayed if there is only one resource location). Click **Install**

    ![Citrix Virtual Desktops service - Select customer and resource location](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-21.png)

1.  Once the installation completes, a service connectivity test runs. Let it complete and you will again see a successful result. Click **Close**

    ![Citrix Virtual Desktops service - Cloud Connector installed](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-22.png)

1.  Click **Refresh all** to refresh the Resource Location page in Citrix Cloud

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-23.png)

1.  Click **Cloud Connectors**

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-24.png)

1.  The newly added Cloud Connector is listed. Repeat the last 8 steps to install another Cloud Connector in the Resource Location on the second Windows server machine that you had prepared.

    ![Citrix Virtual Desktops service - Refresh resource locations](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-25.png)

## Install Citrix Virtual Delivery Agent on the Remote PC Access hosts

We now install the Citrix Virtual Desktops, Virtual Delivery Agent on the physical machines that we are going to give users access to. If you want to install the Citrix Virtual Delivery Agent using [scripts](/en-us/citrix-virtual-apps-desktops/install-configure/vda-install-scripts.html) or a deployment tool like [SCCM](/en-us/citrix-virtual-apps-desktops/install-configure/install-vdas-sccm.html) follow the appropriate links. Ensure to use the install command line parameters as shown in the following instructions.

1.  Connect to the **physical machine via RDP as the a local admin**.

    ![Citrix Virtual Desktops service - RDP to physical host](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-26.png)

1.  Open **Citrix.com** in Internet Explorer. Hover over **Sign In** and click **My Account**

    ![Citrix Virtual Desktops service - Open Citrix.com](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-27.png)

1.  Sign in with your **username and password**. Click **Downloads** in the top menu

    ![Citrix Virtual Desktops service - Log in to Citrix.com](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-28.png)

1.  From the **Select a product...** drop-down list, select **Citrix Virtual Apps and Desktops**

    ![Citrix Virtual Desktops service - Select CVAD from drop-down list](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-29.png)

1.  In the page that opens, select the **latest version of Citrix Virtual Apps and Desktops 7** (without the .x at the end)

    ![Citrix Virtual Desktops service - Select latest version](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-30.png)

1.  Scroll down to **Components that are on the product ISO but also packaged separately**. Click **chevron** to expand the section. Click **Download File** under the **Single-session OS Virtual Delivery Agent *version***

    ![Citrix Virtual Desktops service - Select Single-session OS VDA](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-31.png)

1.  **Check “I have read and certify that I comply with the above Export Control Laws”** check box, if you agree. Click **Accept**. The download begins.

    ![Citrix Virtual Desktops service - Accept terms and download](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-32.png)

1.  **Save** the file. When the download completes, Click **Open Folder**

    ![Citrix Virtual Desktops service - Save and open download folder](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-33.png)

1.  **Search for PowerShell** from the Start menu search bar and Click **Run as administrator**

    ![Citrix Virtual Desktops service - Run PowerShell as administrator](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-34.png)

1.  Traverse to the directory you downloaded the installer in.

    ![Citrix Virtual Desktops service - Change directory to download folder](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-35.png)

1.  Run the following command. (Replace the name of the executable with the one you downloaded and the cloud connector FQDN). **Note**: The Citrix UPM and the Citrix UPM WMI Plugin are essential for monitoring and Citrix Analytics to collect data from the endpoint, so that logon duration, session resilliency and UX score can be reported.
**VDAWorkstationSetup_*version*.exe /quiet /remotepc /includeadditional “Citrix User Profile Manager”,“Citrix User Profile Manager WMI Plugin” /controllers “cloudconnecotrFQDN” /enable_hdx_ports /noresume /noreboot**

    ![Citrix Virtual Desktops service - Run installer](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-36.png)

1.  Wait for the installation to complete. **Reboot** the physical machine.

    ![Citrix Virtual Desktops service - Wait for install and reboot](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-37.png)

Repeat the procedure for all the physical hosts that you want to make available remotely.

## Create a machine catalog in Citrix Virtual Desktops service

Use Citrix Virtual Desktops service to create a catalog of the physical machines

1.  Once the trial is approved, **Log in to Citrix Cloud from your local machine**. Scroll to **My Services**, and locate **Virtual Apps and Desktops** service tile, click **Manage**

    ![Citrix Virtual Desktops service - Log in to Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-38.png)

1.  The service overview page is displayed. **Scroll further** down and find the **Workspace Experience URL**. **Bookmark it**.

    ![Citrix Virtual Desktops service - Book mark Workspace URL](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-39.png)

1.  Click the **chevron** next to Manage on the top left. Click **Web Studio (Preview)**

    ![Citrix Virtual Desktops service - Open Web Studio](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-40.png)

1.  In the left menu under Citrix Studio. Click **Machine Catalogs**

    ![Citrix Virtual Desktops service - Open Web Studio](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-41.png)

1.  In the Actions menu(right side). Click **Create Machine Catalog**.

    ![Citrix Virtual Desktops service - Click Create Machine Catalog](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-42.png)

1.  In the Machine Catalog Setup dialog, click **Next**

    ![Citrix Virtual Desktops service - Intro page](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-43.png)

1.  Select **Remote PC Access**. Click **Next**

    ![Citrix Virtual Desktops service - Select Remote PC Access](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-44.png)

1.  Click **Add Machine Accounts** or click **Add OUs** based on whether you want to add machines or OUs (all the physical machines in the OU). In our example we are adding a machine.

    ![Citrix Virtual Desktops service - Add machines accounts or OU](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-45.png)

1.  In the Select Computers pop up, **enter the first few characters of the machine hostname** you want to add. Click **Check Names**

    ![Citrix Virtual Desktops service - Search for machine host name](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-46.png)

1.  If the search returns more than one machine names, **choose the ones you want to add** (hold down the CTRL key to choose more than one). Once you have selected all the machines. Click **OK**

    ![Citrix Virtual Desktops service - Select machine host names](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-47.png)

1.  Repeat the last 2 steps to add all the machines you want to add to the catalog. Then click **OK** in the Select Computers dialog

    ![Citrix Virtual Desktops service - Repeat steps for other machines](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-48.png)

1.  From the **Select the minimum functional level for this catalog** drop-down list, select **1811 (or newer)**. Click **Next**

    ![Citrix Virtual Desktops service - Choose minimum functional level](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-49.png)

1.  **Enter a name** for the machine catalog. Click **Finish**. You will be returned to the Machine Catalogs page.

    ![Citrix Virtual Desktops service - Complete catalog creation](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-50.png)

## Create a Delivery group

1.  From the left side menu click **Delivery Groups** to start creating your delivery group.

    ![Citrix Virtual Desktops service - Open Delivery Groups](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-51.png)

1.  From the Actions menu(right side), click **Create Delivery Group**.

    ![Citrix Virtual Desktops service - Click Create Delivery Group](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-52.png)

1.  In the Create Delivery Group dialog, click **Next**

    ![Citrix Virtual Desktops service - Intro Page](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-53.png)

1.  Select the **catalog you created earlier**. Click **Next**

    ![Citrix Virtual Desktops service - Select Remote PC Access catalog](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-54.png)

1.  Specify which users can access these desktops. For our example we assign the desktops to a group of users, that have a 1:1 mapping for each of the machines in the delivery group for enhanced security. Click the **Restrict use to this Delivery Group to the following users’** radio button. Click **Add**

    ![Citrix Virtual Desktops service - Restrict Delivery group to specific users](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-55.png)

1.  **Add domain users / groups** that you want to have access to the delivery group. You can check their names by clicking **Check Names**. Once you are done click **OK**

    ![Citrix Virtual Desktops service - Select users or groups to be added](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-56.png)

1.  If the search returns more than one user name, **choose the ones you want to add** (hold down the CTRL key to choose more than one). Once you have selected all the users you want to add. Click **OK**

    ![Citrix Virtual Desktops service - Choose the users](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-57.png)

1.  Repeat the last 2 steps for all the users you want to add to the delivery group. Then click **OK** in the Select Users or Groups dialog. Click **Next** in the Create Delivery group dialog

    ![Citrix Virtual Desktops service - Finish user selection](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-58.png)

1.  Click **Add**

    ![Citrix Virtual Desktops service - Add Desktop Assignment Rule](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-59.png)

1.  In the Add Desktops Assignment Rule dialog. **Enter Display Name** for the delivery group. Click **Add** and **add the same or a subset of the users you chose earlier** again. **Ensure Enable desktop assignment rule** check box is checked. Click **OK**

    ![Citrix Virtual Desktops service - Enter display name of Delivery Group and click Add](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-60.png)

1.  Click **Next**

    ![Citrix Virtual Desktops service - Click Next](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-61.png)

1.  **Enter a Delivery Group name**. Click **Finish**

    ![Citrix Virtual Desktops service - Enter Delivery Group name and start creation](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-62.png)

1.  Once the delivery group is created, the Manage Tab looks like this. Click the **Desktops tab** in the Details section. Click **x machine(s)** is/are not assigned to a user.

    ![Citrix Virtual Desktops service - Choose Desktops Tab and click list of unassigned machines](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-63.png)

1.  **Select the machine you want to assign** to a user. Click **Change User** from the Action menu

    ![Citrix Virtual Desktops service - Select machine and click Change User](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-64.png)

1.  Click **Add**

    ![Citrix Virtual Desktops service - Click Add](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-65.png)

1.  **Search for the user** you want to assign to the machine using the **Check Names** button. Once found, click **OK**. Click **Ok** again.

    ![Citrix Virtual Desktops service - Select user and click OK](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-66.png)

Repeat the steps for the rest of the machines to assign each user to their physical machine.

**Note**: The last 4 steps are needed, if you want to assign specific users to specific desktops, else the users will be auto assigned to next available desktop in the delivery group or you can use PowerShell scripts to perform the assignment.

## Launch the session from Citrix Workspace

1.  **Open the Workspace URL** you had saved earlier (from Citrix Cloud) to gain access to the Citrix Workspace. **Log in as a domain user** you have assigned the remote desktop to.

    ![Citrix Virtual Desktops service - Log in to Citrix Workspace](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-67.png)

1.  If this is the first time you are launching a session from the browser, you may get the following pop up. **Ensure Citrix Workspace App is installed** and click **Detect Workspace**

    ![Citrix Virtual Desktops service - Detect Citrix Workspace app](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-68.png)

1.  Click **View All Desktops**. Click the **Remote PC Access delivery group**

    ![Citrix Virtual Desktops service - View All Desktops and launch session](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-69.png)

1.  The session will launch giving the user access to the remote physical PC

    ![Citrix Virtual Desktops service - Session launches](/en-us/tech-zone/learn/media/poc-guides_remote-pc-access_cvd-70.png)

## Summary

The guide walked you through connecting your physical desktops in your office to the Citrix Virtual Desktops service, so users access them remotely.
You learned how use Citrix Virtual Desktops service to allow users to access their desktops on any device from any location.
The process included how to install a Citrix Cloud connector in your on-premises office location, installing Citrix Virtual Delivery agents on the desktop machines. Creating a machine catalog from those machines and then a delivery group.
Assigning users to their machines and then allowing them to connect to those desktops using Citrix Workspace app.

To learn more about Citrix solutions for Business Continuity, read the [Tech Brief](/en-us/tech-zone/learn/tech-briefs/business-continuity.html)
