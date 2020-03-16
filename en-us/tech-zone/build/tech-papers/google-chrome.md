---
layout: doc
description: Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
---
# Running Google Chrome on Virtual Apps and Desktop

## Contributors

**Author:** [Dennis Span](https://twitter.com/dennisspan)

**Special thanks:** [Martin Zugec](https://twitter.com/MartinZugec)

## Overview

One of the most popular browsers today, Google Chrome, is a must-have for many Citrix Virtual Apps and Desktops environments. Google Chrome was primarily oriented at consumers and desktop operating systems when launched, but today, it is common in the Enterprise and more and more administrators are deploying this browser in their Virtual Apps and Desktops environments. This led Google to release a new Chrome Enterprise Bundle package in 2017 that is much friendlier to enterprise deployments than past iterations. To properly install and configure Google Chrome, there are some details you need to be aware of. This article will show you the recommended steps to a successful deployment, configuration, and optimization of Google Chrome in your organization.

## Installation

First, download the [latest version of Google Chrome](https://chrome.com/enterprise). You can choose to download either the Enterprise Bundle or the stand-alone version. The Enterprise Bundle includes the installers for the Chrome browser and the Chrome Legacy Browser, as well as the Microsoft Group Policy template (ADMX) files. Choose either the 32-bit or 64-bit version. Extract the ZIP file after download.

**NOTE:** The [Chrome Legacy Browser Support extension](https://support.google.com/chrome/a/answer/3019558?hl=en) allows users to switch automatically between Chrome and another browser. When a user clicks a link that requires a legacy browser to open (such as a site that requires ActiveX), the URL will automatically open in the legacy browser from Chrome.

To install Google Chrome on your master image, whether for hosted-shared or VDI, follow these steps:

-  Install Chrome using the MSI installer: `msiexec.exe /i "C:\GoogleChromeStandaloneEnterprise64.msi" /qn /norestart /l*v` `"C:\Logs\GoogleChromeStandaloneEnterprise64.log"` If you run into troubles when Chrome cannot reach the internet, add the MSI parameter `NOGOOGLEUPDATEPING=1`.
-  Optional: Install the Chrome Legacy Browser support extension using the MSI installer: `msiexec.exe /i "C:\LegacyBrowserSupport_4.7.0.0_en_x64.msi" /qn /norestart /l*v "C:\Logs\LegacyBrowserSupport_4.7.0.0_en_x64.log"`

We strongly recommend always using the latest version of Google Chrome. At a minimum, version 59 should be used, because from this version and higher, Chrome automatically detects if it’s running in a remote desktop environment and adjusts its settings accordingly. In addition, publishing Chrome in Citrix Studio is easier than it used to be; you only need to publish the following command line:

`C:\Program Files (x86)\Google\Chrome\Application\chrome.exe`

The installation directory is the same for both 32-bit and 64-bit installations.

It is no longer necessary to add the parameters `–allow-no-sandbox-job –disable-gpu` to the command line.

However, there still remains an issue in combination with the Citrix API hooks. Google Chrome may not launch properly and you may have to exclude the Chrome processes chrome.exe and nacl64.exe from these hooks. The Google article [Run Chrome as a virtual application](https://support.google.com/chrome/a/answer/7380899?vid=0-1331563052758-1499416367347) describes this issue in more detail. The Citrix article [How to Disable Citrix API Hooks on a Per-application](https://support.citrix.com/article/CTX107825) provides step-by-step instructions on how to disable the hooks for individual processes (applications). Just be aware that from XenApp and XenDesktop version 7.9 and newer, changes to API hooks configuration must be followed by a reboot.

The Citrix article [Chrome fails to launch in a published desktop](https://support.citrix.com/article/CTX226044) deals with Chrome errors such as “Aw, Snap!” page crashes and gray screens without any message. The solution for these errors is the same as mentioned above; the processes `chrome.exe` and `nacl64.exe` need to be excluded from the Citrix API hooks.

## Configuration

### Manage Google Chrome using Group Policies

Google Chrome can be managed using Microsoft Group Policy. As mentioned, the Enterprise Bundle includes the ADMX files. Copy the ADMX files and the language files (*.ADML) to your central store for Group Policy administrative templates (e.g. `\\contoso.com\SYSVOL\contoso.com\policies\PolicyDefinitions`). You find all Chrome related policies in the section Computer Configuration \ Policies \ Administrative Templates \ Google.

### Manage the master_preferences file

Google Chrome comes with a *master_preferences* file. This file contains the default Chrome settings. This file can be modified by the administrator to make sure settings are available after installation. By default, the master_preferences file is located in the directory `C:\Program Files (x86)\Google\Chrome\Application`.

Individual user settings are stored in a file called *Preferences*, stored in the user’s profile. This Preferences file is created on first use of Chrome. By default, this file is located in the directory `C:\Users\%UserName%\AppData\Local\Google\Chrome\User Data\Default`.

See the article [Configuring Other Preferences](https://www.chromium.org/administrators/configuring-other-preferences) for more details.

### Roaming User Settings

Google Chrome offers three ways how to roam user settings:

-  Google account
-  Chrome roaming profiles
-  Roaming profiles

#### Google Account (preferred method)

You can create a Google account and sign in with it in all your trusted environments and on all your trusted devices ([sign in or out of Chrome](https://support.google.com/chrome/answer/185277)).

This is the preferred method according to the article [Common Problems and Solutions](https://www.chromium.org/administrators/common-problems-and-solutions) (see the section Can I store my users’ Chrome profiles on a Roaming Profile?)

By default, the following user-specific settings are stored and synchronized:

-  Apps
-  Autofill
-  Bookmarks
-  Extensions
-  History
-  Passwords
-  Settings
-  Themes & Wallpapers
-  Open Tabs
-  Credit cards and addresses using Google Pay

The user can customize which settings are synchronized ([sync your account settings](https://support.google.com/chromebook/answer/2914794?hl=en)).

#### Chrome roaming profiles

If using a Google account to synchronize user settings is not an option for you, use Chrome roaming profiles instead. As explained in the article [Using Chrome on roaming user profiles](https://support.google.com/chrome/a/answer/7349337?hl=en), settings such as bookmarks, auto-fill data, passwords, per-computer browsing history, browser preferences and installed extensions can be stored in a file called profile.pb. By default, this file is stored in the directory `C:\Users\%UserName%\AppData\Roaming\Google\Chrome`, but the default directory can be changed.

All profile solutions, including the Citrix Profile Management, synchronize the directory `C:\Users\%UserName%\AppData\Roaming` (= `%AppData%`), thus ensuring that the file profile.pb is synchronized as well. There are three methods how you can enable the creation of the profile.pb file:

-  Enable the Group Policy setting Enable the creation of roaming copies for Google Chrome profile data in User or Computer Configuration \ Policies \ Administrative Templates \ Google \ Google Chrome.
-  Set the registry value RoamingProfileSupportEnabled to 00000001 in the registry key `HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome` or `HKEY_CURRENT_USER\Software\Policies\Google\Chrome` as described in the section **RoamingProfileSupportEnabled** in the article [Policy List](https://www.chromium.org/administrators/policy-list-3#RoamingProfileSupportEnabled) on Chromium.org.
-  Add the command line flag –enable-local-sync-backend to the Chrome.exe in the Chrome shortcut. See the article [Using Chrome on roaming user profiles](https://support.google.com/chrome/a/answer/7349337?hl=en) for more information.

There are three methods how to change the default directory of the profile.pb file:

-  Enable the Group Policy setting Set the roaming profile directory in User or Computer Configuration \ Policies \ Administrative Templates \ Google \ Google Chrome.
-  Add the profile directory to the registry value RoamingProfileLocation in the registry key `HKEY_LOCAL_MACHINE\Software\Policies\Google\Chrome` or `HKEY_CURRENT_USER\Software\Policies\Google\Chrome` as described in the section RoamingProfileLocation in the article [Policy List](https://www.chromium.org/administrators/policy-list-3#RoamingProfileLocation) on Chromium.org.
-  Add the command line flag `–local-sync-backend-dir=path_to_directory` to the Chrome.exe in the Chrome shortcut. See the article [Using Chrome on roaming user profiles](https://support.google.com/chrome/a/answer/7349337?hl=en) for more information.

#### Roaming profiles

So, what happens if the user is not signed in with a Google account or a Chrome roaming profile ("profile.pb") has not been configured? In cases such as these, Google Chrome stores all user data in the directory `C:\Users\%UserName%\**AppData\Local**\Google\Chrome\User Data` (see also the Chromium article [User Data Directory](https://chromium.googlesource.com/chromium/src/+/master/docs/user_data_dir.md)). This directory is synchronized by default by the Citrix Profile Management.

This method has its drawbacks and should be used carefully. As stated in the article [Common Problems and Solutions](https://www.chromium.org/administrators/common-problems-and-solutions), Chrome user profiles are not backward-compatible. If you try to use mismatched profiles and Chrome versions, you may experience crashes or data loss. This mismatch can often occur if a Chrome profile is synced to a roaming profile or network drive across multiple machines that have different versions of Chrome.

In short, it is important not to mix different versions of Chrome into a single roaming profile. If you want to use this method to synchronize your user’s settings, make sure to create separate roaming profiles for each environment or device type. This method may work for your organization, but you do so at your own risk.

Please note that [Citrix recommends](/en-us/profile-management/current-release/configure/include-and-exclude-items/defaults.html) excluding the following four subfolders:

-  `!ctx_localappdata!\Google\Chrome\User Data\Default\Cache=`
-  `!ctx_localappdata!\Google\Chrome\User Data\Default\Cached Theme Images=`
-  `!ctx_localappdata!\Google\Chrome\User Data\Default\JumpListIcons=`
-  `!ctx_localappdata!\Google\Chrome\User Data\Default\JumpListIconsOld=`

## Optimizations

### Disable Automatic Updates

On non-persistent machines, Chrome should not be allowed to update itself automatically. Installing updates should only be allowed when modifying or creating the master image or when updating the application layer (Citrix App Layering). To disable automatic updates, proceed as follows:

-  Disable the Group Policy setting Update policy override in the section Computer Configuration \ Policies \ Administrative Templates \ Google \ Google Update \ Applications \ Google Chrome on the Organization Unit in Active Directory that contains your productive workers.
-  Disable the following services and scheduled tasks (responsible for automatic updates):
-  Google Update Service (gupdate)
-  Google Update Service (gupdatem)
-  GoogleUpdateTaskMachineCore
-  GoogleUpdateTaskMachineUA

### Disable Active Setup

Chrome also creates an Active Setup item. As explained by Citrix CTP Helge Klein: **"Active Setup is a mechanism for executing commands once per user early during logon. Active Setup is used by some operating system components like Internet Explorer to set up an initial configuration for new users logging on for the first time."** Active Setup is executed at logon by the explorer.exe process, which means that it does not work with published applications. As a general rule, I recommend disabling Active Setup completely to improve user logon time. In case you would like to execute the Chrome Active Setup command once at user logon, I recommend using a logon script that automatically reads the command line from the stubpath registry value and executes the command.

### Remove the Chrome desktop icon

One last item you may want to configure is removing the automatically created desktop icon. This requires two steps:

-  Delete the shortcut file `Google Chrome.lnk`, located in the directory `%Public%\Desktop`, which by default points to `C:\Users\Public`.
-  Add the following lines to Chrome’s master_preferences file (explained previously in the Configuration section) to prevent shortcuts from being created for new users:
    -  `"create_all_shortcuts": false,`
    -  `"do_not_create_desktop_shortcut": true,`
    -  `"do_not_create_quick_launch_shortcut": true`

### Memory and CPU Optimization

Browsers can be quite RAM- and CPU-intensive and Chrome is no exception. On a native client, this might not be much of a problem, but it is in a Citrix Virtual Apps and Desktops environment where (in most cases) all workers are virtual machines, sharing the underlying hardware. Resource-intensive applications reduce the maximum user density per physical host.

**Did you know that Chrome comes with its own task manager that allows you to see the resource consumption for each individual tab?** To access the Chrome task manager, either use the shortcut SHIFT+ESC or go to the menu (the three vertical dots), and navigate to More tools \ Task manager. The task manager allows you to identify which web pages consume the most resources.

It is possible to reduce the memory and CPU utilization of Chrome:

-  First of all, use Citrix Workspace Environment Manager (WEM). The features [CPU Management](/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/cpu-management.html) and [Memory Management](/en-us/workspace-environment-management/current-release/user-interface-description/system-optimization/memory-management.html) reduce the memory and CPU utilization for many processes and applications, Chrome included.
-  Another way of reducing Chrome’s footprint is by using a Chrome extension, such as the [The Great Suspender](https://chrome.google.com/webstore/detail/the-great-suspender/klbibkeccnjlkjkiokjodocebajanakg?hl=en), to manage tabs to free up system resources. This extension suspends unused tabs, thus reducing memory and (especially!) CPU consumption. Each user can customize the extension (eg. which URLs never to suspend, the time-out after which tabs are suspended and more). Test this plugin and use the Chrome task manager to see how the resource consumption of each suspended tab reduces significantly.
-  In case your physical hosts come with a Graphics Processing Unit (GPU), certain processing tasks are offloaded to the GPU, thus freeing up the CPU. Citrix CTP Helge Klein wrote two great articles on this topic: [Impact of GPU Acceleration on Browser CPU Usage](https://helgeklein.com/blog/2014/12/impact-gpu-acceleration-browser-cpu-usage/) and [Comparison: CPU & GPU Usage of 4 Browsers](https://helgeklein.com/blog/2016/06/comparison-cpu-gpu-usage-4-browsers/).

### Manage Chrome extensions

When building your worker master image, whether hosted-shared or VDI, you may be inclined to include the required Google Chrome extensions. Don’t! Chrome extensions can be managed and deployed using Microsoft Group Policy. More importantly, Chrome extensions are installed on a per-user basis! You do not need to update your image to add or remove an extension. If you are adding a lot of extensions through policy, you should be aware that this can have a negative impact on the startup times of Google Chrome.

The installation directory is: `C:\Users\%UserName%\AppData\Local\Google\Chrome\User Data\Default\Extensions`

Using Microsoft Group Policy to manage your extensions also works when using Citrix Provisioning Server (PVS) to deploy your images, not just with Machine Creation Services (MCS).

For more detailed information on how to manage Chrome extensions using Microsoft Group Policy, please see the article [Deploying Google Chrome extensions using Group Policy](http://dennisspan.com/deploying-google-chrome-extensions-using-group-policy/).

## Other Tech Zone content

[Learn -> PoC Guides -> Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html) - Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and your on premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.

[Learn -> PoC Guides -> Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html) - Learn how to deliver the Citrix® HDX™ Optimization for Microsoft® Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops-service.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop deployment in Citrix Cloud.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops On-Prem](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop on-premises deployment.

[Learn -> Tech Insight -> App Layering - User Layers](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html) - User layers persist user profile settings, data, and user-installed applications in non-persistent VDI environments.

[Learn -> Tech Insight -> Remote PC Access](/en-us/tech-zone/learn/tech-insights/remote-pc-access.html) - Remote PC Access allows users to access their physical, office-based Windows PC from remote locations.

[Learn -> Tech Insight -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/tech-insights/virtual-apps-and-desktops-service.html) - Citrix Virtual Apps and Desktops Service provides a fast, low-impact deployment option for on-premises/cloud-hosted, Windows/Linux, desktops/apps.

[Learn -> Tech Insight -> Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) - Single Sign-on to Windows-based virtual apps and desktops when using a non-Active Directory based Citrix Workspace identity.

[Learn -> Tech Insight -> HDX - Display and Adaptive Technologies](/en-us/tech-zone/learn/tech-insights/hdx-adaptive-display.html) - Technologies that ensure an unparalleled user experience when connecting to remote Citrix resources.

[Design -> Reference Architectures -> Remote PC Access](/en-us/tech-zone/design/reference-architectures/remote-pc.html) - Discover the use cases and learn about the detailed architecture of Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.

[Design -> Reference Architectures -> Optimizing Unified Communications Solutions](/en-us/tech-zone/design/reference-architectures/optimizing-unified-communications-solutions.html) - Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Remote PC Access](/en-us/tech-zone/design/design-decisions/remote-pc-access.html) - When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.

[Design -> Design Decisions -> Single Server Scalability](/en-us/tech-zone/design/design-decisions/single-server-scalability.html) - Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Design Decisions -> Designing StoreFront and Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) - Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.

[Design -> Design Decisions -> VDI Model Comparison](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html) - Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.
