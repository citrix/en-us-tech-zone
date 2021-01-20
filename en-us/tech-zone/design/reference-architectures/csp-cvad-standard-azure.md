---
layout: doc
description: The CSP Reference Architecture, provides architectural guidance for Citrix Service Providers to utilize the Virtual Apps and Desktops Service for Azure (formerly Citrix Managed Desktops), and Citrix Cloud technologies to offer services to customers and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base shared across multiple tenants and multiple geographies, using a single pane of glass.
---
# Citrix Service Provider Virtual Apps and Desktops Reference Architecture

## Contributors

**Author:** [Darren Harding](https://Twitter.com/DarrenHarding)

**Special Thanks:**

## Audience

This document is intended for IT decision makers, consultants, solution integrators, cloud engineers, and CSP Partners. Whom are seeking to deploy or migrate existing environments to a multitenant Citrix Cloud.

This document is intended to cover the multitenant aspects of Citrix Virtual Apps and Desktops Standard for Azure.

## Executive Summary

The business objective for Citrix Virtual Apps and Desktops Standard for Azure is to offer a turnkey managed multitenant desktops-as-a-service solution to Citrix Service Providers (CSP).
Enabling CSPs to deliver Windows and Linux Applications and Desktops to their customers, without having to manage complex deployments and infrastructure and simplifying the management of Operating System updates, and patching.
Citrix Virtual Apps and Desktops Service for Azure is a unique offering including the ability to integrate with domain and non-domain joined machines.

## Abstract

The Citrix Virtual Apps and Desktops Service for Azure Service for CSPs, is intended to be a simple and flexible Windows multi or single user, Desktop as a Service solution with the ability to create Azure based desktops in minutes, eliminating the complexity of providing hosted services in a multi-tenant secure and isolated environment.
The Citrix Virtual Apps and Desktops Standard for Azure Service is only available to Citrix Service Providers

## Multitenancy

The Citrix Virtual Apps and Desktops Standard for Azure Service has built in multitenancy to allow CSPs to managed multiple customers as individual accounts, distribute licenses and administer with the built in management tools and consoles, with the ability to easily customise each customers workspace and easily track customer usage.

## Workloads

Citrix Virtual Apps and Desktops Standard for Azure Service (CVADSA) is authorised by Microsoft to extend their Windows Virtual Desktop Solution, delivering Windows 10 multi-session, Windows 7 Extended Security Updates (ESU), and Server Operating systems including Windows Server 2012 R2, 2016 and 2019.
A Service provider can choose to allow Citrix to manage the Virtual Desktops using two cloud options, Citrix Managed Azure, or Partner Managed Azure.

## Deployment Scenarios

The Citrix team has tested the following common scenario focusing on common workloads for Citrix Service providers, the plan is to make this a turnkey experience for CSPs, including bundled Azure capacity and flexible billing options if needed.
Partners and Customers can use the flexibility of the product to bring your own Azure Account (BYOA).

### Deploying the multitenant Virtual Apps and Desktops Standard for Azure

Assuming the CSP partner already has the multitenant Virtual Apps and Desktops Standard for Azure entitlement fulfilled (Otherwise it is enabled via a $0 stocking order from the distributor)

To add the Virtual Apps and Desktops Standard for Azure Service to a customer, created and located in the Customer Dashboard. To do this navigate to the Customer Dashboard. Locate the customer.

[![CSP-Image-009](./en-us/tech-zone/design/media/csp-cvad-standard-azure_009.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_009.png)

Choose Add Service, and select Virtual Apps and Desktops Standard for Azure

[![CSP-Image-010](./en-us/tech-zone/design/media/csp-cvad-standard-azure_010.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_010.png)

After the service has been added, it will take a few minutes to complete on the back end.

[![CSP-Image-011](./en-us/tech-zone/design/media/csp-cvad-standard-azure_011.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_011.png)

## Deployment Options

The Partner and Customer are free to choose the best Architecture to deploy the CVADS4A Workloads, they can select either Citrix Managed Azure, Partner Managed Azure or using their Own Azure (Managed by the Partner)
A Partner can mix and match these features with in the CVADS4A Service to offer a wide range of deployments to their customers

### Single vs Multitenant

Where to choose to manage Single or Multitenant Citrix Virtual Apps and Desktops Standard for Azure depends on the services offered to the end customer and also the size of customer hosted.

Mutitenancy offers a single management point for all customers, connections and resources, however only one Federated Authentication Provider is available per Service Provider.

Single tenant has the advantage of allowing a Customer to use any Federated Authentication Service Available to Citrix Cloud.

### Citrix Managed Azure

With the deployment of the CVADS4A Service Citrix automatically enables Citrix Managed Azure. This is a subscription that can be used a as single point of billing for CVADS4A workloads, enabling the Partner to easily manage and billing in one single place (change). The Citrix team recommends that in a multitenant environment, Citrix managed Azure subscriptions are not shared between customers and each customer is assigned their own Citrix manged Subscription. The is a limitation of 1000 seats per Azure Subscription and also as CSP you can add a Citrix Managed Azure Subscription dedicated to a customer

### Partner Managed Azure

A Partner or Customer can provide Azure subscriptions and connect them to CVADS4A, this gives the flexibility to add additional services to the dedicated Azure and more network connectivity and authentication options. This also allow flexibility on the Azure Subscription. Partner Managed Azure only supports Domain Joined Workloads at this moment.## Deploy

## Workloads with Citrix Managed Azure

The Partner can create multitenant Domain Joined Workloads using Citrix managed Azure, using this scenario Citrix would recommend that a Partner has a dedicated Citrix manged Azure Subscription per Customer. The Partner will enable authentication to the Workloads using Azure vNet Peering. Connectivity to on premises resources can be added via the Azure Subscription with Citrix SD-WAN integrated with CVADS4A or Express Route.

## Deploy Workloads on Partner Managed Azure

It the customer requires domain joined Workloads they can also use the use Partner Managed Azure. This is an Azure subscription dedicated to a customer, this can either be using a Service Provider managed Azure subscription or one that is owned by the customer and managed by the partner. This will require the connectivity of Active Directory or Azure Active Directory. The Azure subscription will need to be connected before a multitenant catalog is created

## Non Domain Joined Workloads

Non-Domain joined Workloads using Citrix managed Azure Active Directory, or connect to a a partner or customer managed AAD. To connect this deployment option to on-premises resources we will need to use a VPN or similar technology on the VDA (maybe explain about partner managed and customer)

This is a very simple use case for quick creating unmanaged virtual machines that are not managed by Active Directory and can be used for several use cases.

### Dedicating a single tenant instance

Using a ST CMD instance for Non-domain Joined or Active Directory

Citrix Virtual Apps and Desktops Service for Azure Service has on premises connectivity options for authentication with Active Directory or to access resources. All user traffic leverages the Citrix Gateway Service, preferring Azure Gateway Point of Presences.

[Citrix Gateway Service – Points-of-presence](https://support.citrix.com/article/CTX270584)

## Azure Architecture

A Citrix Service Provider can provide Azure consumption in various combinations; however, the Citrix team recommends that each customer is located a shared Citrix Managed Azure subscription or in a separate Azure subscription belonging to the Service Provider or the Customer and managed by the Service Provider. This architecture provides simple manageability for customer isolation, allows for workload separation, easy billing units.

Single Tenants managed by a CSP will have their own Manged Azure Subscription, Partners share one Managed Azure Subscription

The Citrix Managed Azure has a Managed Active Directory where users can be invited, and shadow accounts will be created for the users.

The Citrix Service Provider Team has only tested a few of the possible scenarios available to Citrix Virtual Apps and Desktops Standard for Azure

## Citrix Cloud Connectors

In the Citrix Virtual Apps and Desktops Service for Azure Service the Cloud Connectors are created automatically in the corresponding Azure Subscription

### VNets and AAD

Azure vNet Peering - Connect an existing Azure account for authentication and Domain joined virtual machines. All traffic will go through the Azure backbone. Peering also connect to other Azure Customer resources.

For Domain joined workloads the Partner or Customer will need either Active Directory or Azure Active Directory Domain Services in order to manage the VDIs

### SD-WAN

Connect an on-premises access for Domain Joined virtual machines and access to customer resources, this is also a great option when the customer has multiple branches

### Citrix Managed Images

CVADS4A Includes the option for turnkey creation of catalogs with readily prepared images from the Shared Hosted and Dedicated Desktops models. These images are prepared with the Virtual Delivery Agent preinstalled and periodically updated by Citrix.  There are also Windows 10 imaged pre-prepared with Microsoft Office.

Citrix will also include Linux Prepared Imaged in the future (not sure if I should include this)

### Customer or Partner Images

The CSP Partner or on behalf of their customers can also upload images prepared and be available in Azure to use in the catalogs

## Tenant Security

### Isolation using dedicated Azure Subscriptions

Citrix Recommends that a dedicated Azure Subscription is used, this gives the Partner and Customer increased control over security, isolation, dedicated Azure AD, RBAC, maintenance windows, consolidated services, subscriptions to other Azure services such has Office 365. The benefits of multiple Azure subscriptions outweigh the disadvantages of increased network complexity and management

### Managing VNets

Before Creating a catalog using an Azure subscription (Customer or Partner owned and /or controlled), we will need to create a vNet peer before creating a catalog.

## Management

Citrix Manged Azure Standard for Azure Administrators and Operations teams have can monitor their Customer Deployments through the monitor tab, this gives the teams the ability to manage the environment, record usage, monitor consumption, filtering on a per Customer basis.

To manage your customers, on the Virtual Apps and Desktops Standard for Azure service page, select the Monitor Tab:

[![CSP-Image-020](./en-us/tech-zone/design/media/csp-cvad-standard-azure_020.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_020.png)

The information can be viewed per Customer Catalog

[![CSP-Image-021](./en-us/tech-zone/design/media/csp-cvad-standard-azure_021.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_021.png)

Ore Time Period

[![CSP-Image-022](./en-us/tech-zone/design/media/csp-cvad-standard-azure_022.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_022.png)

Monitoring can also be filtered per customer

[![CSP-Image-019](./en-us/tech-zone/design/media/csp-cvad-standard-azure_019.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_019.png)

## Managing CSP Single and multitenant Customers

A CSP can manage Single and multitenant customers under the same Cloud Account. With multitenancy the CSP has the advantage of manging all of their customers using the same Cloud Console with the advantages of reduced management time and also aggregation of resources. For a CSP to manage s Single Tenant with a dedicated CVADS4A subscription they can navigate to Customers, selecting the three-dot menu, expanding Manage services and selecting CVADS4A

[![CSP-Image-023](./en-us/tech-zone/design/media/csp-cvad-standard-azure_023.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_023.png)

The Admin will then be redirected to the Customers Dedicated CVAD4A instance.

### Power and cost Management

Create Power Management Schedules for each customer Catalog to ensure that the minimum number of machines are available for the workload.
You can choose from four cost saving pre-sets to generate a generic plan that can later be modified to suit your customers’ needs.

## Deployment process

### Domain Joined with Citrix Managed Azure subscription

Create a Domain Joined Custom Catalog

Select create a Catalog

[![CSP-Image-001](./en-us/tech-zone/design/media/csp-cvad-standard-azure_001.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_001.png)

Select Custom Create
Choose the Customer
Chose a Machine type (Single or Multi-session, Desktop or Server Operating System)
Choose an Azure subscription
Select a Master Image
Select the resource group

[![CSP-Image-002](./en-us/tech-zone/design/media/csp-cvad-standard-azure_002.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_002.png)

The Partner can also choose a Partner Managed or Citrix Managed Azure Subscription

[![CSP-Image-024](./en-us/tech-zone/design/media/csp-cvad-standard-azure_024.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_024.png)

Add the domain credentials and select the Organisational Unit for the Catalog

[![CSP-Image-003](./en-us/tech-zone/design/media/csp-cvad-standard-azure_003.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_003.png)

Choose the Azure Virtual Machine Type

[![CSP-Image-004](./en-us/tech-zone/design/media/csp-cvad-standard-azure_004.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_004.png)

And Name the catalog according to your nomenclature

[![CSP-Image-005](./en-us/tech-zone/design/media/csp-cvad-standard-azure_005.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_005.png)

To reduce Azure costs, it is advisable to set a power schedule according to the customers usage schedule

[![CSP-Image-006](./en-us/tech-zone/design/media/csp-cvad-standard-azure_006.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_006.png)

[![CSP-Image-007](./en-us/tech-zone/design/media/csp-cvad-standard-azure_007.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_007.png)

[![CSP-Image-002](./en-us/tech-zone/design/media/csp-cvad-standard-azure_002.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_002.png)

### Non-Domain Joined with Citrix Managed Azure subscription

When creating non-Domain joined Workloads currently theses can only be located within the Citrix Managed Azure, because of this no customer cloud account is needed to be created.  To deploy with this option, create a new Catalog using 'Custom Create'

Choose the Partner Account as the Customer

Select a Machine type needed

The subscription will be 'Citrix Managed'

Chose a master Image

and the network connection will be the vNet connected to the Citrix Managed Azure

[![CSP-Image-012](./en-us/tech-zone/design/media/csp-cvad-standard-azure_012.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_012.png)

No domain credentials are needed for the creation of the catalog

Choose an Azure machine Type, select the number of machines needed and Create the catalog

[![CSP-Image-013](./en-us/tech-zone/design/media/csp-cvad-standard-azure_013.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_013.png)

### Adding Apps to a Catalog

### Adding Apps to a Catalog

When a catalog is ready the Desktop Subscription is created automatically, to also add application subscriptions chose the calatlog to edit and select the three button menu

[![CSP-Image-014](./en-us/tech-zone/design/media/csp-cvad-standard-azure_014.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_014.png)

On the catalog page select Desktop and Apps

[![CSP-Image-015](./en-us/tech-zone/design/media/csp-cvad-standard-azure_015.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_015.png)

Select Manage Apps

[![CSP-Image-016](./en-us/tech-zone/design/media/csp-cvad-standard-azure_016.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_016.png)

Select how the Apps are going to be added, either from the Start Menu (Automatically detected) or via a custom path

[![CSP-Image-017](./en-us/tech-zone/design/media/csp-cvad-standard-azure_017.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_017.png)

Select the apps and use the Right Arrow to add them to the list of Added Apps

[![CSP-Image-018](./en-us/tech-zone/design/media/csp-cvad-standard-azure_018.png)](./en-us/tech-zone/design/media/csp-cvad-standard-azure_018.png)

Close manage Apps

## Adding Capacity

There are limits on the capacity of the Azure resource locations and also the Could Connectors
Customers can be split between Resource locations to increase capacity and also

[Scale and size considerations for Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/cc-scale-and-size.html)

[Citrix Virtual Apps and Desktops Standard for Azure Limits](/en-us/citrix-virtual-apps-desktops-standard-azure/limits.html)

## Appendix

### Desktops Cost Calculator

The Citrix Virtual Apps and Desktops Standard for Azure team has created a Cost Calculator to help you plan and compare costs before committing to a Citrix managed Azure or Partner Managed Azure subscription:

<https://costcalculator.apps.cloud.com>

### Resources

[Troubleshooting Catalog Creation Failures](https://support.citrix.com/article/CTX224151)

[Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)

[Citrix Virtual Apps and Desktops service instance Limits](/en-us/citrix-virtual-apps-desktops-service/limits.html#configuration-limits)
