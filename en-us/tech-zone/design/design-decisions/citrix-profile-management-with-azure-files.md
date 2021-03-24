---
layout: doc
h3InToc: true
contributedBy: Loay Shbeilat
specialThanksTo: Paul Wilson
description: The article covers guidance and best practices for using Citrix Profile Management to manage user profiles on Azure Files as the back-end storage location.
---
# Citrix Profile Management with Azure Files

## Introduction

The article covers guidance and best practices for using Citrix User Profile Manager to manage user profiles on Azure Files as the back-end storage location. The primary objective of this article is to provide you the necessary information to determine the best way to deploy User Profile Manager with Azure Files.

The article discusses the Citrix and Microsoft components, describe the test methodology, and report the test results. The article also provides an analysis of the findings and guidance around key deployment decisions. Finally, additional information around securing, protecting, and migrating your user profile data is provided.

## Azure Files

Azure Files is a secure, publicly hosted Server Message Block (SMB) or Network File System (NFS) file share with low latency access. Azure Files supports both Active Directory integration and NTFS file-level permissions and is accessible from Windows, Mac and Linux clients.

Azure Files can be used for various enterprise purposes, including

-  File Servers. Azure Files can directly be mounted from all popular operating systems across the world. Azure Files can also be deployed in a hybrid scenario, where data can be replicated to on-premises Windows servers via Azure File Sync.

-  Application migration. Move both the user applications and their data to the cloud simultaneously. Alternatively you can move the user data first and run in a hybrid configuration until the applications can be moved into the cloud.

-  Simplifying cloud development. Use Azure files to store shared application settings, diagnostic logs, metrics, or developer utilities.

-  Containerization. Use Azure Files for persistent storage of containers or as a shared storage file system.

With a shared access capable, fully managed, resilient file share, Azure Files is an excellent place to store user profile data for your Citrix users. Azure Files uses SMB 3.0 so all the data transferred is protected by encryption while in-transit.

### Performance Tiers

Azure Files has four different performance levels with different costs and performance metrics. The two tiers, transaction optimized and premium, are evaluated and recommended for storing user profile data

***Transaction*** ***optimized*** (formerly known as Standard) performance offers up to 100 TB of storage in a single file share with a maximum throughput of 300 MiB/sec. The transaction performance level is designed to support workloads that do not need high-performance. When using the transaction optimized option for user profile workloads, be sure to enable the "large file shares" setting. The maximum IOPS supported for a transaction file share with "large file shares" enabled is 10,000 IOPS. This is true for a 10 TiB share and a 100 TiB share.

Transaction storage accounts without "large file shares" enabled (\< 5 TiB), support multiple redundancy options. These options include locally redundant, zone redundant, and geo-redundant with read-only and hot or cold tiering available. When support for "large file shares" is enabled, only the locally redundant storage and zone redundant storage options are available.

***Premium*** performance offers up to 100 TiB of storage in a single file share with 100,000 IOPS and throughput based on the provisioned size of the file share. A premium file share is provided a base ingress of 40 MiB/sec + (0.04 \* provisioned GiB) while the base egress rate is 60 MiB/sec + (0.06 \* provisioned GiB). For a 10 TiB share, the throughput equates to an ingress rate of 450 MiB/sec and an egress rate of 675 MiB/sec. The premium performance level is designed to support I/O intensive workloads while delivering latency of under 10 milliseconds.

The IOPS and throughput are provisioned based on the size of the file share. Be sure to provision a size that covers the IOPS and throughput your users need. For a premium file share, the base IOPS allocated is 400 + 1 IOPS for each GiB of provisioned space, with a maximum of 100,000 IOPS. A premium share is allowed to burst up to greater of 4000 IOPS or 3 times of base IOPS allocated. A 10 TiB files share would be allocated 10,240 IOPS. With the premium file share, burst mode allows up to 3 times the allocated IOPS to be used. However burst mode requires the file share be running at less than the allocated IOPS for a period of time to regenerate. In other words, you cannot run in burst mode all the time. If there are enough credits available, the shares can burst up to 30,720 IOPS for up to a maximum of 60 minutes of duration.

Both performance tiers are acceptable options for storing user profile data. Selecting between the performance tiers depends primarily on the user workload. The goal is always to provide the end user with their required performance and data protection at the lowest cost. For user profiles where response time is the highest priority, premium file shares are the best candidates. For more information on the performance of the different tiers, visit [Microsoft's storage performance webpage](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-scale-targets). To determine what performance tier is best starts with monitoring key metrics.

### Monitoring

You can use Azure Monitor to view the key metrics on your Azure File shares and make adjustments. For storage, closely monitor throughput, transactions, and latency metrics. To enable these metrics, in the **Azure portal** select the storage account and then select **Metrics** from the **Storage Account** blade. The key metrics we used to monitor the performance of Azure Files during the test are as follows:

***Ingress (Sum)***: The aggregated sum of inbound data in bytes to Azure Files during the time period, with a minimum granularity of one minute. To determine the average ingress throughput per second, you would need to take the sum of the ingress data for a minute and divide it by 60. To add this metric, click **Add metric** and set the **scope** to the storage account name. Set the **namespace** to Account, then choose Ingress as the **metric** and Sum as the **aggregation** type.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_001.png)

***Egress (Sum)***: The aggregated sum of the outbound data in bytes from Azure Files during the time period, with a minimum granularity of one minute. To determine the average egress throughput per second, you would need to take the sum of the egress data for a minute and divide it by 60. To add this metric, click **Add metric** and set the **scope** to the storage account name. Set the **namespace** to Account then choose Egress as the **metric** and Sum as the **aggregation** type.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_002.png)

***Transactions (Sum)***: The number of requests, both successful and failed, made to Azure Files, during the time period, with a minimum granularity of one minute. This metric is loosely analogous to input/output operations per second (IOPS). To determine the average IOPS during a one-minute time period, you would take the Sum of Transactions for that minute and divide it by 60. To add this metric, click **Add metric** and set the scope to the storage account name. Set the **namespace** to Account, then choose Transactions as the **metric** and Sum as the **aggregation** type.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_003.png)

***Transactions Success with Throttling (Sum)***: The aggregated number of requests that failed due to provisioned limits on the first attempt but were successful after retries. This metric is an API filter on Response Type and is only valid for the premium tier where resources are provisioned. If this number is significant when compared to the sum of transactions, increase your file share size. To add this filter, you must first add the Transactions metric. Then click **Add filter**, choose the **Response type** property, **operator** of = and select one of the filters in the **values** field. This filter is not visible as a selection unless you are seeing throttled transactions. For premium shares, you can also look at specific response types (SuccessWithShareIopsThrottling, SuccessWithShareEgressThrottling, SuccessWithShareIngressThrottling) to determine whether IOPS or throughput is throttled. For more information on the metrics dimensions, check the [Microsoft website](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-monitoring-reference#metrics-dimensions).

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_004.png)

***Success Server Latency (Avg)***: The average amount of time that Azure Files required to complete the request. This value does not contain any network latency. If this value is increasing, the Azure Files infrastructure is working at maximum capacity and falling behind. To add this metric, click **Add metric** and set the **scope** to the storage account name. Set the **namespace** to Account, then choose Success Server Latency as the **metric** and Avg as the **aggregation**.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_005.png)

***Success E2E Latency (Avg)***: The average end-to-end latency required to complete a successful transaction with Azure Files. This value includes the network latency between the client and Azure Files. If this number is increasing when compared to the success server latency value, investigate the network. To add this metric, click **Add metric** and set the **scope** to the storage account name. Set the **namespace** to Account, then choose Success E2E Latency as the **metric** and Avg as the **aggregation**.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_006.png)

## Citrix Profile Management

The use of Citrix Profile Management (CPM) greatly enhanced the end-user experience during our testing. Citrix Profile Management is designed to remove profile bloat and significantly speed logon times, while reducing profile corruption. Citrix Profile Management is a feature of Citrix Virtual Apps and Desktops Service.

### Key Features

The key features of CPM that provide a great end-user experience when Azure Files is used for the back-end storage are as follows:

***Profile Streaming***: Files and folders contained in a profile are fetched from the user store to the local computer only when users access the files after they have logged on. Profile streaming significantly reduces the logon time by reducing the amount of data that is copied down to the local profile at user logon. This setting is enabled by default and decreases the amount of transactions and egress data leaving Azure Files, reducing costs, and improving logon time.

***Active Write-back***: Files and folders that are modified can be synchronized to the user store in the middle of a session, before logoff. Usually, these mid-session writes occur about every 5 minutes. Enabling this setting provides Azure Files with a more consistent stream of write traffic, instead of having all the writes occur at logoff. This frequent write-back resolves the "last writer wins" issue when users are accessing their profile from multiple devices simultaneously.

***Large File Handling***: To improve logon performance and to process large-size files, a symbolic link is created instead of copying files in this list. You must configure the paths where the files are stored but you can use wildcards. This setting is a highly recommended when using the transactional tier and recommended for the premium tier.

### Profile Management Configuration

For the testing, each of the Citrix hosts had the Multi-session OS Virtual Delivery Agent (VDA) 2006 installed. During installation, on the **Additional Components** screen, both the *Citrix User Profile Manager* and *Citrix User Profile Manager WMI Plug-in* were enabled.

The following GPO settings for Profile Management were configured and the GPO was assigned to all the Citrix hosts being used for the tests.

| GPO Setting                                  | Value                |
|----------------------------------------------|----------------------|
| Active write back                            | Enabled              |
| Active write back registry                   | Enabled              |
| Enable Profile Management                    | Enabled              |
| Path to user store                           |                      |
| Customer Experience Improvement Program      | Disabled             |
| Enable Default Exclusion List -- Directories | Enabled              |
| Delete locally cached profiles on logoff     | Enabled              |
| Local profile conflict handling              | Delete local profile |
| Enable Default Exclusion List -- Registry    | Enabled              |
| Large File Handling -- files to be created as symbolic links     | OSTFolder\\\*.ost PSTFolder\\\*.pst    |

### Monitoring

Monitoring the performance can be done through the Citrix Director. The key metric to watch for performance is the profile load time. The profile load time provides the number of seconds it takes for the user's profile to load into the Citrix session. Profile load time includes the amount of time it takes to copy the user's profile down from the profile storage, in this case from Azure Files. The user profile load time has a significant impact on the user experience. Profile load times of greater than 30 seconds usually result in a poor user experience. The profile load time is available through the Citrix Director. From Citrix Cloud Virtual Apps and Desktops Service, select **Monitor** \>\> **Trends** \>\> **Logon Performance** as shown in the following screenshot:

The profile load time is available by hovering over the time period being monitored or viewed in the table below the chart.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_007.png)

## Test methodology

The primary goal was to assess the Azure File shares under a load and determine for a specific workload how the transaction optimized and premium tiers would perform with CPM configured on the Citrix hosts. The following settings were kept static during the test runs

  |Static Configuration   | Value|
  |-----------------------|------------------------------------------------|
  |Number of users        | 1000|
  |File Share Quota Size  | 10 TB|
  |Folder Redirection     | Enabled for Desktop, Documents, and Pictures|
  |Large File Handling    | 4 GB OST file updated with CPM large file enabled|
  |Logon Rate             | 1000 users per hour, or 1 user every 3.6 seconds|
  |User Profile Size      | 4.7 GB, 400 Files|

The following test variables were decided upon after assessing performance during a wide variety of test runs of different configurations.

| Variable               | Values to Test        |
|------------------------|-----------------------|
| Azure File Tier        | Transaction optimized |
|                        | Premium               |
| Files updated per hour | 20,000 files          |
|                        | 40,000 files          |
|                        | 60,000 files          |
|                        | 80,000 files          |
|                        | 100,000 files         |

To simulate a 1000 users with a few resources, we designed a test that used a custom PowerShell script. The script randomly updated several files stored either in the user's redirected folder (87.5%) or in the user's local profile (12.5%). This script when run by a single-user would be updating between 20 and 100 files (depending on the number of files per hour to update) in the session. The script also updated the 4 GB OST file stored in the local profile folder.

To control the launch rate and provide random wait times during the session, we used LoginVSI for the test framework. The random waits helped prevent predictable spikes in network traffic caused by users logging on simultaneously and helps simulate a better workload. The user would logon and access the required files before logging off. To exclude logoff traffic from the results, all users were logged on and the files accessed before any user logged off. The graphs show the metrics from the first one hour of the test while users were logging on.

### Test Environment Setup

For the 100,000 files to be tested, we built an environment that supported 1,000 user sessions logging on within a 60-minute window. To avoid the test infrastructure from becoming a bottleneck, the Citrix host servers were deliberately oversized to move the performance issues to the Azure Files share.

We had two peered virtual networks configured in the Azure US West 2 region. One virtual network contained the LoginVSI infrastructure used to manage the sessions and report on progress. The other virtual networks contained the 30 Citrix hosts running on a Standard FS16_v2 instance with 1 TB Premium SSD drive. The Citrix servers hosted the sessions that required user profiles to be loaded from and written to Azure Files. We enabled and configured Citrix Profile Management to use the Azure Files SMB share as the path store.

Two Azure storage accounts were created, one with a transaction optimized tier SMB share and one with a premium tier SMB share. The "large file shares" setting was enabled on the transaction tier. Both storage accounts used Locally Redundant Storage (LRS). The default user profile had 400 files ranging in size from 85 KB to 5225 KB plus a single 4 GB file use for large-file testing. Total user profile size was approximately 4.7 GB.

Citrix Cloud was used to manage the machine catalogs and the delivery groups. A local StoreFront server was used for enumeration. The 30 Citrix hosts were running Windows Server 2016 with the Citrix Multi-session OS Virtual Delivery Agent (VDA) 2006. The Citrix StoreFront server and the Citrix Cloud Connectors were on the subnet with the LoginVSI infrastructure, the Citrix hosts resided on a different subnet.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_008.png)

### Test Runs

Based on the variables identified earlier, the following test matrix was run, and the performance data was gathered from Azure Monitor and Citrix Director on each run.

  |Run Name           |Azure File Tier         |File Share Quota   |Files Updated|
  |------------------ |----------------------- |------------------ |-------------|
  |Premium-20K        |Premium                 |10 TB              |20,000|
  |Premium-40K        |Premium                 |10 TB              |40,000|
  |Premium-60K        |Premium                 |10 TB              |60,000|
  |Premium-80K        |Premium                 |10 TB              |80,000|
  |Premium-100K       |Premium                 |10 TB              |100,000|
  |Transaction-20K    |Transaction optimized   |10 TB              |20,000|
  |Transaction-40K    |Transaction optimized   |10 TB              |40,000|
  |Transaction-60K    |Transaction optimized   |10 TB              |60,000|
  |Transaction-80K    |Transaction optimized   |10 TB              |80,000|
  |Transaction-100K   |Transaction optimized   |10 TB              |100,000|

After the warm-up test run, each test run consisted of the following steps

1.  For premium tier, wait 2 hours to allow the burst IOPS to regenerate

2.  Update the Computer GPO assigned to the Citrix host servers to set
    the user's profile location to the correct file share (transaction
    or premium)

3.  Restart the Citrix Servers

4.  Set the PowerShell script to update the correct number of files (20,
    40, 60, 80, 100)

5.  Configure the test run to launch all 1000 user sessions within 60
    minutes

6.  Launch the test

7.  Record the start date/time

8.  Each session ran at least 60 minutes before logging off

9.  Record the stop date/time after all sessions are logged off

10.  Gather Azure Monitor data for key metrics

11.  Gather Citrix Director data for Logon Performance

## Test Results

The test results are displayed by the key metrics discussed earlier: Transactions (IOPS), Throughput (egress and ingress), Latency, and Profile Load Time. Note these tests were run before SMB Multichannel was released. Azure Files' SMB Multichannel benefits performance when used with Profile Services.

### Transactions

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_009.png)

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_010.png)

Transactions are recorded as part of the Azure Files metrics. The preceding charts show a rather consistent amount of IOPS being used across all test runs. Regardless of type of file share, the average IOPS per second are under about 3300. The consistent results are expected since the test was only varying the number of files touched between the two file share types. The files themselves were the same for every user profile.

Since both file shares support 10,000 IOPS in the configuration for the test, the IOPS required by our workload is well within the limits. This suggests that Azure Files can support 3X this workload before IOPS became a bottleneck.

### Throughput

Throughput is available as part of the Azure Files metrics. The following charts show the egress and ingress traffic for the different tiers. The egress traffic is higher than the ingress traffic because Citrix Profile Manager is scanning the full profile at logon but only changing a portion of the files. If profile streaming was not enabled as part of Citrix Profile Manager, the entire profile would be read and copied down to the local host. As the number of files updated during the run increases, the egress throughput also increases. This behavior is because the entire file is streamed down to the local host, changed, and written back to the share.

The throughput for the premium share is well within the provisioned amounts of 675MiB/sec egress and 450MiB/sec ingress. The premium share is able to handle approximately 15 times the workload as our test before throughput starts to become an issue.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_011.png)
![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_012.png)

The transaction file share limit is higher at 300 MiB/sec so theoretically, the transactional file share can handle 5x the workload before the throughput becomes a bottleneck.

These results are writing to the 4 GiB OST file, but that file is not read down to the local host because Large File Handling is enabled. The writes to the OST files are made directly on the Azure Files share, much like the folder redirection writes.

### Latency

Latency is available as part of the Azure Files metrics. The following chart shows the Success Server Latency metric from the premium and transaction optimized test runs. Premium tier file shares do best with fewer large files, such as user profile technologies like FSLogix and Citrix Profile Manager VHD container. Using container files improves the performance of Azure Files because the number of file open/read/write/close requests are reduced.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_013.png)
![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_014.png)

Focusing on the premium file share, we see that latency remains relatively steady at around 4 milliseconds (ms). Reviewing the transaction optimized file share, latency stays consistent across the different test runs. The latency stays in the 7-10 ms range, with only occasional spikes outside of that range. These longer latencies translate into longer profile load times as the later users logon. These are discussed further in the Profile Load Time section.

### Profile Load Time

Profile load time is part of the Citrix Director trend analysis. The following table provides a brief overview of the average profile load times based on the run data collected from Citrix Director.

  |Run                |Avg Load Time (seconds)|
  |------------------ |------------------------|
  |Premium-20K        |8.00|
  |Premium-40K        |5.34|
  |Premium-60K        |9.10|
  |Premium-80K        |5.31|
  |Premium-100K       |8.47|
  |Transaction-20K    |15.30|
  |Transaction-40K    |19.12|
  |Transaction-60K    |14.09|
  |Transaction-80K    |18.58|
  |Transaction-100K   |12.09|

The following charts show a moving average over the previous minute for the profile load time for the users during the test runs.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_015.png)
![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_016.png)

As expected, the premium tier was provided the best load times over all. The interesting thing is that the Premium-80K run had the best load times on average followed by the Premium-40K. While the Premium-20K, which had the lightest load, is in the middle with the Premium-100K. The Premium-60K run had the worst load times. These numbers show the variability within the performance tier, even though it can consistently provide profile load times in under 10 seconds.

The transaction file share seemed to fair well, with profile load times averaging under 20 seconds for the users with a slight variability in the load times. Some users did have longer load times, but the overall average for the standard file share was about 15.84. This load time is just over double the overall premium load time average of 7.25 seconds.

## Analysis of the Results

Azure Files does an excellent job of managing end user profiles when configured correctly. The results of the testing indicate with our workload, the premium file share seems to provide the best overall consistent performance at different levels. This performance comes with the added cost of premium tier pricing. While the premium file share does perform well, it would probably perform better using profile container technologies.

### Citrix Profile Manager Configuration Options

Citrix Profile Manager is best used when users have multiple profiles open from different devices to protect against the "last write wins" scenario. This feature keeps the profiles consistent between logins when users have multiple sessions open. However, when using Azure Files with Citrix, a few other features are recommended to improve the user experience.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_017.png)

Always enable Folder Redirection when possible. As the chart shows, enabling folder redirection prevents a significant amount of data from being copied from Azure Files to the Citrix host. Folder redirection was designed to reduce the amount of network traffic at logon/logoff and this greatly improves the user logon experience and lower cost of deployment. Less data being copied to the Citrix host means smaller (and less expensive) storage attached to the Citrix host for storing the user profiles. When Citrix hosts reside in Azure, the latency for connecting to Azure Files is minimal and the user experience is not impacted. When Citrix hosts are on-premises, the redirection saves on the chargeable egress traffic leaving Azure.

![Azure files](/en-us/tech-zone/design/media/design-decisions_citrix-profile-management-with-azure-files_018.png)

Enabling the Large File Handling feature of Citrix Profile Manager is a fantastic way to improve the logon experience for users. The improved logon time is achieved by updating large files directly on the profile store instead of copying them down at logon. Since Microsoft does not recommend storing PST and OST files in a redirected folder, use large file handling to provide the same benefits as folder redirection. Citrix Profile Manager creates a symbolic link to the file, so when it is opened or updated, the file operations are redirected to the user store in Azure Files. This feature allows PST and OST files to remain in the user's profile on a remote file share while being treated as local files. The preceding chart shows the difference in egress traffic between enabling and disabling large file support with the transaction and premium file shares.

The later versions of CPM enable profile streaming by default on Citrix servers. This technology prevents the entire user profile from being downloaded to the Citrix host at logon. Instead, only a list of the files residing on the profile are enumerated and that list is available to the operating system. When a file is requested, then it is fetched from the user store and brought to the Citrix host. This process reduces the number of file copies that happen at logon and improves the user logon experience.

### Selecting A Performance Tier

The results support using a transaction optimized file share when your throughput and IOPS requirements are below the supported levels of 300 MiB/sec and 10,000 IOPS respectively. Creating multiple file shares across different storage accounts to host the user's profile data, distributes the load.

*NOTE: The recommended best practice is to have only one transaction optimized file share per storage account. This recommendation is because two or more transaction optimized file shares under normal use can exceed the maximum limits on a single storage account. For more information see [Scalability and performance targets](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-scale-targets)*

If using the higher performance premium tier for consistent performance and lower latency, do some benchmarking before you settle on a final size. The best way to benchmark is to run the most intensive benchmark tests with a 100 TiB premium file share and monitor the transactions, throughput, and latency metrics. Use that information to determine the amount of provisioned storage for the required performance level.

*NOTE: If the file share has a significant number of small files being updated on a frequent basis, Azure Files may have delays reading and writing the file due to excessive metadata. This delay can also happen if there is a throttling observed on the file shares. If you are experiencing throttling, the Success Server Latency metric on the Azure Files share shows a steady increase. Once that metric exceeds 15 milliseconds, the user experience noticeably degrades.*

In the end, selecting a performance tier comes down to determining your expected response time and redundancy requirements. If the user response time needs to be under 10 ms, the premium tier file shares provide a consistent response time. This response time, in our testing configuration, translates into a profile load time of under 10 seconds. If your users accept a response time of over 10 ms then you can use the more cost-efficient transaction optimized file share. To provide the best user experience use multiple storage accounts to distribute the load.

Use the data provided in this article to determine the optimal configuration for your organization and test the performance of Azure Files with your own workloads.

## Securing Access via Private Endpoint and RBAC Permissions

Azure Files provides two main types of endpoints for accessing Azure
file shares:

-  Public endpoints, which have a public IP address and can be accessed
    from anywhere in the world.

-  Private endpoints, which exist within a virtual network and have a
    private IP address from within the address space of that virtual
    network.

Considering the file share is used for the user profile and data, we recommend that it is only accessible from within the Azure VNET via [private endpoints](https://docs.microsoft.com/en-us/azure/storage/common/storage-private-endpoints). For detailed instructions, use the [following link](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-networking-endpoints?tabs=azure-portal#endpoint-configurations).

Once the private endpoint is configured, validate accessibility from a VM within the Azure VNET as outlined in the document.

Next step is to lock down the permissions via RBAC. Follow the guidance in [Set up Azure Files storage for User personalization layers and Citrix Profile Management](/en-us/tech-zone/build/deployment-guides/citrix-azure-files.html#assign-share-level-permissions-to-users).

## Protecting Data

Once the user data is stored in Azure Files, you need to protect from backup and loss. This section provides guidance for the two built-in features: Soft Delete and Azure Backup, that offer data protection, in addition to the storage account redundancy options that are available when the storage account is created.

### Soft Delete

Soft delete is an Azure feature that allows you to recover files accidentally deleted by a user without the need to go through a complex restore process. Soft delete can be enabled in the file service section of the storage account. Along with enabling soft delete, you can set the number of days the deleted file will be available through the soft delete feature.

### Azure Backup

When using premium tier or when large file shares are enabled on a transaction optimized, the data replication is limited to Locally Redundant Storage and Zone Redundant Storage. Without large files shares you can also use Geo Redundant Storage, but this limits the share to 5 TiB, 60 MiB/s, and 1000 IOPS. Azure Backup also provides a reliable way to protect your data via Azure File snapshots. You can use Azure Backup to set up retention schedules through the configuration options in the Azure Backup vault.

Since Azure Backup is using snapshots to protect your data, do not delete those snapshots or you may lose your recovery points and be unable to restore. To prevent against accidental deletion, Azure Backup locks the storage account. If you delete that lock and the share is deleted, all the backups and snapshots are also deleted and all your data are lost.

In addition to these, many third-party vendors also offer solutions that can be used to backup your Azure Files data.

## Migrating Files

Getting your user data files migrated into Azure Files can be accomplished through several different methods. Before selecting a migration strategy, you need to determine the amount of data, the target number of files shares, and the target structure for the user data store.

Which method works best for you depends on where the data currently resides within your network. No enterprise is the same and the process to migrate files varies greatly between enterprises. Several methods exist to move the files; usually, you end up employing more than one method to achieve the desired migration.

-  ***Azure File Sync:*** Any Windows server running Windows Server 2012 R2 or later can have the Azure File Sync agent installed. The agent will then upload the user data files from the Windows serve to Azure Files. The agent can typically move about 1 TB every two days if the outbound network is not throttled. With Azure File Sync the NTFS permissions, access control lists (ACLs) and file metadata are preserved.

-  ***Storage Migration Service:*** Storage Migration Service (SMS) is designed specifically to help you migrate data from Windows Servers into Azure. The service can inventory existing servers, transfer data and even assume the identity of the source servers to make cutover easier on end-users.

-  ***Microsoft Data Box Disk:*** Microsoft Data Box Disk is an 8 TB SSD flash drive that can be stacked up to 5x for a total of 40 TB. The Data Box Disk supports AES-128 encryption and relies on file copy utilities such as Robocopy to transfer the data.

-  ***Microsoft Data Box:*** Microsoft Data Box is an offline file transfer solution that uses common file transfer utilities such as Robocopy to move data securely from your servers to the Data Box. The Data Box is then shipped to Microsoft and uploaded directly to Azure Files. Microsoft Data boxes come in 2 sizes, 100 TB and 1 PB. Both devices store the data encrypted (AES-256) to protect it while in transit between the on-premises data center and the Azure data center. Microsoft Data Box supports the SMB and NFS NAS protocols.

-  ***Data Box Gateway:*** A virtual appliance running on either Hyper-V or VMware that acts as a storage proxy server backed by Azure Files. Local users access it over SMB or NFS protocols and the gateway accepts the data and transfers it to Azure Files over the internet. The Data Box Gateway provides for continuous data flow from the on-premises servers to Azure Files.

-  ***Storage Explorer:*** The latest version of Storage Explorer uses AZCOPY to speed the file transfers significantly. Storage Explorer can even provide you the AZCOPY command strings if you feel inclined to automate the data transfer.

-  ***AZCOPY:*** An Azure command-line utility that allows you to move data between storage locations, including on-premises file servers and storage accounts within Azure.

-  ***RoboCopy:*** Reliable file copy utility that preserves all the files attributes, permissions, and ACLs during the transfer.

In addition to these file transfer/copy solutions, many third-party vendors also offer solutions that can be used to migrate your data into Azure Files. For more information on the best migration path see [Migrate to Azure file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-migration-overview).

## Conclusion

Using Azure Files with Citrix deployments in Azure is highly recommended. Azure Files provides that high-availability of user profile data without the complicated infrastructure. Azure Files can also be used with on-premises deployments if the latency and networking performance is acceptable for users.

Citrix Profile Manager provides several benefits when used with Azure Files. These benefits include the use of profile streaming and large file handling to reduce the amount of data copied from the user profile store to the Citrix host.

In most cases, the use of the transaction optimized tier for Azure Files is sufficient. Especially, if you can distribute the load across multiple shares and target workloads that keep the number of files updated per hour to around 100,000 for a single share. For low latency, high response time requirements, where users need latency to be consistently under 10 ms, look at using the premium tier of Azure Files.

## References

Content from the following documents has been referenced in this article:

-  [Azure Files scalability and performance targets](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-scale-targets)

-  [Enable and create large file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-how-to-create-large-file-share?tabs=azure-portal)

-  [Planning for an Azure Files deployment](https://docs.microsoft.com/en-us/azure/storage/files/understanding-billing#provisioned-model)

-  [Monitoring Azure Files](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-monitoring?tabs=azure-portal#monitoring-data)

-  [Azure Files monitoring data reference](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-monitoring-reference)

-  [About Azure file share backup](https://docs.microsoft.com/en-us/azure/backup/azure-file-share-backup-overview)

-  [Enable soft delete on Azure file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-enable-soft-delete?tabs=azure-portal)

-  [Migrate to Azure file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-migration-overview)

-  [Get started with AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10)

-  [Storage Migration Service overview](https://docs.microsoft.com/en-us/windows-server/storage/storage-migration-service/overview)

-  [Azure Files Introduction](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-introduction)

-  [Planning for an Azure Files deployment](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-planning\#storage-tiers)

-  [Citrix Product Documentation - Profile Management policy descriptions and defaults](/en-us/profile-management/current-release/policies/descriptions-and-defaults.html)

-  [Citrix Product Documentation - Profile Management](https://www.citrix.com/go/jmp/upm.html)

-  [Citrix Tech Zone - Set up Azure Files storage for User personalization layers and Citrix Profile Management](/en-us/tech-zone/build/deployment-guides/citrix-azure-files.html)
