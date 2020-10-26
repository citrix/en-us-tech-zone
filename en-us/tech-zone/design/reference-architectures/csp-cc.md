---
layout: doc
h3InToc: true
contributedBy: Darren Harding
description: The CSP Content Collaboration Service and Workspace integration simplifies the Citrix Cloud reseller management, customer deployment and provides real-time file sync to data in one secure centrally managed platform.
---
# Citrix Service Provider Content Collaboration Workspace Integration

## Audience

This document is intended for IT decision makers, consultants, solution integrators, cloud engineers, and CSP Partners. Whom are seeking to deploy a CSP Content Collaboration environment integrated with Citrix Workspace.

## Introduction and Scope

This article provides architectural guidance for Citrix Service Providers (CSP) wanting to resell Content Collaboration services to customers and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base.

The Citrix CSP Reference Architecture is designed to be flexible and can be used to implement hosting environments within virtually any infrastructure, during any phase of implementation.

This documentation describes the design and implementation of the Content Collaboration infrastructure. Intended to be vendor agnostic and uses common wording for the specific technology in use.

This version of the Reference Architecture focuses on the Workspace integration of Content Collaboration for Citrix Service Providers, and is designed in principle for new Content Collaboration Resellers.

We are continually expanding the scope of the reference architecture to cover the overall workspace services for CSPs in future versions.

## Workspace Integration with Content Collaboration

The CSP Content Collaboration reseller Service provides the platform and simplifies the management and deployment of collaboration for their customers.
CSP Content Collaboration enables true business-class data security for Service Providers, their customers and users. While maintaining total control, the Customers can access, synchronize, and securely share files from anywhere, on any device. Including automated feedback and approval workflows to streamline and maximize productivity.

### Content Collaboration control subsystem

Citrix maintains and provides management and control functionality, in Citrix Online data centers. The control subsystem handles various operations not related to file contents and performs storage zones health checks.

[Storage zones controllers](/en-us/storage-zones-controller/5-0.html#components)

### Storage Zones

Is a Single or Multitenant location that provides data storage. Where the Citrix Service Provider can store customer data on-premises or in a Public Cloud.

[Storage zones](/en-us/storage-zones-controller/5-0.html#data-storage)

### Storage zone connectors

Provide users secure access to documents or specified network file shares including SharePoint sites, site collections, and document libraries

[Storage zones](/en-us/storage-zones-controller/5-0.html#components)

## Architecture Models for Citrix Service Providers

In order to use Content Collaboration, Citrix Service Providers need to have a reseller account. Enabling them to manage their customers, users, and Storage Zones.

There are two storage options available to Citrix Service Providers.

 -On-premises or Citrix Cloud Single Tenant
 -on-premises Multitenant

Service providers can host customers using a mixture of Storage Zones. However, it is not easy to switch between Storage Zones once the configuration is set. Requiring a migration of data between Storage Zones.

### Single Tenant Storage Zones

The first option is to have the Service provider manage individual customers on their own storage zones. The advantage of this model is complete isolation from other customers. Usually located within dedicated Resource Locations. These Storage Zones can be situated within the CSP or customers data center or using the storage from Citrix Cloud.

[Resource Locations](/en-us/tech-zone/design/reference-architectures/csp-cvads.html#architecture-models-for-citrix-service-providers)

This model allows the customer and CSP the greatest flexibility. However dedicated components and resources are needed per customer.

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-cc_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_001.png)

### Multitenant Storage Zones

The second option is for the Service Provider to manage multiple customers on a shared control plane and storage zone. Aligning with the Shared Resource location simplifying the configuration and reducing one-time processes. Giving the CSP the ability to rapidly add additional customers, reducing complexity and cost.

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-cc_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_002.png)

## Management

The design of the CSP Reseller Account for Content Collaboration. Is to centrally control and manage customer accounts and Storage Zones, additional control is also allowed under the customer accounts. Giving them the flexibility to manage users' settings for their organization

### Storage Zone Management

To manage the Storage Zones, connect to the CSP Partner Cloud Account, choose Manage

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-cc_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_003.png)

Then select Advanced Preferences, and Tenant Management

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-cc_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_004.png)

The list of managed customers is displayed under Tenant Management with the Storage Zones locations and the License usage.

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-cc_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_005.png)

To manage the customer, select the hyperlink on the Account Name and you are redirected to the Customer Content Collaboration configuration page under ShareFile.com. To manage under Citrix Cloud, use the link in the manage customers page.

## Deployment Considerations

For a Service Provider to add the Content Collaboration service they need to first activate the service in their Cloud Account. Then convert the service to a Citrix Service Providers Reseller account using the Stocking SKU provided from their distributor. Creating a Content Collaboration subdomain in their relevant Citrix Region. Matching of their cloud account. Non-United States Partners need to verify they regions and locations of any service before adding the Content Collaboration service.

### Workspace Integration

One of the best features of the Citrix Service Providers Content Collaboration Service. Is the ability to easily add customers and users to the workspace and enable integration between the Applications, Desktop, and File Services.

### Preparing Storage Zones

After the Reseller account configuration, the Service Providers can add a Single or Multitenant Storage Zone to their account. They need to prepare and install the Storage Zone Controller before adding customers.

[Install storage zones controller and create a storage zone](/en-us/storage-zones-controller/5-0/install/controller.html)

## Deployment Process

### Citrix Service Provider Content Collaboration Reseller Account

To deploy the Content Collaboration Service the Citrix Service Provider can Request a new account. The CSP Team is currently working on a procedure to link existing resellers and tenants.

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-cc_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_006.png)

When the Reseller account is available the Citrix Service Provider can add content collaboration to new or existing customers.

### Citrix Service Provider Customer Content Collaboration Account

With the Service Provider reseller account, the Content Collaboration service can be added to existing customers.

To onboard a new customer, navigate to the customers section on the Cloud Portal. Select a customer, choose add services, and select the Content Collaboration Service.

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-cc_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_007.png)

Adding the service starts the reseller process.

Select the Customer use case, this information is useful for identifying Customer Services and reporting, however it does not affect the configuration of the service.

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-cc_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_008.png)

Choose a unique Content Collaboration subdomain for your customer and select Check Availability

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-cc_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_009.png)

Select the Account type (entitlement)

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-cc_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_010.png)

Select the corresponding plan needed

List of Plan features can be found here:

[Citrix Content Collaboration Feature Matrix](https://www.citrix.com/products/citrix-content-collaboration/feature-matrix.html)

[![CSP-Image-011](/en-us/tech-zone/design/media/reference-architectures_csp-cc_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_011.png)

Set up the Master Admin and Support details for the Content Collaboration Service

[![CSP-Image-012](/en-us/tech-zone/design/media/reference-architectures_csp-cc_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_012.png)

Select the Storage Zone for the Customer (Partner Hosted Single and multitenant Storage Zones need to be configured before continuing).

[![CSP-Image-013](/en-us/tech-zone/design/media/reference-architectures_csp-cc_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_013.png)

If multitenant is selected, choose the relevant StorageZone

[![CSP-Image-014](/en-us/tech-zone/design/media/reference-architectures_csp-cc_014.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_014.png)

After the StorageZone process is complete the service is added to the customer and is immediately available for use.

### Customer Administration

The management of the administrators and users of the customers Content Collaboration Service are controlled under the Customers Content Collaboration Service. Located in the Customer Cloud Account. This differs from the Citrix Virtual Apps and Desktops Service where the management is under the Service Providers Cloud Account.

### Role Based Access Control

To allow the CSP to manage the Content Collaboration Service. They must be added to the administrators under the Identity and Access Management form. Select the Content Collaboration Administrator Role.

[![CSP-Image-015](/en-us/tech-zone/design/media/reference-architectures_csp-cc_015.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_015.png)

## Workspace integration

To enable Workspace integration, under the Customers Cloud account, select Workspace Configuration, Service integrations and Select Enable

[![CSP-Image-016](/en-us/tech-zone/design/media/reference-architectures_csp-cc_016.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_016.png)

### Adding Workspace Enabled users

To add users with Workspace integration. There needs to be a connection between the Content Collaboration User and the corresponding user in Active Directory. Therefore, the email address needs to be contiguous, for example;

Email address in Content Collaboration

[![CSP-Image-017](/en-us/tech-zone/design/media/reference-architectures_csp-cc_017.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_017.png)

Email address in Active Directory

[![CSP-Image-018](/en-us/tech-zone/design/media/reference-architectures_csp-cc_018.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cc_018.png)

When the user logs on to the Workspace, using the Active Directory sAMAccountName or UPN. They will have the Content Collaboration integration, even they do not have any Applications or Desktops assigned.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [Source Diagrams](https://citrix.sharefile.com/d-s35a988f69ff4365b)

## References

[CSP CVAD RA](/en-us/tech-zone/design/reference-architectures/csp-cvads.html)

[CSPs and Citrix Content Collaboration](https://support.citrix.com/article/CTX208590)

[Citrix Service Provider – Deliver ShareFile Service in Minutes!](https://www.citrix.com/blogs/2018/03/14/citrix-service-provider-deliver-sharefile-service-in-minutes/)

[Content Collaboration with on-premises storage zones](/en-us/tech-zone/design/reference-architectures/customer-storage-zones-on-premises.html)

[Content Collaboration with storage zones on Azure IaaS](/en-us/tech-zone/design/reference-architectures/storage-zones-azure-iaas.html)

[Citrix Files Deployment Guide with Citrix Virtual Apps and Desktops Service](/en-us/tech-zone/build/deployment-guides/citrix-files.html)
