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
*  The domain-joined machine where the CAC tool will be installed is running .NET 4.7.2 version or higher.
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

1.  Download the Auto Config tool to your On-Premises DDC or a domain-joined machine. **Note:** See the [Pre-requisites section](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html##pre-requisites) for more details on how to run it from a different machine.
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

2.	The Auto Config PowerShell tool will open and the current directory will point to the tool’s default path (```C:\Users\<username>\Documents\Citrix\AutoConfig```). **Note:** This is also where the *.yml* files will be created.

[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-002.png)

3.	On the **Auto Config** Command Prompt, run the ```Export-CvadAcToFile -all $true``` command. This command will export policies, Manually provisioned catalogs and Delivery groups, applications, application folders, icons, zone mappings, tags and other items in the infrastructure.
**Note:** For MCS and PVS catalogs and delivery groups, refer to 

[ section](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html##) in this guide.
 
4.	Once the tool finishes running, the **Overall status** shows as **True** and the export process will be completed. You will see several output lines like the ones on this illustration:
[![Exporting Config](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-automated-configuration_export-config-003.png)

**Note:** If there are any  errors, diagnostic files are created in the action-specific subfolders ```(Export, Import, Merge, Restore, Sync, Backup, Compare)```, which can be found under ```%HOMEPATH%\Documents\Citrix\AutoConfig```. Please get in touch with your Success Management designated team and provide these files for problem resolution.

5.  The resulting YAML (.yml) files will be in the current user’s Documents\Citrix\AutoConfig path and will look as follows:
 

**Note:** See below for an example of the contents on a .yml file (Application.yml):


 

**Note:** If necessary, copy YML files to the machine you will use to import to the Cloud DDC. Export and import can be done from the same machine.


Bulleted List

*  Second item

Numbered list



## Code snipet

Example

```powershell
Connect-MSOLService
```

## Example of navigation within same doc

*  [Active Directory](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html#active-directory): To enable Active Directory authentication, a cloud connector must be deployed within the same data center as an Active Directory domain controller by following the [Cloud Connector Installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/