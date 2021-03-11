---
layout: doc
h3InToc: true
contributedBy: Arnaud Pain
specialThanksTo: Beth Pollock
description: Learn how to migrate your on-premises Citrix Virtual Apps and Desktops to Citrix Cloud and your on-premises VMware vSphere to Microsoft Azure.
---
# Migrating Citrix Virtual Apps and Desktops from VMware vSphere to Citrix Virtual Apps and Desktops service on Microsoft Azure

## Overview

In this document, you'll discover how to migrate from Citrix Virtual Apps and Desktops on VMware vSphere to Citrix Virtual Apps and Desktops service on Microsoft Azure. Migrating to cloud resources modernizes your deployment, providing enhanced elasticity, scalability, and management.

The guidance documented here is based on a deployment in a Citrix- and Microsoft-reviewed and approved lab environment. The initial and final deployments represent typical customer environments.

We migrated these key products and components:

*  [Citrix Virtual Apps and Desktops on-premises to Citrix Virtual Apps and Desktops service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-citrix-virtual-apps-and-desktops-service)
*  [Workspace Environment Management on-premises to Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service)
*  [On-premises StoreFront and Citrix Gateway to Citrix Workspace and Citrix Gateway service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-citrix-workspace-and-citrix-gateway-service)
*  [On-premises vSphere workloads to workloads in Azure](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-on-premises-workloads-to-azure)
*  [On-premises file servers to Azure](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#file-server-technologies)
*  [Citrix workload in Azure](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#move-citrix-workload-to-azure)

The following diagram shows the migration process.

![Migration process](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_full-migration.gif)

### The best migration path

In our experience and testing, the best migration path follows these steps:

1.  [Set up a basic Citrix Cloud environment](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#set-up-a-basic-citrix-cloud-environment).
1.  [Migrate on-premises Citrix Virtual Apps and Desktops to Citrix Virtual Apps and Desktops service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-citrix-virtual-apps-and-desktops-service) using the automated configuration tool developed by Citrix.
1.  [Migrate on-premises Workspace Environment Management to Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service).
1.  [Configure the end-user access layer](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#configure-the-end-user-access-layer).
1.  [Prepare the Azure subscription](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#prepare-the-azure-subscription) to receive the workloads from your on-prem deployment.
1.  [Configure site-to-site connectivity](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#configure-site-to-site-connectivity).
1.  Use Azure tools to [migrate your on-premises servers to Azure](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-on-premises-workloads-to-azure).
1.  [Move your Citrix workload to Azure](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#move-citrix-workload-to-azure).

![Migration path](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_migration-path.png)

If you do the migration in the specified order, you'll have a more organized and orderly migration. The specified order lets you run products, components, and services in a combination of on-premises and cloud deployments, minimizing the disruption to daily workloads.

The following diagram shows our migrated environment, with all workloads on Azure and using Citrix Cloud.

![Post-migration deployment](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_post-migration-deployment.png)

### Audience

We've written this document for users who are

*  Familiar with the administration of a Citrix Virtual Apps and Desktops or XenApp and XenDesktop site
*  Familiar with the administration of a StoreFront environment
*  Familiar with the basics of Azure

It's also helpful if you know Citrix Cloud fundamentals and understand Citrix Gateway or NetScaler Gateway.

## Lab details

We ran the migration workflow in our lab, using virtual machines that had the following components installed and configured:

*  Domain controller, DNS, DHCP, certificate services
*  SQL Server 2019 Standard
*  Citrix delivery controller
*  StoreFront 1912
*  Citrix Director
*  Remote Desktop Services (RDS) and Citrix License Server
*  Citrix Workspace Environment Management server
*  File server
*  Windows 2016 master
*  Windows 10 master
*  Citrix ADC

### Active Directory structure

Each component of the overall solution requires integration with Active Directory (AD) to provide authentication into the Citrix environment and personalization settings that are applied to user sessions via AD Organizational Units (OUs) in each part of the project.

Citrix recommends an OU structure like the one illustrated in the following image. Integrate the OU structure with your current structure. The OU structure hosts the machines and the user accounts for the Citrix Cloud environment.

![Example Active Directory structure](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_example-active-directory-structure.png)

### Active Directory preparation

For our lab environment, we defined GPOs that:

1.  Configure Citrix Workspace Environment Management to define the infrastructure server.
1.  Configure the Citrix Workspace app to allow SSO and to populate applications on the user’s desktop.
1.  Configure a logon script to apply a background using BGInfo. For the purposes of our demo, we apply different background settings based on the migration project’s phase.

With the basic lab setup in place, we’re ready to start the migration.

We recommend that you create the AD structure, GPOs, and link the GPOs that will host your Azure workload before you start the migration project. It’s also helpful to have checkpoints in place so you can validate the successful completion of each step.

## Set up a basic Citrix Cloud environment

If you’re already using Citrix Virtual Apps and Desktops service, you can skip to the section [Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service).

We set up the Citrix Cloud environment in three steps:

Step 1: Onboarding

Step 2: Deploy Citrix Cloud Connector on two VMware ESX VMs

Step 3: Rename the resource location

### Step 1: Onboarding

Ensure you have a valid [Citrix Virtual Apps and Desktops subscription](/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/signing-up-for-citrix-cloud.html).

To proceed, you need access to the Citrix Virtual Apps and Desktops service. If you don’t have access, contact a Citrix representative.

### Step 2: Cloud Connector installation

The Citrix Cloud Connector lets you configure secure communication between your on-premises deployment and Citrix Cloud. To ensure that your Citrix Cloud Connector is set up to spec, review the [System requirements](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html). You need two VMware ESX VMs to host two Citrix Cloud Connectors.

You can install the Cloud Connector software [interactively](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#interactive-installation) or using the [command line](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#command-line-installation).

### Step 3: Rename the resource location

>**Notes from the field:**
>
>When you have multiple resource locations, it’s best practice to have a distinct name for each resource location. That helps you identify it more easily.

1.  Select your resource location, click **...** and select **Rename**.

    ![Rename resource location](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_rename-resource-location.png)

1.  Give your resource location a new name, for example, **On-premises**, and click **Save**.

    ![Save new name](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_save-new-resource-location-name.png)

>**Checkpoint: Citrix Cloud Connector**
>
>When your Cloud Connectors are deployed successfully and running, they are listed with a green bar on the left.
>
>![Cloud Connector status indicators](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connectors-green-bar.png)
>
>If the status is not what you expect, use the **Citrix Cloud Services Connectivity Test Tool**, for more information.

## Migrate to Citrix Virtual Apps and Desktops service

If you’re already using Citrix Virtual Apps and Desktops service, you can skip to the section [Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service).

If you’re using Citrix Virtual Apps and Desktops on-premises, you can use the procedures in this section as a guide for migrating to Citrix Virtual Apps and Desktops service.  

For the MCS provisioning method, the [MCS migration](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#mcs-migration) section provides detailed migration steps.

For the PVS provisioning method, go to [PVS migration](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#pvs-migration).

The following diagram shows our cloud environment on Azure after we migrate to the Citrix Virtual Apps and Desktops service.

![Environment with Citrix Virtual Apps and Desktops service configured](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cvad-service-migrated.png)

### MCS migration

#### Step 1: Preparation

To migrate successfully using the automated configuration tool, for on-premises MCS configured catalogs, you must replicate your on-premises configuration for hosting, machine catalogs, and delivery groups. The migration tool only creates and assigns applications and policies for MCS deployments.

During this migration phase, we recommend that you create only a few VMs for each machine catalog. Having just a few VMs allows you to validate machine catalog creation and then allows you to create delivery groups and run the Azure migration tool.

Because the objective is to migrate your Citrix Workload to Azure, it's not necessary to create all the VMs with Citrix Virtual Apps and Desktop on-premises.

Use your on-premises master images to create Citrix Virtual Apps and Desktops service machine catalogs and delivery groups.

>**Important:**
>
>To run the migration tool successfully, the delivery groups created in the Citrix Virtual Apps and Desktops service must have identical names to the existing on-premises delivery groups.

##### Establish host connection

The host connection is where you establish the network and the storage–the resources–for a connection. See [Create and manage connections](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html) for detailed information about connections.

##### Create machine catalogs

Use machine catalogs to manage collections of machines as single entities. For more information, see [Create machine catalogs](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html). Machine catalogs are a prerequisite for creating delivery groups. When you run the Azure migration tool, the tool brings over all of your settings and creates all the VMs you need. To make sure you have placeholders for your delivery groups, create machine catalogs that contain a minimal number of VMs during this step. Usually, two VMs are enough to satisfy this requirement.

##### Create delivery groups

Use delivery groups to control access and delivery to machines, applications, and desktops. For details, see [Create delivery groups](/en-us/citrix-virtual-apps-desktops-service/install-configure/delivery-groups-create.html). Create delivery groups that have identical names to your existing on-premises delivery groups.

#### Step 2: Migration

We use a Citrix-developed tool, the automated configuration tool, to migrate Citrix Virtual Apps and Desktops from on-premises to Citrix Virtual Apps and Desktops service. You can [download the automated configuration tool](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html) from Citrix. Documentation for the tool is available on Tech Zone in [Automated Configuration](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html).

Because our on-premises lab is using MCS, we only migrate the following:

*  Applications
*  Citrix Policies

##### Export the on-prem site configuration

>**Note:**
>
>Because we are using MCS and can only import applications and group policies, we use two options here: `-Applications` and `-GroupPolicies` to export only the required information.

##### Prerequisites

We run the automated configuration tool on a delivery controller, and we need .NET Framework 4.7.2 or later to be installed on that server.

You can download .NET Framework 4.7.2 from: [https://dotnet.microsoft.com/download/dotnet-framework/net472](https://dotnet.microsoft.com/download/dotnet-framework/net472).

Install `AutoConfig_PowerShell_x64.msi` on the delivery controller. Installing the tool creates a desktop icon called **Auto Config** that launches the PowerShell command prompt. You run the Cloud automated configuration cmdlets from the PowerShell command prompt.

##### Export applications

1.  Click the **Auto Config** icon.

    ![Auto Config icon](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-desktop-icon.png)

1.  Run the following command: `Export-CvadAcToFile –Applications $true`.

    ![Export apps = true](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-export-apps-true.png)

##### Export policies

1.  Run the following command: `Export-CvadAcToFile –GroupPolicies $true`.

    ![Export policies = true](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-export-policies-true.png)

1.  Ensure that the result is reported as **True**.

##### Import settings to Citrix Cloud

All the files that you edit are in the folder where you run the PowerShell command. The **Auto Config** tool creates the files when you run the export command.

##### Prerequisites

1.  Edit and fill the `CustomerInfo.yml` file with `Customer ID`, `Client ID`, and `Secret`.

    ![Example customerinfo.yml file](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_customer-info-yml-empty-copy.png)

1.  Connect to Citrix Cloud and go to **Identity and Access Management**.

    ![Citrix Cloud IAM](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-menu-iam.png)

1.  Click **API Access**.

    ![API access in Citrix Cloud IAM](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_api-access.png)

1.  Take note of your customer ID.

    ![CC customer ID](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-customer-id.png)

1.  Provide a name and click **Create Client**.

    ![CC resource name](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-resource-name.png)

1.  Copy the ID.

    ![ID copy](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_id-copy.png)

1.  Copy the secret

    ![Secret copy](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_secret-copy.png)

1.  Paste the information in a document (it is not possible to see the information again, so keep the document safe).

    ![Customer info YAML](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_customer-info-yml-contents.png)

1.  When the file is filled, save it (insert your information between the quotation marks).

1.  Edit `ZoneMapping.yml` using Notepad.

    ![Original zone mapping YAML file](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_zone-mapping-yml-original.png)

1.  Replace the highlighted text with your Cloud resource location name as shown in the following example:

    ![Zone mapping YAML example](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_zone-mapping-yml-edited.png)

>**Note:**
>
>If the default Cloud resource location name has not been changed, it should be **Primary**.

The correct syntax for the primary zone is to keep a space between the colon `:` and the first quotation mark `"`, in keeping with standard YAML syntax. The name is case-sensitive and must be enclosed in quotation marks as shown.

##### Import applications

1.  Run the following command in the Auto Config tool: `Import-CvadAcToSite -Applications $true`.

    ![Import apps](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-apps-cmd.png)

1.  Enter yes to validate the action.

    ![Validate app import](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-apps-cmd-validate.png)

    The resulting output looks like the following image:

    ![App import result](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-apps-result.png)

##### Import policies

1.  Run the following command in the Auto Config tool: `Import-CvadAcToSite -GroupPolicies $true`.

    ![Import policies](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-policies-cmd.png)

1.  Enter yes to validate the action.

    ![Validate policy import](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-policies-cmd-validate.png)

    The resulting output looks like the following image:

    ![Policy import result](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_import-policies-result.png)

1.  Ensure that the result is **True**.

More details about each VDA reconfiguration option are available from Citrix product documentation in [VDA registration](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html)

>**Checkpoint: Citrix Virtual Apps and Desktops service migration using MCS**
>
>1.  Connect to Citrix Virtual Apps and Desktops service, go to **Applications** and ensure that applications have been created.
>    ![Applications list](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_applications-list.png)
>
>1.  Go to **Policies** and ensure that your policies have been created and assigned.
>    ![Policies list](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_policies-list.png)

If you're using Workspace Environment Management, continue with [Migrate to Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service).

If you're not using Workspace Environment Management, the next step is to [Configure the end user access layer](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#configure-the-end-user-access-layer).

### PVS Migration

We use a Citrix-developed tool, the automated configuration tool, to migrate Citrix Virtual Apps and Desktops from on-premises to Citrix Virtual Apps and Desktops service. You can [download the automated configuration tool](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html) from Citrix.

Documentation for the tool is available on Tech Zone in [Automated Configuration](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html).

#### Export the on-prem site configuration

##### Prerequisites

We run the automated configuration tool on a delivery controller, and we need .NET Framework 4.7.2 or later to be installed on that server.

You can download .NET Framework 4.7.2 from: [https://dotnet.microsoft.com/download/dotnet-framework/net472](https://dotnet.microsoft.com/download/dotnet-framework/net472).

Install `AutoConfig_PowerShell_x64.msi` on the delivery controller. Installing the tool creates a desktop icon called **Auto Config** that launches the PowerShell command prompt. You run the Cloud automated configuration cmdlets from the PowerShell command prompt.

##### Export settings

1.  Click the **Auto Config** icon.

    ![Auto Config icon](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-desktop-icon.png)

1.  Run the following command: `Export-CvadAcToFile`. This command is the same as running the `Export-CvadAcToFile -All $True` command.

    ![Export](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-export-all.png)

1.  Ensure that the result is reported as **True**.

#### Import settings to Citrix Cloud

##### Prerequisites

1.  Edit and fill the `CustomerInfo.yml` file with `Customer ID`, `Client ID`, and `Secret`.

    ![Example customerinfo.yml file](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_customer-info-yml-empty-copy.png)

1.  Connect to Citrix Cloud and go to **Identity and Access Management**.

    ![Citrix Cloud IAM](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-menu-iam.png)

1.  Click **API Access**.

    ![API access in Citrix Cloud IAM](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_api-access.png)

1.  Take note of your **customer ID**.

    ![CC customer ID](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-customer-id.png)

1.  Provide a name and click **Create Client**.

    ![CC resource name](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-cloud-resource-name.png)

1.  Copy the ID.

    ![ID copy](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_id-copy.png)

1.  Copy the secret.

    ![Secret copy](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_secret-copy.png)

1.  Paste the information in a document (it is not possible to see the information again, so keep the document safe).

    ![Customer info YAML](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_customer-info-yml-contents.png)

1.  Add the following line at the end of the file: `HostConnections: True` to allow the hosting configuration.

    ![Host connections true](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_host-connections-true.png)

1.  When the file is filled, save it (insert your information between the quotation marks).

1.  Edit `ZoneMapping.yml` using Notepad.

    ![Original zone mapping YAML file](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_zone-mapping-yml-original.png)

1.  Replace the highlighted text with the name of your on-premises resource location defined in Citrix Cloud, as shown in the following example:

    ![Zone mapping YAML example](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_zone-mapping-yml-edited.png)

    >**Note:**
    >
    >If the default Cloud resource location name has not been changed, it should be **Primary**.

    The correct syntax for the primary zone is to keep a space between the colon `:` and the first quotation mark `"`, in keeping with standard YAML syntax. The name is case-sensitive and must be enclosed in quotation marks as shown.

1.  Edit `CvadAcSecurity.yml` using Notepad.

    ![CvadAcSecurity YAML](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cvadacsecurity-yml.png)

1.  Fill the file with your credentials and save it. The correct format for the user name is `domain\username`.

##### Import settings

1.  Run the following command in the Auto Config tool: `Import-CvadAcToSite -All $true`.

    ![Import all settings](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-import-all.png)

1.  Enter yes to validate the action.

    ![Verify settings import](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-import-verify.png)

The resulting output looks like the following image:

![Settings import result](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-import-result.png)

More details about VDA registration options are available from Citrix product documentation in [VDA registration](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html)

>**Checkpoint: Citrix Virtual Apps and Desktop service using PVS**
>
>1.  Connect to Citrix Virtual Apps and Desktops Service, go to Machine Catalogs and ensure that your Machine Catalogs have been created and machines allocated.
>    ![Machine catalogs](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_machine-catalogs.png)
>
>1.  Go to **Delivery Groups** and ensure that your delivery groups have been created.
>    ![Delivery groups](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_delivery-groups.png)
>1.  Ensure your VMs are registered in the Citrix Cloud studio console.
>    ![Registered VMs in Cloud studio console](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_registered-vms-cloud-studio.png)
>
>1.  Go to Applications and ensure that your applications have been created.
>    ![Applications list](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_applications-list.png)
>
>1.  Go to Policies and ensure that your policies have been created and assigned.
>    ![Policies list](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_policies-list.png)

If you're using Workspace Environment Management, continue with [Migrate to Workspace Environment Management service](/en-us/tech-zone//build/deployment-guides/azure-citrix-migration.html#migrate-to-workspace-environment-management-service). If you're not using Workspace Environment Management, the next step is to [configure the end user access layer](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#configure-the-end-user-access-layer).

## Migrate to Workspace Environment Management service

This step applies only if you use Workspace Environment Management on-premises and want to migrate to the Workspace Environment Management service. If that does not apply to you, then skip to the next section, Citrix workload on Azure.

### Step 1: Meet the system requirements

The toolkit supports the migration from Workspace Environment Management 4.7 and later. To migrate from an earlier version, upgrade Workspace Environment Management 4.x to Workspace Environment Management 4.7 or later, and then migrate the database to the Workspace Environment Management service.

For details about migrating to the Workspace Environment Management service, see [Migrate](/en-us/workspace-environment-management/service/migrate.html).

The following diagram shows the environment with our on-prem and Azure resources, now adding the Workspace Environment Management service to our cloud environment.

![Workspace Environment Management service](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_wem-service.png)

>**Notes from the field:**
>
>To verify that our VDAs switch from on-prem to Cloud, an easy trick is to create a setting that is only configured in the Workspace Environment Management service.

### Step 2: Switch to service agent mode

Use the agent switch feature to switch from on-premises to service agent mode. For information about the agent switch, see Agent Switch.

>**Note:**
>
>The agent switch feature is available in Workspace Environment Management 1909 and later. For earlier versions, you must reinstall the agent or upgrade it to version 1909 or later before using the agent switch.

Alternatively, you can download the agent from the service’s **Downloads** tab and then manually reinstall the agent.

1.  Connect to Workspace Environment Management Console on-prem.

    ![WEM on-prem console](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_wem-console-on-prem.png)

1.  In **Advanced Settings**, click the **Agent Switch** tab.

    ![WEM agent switch tab](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_wem-agent-switch.png)

1.  Check the box **Switch to Service Agent**, provide the Cloud Connector servers, and click **Apply**.

1.  Restart the VDA to apply the new settings.

>**Checkpoint: Workspace Environment Management service migration**
>As a verification step to confirm that our VDAs switched from on-prem to Cloud, we added an app, Notepad, that is uniquely available from the Workspace Environment Management service. We used the following steps to confirm the switch.
>
>1.  Open the Win 10 + Citrix Virtual Apps and Desktops Service Desktop.
>
>1.  Ensure that the new application configured with the Workspace Environment Management service (in our example, Notepad) is populated on the user’s desktop.
>    ![Desktop with Workspace Environment Management app icon](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_wem-desktop-notepad-icon.png)

## Configure the end user access layer

You have two options for configuring the end user access layer:

*  Continue to use [StoreFront with Citrix Gateway](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#storefront-with-citrix-gateway) if you require specific StoreFront features
*  [Migrate to Citrix Workspace and Citrix Gateway service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#migrate-to-citrix-workspace-and-citrix-gateway-service)

### StoreFront with Citrix Gateway

In this section, we configure the on-premises StoreFront and Citrix Gateway to integrate with Citrix Virtual Apps and Desktops service. To support multiple StoreFront sites, we use multisite aggregation.

#### StoreFront

1.  Add your Cloud Connector as a delivery controller in each configured store.

    ![Cloud Connectors added as delivery controllers](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connectors-delivery-controllers.png)

1.  Add a delivery controller configuration by adding the Cloud Connector as the secure ticket authority (STA) in your Citrix Gateway configuration.

    ![Add delivery controller](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_add-delivery-controller.png)

1.  Aggregate resources. To learn more about multisite aggregation, [Designing StoreFront Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) is a useful resource.

1.  Map users to controllers.

#### Citrix Gateway

Add the Citrix Cloud Connector as the STA in your Citrix Gateway configuration.

![Add Cloud Connector as STA](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connector-gateway-sta.png)

>**Checkpoint: STA**
>
>Ensure that all STA servers have a status of UP as shown in the following image.
>
>![STA server status UP](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_sta-server-status-up.png)

Now the STA servers are up and StoreFront is configured.

>**Checkpoint: Citrix Virtual Apps and Desktops service migration with on-premises access layer**
>
>1.  Ensure in Citrix Cloud that the VDAs are registered. The Citrix product documentation provides a deeper understanding of [VDA registration](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html).
>
>1.  Connect to your Citrix Gateway.
>
>1.  Launch a published resource.

### Migrate to Citrix Workspace and Citrix Gateway service

In this section, we migrate to the Citrix Gateway service and Citrix Workspace, as shown in the following diagram.

![Workspace and Citrix Gateway Service](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-and-citrix-gateway-service.png)

>**Note:**
>
>When you evaluate the Citrix Gateway service and Citrix Workspace, make sure the customizations and configurations you need are available.

#### Configuration

1.  Connect to Citrix Cloud.

1.  Click **Home > Workspace Configuration**.

    ![Workspace configuration in Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-workspace-configuration.png)

1.  Edit the Workspace URL and provide a name that meets your requirements. Click **Save**.

    ![Edit Workspace URL](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_edit-workspace-url.png)

1.  Click **Authentication**.

    ![Authentication tab](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-authentication-tab.png)

1.  The supported authentication methods are presented. Select the one you want and click **Customize**.

    ![Customize tab](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-customize-tab.png)

1.  You can customize with two logos. One for the authentication page and one for the Workspace store.

    ![Change colors](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-customize-color.png)

1.  You can change colors if necessary.

    ![Save customizations](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-save-customizations.png)

1.  Click **Save**.

    ![Service integrations](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-service-integrations.png)

1.  Click **Service Integrations**.

    ![Enable Citrix Virtual Apps and Desktops service](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-enable-cvad-service.png)

1.  Enable **Virtual Apps and Desktops**.

    ![Confirm selected service integration](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-confirm-service-integration.png)

1.  Click **Confirm**.

    ![Workspace configuration access](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-config-access.png)

1.  Click **Access**.

    ![Configure Workspace connectivity](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_workspace-connectivity.png)

1.  To the right of the resource location, click the 3 dots **...** and select **Configure Connectivity**.

    ![Save Workspace connectivity settings](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_save-workspace-connectivity-settings.png)

1.  Select **Gateway Service** and click **Save**.

>**Checkpoint: Citrix Workspace and Citrix Gateway service migration**
>
>1.  Ensure in Citrix Cloud that the VDAs are registered. The Citrix product documentation provides a deeper understanding of [VDA registration](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html).
>
>1.  Connect to your Citrix Workspace URL.
>
>1.  Launch a published resource.

## Prepare the Azure subscription

The first step to migrating your workloads to Azure is to set up the Azure environment. If you’re new to Azure, or if your environment has some different components from our lab setup, Microsoft offers many helpful resources, like [New to Azure? Follow these easy steps to get started](https://azure.microsoft.com/en-us/blog/new-to-azure-follow-these-easy-steps-to-get-started/) and the [Azure Migrate documentation](https://docs.microsoft.com/en-us/azure/migrate/).

For a detailed look at how to architect Citrix Virtual Apps and Desktops on Azure, see [Citrix Virtual Apps and Desktops Service on Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html).

We prepped the Azure environment in five basic steps:

Step 1: Select an Azure subscription model

Step 2: Select the Azure regions

Step 3: Create a resource group

Step 4: Create a virtual network

Step 5: Deploy a server in Azure, that in our case, acts as the domain controller. You can deploy any server type that's easy for you, or skip this step altogether.

When our Azure subscription is set up and ready for the next step, our environment looks like the one in the following diagram. The resources and workloads are all on-premises and we have a resource group, a virtual network, and a server in Azure.

![Environment with Azure prepped](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-subscription-prepped.png)

### Step 1: Select an Azure subscription model

Set up your Azure subscription.

Selecting a subscription model is a complex decision that involves understanding the growth of your Azure footprint within and outside the Citrix deployment. Even if the Citrix deployment is small, you might still have a large amount of other resources that are reading/writing heavily against the Azure API. That heavy read/write activity can have a negative impact on the Citrix environment. The reverse is also true, where many Citrix resources can consume an inordinate number of the available API calls, reducing availability for other resources within the subscription. For information about Azure subscription limits and quotas, see [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits).

### Step 2: Select Azure regions

An Azure region is a set of data centers deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network. Azure gives customers the flexibility to deploy applications where they need to. Azure is generally available in 60 regions and 149 countries around the world.
Microsoft provides details in [https://azure.microsoft.com/en-us/global-infrastructure/regions/](https://azure.microsoft.com/en-us/global-infrastructure/regions/).

### Step 3: Create a resource group

A resource group in Azure is a collection of assets in logical groups for easy or even automatic provisioning, monitoring, and access control, and for more effective management of their costs. The benefit of using resource groups in Azure is that you can group related resources that belong to an application. Because the application's resources share a unified lifecycle from creation to usage and finally, de-provisioning, they can be managed together for efficiency.

The key to having a successful resource group design is understanding the lifecycle of the resources that are included in the resource groups.

If you need guidance setting up a resource group, Microsoft provides details in [What is Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview).

One or more resource groups can be applied to a machine catalog during initial creation. These resource groups cannot be shared across machine catalogs. Resource groups are limited to 240 Citrix MCS VMs due to the 800 per resource limit in a resource group. See [CTX237504](https://support.citrix.com/article/CTX237504) for more details about this limitation.

Resource groups are tied to machine catalogs at creation time and cannot be added or changed later. To add resource groups to a machine catalog, the machine catalog must be removed and recreated.

### Step 4: Create a virtual network in Azure

The virtual network is required to enable secure communication among the resources in your environment. For more information about creating virtual networks in Azure, see [Quickstart: Create a virtual network using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal). For help with configuring a virtual network, see [What is Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview).

### Step 5: Deploy a server in Azure (optional)

With our resource group and virtual network established, we’re ready to deploy a server in Azure. We use this server as the domain controller. It helps us validate AD replication after we establish site-to-site connectivity. If you want more detailed information about how to set up virtual machines, see [Quickstart: Create a Windows virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal). Microsoft also provides helpful guidance for choosing the right size VM for your workload in [Sizes for virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Step 5 is optional. We deployed a server in Azure to better represent a typical deployment.

Now that we have our resource groups and virtual network created, and we have a server deployed, our next step is to configure connectivity between our on-premises and Azure environments.

## Configure site-to-site connectivity

To connect your on-premises environment to Azure, you have multiple options:

*  Site-to-site VPN
    Detailed information is available in [Create a Site-to-Site connection in the Azure portal](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

*  Citrix SD-WAN
    Step-by-step guidance for Citrix SD-WAN is available in the [Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity](/en-us/tech-zone/learn/poc-guides/sdwan-cloud-to-onprem-connectivity.html).

*  Express Route
    Step-by-step guidance for Express Route is in the [Express Route documentation](https://docs.microsoft.com/en-us/azure/expressroute/).

For the purposes of our demonstration, we used site-to-site VPN.  

The following diagram shows how our Azure environment is set up to communicate with our on-premises environment.

![Site-to-site VPN configured](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_site-to-site-configured.png)

>**Checkpoint: site connectivity**
>
>When you have connectivity implemented, a checkpoint is to communicate with Azure resources from your on-premises environment and conversely.
>
>In our case, we used two methods:
>
>Option 1: Test using the ping command between resources in each location.
>
>Option 2: Join the deployed server in Azure to our on-premises AD Domain. If you omitted step 5, you can skip this option.

Now that we have site-to-site connectivity configured and validated, our next step is to migrate our on-premises workloads to Azure.

## Migrate on-premises workloads to Azure

In a typical Citrix customer deployment, there are multiple components that can be migrated. Component types and migration plans may vary by customers.

To get a better sense of how to approach your migration, refer to the [Microsoft Cloud Adoption Framework for Azure](https://docs.microsoft.com/en-us/azure/cloud-adoption-framework/).

 In this section we cover the migration of components that are closely related to Citrix, such as the file server for profiles and folder redirection, and the master images for provisioning. For the purposes of this document, we limited the migration to the components in our lab environment. For your production environment, however, you have the option to move more of your infrastructure to Azure.

### File server technologies

Azure offers several file server technologies that can be used to store Citrix user data, roaming profile information, or function as targets for Citrix App Layering network file shares. The Azure options include:

*  Standalone file server using [Azure files](https://azure.microsoft.com/en-us/services/storage/files/)

*  File servers using [Storage Replica](https://docs.microsoft.com/en-us/windows-server/storage/storage-replica/storage-replica-overview)

*  Scale out file server (SOFS) with [Storage Spaces Direct (S2D)](https://docs.microsoft.com/en-us/windows-server/storage/refs/refs-overview#storage-spaces-direct)

*  [Distributed File System Replication (DFS-R)](https://docs.microsoft.com/en-us/previous-versions/windows/desktop/dfsr/distributed-file-system-replication--dfsr-)

*  Third-party storage appliances (such as NetApp and others) from [Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/)

Select file server technologies that best meet your business requirements. [Introduction to Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-introduction) provides a list of storage types and corresponding use cases to help you determine what’s best for your situation.

### Infrastructure cost management

Azure Reserved VM Instances (RIs) significantly reduce costs—up to 72 percent compared to pay-as-you-go prices—with one-year or three-year terms on Windows and Linux virtual machines (VMs). When you combine the cost savings gained from Azure RIs with the added value of the Azure Hybrid Benefit, you can save up to 80 percent. The 80% is calculated based on a three-year Azure Reserved Instance commitment of a Windows Server when compared to the normal pay-as-you-go rate.

While Azure Reserved Instances require making upfront commitments on compute capacity, they also provide flexibility to exchange or cancel reserved instances at any time. A reservation only covers the virtual machine compute costs. It does not reduce any of the additional software, networking, or storage charges. This is good for Citrix infrastructure and the minimum capacity needed for a use case (on and off hours).

### Azure VM instance types

Each Citrix component uses an associated virtual machine type in Azure. Each VM series available is mapped to a specific category of workloads (general purpose, compute-optimized, and so forth), with various sizes controlling the resources allocated to the VM (CPU, Memory, IOPS, network, and others). The [Virtual Machine Series](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/series/) provides detailed descriptions of the series and their capabilities.

Most Citrix deployments use the D-Series and F-Series instance types. The D-Series are commonly used for the Citrix infrastructure components and sometimes for the user workloads when they require more memory beyond what is found in the F-Series instance types. F-Series instance types are the most common in the field for user workloads because of their faster processors, which bring with them the perception of responsiveness.

Why D-Series or F-Series? From a Citrix perspective, most infrastructure components (Cloud Connectors, StoreFront, NetScaler, and so on) use CPU to run core processes. These VM types have a balanced CPU to Memory ratio and are hosted on uniform hardware (unlike the A-Series) for more consistent performance and support premium storage. Certainly, you can and should adjust your instance types to meet your needs and your budget.

The size and number of components within your infrastructure always depends on your requirements, scale, and workloads. However, with Azure we can scale dynamically and on-demand. For the cost conscious, starting smaller and scaling up is the best approach. Azure VMs require a restart when changing size, so plan these events only within scheduled maintenance windows and under established change control policies. Citrix provides a more detailed analysis in [The scalability and economics of delivering Citrix Virtual App and Desktop services on Azure](/en-us/tech-zone/design/design-decisions/azure-instance-scalability.html)

### Storage

Just like any other computer, the virtual machines in Azure use disks as a place to store an operating system, applications, and data. All Azure virtual machines have at least two disks – a Windows operating system disk and a temporary disk. The operating system disk is created from an image, and both the operating system disk and the image are stored within Azure as virtual hard disks (VHDs). Virtual machines may also have other disks attached as data disks, also stored as VHDs.   <https://docs.microsoft.com/en-us/azure/virtual-machines/windows/disks-types>

Azure Disks are designed to deliver enterprise-grade durability. You can select from two performance tiers for storage exist when you create disks: Premium SSD Disks and Standard HDD Storage. The disks may be either managed or unmanaged. Managed disks are the default and are not subject to the storage account limitations like the unmanaged disks.

Microsoft recommends managed disks over unmanaged disks. Unmanaged disks should be considered only for exceptions. Consider that Standard Storage (HDD and SSD) includes transaction costs (storage I/O) but has lower costs per disk. Premium Storage has no transaction costs but has higher per-disk costs and offers an improved user experience.

The disks only offer an SLA when an Availability Set is used. Availability Sets are not supported with Citrix MCS. When you create VMs for Citrix Cloud Connector, Citrix ADC, or StoreFront on Azure, use Availability Sets.

### Azure Migrate services overview

Azure Migrate provides a set of tools to assess and migrate to Azure on-premises servers, infrastructure, applications, and data. For detailed information, see [About Azure Migrate](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview). We used Azure Migrate to migrate our file server and master images.

The following diagram shows the locations of our resources when we migrate to Azure.

![Azure workload migration](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_migrate-azure-workload.png)

We recommend a three-step process:

Step 1: Discovery

Step 2: Assessment

Step 3: Migration

>**Note:**
>
>For PVS users, specific steps are required to prepare your environment before you migrate to Azure. Follow those steps in the next section. MCS users can go directly to [Discovery](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#step-1-discovery).

### Prerequisite for PVS-specific image preparation

For each PVS image, it's necessary to remove the VM from Provisioning and to temporarily configure it so it is hosted on VMware vSphere only. You do this using reverse imaging. There are multiple methods for reverse imaging. One is described in detail in [CTX202159: How to Perform Reverse Imaging on a Provisioning Services Target Device for Windows and its Applicable Usages](https://support.citrix.com/article/CTX202159).

### Step 1: Discovery

The first step is to create a migration project and select the tools to use. You need to set up the appliance for the discovery process. For more details, see [Discover machine apps, roles, and features](https://docs.microsoft.com/en-us/azure/migrate/how-to-discover-applications).

Microsoft provides two methods for setting up the appliance:

*  OVA template (a downloadable file)

*  Script

The Azure Migrate appliance connects to the vCenter Server to discover and migrate VMs using agentless migration.

After the appliance is deployed and you've provided credentials, the appliance starts continuous discovery of VM metadata and performance data, along with discovery of apps, features, and roles. The duration of app discovery depends on how many VMs you have. It typically takes an hour for app discovery of 500 VMs.

### Step 2: Assessment

Based on the discovery results, you create an assessment of all on-premises resources on your vCenter.

For the assessment (which Microsoft recommends), you have multiple options. The two that interest us most for Citrix workloads are:

*  [Azure Migrate: Server Assessment](https://docs.microsoft.com/en-us/azure/migrate/tutorial-assess-vmware)

*  [Movere](https://docs.microsoft.com/en-us/azure/migrate/migrate-services-overview#movere)

We use Azure Migrate: Server Assessment in this project.

The migration tool provides an overview of your environment with details like:

*  Name
*  IP Address
*  Applications installed
*  Dependencies
*  Cores
*  Memory
*  Disks
*  Storage
*  Operating System

![Discovered servers with detail](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_discovered-servers.png)

During the assessment configuration, you have access to assessment properties that allow you to:

*  Select Target Location
*  Select Storage Type
*  Select if you want to assess based on reserved instances
*  Select Sizing criteria
*  Select Performance History
*  Select Percentile utilization
*  Select VMs Series
*  Select Comfort Factor
*  Select Pricing details

![Assessment properties](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_assessment-properties.png)

For dependencies to be discovered, you need to deploy software on the VMs. See [Analyze machine dependencies (agentless)](https://docs.microsoft.com/en-us/azure/migrate/how-to-create-group-machine-dependencies-agentless).

![Dependencies discovery](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_dependencies.png)

Dependencies are helpful to know when you want to migrate application or database servers. Knowing the dependencies lets you determine exactly which servers need to be migrated to ensure a persistent and secure connection between your servers.

To facilitate the migration, we have created a group that only contains the VMs that we intend to migrate.

![Assessment group](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_assessment-group.png)

### Step 3: Migration

For server migration, Microsoft provides options for agent-based and agentless approaches. Our focus here is on the agentless approach, as describe in [Migrate VMware VMs to Azure (agentless)](https://docs.microsoft.com/en-in/azure/migrate/tutorial-migrate-vmware). To determine the best approach for your business, see [Select a VMware migration option](https://docs.microsoft.com/en-us/azure/migrate/server-migrate-overview).

The migration process starts with an initial replication of the on-premises VMs to Azure.

The duration of the process depends on the size of the disks associated to your VMs and your bandwidth. The duration also depends on

*  The number of concurrent migrations
*  Your bandwidth on vCenter
*  The availability of storage (for the agentless approach)
*  Your network bandwidth

During the replication process, you need to select:

*  VMware vSphere
*  Appliance to use
*  Select Virtual machines to migrate (import the Assessment, select Group)
*  Select Target Settings
*  Select Compute
*  Select Disks

![Replicate](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_replicate.png)

When the delta replication begins, after initial replication, you can [run a test migration](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware#run-a-test-migration) before running a full migration to Azure.

We highly recommend that you run a test migration at least once for each machine before you migrate it.

*  Running a test migration checks that migration works as expected, without impacting the on-premises machines, which remain operational, and continues replicating.  

*  Test migration simulates the migration by creating an Azure VM using replicated data (usually migrating to a non-production VNet in your Azure subscription).  

*  You can use the replicated test Azure VM to validate the migration, perform app testing, and address any issues before full migration.

#### Complete the migration

Follow Microsoft’s guidance to [Complete the migration](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware#complete-the-migration) and for [Post-migration best practices](https://docs.microsoft.com/en-us/azure/migrate/tutorial-migrate-vmware#post-migration-best-practices).

1.  After the migration is done, right-click and select **VM > Stop Replication**. This command stops replication for the on-premises machine and cleans up replication state information for the VM.

1.  Install the Azure VM Windows or Linux agent on the migrated machines.

1.  Perform any post-migration app tweaks, such as updating database connection strings, and web server configurations.

1.  Perform final application and migration acceptance testing on the migrated application now running in Azure.

1.  Cut over traffic to the migrated Azure VM instance.

1.  Remove the on-premises VMs from your local VM inventory.

1.  Remove the on-premises VMs from local backups.

1.  Update any internal documentation to show the new location and IP address of the Azure VMs.

#### Post-migration best practices

*  For increased resilience:

    *  Keep data secure by backing up Azure VMs using the Azure Backup service. [Learn more in Back up a virtual machine in Azure](https://docs.microsoft.com/en-us/azure/backup/quick-backup-vm-portal).
    *  Keep workloads running and continuously available by replicating Azure VMs to a secondary region with Site Recovery.  [Learn more in Set up disaster recovery for Azure VMs](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-tutorial-enable-replication).

*  For increased security:

    *  Lock down and limit inbound traffic access with [Azure Security Center - Just in time administration](https://docs.microsoft.com/en-us/azure/site-recovery/azure-to-azure-tutorial-enable-replication).
    *  Restrict network traffic to management endpoints with [Network Security Groups](https://docs.microsoft.com/en-us/azure/virtual-network/security-overview).
    *  Deploy [Azure Disk Encryption](https://docs.microsoft.com/en-us/azure/security/fundamentals/azure-disk-encryption-vms-vmss) to help secure disks, and keep data safe from theft and unauthorized access.
    *  Read more about [securing IaaS resources](https://azure.microsoft.com/en-us/services/virtual-machines/secure-well-managed-iaas/), and visit the [Azure Security Center](https://azure.microsoft.com/en-us/services/security-center/).

For monitoring and management, consider using [Citrix Application Delivery Management service](/en-us/citrix-application-delivery-management-service/overview.html).

Consider deploying [Azure Cost Management](https://docs.microsoft.com/en-us/azure/cost-management-billing/cloudyn/overview) to monitor resource usage and spending.

>**Checkpoint: Azure Migration**
>
>1.  Validate the IP address assigned to the file server on Azure after migration.
>
>1.  Ensure the IP address has been updated on the DNS server.
>
>1.  Connect to the Citrix Gateway with a new user account.
>
>1.  Open the Citrix Virtual Apps and Desktops service desktop.
>    ![Citrix Virtual Apps and Desktops service desktop](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cvads-desktop.png)
>
>1.  Close the session and ensure that the user’s profile has been created on the file server in Azure.
>    ![User profile on Azure server](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-profile.png)

## Move Citrix workload to Azure

Next we move our Citrix workload to Azure.  

The following diagram shows the Azure and Citrix Cloud components that have been migrated and our remaining on-premises environment.

![Citrix workload on Azure](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_citrix-workload-on-azure.png)

### Step 1: Prerequisites

1.  Deploy two Cloud Connectors on Azure.

    ![Cloud Connectors on Azure](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connectors-azure.png)

1.  Create a resource location in Citrix Cloud.

    ![New resource location Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_new-resource-location.png)

1.  Deploy Cloud Connector software on the servers and attach them to the new resource locations.

    ![Deploy Cloud Connector](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connector-resource-location.png)

### Step 2: Create Citrix workload

1.  Create the basic configuration
    1.  hosting
    1.  machine catalogs
    1.  delivery groups
    1.  publish applications
    1.  bind Citrix policies

![Azure delivery group](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-delivery-group.png)

### Step 3: Configure the end user access layer

>**Note:**
>
>If you are not using Citrix Workspace and Citrix Gateway service, follow these steps.

1.  Connect to the on-premises StoreFront server to add the Azure Cloud Connectors as delivery controllers on each store.

    ![Connect to on-prem StoreFront server](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_connect-on-prem-storefront.png)

1.  Configure user mapping and [multi-site aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) (during the transition step) for the internal store.

    ![Internal store configured](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_internal-store-configured.png)

1.  Configure user mapping and multi-site aggregation for the external store.

    ![External site aggregation](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_external-aggregation.png)

1.  Add the Azure-hosted Cloud Connectors as the STA in the Citrix Gateway configuration in StoreFront.

    ![Azure hosted Cloud Connectors as Gateway STA](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-connectors-sta-server-binding.png)

1.  Add the Azure-hosted Cloud Connectors as the STA in the Citrix Gateway Virtual Servers configuration on Citrix ADC.

    ![Gateway virtual servers](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_gateway-virtual-servers.png)

1.  Ensure that all STAs are **UP**.

>**Checkpoint: Citrix workload**
>
>1.  Connect to your Citrix Gateway.
>
>1.  Open the Azure-hosted Citrix Virtual Apps and Desktop service published desktop.
>    ![Azure-hosted CVAD service desktop](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-hosted-cvad-service-desktop.png)
>
>1.  Ensure that the name of the desktop is the one you provisioned on Azure.
>
>1.  Launch apps to make sure they launch. Make note of app performance. If your apps perform poorly, you may need to adjust your Azure machine sizing.
