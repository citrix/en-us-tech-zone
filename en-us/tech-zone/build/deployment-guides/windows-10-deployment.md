---
layout: doc
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices
---
# Windows 10 Deployment Guide

## Contributors

**Author:** [Russell Peters](URL)

## Overview

This guide provides high level instructions to install the Citrix Virtual Delivery Agent (VDA) on Windows 10 & deploy Virtual Machines (VMs) via Machine Creation Services (MCS) in Citrix Virtual Apps & Desktops (CVAD) Service

*  Install VDA
*  Optimize Windows 10 for MCS
*  Create a Machine Catalog & deploy VMs
*  Create a Delivery Group & assign VMs
*  Configure Citrix Workspace & Citrix Files clients

## Prerequisites

 >**Note:**
   >
 > This guide assumes that the reader has a basic understanding of the following technologies:
 > Citrix Cloud, CVAD Service & basic Windows administration

1.  A Windows 10 VM in Microsoft Azure, Google Cloud Platform (GCP) or an on premises virtual environment

1.  A Citrix Cloud admin account

1.  A domain admin account with the ability to create machine accounts in the desired OU

1.  A Citrix.com account to download software

      *  Citrix VDA
      *  Citrix Optimizer

## Windows VM Preparation

1.  Join VM to the domain

1.  Launch windows settings & check for updates

1.  Install applications required for the image template & any OS configuration

## Citrix Virtual Delivery Agent

   >**Note:**
   >
   > At time of writing, VDA 2009 was the latest version. It is recommended to use the latest version available for optimal performance, security, and functionality

1.  Login to Citrix.com/downloads and download the latest version of the VDA

1.  Run the VDA setup file "VDAWorkstationSetup_xxxx.exe" as an administrator.

1.  Select **Create a master MCS image**

    [![Azure VM](/en-us/tech-zone/build/media/Win10-007.png)](/en-us/tech-zone/build/media/Win10-007.png)

1.  If you would like the full workspace experience or to deliver virtual applications, select the **Citrix Workspace App**

    [![Azure VM](/en-us/tech-zone/build/media/Win10-008.png)](/en-us/tech-zone/build/media/Win10-008.png)

1.  If you require access to Citrix Files/Content & Collaboration select the **Citrix Files for Windows** (Outlook integration option is also available)

    [![Azure VM](/en-us/tech-zone/build/media/Win10-010.png)](/en-us/tech-zone/build/media/Win10-010.png)

    >**Note:**
    >
    > For a full list of VDA install options check here: <https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html>

1.  Enter the **Fully Qualified Domain Name (FQDN) of each Cloud Connector** (at least two are recommended for tolerance)

1.  Select **Test Connection** & if successful select **Add**

    [![Azure VM](/en-us/tech-zone/build/media/Win10-012.png)](/en-us/tech-zone/build/media/Win10-012.png)

1.  Depending on your specific requirements, select the firewall configuration accordingly

    [![Azure VM](/en-us/tech-zone/build/media/Win10-014.png)](/en-us/tech-zone/build/media/Win10-014.png)

1.  Review the summary & select **Install**

    [![Azure VM](/en-us/tech-zone/build/media/Win10-015.png)](/en-us/tech-zone/build/media/Win10-015.png)

1.  Confirm everything was installed successfully, **check the box to restart machine** & select **Finish**

    [![Azure VM](/en-us/tech-zone/build/media/Win10-017.png)](/en-us/tech-zone/build/media/Win10-017.png)

## Citrix Optimizer

>**Note:**
 >
> For information on Citrix optimizer see the following Citrix article
(Latest at time of writing): <https://www.citrix.com/blogs/2020/04/09/citrix-optimizer-2-7-whats-new/>

1.  Login & download Citrix Optimizer from Citrix Support Knowledge Center
<https://support.citrix.com/article/CTX224676>
2.  Run Citrix Optimizer as an administrator
3.  Select the appropriate Windows 10 Build

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)

4.  Select the Analyze option

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)

5.  Once the analyses are complete, view the results

6.  Select all the desired optimizations & click optimize

7.  View the results & select done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)


## Create Machine Catalog & Deploy VMs

1. Log into Citrix Cloud

[![Azure VM](/en-us/tech-zone/build/media/Win10--37.png)](/en-us/tech-zone/build/media/Win10--37.png)

2. Select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

3. Select manage, then Web Studio
4. From within Web Studio, select Machine Catalogs, then create Machine Catalog
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-018.png)](/en-us/tech-zone/build/media/Win10-018.png)

5. Select Multi-session OS for Windows 10 WVD or Single-session OS for a non-WVD Windows 10 OS

[![Azure VM](/en-us/tech-zone/build/media/Win10-019.png)](/en-us/tech-zone/build/media/Win10-019.png)

6. Select Machines that are power managed
7. Deploy machines using Machine Creation Services & select the appropriate Resource

>**Note:**
 >
>  The available resources are based on the hosting connections previously set in studio.
>  See the following article for information on creating CVAD hosting connections: https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html


[![Azure VM](/en-us/tech-zone/build/media/Win10-020.png)](/en-us/tech-zone/build/media/Win10-020.png)

7.  Select master image previously created
8.  Select the minimum functional level (This should be the latest VDA version you wish to include in this Machine Catalog)
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-021.png)](/en-us/tech-zone/build/media/Win10-021.png)

9. Select Storage & License Types based on your environment

[![Azure VM](/en-us/tech-zone/build/media/Win10-022.png)](/en-us/tech-zone/build/media/Win10-022.png)

10.   Enter the number of machines you would like to provision, along with the machine size

[![Azure VM](/en-us/tech-zone/build/media/Win10-023.png)](/en-us/tech-zone/build/media/Win10-023.png)

11. Configure Write Back Cache as appropriate

[![Azure VM](/en-us/tech-zone/build/media/Win10-024.png)](/en-us/tech-zone/build/media/Win10-024.png)

12. Select the appropriate Resource Group provisioning method

[![Azure VM](/en-us/tech-zone/build/media/Win10-025.png)](/en-us/tech-zone/build/media/Win10-025.png)

13.   Select Network Interface to be associated with the VM

[![Azure VM](/en-us/tech-zone/build/media/Win10-027.png)](/en-us/tech-zone/build/media/Win10-027.png)

14.   Select whether to create new AD accounts, where to create them & the naming convention to be used

[![Azure VM](/en-us/tech-zone/build/media/Win10-028.png)](/en-us/tech-zone/build/media/Win10-028.png)

15. Add the domain credentials to be used to perform the account operations

[![Azure VM](/en-us/tech-zone/build/media/Win10-029.png)](/en-us/tech-zone/build/media/Win10-029.png)

16.   Verify the summary, add a Machine Catalog name and description, then click Finish (wait for the machine catalog to create and the new VM disk. Time will depend on the amount of VMs specified)

[![Azure VM](/en-us/tech-zone/build/media/Win10-030.png)](/en-us/tech-zone/build/media/Win10-030.png)
    
## Create Delivery Group

1. Log into Citrix Cloud & select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

2. Select manage, then Web Studio
3. From within Web Studio, select Delivery Group, then Create Delivery Group

[![Azure VM](/en-us/tech-zone/build/media/Win10-031.png)](/en-us/tech-zone/build/media/Win10-031.png)

4. Select the Machine Catalog you just created (above)
5. Choose the number of machines you created or want to add to the Delivery Group

[![Azure VM](/en-us/tech-zone/build/media/Win10-032.png)](/en-us/tech-zone/build/media/Win10-032.png)

6.  Select the user assignment method. We are using Citrix Cloud for assignment in this scenario

[![Azure VM](/en-us/tech-zone/build/media/Win10-033.png)](/en-us/tech-zone/build/media/Win10-033.png)

7. If adding applications to be published seperatly add these here. We are only publishing a desktop in this scenario

[![Azure VM](/en-us/tech-zone/build/media/Win10-034.png)](/en-us/tech-zone/build/media/Win10-034.png)

8.  Provide the Delivery Group name and Display name, click Finish

[![Azure VM](/en-us/tech-zone/build/media/Win10-035.png)](/en-us/tech-zone/build/media/Win10-035.png)

9.   Once created, view the machines & check their registration status

[![Azure VM](/en-us/tech-zone/build/media/Win10-036.png)](/en-us/tech-zone/build/media/Win10-036.png)


## Assign Users to Desktop

1. Login to Citrix Cloud & from the landing page select View Library

[![Azure VM](/en-us/tech-zone/build/media/Win10--36.png)](/en-us/tech-zone/build/media/Win10--36.png)

2. Locate the new Windows 10 Desktop resource in the Library, select the ellipsis (3 dot menu item in the top right corner), and select Manage Subscribers

[![Azure VM](/en-us/tech-zone/build/media/Win10-044.png)](/en-us/tech-zone/build/media/Win10-044.png)

3. Add users or groups to assign desktop

[![Azure VM](/en-us/tech-zone/build/media/Win10-045.png)](/en-us/tech-zone/build/media/Win10-045.png)


## Client Configuration

>**Note:**
   >  
>  Best practice would be to automate the configuration of the Citrix Workspace & Files applications

## Citrix Workspace App

1.  From the start menu launch Citrix Workspace
2.  Add the URL to your Citrix environment

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)

3.  Enter your username & password

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)

4.  Once autheniticated you will see any assigned resources in the workspace or populated in the start menu

## Citrix Files

>**Note:**
 >
>  This assumes the option to install Citrix Files was selected when installing the VDA above.
>  If not that latest version can be downloaded from <https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-windows.html>
  
1.  Check Citrix Files status from the icon in the system tray. If offline, right click & select login

[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)

2.  Open Windows Explorer & check Citrix Files S: drive is available

[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)

## Citrix Files For Outlook

>**Note:**
 >
>  This assumes the option to install Citrix Files for Outlook was selected when installing the VDA above.
>  If not that latest version can be downloaded from <https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-outlook.html>

1.  Open Outlook & the Citrix files plug-in will be visable along the Outlook tool bar. If you are logged onto the VM with a valid Citrix Files account, the authentication will pass through and enable the files functionailty

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)

2.  Select the icon to view the default settings. Best practice would be to have these settings deployed centraly from Citrix Content & Collaboration Service

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)
