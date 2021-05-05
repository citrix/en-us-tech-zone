---
layout: doc
h3InToc: true
contributedBy: Elaine Welch, Loay Shbeilat
description: Learn how to deploy Azure Files for use with Citrix User personalization layers and Citrix Profile Management.
tz_title: Deploying Azure Files for Citrix Profile Management and Citrix User personalization layers
tz_products: citrix-virtual-apps-and-desktops;
---
# Deployment Guide: Deploying Azure Files for Citrix Profile Management and Citrix User personalization layers

Azure Files offers fully managed file shares in the cloud, and accessible using Server Message Block (SMB) protocol. Azure Files shares can be mounted simultaneously in the cloud and on-premises, on Windows, Linux, or macOS.

Azure Files support for on-premises Active Directory Domain Service Authentication enables Citrix User personalization layers and Citrix Profile Management to use Azure Files. For more about Active Directory Domain Service Authentication, see [on-premises Active Directory Service Authentication over SMB for Azure File shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-auth-active-directory-enable). This article explains how to set up Azure Files to use with Citrix User personalization layers and Citrix Profile Management.

## Requirements

In addition to the requirements for [User personalization layer](/en-us/citrix-virtual-apps-desktops/install-configure/user-personalization-layer.html) and [Profile Management](/en-us/profile-management/current-release/system-requirements.html), Azure Files requires that your on-premises domain controller is synchronized to your Azure Active Directory.

## Overview

Before you set up User personalization layers or Profile Management, set up Azure Files using the following steps:

-  Step 1: Synchronize Azure AD with your on-premises AD
-  Step 2: Create Azure Files share
-  Step 3: Enable Azure Files AD DS Authentication
-  Step 4: Assign share level and NTFS permissions

## Step 1: Synchronize Azure AD with your on-premises AD

To use Azure Files with AD Authentication, [Synchronize your on-premises AD with Azure AD, using Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap).

>IMPORTANT:
>
>-  The Azure AD tenant and the file share that are used for user personalization layers or Profile Management must be associated with the same subscription.
>-  The accounts being used must be created in the domain controller and synchronized to Azure AD. Accounts sourced from Azure AD are not appropriate.

After the Synchronization completes, give it some time for Users and Groups to be replicated to Azure AD before you proceed.

## Step 2: Create Azure Files share

This procedure explains how to create an Azure Files file share for storing your user layers and profiles.

Currently, there are two tiers of Azure Files, Standard and Premium. Choose the appropriate tier based on your performance requirements. For more about Azure Files performance, refer to [Azure Files scalability and performance targets](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-scale-targets).

This document explains how to set up Standard storage as an example.

1.  Open the Azure Portal.
1.  Click **Create a resource**.
1.  Select **Storage account – blob, file, table, queue**.
1.  Enter the following information into the **Create storage account** page:
    -  For **Resource Group**, click **Create new**.
    -  For **Storage account**, enter a unique name.
    -  For **Location**, we recommend you choose the same location as the Virtual Delivery Agents (VDAs) in the Azure resource location.
    -  For **Performance**, select **Standard**. (example choice)
    -  For **Account kind**, select **StorageV2**.
    -  For **Replication**, select **Locally-redundant storage (LRS)**.
1.  When you're done, select **Review + create**, then select **Create**.
1.  Once storage accounts provisions, select **Go to resource**.
1.  On the **Overview** page, select **File shares** tile.
1.  Select **+File share**.
    -  Enter a **Name**, for example **uplfolder**.
    -  Enter an appropriate **Quota**, or leave the field blank for no quota.
1.  Select **Create**.

For more detail about setting up Standard and Premium Azure Files, refer to these documents:
[Standard](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-how-to-create-large-file-share?tabs=azure-portal)
 and [Premium](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-premium-fileshare).

For further details, refer to [Create an Azure file share](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share?tabs=azure-portal).

## Step 3: Enable Azure Files AD Authentication

Use the instructions in this section to enable Azure Files AD Authentication. You need to run these commands from any machine that is already domain-joined. This action is a one-time task. The VM used to run the process is not needed for the solution once the task is complete.

1.  Use Remote Desktop Protocol (RDP) to connect to the **domain-joined** virtual machine.
2.  To install the **AzFilesHybrid** module and enable authentication, follow the instructions in [Enable AD DS authentication for your Azure file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-enable).

Before proceeding to the next step, validate that Azure Files AD Authentication is enabled as follows:

1.  Open the Azure portal.
1.  Open your **storage account** that is tied to your Azure Files.
1.  Under **Setting**, select **Configuration**, and confirm that Active Directory (AD) is set to **Enabled**.

## Step 4: Assign share level and NTFS permissions

Before assigning user personalization layers and profiles to users and groups, configure the appropriate access to the Azure Files file share.

>**Important:**
>
>The accounts or groups to which you assign permissions must be created in the domain and synchronized with Azure AD. Accounts created in Azure AD are not supported.

### Assign share level permissions to users

The following section describes how to set the share level permissions:

1.  Open the Azure portal.
1.  Open the **storage account** you created in **Step 2**.
1.  Select **Access Control (IAM)**.
1.  Select **Add a role assignment**.
1.  In the **Add role assignment** tab, select **Storage File Data SMB Share Elevated Contributor** for the Share administrator account.
1.  Then select **Storage File Data SMB Share Contributor** for the users or groups that are assigned user personalization layers and profiles.
1.  Select **Save**.

The permissions can take up to 30 minutes before they fully take effect. Give it some time before you proceed to next step.

For details, refer to the [Assign share-level permissions to an identity](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-assign-permissions).

### Configure your NTFS permissions on the shared folder

Once you have assigned your file share permissions, configure your NTFS permissions.

To configure directory and file level NTFS permissions:

1.  Open the Azure portal.
1.  Open the **storage account** you created in step 3.
1.  Click the **File Share** tile
1.  Click the share name you created, for example **uplshare**.
1.  Click **Properties**.
1.  Copy the URL link.
1.  After copying the URL, convert it into the **UNC** format:
    -  Remove `https://`.
    -  Replace all forward slashes `//` with back slashes `\\`. For example:  
      `https://uplshare.file.core.windows.net/uplfolder` becomes `\\uplshare.file.core.windows.net\uplfolder`.
1.  Using RDP, connect to a virtual machine that is domain joined.
1.  Open a command prompt, and run the following cmdlet to mount the Azure file share and assign it a drive letter:
    `net use <drive-letter> UNC-path`
    Example: `net use S:\ \\uplshare.file.core.windows.net\uplfolder`
1.  Once the share is mounted, set the following permissions on the mounted share.

| Setting name | Value | Apply to |
|---|---|---|
| Creator Owner|Modify | Subfolders and Files only |
| Owner Rights | Modify | Subfolders and Files only |
| Users or group: | Create Folder/Append Data; Traverse Folder/Execute File; List Folder/Read Data; Read Attributes | Selected Folder Only |
| System | Full Control | Selected Folder, Subfolders, and Files |
| Domain Admins, and selected Admin group | Full Control | Selected Folder, Subfolders, and Files |

## Set up the User personalization layers and profiles

Next, you can configure User personalization layers and profiles. Follow the instructions for [deploying User personalization layers](/en-us/citrix-virtual-apps-desktops/install-configure/user-personalization-layer.html) and [Profile Management quick start guide](/en-us/profile-management/current-release/quick-start-guide.html). Use the UNC path described in **Step 4** for the **User Layer Repository Path**.
