---
layout: doc
description: Copy & paste description from TOC here
---
# App Protection Policies

## Contributors

**Author:** [Martin Zugec](https://twitter.com/MartinZugec) & [Alvin Raagas](https://twitter.com/AlvinRaagas)

## Overview

This guide is designed to walk you through the technical prerequisites, use cases and configuration of App protection policies for your on-premises Citrix Virtual Apps and Desktops deployment. App protection is an add-on feature for Citrix Workspace app (CWA) that provides enhanced security when using Citrix Virtual Apps and Desktops published resources. Two policies provide anti-keylogging and anti screen capturing capabilities in a Citrix HDX session.

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
    > These operating systems are supported where Citrix Workspace app is installed (typically endpoint). The VDA supports all operating systems, including server OS.

### Licenses

Valid Citrix licenses are required:

-  Citrix Virtual Apps and Desktops
-  App protection add-on license

### Infrastructure

Following server components are required:

-  StoreFront 1912 or higher
-  Delivery Controller 1912 or higher

## Install - Delivery Controller

1.  After you purchase the app protection feature, download the `FeatureTable.OnPrem.AppProtection.xml` file from the Citrix Virtual Apps and Desktops 1912 or later download page

    Note: May need to filter \ narrow results to Components

    ![Download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_13.png)

    ![Download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_14.png)

1.  On any Delivery Controller, launch PowerShell and load the Citrix PowerShell Snapins using cmdlet

    `Add-PSSnapin Citrix*`

    ![Import snapin](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_15.png)

1.  In PowerShell, navigate to folder where XML file has been downloaded
1.  Enable the App protection feature with the following command:

    `Import-ConfigFeatureTable FeatureTable.OnPrem.AppProtection.xml`

    ![Import feature table](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_16.png)

1.  Verify that App Protection is enabled with the following command:

    `Get-ConfigEnabledFeature | Select-String –Pattern "AppProtection"`

    ![Get feature](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_17.png)

1.  Enable XML Trust by running the following command:

    `Set-BrokerSite -TrustRequestsSentToTheXmlServicePort $true`

    ![Set XML trust](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_21.png)

## Install - Licensing

1.  Download the license file and import it into the Citrix License Server alongside an existing Citrix Virtual Desktops license
2.  Use the Citrix Licensing Manager to import the license file (preferred method) or copy the license file to `C:\Program Files (x86)\Citrix\Licensing\MyFiles` on the License Server and restart the Citrix Licensing service. For more information, see [Import license files](/en-us/licensing/current-release/manage/import-license-files.html)

## Install - StoreFront

1.  On StoreFront server, run the following PowerShell command:

    `Add-STFFeatureState -Name "Citrix.StoreFront.AppProtectionPolicy.Control" -IsEnabled $True`
1.  In a multiple-server StoreFront deployment, you must manually propagate these changes to all the other servers in the server group
1.  Verify that the feature is enabled on StoreFront by running the following PowerShell command

    `Get-STFFeatureState -Name "Citrix.StoreFront.AppProtectionPolicy.Control"`

    ![Get STF feature](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_18.png)

## Install - Citrix Workspace app

1.  Include the app protection component using one of the following methods:

    For Windows: During Citrix Workspace app installation (for Windows), select **Enable app protection** and then click **Install** to continue with the installation or use the command-line switch `CitrixWorkspaceApp.exe /includeappprotection`. For more information, see [App protection section](https://docs.citrix.com/en-us/citrix-workspace-app-for-windows/configure.html#app-protection) of Citrix Workspace app for Windows production documentation.

    ![Install feature](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_5.png)

    For macOS: App protection requires no specific installation or configuration on Citrix Workspace for Mac.
1.  Launch Citrix Workspace app and authenticate to StoreFront. Web stores are not supported, only native.
1.  Click on a virtual app or desktop icon.
1.  If App protection was not included as part of the CWA installation, you will see the following popup when trying to launch a protected virtual app or desktop.  Click **Yes**

    ![Optional download](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_8.png)

    ![Optional install](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_9.png)

1.  Click **Finish**

    ![Finish](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_11.png)

1.  Click **Yes** to restart your computer

    ![Restart](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_12.png)

## Configuration - Delivery Group

Anti-keylogging and anti screen capture protection is configured on delivery group level using PowerShell.There are two properties on each delivery group that affects the behavior of app protection policies:

-  AppProtectionKeyLoggingRequired - can be $True (enabled) or $False (disabled)
-  AppProtectionScreenCaptureRequired - can be $True (enabled) or $False (disabled)

1.  On any Delivery Controller, launch PowerShell and load the Citrix PowerShell Snapins using cmdlet

    `Add-PSSnapin Citrix*`
1.  To Enable App protection for the `Admin Desktop` delivery group, use the following command:

    `Set-BrokerDesktopGroup -Name "Admin Desktop" -AppProtectionKeyLoggingRequired $True -AppProtectionScreenCaptureRequired $True`

    ![Set property](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_19.png)

1.  Validate the settings by running the following PowerShell command:

    `Get-BrokerDesktopGroup -Property Name, AppProtectionKeyLoggingRequired, AppProtectionScreenCaptureRequired | Format-Table -AutoSize`

    ![Get properties](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_20.png)

## Testing - Citrix Workspace app for Windows

1.  Launch Citrix Workspace app and login

    ![Launch Workspace](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_25.png)

2.  Click on a protected virtual app or virtual desktop (for example Admin Desktop) and launch the HDX session. If you don't see protected resources, you are probably using web store or unsupported Citrix Receiver / Citrix Workspace app.

    ![Launch resource](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_26.png)

3.  Try to perform a screen capture

    ![Take screenshot](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_27.png)

4.  Confirm that you see a blank screen (expected behavior)

    ![Blank screenshot](/en-us/tech-zone/learn/media/poc-guides_app-protection-policies_28.png)
