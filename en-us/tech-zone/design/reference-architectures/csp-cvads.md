---
layout: doc
---
# Citrix Service Provider Cloud Reference Architecture

## Contributors

**Author:** [Darren Harding](https://Twitter.com/DarrenHarding)

**Special Thanks:** [Selma Wei](https://Twitter.com/_selw), [Bala Swaminathan](https://Twitter.com/Bala)

## Audience

This document is intended for IT deciscion makers, consultants, solution integrators, cloud engineers, and CSP Partners whom are seeling to deploy or migrate existing multi-tenant Citrix Virtual Apps & Desktops environment to the multi-tenanat Citrix Cloud.

## Executive Summary

The Citrix Service Provider Reference Architecture on Citrix Cloud uses the next generation of cloud service delivery approach, provides guidance on deployment architectures that scale easily while increasing user centric mobility for an expanding customer base.

Citrix Cloud enables the delivery of Microsoft® Windows® and Linux® workspaces with people-centric secure applications and desktops hosted in the Service Provider managed environments from on-premise datacentres to private or public clouds. 
Citrix Service Providers can take advantage of the flexible licensing programs to deliver cost-effective services based on subscriber usage. 

The reference architecture is easily adapted to meet specific provider and subscriber requirements, allowing Service Providers to deliver a comprehensive set of workspace offerings and price points while simplifying management and scalability. 
The cloud-ready services model enables lower infrastructure and administrative costs, speed to market and scalability, greater customer satisfaction, and increased business success.


## Introduction and Scope

This document provides architectural guidance for Citrix Service Providers who utilize Citrix Cloud technologies to offer services to customers and subscribers. The Reference Architecture is intended to assist Service Providers scale from a small subscriber base to an extensive user base shared across multiple tenants and multiple geographies.

The Citrix CSP Reference Architecture is designed to be flexible and can be used to implement hosting environments within virtually any infrastructure, during any phase of implementation.

This documentation describes the design and implementation of the Citrix Cloud solution infrastructure to be vendor agnostic and will use common wording for the specific technology in use. 

Multi-tenant resource locations managed by Citrix Service Providers should be highly scalable and available, with great performance and end user experience including the management and incorporation of additional services.

This version of the Reference Architecture focuses on Citrix Virtual Apps and Desktops service for Citrix Service Providers. At time of publication not all Workspace Services support multi-tenancy.  We will expand the scope of the reference architecture to cover the overall workspace services for CSPs in future versions.

## Overviwe
## Citrix Cloud
[Citrix Cloud](https://docs.citrix.com/en-us/citrix-cloud.html )

## Citrix Workspace

Is a unified secure cloud platform managed by Citrix where hosting providers can securely deliver applications and data while maintaining end user experience and productivity in an increasingly mobile workstyle.  

[Citrix Workspace](https://docs.citrix.com/en-us/citrix-workspace.html )

## Citrix Virtual Apps and Desktops service
[Citrix Workspace](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/setup-for-citrix-service-providers.html )

## Citrix Cloud Connectors
[Citrix Cloud Connectors](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/cloud-connectors-install.html )

## Architecture Models for Citrix Service Providers

Citrix Cloud for Citrix Service Providers (CSPs) is the platform for the delivery and management of Citrix technologies, helping Service providers extend existing hosting deployments or move their customers to a hosted cloud solution.  CSPs can create and deploy secure digital workspaces rapidly using Citrix Cloud, while maintaining the control of sensitive data and resources hosted on-prem or in a chosen cloud.

[![CVAD-Image-1](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_001.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_001.png)

## Citrix Virtual Apps and Desktops Service for CSP

The traditional deployment in a hosting environment for Citrix Virtual Apps and Desktops includes highly available delivery controllers, storefront servers, SQL databases, gateway, and management consoles deployed in the service provider’s datacentre along with the Virtual Delivery Agents (VDA).  In the Citrix Cloud model for CSP, the management or control plane, and optional access layer (cloud gateway), are managed by Citrix, leaving the Citrix Service Provider to focus on the customers’ application data and critical services. 

**** CVAD Image ****

## Security and Isolation 
The Citrix Virtual Apps and Desktops Service Architecture consists of layers that connect together to create a complete end-to-end solution for service providers. For general conceptual architecture, and to understand how all layers flow together, please refer to Citrix Tech Zone

[![CVAD-Image-1](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html)]

## External Access Security

A multi-tenant environment will be isolated from the internet using a blended approach with several complimentary technologies such as Firewalls, Application Delivery Controllers, Packet Filtering, intrusion detection and prevention systems etc.  Access to a multi-tenant network from Citrix Cloud is facilitated either by using the Citrix Cloud Connectors or a Citrix Application Delivery Controller and Citrix StoreFront combination.

## Management Separation

The core network services for a Service Provider are located in a separate partitioned that allows the hosting of shared services, depending on the services offered, the components of this partition may be: Active Directory Domain Controllers, Backup, Automation Services, DNS etc.

## Storage Security

Access to the file repositories of each tenant needs to be separated from other tenants, this can be achieved with using dedicated of shared servers that are protected using security partitions or permissions 

## Tenant Isolation

Partitioning of the tenants is defined by the level of separation demanded by the customers. Citrix recommends that each tenant is placed into a segregated network using a SDN for their dedicated workloads and complimentary services, ensuring that there are effective security isolation boundaries, with managed networks and IP management and routing

## Multi-tenant Architecture Models

Citrix Cloud Multi-tenant Virtual Apps and Desktops service enables Service Providers to manage multiple customers using the single instance of the service with same Citrix Studio and Director consoles and Role Based Access Control under the partner cloud account. Citrix license management is also centralised for easy allocation.
Multi-tenancy capabilities provide economies of scale on a single shared infrastructure while providing the required isolation and data protection. Service Providers can make trade-offs regarding price and features to meet individual tenant requirements.
The tenant isolation in multi-tenant deployments needs to include appropriate nomenclature to clearly define the objects that are shared or dedicated within the management consoles and control plane available to the Service Provider admins, for example {Tenant}-{Location}-{Group}.  
The Multi-tenant Virtual Apps and Desktops service supports two architecture models:

	1.	Shared Resource Location for multiple tenants
	2.	Dedicated resource location per tenant 
