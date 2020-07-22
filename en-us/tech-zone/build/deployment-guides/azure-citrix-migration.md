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