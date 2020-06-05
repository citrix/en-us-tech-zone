---
layout: doc
description: Copy & paste description from TOC here
---
# Set up Azure Files storage for User personalization layers

## Contributors

**Author:** [Elaine Welch](mailto:Elaine.Welch@citrix.com), [Loay Shbeilat](mailto:loay.shbeilat@citrix.com) 

Azure Files offers SMB access within Azure storage accounts. Using SMB, you can mount an Azure file share on Windows, Linux, or macOS, either on premises or in cloud virtual machines, without writing any code or attaching any special drivers to the file system.

Azure Files now supports on-premises Active Directory Domain Service Authentication, which enables User personalization layers to use Azure Files.

For full details, see [on-premises Active Directory Service Authentication over SBM for Azure File shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-auth-active-directory-enable).

## Requirements

In addition to the [User personalization layer requirements](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/user-personalization-layer.html), Azure Files requires that your on-premises domain controller is synchromized to your Azure Active Directory.

## Overview

Before you set up User personalization layers, set up Azure Files using the following steps:

-  Step 1: Sync AD to Azure AD (Prereq for Azure Files) 
-  Step 2: Create Azure Files share
-  Step 3: Enable Azure Files AD DS Authentication
-  Step 4: Assign share level and NTFS permissions

## Step 1: Sync Azure AD with your on prem AD

To use Azure Files with AD Authentication, you will need to [sync your on prem AD with Azure AD, using Azure AD Connect](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-roadmap).

>IMPORTANT:
>
>-  The Azure AD tenant and the file share that will be used for UPL must be associated with the same subscription.
>-  The accounts being used must be created in the domain controller and synched to Azure AD. Accounts sourced from Azure AD are not appropriate.

After the sync, please give it some time for Users and Groups to be replicated to Azure AD before you proceed.

## Step 2: Create Azure Files storage

This procedure explains how to create an Azure Files file share for storing your user layers.

Currently, there are two tiers of Azure Files, Stanadrd and Premium. Choose the appropriate tier based on your performance requirements. For more about Azure Files performance, refer to [Azure Files scalability and performance targets](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-scale-targets#file-share-and-file-scale-targets).

This document explains how to set up Standard storage as an example.

1.  In the Azure Portal click **Create a resource**.
1.  Select **Storage account – blob, file, table, queue**.
1.  Enter the following information into the **Create storage account** page:
    -  Create a new **resource group**.
    -  Enter a unique name for your **storage account**.
    -  For **Location**, we recommend you choose the same location as the Virtual Delivery Agents (VDA’s) in the Azure resource location.
    -  For **Performance**, select **Standard**. (example choice)
    -  For **Account type**, select **StorageV2**.
    -  For **Replication**, select **Locally-redundant storage (LRS)**.
1.  When you're done, select **Review + create**, then select **Create**.
1.  Once storage accounts provisions, select **Go to resource**.
1.  On the **Overview** page, select **File shares** tile.
1.  Select **+File share**, create a new file share, for example **uplfolder**, then either enter an appropriate **Quota** or leave the field blank for no quota.
1.  Select **Create**.

For more detail about setting up Standard and Premium Azure Files, refer to these documents:
[Standard](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-how-to-create-large-file-share?tabs=azure-portal)
 and [Premium](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-premium-fileshare).

For further details, refer to [Create an Azure file share](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share?tabs=azure-portal).

## Step 3: Enable Azure Files AD Authentication

Use the instructions in this section to enable Azure Files AD Authentication on a machine that's already domain-joined. 

1.  Use Remote Desktop Protocol (RDP) to connect to the **domain-joined** virtual machine.
1.  To install the **AzFilesHybrid** module and enable authentication, follow the instructions in [Enable Azure AD DS authentication for your Azure file shares](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-enable).

Before proceeding to the next step, validate that Azure Files AD Authentication is enabled as follows:

1.  From the Azure portal, open your storage account that is tied to your Azure Files.
1.  Under **Setting**, select **Configuration**, and confirm that Active Directory (AD) is set to **Enabled**.

## Step 4: Assign share level and NTFS permissions

Before assigning user personalization layers to users and groups, configure the appropriate access to the Azure Files file share. 

>**Important:**
>
>The accounts or groups you assign permissions to should have been created in the domain and synchronized with Azure AD. Accounts created in Azure AD won't work.

### Assign share level permissions to users

The following section describes how to set the share level permissions:

1.  Open the Azure portal.
1.  Open the storage account you created in the section above.
1.  Select **Access Control (IAM)**.
1.  Select **Add a role assignment**.
1.  In the **Add role assignment** tab, select **Storage File Data SMB Share Elevated Contributor** for the Share administrator account.
1.  Then select **Storage File Data SMB Share Contributor** for the users or group that will be using the user personalization layers.
1.  Select **Save**.

The permissions can take up to 30 minutes before they fully take effect. Please give it some time before you proceed to next step.

For additional details, refer to the [Assign share-level permissions to an identity](https://docs.microsoft.com/en-us/azure/storage/files/storage-files-identity-ad-ds-assign-permissions).

### Configure your NTFS permissions on the shared folder

Once you have assigned your file share permissions, configure your NTFS permissions. 

To configure directory and file level NTFS permissions:

1.  Open the Azure portal.
1.  Open the storage account you created in step 3.
1.  Click the **File Share** tile
1.  Click the share name you created, for example **uplshare**.
1.  Click **Properties**.
1.  Copy the URL link.
1.  After copying the URL, convert it into the **UNC** format:
    1.  Remove `https://`.
    1.  Replace all forward slashes `//` with back slashes `\\`. For example:
       `https://uplshare.file.core.windows.net/uplfolder` becomes
       `\\uplshare.file.core.windows.net\uplfolder`.
1.  Using RDP, connect to a virtual machine that is domain joined.
1.  Open a command prompt, and run the following cmdlet to mount the Azure file share and assign it a drive letter:
     `net use <drive-letter> UNC-path`
     Example: `net use S:\ \\uplshare.file.core.windows.net\uplshare`
1.  Once the share is mounted, set the following permissions on the mounted share.

| Setting name | Value | Apply to |
|---|---|---|
| Creator Owner|Modify | Subfolders and Files only |
| Owner Rights | Modify | Subfolders and Files only |
| Users or group: | Create Folder/Append Data; Traverse Folder/Execute File; List Folder/Read Data; Read Attributes | Selected Folder Only |
| System | Full Control | Selected Folder, Subfolders, and Files |
| Domain Admins, and selected Admin group | Full Control | Selected Folder, Subfolders, and Files |

This completes the Azure Files configuration for user personalization layers.

## Set the user personalization layers

Once you configure Azure Files storage for Upl, follow the instructions for [deploying User personalization layers](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/install-configure/user-personalization-layer.html), using the UNC path described in the previous procedure for the **User Layer Repository Path**.
