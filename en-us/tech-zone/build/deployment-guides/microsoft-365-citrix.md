---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz, Paul Wilson
specialThanksTo: Loay Shbeilat
description: Learn how to deploy Microsoft 365 in a Citrix Virtual Apps and Desktops environment.
tz_title: Microsoft 365 with Citrix Virtual Apps and Desktops
tz_products: citrix-virtual-apps-and-desktops;
---
# Deployment Guide: Microsoft 365 with Citrix Virtual Apps and Desktops

Microsoft 365, previously known as Office 365, is a bundled software plus subscription-based offering focused on user productivity-based applications. Microsoft 365 includes a combination of online based applications that are accessed from anywhere via a web browser, in addition to the latest traditional, locally installed version of Microsoft Office. Included with Microsoft 365 is an online email account that has 50 GB of mail storage and 1 TB of file storage per user licensed for OneDrive for Business. Microsoft 365 Business comes in three different editions: Basic, Standard, and Premium. You can find more information on what is included in each edition [here](https://www.microsoft.com/en-us/microsoft-365/business/compare-all-microsoft-365-business-products).

Microsoft 365 is a great solution for any organization. But due to user, application and business requirements, there is often a requirement for a locally installed version of the Office applications in addition to the online versions. Typically, organizations require the locally installed versions for the following reasons:

-  Require full application functionality that may not be available with the online version
-  Line-of-business applications installed locally have a dependency on locally installed versions of an Office application

These challenges are relevant for most organizations.

Historically, Microsoft Office is one of the most common applications delivered via Citrix Virtual Apps and Desktops. This is due to its ability to provide the user with the latest version of Office with the best user experience for a wide range of use cases. With Microsoft 365, the value of Citrix Virtual Apps and Desktops has not changed. To deliver Microsoft 365 to users properly, we provide the following recommendations to enable an optimized user experience while minimizing the potential impact to the underlying infrastructure.

## Outlook

As part of a Microsoft 365 implementation with Citrix Virtual Apps and Desktops, organizations can use Exchange Online instead of managing and maintaining Exchange servers installed on-premises. As part of an Exchange Online implementation, the deployment of the Outlook client requires a choice between two options: Cached Exchange Mode or Online Mode. The decision impacts the user experience and infrastructure. Following are the differences described between the two. More information can be found [here](https://docs.microsoft.com/en-us/outlook/troubleshoot/deployment/cached-exchange-mode).

-  **Cached Exchange Mode**:
    -  Default setting for the user
    -  Continuously syncs the user mailbox and address book to a local file, eliminating service disruptions caused by sporadic or latent network connections. Cached mailbox content is stored locally for mail received within a configured window of time and reverts to Online mode for older content
    -  Best suited for users who frequently move in and out of connectivity, users who work offline or without connectivity, users who have high-latency connections (greater than 500 ms) to the Exchange Server
-  **Online Mode**:
    -  Requires constant networking connection to the back-end Exchange server
    -  Mailbox data is only cached in memory and never written to disk
    -  Best suited for kiosk scenarios (where a computer has access to many Outlook accounts and the delay to download the local cache is not acceptable), heavily regulated compliance, or secure environments where data cannot be stored locally, large mailboxes on computers that don’t have sufficient hard disk space for a local copy of the mailbox

When you use Cached Exchange Mode, there is always a copy of a user’s Exchange mailbox in an offline folder file (*.OST). We recommend Cached Exchanged Mode, so the user has a consistent online and offline experience. More information on Cached Exchanged Mode can be found [here](https://docs.microsoft.com/en-us/outlook/troubleshoot/deployment/cached-exchange-mode).

### Active Directory Group Policy

The following policies are recommended for Active Directory:

-  **Cached Exchange Mode (file)**: Included in the Outlook Active Directory group policy template. This policy specifies the default Cached Exchange Mode for new profiles. The options are Download Headers, Download Full Items, and Download Headers and then Full Items. For our tests, we used Download Full Items.
-  **Sync Settings**: Included in the Outlook Active Directory group policy template. This policy allows an administrator to configure the amount (by date) of user email Outlook synchronizes locally using Cached Exchange Mode. Initially, these policies can be set to one month, although depending on your specific implementation a longer amount of time can be required for your use case.
-  **Disable Fast Access**: Included in the Outlook Active Directory group policy template. When Exchange Fast Access is enabled, Outlook 2016 connects to Exchange in Online Mode while simultaneously building an offline cache file as part of the Cached Exchange Mode. As the latency increases between Outlook and Exchange, Outlook seamlessly utilizes the local cache file. Note: By default, the Disable Exchange Fast Access policy is disabled, which means Exchange Fast Access is enabled by default. Our guidance is to ensure that this policy is disabled.
-  **Use Cached Exchange Mode**: Included in the Outlook 2016 Active Directory group policy template. This policy enables Cached Exchange Mode for new and existing Outlook profiles. Without this policy enabled, Outlook is configured in Online Mode. This policy is set to Enabled.
-  **Cache File**: According to this Microsoft knowledgebase article, the cache file can be located on a network drive if the following three criteria are met:
    -  A high bandwidth/low latency network connection is used.
    -  There is single client access per file (one Outlook client per .PST or .OST).
    -  Either Windows Server 2008 R2 or later Remote Desktop Session Host (for example: Citrix Virtual Apps), or Windows 7 or later virtual desktop infrastructure (for example: Citrix Virtual Apps and Desktop) is used to run Outlook remotely.

## Azure Files

Azure Files is a secure, publicly hosted Server Message Block (SMB) or NFS file share with low latency access. Azure Files supports both Active Directory integration and NTFS file-level permissions and is accessible from Windows, Mac and Linux clients.

Azure Files can be used for various enterprise purposes, including:

-  File Servers. Azure Files can directly be mounted from all popular operating systems across the world. Azure Files also can be deployed in a hybrid scenario, where data can be replicated to on-premises Windows servers via Azure File Sync.
-  Containerization. Use Azure Files for persistent storage of containers or as a shared storage file system.

With a shared access capable, fully managed, resilient file share, Azure Files is an excellent place to store user profile data for your Citrix users. Azure Files uses SMB 3.0 so all the data transferred is protected by encryption while in-transit.

For the testing in this article, the **Premium** performance tier was used with a 10 GiB share hosting the containers. A premium file share is provided a base ingress of 40MiB/sec + (0.04 x provisioned GiB) while the base egress rate is 60MiB/sec + (0.06 x provisioned GiB). For a 10 TiB share, this equates to an ingress rate of 450MiB/sec and an egress rate of 675MiB/sec. The premium performance level is designed to support I/O intensive workloads while delivering latency of under 10 milliseconds.

## Citrix Policy Management

Configure Profile Management through Citrix Studio. It can also be configured through Microsoft Group Policy or WEM. Choose one place for which it is configured—and not configure it in multiple places. If you chose Microsoft Group Policy, make sure that you periodically update the ADMX template to make sure you have access to the latest features.

Citrix Profile Management is designed to remove profile bloat and significantly speed logon times, while reducing profile corruption. Citrix Profile Management is a feature of Citrix Virtual Apps and Desktops Service.

The following Citrix policies are recommended (see image following):

-  **Logon Performance**: The user profile might become large due to the Outlook cache file, it is important to mitigate this risk by implementing the Citrix Profile Management functionality. The following settings are recommended:
    -  **Enable Profile Management**: Policy must be enabled so Citrix profiles are used
    -  **Path to user store**: Policy must specify the unique path for the user profile location
    -  **Profile Streaming**: Files and folders contained in a profile are fetched from the user store to the local computer. They are fetched only when they are accessed by users after they have logged on. Profile streaming significantly reduces the logon time by reducing the amount of data that is copied down to the local profile at user logon. This setting is enabled by default and decreases the amount of transactions and egress data leaving Azure Files, reducing costs, and improving logon time.
    -  **Active Write-back**: Files and folders that are modified can be synchronized to the user store in the middle of a session, before logoff. Usually, these mid-session writes occur about every 5 minutes. Enabling this setting provides Azure Files with a more consistent stream of write traffic, instead of having it all at logoff. This frequent write-back resolves the “last writer wins” issue when users are accessing their profile from multiple devices simultaneously.
    -  **Large File Handling**: This policy improves logon performance and to process large-size files, a symbolic link is created instead of copying files in this list. You have to configure the paths where the files are stored but you can use wildcards.
    Enabling the Large File Handling feature of Citrix Profile Manager is a fantastic way to improve the logon experience for large files that would normally be copied down from the profile store at logon. Since Microsoft does not recommend storing PST and OST files in a redirected folder, you can use large file handling. Large file handling provides the same benefits as folder redirection, but on a per file basis. Instead of redirecting the folder or file, Citrix Profile Manager (CPM) creates a symbolic link to the file. When the file is opened or updated, the file operations are redirected to the user store in Azure Files. This feature allows PST files and OST files to remain in the user’s profile and be treated as local files, even though they reside on a remote file share.
    This policy is not recommended when using FSLogix containers.

[![Policies1](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image1.png)](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image1.png)

Together, these configuration settings help to ensure a better user experience for Outlook on Citrix Virtual Apps with Microsoft 365 Exchange Online.

The later versions of CPM enable profile streaming by default on Citrix servers. This technology prevents the entire user profile from being downloaded to the Citrix host at logon. Instead, only a list of the files residing on the profile are enumerated and that list is available to the operating system. When a file is requested, then it is fetched from the user store and brought to the Citrix host. This process reduces the number of file copies that happen at logon and improves the user logon experience.

When roaming profiles is turned on, %UserProfile%\LocalSettings does not roam with users. This behavior affects Outlook users because some Outlook data (.OST, .PST, and .PAB files) is created in non-roaming folders. This is due to these files being large and hindering roaming profile performance.
Follow the guidelines below to reduce troubleshooting:

-  Use an ADM template for Microsoft Office that prohibits the use of .PST files
-  When users run out of space, increase the storage on the Microsoft Exchange servers rather than the network share
-  Define and enforce an email retention policy for the entire company, rather than granting exceptions for their .PST files.
-  If .PST files cannot be prohibited, do not configure Profile Management or roaming profiles on your Exchange servers.

When using Cached Exchange Mode, the .OST file can grow large. Use the Enable native Outlook search experience to feature instead. With this feature, the Outlook offline folder file (*.OST) and the Microsoft search database specific to the user roam along with the user profile. This feature improves the user experience when searching mail in Microsoft Outlook. A step by step guide on how to turn on the Outlook search experience can be found [here](/en-us/profile-management/current-release/configure/enable-native-outlook-search-experience.html). Make sure to consider this when sizing the user store.

To increase the stability of the Enable search index roaming for Outlook feature, Profile Management saves a backup of the last known good copy of the search index database in case it becomes corrupted. Citrix recommends the following two policies (see image below) to be configured within Citrix Studio:

-  **Enable search index roaming for Outlook**: Policy must be **enabled**. This allows user-based Outlook search experience by automatically roaming Outlook search data along with the user profile. Note: this requires more space in the user store to store the search index for Outlook. This policy is disabled by default.
-  **Outlook search index database—backup and restore**: This policy must be enabled. If this setting is enabled, Profile Management saves a backup of the search index database each time the database is mounted successfully on logon. Profile Management treats the backup as the good copy of the search index database. When an attempt to mount the search index database fails because the database becomes corrupted, Profile Management automatically reverts the search index database to the last known good copy.

[![Policies2](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image2.png)](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image2.png)

Appendix A walks through profile configuration testing.

## App Layer User Layer

User layers allow profile settings, data, and locally installed applications in a non-persistent VDI environment to persist. Every user gets an assigned a user layer at logon where their changes are written. This is created the first time the logon and gets mounted to their session using Citrix App Layering Elastic layering technology. The following user layers are available:

-  **Full:** All the user’s data, settings, and locally installed applications are stored
-  **Office 365:** This is for desktop operating systems. Only the user’s Outlook data and data are stored
-  **Session Office 365:** This is for server operating systems. Only the user’s Outlook data and data are stored

User personalization layers offer user-based customizations in non-persistent virtual environments. User layers provide users with an experience that mimics that of a dedicated desktop while offering the management and cost savings of a non-persistent Windows image.

Make sure that there is enough bandwidth and enough storage space allocated for the user layer. More information on requirements and how to configure user layers can be found [here](/en-us/citrix-app-layering/4/layer/enable-user-layers.html).

Information on App Layering with FSLogix can be found [here](https://www.citrix.com/blogs/2020/01/07/citrix-app-layering-and-fslogix-profile-containers/)

## Skype for Business

When deploying Skype for Business in a Virtual Apps and Desktop environment, Citrix recommends utilizing the HDX RealTime Optimization Pack. This is the only Microsoft-endorsed solution for delivering Skype for Business in a virtual environment. The HDX RealTime Optimization Pack utilizes your existing Skype for Business infrastructure and interoperates with other Microsoft Skype for Business endpoints running natively on your devices. Essentially, the HDX RealTime Optimization Pack, maximizes server scalability by redirecting media processing to the end-user device.

Comparison feature chart of the different Optimization Packs.

|     Feature                                                                 |     Generic HDX RealTime    |     HDX RealTime Optimization Pack 2.9 LTSR    |     HDX RealTime Optimization Pack 2.8    |     HDX RealTime Optimization 2.4 LTSR    |     Native Skype for Business    |
|-----------------------------------------------------------------------------|-----------------------------|------------------------------------------------|-------------------------------------------|-------------------------------------------|----------------------------------|
|     Support for  Skype   for Business server 2019                           |     ✓                       |     ✓                                          |     ✓                                     |     x                                     |     ✓                            |
|     Support for  Skype   for Business server 2015                           |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Support for  Skype   for Business online                                |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Microsoft Lync 2013 server support                                      |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Webcam support                                                          |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     HD video                                                                |                             |     ✓                                          |     ✓                                     |     ✓                                     |                                  |
|     Fall back to server if no local media engine                             |     n/a                     |     ✓                                          |     ✓                                     |     ✓                                     |     n/a                          |
|     Screen sharing (full desktop mode)                                      |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Application for other hosted apps                                       |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Support for 32-bit client                                               |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Support for 64-bit client                                               |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Instant messaging                                                       |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     Support for Skype for Business call when the Edge is not   reachable    |     n/a                     |     ✓                                          |     ✓                                     |     x                                     |     ✓                            |
|     Gallery View                                                            |                             |                                                |                                           |                                           |     ✓                            |
|     Location Services (for emergencies)                                     |     x                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |
|     H.264 encoder                                                           |     ✓                       |     ✓                                          |     ✓                                     |     ✓                                     |     ✓                            |

Following are a couple of considerations to think about when delivering Skype for Business in a virtualized environment:

-  The inclusion of hardware acceleration for video increases the amount of data being sent if you deploy devices that support hardware acceleration for video. Ensure that sufficient bandwidth is available to all your endpoints or update your Skype for Business server media bandwidth policies.
-  Make sure your Virtual Delivery Agent (VDA) has a minimum of two CPUs for those users that need Fallback mode.
-  Ensure your users have a supported version of the Optimization Pack.
-  HDX RealTime Optimization Pack is only supported on server and client operating system versions that are supported by their manufacturer and only with Skype for Business versions that are still supported by Microsoft under mainstream or extended support.
-  HDX RealTime Optimization Pack 2.9 LTSR is the last version of the RealTime Optimization Pack released and is supported until October 2025.

More information on the HDX RealTime Optimization Pack can be found [here](/en-us/hdx-optimization/2-9-ltsr/).

## Microsoft Teams

Citrix delivers optimization for desktop-based Microsoft Teams using Citrix Virtual Apps and Desktops and Citrix Workspace app. By default, we bundle all the necessary components into the Citrix Workspace app and the Virtual Delivery Agent (VDA).

Our optimization for Microsoft Teams allows the endpoint to decode and render the media locally. It does so by opening a control virtual channel (CTXMTOP) to the Citrix Workspace app-side media engine. Currently, only client-fetch/client-render is available.
Following are a couple of recommendations and things to consider when deploying Microsoft Teams in a virtual environment:

-  The VDA must be installed before Teams in the golden image. If the golden image already had Teams installed before the VDA is installed, uninstall it and reinstall once the VDA has been installed. Installation guidelines by Microsoft can be found here.
-  The ALLUSER=1 mode is recommended for non-persistent environments. This ensures that the application doesn’t auto-update when there is a new version available.
-  We recommend disabling the auto-start of Microsoft Teams to prevent VM CPUs to spike.
-  If the virtual desktop doesn’t have GPU/vGPU available, we recommend the setting Disable GPU hardware acceleration in the Teams setting.
-  Ensure that the Microsoft Teams Redirection policy is turned on in Studio. This is turned on by default.
-  Use the Skype for Business Network Assessment Tool to check if your network is ready for Microsoft Teams
-  When using Teams in a non-persistent environment, a profile caching manager is recommended to ensure that appropriate user-specific information is cached during the user session.
-  Although Skype for Business and Teams can be deployed side by side, peripheral access can only be granted to a single application at any given time.

If Microsoft Teams fails to load in an optimized VDI mode, the VDA utilizes legacy HDX technologies like webcam, client audio, and microphone redirection.

More information on system requirements, on recommended exclusions, network requirements, and process flow information can be found [here](/en-us/citrix-virtual-apps-desktops/multimedia/opt-ms-teams.html).

## OneDrive for Business

Included with the Microsoft 365 subscription is access to OneDrive for Business, allowing a user to store, sync, and share their work files. OneDrive for Business lets users update and share files from anywhere and work on Office documents with others at the same time. In environments that use RDS/VDI type implementations like Citrix Virtual Apps and Desktop simply installing the OneDrive for Business agent can cause some unexpected challenges.

-  **Consumer vs Business**: There are two flavors of OneDrive: OneDrive and OneDrive for Business. Both solutions are different. OneDrive uses a personal account for user file storage in the cloud. OneDrive for Business uses a business account with a SharePoint back-end infrastructure, allowing for joint collaboration and greater administration capabilities. OneDrive for Business can be hosted in the cloud or on-premises, while OneDrive is entirely hosted in the cloud.
-  **Sync**: The synchronization tool, included with OneDrive for Business, syncs the user’s entire library to a local, non-network folder. Performing this action on a Citrix Virtual Apps environment or non- persistent VDI machine results in significant amount of data being copied during each logon. The large amount of data copied causes poor user experience. Microsoft currently only supports the sync app in the following virtual environment scenarios:
    -  Virtual desktops that persist between sessions
    -  Non-persistent virtual desktops that use Azure Virtual Desktop
    -  Non-persistent virtual desktops that have FSLogix Apps 2.8 or later, FSLogix Office Container, and a Microsoft 365 subscription
-  **Storage Space**: Each OneDrive for Business user is granted 1 TB of storage space for their personal library. Synchronizing the user’s entire library across multiple devices consumes a significant amount of storage.  With FSLogix, it will not have to be synchronized across multiple devices, the VHD gets sync’d once and follows the user.
-  **Network Sync**: OneDrive for Business does not support syncing to a network drive.

To support non-persistent virtual desktops that are using FSLogix Office Containers or CPM Profile Containers, the installation must meet the following requirements per [Microsoft](https://docs.microsoft.com/en-us/onedrive/sync-vdi-support):

-  One of the following Windows operating systems:
    -  Windows 7 or Windows 10 (32-bit or 64-bit)
    -  Windows Server 2008 R2, 2012 R2, 2016 or 2019
-  OneDrive Sync app version 19.174.0902.0013 or [later](https://docs.microsoft.com/en-us/onedrive/per-machine-installation). This build includes support for the per-machine installation that provides a single installation for all users on the machine instead of each user needing to install and configure OneDrive Sync. For this article, the version used was 20.201.1005.0009.
-  Windows Servers requires the SMB network file sharing protocol enabled
-  OneDrive Sync app does not support multiple instances of the same container running simultaneously

OneDrive installation must be done as a per-machine install for multi-session VDAs. To install OneDrive in per-machine mode, use the following command-line:

OneDriveSetup.exe /allusers

By default, the OneDrive Sync application is configured to automatically update itself. To allow this default behavior, keep the URLs “oneclient.sfx.ms” and “g.live.com” accessible through corporate firewalls. To prevent the OneDrive Sync application from updating automatically, complete the following tasks:

-  Disable the “OneDrive Updater Service” in the Services control panel
-  Disable the “OneDrive Per-Machine Standalone Update Task” in Scheduled Tasks

### OneDrive with CPM Profile Container on Azure Files

When using OneDrive with the CPM Profile Container, include the following path in the Profile container settings/Profile container setting:

AppData\Local\Microsoft\OneDrive

## Licensing

Typically, with Microsoft 365, users are allowed to download the applications on up to 5 devices. In a Virtual Apps and Desktop deployment, this would not work as users are connecting to different back-end VMs every time they logon. For virtual environments, **shared computer activation** needs to be enabled. When using the Shared Computer Activation approach, the following occurs:

1.  User logs on to a machine and starts a Microsoft 365 application (Microsoft Word)
2.  Microsoft 365 contacts the Office Licensing Service via the internet to obtain a license token for the user-machine combination. If the environment is configured correctly, the user does not see an activation wizard.
3.  When properly licensed, the license token is stored in the user profile.
4.  The steps are repeated for each user-machine combination. If the same user logs on to another machine, they must activate Microsoft 365 on that machine, too.
5.  If the user logs on to a shared machine where they have already gone through an activation process, the token, stored in the user profile, is reused.

However, the Shared Computer Activation does have a couple of caveats:

-  Licensing token renewal: The licensing token stored on the shared computer is only valid for 30 days. Microsoft 365 will automatically renew the token before it expires if the user logs on to that specific machine a couple of days before the token expiration. However, if the user doesn’t log on to that shared computer for 30 days and the licensing token expires, Microsoft 365 Apps will contact the Office Licensing Service online the next time the user tries to access the apps.
-  Internet Connectivity: During the license renewal, there must be an internet connection to reach the Office Licensing Service.
-  Reduced Functionality Mode: If the Activation Office dialog box is closed, if there is no internet connectivity, or if the user is not licensed for Microsoft 365 Apps, then no licensing token is obtained. This results in Microsoft 365 to launch in reduced functionality mode. Users will only be able to see and print documents, but they will not be able to edit or create documents.
-  Activation limits: When shared activation mode is enabled, users don’t get the 5 device limit. Microsoft allows a single user to active Microsoft 365 Apps on reasonable number of shared computers in a given time period.
-  Single Sign on: The use of SSO is recommended to reduce how often the users are prompted to sign in for activation. More information can be found [here](https://docs.microsoft.com/en-us/microsoft-365/enterprise/about-microsoft-365-identity?view=o365-worldwide)
-  Licensing token roaming: Starting with Version 1704 of Microsoft 365 Apps, you can configure the licensing token to roam with the user's profile or be located on a shared folder on the network. Previously, the licensing token was always saved to a specific folder on the local computer and was associated with that specific computer. In those cases, if the user signed into a different computer, the user would be prompted to activate Microsoft 365 Apps on that computer to get a new licensing token. The ability to roam the licensing token is especially helpful for non-persistent virtual environment scenarios. This can be done through Group Policy, Registry Editor, or the Office Deployment Tool.
-  Licensing token roaming: Starting with Version 1704 of Microsoft 365 Apps, you can configure the licensing token to roam with the user's profile or be located on a shared folder on the network. Previously, the licensing token was always saved to a specific folder on the local computer and was associated with that specific computer. In those cases, if the user signed into a different computer, the user would be prompted to activate Microsoft 365 Apps on that computer to get a new licensing token. The ability to roam the licensing token is especially helpful for non-persistent virtual environment scenarios. This can be done through Group Policy, Registry Editor, or the Office Deployment Tool.
    -  If you're using Group Policy, download the most current Administrative Template files (ADMX/ADML) for Office and enable the "Specify the location to save the licensing token used by shared computer activation" policy setting. This policy setting is found under Computer Configuration\Policies\Administrative Templates\Microsoft Office 2016 (Machine)\Licensing Settings.
[![GPO](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image3.png)](/en-us/tech-zone/build/media/deployment-guides_microsoft-365-citrix_image3.png)

    -  If you're using the Office Deployment Tool, include the SCLCacheOverride and SCLCacheOverrideDirectory in the Property element of your configuration.xml file.
    -  To edit the registry, go to HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\ClickToRun\Configuration, add a string value of SCLCacheOverride, and set the value to 1. Also, add a string value of SCLCacheOverrideDirectory and set the value to the path of the folder to save the licensing token.

    Microsoft 365 Business premium is the only business plan that includes support for shared computer activation. More information on Microsoft 365 with Shared Computer Activation can be found [here](https://docs.microsoft.com/en-us/deployoffice/overview-shared-computer-activation?redirectSourcePath=%252fen-us%252farticle%252f836f882c-8ff6-4f19-8b24-0212e0111c94).

## Microsoft 365 Acceleration

Citrix SD-WAN directs Microsoft 365 traffic from Enterprise branch offices directly to the nearest Internet POP providing optimal user experience.

It does this by automating the implementation of [Microsoft Office 365 Network Connectivity Principles](https://docs.microsoft.com/en-us/microsoft-365/enterprise/microsoft-365-network-connectivity-principles?view=o365-worldwide#bkmk_principles). These principles provide a combined set of information to optimize network routes, firewall rules, browser proxy settings, and bypass of network inspection devices for certain endpoints.

With this information deployed to a deep packet inspection engine, Citrix SD-WAN appliances automatically identify M365 application flows, resolve the nearest service POP, and direct endpoint sessions to it. This ensures minimal latency and maximum user experience for the M365 application.

For more information see [M365 Optimization](/en-us/citrix-sd-wan/11-3/office-365-optimization.html)

Also to gain more insight about the Citrix SD-WAN implementation see [M365 Optimization for Branch Offices](/en-us/tech-zone/learn/tech-insights/office365-optimization.html)

## Appendix A: Profile Configuration Testing

In addition to the traditional Citrix Profile Manager deployment, Citrix Virtual Apps and Desktops supports two options for profile containers when used with M365 on non-persistent, multi-session hosts: Citrix Profile Manager (CPM) or FSLogix. For more information on the lab configurations in the test environment, see the Appendix.

The Citrix lab reviewed the common scenarios for using M365 with Azure Files:

-  Citrix Profile Manager with Large File Handling Enabled: Folder redirection used for all folders not stored in the CPM Profile and the user’s OST/PST files stored in the CPM Profile
-  FSLogix Office Containers and Profile Containers: No folder redirection configured, all user profile folders, and OST/PST files stored in the FSLogix containers
-  FSLogix Office & Profile Containers enabled and Citrix Profile Manager with Large file handling disabled: No folder redirection configured, all user profile folders, and OST/PST files stored in the FSLogix container and CPM provides profile size management

In all the scenarios tested, Microsoft Outlook was set to Cached Exchange Mode. All users tested had an OST file that was at least 4 GiB. One of the scenarios tested included using the search function within Outlook, since the search index is user specific and needs to roam with the user. With Microsoft 365, the search functionality is quick and roams with the user between workstations because it is stored online with the user’s mailbox. When Outlook is used and the outbound internet is not available, due to proxy or configuration errors, the search cache is no longer accessible. When the Outlook client is working offline, any search requests are slow since Outlook needs to build the index database. The Outlook search index then is stored in the Windows Search database found under ProgramData.

As mentioned earlier all user data storage for the non-persistent sessions was stored on a single Azure Files 10 TiB premium file share. Metrics were manually recorded for the Logon and Log off times. The results of the testing are shown in the table below:

|     Profile Configuration                                                                                 |     Logon Time       |     Logoff Time      |
|-----------------------------------------------------------------------------------------------------------|----------------------|----------------------|
|     FSLogix Office & Profile Containers                                                                   |     22–42 seconds    |     10–16 seconds    |
|     Citrix Profile Manager with   Folder Redirection and Large File Handling Enabled                      |     36–55 seconds    |     1–5 minutes      |
|     FSLogix Office & Profile Containers with Citrix   Profile Manager and Large File Handling Disabled    |     42–59 seconds    |     18–42 seconds    |

Although the FSLogix Office & Profile Containers provided the quickest logon, they are not always the best solution. Each of these profile configurations comes with caveats to consider before deploying and are discussed below.

### FSLogix Containers

FSLogix supports two types of containers: office and profile. Each container can be enabled and configured separately and servers a different purpose. The office profile container virtualizes office settings and files, including Outlooks OST/PST files. The profile container virtualizes the rest of the user profile, including all data stored under the user’s profile folder, such as documents, application data, and browser history. In our lab configuration, the VHD files were stored on the Azure Files Premium file share. The container VHDs took only a few seconds to attach during logon. When only the office container was enabled, we saw logons averaging between 12 and 14 seconds. With both containers, the average logon times jumped to between 23 and 24 seconds.

The office and profile containers are stored in separate VHD files. In our testing, the users had a 4.7 GiB profile. After logon and logoff testing was complete, the office container had grown to 6.9 GiB and the profile container to 6.5 GiB. Which means for 4.7 GiB worth of user data, we were using 13.4 GiB of storage space for FSLogix, that is almost a 300% increase in space used.

Microsoft recommends he FSLogix container for storing Office related data, including the Outlook OST files and the Windows search database. The profile container is more of an optional container. One useful feature is that FSLogix can parse out a single-user’s search information from the multi-user database stored in C:\ProgramData\Microsoft\Search. So on multi-session hosts, the search index database can follow the user.

### CPM Profiles with Large File Handling

With this scenario, the Citrix VDA install included the Profile Management feature. The large file handing was enabled only for OST/PST files. For more information on how Citrix Profile Manager was configured in the lab test environment, see the Appendix B.

During the testing, the logon times with CPM profiles using a combination of folder redirection and Profile Management were fairly normal and provided a good user experience. The use of large file handing provides a great user logon experience because the OST/PST files do not need to be downloaded to the local workstation, saving time at logon and space on the local drive. When CPM is used, users with multiple simultaneous sessions, such running application sessions across different hosts, will have their changes saved to the pending folder and not written to the profile until logoff, protecting against the last-write wins data loss scenario.

The longer logoff times are attributed to CPM’s multi-session support where writes are placed into a pending folder during the session and then written to the Azure File share at logoff. The one downside to this behavior is that if users log off and then attempt to log on again quickly, they receive a temporary profile. This behavior is because CPM cannot get a lock on their profile folder because the pending folder data is still being written to the user’s profile. So the service eventually times out and opts for a temporary profile.

### CPM Profiles with FSLogix Containers

This profile configuration represents a hybrid approach with the two technologies. The faster logon of the FSLogix containers combined with CPM’s granular control of the profile contents. With this solution, you can decide to deploy both FSLogix’s containers, office and profile, or just the office container and use CPM for the Profile Management. However, when deploying traditional CPM profiles with FSLogix containers, do not enable the large file handling feature. With FSLogix containers, the entire container is redirected, so redirecting individual files does not provide any benefit.

One of the primary downsides to using container files is the additional management overhead for user VHDs. VHDs bloat over time, as files are deleted from the user profile, they are removed from the VHD, but the physical space is not reclaimed. This behavior causes an extra administrative burden as someone is required to go through and shrink the VHDs periodically. By using CPM Profile Management with FSLogix containers, the amount of space consumed by the VHD can be reduced and the growth rate of the file slowed.

The additional storage management benefits though increase the logon and logoff times by about 20 seconds. Carefully consider the overall impact to the user’s experience when deciding to deployment in this hybrid configuration.

### Key Takeaways

This section provides key takeaways from the testing and guidance on applying them to common scenarios:

-  FSLogix integration with Windows provides a faster logon time for end-users
-  FSLogix is flexible and can be configured with or without CPM.
-  OneDrive can be used for managing user profiles, but both CPM and FSLogix provide a better logon experience.
-  CPM with large file handling supports concurrent sessions on multiple hosts.
-  OneDrive does not support multiple simultaneous connections / multiple concurrent connections, using the same profile, under any circumstances. More information can be found [here](https://docs.microsoft.com/en-us/fslogix/configure-concurrent-multiple-connections-ht).
-  FSLogix does not support multiple simultaneous connections for the same user from different sessions
-  Selecting CPM with FSLogix containers reduces the VHD bloat
-  Choose only one profile container solution for the VDI to avoid any logon or profile conflicts.

### Use Cases

This section includes recommendations for common scenarios when hosting users within Azure.

#### Multiple Simultaneous Sessions per user

When users have multiple simultaneous windows sessions, such as when using Citrix Virtual Applications, Citrix Profile Manager with Large File Handling and folder redirection is recommended. Neither OneDrive nor FSLogix support this configuration and data loss was experienced in the lab during the testing with these solutions. Citrix Profile Manager provides a great balance between user logon performance and efficient use of the storage and scales well. OneDrive or Citrix ShareFile can be leveraged to store personal or department data that is not stored in user profile folders managed by CPM.

#### Multi-session, Non-persistent Hosts without Simultaneous Sessions

When multi-session hosts are used and users do not have the possibility of simultaneous sessions, the best user experience is gained by using either FSLogix containers or FSLogix containers with CPM enabled.

#### Single-session, Non-persistent Hosts

With single-session VDI hosts for the end-users, the best user-experience is with straight FSLogix containers and either OneDrive or Citrix ShareFile support for department data.

## Appendix B: Configuration and Setup Details

This appendix contains information on the setup and configuration of the technologies configured in the lab environment. You can use this as a point of reference that you can use in your environment.  The following software versions were used in the lab:

|     Software Package            |     Version                              |
|---------------------------------|------------------------------------------|
|     Citrix VDA 2012             |     Version 2012.0.0.28051               |
|     OneDrive Sync App           |     Version 19.174.0902.0013             |
|     Windows 10 Multi-session    |     Version 20H2 (OS Build 19042.746)    |
|     FSLogix                     |     Version 2.9.7654.46150               |

Citrix Profile Manager Configuration on Azure Files with Large-File Handling
The following Citrix policies were configured when using the CPM Profiles with Large File Handling for this scenario:

|     GPO Setting                                                        |     Value                                                                                                          |
|------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
|     Active write-back                                                  |     Enabled                                                                                                        |
|     Active write-back registry                                         |     Enabled                                                                                                        |
|     Enable Profile Management                                          |     Enabled                                                                                                        |
|     Path to user store                                                 |     \\<"storageaccountname">.file.core.windows.net\   upmprofiles\%username%                                         |
|     Customer Experience Improvement Program                            |     Disabled                                                                                                       |
|     Enable Default Exclusion List –   Directories                      |     Enabled                                                                                                        |
|     Delete locally cached profiles on logoff                           |     Enabled                                                                                                        |
|     Local profile conflict handling                                    |     Delete local profile                                                                                           |
|     Enable Default Exclusion List – Registry                           |     Enabled                                                                                                        |
|     Large File Handling – files to   be created as symbolic links      |     %userprofile%\AppData\Local\Microsoft\Outlook\*.OST     %userprofile%\AppData\Local\Microsoft\Outlook\*.pst    |
|     Enable multi-session write-back for FSLogix Profile   Container    |     Enabled (when using with FSLogix)                                                                              |
|     Folder Redirection                                                 |     Enabled for all folders that will not be stored in the  Profile                                           |

For this article, we configured Profile Management through Microsoft Group Policy. Minimum recommended version for using Profile Containers is Citrix Virtual Apps and Desktops 7 2012 (2012.0.0.28051), and that version was used for this article.

### FSLOGIX Configuration on Azure Files

For the lab environment we used FSLogix version 2.9.7654.46150. The following settings were configured via the FSLOGIX.ADMX template

|     Setting                                                                           |     Value                                                                   |
|---------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|
|     Office 365 Containers/Include Office activation   data in container               |     Enabled                                                                 |
|     Office 365 Containers/Include   Outlook data in container                         |     Enabled                                                                 |
|     Office 365 Containers/Sync OST to VHD                                             |     Move OST to VHD                                                         |
|     Office 365 Containers/VHD   Location                                              |     \\<"storageaccountname">.file.core.windows.net\   sharename\containers    |
|     Office 365 Containers/Include Office cache data in   container                    |     Enabled                                                                 |
|     Office 365 Containers/Include   OneDrive data in container                        |     Enabled                                                                 |
|     Office 365 Containers/Outlook Folder Path                                         |     %userprofile%\AppData\Local\Microsoft\Outlook                           |
|     Office 365 Containers/Enabled                                                     |     Enabled                                                                 |
|     Office 365 Containers/VHD access type                                             |     Direct Access                                                           |
|     Office 365 Containers/Include   Outlook personalization data in container         |     Enabled                                                                 |
|     Office 365 Containers/Store search database in   Office 365 container             |     Multi-user search                                                       |
|     Office 365 Containers/Include   OneNote UWP notebook in container                 |     Enabled                                                                 |
|     Office 365 Containers/Set Outlook cached mode on   successful container attach    |     Enabled                                                                 |
|     Office 365 Containers/Include   OneNote data in container                         |     Enabled                                                                 |
|     Office 365 Containers/Include Teams data in   container                           |     Enabled                                                                 |
|     Office 365   Containers/Advanced/Volume reattach retry count                     |     2                                                                       |
|     Office 365 Containers/Advanced/Volume reattach   retry interval                  |     10 seconds                                                              |
|     Profile Containers/Enabled                                                        |     Enabled                                                                 |
|     Profile Containers/Profile Type                                                   |     Try for read-write profile and fallback to   read-only                  |
|     Profile Containers/Set Outlook   cached mode on successful container attach       |     Set Outlook cached mode on successful container attach                  |
|     Profile Containers/Store search database in profile   container                   |     Multiuser Search                                                        |
|     Profile Containers/VHD location                                                   |     \\<"storageaccountname">.file.core.windows.net\   sharename\containers    |
|     Profile Containers/Advanced/Volume reattach retry   count                        |     2                                                                       |
|     Profile   Containers/Advanced/Volume reattach retry interval                     |     10 seconds                                                              |

Configuration directions are [here](https://docs.microsoft.com/en-us/fslogix/configure-office-container-tutorial)
