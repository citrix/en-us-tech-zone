---
layout: doc
description: Learn how to install the Citrix VDA into a WIndows 10 platform & prep it to be used in a CVAD environment, including related components, tips and best practices.
---
# Windows 10 Deployment Guide

## Contributors

**Author:** [Russell Peters](URL)

## Overview

This guide provides high level instructions to install the Citrix Virtual Delivery Agent (VDA) on Windows 10 & deploy Virtual Mchines (VMs) via Machine Creation Services (MCS) in Citrix Cloud Virtual Apps & Desktop (CVAD) Service.
*  Install Virtual Delivery Agent
*  Create a Machine Catalog & Deploy VMs
*  Create a Delivery Group
*  Assign Resources in Citrix Cloud Library

Also covers:
*  Create Azure VM to be used as a  WVD Tempate for Citrix MCS
*  Optimization Windows 10 for MCS Image
*  Citrix Workspace & Citrix Files configuration

 
>**Note:**
  >
  > The Citrix VDA is the client software that registers the resource with the CVAD service enabling the delivery of applications & desktops.

## Prerequisites

This guide assumes that the reader has a basic understanding of Citrix Cloud, CVAD Service, basic Windows administration & Microsoft Azure.

   1. A Microsoft Azure tennant & ability to create a VM, or an on-prem virtual environment with a windows 10 VM

2. A Citrix Cloud Admin Account

3. A Citrix.com account to download the software

      *  Citrix VDA
      *  Citrix Optimiser

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

4.  If you require to deliver virtal applications or would like to utlise the benifits of Citrix Workspace, select the Citrix Workspace App

   [![Azure VM](/en-us/tech-zone/build/media/Win10-008.png)](/en-us/tech-zone/build/media/Win10-008.png)

8.  If you require access to Citrix Files/Content & Colaboration select the Citrix Files for windows & for Outlook option.

  >**Note:**
    >
    > For a full list of VDA install options check here :
    <https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html>

   [![Azure VM](/en-us/tech-zone/build/media/Win10-010.png)](/en-us/tech-zone/build/media/Win10-010.png)


9.   Enter the FQD name of each Cloud Connector (At lease two are recomended)
10.  Select Test Connection & if successfull select Add the connector

   [![Azure VM](/en-us/tech-zone/build/media/Win10-012.png)](/en-us/tech-zone/build/media/Win10-012.png)


11.  Depending on your specific requirements, select the firewall configuration accordingly.

   [![Azure VM](/en-us/tech-zone/build/media/Win10-014.png)](/en-us/tech-zone/build/media/Win10-014.png)

12.  Review the summary & select install

   [![Azure VM](/en-us/tech-zone/build/media/Win10-015.png)](/en-us/tech-zone/build/media/Win10-015.png)

13.  Confirm everything was installed successfully, check the box to restart machine & select Finish

   [![Azure VM](/en-us/tech-zone/build/media/Win10-017.png)](/en-us/tech-zone/build/media/Win10-017.png)


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

5.  Once analysesd, view the results.

6.  Select all the desired optimisations & Select Optimise

7.  View the results & select Done

[![Citrix Optimizer](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_optimizer-3.png)



## Create Machine Catalog & Delploy VMs

1. Log into Citrix Cloud & select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

2. Select manage, then Web Studio
3. From within Web Studio, select Machine Catalogs, then create Machine Catalog
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-018.png)](/en-us/tech-zone/build/media/Win10-018.png)

4. Select Multi-session OS for WVD, for Windows 10 select Single-session OS

[![Azure VM](/en-us/tech-zone/build/media/Win10-019.png)](/en-us/tech-zone/build/media/Win10-019.png)

5. Select Machines that are power managed.
6. Deploy machines using Machine Creation Services & select Resource location

[![Azure VM](/en-us/tech-zone/build/media/Win10-020.png)](/en-us/tech-zone/build/media/Win10-020.png)
7.  Select master image created in Azure (See Create Machine in Azure below)
8.  Select the minimum functionsal level. This should be the latestfesion that you have in your environment)
   
[![Azure VM](/en-us/tech-zone/build/media/Win10-021.png)](/en-us/tech-zone/build/media/Win10-021.png)

9. Select Storage & License Types based on your environment

[![Azure VM](/en-us/tech-zone/build/media/Win10-022.png)](/en-us/tech-zone/build/media/Win10-022.png)

10.   Enter the nmuber of mchines you would like to provision.
11.   Select machine size

[![Azure VM](/en-us/tech-zone/build/media/Win10-023.png)](/en-us/tech-zone/build/media/Win10-023.png)

12. Configure Write Back Cache as appropriate

[![Azure VM](/en-us/tech-zone/build/media/Win10-024.png)](/en-us/tech-zone/build/media/Win10-024.png)

13. Select Resource Group Provisioning

[![Azure VM](/en-us/tech-zone/build/media/Win10-025.png)](/en-us/tech-zone/build/media/Win10-025.png)

14.   Select Network Interface to be assossiated with the VM

[![Azure VM](/en-us/tech-zone/build/media/Win10-027.png)](/en-us/tech-zone/build/media/Win10-027.png)

15.   Select whether to create new AD accounts, where to create them & the naming convention to be used.

[![Azure VM](/en-us/tech-zone/build/media/Win10-028.png)](/en-us/tech-zone/build/media/Win10-028.png)

16. Add domain credential to be used to oreform the account operation

[![Azure VM](/en-us/tech-zone/build/media/Win10-029.png)](/en-us/tech-zone/build/media/Win10-029.png)

17.   Machine Catalog Name in acordance with your corporate naming convention

[![Azure VM](/en-us/tech-zone/build/media/Win10-030.png)](/en-us/tech-zone/build/media/Win10-030.png)

1.    Click Finish & wait for the machine catalog to ctreate and the new VM disk to be created. Time will obviously depend on the amount of VMs specified.
         
## Create Delivery Group

1. Log into Citrix Cloud & select the Virtual Apps & Desktop tile

[![Azure VM](/en-us/tech-zone/build/media/Win10-051.png)](/en-us/tech-zone/build/media/Win10-051.png)

2. Select manage, then Web Studio
3. From within Web Studio, select Delivery Group, then create Delivery Group

[![Azure VM](/en-us/tech-zone/build/media/Win10-031.png)](/en-us/tech-zone/build/media/Win10-031.png)

4. Select the Machine Catalog you just cretaed (above)
5. Choose the number of machiens you created or want to add

[![Azure VM](/en-us/tech-zone/build/media/Win10-032.png)](/en-us/tech-zone/build/media/Win10-032.png)

6.  Select you user assignment method. Using Citroc Cloud for assignment in this scenario.

[![Azure VM](/en-us/tech-zone/build/media/Win10-033.png)](/en-us/tech-zone/build/media/Win10-033.png)

7. If adding applications to be published seperatly add these here. We are just publishing a WVD desktop in this scenario.

[![Azure VM](/en-us/tech-zone/build/media/Win10-034.png)](/en-us/tech-zone/build/media/Win10-034.png)

8.  Give the Delivery Group a name & the display name the users will see in their Workspace
9.  Finish

[![Azure VM](/en-us/tech-zone/build/media/Win10-035.png)](/en-us/tech-zone/build/media/Win10-035.png)

10.   Once created, view the machines & check their registration status.

[![Azure VM](/en-us/tech-zone/build/media/Win10-036.png)](/en-us/tech-zone/build/media/Win10-036.png)


## Assign Users to Desktop

1. Login to Citrix Cloud & select appropriate customer where resource reside

[![Azure VM](/en-us/tech-zone/build/media/Win10--37.png)](/en-us/tech-zone/build/media/Win10--37.png)

2. From the landing page select View Library
 
[![Azure VM](/en-us/tech-zone/build/media/Win10--36.png)](/en-us/tech-zone/build/media/Win10--36.png)

3. Find the resource in the Library, hit the 3 dot menu in the top right corner & select Manage Subscribers.

[![Azure VM](/en-us/tech-zone/build/media/Win10-044.png)](/en-us/tech-zone/build/media/Win10-044.png)

4. Add users or groups to assign desktop

[![Azure VM](/en-us/tech-zone/build/media/Win10-045.png)](/en-us/tech-zone/build/media/Win10-045.png)


## Client Configuration

  >**Note:**
    > For domain joined VMs, best practice would be to populate Citrix application configuration via GPO

## Citrix Workspace App

1.  From the start menu launch Citrix Workspace

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-1.png)

2.  Add the URL to your Citrix environment
3.  Select the check box to not show this at sign-in

[![Citrix Workspace](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)](/en-us/tech-zone/build/media/deployment-guides_windows-10-deployment_Workspace-2.png)

4.  Add your username & password
5.  Once logged into your Citrix Workspace you will see the assigned resource under Desktops

[![Azure VM](/en-us/tech-zone/build/media/Win10-047.png)](/en-us/tech-zone/build/media/Win10-047.png)


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

## Create Virtual Machine in Microsoft Azure

1.  From within the Azure Portal, select the appropriate Subscription, Virtual Machines Resource & then select Add Virtual Machine

   [![Azure VM](/en-us/tech-zone/build/media/Win10-101.png)](/en-us/tech-zone/build/media/Win10-101.png)

2.  Add Basic Details
*  Subscription (Default)
*  Resource Group – In accordance with corporate naming convention
*  VM Name - In accordance with corporate naming convention
*  Region – Defaults to your current Region, change as appropriate
*  Availability Options (None as creating a template VM)
*  Image – Select image as per requirement.
   *  For the purpose of this guide:	Windows 10 Enterprise Multi-session, Version 20H2 + Microsoft 365 Apps
*  Select Size - Select size as per requirement.
   *  For the purpose of this guide:	Standard_D4s_v3 – 4 vcpus, 16 GiB memory
*  Enter Administrator Account Details
*  Enter Public Inbound Ports Details
*  Select Next: Disk
3.  Add Disk Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-102.png)](/en-us/tech-zone/build/media/Win10-102.png)

4.  Add Network Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-103.png)](/en-us/tech-zone/build/media/Win10-103.png)

5.  Enter Management Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-104.png)](/en-us/tech-zone/build/media/Win10-104.png)

6.  Enter Advanced Details

   [![Azure VM](/en-us/tech-zone/build/media/Win10-105.png)](/en-us/tech-zone/build/media/Win10-105.png)

7.  Next: Review & Create
