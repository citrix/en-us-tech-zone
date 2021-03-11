---
layout: doc
h3InToc: true
contributedBy: Martin Zugec, Alvin Raagas
description: Learn how to enhance the security of your endpoints with App protection policies as part of Citrix Virtual Apps and Desktops deployment. Protect your users with anti-keylogging and anti screen capture functionality.
---
# App Protection Policies

## Overview

This guide is designed to walk you through the technical prerequisites, use cases, and configuration of App protection policies for your on-premises Citrix Virtual Apps and Desktops deployment. App protection is an add-on feature for Citrix Workspace app (CWA) that provides enhanced security when using Citrix Virtual Apps and Desktops published resources. Two policies provide anti-keylogging and anti screen capturing capabilities in a Citrix HDX session.

## System Requirements

App protection policies feature requires specific version of Citrix Workspace app and supports Windows and macOS endpoints. Special add-on license is required together with configuration changes on StoreFront and Delivery Controller servers. You can read more about system requirements in [product documentation](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html#system-requirements).

### Citrix Workspace app

Minimum versions:

-  Citrix Workspace app for Windows 1912 LTSR
-  Citrix Workspace app for Mac 2001+
-  Citrix Workspace app for Windows 2002+

Operating Systems Supported:

-  Windows 7, 8.1 and 10
-  macOS 10.13 (High Sierra) and newer

Server operating systems (for example Windows Server 2019) are not supported.

  >**Note:**
  >
  >These operating systems are supported where Citrix Workspace app is installed (typically endpoint). The VDA supports all operating systems, including server OS.

### Licenses

Valid Citrix licenses are required:

-  Citrix Virtual Apps and Desktops
-  App protection add-on license

### Infrastructure

Following server components are required:

-  StoreFront 1912 or higher
-  Delivery Controller 1912 or higher

## Installation - Delivery Controller

  >**Note:**
  >
  >Following steps are only required for Citrix Virtual Apps and Desktops versions 1912, 2003 and 2006, app protection feature is automatically included in newer releases. Only required step on newer releases is to enable XML trust (first step).

1.  Enable XML Trust by running the following command:

    `Set-BrokerSite -TrustRequestsSentToTheXmlServicePort $true`

    ![Set XML trust](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_21.png)

1.  After you purchase the app protection feature, download the `FeatureTable.OnPrem.AppProtection.xml` file from the Citrix Virtual Apps and Desktops 1912 or later download page.

    >**Note:**
    >
    >App Protection Policies XML file is located under Components

    ![Download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_13.png)

1.  Click on **Download File** and save it to local disk

    ![Download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_14.png)

1.  On any Delivery Controller, launch PowerShell and load the Citrix PowerShell snap-ins using cmdlet

    `Add-PSSnapin Citrix*`

    ![Import snap-in](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_15.png)

1.  In PowerShell, navigate to folder where XML file has been downloaded
1.  Enable the App protection feature with the following command:

    `Import-ConfigFeatureTable FeatureTable.OnPrem.AppProtection.xml`

    ![Import feature table](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_16.png)

1.  Verify that App Protection is enabled with the following command:

    `Get-ConfigEnabledFeature | Select-String –Pattern "AppProtection"`

    ![Get feature](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_17.png)

## Installation - Licensing

1.  Download the license file and import it into the Citrix License Server alongside an existing Citrix Virtual Desktops license
2.  Use the Citrix Licensing Manager to import the license file. For more information, see [Install licenses](/en-us/licensing/current-release/citrix-licensing-manager/install.html)

## Installation - Citrix Workspace app

1.  Include the app protection component using one of the following methods:

    **For Windows:** During Citrix Workspace app installation (for Windows), select **Enable app protection** and then click **Install** to continue with the installation or use the command-line switch `CitrixWorkspaceApp.exe /includeappprotection`. For more information, see [App protection section](/en-us/citrix-workspace-app-for-windows/configure.html#app-protection) of Citrix Workspace app for Windows production documentation.

    ![Install feature](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_5.png)

    **For macOS:** App protection requires no specific installation or configuration on Citrix Workspace for Mac.

    >**Note:**
    >
    >It is not possible to add App protection support to older clients. Uninstall old version of Citrix Receiver / Citrix Workspace app and install new version with App protection component.

1.  Click **Finish**

    ![Finish](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_11.png)

1.  Click **Yes** to restart your computer

    ![Restart](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_12.png)

## Configuration - Delivery Group

Anti-keylogging and anti screen capture protection is configured on delivery group level using PowerShell. There are two properties on each delivery group that affects the behavior of app protection policies:

-  `AppProtectionKeyLoggingRequired` - can be `$True` (enabled) or `$False` (disabled)
-  `AppProtectionScreenCaptureRequired` - can be `$True` (enabled) or `$False` (disabled)

1.  On any Delivery Controller, launch PowerShell and load the Citrix PowerShell snap-ins using cmdlet

    `Add-PSSnapin Citrix*`
1.  To Enable App protection for the `Admin Desktop` delivery group, use the following command:

    `Set-BrokerDesktopGroup -Name "Admin Desktop" -AppProtectionKeyLoggingRequired $True -AppProtectionScreenCaptureRequired $True`

    ![Set property](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_19.png)

1.  Validate the settings by running the following PowerShell command:

    `Get-BrokerDesktopGroup -Property Name, AppProtectionKeyLoggingRequired, AppProtectionScreenCaptureRequired | Format-Table -AutoSize`

    ![Get properties](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_20.png)

## Testing - Citrix Workspace app for Windows

Following steps provides guidance for anti screen sharing testing only. To test anti-keylogging protection, we recommend consulting with your own security team.

1.  Launch Citrix Workspace app and login

    ![Launch Workspace](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_25.png)

1.  Click on a protected virtual app or virtual desktop (for example Admin Desktop) and launch the HDX session. If you don't see protected resources, you are probably using web store or unsupported Citrix Receiver / Citrix Workspace app.

    ![Launch resource](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_26.png)

1.  (Optional) If App protection is not installed, you get the following popup when trying to launch a protected virtual app or desktop. Click **Yes**

    ![Optional download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_8.png)

    >**Note:**
    >
    >This option is not available with older versions of Citrix Receiver / Citrix Workspace app

1.  Try to perform a screen capture

    ![Take screenshot](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_27.png)

1.  Confirm that you see a blank screen (expected behavior)

    ![Blank screenshot](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_28.png)

When testing anti-keylogging and anti screen capture protection, be aware of expected behavior:

-  **Anti-keylogging** - This feature is active only when a protected window is in focus
-  **Anti screen capture** - This feature is active when a protected window is visible (not minimized)

Another simple method to test the anti screen capture protection is to use one of the popular conference tools (GoToMeeting, Microsoft Teams, Zoom, or Slack). Screen sharing should not be possible when protection is enabled.

## References

[Product Documentation - Citrix Workspace app](/en-us/citrix-workspace-app-for-windows/configure.html#app-protection)

[Product Documentation - App protection](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html)
