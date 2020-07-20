---
layout: doc
description: Copy & paste description from TOC here
---
# Proof of Concept: Citrix Automated Configuration (Auto Config) tool

## Contributors

**Author:** [Thamara Trejos](linkedin.com/in/thamaratrejos/)

**Special Thanks:** [Amir Trujillo](linkedin.com/in/amirtrujillo), Nitin Mehta, [Daniel Feller](https://twitter.com/djfeller)

## Overview

The Citrix Automated Configuration (Auto Config) tool facilitates migrating and exporting configurations to the Citrix Virtual Apps and Desktop Service (CVADS). This guide illustrates the step by step instructions on how to leverage this tool.

Administrators can easily test and explore the Citrix Virtual Apps and Desktop Service (CVADs) features and advantages, while simultaneously running existing On-Premises environments, as well as facilitating moves between cloud regions and other use cases.

### What is the Citrix Automated Configuration tool (Auto Config) for Virtual Apps and Desktops?

This tool is designed to help automate the migration of Citrix Virtual Apps and Desktop configuration (such as policies, applications, catalogs, and others) from one or more on-premises site(s) to the Citrix Virtual Apps and Desktop service (CVADs) hosted on Citrix Cloud. It can also be used to migrate information between different Cloud regions or tenants.

The migration can be performed in stages when executing the tool multiple times, allowing administrators to achieve the desired configuration state between On-Premises site(s) and Cloud, or between different Cloud regions.

### Why use the Auto Config tool?

IT Administrators, particularly those dealing with large environments, often find migration to be a very tedious process. As it tends to be specific to their use cases, they frequently end up writing their own tools to accomplish this task successfully.

Citrix wants to help ease this process by providing a tool that addresses customer use cases by means of automation. Customers can now take advantage of the benefits that CVADs has to offer, such as a reduced administrative overload when managing the control plane, and be able to utilize their existing policies and applications.

### How is the Auto Config tool implemented?

Citrix has leveraged industry standard configuration as code to provide a mechanism to help automate the migration process. Essentially, with this tool, we discover and export one or more on-premises sites as a collection of configuration files (which customers can optionally edit) and then import these edited files into CVADs.

This code is not limited to migration: it’s the future for creating configuration for Citrix sites, as well for use cases such as Disaster recovery, Development/Testing/Staging to Production site synchronization, Geographic (GEO) moves, creating a combination of objects automatically (parallel to Microsoft Azure ARM templates / AWS Cloud formation), and several other scenarios.

## Pre-requisites

### On-Premises environment

*  CVAD On-Premises environment with at least one registered VDA.
*  CVAD On-Premises environment is running on one of the following versions: Any **Long Term Service Release (LTSR)** versions with their latest CU (7.6, 7.15, 1912); Or one of the corresponding latest two **Current Releases (CR)** versions (for example: 2003, 2006).
*  The domain-joined machine where the Citrix Auto Config tool will be installed is running .NET 4.7.2 version or higher.
*  A machine with the Citrix PowerShell SDK, which is automatically installed on the DDC. **Note:** if running on a different machine, it needs to be domain-joined and Studio must have the correct PowerShell snap-ins installed. This installer can be found on your corresponding version’s Product ISO installation media, which can be obtained from the [Citrix Downloads > Citrix Virtual Apps and Desktops](https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/) website.

### Cloud-related components

*  Valid CVADs or Workspace Premium Plus licenses.
*  Administrator can log into the Cloud Portal and obtain: The resource location name, customer ID, client Secret (app ID and Secret Key)
*  The existing cloud Resource Location has at least 1 Cloud Connector, which is marked as green (Healthy) and part of the same domain as the On-Premises setup. **Note:** Citrix recommends having 2 or more Cloud Connectors (for redundancy and High Availability). For information on how to set up your Cloud Connectors, please refer to [this guide]([https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/cvads.html).

### This proof of concept guide demonstrates how to

1.  Complete the On-Premises pre-requisites
2.  Export your On-Premises site configuration into YAML files
3.  Complete the cloud pre-requisites
4.  Complete the requisites for importing site configuration when using different provisioning methods (PVS and MCS)
5.  Import your Site Configuration into Cloud (by editing the required files)
6.  Troubleshooting tips and where to find more information

### Complete Prerequisites for Exporting from on-premises site

These steps must be run in your DDC or domain-joined machine where you wish to install the Auto Config tool.

1.  Download the Auto Config tool to your On-Premises DDC or a domain-joined machine. **Note:** See the [Pre-requisites section](#pre-requisites) for more details on how to run it from a different machine.

2.  Run the **MSI** on your On-Premises DDC, by right clicking on the **AutoConfig_PowerShell_x64.msi** installer and clicking on **Install**.

[![On Prem Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-001.png)

3.  Read the License Agreement and check the box to accept the terms if you accept them and then click on **Install**:

[![On Prem Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-002.png)

4.  After the MSI runs, a window indicating successful completion pops up. Click on **Finish** to close the MSI Setup window. **Note:** Upon successful execution, the MSI will create the corresponding folder structure, located in ```C:\Users\<username>\Documents\Citrix\AutoConfig``` as well as a desktop icon called **Auto Config** which launches a PowerShell command prompt. We will use this **Auto Config** tool on the subsequent steps.

[![On Prem Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_on-prem-pre-requisites-003.png)

## Export your On-Premises site configuration

Using an export PowerShell command, you can export your existing On-Premises configuration and obtain the necessary *.yml* files which will be used to import your desired configuration into Citrix Cloud.

1.  After running the MSI installer on the previous step, you will get an **Auto Config** shortcut automatically created on the Desktop. Right click this shortcut and click on **Run as Administrator**:

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-001.png)

2.  The Auto Config PowerShell tool will open and the current directory will point to the tool’s default path (```C:\Users\<username>\Documents\Citrix\AutoConfig```). **Note:** This is also where the *.yml* files will be created.

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002.png)

3.  On the **Auto Config** Command Prompt, run the ```Export-CvadAcToFile -all $true``` command. This command will export policies, Manually provisioned catalogs and Delivery groups, applications, application folders, icons, zone mappings, tags and other items in the infrastructure.
**Note:** For MCS and PVS catalogs and delivery groups, refer to the [Requisites for Importing Site Configuration using different Provisioning Methods section](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html##Requisites) in this guide.

4.  Once the tool finishes running, the **Overall status** shows as **True** and the export process will be completed. You will see several output lines like the ones on this illustration:

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)

*  **Note:** If there are any  errors, diagnostic files are created in the action-specific subfolders ```(Export, Import, Merge, Restore, Sync, Backup, Compare)```, which can be found under ```%HOMEPATH%\Documents\Citrix\AutoConfig```. Please get in touch with your Success Management designated team and provide these files for problem resolution.

5.  The resulting *.yml* files will be in the current user’s ```Documents\Citrix\AutoConfig``` path and will look as follows:

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-004.png)

*  **Note:** See below for an example of the contents on a *.yml* file (```Application.yml```)

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-005.png)

*  **Note:** If necessary, copy *.yml* files to the machine you will use to import to the Cloud environment. Export and import can be done from the same machine.

## Complete Prerequisites in Cloud

Go to your **Resource Location** and make sure your **Cloud Connectors** are both showing green (Available). **Note:** If you need instructions on how to set up your Connectors, please refer to [this guide](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/cvads.html).

1.  To verify the health status of your Cloud Connectors, first log onto your [cloud portal](https://citrix.cloud.com) with your Citrix credentials.

[![Cloud Pre Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-001.png)

2.  If you have more than one Organization ID (Org ID), select your corresponding tenant

[![Cloud Pre Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-002.png)

3.  Upon logon, click on the hamburger menu and then click on Resource Locations:

[![Cloud Pre Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-003.png)

4.  Click on the Cloud Connector(s) tile under your Resource Location.

[![Cloud Pre Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-004.png)

*  **Note:** The Cloud Connector(s) should show green, indicating a Healthy status as seen on the image below. Citrix recommends having more than one Cloud Connector per Resource Location, for redundancy.

[![Cloud Pre Requisites](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_cloud-pre-requisites-005.png)

## Requisites for Importing Site Configuration using different Provisioning Methods

### Dealing with Provisioning Services (PVS) Machine Catalogs, Delivery and Application Groups and Policies

Additional steps are required in order to import your PVS Catalogs and their corresponding applications. Please follow the steps below in order to prepare your environment, before proceeding to import the Application settings.

**Note:** After executing these actions, please follow the steps mentioned on the
[Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

1.  Run the **Auto Config tool** as an Administrator
2.  Type the following command ```Export-CvadAcToFile -all $true``` and hit Return (Enter).

[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-001.png)

*  **Note:** Successfully completed items will show in green (Ok) and you will see a screen like the following, once it’s done running:

[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-002.png)

3.  Access the ```C:\Users\<username>\Documents\Citrix\AutoConfig``` folder and open up the ```CvadAcSecurity.yml``` file, which you will need to edit for PVS to work. **Note:** The UserName and Password fields refer to your PVS Site Server credentials. If you use a DOMAIN\Username password, be sure to specify it as such. Save the file when you’re ready.

[![Provisioning Method PVS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-pvs-process-003.png)

4.  After this, please follow the steps mentioned on the [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

### Dealing with Machine Creation Services (MCS) Machine Catalogs, Delivery and Application Groups and Policies

Currently, this tool does not support importing MCS machine catalogs or their corresponding delivery groups in an automated way. However, you can still import other configuration such as Applications and policies using this tool. You will need to ensure you create the machine catalog and delivery group using the same name as the On-Premises setup one. Please follow the steps below in order to prepare your environment, before proceeding to import the Application settings:

1.  In your [Cloud portal](https://citrix.cloud.com), click on the Hamburger menu > **My Services > Virtual Apps and Desktops Service > Manage** tab, in order to create your MCS machine Catalog as you normally would (by selecting the desired OS Type, Master Image, Storage, Licensing, Network and Account settings). If needed, please refer to [this guide](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/cvads.html) on how to set up your catalogs. **Important Note:** Be sure to name your catalog **exactly** the **same way** your existing On-Premises catalog is named.

2.  Still in **Cloud Studio**, create the corresponding Delivery Group for the new Catalog and ensure you name it exactly after the corresponding **On-Premises Delivery Group** as well. **Note:** For additional details on how to create your Machine Catalogs and Delivery Groups, please refer to [this guide](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/cvads.html).

3.  In your On-Premises environment’s Citrix Studio, under the **Applications** node, confirm that the desired applications belong to the matching **Delivery Groups** by selecting the desired app and then right clicking on the app to go to the **Properties** as follows:

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-001.png)

4.  Click on the **Groups** node to confirm the groups the app in question belongs to:

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-002.png)

5.  Back on the **PowerShell console**, run the **Merge** command and use the **byDeliveryGroupName** flag, which will filter the **Applications** by **Delivery Group** name. Complete Syntax example: ```Merge-CvadAcToSite –Applications $true –ByDeliveryGroupName <DG_name>```

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-003.png)

6.  Hit **Return (Enter)** on your keyboard in order to execute the above command and type **yes** to continue.

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-004.png)

*  **Note:** Upon Successful execution and completion, the output will look similar to the following:

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-005.png)

7.  On your **Cloud Studio** console, go to the **Applications** node and refresh to make sure the apps are listed as expected. Select the applications and go to **Application Properties > Groups** to doublecheck.

*  **Application Folders in Cloud Studio Before running the Migration tool**

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-006.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-006.png)

*  **Application Folders in Cloud Studio After running the Migration tool**

[![Provisioning Method MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-007.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-process-007.png)

*  **Note:** After executing these actions, please follow the steps mentioned on the [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

### Importing MCS-related Policies

Once you’ve performed the actions above, if you need to import policies associated with your MCS Catalogs/groups, follow these instructions:

1.  Run the ```Merge-CvadAcToSite -GroupPolicies $true``` command in the **PowerShell console** and type ```yes``` to continue, as shown in the screenshot below, and then hit Return (Enter):

[![Policies for MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-001.png)

*  **Note:** Successful execution will show a similar output (Added values). The screenshot below also shows the result of a line for which there were no changes (No Change):

[![Policies for MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-002.png)

2.  Upon execution, refresh the **Cloud Studio** window and access the **Policies** node on the left hand side.

[![Policies for MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-003.png)

3.  Check the Policies Assigned to tab and compare it against your On-Premises policy assignment, as illustrated on the following example.

*  **On-Premises Studio:**

[![Policies for MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-004.png)

*  **Cloud Studio:**

[![Policies for MCS](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-prov-mcs-policies-005.png)

## Import your Site Configuration into Cloud

During this step, you will obtain the **customer connection details**, manually create your **Zone mappings** and **import the configuration** to your Cloud tenant.
Note: For PVS and MCS, first follow the corresponding subsections under the [Import your Site Configuration into Cloud section](#import-your-site-configuration-into-cloud) in this guide.

### Obtaining Customer Connection Details

Administrators must edit the ```CustomerInfo.yml``` file and add the corresponding **CustomerName**, **CustomerID** and **SecretKey** values to it. These values can be obtained and generated from the **Cloud portal**, as shown below.

1.  First, open your ```CustomerInfo.yml``` file using a text editor application, such as Notepad. The following screenshot shows the CustomerInfo.yml file values that will need to be edited (underlined in red):

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-001.png)

2.  On your [Cloud portal](https://citrix.cloud.com) click on the Hamburger Menu again and go to **Identity and Access Management**:

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-002.png)

3.  Click on the **API Access** tab and copy the **Customer ID** value, which can be found next to the ‘customer ID’ text as seen on the screenshot below (red rectangle):

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-003.png)

4.  Paste the retrieved value **between the quotes** that follow the **CustomerId field** in your ```CustomerInfo.yml``` file, right between the “” (quotes):

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-004.png)

5.  Back on your Cloud portal, under the **Identity and Access Management** portal and **API Access** tab, enter the name you wish to identify this API key with on the **Name your Secure Client** box and then click on the **Create Client** button. **Note:** This will generate the **Client ID** and the **Secret Key**.

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-005.png)

6.  Copy the **ID** and the **Secret** values, one by one (paste them on the ```CustomerInfo.yml``` file as shown in the step below) and click on **Download** to save the file for later reference.

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-006.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-006.png)

7.  Paste the **ID** and **Secret** values onto the corresponding fields in the ```CustomerInfo.yml``` file:

[![Importing Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_importing-connection-details-007.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_other-importing-connection-details-007.png)

### Manually create the Zone Mapping file (ZoneMapping.yml)

**On-Premises Zones** cannot be automatically migrated to a **cloud Resource Location**, so they must be mapped using the ```ZoneMapping.yml``` file. **Note:** Migration failures will occur if the zone is not mapped with a homonymous resource location (a Resource Location with the exact same name).

1.  Back in the same directory where your *.yml* files reside ```(Documents\Citrix\AutoConfig)```, open up the ```ZoneMapping.yml``` using Notepad or another text editor. **Note:** The **Primary** value must be replaced with the name of your corresponding Zone you wish to migrate objects from in your On-Premises environment.

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-001.png)

2.  You will find this name under your **On-Premises Citrix Studio console > Configuration > Zones**. **Note:** if your Zone is named ```Primary``` in your On-Premises environment, this value on the ```ZoneMapping.yml``` file doesn’t need to be changed:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-002.png)

3.  Still on the ```ZoneMapping.yml``` file, the ```Name_Of_Your_Resource_Zone``` value should be replaced with your Cloud Resource Location name, which can be found on your [cloud portal](https://citrix.cloud.com) under the Hamburger Menu > **Resource Locations**:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-003.png)

4.  Copy your Resource Location name (Shown as My Resource Location on the screenshot below):

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-004.png)

5.  Paste this value on the ```ZoneMapping.yml``` file in lieu of the ```Name_Of_Your_Resouce_Zone``` value:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-005.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-005.png)

*  **Note:** Multiple Zones in your On-Premises environment can also map to **one Resource Location** in the cloud, but there always needs to be one row in the file for **each zone** in the **On-Premises environment**. For **multiple zones on-premises** and **one Resource Location**, the format on this file would look as follows:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-006.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-006.png)

When mapping **Zones** to different **Resource locations**, the file must look like this instead:

[![Zone Mapping](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-007.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_zone-mapping-007.png)

### Merging configuration

1.  Back on the migration tool PowerShell console, run the following command: Merge-CvadAcToSite  in order to merge the existing Cloud configuration (should there be any) with the configuration exported from the On-Premises site.

[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-001.png)

2.  As tasks execute successfully, the output will look green as .yml files are imported and the corresponding components are added to the cloud site:

[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-002.png)

3.  The resulting files will show in the following directory: ```<This PC>\Documents\Citrix\AutoConfig\Import_<YYYY_MM_DD_HH_mm_ss>```

[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-003.png)

4.  On this same folder, you will find a ```Backup_YYYY_MM_DD_HH_mm_ss``` folder. Copy this folder somewhere safe as a backup of the configuration.

5.  The ```Backup``` folder contains the following files, which are helpful for reverting back, if needed:

[![Merging Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-004.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_merging-config-004.png)

### Verify configuration created in Cloud Studio

1.  Access your Virtual Apps and Desktops service Manage tab via the [Cloud Console](https://citrix.cloud.com) > **My Services > Virtual Apps and Desktops service > Manage tab**).

[![Verifying Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-001.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-001.png)

2.  Refresh to make sure the **Machine catalogs**, **Delivery groups**, **policies**, **tags** and **applications** are now showing as expected. **Note:** This is dependent on what you import, so the results will vary depending on your own unique configuration. Review **each section** to make sure the expected items are listed.

*  Machine Catalogs list example:

[![Verifying Configuration](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_verifying-config-002.png)

If everything looks as expected, your CVADS migration is complete.

## Troubleshooting Tips

General information for troubleshooting:

*  Executing any cmdlet results in a created log file and an entry in the master history log file.
*  All operation log files are placed in a backup folder.
*  All log file names begin with CitrixLog, then show the auto-config operation and the date and timestamp of the cmdlet execution.
*  Logs do not auto-delete.
*  The master history log is located in ```%HOMEPATH%\Documents\Citrix\AutoConfig```, in the file named ```History.Log```.
*  Each cmdlet execution results in a master log entry containing the date, operation, result, backup, and log file locations of the execution. This log provides potential solutions and fixes to common errors.

For more information:

1.  Please consult the [Auto Config Tool Troubleshooting FAQ article](https://support.citrix.com/article/CTX277730).

2.  You can also reach out via the [Support Forum](https://discussions.citrix.com/forum/1804-automated-configuration-for-virtual-apps-and-desktops-tech-preview/).

3.  If after consulting the FAQ article and the forum, you still need assistance, please get in touch with your Citrix representatives, Customer Success Manager or Support.
