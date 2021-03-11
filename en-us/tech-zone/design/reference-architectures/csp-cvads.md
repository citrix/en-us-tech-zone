---
layout: doc
h3InToc: true
contributedBy: Darren Harding
specialThanksTo: Selma Wei, Bala Swaminathan, Jose Augustin, Alex Tompkins
description: The CSP Reference Architecture, provides architectural guidance for Citrix Service Providers to utilize the Virtual Apps and Desktops Service, and Citrix Cloud technologies to offer services to customers and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base shared across multiple tenants and multiple geographies, using a single pane of glass.
---
# Citrix Service Provider Virtual Apps and Desktops Reference Architecture

## Audience

This document is intended for IT decision makers, consultants, solution integrators, cloud engineers, and CSP Partners. Whom are seeking to deploy or migrate existing multitenant Citrix Virtual Apps & Desktops environment to the multitenant Citrix Cloud.

## Executive Summary

The Citrix Service Provider Reference Architecture on Citrix Cloud uses the next generation of cloud service delivery approach. Provides guidance on deployment architectures that scale easily while increasing user centric mobility for an expanding customer base.

Citrix Cloud enables the delivery of Microsoft® Windows® and Linux® workspaces with people-centric secure applications and desktops. Hosted in the Service Provider managed environments from on-premises data centers to private or public clouds.
Citrix Service Providers can take advantage of the flexible licensing programs to deliver cost-effective services based on subscriber usage.

The reference architecture is easily adapted to meet specific provider and subscriber requirements. Allowing Service Providers to deliver a comprehensive set of workspace offerings and price points while simplifying management and scalability.
The cloud-ready services model enables lower infrastructure and administrative costs, speed to market and scalability, greater customer satisfaction, and increased business success.

## Introduction and Scope

This document provides architectural guidance for Citrix Service Providers (CSP) who utilize Citrix Cloud technologies to offer services to customers and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base shared across multiple tenants and multiple geographies.

The Citrix CSP Reference Architecture is designed to be flexible and can be used to implement hosting environments within virtually any infrastructure, during any phase of implementation.

This documentation describes the design and implementation of the Citrix Cloud solution infrastructure to be vendor agnostic and uses common wording for the specific technology in use.

Multitenant resource locations managed by Citrix Service Providers are highly scalable and available. Along with great performance, and end user experience including the management and incorporation of additional services.

This version of the Reference Architecture focuses on Citrix Virtual Apps and Desktops service for Citrix Service Providers. At time of publication not all Workspace Services support multitenancy. We will expand the scope of the reference architecture to cover the overall workspace services for CSPs in future versions.

## Overview

### Citrix Cloud

Citrix Cloud is a platform that hosts and administers Citrix services, such as Citrix Workspace and Citrix Virtual Apps and Desktops. It connects to hosted resources through the Citrix Cloud Connector on any cloud or infrastructure.

Citrix Cloud allows Citrix Service Providers to create multiple types of workspace hosting environments as resource locations (for example on-premises, public cloud, private cloud, or hybrid cloud).

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_001.png)

[More information Citrix Cloud](/en-us/citrix-cloud.html )

### Citrix Workspace

Is a unified secure cloud platform managed by Citrix. Where hosting providers can securely deliver applications and data while maintaining end user experience and productivity, in an increasingly mobile work style.

[More information on Citrix Workspace](/en-us/citrix-workspace.html )

### Citrix Virtual Apps and Desktops service

Around 80% of our Citrix Service Providers offer application and desktops solutions to their customers. Traditionally these offerings are hosted and managed on-premises. The Citrix Virtual Apps and Desktops service adds flexibility by hosting Access the and Control Layers in Citrix Cloud. Providing the Service Provider flexibility and allowing them to focus on their customer workloads from their chosen Public cloud or maintained on-premises.

[More information on the Citrix Virtual Apps and Desktops Service](/en-us/citrix-virtual-apps-desktops-service/setup-for-citrix-service-providers.html )

### Citrix Cloud Connectors

The Cloud Connector is a Citrix component that authenticates and encrypts all communications between Citrix Cloud and Service Provider managed resource location. All communication between Citrix Cloud and the Resource Location environment is encrypted, negating the need for ingress firewall rules.

[More information on Citrix Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/cloud-connectors-install.html )

## Architecture Models for Citrix Service Providers

Citrix Cloud for Citrix Service Providers (CSPs) is the platform for the delivery and management of Citrix technologies. Helping Service Providers extend existing hosting deployments or move their customers to a hosted cloud solution. CSPs can create and deploy secure digital workspaces rapidly using Citrix Cloud, while maintaining the control of sensitive data and resources hosted on-prem or in a chosen cloud.

### Citrix Virtual Apps and Desktops Service for CSP

 For CSPs the traditional deployment is hosting a Citrix Virtual Apps and Desktops environment on-premises. Deployed in the Service Provider’s data center with highly available components. In the Citrix Cloud model for CSP, high availability is built into the management and control plane, with the optional access layer (CloudGateway) Allowing the Citrix Service Provider to focus on the customers’ application data and critical services.  This model also allows the Service Provider to quickly add additional service hosted by citrix in the cloud

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_002.png)

### Security and Isolation

The Citrix Virtual Apps and Desktops Service Architecture consists of layers that connect together to create a complete end-to-end solution for Service Providers. For general conceptual architecture, and to understand how all layers flow together, refer to [Citrix Tech Zone](/en-us/tech-zone.html)

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_003.png)

#### External Access Security

A multitenant environment is isolated from the internet using a blended approach. Leveraging complimentary technologies such as Firewalls, Application Delivery Controllers, Packet Filtering, intrusion detection and prevention systems and so forth. Access to a multitenant network from Citrix Cloud is facilitated either by using the Citrix Cloud Connectors or a Citrix Application Delivery Controller and Citrix StoreFront combination.

#### Management Separation

The core network services for a Service Provider are located in a separate partition that allows the hosting of shared services. Depending on the services offered, the components of this partition can include: Active Directory Domain Controllers, Backup, Automation Services, DNS and so forth.

#### Storage Security

Access to the file repositories of each tenant needs to be separated from other tenants. Isolation can be achieved with using dedicated of shared servers that are protected using security partitions or permissions

#### Tenant Isolation

Partitioning of the tenants is defined by the level of separation demanded by the customers. Citrix recommends that each tenant is placed into a segregated network using a Software defined Network (SDN) for their dedicated workloads and complimentary services. Ensuring that there are effective security isolation boundaries, with managed networks and IP management and routing

### Multitenant Architecture Models

Citrix Cloud multitenant Virtual Apps and Desktops service enables Service Providers to manage multiple customers. Using the single instance of the CVAD service with shared multitenant Citrix Studio and Director consoles. Using Role Based Access Control under the partner cloud account. Citrix license management is also centralized for easy allocation.

Multitenancy capabilities provide economies of scale on a single shared infrastructure while providing the required isolation and data protection. Service Providers can make trade-offs regarding price and features to meet individual tenant requirements.

The tenant isolation in multitenant deployments needs to include appropriate nomenclature to clearly define the objects that are shared or dedicated within the management consoles and control planes. For example {Tenant}-{Location}-{Group}.
The multitenant Virtual Apps and Desktops service supports two architecture models:

  1.Shared Resource Location for multiple tenants.
  2.Dedicated resource location per tenant.

### Shared Resource Location

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_004.png)

[Shared Resource Location, showing an overview of the components that can be shared between tenants under Citrix Service Provider’s cloud account]

In this multitenant architecture model. Customers or tenants of the Service Provider share the partner’s Citrix Virtual Apps and Desktops service, the same resource location, and a hosted Active Directory. Each customer has a dedicated Workspace experience. Allowing them to customize own workspace configurations including authentication, branding, and Workspace URL to closely align with the customer’s business name and brand.

The advantage of this model is to provide the best economics for hosting a wide range of shared customers using shared infrastructure and management components. Service Providers are able to elastically scale easily and incorporate small customers rapidly. Shared resource location can be located on-premises or hosted in a public or private cloud. This option would not allow for hosting at a customer data center.

It is recommended that the Machine Catalogs managed in a shared resource location are dedicated per tenant and assigned to specific Customer scope. However, it is possible to share machine catalogs of some common applications for small tenants, based on the Service Provider’s discretion. The naming convention also extends to objects managed by the Service Provider that are contained within the infrastructure. When managing shared Resource Location Delivery Groups, it is highly recommended that they are dedicated per tenant. Assigning with correspondingly named Active Directory Security groups via managing subscribers page on the cloud control plane. Adding individual users to a delivery group is not recommended due to the high administrative overhead and low scalability.

In summary, under the shared resource location model, each customer has dedicated workspace experience and delivery groups, but share:
• Active Directory
• Resource location and cloud connectors
• Citrix Virtual Apps and Desktops service

The advantages of this model are the best economics, easy and fast cloud transition for existing on-prem multitenant AD environment, and good elasticity and scalability. However, it has limitations for integrating custom environments with complex applications and high compliance requirements.

### Dedicated Resource Location

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_005.png)

[Dedicated Resource Location, showing the dedicated and shared components between tenants under Citrix Service Providers cloud account]

Comparing with the shared resource location model, customers that need more isolation from their hosting provider can use the dedicated Resource location model. Sharing the Service Provider’s Virtual Apps and Desktops service instance, but maintain its isolated active directory, cloud connectors and infrastructure resources.

The dedicated Active Directory and infrastructure resources ensure higher customer isolation and security. Sharing cloud service instance still maintains the ease of the license allocation and centralized management via the partner control plane, Studio, and Monitor console. This model can be hosted using the Service Providers data center, public or private cloud locations, or a customer’s data center.

Citrix recommends rational behind the nomenclature in the Citrix Studio. Indicating information about the workload of the machine catalogs. Assign each catalog and delivery group to specific tenant scope. Similarly named Active Directory Security groups are used instead of adding individual users as subscribers to be assigned to corresponding libraries on the partner cloud portal.

This naming convention is to be extended to all objects assigned or managed for the tenant. Including but not limited to hosting connections, Active Directory objects, network subnet, and so forth.

The dedicated resource location is typically be focused on small to medium customer adoption.
In summary, Customers share CSP’s Citrix Virtual Apps and Desktops service under the dedicated resource location model, but each customer has dedicated:
• Workspace experience, resource location, active directory
• Machine Catalog, delivery groups
• Most likely have dedicated subnet/vNet
• Possible Hosting Connection and different cloud location
• For  small customers, it is not the most economic model, however there are many advantages of this architecture model:
• Less administration cost when compared to complete private isolation
• Centralized management and easy license allocation
• Supports hybrid and multi cloud adoption
• Good flexibility and scalability
• Balanced approach and suits most common use cases

### Private Workspace (Non-Multitenant)

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_006.png)

[Private Workspace, showing that the tenant has a fully isolated Workspace and no service instance is shared from the Service Provider’s Cloud account]

Some large enterprise customers need the ability to have a private Workspace managed by their Citrix Service Provider. For complex applications. With strict security and compliance requirements, the private workspace does not have any shared components with other customers of the same Service Provider. The Service Provider invited by the customer to manage the Cloud environment. This isolation allows for flexibility, and control for the customer and Service Provider. The management and control from the Citrix Service Providers perspective are duplicated with the complete service instance being dedicated to the customer.

The design and deployment for this mode is the same as standalone enterprise accounts on Citrix Cloud. Except the Service Provider is invited to connect and administer these accounts, the deployment model before multitenant support became available at the end of 2019. The detailed design, deployment, and best practices of the single tenant private workspace model can be found on [Citrix Tech Zone](/en-us/tech-zone.html).

### Combination of Different Architecture Models

The different architecture models are not mutually exclusive. A Service Provider can apply each model or mixed architectures under their partner cloud account or manage a separate Cloud Account for their large Customer. The Service Provider models are developed to be flexible to meet the needs of their customers. Offering solutions for providing a return of investment on shared infrastructure or isolation to solve data sovereignty challenges

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_007.png)

[Combined architecture models for customer use cases managed under a single Citrix Service Provider Account]

### Workspace Experience and Authentication

Each customer or tenant has its own workspace, the authentication method used can vary from tenant to tenant if necessary.
There are several identity providers available to the customers of a Citrix Service Provider.

### Active Directory

Default provider for a CSP. Offering the Virtual Apps and Desktops Service and authenticating using Kerberos to a shared or dedicated Active Directory. Allowing multiple Customer domains using distinct UPN suffixes.

Customers under a multitenant setup, with dedicated resource location under the partner account. Can use Active Directory credentials to authenticate users for their Office 365 access once the AD credentials are synced to their Azure AD.

### Time-Based One-Time Password

Either single or multitenant with or without a token as a secondary factor of authentication. Supporting the Times Based One-Time Password standard such as Citrix Single Sign On (SSO), Google, or Microsoft Authenticator.

### Azure Active Directory

For customers with private workspace (Single Tenant), a CSP can connect the customer’s Azure AD to its Citrix Cloud account, and authenticate users to the workspace.

### Citrix Gateway

The Citrix Virtual Apps and Desktops Service supports the use per tenant of an on-premises Citrix ADC Gateway and StoreFront that enables multiples of authentication, and authorization functions.

### OKTA

Using a Cloud based identity provider such as OKTA allows CSPs to authenticate Customers providing a common sign-in procedure. Simplifying the management of multiple authentication points for CSPs. At the time of publication OKTA is in Preview.

### Cloud Federated Authentication Service

The Federated Authentication Service (FAS) is only currently supported on the Service Providers cloud account, and currently is not supported with the Federated Domain option for CSPs. Which allows customers to use their own workspace configuration, found here:

[Configure Federated Domain for the New Customer](#deployment-steps)

 The FAS service enables customers to connect their on-premises FAS deployment to the Service Provider account in Citrix Cloud. It enables end-users to achieve Single Sign On (SSO) to Citrix Virtual Apps and Desktops resources. Using a federated identity provider in Workspace such as Azure Active Directory or OKTA.

## Deployment Considerations

The Citrix Service Providers Cloud model allows for a wide range of deployment options. Suited the needs of the Service Providers’ customers for a wide range of public clouds and hypervisors. Service Providers and their customer can combine these deployment options to provide hybrid cloud migration or multi cloud adoptions.

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_008.png)

[Combined deployment options for tenants managed under a single Citrix Service Provider Account]

When ordering the Citrix Virtual Apps and Desktops Service from your chosen distributor. It is important to consider the diverse customer base managed by the Service Provider via Citrix Cloud. If the customer has an existing Citrix Virtual Apps and Desktops service. They cannot be invited to participate as a tenant under the Citrix Service Provider’s service instance. However, they can be invited to connect subsequently managed by the CSP. Other customers without an existing service instance can be invited or added to the Citrix Service Provider’s instance to either a Shared or Dedicated Resource Location.

In relation to the architecture models, there are two SKUs available to CSPs:

Single Tenant SKU – The existing SKU that the Citrix Service Provider orders for their Customer, and the entitlement and Service instance are allocated on the Customer Cloud Account. SKU maps to the single tenant private workspace model.

Multitenant SKU – The new SKU with entitlement only delivered to the Citrix Service Provider Partner account that allows managing and distributing licenses between multiple customers.

### Data centers

Some Service Providers have invested into long term infrastructure and compute to host services or meet stringent compliance requirements. To utilize these existing resources, the suitable option is to have the Resource location deployed in the Citrix Service Provider Data center.

Citrix Virtual Apps and Desktops Services supports the main hypervisors available. Including integration with Machine Creation Service and Provisioning Services, automating the delivery and operation of the compute resources.

Service Providers normally offer a tired storage option to their customers to ensure that there is distributed performance to allow for their current offering and future expansion.

### Microsoft Azure

Many of our Citrix Service Providers are also Microsoft Cloud Solution Providers. Azure is a public cloud option from Microsoft for Service Providers looking to host workloads in a flexible and elastic way. Citrix Virtual Apps and Desktops has built in support for Azure capabilities allowing for Machine Creation Services Integration. Citrix Autoscale proactively manages the workloads to balance the costs and service levels demanded by the customer. Any unused workloads would be reduced during off-peak hours and increased prior to peak hours.

Service Providers hosting their customers in Resource Groups in Azure. Using a collection of assets (for example Virtual Network, Virtual Machines, Storage accounts) in logical allocations for easy automatic provisioning, monitoring, and access control. Dividing the dedicated or shared resource in separate Azure virtual networks, typically the access is controlled by the Cloud Connectors linking the Azure resource to Citrix Cloud.
For more recommendations regarding Citrix Virtual Apps and Desktops service on Azure see:

[Microsoft Azure](https://www.citrix.com/blogs/2018/06/07/cloud-guidepost-citrix-virtual-apps-and-desktops-service-on-azure-part-2/)

### Amazon Web Services

Amazon Web Services is another public hosting option for Citrix Service Providers looking to host workloads in a flexible and controllable environment. Using an operations cost model to grow their business according to customer demands. Citrix Virtual Apps and Desktops has built in AWS capabilities. Allowing for Machine Creation Services Integration for on-demand provisioning. In conjunction with Citrix Autoscale to proactively manage the workloads to balance the cost and services levels demanded by the customer. Any unused workloads would be reduced during off-peak hours and increased prior to peak hours.

In Amazon Elastic Compute Cloud, an Availability Group is a collection of assets. For example(Virtual Network, Virtual Machines, Storage accounts) in logical groups for easy or even automatic provisioning, monitoring, and access control. Resource Groups in EC2 are for grouping related resources that belong to Citrix Virtual Apps and Desktops deployment, as they share a unified resource.

The Virtual Machines used for Citrix Virtual Apps and Desktops workloads in EC2 are typically the T type machines. These Virtual Machines have the best balance for CPU and memory for Citrix Service Providers. Scaling up and down busing Autoscale to accommodate customer requirements and control the cost. Any unused workloads would be reduced during off-peak hours and increased prior to peak hours.

For more details regarding Citrix Virtual Apps and Desktops on AWS see:

[Amazon Web Services](https://aws.amazon.com/about-aws/whats-new/2019/01/deploy-citrix-virtual-apps-and-desktops-service-on-aws-with-new-quick-start/)

### Google Cloud

The Google Public Cloud offering for Citrix Virtual Apps and Desktops service allows Service Providers. Provisioning and managing machines within a Project on Google Cloud Platform (GCP), using Machine Creation Services (MCS) to provision workloads and enable lifecycle image management.

The automated provisioning for GCP, working in conjunction with Citrix Autoscale to scale up and down these workloads on demand. At least one Project is needed to run the Citrix Virtual Apps and Desktops Service in conjunction with the Compute Engine API and the “Cloud Resource Manager API. Controlled via a GCP Service Account and can be shared between multiple CGP Projects, and the MCS Service uses it to power manage the virtual machines.

For details on setting up a Citrix Virtual Apps and Desktops service resource location on GCP, see:

[Google Cloud](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html)

## Deployment Steps

### Onboard a Customer

#### Customer Dashboard

To add a new customer or invite an existing customer to be managed by the Citrix Service Provider. The onboarding process is the same for both multitenant and single tenant customers.

To simplify account sprawl and centralize customer management. CSP Team recommends that the add customer option is used within the Service Provider using a service account. This reduces the number of administrator accounts in use when setting up separate customer cloud accounts. Allowing for continual service management when administrators leave the CSP organisation.

#### Add a new Customer

In the Citrix Cloud Dashboard page, select Customers

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_009.png)

On the Customer Dashboard, displayed is a list of the Citrix Service Provider’s managed tenants, to Add a new Customer select Invite or Add:

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_010.png)

Select Add and Continue

[![CSP-Image-011](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_011.png)

Complete the onboarding information for the Customer, make sure the email address used here is unique and has not been used for any other Citrix Cloud accounts:

[![CSP-Image-012](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_012.png)

This creates a new Customer with a unique Organization ID (Org ID).

#### Invite a Customer

To invite an existing Citrix Cloud Customer, managed by the Citrix Service Provider, you can select the Invite option.

[![CSP-Image-013](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_013.png)

Select Invite and Continue.

[![CSP-Image-014](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_014.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_014.png)

Copy the Invite Link and email to the Administrator of the Customer you would like to invite:

### Enable Virtual Apps and Desktops Service to a New Customer

After a new customer on boarded or an existing customer accepted the invite, the Citrix Service Provider can enable services to that customer (tenant).

#### Enable Single Tenant (private) Citrix Virtual Apps and Desktops Service

For a new customer in a private workspace to have single tenant service. For example, the customer has their own instance of Virtual Apps and Desktops service. The CSP needs to make a $0 order via its distributor and “ship to” the customer’s Citrix Cloud account.

Once the single tenant service instance is enabled for the customer (stocking order fulfilled), “Manage” option appears inside the Virtual Apps and Desktops service tile. By selecting “Manage” option, the customer’s instance of Studio loads.

#### Enable Multitenant Citrix Virtual Apps and Desktops Service

Assuming the CSP partner already has the multitenant Virtual Apps and Desktops service entitlement fulfilled (Otherwise it is enabled via a $0 stocking order from the distributor).

Adding a new customer to be managed under the CSP’s multitenant service, follow the steps:

 1 - In the Citrix Cloud Dashboard page, select Customers
 2 - On the Customer Dashboard locate the Customer you want to add services to and select the three-dot button and select Add Services

[![CSP-Image-016](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_016.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_016.png)

 3 - Select “Continue” next to the Citrix Virtual Apps and Desktops Service

[![CSP-Image-017](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_017.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_017.png)

Once the “add service” process is completed (I can take a few minutes). “Manage” option appears inside the Virtual Apps and Desktops service tile within the tenant’s cloud account. However when selecting “Manage” option, “This instance of the Citrix Virtual Apps and Desktops Service is managed by your Citrix Service Provider” message is displayed.

[![CSP-Image-018](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_018.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_018.png)

### Configure Multitenant Virtual Apps and Desktops Service for the New Customer

This document focuses on the deployment configurations of multitenant architecture models, for single tenant Virtual Apps and Desktops Service refer to

[Virtual Apps and Desktops Service](/en-us/citrix-virtual-apps-desktops-service/install-configure.html)

The following section of multitenant deployment uses a hybrid cloud solution as an example to run workloads in an on-premises data center.

#### Deploy a New Resource Location

The resource location and domain are a 1:1 relationship.

##### Dedicated Resource Location

Each time when onboarding a new tenant, a new active directory, resource location, and a pair of cloud connectors need to be configured for the tenant.

###### Shared Resource Location

The resource location, active directory, and cloud connectors only need to be set up when the first tenant of the resource location is onboarded. The subsequent tenants share setup except the actual resources to be consumed, for example AD OU and VDAs, and so forth. The Service Provider is responsible to partition the active directory and resources for each tenant with secure isolation.

##### Process

When connected to the Citrix Cloud Console, select Resource Location (Edit or Add New)

[![CSP-Image-019](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_019.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_019.png)
Select Add Resource Location, name the Resource location to the multitenant nomenclature choose add Cloud Connector. Download and install the cloud connector to at least two dedicated Servers, for detailed steps follow

[How to install Citrix Cloud Connector](https://support.citrix.com/article/CTX223580)

You can view the Active Directory Domain and Cloud Connectors after deployment.

[![CSP-Image-020](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_020.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_020.png)

#### Define Hosting Connection

Since it is possible that each resource location can be deployed in different cloud infrastructure. For example Azure, GCP, AWS, on-premises hypervisor a new Hosting Connection to the resources needs to be defined for the new resource location.
Navigate to the hamburger menu at the top left side of the page and choose Citrix Virtual Apps and Desktops.

[![CSP-Image-021](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_021.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_021.png)

Select Manage Service, the Citrix Studio loads, select hosting from the Left-Hand Studio menu.

[![CSP-Image-022](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_022.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_022.png)

Select Add Connection or Resource from the Action pane.
Select Create a new Connection, choose the Connection type, and enter the credentials and address for the connection, name the connection using the correct nomenclature.

[![CSP-Image-022](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_023.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_023.png)

Select the storage location for the Resources.

Select the Network Associated with the new customer.

[![CSP-Image-024](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_024.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_024.png)

Select the scope of the Customer recently onboarded, the review the hosting connection and choose finish.

[![CSP-Image-025](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_025.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_025.png)

#### Configure Machine Catalogs for the New Customer

In the CSP partner’s Citrix Cloud portal page, navigate to Citrix Virtual Apps and Desktops service, and select Manage Service.

From the Citrix Studio, select Machine Catalogs, and Create Machine Catalogs from the Action Pane.

In this example we are using machines that are created with Machine Creation Services. Hosted on a hypervisor in the data center that is able to control the power state. Select the appropriate Resource Location Shared or Single and so forth, for the corresponding Customer assign the Machine Catalog, select Next

[![CSP-Image-026](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_026.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_026.png)

Add the Machines from the Corresponding Active Directory and also the Zone for the Customer.
Enter the name of the machines(s) and select OK.
Confirm the Zone and the minimal functional level of the VDA installed on the machines to be added. Shown is a VDA that is from version 1811 or newer, select Next.

[![CSP-Image-027](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_027.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_027.png)

Choose the Scope of the new customer, select Next.

[![CSP-Image-028](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_028.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_028.png)

Since machine catalogs are created for specific customer scope, predefined naming convention is necessary in a multitenant deployment.
The Machines appear in the Machine Catalog list.

[![CSP-Image-029](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_029.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_029.png)

Use the View Machines Search option, to confirm the registration status of the new Machine Catalog.

[![CSP-Image-030](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_030.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_030.png)

[![CSP-Image-031](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_031.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_031.png)

#### Create Delivery Groups for the New Customer

From the Citrix Studio, select Delivery Group, and Create Delivery Group from the Action Pane.
Read the Getting Started information and select Next.
Select a relevant Machine Catalog that is assigned with the customer’s scope, select Next.

[![CSP-Image-032](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_032.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_032.png)

The recommendation is to leave the management of Users to Citrix Cloud, select Next.

Select Add Applications from a source, usually it is the Start menu if an application appears there on the corresponding VDA. All applications selected appear under the same delivery group and be available as Libraries, to all subscribers that are later added via the Citrix Cloud portal. Separate delivery groups can be created for applications and user groups that need restricted access.

Under the multitenant deployment, some delivery groups can contain applications with the same name for different tenants. To avoid confusion and clearly define the ownership of these applications. The recommendation is to update the application naming to be tenant specific as shown in the example below. Application name for user can remain unchanged.

[![CSP-Image-033](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_033.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_033.png)

[![CSP-Image-034](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_034.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_034.png)

Assign the scope of the customer to the delivery group, and select Next

[![CSP-Image-035](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_035.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_035.png)

To securely isolate customers in a multitenant setup, a delivery group is only be assigned to a specific customer scope. Different customer scopes do not share delivery groups. Predefined naming convention for delivery groups is also necessary in a multitenant deployment.

### Configure Federated Domain for the New Customer

Even though the nomenclature is very similar, the CSP Domain Federation is not the same as the Federated Authentication Service (FAS).

For large customers under the single tenant (private workspace) architecture model, this step is not required. Domains and resource locations are configured directly within the customer’s cloud account.

For a new customer to be managed under the partner’s multitenant Virtual Apps and Desktops service deployment and still maintain its own workspace experience. For example to enable the Customers Gateway URL, the customer needs to be federated to the domain configured under the partner account.
Within the partner’s Citrix Cloud account, select the Customers domain from the Domains tab in the Identity and access management page, select Manage Federated Domain.

[![CSP-Image-036](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_036.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_036.png)

Select the customer(s) to be added to the Domain, allowing the tenant to use their customized Workspace Configurations.

[![CSP-Image-037](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_037.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_037.png)

Note: The Federated Domain described here for multitenant Virtual Apps and Desktops service is for workspace configuration only, it is not integrated with ADFS or Citrix Federated Authentication Service.

### Subscribe Customer User Groups to Offerings

Under the Single Tenant architecture model where each customer has own instance of the service. Managing subscribers to libraries is performed directly within the customer’s cloud account, for details refer to the online document

[Assign users and groups to service offerings using Library](/en-us/citrix-cloud/citrix-cloud-management/assign-users-to-offerings-using-library.html)

Under multitenant architecture models, subscribing user groups to libraries is performed inside the CSP partner’s Citrix Cloud account. The preferred method is to assign well named Active Directory groups to the library resources for easy administration and scalability.

To add users to a published application or desktop offering from either a Shared or Dedicated resource location of the multitenant service. Locate the Library Offerings from the Citrix Cloud home page, in the Library offering. Select the View Library option, search, or find the resource you would like to add users too using the three-dot menu. Manage Subscribers, choose from the list of Managed domains and then add the Resource Group.

[![CSP-Image-038](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_038.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_038.png)

### Configure Tenant Workspace

The CSP multitenant Virtual Apps and Desktops service allows each tenant to maintain its own Workspace Experience.
To change the Workspace for a customer from the Citrix Cloud Dashboard page, select Customers, view Details.
Select a customer and Expand using the Arrow, select View Customer Details.

[![CSP-Image-039](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_039.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_039.png)

Select Access Customer Account (there is also an alternative way to access the customer’s account via Change Customer)

[![CSP-Image-040](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_040.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_040.png)

Confirm that you are leaving the Citrix Service Provider’s Account to enter the Customer Account and select Continue.

After entering the tenant’s Citrix Cloud account, navigate to the hamburger menu and choose, Workspace configuration:

[![CSP-Image-041](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_041.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_041.png)

#### Access URL

Under the Access tab, the Customers Gateway URL can be customized. Edit the URL and select Save.

[![CSP-Image-042](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_042.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_042.png)

#### Authentication

In the Authentication tab, specify the Authentication method for the customer:

[![CSP-Image-043](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_043.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_043.png)

If Active Directory Authentication is used and the tenant is configured within a shared resource location. For example the tenant user accounts and groups reside within an OU of the hosted multitenant Active Directory. The users’ UPN suffix, which is normally the customer’s own domain, differs from the AD system domain. For example customer domain selwfashion.nz in the example below versus cms.azr system domain of the hosting AD. The user’s UPN domain to be recognized and authenticate through the custom Workspace URL. The UPN suffix needs to be added to the hosting Active Directory at the root level.

[![CSP-Image-044](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_044.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_044.png)

#### Appearance

Customized branding and appearance often help the end user experience. From Customize tab, configure the customer logo and preferences.

[![CSP-Image-045](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_045.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_045.png)

### User Log in to Workspace

When the users of a customer login to the Workspace via the customized URL. For example <https://selwfashion.cloud.com,> same set of credentials of UPN and password (for example email address and password that match their Office 365 accounts) are used.

[![CSP-Image-046](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_046.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_046.png)

After logging on the user’s workspace would look similar as following.

[![CSP-Image-047](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_047.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_047.png)

## Performance and Monitoring

The Citrix Virtual Apps and Desktops Service allows Citrix Service Providers to control and monitor the workloads centrally in the Cloud Console. Lowering the cost and administration effort of the management and allows the operations team to deliver greater uptime.

### Director

The Citrix Service Provider admins are able to manage their multitenant Shared and Dedicated Resource location Customers using a single Monitoring console. The CSP admin can choose to view an overview of all resources or drill down to a specific Customer. The Service Provider can also set Role Based Access Control permissions for its team remembers to manage specific customer scope or perform a subset of functions.

[![CSP-Image-048](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_048.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_048.png)

[![CSP-Image-049](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_049.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_049.png)

Single tenants in their private workspace with own instance of Citrix Virtual Apps and Desktops Service, their Monitoring console is dedicated. A Citrix Service Provider with administrator rights, logs into the customer’s cloud account to access and manage through this console.

### Citrix Analytics Service

The Analytics service included in the Citrix Service Providers Workspace collects data across the hosting network, users, files, and endpoints. A Service Provider can centrally manage the insights to handle security threats, monitor service performance and optimize, and improve their offering.

[![CSP-Image-050](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_050.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_050.png)

## License Usage

Citrix Service Providers can gain insights on the number of User Licenses that are assigned against their total commitment amount within the “Licensing” page of Citrix Cloud.

When the Citrix Service Provider specific entitlement is provisioned, the licensing rules on the page are aligned with the program rules. Service Providers can expect “Assigned” license counts to reset monthly, and overage amounts to be highlighted separately from the committed amount. Refer to the associated number in the image for the appropriate detail around the experience.

 1.  Provides the “Assigned” license count across all tenants and the total commitment amount. This “Assigned” resets every month.
 2.  A graphical representation of the monthly assigned licenses across all tenants against the commitment amount.
 3.  The ability to export the current month’s detailed list of users that are listed in item 4. Until the Longer-Term solution for Multitenancy is delivered in the “License Usage Insights” Service, the best way for partners to break out the users across the different tenants.
 4.  The detailed list of users that have an assigned license in the current month. This list makes up the total “Assigned” count. Additional insights are provided when the first time a license was assigned.

[![CSP-Image-051](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_051.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads_051.png)

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [Source Diagrams](https://citrix.sharefile.com/d-sf5413a717944a3e9)

## References

[Resource Location](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)

[Identity and Access Management](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html)

[Library Offerings and User Assignment](/en-us/citrix-cloud/citrix-cloud-management/assign-users-to-offerings-using-library.html)

[Cloud Connector Internet Connectivity Requirements](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html)

[Cloud Connector Secure Deployment](/en-us/citrix-cloud/overview/secure-deployment-guide-for-the-citrix-cloud-platform.html)

[Citrix FAS](/en-us/xenapp-and-xendesktop/7-15-ltsr/secure/federated-authentication-service/fas-architectures.html)

[Citrix Gateway Service](/en-us/citrix-virtual-apps-desktops-service/netscaler.html)

[Virtual Delivery Agent](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-vdas.html)

[Hosting Connections](/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)
