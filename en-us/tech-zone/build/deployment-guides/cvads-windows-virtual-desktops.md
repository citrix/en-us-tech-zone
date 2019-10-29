---
layout: doc
---
# Deployment guide for Citrix Virtual Apps and Desktops service with Windows Virtual Desktop

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

## Overview

Microsoft announced the general availability of Windows Virtual Desktop on 30th Sept 2019. This deployment guide is designed to help you quickly configure Citrix Virtual Apps and Desktops service with Windows Virtual Desktop for a trial evaluation only, in a hybrid environment. At the end of this Reviewer’s Guide you will be able to bridge your on-premises Citrix Virtual Apps and Desktops deployment with a Microsoft Azure subscription using Citrix Virtual Apps and Desktops service. You will be able to let your users launch a Windows Virtual Desktop virtual app or desktop utilizing the new Windows 10 Multi-Session experience, while also accessing on-premises resources.

## Conceptual Architecture

[![Citrix Virtual Apps and Desktops Service with Windows Virtual Desktop Architecture](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_2.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_2.png)

## Scope

In this deployment guide, you will experience the role of a Citrix Cloud and Microsoft Azure administrator and you will create a hybrid environment that spans your organization’s on-premises deployment and Azure. You will provide access to a virtualization environment consisting of the Windows 10 Multi-Session experience in Windows Virtual Desktop (WVD) and on-premises resources to an end user with Citrix Virtual Apps and Desktops service.

This guide will showcase how to do the following actions:
1.  Create a new Azure subscription and an Azure Active Directory (AAD) Tenant (if you don’t have one)
2.  Connect your on-premises AD to your AAD using Azure AD Connect
3.  Create Master Image using Windows 10 Enterprise for Virtual Desktops
4.  Create a Citrix Cloud account (if you don’t have one)
5.  Request a Citrix Virtual Apps and Desktops service trial 
6.  Create a Citrix Virtual Apps and Desktops service account (Citrix Cloud account) and add the Azure tenant as a Resource Location
7.  Create a Windows Server VM and install the Citrix Cloud Connector in your Azure resource location.
8.  Prepare the Windows Virtual Desktop template for the session host virtual machines (VMs). Install the Citrix Virtual Desktop Agent on the WVD VM 
9.  Utilize your Citrix Virtual Apps and Desktops service account (Citrix Cloud account) to connect to your Azure subscription using the Citrix Cloud Connector.
10.  Use Citrix Machine Creation Services for deploying a catalog and then create a delivery group.
11.  Create a Windows Server VM and install the Citrix Cloud Connector in your on premises Resource Location and add it as a resource location.
12.  Utilize your Citrix Virtual Apps and Desktops service account (Citrix Cloud account) to connect to your on-premises resources using the Citrix Cloud Connector.
13.  Let your users connect to the WVD or on premises sessions via Citrix Workspace.

There is a requirement from Microsoft that the WVD session hosts must be joined to a Windows Active Directory (AD) domain that has been synchronized with either Azure AD using Azure AD Connect or with Azure AD Domain Services. This would require you to connect your on-premises Active Directory to your organization’s Azure subscription. This is out-of-scope for this guide but if you are also a Citrix Networking or Citrix SDWAN customer then you can use Site-to-Site VPN [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) with Citrix ADC https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html (which requires a public IP) or Citrix SDWAN [https://docs.citrix.com/en-us/citrix-sd-wan-center/10-2/azure-virtual-wan.html](https://docs.citrix.com/en-us/citrix-sd-wan-center/10-2/azure-virtual-wan.html). The two preceding options are creating IP-Sec tunnels between your on-premises environment and the WVD network in Azure.

If you are looking for a solution that does much more than just setup a link between these 2 locations, then we suggest considering creating an end to end SDWAN solution. The main advantages this gives you is integrated security, orchestration, and policy based configuration. SDWAN has further benefits:

*  Enables direct access to video-on-demand that is rendered from the customer data center
*  Provides intelligent traffic steering from the VDA to other on-premises properties
*  VoIP and real-time video traffic are navigated from the corporate data center
*  Aggregate more than 1 link to give you resiliency and expanded bandwidth by combining the different links

To set up an end to end SDWAN solution you can follow these guides:

[https://docs.citrix.com/en-us/citrix-sd-wan/10-2/configuration/configuring-virtual-path-service-between-mcn-client-sites.html](https://docs.citrix.com/en-us/citrix-sd-wan/10-2/configuration/configuring-virtual-path-service-between-mcn-client-sites.html)

[https://docs.citrix.com/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html](https://docs.citrix.com/en-us/citrix-sd-wan-platforms/vpx-models/vpx-se/sd-wan-se-on-azure-10-2.html)

[https://docs.citrix.com/en-us/citrix-sd-wan-center/11/deploying-sd-wan-appliance/deploy-citrix-sd-wan-on-azure-from-citrix-sd-wan-center.html](https://docs.citrix.com/en-us/citrix-sd-wan-center/11/deploying-sd-wan-appliance/deploy-citrix-sd-wan-on-azure-from-citrix-sd-wan-center.html)

Express route [https://azure.microsoft.com/en-in/services/expressroute/](https://azure.microsoft.com/en-in/services/expressroute/) or Point-to-Site VPN [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) which doesn’t require a public IP are other options to establish the connectivity.

This guide will provide detailed instructions on how to deploy and configure your environment including VMs, connecting your AD to Azure AD. As a Citrix and Azure tenant administrator, you will create the WVD environment to enable your users to test various scenarios that showcase Citrix Virtual Apps and Desktops service and Windows Virtual Desktop integration.

## Deployment Steps

### Create an Azure Subscription and an Azure Active Directory Tenant

If you are an existing Microsoft O365 customer you should already have an Azure Active Directory (AAD) and so you can login to Azure as the global administrator of the subscription and skip to the next section.

1.  Go to the url: [Sign into Azure](https://signup.azure.com/signup?offer=MS-AZR-0044P&returnUrl=https:%2F%2Faccount.azure.com%2FSubscriptions&l=en-US) and **login to Azure**

![Login to Azure](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_3.png)

2.  Enter you information, click **Next**

![Azure Account Info](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_4.png)

3.  **Verify your identity** and then **provide your credit card details** for billing purposes. You may be asked to verify your card details by making a payment of ~ USD 1

![Azure Identity](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_5.png)

4.  Once you are done you should see this in Azure Portal. If that is the payment method you want to use click **Next**. Else change it and then click **Next**.

![Azure Account Card](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_6.png)

5.  If you agree, check **I agree** check box for subscription agreement, offer details and privacy statement. Click **Sign Up**.

![Azure Sign Up](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_7.png)

6.  Alternately you can enroll in an O365 Enterprise E3 trial by going to this link https://signup.microsoft.com/Signup?OfferId=B07A1127-DE83-4a6d-9F85-2C104BDAE8B4&dl=ENTERPRISEPACK&culture=en-US&country=US&ali=1 and providing your details

![O365 enrollment](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_8.png)

7.  Click **+ Create a Resource** and search for **Azure Active Directory** and select it

![Azure Active Directory](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_9.png)

8.  Click **Create**

![Create Azure Active Directory](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_10.png)

9.  Provide the **Organization name** and **Initial domain name** of the AD that you wish to create. Select the **Country or Region** and then click **Create**

![Create Azure Active Directory](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_11.png)

10.  Wait for the Azure AD to be created

![Create Azure Active Directory - Wait](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_12.png)

### Connect the on premises AD to Azure AD using Azure AD Connect

1.  Launch an RDP session to the AD.

![Connect to AD](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_13.png)

2.  Open a browser and login to Azure as the global administrator of the subscription and Azure AD. Click **Azure Active Directory** and then **Azure AD Connect**. Click **Download Azure AD Connect**

![Download Azure AD connect](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_14.png)

3.  In the browser window that opens click **Download**

![Download Azure AD connect](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_15.png)

4. Click **Run**

![Run Azure AD connect](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_16.png)

5.  In the Azure AD connect dialog, click **Continue.

![Azure AD connect Exec](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_17.png)

6.  Click **Use Express Settings**

![Azure AD connect Express](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_18.png)

7.  Provide the Azure Active Directory global administrator **Username and Password**. Click **Next** and **login again** if requested

![Azure AD connect Express](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_19.png)

8.  Provide the Active Directory enterprise administrator **Username and Password**. Click **Next**.

![Azure AD connect AD creds](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_20.png)

9. Check the **Continue without matching all UPN suffixes to verified domains**, click **Next**

![Azure AD connect AD creds](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_21.png)

10.  Click **Install**

![Azure AD connect AD Install](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_22.png)

11.  Once the config is complete. Click **Exit**

![Azure AD connect AD Done](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_23.png)

12.  Go back to the Azure Active Directory page in the Azure portal and click **Users**. Validate that the user(s) you created are visible in the list.

![Azure AD connect AD Done](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_24.png)

### Create a master image using Windows 10 Enterprise for Virtual Desktops

1.  Select **+** or **+ Create a resource**. Search for **Microsoft Windows 10** and select the Microsoft Windows 10 option that shows Windows 10 Enterprise 2016 LTSB in the drop down.

![Master Image - Win10 search](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_25.png)

2.  Select one of the **Windows 10 Enterprise multi-session** options from the dropdown and then click **Create**

![Master Image - Win10 select](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_26.png)

3.  Select the appropriate **Subscription** and the **Resource group** created for WVD to deploy the machine in. Provide a **name** for the **Master Image VM**. Choose the same **region as the AD VM**. Enter the **credentials for the administrator account**. Click **Next: Disks**

![Master Image - Basics](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_27.png)

4.  Select the **appropriate OS disk type** for your deployment. Click **Next: Networking**

![Master Image - Disks](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_28.png)

5.  Select the **virtual network** that your other VMs are on and ensure that a **Public IP** is being created for the Master Image. Click **Review + Create**

![Master Image - NW](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_29.png)

6.  Ensure that the **Validation Passed** message appears and **check the machine settings**. Click **Create** to begin the Master Image VM creation

![Master Image - Review](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_30.png)

7.  Once the VM creation completes. Click **Go to resource**.

![Master Image - Created](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_31.png)

8.  The VM needs to have a networking rule to allow incoming RDP traffic on it Public IP. Click **Networking** in the Favorites column. Click **Add inbound port rule**

![Master Image - Inbound Rule](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_32.png)

9.  Your public IP can be obtained by running a google search for “whatsmyip” address to make RDP connections to the AD VM.
Select **IP Address** as Source, enter the **Public IP Address of the machine** you wish to connect from in the Source IP field, set Destination Port ranges to **3389**, and **Protocol** to **TCP**. Set an appropriate **Priority value** and provide a **name to the rule.** * Click **Add**

![Master Image - Inbound Rule Create](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_33.png)

*: Please note that leaving port 3389 open remotely long-term could pose a security risk.

10.  **RDP in to the machine with the admin credentials** you provided when creating the VM and **join the VM to the domain** and **reboot** the machine.

### Create a Cloud Connector in your Azure subscription

1.  Select **+** or **+ Create a resource** in Azure. Select **Windows Server 2016 Datacenter** under Get Started to create a new Windows Server 2016 machine.

![Cloud Connector - Create](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_34.png)

2.  Select the appropriate **Subscription** and the **Resource group** created for WVD to deploy the machine in. Provide a **name for the Cloud connector VM**. Chose the **same region as the AD** VM. Enter the **credentials for the administrator account**. Click **Next: Disks**

![Cloud Connector - Basics](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_35.png)

3.  Select the **appropriate OS disk type** for your deployment. Click **Next: Networking**

![Cloud Connector - Disks](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_36.png)

4.  Select the **virtual network** that your other VMs are on and ensure that a **Public IP** is being created for the Cloud Connector VM. Click **Review + Create**

![Cloud Connector - NW](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_37.png)

5.  Ensure that the **Validation Passed** message appears and **check the machine settings**. Click **Create** to begin the Cloud connector VM creation

![Cloud Connector - Review](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_38.png)

6.  Once the VM creation completes. Click **Go to Resource**

![Cloud Connector - Created](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_39.png)

7.  The VM needs to have a networking rule to allow incoming RDP traffic on it Public IP. * Click **Networking** in the favorites column and then click on the **name of the network interface**

![Cloud Connector - NW Interface](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_40.png)

8.  Click **Network Security Group** and then click **Edit**

![Cloud Connector - NW Security Group Edit](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_41.png)

9.  Click the **Network security group**

![Cloud Connector - NW Security Group Select](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_42.png)

10.  Select the **Network Security group** of your WVD VM as it already has the port rules to allow access to your machine. Click **Save** *

![Cloud Connector - NW Security Group Set](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_43.png)

*: Please note that leaving port 3389 open remotely long-term could pose a security risk.

11.  RDP in to the machine with the admin credentials you provided when creating the VM and join the Cloud Connector VM to the domain and reboot the machine.

### Create a Citrix Cloud account

1.  RDP to the Cloud Connector VM and login as the AD admin. Goto the URL: https://citrix.cloud.com. If you are an existing Citrix Cloud customer go to Create a new Resource Location section. Please ensure that you have an active Citrix cloud account. If your account has expired you will need to contact sales to enable it.

2.  If you are new to Citrix Cloud click Sign up and try it free in the bottom left.
![Citrix Cloud - Sign Up](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_44.png)

3.  Enter your **business details** and **check** the **“I’ve read, understand and agree to the Terms of Service”** check box, if you agree. Click **Continue**.

![Citrix Cloud - Details](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_45.png)

4.  Select an **appropriate region** to host your Citrix cloud deployment. Click **Continue**

![Citrix Cloud - Region](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_46.png)

5.  Verify your email id, by **clicking the link** sent to you **in your email**.

![Citrix Cloud - Verify Email](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_47.png)

6.  Open the email and click on the **Confirm Your Account** link.

![Citrix Cloud - Confirm email](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_48.png)

7.  Once the account is confirmed, **enter and confirm the password**. Click **Create Account**

![Citrix Cloud - Create Account](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_49.png)

8.  Click **Sign In**

![Citrix Cloud - Sign In](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_50.png)

### Create a new Resource Location

1.  Enter **Username and Password**. Click **Sign In**. (If your account manages more than one customer select the appropriate one)

![Resource Location - Login](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_51.png)

2.  Under Resource Locations Click **Edit or Add New**

![Resource Location - Edit or New](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_52.png)

3.  Click + Resource Location and enter name of the New Resource Location. Click Save

![Resource Location - Create](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_53.png)

4.  Under the newly created resource location click + Cloud Connectors

![Resource Location - + Cloud Connector](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_54.png)

5.  Click Download. Click Run when the download begins

![Resource Location - Download](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_55.png)

6.  Citrix Cloud connectivity test successful message should be displayed. Click Close.
If the test fails, check the following link to resolve the issue - https://support.citrix.com/article/CTX224133 

![Resource Location - Connectivity Check](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_56.png)

7.  Click Sign In and Sign in to Citrix Cloud

![Resource Location - Sign In](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_57.png)

8.  From the drop downs select the appropriate Customer and Resource Location. Click Install

![Resource Location - Install](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_58.png)

9.  Once the installation completes. A service connectivity test will run. Let it complete and you should again see a successful result. Click Close

![Resource Location - Done](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_59.png)

10.  Refresh the Resource Location page in Citrix Cloud. Click on Cloud Connectors

![Cloud Connector - Refresh list](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_60.png)

11.  The newly added Cloud Connector is listed. In Production environments, ensure to have 2 Cloud Connectors per Resource Location

![Cloud Connector - Verify](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_61.png)

![Cloud Trail - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_62.png)
![Cloud Trail - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_63.png)
![Cloud Trail - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_64.png)


![VDA Install - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_65.png)
![VDA Install - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_66.png)
![VDA Install - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_67.png)
![VDA Install - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_68.png)
![VDA Install - ](/en-us/tech-zone/build/media/deployment-guides_cvads-windows-virtual-desktops_69.png)

