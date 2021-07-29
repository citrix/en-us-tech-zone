---
layout: doc
h3InToc: true
contributedBy: Martin Zugec, Miguel Contreras
specialThanksTo: Judong Liao, James Kindon, Dmytro Bozhko, Dai Li
description: Tech Paper focused on proper configuration, and recommendations for running an antivirus solution in Citrix Virtual Apps & Desktops environments. Recommended exclusions, configuration, and leading practices.
tz_title: Endpoint Security, Antivirus, and Antimalware Best Practices
tz_products: security;
---
# Tech Paper: Endpoint Security, Antivirus, and Antimalware Best Practices

## Overview

This article provides guidelines for configuring antivirus software in Citrix Virtual Apps and Desktops environments, and resources for configuring antivirus software on other Citrix technologies and features (for example, Cloud Connectors, Provisioning Services, and so on). Incorrect antivirus configuration is one of the most common problems that we see in the field. It can result in various issues, ranging from performance issues or degraded user experiences to timeouts and failures of various components.

In this Tech Paper, we cover a few major topics relevant to optimal antivirus deployments in virtualized environments: agent provisioning and deprovisioning, signature updates, a list of recommended exclusions and performance optimizations. Successful implementation of these recommendations depends upon your antivirus vendor and your security team. Consult them to get more specific recommendations.

**Warning!** This article contains antivirus exclusions. It is important to understand that antivirus exclusions and optimizations increase the attack surface of a system and might expose computers to various security threats. However, the following guidelines typically represent the best trade-off between security and performance. Citrix does not recommend implementing any of these exclusions or optimizations until rigorous testing has been conducted in a lab environment to thoroughly understand the tradeoffs between security and performance. Citrix also recommends that organizations engage their antivirus and security teams to review the following guidelines before proceeding with any type of production deployment.

## Agent Registrations

Agent software that is installed on every provisioned virtual machine usually needs to register with a central site for management, reporting of status and other activities. For registration to be successful, each agent needs to be uniquely identifiable.

With machines provisioned from a single image using technologies such as Provisioning Services (PVS) or Machine Creation Services (MCS), it is important to understand how each agent is identified - and if there are any instructions required for virtualized environments. Some vendors use dynamic information such as the MAC address or computer name for machine identification. Others use the more traditional approach of a random string generated during installation. To prevent conflicting registrations, each machine needs to generate a unique identifier. Registration in non-persistent environments is often done using a startup script that automatically restores machine identification data from a persistent location.

In more dynamic environments, it is also important to understand how de-provisioning of machines behaves, if cleanup is a manual operation, or if it is performed automatically. Some vendors offer integration with hypervisors or even delivery controllers where machines can be automatically created or deleted as they are provisioned.

**Recommendation:** Ask your security vendor how is the registration/unregistration of their agents implemented. If registration requires more steps for environments with single-image management, include these steps in your image sealing instructions, preferably as a fully automated script.

## Signature Updates

Timely consistently updated signatures are one of the most important aspects of endpoint security solutions. Most vendors use locally cached, incrementally updated signatures that are stored on each of the protected devices.

With non-persistent machines, it is important to understand how signatures are updated and where they are stored. This enables you to understand and minimize the window of opportunity for malware to infect the machine.

Especially in a situation in which updates are not incremental and can reach significant size, you might consider a deployment in which persistent storage is attached to each of the non-persistent machines to keep the update cache intact between resets and image updates. Using this approach, the window of opportunity and the performance impact of a definitions update is minimized.

Aside from signature updates for each of the provisioned machines, it is also important to define a strategy for updating the master image. Automating this process is recommended, so is updating the master image regularly with the latest signatures. This is especially important for incremental updates in which you are minimizing the amount of traffic required for each virtual machine.

Another approach to managing signature updates in virtualized environments is to completely replace the nature of the decentralized signatures with a centralized scanning engine. While this is primarily done to minimize the performance impact of an antivirus, it has the side benefit of centralizing signature updates as well.

**Recommendation:** Ask your security vendor how signatures are updated in your antivirus. What is the expected size and frequency, and are updates incremental? Are there any recommendations for non-persistent environments?

## Performance Optimizations

An antivirus, especially if improperly configured, can have a negative impact on scalability and overall user experience. It is, therefore, important to understand the performance impact to determine what is causing it and how it can be minimized.

Available performance optimization strategies and approaches are different for various antivirus vendors and implementations. One of the most common and effective approaches is to provide centralized offloading antivirus scanning capabilities. Rather than each machine being responsible for scanning (often identical) samples, scanning is centralized and performed only once. This approach is optimized for virtualized environments; however, make sure you understand its impact on high-availability.

![Antivirus Offloading](/en-us/tech-zone/build/media/tech-papers_antivirus-best-practices_offload.png)
*Offloading scans to a dedicated appliance can be highly effective in virtualized environments*

Another approach is based on pre-scanning of read-only portions of the disks, performed on the master images before provisioning. It is important to understand how this affects the window of opportunity (for example, what if a disk already contains infected files but signatures are not available during pre-scan phase?). This optimization often is combined with scanning for write-only events, as all reads will either originate from pre-scanned disk portions or from a session-specific write cache/differential disk that was already scanned during write operation. Often, a good compromise is to combine real-time scans (optimized) with scheduled scans (full scans of the system).

![Antivirus Write Scans](/en-us/tech-zone/build/media/tech-papers_antivirus-best-practices_diffs.png)
*The most common scan optimization is to focus only on the differences between virtual machines*

**Recommendation:** Performance optimizations can greatly improve user experiences. However they can also be regarded as a security risks. Therefore, consultation with your vendor and your security team is recommended. Most antivirus vendors with solutions for virtualized environments offer optimized scanning engines.

## Antivirus Exclusions

The most common (and often the most important) optimization for antivirus is the proper definition of antivirus exclusions for all components. While some vendors can automatically detect Citrix components and apply exclusions, for most environments, this is a manual task that needs to be configured for the antivirus in the management console.

Exclusions are typically recommended for real-time scanning. However Citrix recommends scanning the excluded files and folders regularly using scheduled scans. To mitigate any potential performance impact, it is recommended to perform scheduled scans during non-business or off-peak hours.

The integrity of excluded files and folders needs to be maintained always. Organizations can consider using a commercial File Integrity Monitoring or Host Intrusion Prevention solution to protect the integrity of files and folders that have been excluded from real-time or on-access scanning. Database and log files are excluded in this type of data integrity monitoring because these files are expected to change. If an entire folder must be excluded from real-time or on-access scanning, Citrix recommends closely monitoring the creation of new files in the excluded folders.

Scan only local drives - or disable network scanning. The assumption is that all remote locations that might include file servers that host user profiles and redirected folders are being monitored by antivirus and data integrity solutions. If not, it is recommended that network shares accessed by all provisioned machines be excluded. An example includes shares hosting redirected folders or user profiles.

**Recommendation:** Review these recommendations with your vendor and security team.

-  Review all files/folders for exclusion and confirm they exist before you create an exclusion policy.
-  Implement multiple exclusion policies for different components instead of creating one large policy for all of them.
-  To minimize the window of opportunity, implement a combination of real time and scheduled scans.

### Virtual Apps and Desktops

#### Delivery Controllers

Files:

-  `%SystemRoot%\ServiceProfiles\NetworkService\HaDatabaseName.mdf (7.12+)`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaImportDatabaseName.mdf (7.12+)`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaDatabaseName_log.ldf (7.12+)`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaImportDatabaseName_log.ldf (7.12+)`

Folders:

-  `%ProgramData%\Citrix\Broker\Cache (7.6+)`

Processes:

-  `%ProgramFiles%\Citrix\Broker\Service\BrokerService.exe`
-  `%ProgramFiles%\Citrix\Broker\Service\HighAvailabilityService.exe (7.12+)`
-  `%ProgramFiles%\Citrix\ConfigSync\ConfigSyncService.exe (7.12+)`

#### Virtual Delivery Agents

Files:

-  `%UserProfile%\AppData\Local\Temp\Citrix\HDXRTConnector\*\*.txt`

Processes:

-  `%ProgramFiles%\Citrix\User Profile Manager\UserProfileManager.exe`
-  `%ProgramFiles%\Citrix\Virtual Desktop Agent\BrokerAgent.exe`
-  `%ProgramFiles%\Citrix\ICAService\picaSvc2.exe` (Desktop OS only)
-  `%ProgramFiles%\Citrix\ICAService\CpSvc.exe` (Desktop OS only)

WebSocketService.exe file can be found in different locations in various CVAD versions. Below is a list of supported LTSR releases and the latest CR release. If you are running any other version of CVAD, we recommend confirming the file location first.

-  `%ProgramFiles%\Citrix\HTML5 Video Redirection\WebSocketService.exe` (CVAD 7.15 LTSR - both desktop and server OS)
-  `%ProgramFiles(x86)%\Citrix\System32\WebSocketService.exe` (CVAD 1912 LTSR - Server OS only)
-  `%ProgramFiles%\Citrix\ICAService\WebSocketService.exe` (CVAD 1912 LTSR - Desktop OS only)
-  `%ProgramFiles(x86)%\Citrix\HDX\bin\WebSocketService.exe` (CVAD 2003+ - both desktop and server OS)

#### Virtual Delivery Agents - HDX RealTime Optimization Pack

Files:

-  `%UserProfile%\AppData\Local\Temp\Citrix\RTMediaEngineSRV\MediaEngineSRVDebugLogs**.txt`

Processes:

-  `%ProgramFiles(x86)%\Citrix\HDX RealTime Connector\AudioTranscoder.exe`
-  `%ProgramFiles(x86)%\Citrix\HDX RealTime Connector\MediaEngine.Net.Service.exe`
-  `%ProgramFiles(x86)%\Citrix\HDX RealTime Connector\MediaEngineService.exe`

### Workspace app / Receiver for Windows

Files:

-  `%UserProfile%\AppData\Local\Temp\Citrix\RTMediaEngineSRV\MediaEngineSRVDebugLogs\*\*.txt`

Processes:

-  `%ProgramFiles(x86)%\Citrix\ICA Client\MediaEngineService.exe` (HDX RealTime Optimization Pack)
-  `%ProgramFiles(x86)%\Citrix\ICA Client\CDViewer.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\concentr.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\wfica32.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\AuthManager\AuthManSvr.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\SelfServicePlugin\SelfService.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\SelfServicePlugin\SelfServicePlugin.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\HdxTeams.exe` (Optimization for Microsoft Teams for Workspace app 2009.5 or older)
-  `%ProgramFiles(x86)%\Citrix\ICA Client\HdxRtcEngine.exe` (Optimization for Microsoft Teams for Workspace app 2009.6 or higher)

    >**Note:**
    >
    >These exclusions for the Citrix Workspace app are typically not required. We have only seen a need for these in environments when the antivirus is configured with policies that are more strict than usual, or in situations in which multiple security agents are in use simultaneously (AV, DLP, HIP, and so on).

### Provisioning

#### Provisioning Server

Files:

-  `*.vhd`
-  `*.avhd`
-  `*.vhdx`
-  `*.avhdx`
-  `*.pvp`
-  `*.lok`
-  `%SystemRoot%\System32\drivers\CvhdBusP6.sys` (Windows Server 2008 R2)
-  `%SystemRoot%\System32\drivers\CVhdMp.sys` (Windows Server 2012 R2)
-  `%SystemRoot%\System32\drivers\CfsDep2.sys`
-  `%ProgramData%\Citrix\Provisioning Services\Tftpboot\ARDBP32.BIN`

Processes:

-  `%ProgramFiles%\Citrix\Provisioning Services\BNTFTP.EXE`
-  `%ProgramFiles%\Citrix\Provisioning Services\PVSTSB.EXE`
-  `%ProgramFiles%\Citrix\Provisioning Services\StreamService.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\StreamProcess.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\soapserver.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\Inventory.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\Notifier.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\MgmntDaemon.exe`
-  `%ProgramFiles%\Citrix\Provisioning Services\BNPXE.exe` (only if PXE is used)

#### Provisioning Target Device

Files:

-  `.vdiskcache`
-  `vdiskdif.vhdx` (7.x and above when using RAM cache with overflow)
-  `%SystemRoot%\System32\drivers\bnistack6.sys`
-  `%SystemRoot%\System32\drivers\CfsDep2.sys`
-  `%SystemRoot%\System32\drivers\CVhdBusP6.sys`
-  `%SystemRoot%\System32\drivers\cnicteam.sys`
-  `%SystemRoot%\System32\drivers\CVhdMp.sys` (7.x only)

Processes:

-  `%ProgramFiles%\Citrix\Provisioning Services\BNDevice.exe`

### StoreFront

Files:

-  `%SystemRoot%\ServiceProfiles\NetworkService\AppData\Roaming\Citrix\SubscriptionsStore\**\PersistentDictionary.edb`

Processes:

-  `%ProgramFiles%\Citrix\Receiver StoreFront\Services\SubscriptionsStoreService\Citrix.DeliveryServices.SubscriptionsStore.ServiceHost.exe`
-  `%ProgramFiles%\Citrix\Receiver StoreFront\Services\CredentialWallet\Citrix.DeliveryServices.CredentialWallet.ServiceHost.exe`

### Cloud Connector

Files:

-  `%SystemRoot%\ServiceProfiles\NetworkService\HaDatabaseName.mdf`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaImportDatabaseName.mdf`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaDatabaseName_log.ldf`
-  `%SystemRoot%\ServiceProfiles\NetworkService\HaImportDatabaseName_log.ldf`

Folders:

-  `%SystemDrive%\Logs\CDF`
-  `%ProgramData%\Citrix\WorkspaceCloud\Logs`

Processes:

-  `%ProgramFiles%\Citrix\XaXdCloudProxy\XaXdCloudProxy.exe`
-  `%ProgramFiles%\Citrix\Broker\Service\HighAvailabilityService.exe`
-  `%ProgramFiles%\Citrix\ConfigSync\ConfigSyncService.exe`

### Workspace Environment Management - Server

Processes:

-  `Norskale Broker Service.exe`
-  `Norskale Broker Service Configuration Utility.exe`
-  `Norskale Database Management Utility.exe`

### Workspace Environment Management - Agent

Processes:

-  `AgentGroupPolicyUtility.exe`
-  `Citrix.Wem.Agent.LogonService.exe`
-  `Citrix.Wem.Agent.Service.exe` (before on-premises release 1909 and cloud release 1903 this process was called `Norskale Agent Host Service.exe`)
-  `VUEMCmdAgent.exe`
-  `VUEMUIAgent.exe`

### Session Recording - Server

Processes:

-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\SsRecStorageManager.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\SsRecAnalyticsService.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\SsRecWebSocketServer.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\icldb.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\iclstat.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\SsRecServerConsole.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\TestPolicyAdmin.exe`

Files:

-  `%ProgramFiles%\Citrix\SessionRecording\Server\App_Data*.xml`

Folders:

-  `C:\SessionRecordings`
-  `C:\SessionRecordingsRestored`
-  `%SystemRoot%\System32\msmq`
-  `%ProgramFiles%\Citrix\SessionRecording\Server\Bin\log`

### Session Recording - Agent

Processes:

-  `%ProgramFiles%\Citrix\SessionRecording\Agent\Bin\SsRecAgent.exe`
-  `%ProgramFiles%\Citrix\SessionRecording\Agent\Bin\SsRecAgentWrapper.exe`

Files:

-  `%SystemRoot%\System32\drivers\ssrecdrv.sys`
-  `%SystemRoot%\System32\drivers\srminifilterdrv.sys`

Folders:

-  `%SystemRoot%\System32\msmq`

### Session Recording - Player

Processes:

-  `%ProgramFiles(x86)%\Citrix\SessionRecording\Player\Bin\SsRecPlayer.exe`

Folders:

-  `%UserProfile%\AppData\Local\Citrix\SessionRecording\Player\Cache`

## Antivirus Vendors

[Bitdefender - Implementing Security Best Practices in the Virtual Data Center](https://businessinsights.bitdefender.com/implementing-security-best-practices-in-the-virtual-data-center)

[Microsoft - Windows Defender in VDI environments](https://docs.microsoft.com/en-us/windows/security/threat-protection/microsoft-defender-antivirus/deployment-vdi-microsoft-defender-antivirus)

[Microsoft - FSLogix Antivirus Exclusions](https://docs.microsoft.com/en-us/azure/architecture/example-scenario/wvd/windows-virtual-desktop-fslogix#antivirus-exclusions)

[Trend Micro - Deep Security Recommended Exclusions](https://success.trendmicro.com/solution/1102554-citrix-recommended-exclusions-on-deep-security)

## References

[Citrix Ready Workspace Security Program](https://citrixready.citrix.com/program/workspace-security-program.html)

[Citrix Guidelines for Antivirus Software Configuration](https://support.citrix.com/article/CTX127030)

[Provisioning Services Antivirus Best Practices](https://support.citrix.com/article/CTX124185)

[Antivirus layering with Citrix App Layering](/en-us/citrix-app-layering/4/layer/layer-antivirus-apps.html)
