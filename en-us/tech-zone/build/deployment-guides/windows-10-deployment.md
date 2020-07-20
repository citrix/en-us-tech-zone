---
layout: doc
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices.
---
# Windows 10 Deployment Guide

## Contributors

**Author:** [Russell Peters](URL)

## Overview

This guide provides step by step guidance to install the Citrix Virtual Delivery Agent (VDA) on Windows 10. Configure & connect to your Citrix infrastructure.
The Citrix VDA is the client software that registers the resource with Citrix Virtual Apps & Desktops (CVAD) enabling the participation of resource deployment.
It will also cover the Citrix Optimiser, Citrix Workspace App & and other basic best practices

## Prerquisites

This guide assumes that the reader has a basic understanding of Citrix Virual Apps & Desktops & basic Windows administration.

1   A windows 10 VM or physical device

2   A Citrix.com account to download the software

*  Citrix VDA

  >**Note:**
    >
    > If using WVD in Microsoft Azure, be sure to use the multi user VDA (VDAServerSetup). For all other Windows 10 instalations, the single user version is required (VDAWorkstationSetup).

*  Citrix Optimiser
*  Citrix Workspace App
*  Citrix Files
*  Citrix Files for Outlook
*  Citrix HDX Monitor

## Windows 10 Prep

1.  From with the windows 10 VM ensure it is domain joined & has access to the network
2.  Launch windows settings & check for updates

[![Windows Settings update Screen](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_windows-update.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_windows-update.png)

## Citrix Virtual Delivery Agent

1.  Login to Citrix.com/downloads and download the latest version of the virtual delivery agent.

 >**Note:**
    >
    >At time of writing, Virtual Delivery Agent 2006 was the latest version. We recommend using the latest version available for optimal performance, security, and functionality.

[![Citrix VDA Download Screen](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-download.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-download.png)

2.  As an administrator run the instal file "VDAServerSetup_xxxx.exe"
3.  Depending on your install type slect the configuration option:
4.  Create a master MCS image
5.  Create a master imageusing Citrix Provisioning or third-party provisioning tools
6.  Enable Brokered Connection to a Server (option selected for this guide)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)

7. If you required to deliver virtal applications or the benifits of Citrix Workspace, select the Citrix Workspace App

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)

8. If you have access to Citrix Content & Colaboration select the Citrix Files for windows & for Outlook option.
  >**Note:**
   >
     > For a full list of VDA install options check here

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)

9. If you know your delivery controllers, select do it manually.

10. Enter the FQD name of each delivery controller (At lease two is recomended)
11. Select Test Connection & if successfull select Add

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)

12.  Depending on your Windows 10 configuration management, select either Automatically or Manually to configure the local Windows 10 firewall

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)

13. Review the summary & select install

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)

14. Confirm everything was installed successfully, check the box to restart machine & select Finish

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)

## Citrix Workspace App

1.  From the start menu launch Citrix Workspace
   
[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)

2.  Add the URL to your Citrix environment
3.  Select the check box to not show this at sign-in

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)

4.  Add your username & password
5.  Select the check box to remember your password

## Citrix Files

>**Note:**
 >This asumes the option to install Citrix Files was selected when installing the VDA above.
  >If not that latest version can be downloaded from https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-windows.html
  


1.  Check Citrix Files status from the icon in the taskbar. If offline, right click & select login.
   
[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-1.png)

2.  Open Windows Explorer & check Citrix Files S: drive is available

[![Citrix Files](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Files-3.png)

## Citrix Files For Outlook

>**Note:**
 >This asumes the option to install Citrix Files for Outlook was selected when installing the VDA above.
  >If not that latest version can be downloaded from https://www.citrix.com/downloads/sharefile/clients-and-plug-ins/citrix-files-for-outlook.html

1.  Open outlook & the Citrix files plug-in will be visable along the Outlook tool bar. If you are logged onto the WIndows 10 machine with an account with a valid Citrix Files account the authentication will pass through and enable the fles functionailty
   
[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-1.png)

2.  Select the icon to view the default settings. Best practice would be to have these settings deployed centraly from Citrix Content & Collaboration Service

[![Citrix Files for Outlook](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Outlook-2.png)

## Citrix Optimizer

>**Note:**
 >For information on Citrix optimizer see the latest Citrix article.
  >
 > Latest at time of writing: <https://www.citrix.com/blogs/2020/04/09/citrix-optimizer-2-7-whats-new/>

1.  Login & download Citrix Optimizer from Citrix Support Knowledge Center
<https://support.citrix.com/article/CTX224676>
2.  Run CitrixOptimizer as an administrator
3.  Select the latest Windows 10 Build from Citrix

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-1.png)

4. Run the alalysis option
   
[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-2.png)
1. Once analysesd, view the results.

2.  Select all the desired optimisations & Select Optimise

3.  View the results & select Done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)


## HDX Monitor

1.  From Citrix Insight Services - <https://cis.citrix.com/hdx/download/>

2.  Select Current Version, Online install (if for this instance only) and select Download
   
[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-1.png)

3.  Agree to the license terms
4.  Select Install

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-3.png)

5. Leave the default local hostname & select open

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-4.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-4.png)

6. Review the HDX Settings & Performance
7. Click on each item for more information

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-5.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_hdx-5.png)
