---
layout: doc
h3InToc: true
contributedBy: Allen Furmanski
description: Learn how to deploy the Citrix Files Client for Windows in a virtual app and desktop environment. This article includes related components, tips, and leading practices for optimal performance and management.
---
# Citrix Files Deployment Guide with Citrix Virtual Apps and Desktops Service

## Overview

This article provides step-by-step guidance to deploy Citrix Files with the Citrix Virtual Apps and Desktops service. Citrix Files for Windows is the client software used to securely interact with files in the Content Collaboration platform. Users can view all their files in their account from a single location regardless of where the files are located on the back end (public cloud, corporate file share, etc.). Files do not need to be synced locally which is useful to save space – especially in shared VDI environments.
Users can also easily upload files and share files with others including full context menu integration within Windows Explorer. For more information on Citrix Files for Windows, please refer to [CTX237126](https://support.citrix.com/article/CTX237126) and [CTX228273](https://support.citrix.com/article/CTX228273).

## Prerequisites

This guide assumes that the reader has a basic understanding of the following Citrix offerings as well as general Windows administrative experience:

*  Citrix Virtual Apps and Desktops Service
*  Citrix Content Collaboration

Refer to the following reference architectures for additional details as needed:

*  [Citrix Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.
*  [Citrix Content Collaboration with on-premises storage zones](/en-us/tech-zone/design/reference-architectures/customer-storage-zones-on-premises.html) - Learn about the architecture and design considerations for deploying an on-premises customer-managed storage zone.

    >**Note:**
    >
    >This guide is based on offerings from Citrix Cloud though also would mostly apply to traditional on-premises offerings as well.

## Section 1a: Installing Citrix Files as part of the Virtual Delivery Agent install

The Citrix Files for Windows app is included within the Virtual Delivery Agent (VDA) installation. In this section we install it in a Windows Server image using the installer from Citrix Cloud within the Virtual Apps and Desktops Service.
It can also be installed within a desktop operating system image. Refer to the product documentation page on [Install VDAs](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html) for additional details on installing the VDA.

   >**Note:**
   >
   >It is recommended to install the latest version of the Citrix Files for Windows app. The app is typically updated every 6 weeks so the version within the VDA install might not be the latest. Compare the version on the VDA to the version at [https://www.citrix.com/downloads/citrix-content-collaboration/product-software/citrix-files-for-windows.html](https://www.citrix.com/downloads/citrix-content-collaboration/product-software/citrix-files-for-windows.html). If the version on the VDA is behind, upgrade to the latest version using the command-line. If MSI installation is preferred (e.g. for use with SCCM and similar tools) first uninstall existing version before pushing out the MSI.

### Step-by-step guidance

**Estimated time to complete:** 10 minutes

1.  From within the Virtual Apps and Desktops Service in Citrix Cloud, click **Downloads**.
[![Citrix Virtual Apps and Desktops Home Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Home.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Home.png)

1.  Click **Download** next to the Virtual Delivery Agent (VDA).
[![Citrix Virtual Apps and Desktops Downloads Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Downloads.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Downloads.png)

1.  Download the Server OS Virtual Delivery Agent

    >**Note:**
    >
    >At time of writing, version 1906 was the latest but the version shown can be newer. We recommend using the latest version available for optimal performance, security, and functionality.

    ![Download Server OS VDA](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Download-Server-VDA.png)

1.  Run the VDA installer on your server VM image. Select the appropriate configuration option for your environment and then click **Next**. For more information on image delivery with MCS and PVS, refer to the [Image Management Reference Architecture](/en-us/tech-zone/design/reference-architectures/image-management.html).
[![VDA Install Environment Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Environment.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Environment.png)

1.  If needed, click the check box to install the optional Citrix Workspace app client in your image for connecting to resources (double-hop scenario). Then click **Next**.
[![VDA Install Core Components Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Core-Components.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Core-Components.png)

1.  Make sure that Citrix Files for Windows is checked to install the appropriate client software. Check any additional features you would like in your image and then click **Next**.
[![VDA Install Additional Components Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Additional-Components.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Additional-Components.png)

1.  Specify your Cloud Connector VMs under the Delivery Controller step. At least two are recommended for high availability. Click **Next**.
[![VDA Install Delivery Controller Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Delivery-Controller.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Delivery-Controller.png)

1.  Leave Optimize performance selected. Select any additional options as needed and click **Next**.

    >**Note:**
    >
    >For optimizing your master image before deployment we recommend running Citrix Optimizer from [https://support.citrix.com/article/CTX224676](https://support.citrix.com/article/CTX224676).

    [![VDA Install Features Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Features.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Features.png)

1.  Leave the default option for firewall configuration or adjust as needed and click **Next**.
[![VDA Install Firewall Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Firewall.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Firewall.png)

1.  Confirm the details on the Summary screen and click **Install**.
[![VDA Install Summary Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Summary.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Summary.png)

1.  Wait for the installation to complete.

    >**Note:**
    >
    >The machine can be restarted during the install process.

    [![VDA Install Progress Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Progress.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Progress.png)

1.  Opt in to collect diagnostic information if you’d like to help prevent issues. Then click **Next**.
[![VDA Install Diagnostics Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Diagnostics.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Diagnostics.png)

1.  Click **Finish** to complete the installation. Then allow the machine to restart.
[![VDA Install Finish Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Finish.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_VDA-Install-Finish.png)

### Key Takeaways for Section 1a: Installing Citrix Files as part of the VDA Install

*  The Citrix Files client installer is included within the VDA installer. It is still important to check for the latest version though.
*  Make sure Citrix Files is available within your base images when using Content Collaboration. Options are configured later for user groups as required.

## Section 1b: Installing Citrix Files with the VDA Command-line

The Citrix Files Client for Windows is included within the Virtual Delivery Agent (VDA) installation. In this section we install it in a Windows Server image using the command-line with the installer from Citrix Cloud within the Virtual Apps and Desktops Service. It can also be installed within a desktop operating system image. Refer to the product documentation page at [/en-us/citrix-virtual-apps-desktops-service/install-configure/install-command.html](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-command.html) for additional info on installing the VDA from the command-line.

### Step-by-step guidance

**Estimated time to complete:** 10 minutes

1.  Using the software deployment tool of your choice, such as Microsoft System Center Configuration Manager, run the command-line installation of the VDA on a virtual or physical machine as required. Use the /quiet and /includeadditional “Citrix Files for Windows” switches at a minimum.

    **Example:** 'VDAServerSetup_1906.exe /quiet /controllers “CC1.acf.lab”,“CC2.acf.lab” /enable_hdx_ports /includeadditional “Citrix Files for Windows”'

    >**Note:**
    >
    >The machine restarts one or more times during installation.

    [![Command-line VDA Install](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Command-Line-VDA-Install.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Command-Line-VDA-Install.png)

1.  When the install completes, verify that both the Virtual Delivery Agent and Citrix Files have been installed under Programs and Features.
[![Programs and Features Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Programs-and-Features.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Programs-and-Features.png)

### Key Takeaways for Section 1b: Installing Citrix Files with the VDA Command-line

*  The Citrix Files client installer is included within the VDA installer and can be installed silently
*  Silent installs of the VDA software are typically done on unmanaged virtual and physical machines as opposed to managed machines created from a master image.

## Section 2: Provisioning VDAs Using MCS or PVS

Deploy VDAs using MCS, PVS and/or manually as required. Refer to existing image management reference architecture and product docs for additional details.

### Helpful resources

*  [Image Management Reference Architecture](/en-us/tech-zone/design/reference-architectures/image-management.html)
*  [Machine Catalog Creation](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html)
*  [Provisioning Documentation](/en-us/provisioning)

## Section 3: Enabling Session Lingering

Session Lingering allows published application sessions to remain active for a specified duration after the last application from the session has been closed. The primary use case for this feature is to provide fast subsequent application launches when users close their published application and shortly afterwards launch another. In the case of Citrix Files, Session Lingering ensures that any background file upload tasks complete within the session before it is terminated. For more details on Session Lingering, please refer to [/en-us/citrix-virtual-apps-desktops-service/install-configure/delivery-groups-manage.html](/en-us/citrix-virtual-apps-desktops-service/install-configure/delivery-groups-manage.html).

   >**Note:**
   >
   >Session sharing is the default and preferred method to have multiple published applications running simultaneously. It conserves resources and prevents possible race conditions. See [CTX159159](https://support.citrix.com/article/CTX159159) for additional details.

### Step-by-step guidance

**Estimated time to complete:** 5 minutes

1.  From within the Studio management console, right-click on an existing Delivery Group that you would like to enable Session Lingering for and select **Edit Delivery Group**
[![Studio Edit Delivery Group](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Edit-Delivery-Group.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Edit-Delivery-Group.png)

1.  Click the **Application Lingering** tab
[![Application Lingering Tab](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Edit-Delivery-Group-Dialog.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Edit-Delivery-Group-Dialog.png)

1.  Select **Keep sessions active until** and enter a value of 1 or 2 minutes. This should prove sufficient for most environments and file sizes.
[![Application Lingering Dialog](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Application-Lingering.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Application-Lingering.png)

1.  Complete steps 1–3 above for any additional Delivery Groups that you want to enable the feature on.

### Key Takeaways for Section 3: Enabling Session Lingering

*  Session lingering keeps sessions active to make sure any background file transfers within Citrix Files complete
*  Setting a linger value for 1 or 2 minutes should prove sufficient in most environments

## Section 4: Testing Access and Single Sign-On

In this section we test access to the Citrix Files app including single sign-on (SSO). SSO takes place from Active Directory credentials associated with an email address that matches a user account registered for Content Collaboration.
See [/en-us/citrix-content-collaboration/citrix-files-app/configuration.html](/en-us/citrix-content-collaboration/citrix-files-app/configuration.html) for details.

### Step-by-step guidance

**Estimated time to complete:** 10 minutes

1.  Log in to Citrix Workspace and launch a desktop session with a user account enrolled in the Content Collaboration Service

    >**Note:**
    >
    >Each user’s email address in AD/AAD must match with one in Content Collaboration. Also, make sure you are using VDA version 1808 or later for this to work.

1.  Open the Citrix Files app. Notice that you are not prompted to enter credentials to the app and any files available are presented.

1.  For earlier versions of the VDA before 1808 or when SAML is preferred, refer to [CTX208557](https://support.citrix.com/article/CTX208557) for the required configuration details.
The “Account” GPO must be deployed for the Citrix Files app on Windows to ensure the app knows where to send the user for authentication. GPO configuration information is specific in a later section.

  >**Note:**
  >
  >For seamless SSO, use an IdP that supports it, such as AD FS, and configure the ShareFile SSO configuration page (found under Admin Settings – Security – Login and Security for Windows Integrated Authentication). If not using authentication via Workspace or Windows Integrated Authentication, then the user is prompted with an authentication challenge.
  
1.  SSO with Citrix Files works the same for both published desktops and published apps

### Key Takeaways for Section 4: Testing Access and SSO

*  Citrix Files seamlessly integrates to virtual app and desktop sessions
*  SSO is primarily handled by email address tied to AD account and Content Collaboration account

## Section 5: Configuring File Type Association (Optional)

Applications can be associated with file types/extensions to make sure users can easily work with their files in Citrix Files – creating and editing content as required. Some scenarios include Citrix Files/Workspace app on an endpoint device without appropriate software available or double-hop situations with a generic desktop containing Citrix Files/Workspace app and appropriate software not available. Refer to [/en-us/citrix-content-collaboration/files-workspace.html](/en-us/citrix-content-collaboration/files-workspace.html) for more details.

   >**Note:**
   >
   >We enable FTA for specific file name extensions with Microsoft Word. Similar steps apply for any file name extension type and published app combination.

### Step-by-step guidance

**Estimated time to complete:** 5 minutes

1.  From the Studio management console, select the **Applications** node
[![Studio Applications Node](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Applications-Node.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_CVADS-Applications-Node.png)

1.  Right-click the **Microsoft Word** published application and select **Properties**
[![Studio Application Properties](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Published-App-Properties.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_Published-App-Properties.png)

1.  Select the **File Type Associations** tab
[![FTA Screen](/en-us/tech-zone/build/media/deployment-guides_citrix-files_FTA-Screen.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_FTA-Screen.png)

1.  Select the file name extensions you’d like to associate and click **OK**
[![FTAs Selected](/en-us/tech-zone/build/media/deployment-guides_citrix-files_FTAs-Selected.png)](/en-us/tech-zone/build/media/deployment-guides_citrix-files_FTAs-Selected.png)

### Key Takeaways for Section 5: Configuring File Type Association (Optional)

*  FTA ensures that users can easily open the appropriate virtual applications for file types they are working with
*  FTA is configured with known file name extensions mapped to specific published applications

## Section 6: Configuring GPO Settings (Optional)

Referring to [CTX228273](https://support.citrix.com/article/CTX228273), there are several GPO settings to optionally configure that alter the behavior of Citrix Files in an environment. Some of the common ones that we cover here are mount points, enabling/disabling Citrix Files for specific users/machines and disabling auto-updates for non-persistent machines.

### Step-by-step guidance

**Estimated time to complete:** 10 minutes

1.  Per [CTX228273](https://support.citrix.com/article/CTX228273), Copy the .admx file to c:\Windows\PolicyDefinitions and the \en-us\ .adml file to c:\Windows\PolicyDefinitions\en-us\

1.  Open Group Policy Editor and the policy options are available under:
Computer Configuration → Administrative Templates → Citrix Files
User Configuration → Administrative Templates → Citrix Files

1.  Configure Mount Points if necessary to give users access to a Citrix Files folder mounted as a network drive. Network drives provide users with a familiar experience and also not break internal links within documents (e.g. a Word document with an OLE Excel sheet relying on the absolute path of that Excel file) that have been created prior to a move away from network drive mappings.

1.  For users/groups and/or machines where Citrix Files should not be enabled yet the application is installed, configure the “Enable Application” setting under User or Computer

1.  Clear “Enable Auto-update” check box on non-persistent VDA images. Include Citrix Files updates as part of your regular image update cycles.

1.  Another option worth considering is the “Cache Location”.

    >**Note:**
    >
    >The default cache location is sufficient for pooled desktops. The cache is wiped upon logoff and not written back to the roaming profile which is very efficient. For hosted-shared desktops, it depends on whether you want the cache on the OS drive depending on space available. Else use a secondary disk, mounted invisible to the user to put the cache. For unmanaged desktops, it’s usually best to not attach a second disk as the changes would be stored inside the unmanaged image. And it’s a cache, so if it is destroyed at some point the worst case scenario is downloading the file objects again.

### Key Takeaways for Section 6: Configuring GPO Settings (Optional)

*  GPO settings allow customizing the Content Collaboration environment regardless of where the Citrix Files client software is installed (e.g. endpoint, non-persistent VDA, etc.)
*  GPO settings are available in both user and computer configuration options

## Section 7: Configuring Profile Containers with Citrix Profile Management (Optional)

As users access their files within the Citrix Files app, the files are cached locally helping to speed up subsequent access requests. Once a user logs off from a non-persistent machine though, the cache is lost and must be rebuilt in future sessions. Profile Container technology in Citrix Profile Management allows the Citrix Files cache to efficiently persist across user sessions offering an improved experience. Refer to
[/en-us/profile-management/current-release/configure/profile-container.html](/en-us/profile-management/current-release/configure/profile-container.html) for more information on Profile Containers.

   >**Note:**
   >
   >When using Citrix Profile Management and the Citrix Files app, it is important to ensure users properly logoff or force a session logoff. This helps guarantee that the data syncs up correctly with the profile. Define the session limits policy as outlined at [/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-limits-policy-settings.html](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-limits-policy-settings.html).

### Step-by-step guidance

**Estimated time to complete:** 10 minutes

1.  Open the Group Policy Management Editor.

1.  Under Policies > Administrative Templates: Policy definitions (ADM) > Classic Administrative Templates (ADM) > Citrix > Profile Management > File system > Synchronization, double-click the **Profile container** policy.

1.  Select **Enabled**

1.  Click Show to add the relative paths of the folders to the list. Add the following paths:
AppData\Local\Citrix\Citrix Files\PartCache
AppData\Local\Citrix\Citrix Files\db

1.  Click **OK**

1.  Under Policies > Administrative Templates > Citrix Files, set the "Cache Mode" policy to "Queued".

1.  Make sure Cache Location policy is not configured.

### Key Takeaways for Section 7: Configuring Profile Containers with Citrix Profile Management (Optional)

*  Enabling Profile Containers with Citrix Profile Management can improve the user experience with Citrix Files in non-persistent environments – especially for users with many files and/or large files
*  The profile container is a Microsoft VHDX virtual disk that is mounted on the fly to a user session

## Summary

The Citrix Files for Windows Client software allows the secure saving, retrieving, and sharing of data. It works with the Content Collaboration service offering in Citrix Cloud - aggregating various back end cloud and on-premises data storage locations for a seamless experience. This deployment guide covered the specific steps necessary to install and configure the Citrix Files for Windows Client in a virtual app and desktop environment. The end result is an integrated solution with true single sign-on for users to access all of their files from any hosted-shared or VDI session.
