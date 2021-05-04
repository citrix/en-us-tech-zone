---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
description: Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and on-premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.
tz_title: Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid
tz_products: citrix-virtual-apps-and-desktops;
---
# PoC Guide: Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid

## Overview

Microsoft announced the general availability of Windows Virtual Desktop on 30th Sept 2019. This proof of concept (PoC) guide is designed to help you quickly configure Citrix Virtual Apps and Desktops service with Windows Virtual Desktop for a trial evaluation only, in a hybrid environment. At the end of this PoC guide you will be able to bridge your on-premises Citrix Virtual Apps and Desktops deployment with a Microsoft Azure subscription using Citrix Virtual Apps and Desktops service. You will be able to let your users launch a Windows Virtual Desktop virtual app or desktop utilizing the new Windows 10 Multi-Session experience, while also accessing on-premises resources.

## Conceptual Architecture

[![Citrix Virtual Apps and Desktops Service with Windows Virtual Desktop Architecture](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_2.png)](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_2.png)

## Scope

In this PoC guide, you experience the role of a Citrix Cloud and Microsoft Azure administrator and create a hybrid environment that spans your organization’s on-premises deployment and Azure. You provide access to a virtualization environment consisting of the Windows 10 Multi-Session experience in Windows Virtual Desktop (WVD) and on-premises resources to an end user with Citrix Virtual Apps and Desktops service.

This guide showcases how to perform the following actions:

1.  Create a new Azure subscription and an Azure Active Directory (AAD) Tenant (if you don’t have one)
2.  Connect your on-premises AD to your AAD using Azure AD Connect
3.  Create Master Image using Windows 10 Enterprise for Virtual Desktops
4.  Create a Citrix Cloud account (if you don’t have one)
5.  Request a Citrix Virtual Apps and Desktops service trial
6.  Create a Citrix Virtual Apps and Desktops service account (Citrix Cloud account) and add the Azure tenant as a Resource Location
7.  Create a Windows Server VM and install the Citrix Cloud Connector in your Azure resource location
8.  Prepare the Windows Virtual Desktop template for the session host virtual machines (VMs). Install the Citrix Virtual Delivery Agent on the WVD VM
9.  Utilize your Citrix Virtual Apps and Desktops service account (Citrix Cloud account) to connect to your Azure subscription using the Citrix Cloud Connector
10.  Use Citrix Machine Creation Services for deploying a catalog and then create a delivery group
11.  Create a Windows Server VM and install the Citrix Cloud Connector in on-premises Resource Location and add it as a resource location
12.  Utilize your Citrix Virtual Apps and Desktops service account (Citrix Cloud account) to connect to your on-premises resources using the Citrix Cloud Connector
13.  Let your users connect to the WVD or on premises sessions via Citrix Workspace

There is a requirement from Microsoft that the WVD session hosts must be joined to a Windows Active Directory (AD) domain that has been synchronized with either Azure AD using Azure AD Connect or with Azure AD Domain Services. This would require you to connect your on-premises Active Directory to your organization’s Azure subscription. This is out-of-scope for this guide but if you are also a Citrix Networking or Citrix SD-WAN customer then you can use [Site-to-Site VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) with [Citrix ADC](/en-us/citrix-adc/current-release/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) (which requires a public IP) or [Citrix SD-WAN](/en-us/citrix-sd-wan-center/11-2/azure-virtual-wan.html). The two preceding options are creating IPsec tunnels between your on-premises environment and the WVD network in Azure.

If you are looking for a solution that does much more than just set-up a link between these 2 locations, then we suggest considering creating an end to end SDWAN solution. The main advantages this gives you are integrated security, orchestration, and policy based configuration. SDWAN has further benefits:

*  Enables direct access to video-on-demand that is rendered from the customer data center
*  Provides intelligent traffic steering from the VDA to other on-premises properties
*  VoIP and real-time video traffic are navigated from the corporate data center
*  Aggregate more than 1 link to give you resiliency and expanded bandwidth by combining the different links

To set up an end to end SDWAN solution you can follow these guides:

[Configuring SDWAN to connect to Azure virtual network](/en-us/citrix-sd-wan/11-2/configuration/configuring-virtual-path-service-between-mcn-client-sites.html)

[Deplopy SDWAN on Azure](/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html)

[Deploy SDWAN on Azure using SDWAN Center](/en-us/citrix-sd-wan-center/11-2/deploying-sd-wan-appliance/deploy-citrix-sd-wan-on-azure-from-citrix-sd-wan-center.html)

[Express route](https://azure.microsoft.com/en-in/services/expressroute/) or [Point-to-Site VPN](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) which doesn’t require a public IP are other options to establish the connectivity.

This guide provides detailed instructions on how to deploy and configure your environment including VMs, connecting your AD to Azure AD. As a Citrix and Azure tenant administrator, you create the WVD environment to enable your users to test various scenarios that showcase Citrix Virtual Apps and Desktops service and Windows Virtual Desktop integration.

## Create an Azure Subscription and an Azure Active Directory Tenant

If you are an existing Microsoft O365 customer you should already have an Azure Active Directory (AAD) and so you can log in to Azure as the global administrator of the subscription and skip to the next section.

1.  Go to the url: [Sign into Azure](https://signup.azure.com/signup?offer=MS-AZR-0044P&returnUrl=https:%2F%2Faccount.azure.com%2FSubscriptions&l=en-US) and **login to Azure**

    ![Log in to Azure](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_3.png)

1.  Enter you information, click **Next**

    ![Azure Account Info](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_4.png)

1.  **Verify your identity** and then **provide your credit card details** for billing purposes. You may be asked to verify your card details by making a payment of ~ USD 1

    ![Azure Identity](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_5.png)

1.  Once you are done you should see this in Azure Portal. If that is the payment method you want to use click **Next**. Else change it and then click **Next**.

    ![Azure Account Card](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_6.png)

1.  If you agree, check **I agree** check box for subscription agreement, offer details and privacy statement. Click **Sign Up**.

    ![Azure Sign Up](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_7.png)

1.  Alternately you can enroll in an O365 Enterprise E3 trial by going to this [link](https://signup.microsoft.com/Signup?OfferId=B07A1127-DE83-4a6d-9F85-2C104BDAE8B4&dl=ENTERPRISEPACK&culture=en-US&country=US&ali=1) and providing your details

    ![O365 enrollment](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_8.png)

1.  Click **+ Create a Resource** and search for **Azure Active Directory** and select it

    ![Azure Active Directory](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_9.png)

1.  Click **Create**

    ![Create Azure Active Directory](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_10.png)

1.  Provide the **Organization name** and **Initial domain name** of the AD that you want to create. Select the **Country or Region** and then click **Create**

    ![Create Azure Active Directory](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_11.png)

1.  Wait for the Azure AD to be created

    ![Create Azure Active Directory - Wait](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_12.png)

## Connect the on premises AD to Azure AD using Azure AD Connect

1.  Launch an RDP session to the AD.

    ![Connect to AD](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_13.png)

1.  Open a browser and login to Azure as the global administrator of the subscription and Azure AD. Click **Azure Active Directory** and then **Azure AD Connect**. Click **Download Azure AD Connect**

    ![Download Azure AD connect](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_14.png)

1.  In the browser window that opens click **Download**

    ![Download Azure AD connect](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_15.png)

1.  Click **Run**

    ![Run Azure AD connect](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_16.png)

1.  In the Azure AD connect dialog, click **Continue**

    ![Azure AD connect Exec](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_17.png)

1.  Click **Use Express Settings**

    ![Azure AD connect Express](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_18.png)

1.  Provide the Azure Active Directory global administrator **Username and Password**. Click **Next** and **login again** if requested

    ![Azure AD connect Express](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_19.png)

1.  Provide the Active Directory enterprise administrator **Username and Password**. Click **Next**.

    ![Azure AD connect AD credentials](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_20.png)

1.  Check the **Continue without matching all UPN suffixes to verified domains**, click **Next**

    ![Azure AD connect AD credentials](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_21.png)

1.  Click **Install**

    ![Azure AD connect AD Install](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_22.png)

1.  Once the config is complete, click **Exit**

    ![Azure AD connect AD Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_23.png)

1.  Go back to the Azure Active Directory page in the Azure portal and click **Users**. Validate that the user(s) you created are visible in the list.

    ![Azure AD connect AD Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_24.png)

## Create a master image using Windows 10 Enterprise for Virtual Desktops

1.  Select **+** or **+ Create a resource**. Search for **Microsoft Windows 10** and select the Microsoft Windows 10 option that shows Windows 10 Enterprise 2016 LTSB in the drop-down list.

    ![Master Image - Win10 search](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_25.png)

1.  Select one of the **Windows 10 Enterprise multi-session** options from the drop-down list and then click **Create**

    ![Master Image - Win10 select](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_26.png)

1.  Select the appropriate **Subscription** and the **Resource group** created for WVD to deploy the machine in. Provide a **name** for the **Master Image VM**. Choose the same **region as the AD VM**. Enter the **credentials for the administrator account**. Click **Next: Disks**

    ![Master Image - Basics](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_27.png)

1.  Select the **appropriate OS disk type** for your deployment. Click **Next: Networking**

    ![Master Image - Disks](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_28.png)

1.  Select the **virtual network** that your other VMs are on and ensure that a **Public IP** is being created for the Master Image. Click **Review + Create**

    ![Master Image - NW](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_29.png)

1.  Ensure that the **Validation Passed** message appears and **check the machine settings**. Click **Create** to begin the Master Image VM creation

    ![Master Image - Review](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_30.png)

1.  Once the VM creation completes, click **Go to resource**.

    ![Master Image - Created](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_31.png)

1.  The VM must have a networking rule to allow incoming RDP traffic on it Public IP. Click **Networking** in the Favorites column. Click **Add inbound port rule**

    ![Master Image - Inbound Rule](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_32.png)

1.  Your public IP can be obtained by running a google search for `whatsmyip` address to make RDP connections to the AD VM.
Select **IP Address** as Source, enter the **Public IP Address of the machine** you want to connect from in the Source IP field, set Destination Port ranges to **3389**, and **Protocol** to **TCP**. Set an appropriate **Priority value** and provide a **name to the rule.** * Click **Add**

    ![Master Image - Inbound Rule Create](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_33.png)

    *: Leaving port 3389 open remotely long-term can pose a security risk.

1.  **RDP in to the machine with the admin credentials** you provided when creating the VM and **join the VM to the domain** and **reboot** the machine.

## Create a Cloud Connector in your Azure subscription

1.  Select **+** or **+ Create a resource** in Azure. Select **Windows Server 2016 Datacenter** under Get Started to create a new Windows Server 2016 machine.

    ![Cloud Connector - Create](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_34.png)

1.  Select the appropriate **Subscription** and the **Resource group** created for WVD to deploy the machine in. Provide a **name for the Cloud connector VM**. Choose the **same region as the AD** VM. Enter the **credentials for the administrator account**. Click **Next: Disks**

    ![Cloud Connector - Basics](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_35.png)

1.  Select the **appropriate OS disk type** for your deployment. Click **Next: Networking**

    ![Cloud Connector - Disks](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_36.png)

1.  Select the **virtual network** that your other VMs are on and ensure that a **Public IP** is being created for the Cloud Connector VM. Click **Review + Create**

    ![Cloud Connector - NW](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_37.png)

1.  Ensure that the **Validation Passed** message appears and **check the machine settings**. Click **Create** to begin the Cloud connector VM creation

    ![Cloud Connector - Review](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_38.png)

1.  Once the VM creation completes, click **Go to Resource**

    ![Cloud Connector - Created](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_39.png)

1.  The VM must have a networking rule to allow incoming RDP traffic on it Public IP. Click **Networking** in the favorites column and then click the **name of the network interface**

    ![Cloud Connector - NW Interface](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_40.png)

1.  Click **Network Security Group** and then click **Edit**

    ![Cloud Connector - NW Security Group Edit](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_41.png)

1.  Click the **Network security group**

    ![Cloud Connector - NW Security Group Select](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_42.png)

1.  Select the **Network Security group** of your WVD VM as it already has the port rules to allow access to your machine. Click **Save** *

    ![Cloud Connector - NW Security Group Set](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_43.png)

    *: Leaving port 3389 open remotely long-term can pose a security risk.

1.  RDP in to the machine with the admin credentials you provided when creating the VM and join the Cloud Connector VM to the domain and reboot the machine.

## Create a Citrix Cloud account

1.  RDP to the Cloud Connector VM and login as the AD admin. Goto the URL: [Citrix Cloud](https://citrix.cloud.com). If you are an existing Citrix Cloud customer go to Create a new Resource Location section. Ensure that you have an active Citrix Cloud account. If your account has expired you need to contact sales to enable it.

1.  If you are new to Citrix Cloud click Sign-up and try it free in the bottom left.

    ![Citrix Cloud - Sign Up](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_44.png)

1.  Enter your **business details** and **check** the **“I’ve read, understand, and agree to the Terms of Service”** check box, if you agree. Click **Continue**.

    ![Citrix Cloud - Details](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_45.png)

1.  Select an **appropriate region** to host your Citrix Cloud deployment. Click **Continue**

    ![Citrix Cloud - Region](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_46.png)

1.  Verify your email id, by **clicking the link** sent to you **in your email**.

    ![Citrix Cloud - Verify Email](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_47.png)

1.  Open the email and click the **Confirm Your Account** link.

    ![Citrix Cloud - Confirm email](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_48.png)

1.  Once the account is confirmed, **enter and confirm the password**. Click **Create Account**

    ![Citrix Cloud - Create Account](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_49.png)

1.  Click **Sign In**

    ![Citrix Cloud - Sign In](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_50.png)

## Create a new Resource Location

1.  Enter **Username and Password**. Click **Sign In**. (If your account manages more than one customer select the appropriate one)

    ![Resource Location - Login](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_51.png)

1.  Under Resource Locations Click **Edit or Add New**

    ![Resource Location - Edit or New](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_52.png)

1.  Click **+ Resource Location** and enter **name of the New Resource Location**. Click **Save**

    ![Resource Location - Create](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_53.png)

1.  Under the newly created resource location click **+ Cloud Connectors**

    ![Resource Location - + Cloud Connector](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_54.png)

1.  Click **Download**. Click **Run** when the download begins

    ![Resource Location - Download](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_55.png)

1.  Citrix Cloud connectivity test successful message should be displayed. Click **Close**.
If the test fails, check the following link to resolve the issue - [CTX224133](https://support.citrix.com/article/CTX224133)

    ![Resource Location - Connectivity Check](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_56.png)

1.  Click **Sign In** and sign in to Citrix Cloud

    ![Resource Location - Sign In](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_57.png)

1.  From the drop-down lists select the appropriate **Customer and Resource Location**. Click **Install**

    ![Resource Location - Install](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_58.png)

1.  Once the installation completes, a service connectivity test runs. Let it complete and you should again see a successful result. Click **Close**

    ![Resource Location - Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_59.png)

1.  Refresh the Resource Location page in Citrix Cloud. Click on **Cloud Connectors**

    ![Cloud Connector - Refresh list](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_61.png)

1.  The newly added Cloud Connector is listed. In production environments, ensure to have 2 Cloud Connectors per resource location

    ![Cloud Connector - Verify](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_62.png)

## Request a Citrix Virtual Apps and Desktops service trial

1.  Click **Citrix Cloud** on top of the page. If you don’t have a Virtual Apps and Desktops Service trial enabled, **scroll down** to **Available Services** and click **Request Demo** in the Virtual Apps and Desktops Service tile

    ![Cloud Trail - Request Demo](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_63.png)

1.  Click **Request a call**

    ![Cloud Trail - Request call](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_64.png)

1.  **Enter your details** and in Comments section specify **“Windows Virtual Desktop Trial”**. Click **Submit**

    ![Cloud Trail - Submit](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_65.png)

## Install Virtual Delivery Agent on the WVD host VM

While we wait, we can install the Citrix Virtual Apps and Desktops, Virtual Delivery Agent on the Windows 10 Multiuser VM that we created.

1.  Connect to the **WVD VM via RDP as the domain admin**

    ![VDA Install - RDP](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_66.png)

1.  Open **Citrix.com** in Internet Explorer. Hover over **Sign in** and click **My Account**

    ![VDA Install - Citrix.com](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_67.png)

1.  Sign in with your **Username and Password**. Click **Downloads** in the top menu

    ![VDA Install - Sing In](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_68.png)

1.  From the **Select a Product** drop-down list, select **Citrix Virtual Apps and Desktops**

    ![VDA Install - Select CVAD](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_69.png)

1.  In the page that opens, select the **latest version of Citrix Virtual Apps and Desktops 7** (without the .x at the end)

    ![VDA Install - Choose latest](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_70.png)

1.  Scroll down to **Components that are on the product ISO but also packaged separately**. Click the **chevron** to expand the section. Click **Download File** under Server OS Virtual Delivery Agent

    ![VDA Install - Select components](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_71.png)

1.  **Check** “I have read and certify that I comply with the above Export Control Laws” check box, if you agree. Click **Accept**. The download should begin.

    ![VDA Install - Agree terms](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_72.png)

1.  **Save** the file and **Run** it when the download completes

    ![VDA Install - Run](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_73.png)

1.  Click **Next** in the Environment section to create a master MCS image.

    ![VDA Install - Environment](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_74.png)

1.  In the Core Components section, check the Citrix Workspace app check box if your users would use the session to launch sessions from within it. Click **Next**

    ![VDA Install - Workspace App](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_75.png)

1.  In the Additional section choose the components you need and click **Next**

    ![VDA Install - Additional Components](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_76.png)

    **NOTE:** To see logon information in Citrix Director, select also Citrix User Profile Manager

1.  Enter the **UPN for the Cloud Connector** VM and click **Test Connection**. Ensure that the test is successful a green tick appears next to the entered UPN. Click **Add** and click **Next**

    ![VDA Install - CC UPN](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_77.png)

1.  Click **Next** in the Feature section and **Next** again in the Firewall section.

    ![VDA Install - Next](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_78.png)

1.  Click **Install** in the Summary section

    ![VDA Install - Install](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_79.png)

1.  Once the installation completes, in the Diagnostics section click **Connect**. Enter your **Citrix Cloud credentials**, click **OK**. Once the credentials are validated, click **Next**

    ![VDA Install - Connect to Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_80.png)

1.  Click **Finish** and let the VM **reboot**.

    ![VDA Install - Finish](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_81.png)

## Create a hosting connection between Citrix Virtual Apps and Desktops and Azure

Configure Citrix Virtual Apps and Desktops service to connect to the Azure Subscription that hosts the Windows Virtual Desktop VMs.

1.  Once the trial is approved, **Log in to Citrix Cloud** from your local machine. Scroll to **My Services**, and locate **Virtual Apps and Desktops** service tile, click **Manage**

    ![Hosting Connect - Manage](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_82.png)

1.  Click **Manage Service**. If you scroll further down you see the **Workspace Experience URL. Bookmark it**.

    ![Hosting Connect - Bookmark](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_83.png)

1.  In the left menu under Configuration. Click **Hosting** and then click **Add Connection and Resources** that host the machines.

    ![Hosting Connect - Add Hosting](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_84.png)

1.  From the drop downs select **Microsoft© AzureTM** as Connection Type, **Azure Global** for Azure environment and an
**appropriate Zone** for Zone name. Leave **Studio Tools** selected. Click **Next**

    ![Hosting Connect - Select Azure](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_85.png)

1.  **Hover over the gray notch** just below the Manage text (center of the screen). A **workspace icon** slides down **click it**

    ![Hosting Connect - Workspace tool](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_86.png)

1.  **Copy your Azure Subscription ID** to the clipboard. Click the **Open clipboard button** in the middle and **paste the Subscription ID** in it. Click anywhere outside the text box

    ![Hosting Connect - Clipboard](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_87.png)

1.  **Paste the ID** in the Subscription ID text box and enter a **Connection Name**. Click **Create New** to create a new service principal. Alternately you can manually grant Citrix Cloud Access to the Azure subscription (with more restrictive roles than the default contributor) [CTX224110](https://support.citrix.com/article/CTX224110)

    ![Hosting Connect - New Connection](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_88.png)

1.  **Sign in** to your Azure account when prompted. **Ensure that the user is an owner and not an external user** in the subscription

    ![Hosting Connect - Sign in](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_89.png)

1.  Check the **Consent on behalf of your organization** check box and click **Accept** if you agree. Once the validation completes Connected is displayed. Click **Next**

    ![Hosting Connect - Agree terms](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_90.png)

1.  Select the **appropriate Region** and click **Next**

    ![Hosting Connect - Region](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_91.png)

1.  Enter a **Name for the network** and select the appropriate **Virtual network** and **Subnet** where the VMs are to be created. Click **Next**. Review the Summary and click **Finish**

    ![Hosting Connect - NW & Finish](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_92.png)

1.  You are returned to the Hosting page. Once you are done click **Machine Catalogs** to start creating your catalog.

    ![Hosting Connect - Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_93.png)

## Create a Machine Catalog and a Delivery Group

1.  Click **Create Machine Catalog** (right side menu). In the Machine Catalog Setup dialog click **Next**

    ![Catalog and DG - Create](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_94.png)

1.  Ensure **Multi-Session OS** is selected. Click **Next**

    ![Catalog and DG - Multi Session OS](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_95.png)

1.  Ensure **Machines that are power managed** and **Citrix Machine Creation service** are selected and the correct Azure network is shown in the Resources. Click **Next**

    ![Catalog and DG - Power Mgmt & MCS](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_96.png)

1.  Choose the correct **disk that is associated with the WVD VM**. From the minimum functional level drop-down **select 1811 (or newer)**. Click **Next**. A pop-up appears to ask for the VM attached to the VHD to be stopped.

    ![Catalog and DG - Master Image & Functional Level](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_97.png)

1.  **Log in to the Azure portal** and Under **Virtual Machines**, go to the **WVD VM** and Click the **Stop** button.
Ignore the warning about losing the Public IP. Wait for **status to show Stopped (deallocated)**. Return to the **Citrix Cloud tab** and click **Close**

    ![Catalog and DG - Stop VM](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_98.png)

1.  Leave **Defaults** and click **Next**

    ![Catalog and DG - Storage](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_99.png)

1.  **Modify the number of VMs** if you want and select the **machine size** you want for your VMs. Click **Next**

    ![Catalog and DG - VM no & size](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_100.png)

1.  Set the **write back cache size** if you want it. Click **Next**

    ![Catalog and DG - Cache](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_101.png)

1.  Click **Next** and click **Close**

    ![Catalog and DG - Warning](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_102.png)

1.  Click **Next**

    ![Catalog and DG - NIC](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_103.png)

1.  Select the **OU** in which the VMs should be placed. Enter the computer **Account naming scheme**. Ensure the name is fewer than 15 chars long and ends with a #. Click **Next**

    ![Catalog and DG - AD accounts](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_104.png)

1.  Click **Enter Credentials**. In the dialog that opens enter **username and password** of the AD admin. Click **OK**. Click **Next**

    ![Catalog and DG - AD Admin credentials](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_105.png)

1.  Enter a **name** for the machine catalog. Click **Finish**

    ![Catalog and DG - Catalog name](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_106.png)

1.  Wait for the catalog to be created.

    ![Catalog and DG - Creation](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_107.png)

1.  You are returned to the Machine Catalogs page. From the left side menu click **Delivery Groups** to start creating your delivery group.

    ![Catalog and DG - Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_108.png)

1.  Click **Create Delivery Group** from the right side menu. In the Create Delivery Group dialog that opens click **Next**

    ![Catalog and DG - Create DG](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_109.png)

1.  Select the **WVD Catalog**. **Increment the number of machines** to the number of VMS you want to add to the delivery group. Click **Next**

    ![Catalog and DG - Select machines](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_110.png)

1.  For our example we assign the users to a particular group. Click the **Restrict use to this Delivery Group to the following users’** radio button. Click **Add**

    ![Catalog and DG - Users restriction](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_111.png)

1.  **Add domain users** that you want to have access to the delivery group. You can check their names by clicking **Check Names**. Once you are done click **OK**

    ![Catalog and DG - Apps](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_112.png)

1.  If you want to also make apps available from this delivery group click the **Add** drop-down list and choose **From Start Menu**

    ![Catalog and DG - Choose Apps](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_113.png)

1.  From the Add Applications from Start Menu Dialog **check the boxes next to the apps** you want to make available. Then click **OK**

    ![Catalog and DG - Apps done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_114.png)

1.  Click **Next**

    ![Catalog and DG - Add Desktops](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_115.png)

1.  In the Desktops section click **Add**. Enter **Display Name** for the delivery group. Ensure **Enable desktop** is checked. Click **OK**

    ![Catalog and DG - Display Name & Enable](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_116.png)

1.  Click **Next**

    ![Catalog and DG - Finish](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_117.png)

1.  Enter a **Delivery Group name**. Click **Finish**

    ![Catalog and DG - Done](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_118.png)

1.  Once the delivery group is created, the Manage Tab should look like this.

    ![Catalog and DG - DG list](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_119.png)

If you want to add your on-premises resources to the Workspace follow the below steps.

## Create a Cloud Connector in your on-premises data center

1.  Add a cloud connector in your on-premises data center. Create a Windows server 2012 R2 or 2016 VM in your on-premises. Repeat the steps in the “[Create a new Resource Location](#create-a-new-resource-location)” section.

## Add on-premises site to the Workspace

1.  Follow the steps in the [guide](/en-us/citrix-cloud/workspaces/add-on-premises-site.html#prerequisites) to add the on-premises site to the Workspace. Complete till the end of Task 3: Configure connectivity and confirm settings in this page.

![On-prem Site in WS](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_120.png)

## Launch the session from Citrix Workspace

1.  **Open the Workspace URL** you had saved earlier (from Citrix Cloud) to the Citrix Workspace. **Log in as one of the domain users** that you had added to the Delivery Group.

    ![Launch Session - Open WS](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_121.png)

1.  Click **View all Desktops**

    ![Launch Session - Desktops list](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_122.png)

1.  Click on the **Windows 10 Multi-session DG** desktop that you created in Azure.

    ![Launch Session - Click desktop](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_123.png)

1.  The session should launch giving you access to the Windows Virtual Desktop

    ![Launch Session - Launch](/en-us/tech-zone/learn/media/poc-guides_cvads-windows-virtual-desktops_124.png)

## Summary

The guide walked you through bringing your Azure hosted Windows Virtual Desktop and on premises resources (using Workspace Configuration) together, so users access them in one place. You learned how to create a hybrid setup to manage both Windows Virtual Desktops based VMs and on premises based resources using Citrix VIrtual Apps and Desktops. The process included creating a network conneciton between the Azure virtual network and your on premises data center. Also you learned how to synchronize your on premises Active Directory with Azure Active Directory with Azure AD connect. We even looked at how to create a Citrix Cloud account, if you didn't have one and get access to Citrix Virtual Apps and Desktops service, which makes all this work.

To learn more about migrating your on premises Citrix Virtual Apps and Desktops setup to the cloud, read the [deployment guide](/en-us/tech-zone/build/deployment-guides/cvads-migration.html)
