---
layout: doc
h3InToc: true
contributedBy: Thamara Trejos
specialThanksTo: Amir Trujillo, Nitin Mehta, Daniel Feller, Mark Hoffman, Bala Swaminathan
description: This Proof of Concept guide provides instructions on using an Automated Configuration tool to automate moving your Citrix Virtual Apps and Desktops configuration to your Citrix Virtual Apps and Desktops Service deployment. The tool also supports the use case of moving your configuration between Citrix Virtual Apps and Desktops Service deployments.
tz_title: Automated Configuration Tool
tz_products: citrix-virtual-apps-and-desktops;
---
# PoC Guide: Automated Configuration Tool

## Overview

The [Automated Configuration tool](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html) facilitates migrating and exporting configurations to the **Citrix Virtual Apps and Desktop Service** (CVADS). This Proof of Concept guide illustrates the step by step instructions on how to use this tool.

Administrators can easily test and explore the **Citrix Virtual Apps and Desktop Service** (CVADS) features and advantages, while simultaneously running existing On-Premises environments and even facilitate moves between cloud regions, back up existing configurations, and other use cases. The [Automated Configuration download link](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html) also contains **additional information** and [**detailed documentation**](/en-us/citrix-virtual-apps-desktops-service/migrate.html) on said **use cases** and **customizations**.

### What is the Automated Configuration tool for Citrix Virtual Apps and Desktops?

This tool is designed to help automate the migration of **CVAD** configuration (policies, applications, catalogs, admin roles, scopes and others) from one or more On-Premises sites to the **Citrix Virtual Apps and Desktop service** (CVADS) hosted on Citrix Cloud. It can also be used to migrate information between **different Cloud regions** or **tenants**.

The migration can be performed in stages by running the tool multiple times, allowing administrators to easily achieve the desired configuration state. This [**2-minute video**] provides a quick tour of the Automated Configuration tool.

### Why use this tool?

IT Administrators in charge of large or complex environments often find migrations to be a tedious process. They frequently end up writing their own tools to accomplish this task successfully since it tends to be specific to their use cases.

Citrix wants to help ease this process by providing a tool that addresses use cases through automation. Administrators can easily test current configurations in **Citrix Cloud** and take advantage of the benefits offered by **CVADS** while keeping their current environments intact. Such benefits include reduced administrative overload when Citrix manages part of the back-end and control plane, automatic and customizable **Citrix Cloud** component updates, and others.

### How is this tool implemented?

Citrix uses industry standard configuration as code to provide a mechanism to help automate migration processes. This tool discovers and exports one or more on-premises sites as **a collection of configuration files**, which administrators can optionally edit. These files' configuration can then be imported into CVADS.

This code is not limited to migrations, it is the future for creating configuration for Citrix sites, and as such, applicable for **many different use cases**. Disaster recovery, Development/Testing/Staging to Production site synchronization, Geographic (GEO) moves, and several other scenarios are supported. For administrators using public cloud providers, this can help create a combination of objects automatically (parallel to Microsoft Azure ARM templates and AWS CloudFormation).

The tool also allows administrators to **merge multiple on-premises sites into a single site**, while avoiding name collisions. Administrators can control whether the On-Premises or Cloud controls resources. Files can now be placed **in a secure network file share** that requires authentication when using the **SecurityFileFolder** parameter which points to the *CvadAcSecurity.yml* file.

## Pre-requisites

### On-Premises environment

*  Citrix Virtual Apps and Desktops (CVAD) On-Premises environment with **at least one registered VDA**.
*  CVAD On-Premises environment running on one of the following versions: Any **Long Term Service Release (LTSR)** versions with their latest CU (7.6, 7.15, 1912); Or one of the corresponding latest two **Current Releases (CR)** versions (for example: 2003, 2006).
*  The domain-joined machine where you plan on running the **Automated Configuration tool commands** must be running **.NET 4.7.2 version or higher**.
*  A machine with the **Citrix PowerShell SDK**, which is automatically installed on the DDC. **Note:** If running the tool on a different machine, it must be domain-joined and **Studio** must have the correct **PowerShell snap-ins** installed. This installer can be found on your corresponding version’s **Product ISO installation media**, which can be obtained from the [**Citrix Downloads > Citrix Virtual Apps and Desktops**](https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/) website.
*  Download the Automated Configuration tool MSI from the [Official Downloads website](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html)

### Cloud-related components

*  Valid **CVADS** or **Workspace Premium Plus** licenses.
*  Administrator must be able to log into the [Cloud Portal](https://citrix.cloud.com) and obtain: The **resource location name**, **customer ID**, **client Secret** (**app ID** and **Secret Key**)
*  The existing Citrix Cloud **Resource Location** has at least **one Cloud Connector**, which is marked as green (Healthy) and is part of the same domain as the On-Premises setup. **Note:** Citrix recommends having **two or more Cloud Connectors** (for redundancy and High Availability). For information on how to set up your Cloud Connectors, refer to [this guide](/en-us/tech-zone/learn/poc-guides/cvads.html).

### This proof of concept guide demonstrates how to

1.  [Complete the On-Premises pre-requisites](#complete-pre-requisites-for-exporting-from-on-premises-site)
2.  [Export your On-Premises site configuration into YAML (*.yml*) files](#export-your-on-premises-site-configuration)
3.  [Complete the cloud pre-requisites](#complete-prerequisites-in-cloud)
4.  [Complete the requisites for importing site configuration when using different provisioning methods (Provisioning Services (PVS) and Machine Creation Services (MCS))](#dealing-with-provisioning-services-pvs-machine-catalogs-delivery-and-application-groups-and-policies)
5.  [Import your Site Configuration into Cloud (by editing the required files)](#import-your-site-configuration-into-cloud)
6.  [Troubleshooting tips and where to find more information](#troubleshooting-tips)

### Complete Pre-requisites for Exporting from on-premises site

These steps must be run in your DDC or the domain-joined machine where you want to run the **Automated Configuration** tool.

1.  Download the latest **Automated Configuration tool MSI** to your **On-Premises DDC** or a domain-joined machine. **Note:** See the **Pre-requisites section** for more details on how to run it from a different machine. The tool can be downloaded from [here](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html).
**Note:** See the [Pre-requisites section](#pre-requisites) for more details on how to run it from a different machine.
2.  Run the **MSI** on your **On-Premises DDC**, by right-clicking on the **AutoConfig_PowerShell_x64.msi** installer and clicking on **Install**. [![Pre-requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-001.png)
3.  Read the **License Agreement** and check the box if you accept the terms. Then click **Install**: [![Pre-requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-002.png)
4.  Files are copied and the progress bar continues moving until it finishes the install. [![Pre-requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-003.png)

5.  After the **MSI** runs, a window indicating successful completion pops up. Click **Finish** to close the **MSI setup** window. [![Pre-requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-install-004.png)

*  **Note**: Upon successful execution, the **MSI** creates the corresponding folder structure, located in ```C:\Users\<username>\Documents\Citrix\AutoConfig```, and also a desktop icon called **Auto Config**, which launches a PowerShell command prompt. This tool is the one used on subsequent steps.

## Export your On-Premises site configuration

Using an ```export``` PowerShell command, you can export your existing On-Premises configuration and obtain the necessary *.yml* files. These files are used to import your desired configuration into **Citrix Cloud**.

1.  After running the **MSI** installer on the previous step, you get an **Auto Config** shortcut automatically created on the Desktop. Right-click this shortcut and click **Run as Administrator.**
2.  Run the ```Export-CvadAcToFile``` command. This command exports policies, manually provisioned catalogs, and delivery groups. It also exports applications, application folders, icons, zone mappings, tags, admin roles and scopes, and other items. **Note:** For **MCS** and **PVS** machine catalogs and delivery groups, refer to the steps on [Requisites for Importing Site Configuration using different Provisioning Methods section](#requisites-for-importing-site-configuration-using-different-provisioning-methods) in this guide.
[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002-1.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002-1.png)

3.  Once the tool finishes running, the **Overall status** shows as **True** and the export process is completed (the output lines shown match the following illustration). **Note:** If there are any errors, diagnostic files are created in the action-specific subfolders ```(Export, Import, Merge, Restore, Sync, Backup, Compare)```, which can be found under ```%HOMEPATH%\Documents\Citrix\AutoConfig```. Refer to the [Troubleshooting Tips section](#troubleshooting-tips) if you encounter any errors.
[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)

4.  The resulting *.yml* files are now in the current user’s ```Documents\Citrix\AutoConfig``` path:
[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-004.png)

*  **Note:** See the following image for an example of the contents on a *.yml* file (```Application.yml```)

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-005.png)

*  **Note:** If necessary, copy the *.yml* files to the machine you want to use to import settings to your **Citrix Cloud** environment. Exporting and importing can be done from the same machine.

## Complete Prerequisites in Cloud

Go to your **Resource Location** and make sure that your **Cloud Connectors** are both showing green (Available). **Note:** If you need instructions on how to set up your **Cloud Connectors**, refer to [this guide](/en-us/tech-zone/learn/poc-guides/cvads.html).

1.  To verify the health status of your **Cloud Connectors**, first log in to your [cloud portal](https://citrix.cloud.com) with your Citrix administrator credentials (or your Azure AD credentials, when applicable).
[![Cloud Prerequisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-001.png)

2.  If you have more than one **Organization ID** (Org ID), select your corresponding tenant.
[![Cloud Prerequisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-002.png)

3.  Upon logon, go to the **hamburger menu** on the top left corner and then click **Resource Locations**:
[![Cloud Prerequisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-003.png)

4.  Access the **Cloud Connector(s)** tile under your **Resource Location**.
[![Cloud Prerequisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-004.png)

*  **Note:** The **Cloud Connector(s)** must show green, indicating a Healthy status, as seen on the following image. Citrix recommends having more than one **Cloud Connector** per **Resource Location**, for redundancy purposes.

[![Cloud Prerequisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-005.png)

## Requisites for Importing Site Configuration using different Provisioning Methods

### Dealing with Provisioning Services (PVS) Machine Catalogs, Delivery and Application Groups and Policies

Extra steps are required to import your **PVS Catalogs** and their corresponding applications. Follow these instructions to prepare your environment, before proceeding to import the **Application** settings.

**Note:** After performing these actions, follow the steps mentioned on the
[Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

1.  Run **PowerShell** as an Administrator
2.  Type the following command ```Export-CvadAcToFile``` and press **Return (Enter)** on your keyboard.
[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-001.png)

3.  Successfully completed items show in green ```OK```. Once the tool finishes running, a screen like the following shows up:
[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-002.png)

4.  Access the ```C:\Users\<username>\Documents\Citrix\AutoConfig``` folder and open up the ```CvadAcSecurity.yml``` file, which you must edit for PVS to work. **Note:** The **UserName** and **Password** fields refer to your **PVS Site Server** credentials. Be sure to specify your logon as ```DOMAIN\Username```, add the password, and save the file when ready.
[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-003.png)

*  After performing these actions, follow the steps mentioned on the [Import your Site Configuration into Cloud](#import-your-site-configuration-into-cloud) section in this guide.

### Dealing with Machine Creation Services (MCS): Pooled VDI multi-session (Random) and RDS Machine Catalogs (PREVIEW)

**Note:** A separate section is available with instructions for Static assigned virtual machines. Refer to the steps mentioned on the [MCS Static Assigned VDIs](#dealing-with-machine-creation-services-mcs-static-assigned-machines) section in this guide.

The import and export commands are supported for this task now. Both the golden image and the configuration in Catalogs with **User data: Discard** can be migrated. However the virtual machines in these catalogs do not get migrated, since the site you are importing from is responsible for maintaining the life cycle of the virtual machine.

When machines are turned on, their state might change, affecting import data for the virtual machines synchronization. Therefore when migrating these catalogs using the tool, it creates a catalog metadata and initiates master image creation. However zero machines are imported.

**Important Considerations:**

*  Since this process is still in preview, it is not automatically enabled. You must contact Citrix support or your Customer Success Management team to enable this feature for your org ID.
*  The MCS catalog import process can take a couple of hours based on the size of the master image. Therefore the import command within the tool only starts the MCS catalog creation and does not wait for it to finish.
*  After the import has completed, the catalog creation progress can be monitored via Studio in the cloud deployment.
*  Once the master image is created you can provision machines. Consider your hypervisor's existing capacity, since you have consumption from your on-premises usage.
*  All other objects (including the Delivery Group, applications, policies, and everything that uses the catalog) can be imported, without having to wait for the master image creation. The same commands available within the tool can be used to migrate catalogs and all other objects.
*  When the catalog has finished creating, machines can be added to the imported catalog, and then users can launch their resources.

**Note:** After these considerations, follow [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide to merge your configuration as needed.

## Dealing with Machine Creation Services (MCS): Static Assigned Machines (PREVIEW)

**Note:** A separate section is available with instructions for Pooled and RDS Machines. Refer to the steps mentioned on the [MCS Pooled VDI and RDS Machines](#dealing-with-machine-creation-services-mcs-pooled-random-and-rds-machine-catalogs)

The import and export commands are supported for this task now. This process imports some low-level details which are stored in the database so **it needs to be run from a machine with database access**. The tool import process migrates the configuration, master image and the machines as well. It is a quick operation since no images are created.

**Important Considerations:**

*  Since this process is still in preview, it is not automatically enabled. You must contact Citrix support or your Customer Success Management team to enable this feature for your org ID.
*  The VDAs **need** to be pointed to the Cloud Connectors for them to register with **Citrix Cloud**.
*  Refer to the [**Activating sites**](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/migrate.html#activating-sites) documentation to activate your Cloud site and thus control reboot schedule, power management, and others, via **Citrix Cloud**.
*  Once the migration is completed, if you want to delete the corresponding catalog from your **on-premises site**, you **must** select the option to **leave VM and AD account**. Otherwise both records will be deleted and the Cloud site left pointing to the deleted virtual machine.

**Note:** After these considerations, follow [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide to merge your configuration as needed.

## Import your Site Configuration into Cloud

During this step, you obtain the **customer connection details**, manually create your **Zone mappings,** and **import the configuration** to your Cloud tenant.
**Note:** For **PVS** and **MCS**, first follow the corresponding subsections under the [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

### Obtaining Customer Connection Details

Administrators must edit the ```CustomerInfo.yml``` file and add the corresponding **CustomerName**, **CustomerID,** and **SecretKey** values to it. These values can be obtained and generated from the **Cloud portal**, as shown in the following steps.

1.  First, open your ```CustomerInfo.yml``` file using a text editor application, such as **Notepad**. The following screenshot shows the ```CustomerInfo.yml``` file values that must be edited (underlined in red):
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-001.png)

2.  On your [Cloud portal](https://citrix.cloud.com) click the **hamburger menu** again and go to **Identity and Access Management**:
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-002.png)

3.  Go to the **API Access** tab and copy the **Customer ID** value, which can be found next to the ```customer ID``` text as seen on the following screenshot (red rectangle):
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-003.png)

4.  Paste the retrieved value **between the quotes** that follow the **CustomerId field** in your ```CustomerInfo.yml``` file, between the ```“”``` (quotes):
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-004.png)

5.  Back on your **Cloud portal**, go to the **Identity and Access Management** portal and **API Access** tab. Enter the name you want to identify this API key with on the **Name your Secure Client** box. Then click the **Create Client** button. **Note:** This action generates the ```Client ID``` and the ```Secret Key```.
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-005.png)

6.  Copy the ```ID``` and the ```Secret``` values, one by one (paste them on the ```CustomerInfo.yml``` file as shown in the following step). Then click **Download** to save the file for later reference.
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-006.png)

7.  Paste the ```ID``` and ```Secret``` values onto the corresponding fields in the ```CustomerInfo.yml``` file:
![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-007.png)

### Manually create the Zone Mapping file (ZoneMapping.yml)

**On-Premises Zones** cannot be automatically migrated to a **cloud Resource Location**, so they must be mapped using the ```ZoneMapping.yml``` file. **Note:** Migration failures occur if the zone is not mapped with a homonymous resource location (a Resource Location with the **exact same name**).

1.  Back in the same directory where your *.yml* files reside ```(Documents\Citrix\AutoConfig)```, open up the ```ZoneMapping.yml``` using **Notepad** or your preferred text editor. **Note:** The ```Primary``` value must be replaced with the name of your corresponding **Zone** you want to migrate objects from (in your On-Premises environment).
[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-001.png)

2.  You can find this name under your **On-Premises Citrix Studio console > Configuration > Zones**. **Note:** if your Zone is named ```Primary``` in your On-Premises environment, this value on the ```ZoneMapping.yml``` file doesn’t need to be changed:
[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-002.png)

3.  Still on the ```ZoneMapping.yml``` file, the ```Name_Of_Your_Resource_Zone``` value must be replaced with your **Cloud Resource Location name**. This value can be found on your [cloud portal](https://citrix.cloud.com) under the Hamburger menu > **Resource Locations**:
[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-003.png)

4.  Copy your **Resource Location name** (Shown as ```My Resource Location``` on the following screenshot):
[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-004.png)

1.  Paste this value on the ```ZoneMapping.yml``` file instead of the ```Name_Of_Your_Resouce_Zone``` value:
[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-005.png)

*  **Note:** Multiple Zones in your On-Premises environment can also map to **one Resource Location** in the cloud. However there always must be one row in the file for **each zone** in the **On-Premises environment**. For **multiple zones on-premises** and **one Resource Location**, the format on this file would look as follows:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-006.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-006.png)

When mapping **Zones** to different **Resource locations**, the file must look like this instead:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-007.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-007.png)

### Merging configuration

1.  Back on the **Migration tool PowerShell console**, run the following command: ```Merge-CvadAcToSite``` to merge the existing Cloud configuration (if any exists) with the configuration exported from the On-Premises site.
[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-001.png)

2.  When each task runs successfully, the output looks green as *.yml* files are imported and the corresponding components are added to the cloud site:
[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-002.png)

3.  The resulting files show in the following directory: ```<This PC>\Documents\Citrix\AutoConfig\Import_<YYYY_MM_DD_HH_mm_ss>```
[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-003.png)

4.  On this same folder, you can find a ```Backup_YYYY_MM_DD_HH_mm_ss``` folder. **Note:** Copy this folder somewhere safe as it is a backup of the configuration.

5.  The ```Backup``` folder contains the following files, which are helpful for reverting changes, if needed:
[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-004.png)

### Verify configuration created in Cloud Studio

1.  Access your Virtual Apps and Desktops service **Manage** tab via the [Cloud Console](https://citrix.cloud.com) > **My Services > Virtual Apps and Desktops service > Manage tab**).
[![Verifying Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-001.png)

1.  Refresh to make sure that the **Machine catalogs**, **Delivery groups**, **policies**, **tags,** and **applications** are now showing as expected. **Note:** Depending on what you import, the results vary as they are specific to your own unique configuration. Review **each section** to confirm that the expected items are listed.

*  **Machine Catalogs list example:**

[![Verifying Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-002.png)

If everything looks as expected, your CVADS migration is complete.

## Troubleshooting Tips

**General information for troubleshooting:**

*  Running any cmdlet creates a **log file** and an entry in the **master history log file**. The entries contain the date, operation, result, backup, and log file locations of the execution. This log provides potential solutions and fixes to common errors.
*  The **master history log** is located in ```%HOMEPATH%\Documents\Citrix\AutoConfig```, in the file named ```History.Log```.*  All operation log files are placed in a **backup folder**.
*  All log file names begin with ```CitrixLog```, then show the ```auto-config``` operation and the **date** and **timestamp** of the cmdlet execution.
*  Logs **do not** auto-delete.
*  Console logging can be suppressed by using the ```-quiet``` parameter
*  New support cmdlet to zip all log files for transfer to Citrix for support. Back up the cloud’s current state by running the ```Backup-CvadAcToFile``` command. Then collect all log and ```*.yml``` files into a single zip by running ```New-CvadAcZipInfoForSupport```. **No customer security information is included.** Forward the zip file at the following location ```%HOMEPATH%\Documents\Citrix\AutoConfig\CvadAcSupport_yyyy_mm_dd_hh_mm_ss.zipNew-CvadAcZipInfoForSupport```.

**For more information:**

1.  Refer to the [Automated Configuration Tool Troubleshooting FAQ article](https://support.citrix.com/article/CTX277730).

2.  You can also reach out via the [Support Forum](https://discussions.citrix.com/forum/1804-automated-configuration-for-virtual-apps-and-desktops-tech-preview/).

3.  Check out our On-demand **August 19 webinar recording** - ["Why Citrix Cloud migration is easier than ever"](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/form/technology-in-practice-webinar-august-2020/). We shared more information on the tool and hosted a **Live Q&A session** with a panel of Citrix experts.

4.  If after consulting the information listed previously you still need assistance, get in touch with your Citrix representatives, Customer Success Manager, or Support.
