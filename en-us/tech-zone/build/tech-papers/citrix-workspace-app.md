---
layout: doc
h3InToc: true
contributedBy: Dennis Span
specialThanksTo: Santosh Sampath, Martin Zugec, Nick Rintalan
description: Quick start guide for Citrix Workspace app - everything you need to know in one place, including installation, configuration, and optimizations.
tz_title: Citrix Workspace app quick start guide
tz_products: citrix-workspace;
---
# Tech Paper: Citrix Workspace app quick start guide

## Overview

Hi again!

Citrix Workspace app for Windows provides access to a user’s resources using Citrix Virtual Apps and Desktops. These resources include SaaS, web and legacy applications and desktops. Citrix Workspace app provides access from the desktop, start menu, Citrix Workspace user interface and web browsers.

On a side note, it is possible to access resources on a Windows device using the Citrix Workspace app for HTML5, without installing the Citrix Workspace app for Windows. However, there is a significant feature disparity between both clients. For a complete overview of all features supported in each of the available Workspace app versions see the [Citrix Workspace app feature matrix](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-workspace-app-feature-matrix.pdf?_ga=2.27649141.890901474.1583830777-25366109.1583146462).

## Installation

The latest version of Citrix Workspace app for Windows can be downloaded [here](https://www.citrix.com/downloads/workspace-app/windows/workspace-app-for-windows-latest.html). Citrix Workspace app can be installed on both your Windows clients and on your Citrix workers (your VDAs).

Although technically possible, there is no need to extract the files included in the *CitrixWorkspaceApp.exe*. It is highly recommended to install Citrix Workspace app using the executable directly.

It is possible to rename the installation file *CitrixWorkspaceApp.exe* to *CitrixWorkspaceApp**Web**.exe*. Renaming this file removes the *Add Account* button from the last dialog window at the end of the installation. It also prevents the *Add Account* window (First Time Use, FTU) from appearing at first login. This mode is more suitable if only access through web store is planned.

In case you would like to install Citrix Workspace app **without administrative privileges** be aware of the following:

-  The Microsoft Visual C++ Redistributable 2017 (both 32-bit and 64-bit) must be pre-installed on the local machine. This is a prerequisite for the installation of Citrix Workspace app. The Visual C++ Redistributable can only be installed with administrative privileges.
-  The following components of Citrix Workspace app can only be installed with administrative privileges:
    -  SSON (Single Sign-On)
    -  URL Redirection
    -  Local App Access
    -  USB support
    -  App Protection
-  If the installation is user-based, Citrix Workspace app must be installed for each user who logs on to the local machine. The default installation path for user-based installations is `C:\Users\%UserName%\AppData\Local\Citrix\ICA Client`.
-  To avoid potential conflicts, make sure to uninstall all user-based installations of Citrix Workspace app on the local machine before installing Citrix Workspace app using administrative rights.

### Command Line Arguments

Citrix Workspace app comes with many installation parameters. For a complete overview of all available parameters see the [product documentation](/en-us/citrix-workspace-app-for-windows/install.html#using-command-line-parameters).

For a successful rollout make sure to understand each of the parameters and that they are aligned with your organization’s requirements. For example:

-  Do some users in your organization require Local App Access? Local App Access enables the integration of locally installed applications within a hosted desktop. If yes, make sure to include the parameter */FORCE_LAA=1* to install the Local App Access component.
-  Are users allowed to use Citrix Virtual Apps and Desktops without having to re-authenticate again? If yes, make sure to include the parameters */includeSSON* to install the single sign-on component and /*ENABLE_SSON=Yes* to enable the single sign-on component.

    >**Note:**
    >
    >You can also use the **Citrix Workspace app Commandline Tool** to help you to build the exact command line syntax ([CTX227370](https://support.citrix.com/article/CTX227370)).

When upgrading to Citrix Workspace app from an older and unsupported version (for example Citrix Receiver 3.4) make sure to use the parameter */rcu* or */forceinstall* (Citrix Workspace app 1909 and later).

Keep in mind that some components, for example enabling single sign-on, require a reboot of the local machine.

Here is an example of the command line syntax: `CitrixWorkspaceApp.exe /silent /includeSSON /FORCE_LAA=1`

### Installation Path

The default installation path for machine-based installations is *C:\Program Files (x86)\Citrix\ICA Client*.

Citrix Workspace app writes multiple log files to the *%TEMP%* directory in the subfolder *CTXReceiverInstallLogs-%Date%-%Time%*. The following log files are created:

-  Log files from the main installer:
    -  TrolleyExpress-%Date%-%Time%.log
-  Log files per component (MSI installer):
    -  CtxInstall-AppProtection-%Date%-%Time%.log
    -  CtxInstall-AuthManager-%Date%-%Time%.log
    -  CtxInstall-CtxBrowserInstaller-%Date%-%Time%.log
    -  CtxInstall-DesktopViewer-%Date%-%Time%.log
    -  CtxInstall-GenericUSB-%Date%-%Time%.log
    -  CtxInstall-ICAWebWrapper-%Date%-%Time%.log
    -  CtxInstall-PackageInstaller-%Date%-%Time%.log
    -  CtxInstall-RIInstaller-%Date%-%Time%.log
    -  CtxInstall-SelfServicePlugin-%Date%-%Time%.log
    -  CtxInstall-Vd3dClient-%Date%-%Time%.log
    -  CtxInstall-WebHelper-%Date%-%Time%.log
    -  CtxInstall-WinDockerInstaller-%Date%-%Time%.log

The exact location of the temp folder depends on the user executing the installation. For example, the temp folder of the local system account (used by Microsoft SCCM for example) is *C:\Windows\Temp*.
The logfile path cannot be changed. It is possible to copy the log files to a different directory after the installation of Citrix Workspace app has finished.

## Configuration

In an Active Directory infrastructure, Citrix Workspace app can be centrally configured using Microsoft group policies. This requires that the administrative templates (the ADMX and ADML files) for Citrix Workspace app are copied to your Group Policy Central Store.

ADMX files contain the actual settings. ADML files are the language files that contain the text displayed in the Group Policy Management Console.

The administrative template files are included in the installation directory of Citrix Workspace app:

-  `C:\Program Files (x86)\Citrix\ICA Client\Configuration\CitrixBase.admx`
-  `C:\Program Files (x86)\Citrix\ICA Client\Configuration\receiver.admx`
-  `C:\Program Files (x86)\Citrix\ICA Client\Configuration\%language%\CitrixBase.adml`
-  `C:\Program Files (x86)\Citrix\ICA Client\Configuration\%language%\receiver.adml`

They can also be downloaded from the Citrix website in the section [Downloads for admins (deployment tools)](https://www.citrix.com/downloads/workspace-app/windows/workspace-app-for-windows-latest.html).

Copy the ADMX and ADML files to the Group Policy Central Store:

-  The default path for the ADMX files is: `%LogonServer%\sysvol\%Domain%\Policies\PolicyDefinitions`
-  The default path for the ADML files is: `%LogonServer%\sysvol\%Domain%\Policies\PolicyDefinitions\%Language%`

For testing purposes, you can copy the administrative templates to a local machine (*C:\Windows\PolicyDefinitions*) and use the local group policy editor (gpedit.msc) to view and manage the settings.

Citrix Workspace app comes with both per-machine and per-user settings. There are many settings available, for example:

-  *Computer Configuration \ Policies \ Administrative Templates \ Citrix Components \ Citrix Workspace*
    -  **DPI \ High DPI:** in most environments the default setting will suffice, but in case users with multiple screens report issues you may need to change the DPI configuration.
    -  **SelfService \ EnableFTU:** disable this setting to prevent the *Add Account* window at the user’s first login. Renaming the installation file *CitrixWorkspaceApp.exe* to *CitrixWorkspaceApp**Web**.exe* has the same result.
    -  **StoreFront \ NetScaler Gateway URL/StoreFront Accounts List:** use this setting to automatically add NetScaler Gateway or StoreFront URLs to the user’s Workspace app.
-  *User Configuration \ Policies \ Administrative Templates \ Citrix Components \ Citrix Workspace*
    -  **User Authentication \ Local username and password:** to allow single sign-on, enable this setting and tick the options *Enable pass-through authentication* and *Allow pass-through authentication for all ICA connections*.

When launching a session, the user may be presented with a dialog window asking for permissions concerning device access (for example for local drives, webcams or microphones). By default, the Desktop Viewer client device restrictions are based on the Internet region. This behavior can be changed by creating and configuring the *Client Selective Trust* registry keys. As an administrator, you can define the access level by modifying the registry. There are four access levels:

-  0 = No Access
-  1 = Read-Only
-  2 = Full Access
-  3 = Prompt User

The *Client Selective Trust* registry keys and values are not created automatically. They can be created and configured either using Group Policy or directly in the registry. The following article provides detailed information about how to configure *Client Selective Trust* registry settings: [CTX133565](https://support.citrix.com/article/CTX133565).

In the article, a ZIP file can be downloaded containing both Group Policy files (ADM, ADMX and AMDL) and REG files. When using Group Policy to configure your clients the recommended approach is to use the ADMX and ADML files.

![Client Selective Trust Group Policies](/en-us/tech-zone/build/media/tech-papers_citrix-workspace-app_ClientSelectiveTrustGroupPolicies.png)

It is also possible to use the REG file to create and configure the *Client Selective Trust* registry keys and values. Use the file *ReceiverCSTRegUpx64.reg* for 64-bit operating systems or the file *ReceiverCSTRegUpx86.reg* for 32-bit operating systems. When using the REG file, keep in mind that users can change the device access settings. If you want to prevent the user from changing the preferences, set the value *(Default)* in the following registry key to *false*:

`HKLM\SOFTWARE\WOW6432Node\Citrix\ICA Client\Client Selective Trust\oidPredefinedSecurityPolicySettings\InstantiatedSecurityPolicyEditable`

Sometimes, administrative templates from other products are required to configure the behavior of Citrix Workspace app.

**Warning:** the following configurations might allow malicious websites to launch a session silently to their malicious VDAs without any prompts.

For example, when the prompt **Open Citrix Workspace Launcher** is displayed in Google Chrome and Microsoft Edge on Chromium each time a user launches a StoreFront resource, configure the following settings:

-  User Configuration \ Administrative Templates \ Google \ Google Chrome
    -  **Policy setting:** Allow access to a list of URLs -> enable
    -  **Policy value:** receiver://*
-  User Configuration \ Administrative Templates \ Microsoft Edge
    -  **Policy setting:** Define a list of allowed URLs -> enable
    -  **Policy value:** receiver://*

## Optimization and security

Working with virtual applications and desktops on remote systems means two things:

1.  The resources (CPU/RAM) of the remote system are used by multiple users at the same time and may at times run under heavy load.
2.  Communication between the local endpoint and the remote system may be less than optimal due to network performance (for example latency and jitter).

For this reason, various optimization features have been introduced.

### Citrix HDX RealTime Optimization Pack

One of these features concerns the Citrix HDX RealTime Optimization Pack for Microsoft Skype for Business. The RealTime Optimization Pack consists of two components:

1.  The HDX RealTime Media Engine (runs on the local endpoint together with Citrix Workspace app).
2.  The HDX RealTime Connector (runs on the VDA in the data center together with the Microsoft Skype for Business client).

The HDX RealTime Media Engine performs media processing directly on the user device. It offloads the server for maximum scalability, minimizing network bandwidth consumption, and ensuring optimal audio-video quality.

The HDX RealTime Media Engine can only be installed when the Citrix Workspace app is already present.

Download the latest version of the Citrix RealTime Optimization Pack [here](/en-us/hdx-optimization/current-release/download.html).

For more information on the Citrix HDX RealTime Optimization Pack for Microsoft Skype for Business see the [product documentation](/en-us/hdx-optimization/current-release.html).

### Optimization for Microsoft Teams

Concerning Microsoft Teams, no further optimization is required. All available optimization features are included by default in Citrix Workspace app and the Virtual Delivery Agent (VDA) in the following versions:

-  Citrix Workspace app 1907 or later
-  Delivery Controller 1906.2 or later
-  Virtual Delivery Agent (VDA) version 1906.2 or later
-  Microsoft Teams version 1.2.00.31357 or later

For more information see the [product documentation](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html).

### Browser Content Redirection

The feature *Browser content redirection* prevents the rendering of whitelisted webpages on the VDA side. Instead, the webpages are rendered on the local endpoint.

For more information on how to configure *Browser Content Redirection* see the [product documentation](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html).

### Citrix Desktop Lock

Some users in your organization may not need to interact with the local desktop of their PC; they may only need their virtual desktop. Citrix Desktop Lock can lock down physical PCs and effectively puts these machines into kiosk mode. When the PC is started, the user is presented with their virtual desktop instead of the desktop of the local OS.

Citrix Desktop Lock is a separate component and is not included in Citrix Workspace app. Citrix Workspace app must be installed before Citrix Desktop Lock can be installed. SSON must be enabled when installing Citrix Workspace app and a store must be configured, either during installation or using a Group Policy. Citrix Desktop Lock works on domain-joined machines.

Download the latest version of Citrix Desktop Lock [here](https://www.citrix.com/downloads/workspace-app/additional-client-software/workspace-app-desktop-lock-Latest2.html).

For more information about installing and configuring Citrix Desktop Lock see the [product documentation](/en-us/citrix-workspace-app-for-windows/workspace-windows-desktop-lock.html).

### App protection

Citrix Workspace app version 1912 introduced the new security feature *app protection*. This is an add-on feature for Citrix Workspace app that provides enhanced security when using Citrix Virtual Apps and Desktops published resources. To be more specific. *App protection* provides anti-keylogging and anti-screen-capturing capabilities in a Citrix HDX session.

For silent installations, make sure to include the switch */includeappprotection* when installing Citrix Workspace app ([product documentation](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html#2-citrix-workspace-app)).

For more information about *app protection* see the [product documentation](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html).

## Automation

Larger organizations may want to automate the installation and configuration of Citrix Workspace app on their Windows endpoint devices. There are various ways how to go about this.

Several deployment scripts (\*.bat files) can be downloaded from the following URL in the section [Downloads for admins](https://www.citrix.com/downloads/workspace-app/windows/workspace-app-for-windows-latest.html).

Before they can be deployed, these batch files first need to be customized to suit your organization's environment.

Another option is to use a custom PowerShell script such as this one:

### Disclaimer

*These software applications are provided to you as is with no representations, warranties or conditions of any kind.  You may use and distribute it at your own risk. CITRIX DISCLAIMS ALL WARRANTIES WHATSOEVER, EXPRESS, IMPLIED, WRITTEN, ORAL OR STATUTORY, INCLUDING WITHOUT LIMITATION WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NONINFRINGEMENT. Without limiting the generality of the foregoing, you acknowledge and agree that (a) the software application may exhibit errors, design flaws or other problems, possibly resulting in loss of data or damage to property; (b) it may not be possible to make the software application fully functional; and (c) Citrix may, without notice or liability to you, cease to make available the current version and/or any future versions of the software application.  In no event should the code be used to support of ultra-hazardous activities, including but not limited to life support or blasting activities.  NEITHER CITRIX NOR ITS AFFILIATES OR AGENTS WILL BE LIABLE, UNDER BREACH OF CONTRACT OR ANY OTHER THEORY OF LIABILITY, FOR ANY DAMAGES WHATSOEVER ARISING FROM USE OF THE SOFTWARE APPLICATION, INCLUDING WITHOUT LIMITATION DIRECT, SPECIAL, INCIDENTAL, PUNITIVE, CONSEQUENTIAL OR OTHER DAMAGES, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGES.  You agree to indemnify and defend Citrix against any and all claims arising from your use, modification or distribution of the code.*

### Script Example

```powershell
#==========================================================================
# INSTALLING AND CONFIGURING CITRIX WORKSPACE APP FOR WINDOWS
#
# Author: Citrix Systems, Inc.
# Date  : 16.03.2020
# Editor: Microsoft Visual Studio Code
# Citrix Workspace app versions supported by this script: ALL
#==========================================================================

# Error handling
$global:ErrorActionPreference = "Stop"
if($verbose){ $global:VerbosePreference = "Continue" }

# Disable File Security (prevents the "Open File – Security Warning" dialog -> "Do you want to run this file")
$env:SEE_MASK_NOZONECHECKS = 1

# Custom variables [edit | customize to your needs]
$LogDir = "C:\Logs\Citrix Workspace app"                                       # the full path to your log directory
$LogFile = Join-Path $LogDir "Install Citrix Workspace app.log"                # the full path to your log file
$StartDir = $PSScriptRoot                                                      # the directory path of the installation file(s). $PSScriptRoot is the directory of the current script.
$InstallFileName = "CitrixWorkspaceApp.exe"                                    # the name of the installation file. Options: 'CitrixWorkspaceApp.exe' or 'CitrixWorkspaceAppWeb.exe'.
$InstallArguments = "/silent /includeSSON /FORCE_LAA=1"                        # the command line arguments for the installation file
$ClientSelectiveTrustRegKeys = "CitrixWorkspaceApp_Client_Selective_Trust.reg" # the name of the registry file containing the Client Selective Trust settings

# Create the log directory if it does not exist
if (!(Test-Path $LogDir)) { New-Item -Path $LogDir -ItemType directory | Out-Null }

# Function WriteToLog
Function WriteToLog {
    param(
        [string]$InformationType,
        [string]$Text
    )

    $DateTime = (Get-Date -format dd-MM-yyyy) + " " + (Get-Date -format HH:mm:ss)
    if ( $Text -eq "" ) {
        Add-Content $LogFile -value ("")   # Write an empty line
    } else {
        Add-Content $LogFile -value ($DateTime + " " + $InformationType.ToUpper() + " - " + $Text)
    }
}  

# Create a new log file (overwriting any existing one)
New-Item -Path $LogFile -ItemType "file" -force | Out-Null

# Write to log file
WriteToLog "I" "Install Citrix Workspace app" $LogFile
WriteToLog "I" "----------------------------" $LogFile
WriteToLog "-" "" $LogFile

############################
# Pre-Installation         #
############################

# Cleanup: delete existing group policy registry keys (reference: https://docs.citrix.com/en-us/citrix-workspace-app-for-windows/install.html#uninstall)
WriteToLog "I" "Cleanup: delete existing Citrix Workspace group policy registry keys" $LogFile
$x = 0
try {
    $RegKeyPath = "hklm:\SOFTWARE\Policies\Citrix\ICA Client"
    if ( Test-Path $RegKeyPath ) {
        $x++
        Remove-Item -Path $RegKeyPath -recurse
    }
    $RegKeyPath = "hklm:\SOFTWARE\Wow6432Node\Policies\Citrix\ICA Client"
    if ( Test-Path $RegKeyPath ) {
        $x++
        Remove-Item -Path $RegKeyPath -recurse
    }
    if ( $x -eq 0 ) {
        WriteToLog "I" "No existing group policy registry keys were found. Nothing to do." $LogFile
    } else {
        WriteToLog "S" "The group policy registry keys were deleted successfully" $LogFile
    }
} catch {
    WriteToLog "E" "An error occurred trying to delete the group policy registry keys (error: $($Error[0]))" $LogFile
    Exit 1
}

# Write an empty line to the log file
WriteToLog "-" "" $LogFile

# Cleanup: delete old Citrix Workspace app log folders in the TEMP directory
WriteToLog "I" "Cleanup: delete old Citrix Workspace app log folders" $LogFile
try {
    Get-ChildItem -path ( Join-Path $env:Temp "CTXReceiverInstallLogs*" ) -directory | Remove-Item -force -recurse
    WriteToLog "S" "The old log folders were deleted successfully (or they did not exist in the first place)" $LogFile
} catch {
    WriteToLog "E" "An error occurred trying to delete the old log folders (error: $($Error[0]))" $LogFile
    Exit 1
}

# Write an empty line to the log file
WriteToLog "-" "" $LogFile

############################
# Installation             #
############################

$InstallFile = Join-Path $StartDir $InstallFileName
WriteToLog "I" "Install Citrix Workspace app" $LogFile
WriteToLog "I" "Command: $InstallFile $InstallArguments" $LogFile
if ( Test-Path $InstallFile ) {
    $Process = Start-Process -FilePath $InstallFile -ArgumentList $InstallArguments -PassThru -ErrorAction Stop
    Wait-Process -InputObject $process
    switch ($Process.ExitCode) {
        0 { WriteToLog "S" "Citrix Workspace app was installed successfully (exit code: 0)" $LogFile }
        3 { WriteToLog "S" "Citrix Workspace app was installed successfully (exit code: 3)" $LogFile } # Some Citrix products exit with 3 instead of 0
        1603 {
            WriteToLog "E" "A fatal error occurred (exit code: 1603). Some applications throw this error when the software is already (correctly) installed! Please check the log files!" $LogFile
            Exit 1
            }
        1605 {
            WriteToLog "E" "Citrix Workspace app is not currently installed on this machine (exit code: 1605)" $LogFile
            Exit 1
            }
        1619 {
            WriteToLog "E" "The installation files cannot be found. The PS1 script should be in the root directory and all source files in the subdirectory 'Files' (exit code: 1619)" $LogFile
            Exit 1
            }
        3010 { WriteToLog "W" "A reboot is required (exit code: 3010)!" $LogFile }
        40008 {
            WriteToLog "I" "This version of Citrix Workspace app has already been installed. Nothing to do!" $LogFile
            # Re-enable File Security
            Remove-Item env:\SEE_MASK_NOZONECHECKS

            # Write an empty line to the log file
            WriteToLog "-" "" $LogFile
            WriteToLog "I" "End of script" $LogFile
            Exit 0
        }
        default {
            WriteToLog "E" "The installation ended in an error (exit code: $($Process.ExitCode))" $LogFile
            Exit 1
        }
    }
} else {
    WriteToLog "E" "The file '$InstallFile' could not be found" $LogFile
    Exit 1
}

# Write an empty line to the log file
WriteToLog "-" "" $LogFile

############################
# Post-Installation        #
############################

# Optional: import the Client Selective Trust registry keys and values. This prevents security popup messages regarding permissions for access to files, microphones, cameras, scanners, etc. in the local intranet and trusted sites.
# Reference: How to Configure Default Device Access Behavior of Receiver, XenDesktop and XenApp (https://support.citrix.com/article/CTX133565)
$RegFile = Join-Path $StartDir $ClientSelectiveTrustRegKeys
WriteToLog "I" "Optional: import the Client Selective Trust registry keys and values. This prevents security popup messages during logon" $LogFile
WriteToLog "I" "Import registry file '$RegFile'" $LogFile
if ( Test-Path $RegFile ) {
    try {
        $process = start-process -FilePath "reg.exe" -ArgumentList "IMPORT ""$RegFile""" -WindowStyle Hidden -Wait -PassThru
        if ( $process.ExitCode -eq 0 ) {
            WriteToLog "S" "The registry settings were imported successfully (exit code: $($process.ExitCode))" $LogFile
        } else {
            WriteToLog "E" "An error occurred trying to import registry settings (exit code: $($process.ExitCode))" $LogFile
            Exit 1
        }
    } catch {
        WriteToLog "E" "An error occurred trying to import the registry file '$RegFile' (error: $($Error[0]))!" $LogFile
        Exit 1
    }
} else {
    WriteToLog "I" "The file '$RegFile' could not be found. Nothing to do." $LogFile
}

# Write an empty line to the log file
WriteToLog "-" "" $LogFile

# Copy the Citrix Workspace app log files to the custom log path defined in the variable '$LogDir'
WriteToLog "I" "Copy the log files from the TEMP directory to '$LogDir'" $LogFile
$CitrixLogPath = (Get-ChildItem -directory -path $env:Temp -filter "CTXReceiverInstallLogs*").FullName
if ( Test-Path ( $CitrixLogPath + "\*.log" ) ) {
    $SourceFiles = Join-Path $CitrixLogPath "*.log"
    WriteToLog "I" "Source files          = $SourceFiles" $LogFile
    WriteToLog "I" "Destination directory = $LogDir" $LogFile
    try {
        Copy-Item $SourceFiles -Destination $LogDir -Force -Recurse
        WriteToLog "S" "The log files were copied successfully" $LogFile
    } catch {
        WriteToLog "E" "An error occurred trying to copy the log files" $LogFile
        Exit 1
    }
} else {
    WriteToLog "I" "There are no log files in the directory '$CitrixLogPath'. Nothing to copy." $LogFile
}

# Re-enable File Security
Remove-Item env:\SEE_MASK_NOZONECHECKS

# Write an empty line to the log file
WriteToLog "-" "" $LogFile
WriteToLog "I" "End of script" $LogFile
```

Save the script as a ***.ps1** file and execute the script as follows:

`powershell.exe -executionpolicy bypass -file "C:\Temp\Citrix Workspace app installation.ps1"`

Make sure to run the PowerShell script as an administrator. By default, the PowerShell script expects the installer (CitrixWorkspaceApp.exe), and optionally the registry file containing the *Client Selective Trust* registry keys and values, to be in the same directory as the script itself. Change the installation path and script name to your requirements.

Log files are written to `C:\Logs\Citrix Workspace app`, but this path can be changed by modifying the variables `$LogDir` and `$LogFile`. All log files generated by the Citrix Workspace app installer are copied to the log directory (defined in the variable `$LogDir`) after the installation has finished.

Also, the script removes any existing machine-specific group policy settings by deleting the following two registry keys:

-  `HKLM\SOFTWARE\Policies\Citrix\ICA Client`
-  `HKLM\SOFTWARE\Wow6432Node\Policies\Citrix\ICA Client`

The script also imports the *Client Selective Trust* registry file, but this is optional. If no **.reg* file is found the script will not end in an error.

You can execute scripts in an Active Directory Group Policy or using Electronic Software Distribution software (ESD), for example Microsoft SCCM.

## Adoption

After initial installation and configuration, one of the most critical deployment phases is end-users training and onboarding. Citrix has prepared a list of [Citrix Workspace end-user adoption resources](https://www.citrix.com/products/citrix-workspace/resources/user-adoption/) that includes everything you need to help your end users to start using Citrix Workspace. You can find everything including a sample deployment timeline, email templates, flyers and adoption kits in [tools section](https://www.citrix.com/products/citrix-workspace/resources/user-adoption/tools-for-successful-citrix-workspace-adoption.html).

## Uninstallation

Uninstalling Citrix Workspace app for Windows can be accomplished with the following command line: `CitrixWorkspaceApp.exe /silent /uninstall`

For more information see the [product documentation](/en-us/citrix-workspace-app-for-windows/install.html#uninstall)
