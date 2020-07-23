---
layout: doc
description: Copy & paste description from TOC here
---
# Migrate Citrix Virtual Apps and Desktops to Microsoft Azure

## Contributors

**Author:** [Arnaud Pain](URL)

## Objective

In this document, you'll discover how to migrate from Citrix Virtual Apps and Desktops to Citrix Virtual Apps and Desktops service and from VMware vSphere on-premises to Microsoft Azure. Migrating to cloud resources modernizes your deployment, providing enhanced elasticity, scalability, and management.

## Project overview

The guidance documented here is based on deployment in a lab environment that was reviewed and approved by Citrix and by Microsoft. The initial and final deployments represent typical customer environments.

We migrated these key products and components:

*  Citrix Virtual Apps and Desktops on-premises to Citrix Cloud and Citrix Virtual Apps and Desktops service
*  Workspace Environment Management on-premises to Workspace Environment Management service
*  On-premises StoreFront and Citrix Gateway to Citrix Workspace and Citrix Gateway service
*  On-premises vSphere workloads to workloads in Azure
*  On-premises file servers to Azure
*  Application back-end

The following diagram shows the migration process.

![Migration process](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_full-migration.gif)

### The best migration path

In our experience and testing, the best migration path follows these steps:

1.  Prepare the Azure subscription to receive the workloads from your on-prem deployment.
1.  Configure site-to-site communication.
1.  Set up Citrix Cloud.
1.  Migrate on-premises Citrix Virtual Apps and Desktops to Citrix Virtual Apps and Desktops service using the automated configuration tool developed by Citrix.
1.  Migrate on-premises Workspace Environment Management to Workspace Environment Management service.
1.  Use Azure tools to assess your on-premises environment.
1.  Use Azure tools to migrate your on-premises servers to Azure.
1.  Configure Citrix workload on Azure.
1.  Migrate from StoreFront and Citrix Gateway to Citrix Workspace and Citrix Gateway Service.

![Migration path](migration-path.png)

Doing the migration in the specified order provides a more organized and orderly migration. It allows you to run products, components, and services in a combination of on-premises and cloud deployments, minimizing disruption to daily workloads.

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

Citrix recommends an OU structure, like the one illustrated in the following image, that integrates with your current structure to host the machines and the user accounts for the Citrix Cloud environment.

![Example Active Directory structure](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_example-active-directory-structure.png)

### Active Directory preparation

For our lab environment, we defined GPOs that:

1.  Configure Citrix Workspace Environment Management to define the infrastructure server.
1.  Configure the Citrix Workspace app to allow SSO and to populate applications on the user’s desktop.
1.  Configure a logon script to apply a background using BGInfo. For the purposes of our demo, we apply different background settings based on the migration project’s phase.

With the basic lab setup in place, we’re ready to start the migration.

We recommend that you create the AD structure that will host your Azure workload before you start the migration project. It’s also helpful to have checkpoints in place to help you validate the successful completion of each step.

## Prepare the Azure subscription

The first step to migrating your workloads to Azure is to set up the Azure environment. If you’re new to Azure, or if your environment has some different components from our lab setup, Microsoft offers many helpful resources, like [New to Azure? Follow these easy steps to get started](https://azure.microsoft.com/en-us/blog/new-to-azure-follow-these-easy-steps-to-get-started/) and the [Azure Migrate documentation](https://docs.microsoft.com/en-us/azure/migrate/).

For a detailed look at how to architect Citrix Virtual Apps and Desktops on Azure, see [Citrix Virtual Apps and Desktops Service on Azure](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html).

We prepped the Azure environment in five basic steps:

Step 0: Select an Azure subscription model
Step 1: Select Azure regions
Step 2: Create a resource group
Step 3: Create a virtual network.
Step 4: Deploy a server in Azure that will serve as the domain controller.

When our Azure subscription is set up and ready for the next step, our environment looks like the one in the following diagram. The resources and workloads are all on-premises and we have a resource group, a virtual network, and a server in Azure.

![Environment with Azure prepped](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_azure-subscription-prepped.png)

### Step 0: Select an Azure subscription model

Set up your Azure subscription.

Selecting a subscription model is a complex decision that involves understanding the growth of your Azure footprint within and outside the Citrix deployment. Even if the Citrix deployment is small, you might still have a large amount of other resources that are reading/writing heavily against the Azure API, which can have a negative impact on the Citrix environment. The reverse is also true, where many Citrix resources can consume an inordinate number of the available API calls, reducing availability for other resources within the subscription. For information about Azure subscription limits and quotas, see [Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits).

### Step 1: Select Azure regions

An Azure region is a set of data centers deployed within a latency-defined perimeter and connected through a dedicated regional low-latency network. Azure gives customers the flexibility to deploy applications where they need to. Azure is generally available in 60 regions and 149 countries around the world.
Microsoft provides details in [https://azure.microsoft.com/en-us/global-infrastructure/regions/](https://azure.microsoft.com/en-us/global-infrastructure/regions/).

### Step 2: Create a resource group

A resource group in Azure is a collection of assets in logical groups for easy or even automatic provisioning, monitoring, and access control, and for more effective management of their costs. The benefit of using resource groups in Azure is that you can group related resources that belong to an application, because those resources share a unified lifecycle from creation to usage and finally, de-provisioning.

The key to having a successful resource group design is understanding the lifecycle of the resources that are included in the resource groups.

If you need guidance setting up a resource group, Microsoft provides details in [What is Azure Resource Manager](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview).

One or more resource groups can be applied to a machine catalog during initial creation. These resource groups cannot be shared across machine catalogs. Resource groups are limited to 240 Citrix MCS VMs due to the 800 per resource limit in a resource group. See [CTX237504](https://support.citrix.com/article/CTX237504) for more details about this limitation.

Resource groups are tied to machine catalogs at creation time and cannot be added or changed later. To add resource groups to a machine catalog, the machine catalog must be removed and recreated.

### Step 3: Create a virtual network in Azure

The virtual network is required to enable secure communication among the resources in your environment. For more information about creating virtual networks in Azure, see [Quickstart: Create a virtual network using the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-network/quick-create-portal). For help with configuring a virtual network, see [What is Azure Virtual Network](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-networks-overview).

### Step 4: Deploy a server in Azure

With our resource group and virtual network established, we’re ready to deploy a server in Azure. We use the server to test. If you want more detailed information about how to set up virtual machines, see [Quickstart: Create a Windows virtual machine in the Azure portal](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-portal). Microsoft also provides helpful guidance for choosing the right size VM for your workload in [Sizes for virtual machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).

Now that we have our resource groups and virtual network created, and we have a server deployed, our next step is to configure connectivity between our on-premises and Azure environments.

## Configure site-to-site connectivity

To connect your on-premises environment to Azure, you have multiple options:

*  Site-to-site VPN
    Detailed information is availabe in [Create a Site-to-Site connection using the Azure portal (classic)](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal).

*  Citrix SD-WAN
    Step-by-step guidance for Citrix SD-WAN is available in the [Proof of Concept Guide: Citrix SD-WAN Cloud-to-Data Center Connectivity](/en-us/tech-zone/learn/poc-guides/sdwan-cloud-to-onprem-connectivity.html).

*  Express Route
    Step-by-step guidance for Express Route is in the [Express Route documentation](https://docs.microsoft.com/en-us/azure/expressroute/).

For the purposes of our demonstration, we used site-to-site VPN.  

The following diagram shows how our Azure environment is set up to communicate with our on-premises environment.

![Site-to-site VPN configured](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_site-to-site-configured.png)

>**Checkpoint: site connectivity**
>
>When you have connectivity implemented, a checkpoint is to communicate with Azure resources from your on-premises environment and vice versa.
>
>In our case, we used two methods:
>
>Option 1: Test using the ping command between resources in each location.
>
>Option 2: Join the deployed server in Azure to our on-premises AD Domain.

Now that we have site-to-site connectivity configured and validated, our next step is to configure Citrix Cloud.

## Set up a basic Citrix Cloud environment

With our Azure environment established and site-to-site communication working, we’re ready to add Citrix Cloud.

If you’re already using Citrix Virtual Apps and Desktops service, you can skip to the section [Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#Workspace Environment Management service).

We set up the Citrix Cloud environment in three steps:

Step 1: Onboarding

Step 2: Deploy Citrix Cloud Connector

Step 3: Rename the resource location

### Step 1: Onboarding

Ensure you have a valid [Citrix Virtual Apps and Desktops subscription](/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/signing-up-for-citrix-cloud.html).

To proceed, you need access to the Citrix Virtual Apps and Desktops service. If you don’t have access, please work with a Citrix representative.

### Step 2: Cloud Connector installation

The Citrix Cloud Connector lets you configure secure communication between your on-premises deployment and Citrix Cloud. To ensure that your Citrix Cloud Connector is set up to spec, review the [System requirements]( https://docs.citrix.com/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html). You need two VMware ESX VMs to host two Citrix Cloud Connectors.

You can install the Cloud Connector software [interactively](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#interactive-installation) or using the [command line](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#command-line-installation).

### Step 3: Rename the resource location

Because we will have multiple resource locations between on-premises and Azure, it’s useful to have a distinct name for each resource location.

1.  Click the **Arrow** to go back to **Resource Location**.

    ![Resource location back arrow](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_resource-location-back-arrow.png)

1.  Click **...** and select **Rename**.

    ![Rename resource location](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_rename-resource-location.png)

1.  Give your resource location a new name, for example, "On-premises", and click **Save**.

    ![Save new name](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_save-new-resource-location-name.png)

>**Checkpoint: Citrix Cloud Connector**
>
>When your Cloud Connectors are deployed successfully, you should see them with a green bar on the left.
>
>![Cloud Connector status indicators](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cloud-connectors-green-bar.png)

## Migrate to Citrix Virtual Apps and Desktops service

If you’re already using Citrix Virtual Apps and Desktops service, you can skip to the section [Workspace Environment Management service](/en-us/tech-zone/build/deployment-guides/azure-citrix-migration.html#Workspace Environment Management service).

If you’re using Citrix Virtual Apps and Desktops on-premises, you can use the procedures in this section as a guide for migrating to Citrix Virtual Apps and Desktops service.  

For MCS users, the [MCS migration](#MCS migration) section provides detailed migration steps.

If you use PVS, please go to [PVS migration](#PVS migration).

The following diagram shows our cloud environment on Azure after we migrate to the Citrix Virtual Apps and Desktops service.

![Environment with Citrix Virtual Apps and Desktops service configured](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_cvad-service-migrated.png)

### MCS migration

#### Step 1: Preparation

To migrate successfully using the automated configuration tool, for on-premises MCS configured catalogs, you will need to create a host connection, machine catalog, and delivery group within Citrix Virtual Apps and Desktops service.

>**Important:**
>
>To run the migration tool successfully, the delivery groups created in the Citrix Virtual Apps and Desktops service must have identical names to the existing on-premises delivery groups.

##### Establish host connection

The host connection is where you establish the network and the storage–the resources–for a connection. See [Create and manage connections](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html) for detailed information about connections.

##### Create machine catalogs

Use machine catalogs to manage collections of machines as single entities. For more information, see [Create machine catalogs](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html). Machine catalogs are a prerequisite for creating delivery groups. When you run the Azure migration tool, the tool will bring over all of your settings and will create all of the VMs you need. To make sure you have placeholders for your delivery groups, create machine catalogs that contain a minimal number of VMs during this step. In most cases, two VMs is enough to satisfy this requirement.

##### Create delivery groups

Use delivery groups to control access and delivery to machines, applications, and desktops. For details, see [Create delivery groups](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/delivery-groups-create.html). Create delivery groups that have identical names to your existing on-premises delivery groups.

Step 2: Migration

We use a Citrix-developed tool, the automated configuration tool, to migrate Citrix Virtual Apps and Desktops from on-premises to Citrix Virtual Apps and Desktops service. You can [download the automated configuration tool](https://www.citrix.com/downloads/citrix-cloud/betas-and-tech-previews/automated-configuration-technology-preview.html) from Citrix. Documentation for the tool is available on Tech Zone in [Automated Configuration](https://docs.citrix.com/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html). The tool is available as a preview.

Because our on-premises lab is using MCS, we will only migrate the following:

*  Applications
*  Citrix Policies

##### Export the on-prem site configuration

>**Note:**
>
>Because we are using MCS and can only import applications and group policies, we use two options here: `-Applications` and `-GroupPolicies` to export only the required information.

#### Prerequisites

We run the automated configuration tool on a delivery controller, and we need .NET Framework 4.7.2 or later to be installed on that server.

You can download .NET Framework 4.7.2 from: [https://dotnet.microsoft.com/download/dotnet-framework/net472](https://dotnet.microsoft.com/download/dotnet-framework/net472).

Install `AutoConfig_PowerShell_x64.msi` on the delivery controller. Installing the tool creates a desktop icon called **Auto Config** that launches the PowerShell command prompt. You run the Cloud automated configuration cmdlets from the PowerShell command prompt.

#### Export applications

1.  Click the **Auto Config** icon.

    ![Auto Config icon](/en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-desktop-icon.png)

1.  Run the following command: `Export-CvadAcToFile –Applications $true`.

    ![Export apps = true](en-us/tech-zone/build/media/deployment-guides_azure-citrix-migration_auto-config-export-apps-true.png)

#### Export policies

