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

5.  
