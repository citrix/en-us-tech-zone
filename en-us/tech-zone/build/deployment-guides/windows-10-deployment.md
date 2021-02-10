---
layout: doc
h3InToc: true
contributedBy: Russell Peters
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices
---
# Windows 10 Deployment Guide

## Overview

This guide provides high level instructions to install the Citrix Virtual Delivery Agent (VDA) on Windows 10. Deploy Virtual Machines (VMs) with Citrix Machine Creation Services (MCS) via Citrix Virtual Apps & Desktops (CVAD) Service

The following steps covered in this Guide

*  Install VDA
*  Optimize Windows 10 for MCS
*  Create a Machine Catalog & deploy VMs
*  Create a Delivery Group & assign VMs
*  Configure Citrix Workspace & Citrix Files clients

## Prerequisites

> **Note**
>
> This guide assumes that the reader has a basic understanding of the following technologies:
>
> Citrix Cloud, CVAD Service & basic Windows administration

1.  A Windows 10 VM in Microsoft Azure, Google Cloud Platform (GCP) or an on-premises virtual environment

1.  A Citrix Cloud account with CVAD Service permissions

1.  A domain account with the ability to create machine accounts in the desired OU

1.  A Citrix.com account to download software

      *  Citrix VDA
      *  Citrix Optimizer

## Windows VM Preparation

1.  Join VM to the domain

1.  Launch windows settings & check for updates

1.  Install applications required for the image template & any OS configuration

## Citrix Virtual Delivery Agent

> **Note**
>
> It is recommended to use the latest VDA version for optimal performance, security, and functionality

1.  Log in to Citrix.com/downloads and download the latest version of the VDA

1.  Run the VDA setup file "VDAWorkstationSetup_xxxx.exe" as an administrator.

1.  Select **Create a master MCS image**, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-007.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-007.png)

1.  If you would like the full workspace experience or to deliver virtual applications, select the **Citrix Workspace App**, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-008.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-008.png)

1.  If you require access to Citrix Files/Content & Collaboration select the **Citrix Files for Windows** (Outlook integration option is also available), click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-010.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-010.png)

    > **Note**
    >
    > For a full list of VDA install options see [Install VDA](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html)

1.  Enter the **Fully Qualified Domain Name (FQDN) of each Cloud Connector** (at least two are recommended for tolerance)

1.  Select **Test Connection** & if successful select **Add**

1.  Click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-012.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-012.png)

1.  Depending on your specific requirements, select the firewall configuration accordingly, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-014.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-014.png)

1.  Review the summary & click **Install**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-015.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-015.png)

1.  Confirm everything was installed successfully, **check the box to restart machine** & click **Finish**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-017.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-017.png)

## Citrix Optimizer

> **Note**
>
> For information on Citrix Optimizer see [Citrix Optimizer – What’s new](https://www.citrix.com/blogs/2020/04/09/citrix-optimizer-2-7-whats-new/)

1.  Log in & download Citrix Optimizer from [Citrix Optimizer](https://support.citrix.com/article/CTX224676)

1.  Run Citrix Optimizer as an administrator

1.  Select the appropriate Windows 10 Build

    [![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)

1.  Select **Analyze**

    [![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)

1.  Once the analyses are complete **View results**

1.  Select all the desired optimizations & select **Optimize**

1.  View the results & click **Done**

    [![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)

## Create Machine Catalog & Deploy VMs

> **Note**
>
> *Resources*
>
> We are using **Microsoft Azure** as a resource location in this scenario. For a full list of supported Hypervisors & Clouds see [Hosts / virtualization resources](/en-us/citrix-virtual-apps-desktops-service/system-requirements.html#hosts--virtualization-resources)
>  
> *Hosting Connections*
>
> The available resources are based on the hosting connections already added in Citrix Studio. See the following article for information on creating [CVAD hosting connections](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)

1.  Log into Citrix Cloud

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-051.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-051.png)

1.  Select the **Virtual Apps & Desktops** tile

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-052.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-052.png)

1.  Select **Manage**, then **Web Studio**

1.  From within Web Studio, select **Machine Catalogs**, then **Create Machine Catalog**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-018.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-018.png)

1.  Select **Multi-session OS** for Windows 10 WVD or Operating System as appropriate based on your deployment, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-019.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-019.png)

1.  Select **Machines that are power managed**

1.  Deploy machines using **Citrix Machine Creation Services**, select the appropriate Resource, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-020.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-020.png)

1.  Select **Master Image** previously created

1.  Select the minimum functional level & click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-021.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-021.png)

1.  Select **Storage & License Types** based on your environment

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-022.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-022.png)

1.  Enter the number of **Virtual Machines** you would like to create, along with the machine size, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-023.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-023.png)

1.  Configure **Write Back Cache** as appropriate, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-024.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-024.png)

1.  Select the appropriate **Resource Group** provisioning method, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-025.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-025.png)

1.  Select **Network Interface Card** to be associated with the VM, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-027.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-027.png)

1.  Select whether to create new AD accounts, where to create them & the naming convention to be used, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-028.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-028.png)

1.  Add the **Domain credentials** to be used to perform the account operations, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-029.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-029.png)

1.  Verify the **Summary**, add a **Machine Catalog name** and **Machine Catalog description**, then click **Finish**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-030.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-030.png)

> **Note**
>
> Wait for the machine catalog to create and the new VM disk. Time depends on the number of VMs specified

## Create Delivery Group

1.  Log into Citrix Cloud & select the **Virtual Apps & Desktops** tile

1.  Select **Manage**, then **Web Studio**

1.  From within Web Studio, select **Delivery Group**, then **Create Delivery Group**, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-031.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-031.png)

1.  Select the **Machine Catalog** you created (above)

1.  Choose the number of machines you created or want to add to the Delivery Group, click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-032.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-032.png)

1.  Select the **Users** assignment method (We are using Citrix Cloud for assignment in this scenario). Click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-033.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-033.png)

1.  If adding applications to be published separately add these here (We are only publishing a desktop in this scenario). Click **Next**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-034.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-034.png)

1.  Provide the **Delivery Group name** and **Display name**, click **Finish**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-035.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-035.png)

1.  Once created, view the machines & check their registration status

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-036.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-036.png)

## Assign Users to Desktop

1.  Log in to Citrix Cloud & from the landing page select **View Library**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-053.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-053.png)

1.  Locate the new Windows 10 Desktop resource in the Library, select the **ellipsis** (3 dot menu item in the top right corner), and select **Manage Subscribers**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-112.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-112.png)

1.  Add users or groups to assign desktop

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-045.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-045.png)

## Client Configuration

> **Note**
>  
> Best practice would be to automate the configuration of the **Citrix Workspace** & **Citrix Files** clients

Once the user has launched the virtual desktop & logged in they are able to carry out the following tasks

## Citrix Workspace App

1.  From the start menu launch **Citrix Workspace**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-057.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-057.png)

1.  Add the URL to your Citrix environment

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-107.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-107.png)

1.  Enter your **User name** & **Password**

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-113.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-113.png)

1.  Once authenticated you see any assigned resources in the workspace or populated in the start menu

## Citrix Files

> **Note**
>
> This assumes the option to install Citrix Files was selected when installing the VDA above
>
> If not the latest version can be downloaded from [Citrix Files for Windows](https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-windows.html)
  
1.  Check Citrix Files status from the icon in the system tray. If offline, right-click the icon & select login

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-049.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-049.png)

1.  Open Windows Explorer & check Citrix Files S: drive is available

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-060.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-060.png)

## Citrix Files For Outlook

> **Note**
>
> This assumes the option to install Citrix Files for Outlook was selected when installing the VDA above
>
> If not the latest version can be downloaded from [Citrix Files for Outlook](https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-outlook.html)

1.  Open Outlook & the Citrix files plug-in is visible along the Outlook tool bar. If you are logged on to the VM with a valid Citrix Files account, the authentication passes through and enables Citrix files functionality

    [![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)

1.  Select the icon to view the default settings

    [![Azure VM](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-109.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Win10-109.png)

> **Note**
>
> Best practice would be to have **Citrix Files** & **Citrix Files for Outlook** settings configured centrally
>
> For information on deploying & configuring **Citrix Content & collaboration** see the following article [Citrix Content & Collaboration](/en-us/citrix-content-collaboration.html)
