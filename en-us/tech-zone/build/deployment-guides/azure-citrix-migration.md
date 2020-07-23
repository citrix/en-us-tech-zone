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
1.  Migrate on-premises Citrix Virtual Apps and Desktops to Citrix Virtual Apps and Desktops service.
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
