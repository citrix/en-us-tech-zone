---
layout: doc
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices.
---
# Windows 10 Deployment Guide

## Contributors

**Author:** [Russell Peters](URL)

## Overview

This guide provides high level instructions to install the Citrix Virtual Delivery Agent (VDA) on Windows 10 & deploy Virtual Mchines (VMs) via Machine Creation Services (MCS) in Citrix Cloud, Virtual Apps & Desktop (CVAD) Service.
*  Install Virtual Delivery Agent
*  Create a Machine Catalog & Deploy VMs
*  Create a Delivery Group
*  Assign Resources in Citrix Cloud Library

Also covers:
*  Create Azure VM to be used as a WVD Template for Citrix MCS
*  Optimization Windows 10 for MCS Image
*  Citrix Workspace & Citrix Files configuration

## Prerequisites

 >**Note:**
   >
    > This guide assumes that the reader has a basic understanding of the following technologies: Citrix Cloud, CVAD Service, basic Windows administration & Microsoft Azure.

   1. A Microsoft Azure tennant & ability to create a VM, or an on-prem virtual environment with a windows 10 VM

2. A Citrix Cloud Admin Account

3. A Citrix.com account to download the software

      *  Citrix VDA
      *  Citrix Optimiser

## Windows VM Preparation

1.  Join VM to the domain
2.  Launch windows settings & check for updates
3.  Install applications required for the image template & any OS configuration.

## Citrix Virtual Delivery Agent

1.  Login to Citrix.com/downloads and download the latest version of the virtual delivery agent.

 >**Note:**
   >
    > At time of writing, Virtual Delivery Agent 2009 was the latest version. We recommend using the latest version available for optimal performance, security, and functionality.

2.  Run the VDA setup file "VDAWorkstationSetup_xxxx.exe" as an administrator.
3.  Select Create a master MCS image

   [![Azure VM](/en-us/tech-zone/build/media/Win10-007.png)](/en-us/tech-zone/build/media/Win10-007.png)

4.  If you would like the full workspace experience or deliver virtal applications, select the Citrix Workspace App.

   [![Azure VM](/en-us/tech-zone/build/media/Win10-008.png)](/en-us/tech-zone/build/media/Win10-008.png)

5.  If you require access to Citrix Files/Content & Colaboration select the Citrix Files for Windows (optional Outlook integration option is additionally available).

  >**Note:**
    >
      >For a full list of VDA install options check here:
      <https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html>

   [![Azure VM](/en-us/tech-zone/build/media/Win10-010.png)](/en-us/tech-zone/build/media/Win10-010.png)


6.   Enter the Fully Qualified Domain Name (FQDN) of each Cloud Connector (at least two are recommended for tolerance)
7.  Select Test Connection & if successfull select Add

   [![Azure VM](/en-us/tech-zone/build/media/Win10-012.png)](/en-us/tech-zone/build/media/Win10-012.png)


8.  Depending on your specific requirements, select the firewall configuration accordingly.

   [![Azure VM](/en-us/tech-zone/build/media/Win10-014.png)](/en-us/tech-zone/build/media/Win10-014.png)

9.  Review the summary & select Install

   [![Azure VM](/en-us/tech-zone/build/media/Win10-015.png)](/en-us/tech-zone/build/media/Win10-015.png)

10.  Confirm everything was installed successfully, check the box to restart machine & select finish.

   [![Azure VM](/en-us/tech-zone/build/media/Win10-017.png)](/en-us/tech-zone/build/media/Win10-017.png)


## Citrix Optimizer

>**Note:**
 >
  >For information on Citrix optimizer see the below Citrix article.
  >
  > Latest at time of writing: <https://www.citrix.com/blogs/2020/04/09/citrix-optimizer-2-7-whats-new/>

1.  Login & download Citrix Optimizer from Citrix Support Knowledge Center
<https://support.citrix.com/article/CTX224676>
2.  Run Citrix Optimizer as an administrator
3.  Select the appropriate Windows 10 Build

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)

4.  Select the Analyse option

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)

5.  Once the analyses is complete, view the results.

6.  Select all the desired optimisations & select optimise

7.  View the results & select done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)


## Create Machine Catalog & Deploy VMs

1. Log into Citrix Cloud & select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

2. Select manage, then Web Studio
3. From within Web Studio, select Machine Catalogs, then create Machine Catalog
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-018.png)](/en-us/tech-zone/build/media/Win10-018.png)

4. Select Multi-session OS for Windows 10 WVD or Single-session OS for a non-WVD Windows 10 OS

[![Azure VM](/en-us/tech-zone/build/media/Win10-019.png)](/en-us/tech-zone/build/media/Win10-019.png)

5. Select Machines that are power managed.
6. Deploy machines using Machine Creation Services & select the appropriate Resource

>  Note: The available resources are based on the hosting connections previously set in studio.
Creating CVAD Hosting connections: https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html


[![Azure VM](/en-us/tech-zone/build/media/Win10-020.png)](/en-us/tech-zone/build/media/Win10-020.png)

7.  Select master image created in Azure (See Create Machine in Azure below)
8.  Select the minimum functional level (This should be the latest version you have in your environment).
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-021.png)](/en-us/tech-zone/build/media/Win10-021.png)

9. Select Storage & License Types based on your environment

[![Azure VM](/en-us/tech-zone/build/media/Win10-022.png)](/en-us/tech-zone/build/media/Win10-022.png)

10.   Enter the number of machines you would like to provision, along with the machine size.

[![Azure VM](/en-us/tech-zone/build/media/Win10-023.png)](/en-us/tech-zone/build/media/Win10-023.png)

11. Configure Write Back Cache as appropriate

[![Azure VM](/en-us/tech-zone/build/media/Win10-024.png)](/en-us/tech-zone/build/media/Win10-024.png)

12. Select Resource Group Provisioning

[![Azure VM](/en-us/tech-zone/build/media/Win10-025.png)](/en-us/tech-zone/build/media/Win10-025.png)

13.   Select Network Interface to be associated with the VM

[![Azure VM](/en-us/tech-zone/build/media/Win10-027.png)](/en-us/tech-zone/build/media/Win10-027.png)

14.   Select whether to create new AD accounts, where to create them & the naming convention to be used.

[![Azure VM](/en-us/tech-zone/build/media/Win10-028.png)](/en-us/tech-zone/build/media/Win10-028.png)

15. Add the domain credentials to be used to perform the account operations.

[![Azure VM](/en-us/tech-zone/build/media/Win10-029.png)](/en-us/tech-zone/build/media/Win10-029.png)

16.   Verify the summary, add a Machine Catalog name and description,  and click Finish (wait for the machine catalog to create and the new VM disk. Time will depend on the amount of VMs specified)

[![Azure VM](/en-us/tech-zone/build/media/Win10-030.png)](/en-us/tech-zone/build/media/Win10-030.png)

17.    Click Finish & wait for the machine catalog to ctreate and the new VM disk to be created. Time will obviously depend on the amount of VMs specified.
         
## Create Delivery Group

1. Log into Citrix Cloud & select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

2. Select manage, then Web Studio
3. From within Web Studio, select Delivery Group, then create Delivery Group

[![Azure VM](/en-us/tech-zone/build/media/Win10-031.png)](/en-us/tech-zone/build/media/Win10-031.png)

4. Select the Machine Catalog you just created (above)
5. Choose the number of machines you created or want to add

[![Azure VM](/en-us/tech-zone/build/media/Win10-032.png)](/en-us/tech-zone/build/media/Win10-032.png)

6.  Select you user assignment method. Using Citrix Cloud for assignment in this scenario.

[![Azure VM](/en-us/tech-zone/build/media/Win10-033.png)](/en-us/tech-zone/build/media/Win10-033.png)

7. If adding applications to be published seperatly add these here. We are just publishing a desktop in this scenario.

[![Azure VM](/en-us/tech-zone/build/media/Win10-034.png)](/en-us/tech-zone/build/media/Win10-034.png)

8.  Provide the Delivery Group name and Display name, click Finish.

[![Azure VM](/en-us/tech-zone/build/media/Win10-035.png)](/en-us/tech-zone/build/media/Win10-035.png)

9.   Once created, view the machines & check their registration status.

[![Azure VM](/en-us/tech-zone/build/media/Win10-036.png)](/en-us/tech-zone/build/media/Win10-036.png)


## Assign Users to Desktop

1. Login to Citrix Cloud & select appropriate customer where resource reside

[![Azure VM](/en-us/tech-zone/build/media/Win10--37.png)](/en-us/tech-zone/build/media/Win10--37.png)

2. From the landing page select View Library
 
[![Azure VM](/en-us/tech-zone/build/media/Win10--36.png)](/en-us/tech-zone/build/media/Win10--36.png)

3. Locate the new Windows 10 Desktop resource in the Library, select the ellipsis (3 dot menu item in the top right corner), and select Manage Subscribers.

[![Azure VM](/en-us/tech-zone/build/media/Win10-044.png)](/en-us/tech-zone/build/media/Win10-044.png)

4. Add users or groups to assign desktop

[![Azure VM](/en-us/tech-zone/build/media/Win10-045.png)](/en-us/tech-zone/build/media/Win10-045.png)


## Client Configuration

  >**Note:**
    > Best practice would be to automate the configuration of the Citrix Workspace & Files applications.

## Citrix Workspace App

1.  From the start menu launch Citrix Workspace

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)

2.  Add the URL to your Citrix environment
3.  Select the check box to not show this at sign-in

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)

4.  Add your username & password
5.  Once autheniticated you will see any assigned resources available in the workspace or from your start menu

## Citrix Files

>**Note:**
 >
  >This asumes the option to install Citrix Files was selected when installing the VDA above.
  >If not that latest version can be downloaded from <https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-windows.html>
  
1.  Check Citrix Files status from the icon in the taskbar. If offline, right click & select login.

[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)

2.  Open Windows Explorer & check Citrix Files S: drive is available

[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)

## Citrix Files For Outlook

>**Note:**
 >
  >This asumes the option to install Citrix Files for Outlook was selected when installing the VDA above.
  >If not that latest version can be downloaded from <https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-outlook.html>

1.  Open outlook & the Citrix files plug-in will be visable along the Outlook tool bar. If you are logged onto the WIndows 10 VM with a valid Citrix Files account, the authentication will pass through and enable the fles functionailty.

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)

2.  Select the icon to view the default settings. Best practice would be to have these settings deployed centraly from Citrix Content & Collaboration Service

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)
