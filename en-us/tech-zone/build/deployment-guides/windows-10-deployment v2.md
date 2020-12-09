---
layout: doc
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices.
---
# Windows 10 Deployment Guide

## Contributors

**Author:** [Russell Peters](URL)

Test

## Overview

This guide provides high level, guide to install the Citrix Virtual Delivery Agent (VDA) on Windows 10 & deploy Virtual Mchines (VMs) via Machine Creation Services (MCS) in Citrix Cloud Virtual Appps & Desktop (CVAD) Service.
*  Create a Machine Catalog
*  Create a Delivery Group
*  Assign Resources in Citrix Cloud Library

Also covers:
*  Create Azure VM to build WVD Tempate for Citrix MCS
*  Optimization Windows 10 for MCS Image

>**Note:**
  >
  > The Citrix VDA is the client software that registers the resource with the CVAD service enabling the delivery of applications & desktops.
It will also cover the Citrix Optimiser, Citrix Workspace, Citrix Files & and other basic best practices

## Prerequisites

This guide assumes that the reader has a basic understanding of Citrix Cloud, CVAD Service, basic Windows administration & Microsoft Azure.

1   A Microsoft Azure tennant & ability to create a VM, or an on-prem virtual environment with a windows 10 VM

2   A Citrix Cloud Admin Account

3   A Citrix.com account to download the software

*  Citrix VDA
*  Citrix Optimiser

## Create Virtual Machine in Microsoft Azure
 >**Note:**
    >
    >If not using Azure go to next section, Windows VM Preperation

1.  From within the Azure Portal, select Virtual Machines & Add Virtual Machine

   [![Azure VM](/en-us/tech-zone/build/media/Win10-001.png)](/en-us/tech-zone/build/media/Win10-001.png)

2.  Add Basic Details
*  Subscription (Default)
*  Resource Group – In accordance with corporate naming convention
*  VM Name - In accordance with corporate naming convention
*  Region – Defaults to your current location, change as appropriate
*  Availability Options (None as creating a template VM)
*  Image – Select image as per requirement.
   *  For the purpose of this guide:	Windows 10 Enterprise Multi-session, Version 20H2 + Microsoft 365 Apps
*  Select Size - Select size as per requirement.
   *  For the purpose of this guide:	Standard_D4s_v3 – 4 vcpus, 16 GiB memory
*  Enter Administrator Account Details
*  Enter Public Inbound Ports Details
*  Select Next: Disk
3.  Add Disk Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-002.png)](/en-us/tech-zone/build/media/Win10-002.png)

4.  Add Network Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-003.png)](/en-us/tech-zone/build/media/Win10-003.png)

5.  Enter Management Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-004.png)](/en-us/tech-zone/build/media/Win10-004.png)

6.  Enter Advanced Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-005.png)](/en-us/tech-zone/build/media/Win10-005.png)

7.  Next: Review & Create



## Windows VM Preperation

1.  Join VM to the domain
2.  Launch windows settings & check for updates
3.  Install Basic applications required for the image template & any OS configuration.

## Citrix Virtual Delivery Agent

1.  Login to Citrix.com/downloads and download the latest version of the virtual delivery agent.

 >**Note:**
    >
    >At time of writing, Virtual Delivery Agent 2009 was the latest version. We recommend using the latest version available for optimal performance, security, and functionality.

2.  As an administrator run the instal file "VDAWorkstationSetup_xxxx.exe"
3.  Select Create a master MCS image

   [![Azure VM](/en-us/tech-zone/build/media/Win10-007.png)](/en-us/tech-zone/build/media/Win10-007.png)



[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)

7.  If you require to deliver virtal applications or would like to utlise the benifits of Citrix Workspace, select the Citrix Workspace App

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)

8.  If you require access to Citrix Files/Content & Colaboration select the Citrix Files for windows & for Outlook option.

  >**Note:**
    >
    > For a full list of VDA install options check here :
    <https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html>

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)

9.  If you know your Cloud controllers, select do it manually.

10.  Enter the FQD name of each Cloud controller (At lease two are recomended)
11.  Select Test Connection & if successfull select Add the controller

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)

12.  Depending on your Windows 10 configuration management, select either Automatically or Manually to configure the local Windows 10 firewall

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)

13.  Review the summary & select install

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)

14.  Confirm everything was installed successfully, check the box to restart machine & select Finish

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)

## Citrix Workspace App

1.  From the start menu launch Citrix Workspace

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)

2.  Add the URL to your Citrix environment
3.  Select the check box to not show this at sign-in

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)

4.  Add your username & password
5.  Only select the check box to remember your password if this is one off Windows 10 build. For any image creation leave this unselected.

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

1.  Open outlook & the Citrix files plug-in will be visable along the Outlook tool bar. If you are logged onto the WIndows 10 machine with an account with a valid Citrix Files account the authentication will pass through and enable the fles functionailty

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)

2.  Select the icon to view the default settings. Best practice would be to have these settings deployed centraly from Citrix Content & Collaboration Service

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)

## Citrix Optimizer

>**Note:**
 >
  >For information on Citrix optimizer see the latest Citrix article.
  >
  > Latest at time of writing: <https://www.citrix.com/blogs/2020/04/09/citrix-optimizer-2-7-whats-new/>

1.  Login & download Citrix Optimizer from Citrix Support Knowledge Center
<https://support.citrix.com/article/CTX224676>
2.  Run CitrixOptimizer as an administrator
3.  Select the latest Windows 10 Build from Citrix

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)

4.  Run the alalysis option

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)

1.  Once analysesd, view the results.

2.  Select all the desired optimisations & Select Optimise

3.  View the results & select Done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)

## HDX Monitor - Optional

1.  From Citrix Insight Services - <https://cis.citrix.com/hdx/download/>

2.  Select Current Version, Online install (if for this instance only) and select Download

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-1.png)

3.  Agree to the license terms
4.  Select Install

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-3.png)

5.  Leave the default local hostname & select open

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-4.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-4.png)

6.  Review the HDX Settings & Performance
7.  Click on each item for more information

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-5.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-5.png)
