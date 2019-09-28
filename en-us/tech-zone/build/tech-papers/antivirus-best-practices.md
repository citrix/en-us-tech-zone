---
layout: doc
---
# Endpoint Security and Antivirus Best Practices

## Contributors

**Author:** [Martin Zugec](https://twitter.com/MartinZugec)

**Special thanks:** [Miguel Contreras](https://twitter.com/ctxmigs)

## Overview

This article provides guidelines for configuring antivirus software in Citrix Virtual Apps and Desktops environments, as well as resources for configuring antivirus software on other Citrix technologies and features (e.g., Cloud Connectors, Provisioning Services, and so on). Incorrect antivirus configuration is very common problem that we see in the field. It can result in various issues, ranging from performance issues or degraded user experiences to timeouts and failures of various components.

In this Tech Paper, we will cover a few major topics relevant to optimal antivirus deployments in virtualized environments: agent provisioning and deprovisioning, signature updates, a list of recommended exclusions and performance optimizations. Successful implementation of these recommendations depends upon your antivirus vendor and your security team. You should consult them to get more specific recommendations.

**Warning!** This article contains antivirus exclusions. It is important to understand that antivirus exclusions and optimizations increase the attack surface of a system and might expose computers to a variety of real security threats. However, the following guidelines typically represent the best trade-off between security and performance. Citrix does not recommend implementing any of these exclusions or optimizations until rigorous testing has been conducted in a lab environment to thoroughly understand the tradeoffs between security and performance. Citrix also recommends that organizations engage their antivirus and security teams to review the following guidelines before proceeding with any type of production deployment.

## Agent Registrations

Agent software that is installed on every provisioned virtual machine usually needs to register with a central site for management, reporting of current status and other activities. For registration to be successful, each agent needs to be uniquely identifiable.

With machines provisioned from a single image using technologies such as Provisioning Services (PVS) or Machine Creation Services (MCS), it is important to understand how each agent is identified - and if there are any instructions required for virtualized environments. Some vendors use dynamic information such as the MAC address or computername for machine identification. Others use the more traditional approach of a random string generated during installation. To prevent conflicting registrations, each machine needs to generate a unique identifier. This is usually done using a startup script that automatically restores machine identification data from a persistent location.

In more dynamic environments, it is also important to understand how de-provisioning of machines behaves, if cleanup is a manual operation, or if it is performed automatically. Some vendors offer integration with hypervisors or even delivery controllers where machines can be automatically created or deleted as they are provisioned.

**Recommendation:** Ask your security vendor how registration/unregistration works with your specific product. If registration requires additional steps for environments with single-image management, include these steps in your image sealing instructions, preferably as a fully automated script.

## Signature Updates

Timely consistently updated signatures are one of the most important aspects of endpoint security solutions. Most vendors use locally cached, incrementally updated signatures that are stored on each of the protected devices.

With non-persistent machines, it is important to understand how signatures are updated and where they are stored. This will enable you to understand the window of opportunity for malware to infect the machine. Additionally, it will help you in designing the environment to minimize this window.

Especially in a situation in which updates are not incremental and can reach significant size, you might consider a deployment in which persistent storage is attached to each of the non-persistent machines to keep the update cache intact between resets and image updates. Using this approach, the window of opportunity and the performance impact of a definitions update is minimized.

Aside from signature updates for each of the provisioned machines, it is also important to define a strategy for updating the master image. Automating this process is recommended as is updating the master image on a regular basis with latest signatures. This is especially important for incremental updates in which you are minimizing the amount of traffic required for each virtual machine.

Another approach to managing signature updates in virtualized environments is to completely replace the nature of the decentralized signatures with a centralized scanning engine. While this is primarily done to minimize the performance impact of an antivirus, it has the side benefit of centralizing signature updates as well.

**Recommendation:** Ask your security vendor how signatures are updated in your antivirus. What is the expected size and frequency, and are updates incremental? Are there any recommendations for non-persistent environments?

## Performance Optimizations

An antivirus, especially if improperly configured, can have a negative impact on scalability and overall user experience. It is, therefore, very important to understand the performance impact in order to determine what is causing it and how it can be minimized.

Available performance optimization strategies and approaches are different for various antivirus vendors and implementations. One of the most common and effective approaches is to provide centralized offloading antivirus scanning capabilities. Rather than each machine being responsible for scanning (often identical) samples, scanning is centralized and performed only once. This approach is optimized for virtualized environments; however, please make sure you understand its impact on high-availability.

![Antivirus Offloading](/en-us/tech-zone/build/media/tech-papers_antivirus-best-practices_offload.png)
*Offloading scans to a dedicated appliance can be highly effective in virtualized environments*

Another approach is based on pre-scanning of read-only portions of the disks, usually performed on the master images before provisioning. It is important to understand how this affects the window of opportunity (e.g., what if disk already contains infected files but signatures are not available during pre-scan phase?). This optimization often is combined with scanning for write-only events, as all reads will either originate from pre-scanned disk portions or from a session-specific write cache/differential disk that was already scanned during write operation. Often, a good compromise is to combine real-time scans (optimized) with scheduled scans (full scans of the system).

![Antivirus Write Scans](/en-us/tech-zone/build/media/tech-papers_antivirus-best-practices_diffs.png)
*The most common scan optimization is to focus only on the differences between virtual machines*

**Recommendation:** Performance optimizations can greatly improve user experiences. However it should be noted that they can be regarded as a security risks. Therefore, consultation with your vendor and your security team is recommended. Most antivirus vendors with solutions for virtualized environments offer optimized scanning engines.

## Antivirus Exclusions

The most common (and often the most important) optimization for antivirus is proper definition of antivirus exclusions for all components. While some vendors can automatically detect Citrix components and apply exclusions, for most environments, this is a manual task that needs to be configured for the antivirus in the management console.

Exclusions typically are recommended for real-time scanning; however Citrix recommends scanning the excluded files and folders on a regular basis using scheduled scans. To mitigate any potential performance impact, it is recommended to perform scheduled scans during non-business or off-peak hours.

The integrity of excluded files and folders should be maintained at all times. Organizations should consider leveraging a commercial File Integrity Monitoring or Host Intrusion Prevention solution to protect the integrity of files and folders that have been excluded from real-time or on-access scanning. It is noteworthy that database and log files should not be included in this type of data integrity monitoring because these files are expected to change. If an entire folder must be excluded from real-time or on-access scanning, Citrix recommends very closely monitoring the creation of new files in the excluded folders.

Scan only local drives - or disable network scanning. The assumption is that all remote locations that might include file servers that host user profiles and redirected folders are being monitored by antivirus and data integrity solutions. If this is not the case, it is recommended that network shares accessed by all provisioned machines be excluded. An example includes shares hosting redirected folders or user profiles.

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
-  `%SystemRoot%\System32\spoolsv.exe`
-  `%SystemRoot%\System32\winlogon.exe`
-  `%ProgramFiles%\Citrix\ICAService\picaSvc2.exe` (Desktop OS only)
-  `%ProgramFiles%\Citrix\ICAService\CpSvc.exe` (Desktop OS only)

### Workspace app / Receiver for Windows

Files:

-  `%UserProfile%\AppData\Local\Temp\Citrix\RTMediaEngineSRV\MediaEngineSRVDebugLogs\*\*.txt`

Processes:

-  `%ProgramFiles(x86)%\Citrix\ICA Client\MediaEngineService.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\CDViewer.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\concentr.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\wfica32.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\AuthManager\AuthManSvr.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\SelfServicePlugin\SelfService.exe`
-  `%ProgramFiles(x86)%\Citrix\ICA Client\SelfServicePlugin\SelfServicePlugin.exe`

Please note that these exclusions for Receiver typically are not needed. We have only seen a need for these in environments when the antivirus is configured with policies that are more strict than usual, or in situations in which multiple security agents are in use simultaneously (AV, DLP, HIP, etc.)

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

### Workspace Environment Management

Processes:

-  `Norskale Broker Service.exe`
-  `Norskale Broker Service Configuration Utility.exe`
-  `Norskale Database Management Utility.exe`

## References

[Citrix Ready Workspace Security Program](https://citrixready.citrix.com/program/workspace-security-program.html)

[Citrix Guidelines for Antivirus Software Configuration](https://support.citrix.com/article/CTX127030)

[Provisioning Services Antivirus Best Practices](https://support.citrix.com/article/CTX124185)

[Antivirus layering with Citrix App Layering](/en-us/citrix-app-layering/4/layer/layer-antivirus-apps.html)
