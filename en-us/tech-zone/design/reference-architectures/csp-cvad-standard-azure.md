---
layout: doc
h3InToc: true
contributedBy: Darren Harding
specialThanksTo: 
description: The Citrix Service Providers Reference Architecture, provides architectural guidance for Citrix Service Providers to utilize the Virtual Apps and Desktops Service for Azure (formerly Citrix Managed Desktops), and Citrix Cloud technologies to offer services to customer's and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base shared across multiple tenants and multiple geographies, using a single pane of glass.
---
# Citrix Service Provider  Virtual Apps and Desktops Standard for Azure

## Audience

This document is intended for IT decision makers, consultants, solution integrators, cloud engineers, and Citrix Service Providers. Whom are seeking to deploy or migrate existing environments to a multitenant Citrix Cloud.

This document covers the multitenant aspects of Citrix Virtual Apps and Desktops Standard for Azure.

## Abstract

The business objective for Citrix Virtual Apps and Desktops Standard for Azure is to offer a turnkey managed multitenant desktops-as-a-service solution to Citrix Service Providers (CSP).
Enabling CSPs to deliver Windows and Linux Applications and Desktops to their customers. Without having to manage complex deployments and infrastructure and simplifying the management of Operating System updates, patching, and rapid deployment.

Citrix Virtual Apps and Desktops Service for Azure is a unique offering including the ability to integrate with domain and non-domain joined machines. Allowing simple and flexible Windows multi or single user Desktops. The goal is to eliminate the complexity of providing hosted services in a multitenant secure and isolated environment.

The Citrix Virtual Apps and Desktops Standard for Azure Service is only available to Citrix Service Providers.

## Multitenancy

The Citrix Virtual Apps and Desktops Standard for Azure. Has built-in multitenancy to allow CSPs to managed multiple customers as individual instances, distribute licenses and administer with the built-in management tools and consoles. Multitenancy includes the ability to easily customize each customer's workspace and easily track customer usage.

## Scenarios

The Citrix team has tested the following common scenario focusing on common multitenant workloads for Citrix Service providers. Using the turnkey Citrix Virtual Apps and Desktops for Azure experience for, to create customer workloads including bundled Azure capacity and flexible billing options if needed.

Partners and customers have the option of using the Citrix Managed Azure Subscription or bring your own Azure Account (BYOA) option.

The Partner and Customer are free to choose the best Architecture to deploy the Citrix Virtual Apps and Desktops Standard for Azure Workloads. They can select either Citrix Managed Azure, Partner Managed Azure, or using their Own Azure (Managed by the Partner).

A Partner can mix and match these features with in the Citrix Virtual Apps and Desktops Standard for Azure Service to offer a wide range of deployments to their customers

### Where to use Multitenancy

A CSP can manage single or multitenant Citrix Virtual Apps and Desktops Standard for Azure under their Partner account. The deployment depends on the accompanying services offered to the end customer. Generally if a customer only uses applications and desktops they are better suited in a multitenant environment. However if they would also like to add other workspace services. Such as Citrix Endpoint Management, or Federated Authentication Services to authenticate to Citrix Cloud, it is preferable to manage them as a single tenant.

### Citrix Managed Azure

As previously mentioned Citrix Managed Azure is deployed automatically with the Citrix Virtual Apps and Desktops Standard for Azure Service. This subscription can be used a as single point of billing for customer workloads. Helping the Citrix Service Provider Partner to easily manage licensing, and billing in one single place. The Citrix team recommends that in a multitenant environment, Citrix managed Azure subscriptions are not shared between customers. With each customer is assigned their own Citrix manged Subscription. There is a limitation of 1000 seats per Azure Subscription, and also as CSP you can add a Citrix Managed Azure Subscription dedicated to a customer

### Partner Managed Azure

A Partner or Customer can provide Azure subscriptions, and connect them to Citrix Virtual Apps and Desktops Standard for Azure. Giving the flexibility to add other services to the dedicated Azure, and more network connectivity, and authentication options. This allows flexibility on the Azure Subscription. Partner Managed Azure only supports Domain Joined Workloads at this moment.## Deploy

## Workloads

****

### Desktop Workloads

Citrix Virtual Apps and Desktops Standard for Azure is an authorized Microsoft extension to their Windows Virtual Desktop Solution. Delivering Windows 10 single and multi-session, Windows 7 Extended Security Updates (ESU) single session, and single and multi-session Server Operating Systems including Windows Server 2012 R2, 2016 and 2019.

### Workloads with Citrix Managed Azure

When using Citrix managed Azure a Partner can create multitenant Domain Joined Workloads. Using this scenario Citrix would recommend that a Partner has a dedicated Citrix manged Azure Subscription per Customer. The Partner enables authentication to the Workloads using Azure VNet Peering. Connectivity to on-premises resources can be added via the Azure Subscription with Citrix SD-WAN integrated with Citrix Virtual Apps and Desktops Standard for Azure or Express Route.

### Deploy Workloads on Partner Managed Azure

If the customer requires domain joined workloads they can also use the use Partner Managed Azure. This Azure subscription is dedicated to a customer. Either be using a Service Provider managed Azure subscription or one owned by the customer and managed by the partner. This deployment requires the connectivity of Active Directory or Azure Active Directory. The Azure subscription must be connected before a multitenant catalog is created

### Non-Domain Joined Workloads

Non-Domain joined workloads using Citrix managed Azure Active Directory, or connect to a partner or customer managed AAD. To connect this deployment option to on-premises resources we needs to use a VPN or similar technology on the VDA (maybe explain about partner managed and customer)

NDJ workloads are a simple use case for quick creating unmanaged virtual machines that are not managed by Active Directory and can be used for several use cases.

[Citrix Gateway Service – Points-of-presence](https://support.citrix.com/article/CTX270584)

## Azure Architecture

A Citrix Service Provider can provide Azure consumption in various combinations. The Citrix team recommends that each customer is hosted is a dedicated Citrix Managed Azure subscription, or in a separate Azure subscription. Belonging to the Service Provider or the Customer and managed by the Service Provider. This architecture provides simple manageability for customer isolation, allows for workload separation, easy billing units.

Single Tenants managed by a CSP have their own Manged Azure Subscription, Partners share one Managed Azure Subscription

The Citrix Managed Azure has a Managed Active Directory where users can be invited, and shadow accounts are created for the users.

The Citrix Service Provider Team has only tested a few of the possible scenarios available to Citrix Virtual Apps and Desktops Standard for Azure

## Citrix Cloud Connectors

In the Citrix Virtual Apps and Desktops Service for Azure Service the Cloud Connectors are created automatically in the corresponding Azure Subscription

### VNets and AAD

Azure VNET Peering - Connect an existing Azure account for authentication and Domain joined virtual machines. All traffic goes through the Azure backbone. Peering also connect to other Azure Customer resources.

For Domain joined workloads the Partner or Customer needs either Active Directory or Azure Active Directory Domain Services to manage the VDIs

### SD-WAN

Connect an on-premises access for Domain Joined virtual machines and access to customer resources. Service Providers can use as an option when their customer have multiple branches and locations.

### Citrix Managed Images

Citrix Virtual Apps and Desktops Standard for Azure Includes the option for turnkey creation of catalogs with readily prepared images from the Shared Hosted and Dedicated Desktops models. These images are prepared with the Virtual Delivery Agent preinstalled and periodically updated by Citrix. There are also Windows 10 imaged pre-prepared with Microsoft Office.

Citrix will also include Linux Prepared Imaged in the future

### Customer or Partner Images

The CSP Partner or on behalf of their customer's can also upload images prepared and be available in Azure to use in the catalogs

## Isolation using dedicated Azure Subscriptions

Citrix Recommends that a dedicated Azure Subscription is used. This solution gives the Partner and Customer increased control over their security and isolation. Including dedicated Azure AD, Role Based Access Control, maintenance windows, consolidated services, subscriptions to other Azure services such has Office 365. The benefits of multiple Azure subscriptions outweigh the disadvantages of increased network complexity and management

### Managing VNets

Before Creating a catalog using an Azure subscription (Customer or Partner owned and /or controlled), we need to create a VNet peer before creating a catalog.

## Management

Citrix Manged Azure Standard for Azure Administrators and Operations teams have can monitor their Customer Deployments through the monitor tab. The teams then have the ability to manage the environment, record usage, monitor consumption, filtering on a per Customer basis.

To manage your customer's, on the Virtual Apps and Desktops Standard for Azure service page, select the ***Monitor*** Tab:

[![CSP-Image-020](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_020.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_020.png)

The information can be viewed per Customer Catalog

[![CSP-Image-021](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_021.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_021.png)

Or per Time Period

[![CSP-Image-022](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_022.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_022.png)

Monitoring can also be filtered per customer

[![CSP-Image-019](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_019.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_019.png)

## Managing CSP Single and multitenant customer's

A CSP can manage their customers under the same Cloud Account. With multitenancy the CSP has the advantage of managing all of their customer's using the same Cloud Console. With the advantages of reduced management time and also aggregation of resources. For a CSP to manage a Single Tenant with a dedicated Citrix Virtual Apps and Desktops Standard for Azure subscription. They can navigate to their customers, selecting the three-dot menu, expanding Manage services and selecting Citrix Virtual Apps and Desktops Standard for Azure

[![CSP-Image-023](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_023.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_023.png)

The Admin is then redirected to the customer's Dedicated Citrix Virtual Apps and Desktops Standard for Azure instance.

### Power and cost Management

Create Power Management Schedules for each customer Catalog to ensure that the minimum number of machines are available for the workload.
You can choose from four cost saving pre-sets to generate a generic plan that can later be modified to suit your customers’ needs.

## Getting started

Before starting a new Virtual Apps and Desktops Standard for Azure deployment we need to consider the costs and limitations of the architecture.

To start to understand the overall costs, the Citrix team has created a Cost Calculator. This calculator helps CSPs plan and compare before committing to a Citrix managed Azure or Partner Managed Azure subscription:

<https://costcalculator.apps.cloud.com>

Also depending on the size of the Partners customers we can identify how we are going to allocate Azure subscriptions:

[Citrix Virtual Apps and Desktops service instance Limits](/en-us/citrix-virtual-apps-desktops-service/limits.html#configuration-limits)

[Azure subscription and service limits, quotas, and constraints](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/azure-subscription-service-limits)

### Deploying the multitenant Virtual Apps and Desktops Standard for Azure

The first process it to order the multitenant Virtual Apps and Desktops Standard for Azure entitlement from the your distributor ($0 Stocking order).

Then the Partner needs to deploy the Citrix Virtual Apps and Desktops Standard for Azure to a customer before deploying catalogs to that customer. To start navigate to the Customer Dashboard. Locate the customer.

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_009.png)

Choose **Add Service**, and select **Virtual Apps and Desktops Standard for Azure**

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_010.png)

After the service has been added, it will take a few minutes to complete on the back end.

[![CSP-Image-011](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_011.png)

### Domain Joined with Citrix Managed Azure subscription

Create a Domain Joined Custom Catalog.

Select **create a catalog**

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_001.png)

Select **Custom Create**.
Choose the Customer.
Choose a Machine type (Single or Multi-session, Desktop or Server Operating System).
Choose an Azure subscription.
Select a main Image.
Select the resource group.

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_002.png)

The Partner can also choose a Partner Managed or Citrix Managed Azure Subscription.

[![CSP-Image-024](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_024.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_024.png)

Add the domain credentials and select the **Organizational Unit** for the Catalog.

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_003.png)

Choose the **Azure Virtual Machine Type**

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_004.png)

And Name the catalog according to your nomenclature

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_005.png)

To reduce Azure costs, it is advisable to set a power schedule according to the customer's usage schedule

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_006.png)

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_007.png)

### Non-Domain Joined with Citrix Managed Azure subscription

When creating non-Domain joined Workloads. The workloads can only be located within the Citrix Managed Azure. Because of this limitation no customer cloud account in the customer dashboard is needed. To deploy with this option, **create a new** Catalog using **Custom Create**

Choose the Partner Account as the Customer

Select a Machine type needed

The subscription is 'Citrix Managed'

Choose a main Image

And the network connection is the VNET connected to the Citrix Managed Azure

[![CSP-Image-012](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_012.png)

No domain credentials are needed for the creation of the catalog

Choose an Azure machine Type, select the number of machines needed, and Create the catalog

[![CSP-Image-013](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvad-standard-azure_013.png)

## Adding Capacity

There are limits on the capacity of the Azure resource locations. Also the Cloud Connectors
can be split between Resource locations to increase capacity for customers.

[Scale and size considerations for Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/cc-scale-and-size.html)

[Citrix Virtual Apps and Desktops Standard for Azure Limits](/en-us/citrix-virtual-apps-desktops-standard-azure/limits.html)

### Resources

[Using Citrix SD-WAN with Citrix Virtual Apps and Desktops Standard for Azure](/en-us/citrix-managed-desktops/network-connections.html#sd-wan-connection-requirements-and-preparation)

[Azure VNet peering requirements and preparation[(/en-us/citrix-managed-desktops/network-connections.html#azure-vnet-peering-requirements-and-preparation)

[Citrix Managed Desktops service for Citrix Service Providers](/en-us/citrix-managed-desktops/setup-for-citrix-service-providers.html)

### Troubleshooting

[Troubleshooting Catalog Creation Failures](https://support.citrix.com/article/CTX224151)

[Citrix Virtual Apps and Desktops Standard for Azure Troubleshooting](/en-us/citrix-managed-desktops/troubleshoot.html)
