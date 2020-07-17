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

*  Citrix Workspace App
*  Citrix Optimiser
*  Citrix Health Assistant
*  Citrix Connection Quality Indicator
*  Citrix HDX Monitor

## Windows 10 Prep

1.  From with the windows 10 VM ensure it is domain joined & has access to the network
2.  Launch windows settings & check for updates

[![Windows Settings update Screen](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_windows-update.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_windows-update.png)

## Citrix Virtual Delivery Agent

1.  Log into Citrix.com/downloads and download the latest version of the virtual delivery agent.

 >**Note:**
    >
    >At time of writing, Virtual Delivery Agent 2006 was the latest version. We recommend using the latest version available for optimal performance, security, and functionality.

[![Citrix VDA Download Screen](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-download.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-download.png)

2.  As an administrator run the instal file "VDAServerSetup_xxxx.exe"
3.  Depending on your install type slect the configuration option:
1.  Create a master MCS image
2.  Create a master imageusing Citrix Provisioning or third-party provisioning tools
3.  Enable Brokered Connection to a Server (option selected for this guide)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-1.png)
4. If you required to deliver virtal applications or the benifits of rCitrix Workspace, select the Citrix Workspace

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-2.png)
5. If you have access to Citrix Content & Colaboration select the Citrix Files for windows option.
  >**Note:**
   >
     > For a full list of VDA install options check here

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-3.png)
6. If you know your delivery controllers, select do it manually.
7. Enter the FQD name of each delivery controller (At lease two is recomended)
8. Select Test Connection & if successfull select Add

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-4.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-5.png)
9.  Depending on your Windows 10 configuration management, select either Automatically or Manually to configure the local Windows 10 firewall

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-6.png)
10. Review the summary & select install

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-7.png)

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-8.png)
11. Confirm everything was installed successfully, check the box to restart machine & select Finish

[![Citrix VDA install](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_vda-install-9.png)

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
5. Once analysesd, view the results.

6.  Select all the desired optimisations & Select Optimise

7.  View the results & select Done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)

## Citrix Health Assistant

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
