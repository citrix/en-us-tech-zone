---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Mayank Singh, Dan Feller, & Matthew Brooks
description: Learn how to design an environment that uses Desktop-as-a-Service as a Business Continuity solution. This reference architecture incorporates Citrix Virtual Apps and Desktops service, SD-WAN, and Performance Analytics.
tz_title: Desktop-as-a-Service for Business Continuity
tz_products: citrix-analytics;citrix-networking;citrix-secure-internet-access;citrix-secure-private-access;citrix-virtual-apps-and-desktops-standard-for-azure;citrix-virtual-apps-and-desktops;citrix-workspace;other;security
---
# Reference Architecture: Desktop-as-a-Service for Business Continuity

## Overview

Company A has always had a subset of remote users relying on the Citrix Virtual Apps and Desktops environment. These users used Citrix to securely access their apps and desktops when they were outside the company network. However, as Covid-19 happened, most of their end-users began working remotely and using Citrix Virtual Apps and Desktops. CompanyA has limited capacity in their on-premises data center and realizes that a fully on-prem solution can't scale as quickly as they would like. Due to greater number of employees working remotely, their servers became overloaded, causing some of their employees to not be able to work. Therefore, they decided to come up with a solution that would give them the agility to scale.

CompanyA decided to migrate to Citrix Virtual Apps and Desktops service to provide users with the tools they need to work from anywhere. Citrix Virtual Apps and Desktops service allows them to keep their images on-premises and have Desktop-as-a-Service (DaaS) in the cloud. This allows them to be prepared when there is a failure with their on-prem VDAs or when there is increase demand causing their VDAs to be overloaded. They have decided to deploy DaaS as their business continuity solution. DaaS allows CompanyA to scale quickly without having to deploy more servers on-premises. It also reduces the administrative overload by having a solution that the vendor fully manages.

This reference architecture explains how CompanyA is planning their environment to ensure they have a business continuity strategy in place. They want a plan in place that is cost-effective and allows end-users to continue working even with unforeseen circumstances

## Success Criteria

Company A has defined a list of success criteria that formed the basis for the overarching design.

Note: CompanyA already has an established DR and business continuity plan in place for other non-Citrix components on which Citrix is dependent (Active Directory, file shares, application back-ends, and so forth)

### User Experience

| Success Criteria                             | Description                                                                                                              | Solution         |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------|------------------|
| Seamless Experience                          | To reduce user disruption, end users have a similar look and feel                                                        | Citrix Workspace |
| Access to profile and app data/ file servers | Applications and desktops in the DaaS environment must be able to effectively make back-end calls to data center systems | SD-WAN           |

### Administrative Criteria

| Success Criteria                  | Description                                                                                          | Solution                                                   |
|-----------------------------------|------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| Minimize cloud costs              | Ensure workload availability while controlling costs by prioritizing on-prem resources before cloud  | Autoscale                                                  |
| Minimize CAPEX costs              | Have a DR strategy that doesn't involve buying extra hardware infrastructure to support it      | Citrix Cloud DaaS                                          |
| Single management console         | Single management console to manage both their on-prem resources and their DaaS resources     | Citrix Virtual Apps and Desktops Service                   |
| Reduce on-premises infrastructure | Reduce on-premises infrastructure and management overhead                                            | Gateway service & Citrix Virtual Apps and Desktops service |
| Overcome cloud failures           | Reduce end-user impact when cloud services are unavailable                                           | Service continuity                                         |
| Resource prioritization           | Cloud resources used when on-prem capacity is reached or on-prem resources are unavailable       | Autoscale                                                  |
| Managed cloud hosts               | Fully integrated DaaS solution that the vendor fully manages                               | Citrix Managed Azure                                       |
| Insights into your environment    | Proactive insights into the environment showing both on-premises resource locations and cloud | Citrix Analytics for Performance                           |

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the following high-level conceptual architecture. This architecture meets all the prior requirements while giving CompanyA the foundation to expand to more use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_01.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_01.png)

The architecture framework is divided into multiple layers. The framework provides a foundation for understanding the technical architecture for the flexible work deployment scenarios. All layers flow together to create a complete, end-to-end solution.

At a high level:

**User Layer:** The user layer describes the end-user environment and endpoint devices that are used to connect to resources.

-  Regardless of device, users access resources from Workspace app, resulting in an experience that is identical across every form factor and device platform.
-  Regardless of where the back-end resource is hosted, the user experience must be the same.
-  Regardless of VDI resources, the experience is the same.

**Access Layer:** The access layer describes details surrounding how users authenticate to their Workspace and secondary resources.

-  Users authenticate with the same credentials regardless of whether they are accessing on-premises resources or DaaS resources.
-  The on-premises Active Directory syncs with the Azure Active Directory in the organization's Citrix cloud subscription using Azure AD connect.
-  Citrix Workspace provides the primary authentication broker for all subsequent resources. CompanyA requires multifactor authentication to improve authentication security.

**Resource Layer:** The resource layer authorizes specific SaaS, web, and virtual resources for defined users and groups and the security policies associated with the resource.

-  User has access to virtual apps and desktops that are pertinent to their role.
-  CompanyA is primarily using DaaS resources in case their other machines become overloaded or aren't available as a business continuity plan, which is achieved through Autoscale.
-  SD-WAN is used so that applications and desktops in the DaaS environment can effectively make back-end calls to data center systems.

**Control Layer:** The control layer defines how the underlying solution adjusts based on the underlying activities of the user.

-  Virtual Apps and Desktops Service: This cloud-based service manages the authorization and brokering to Azure Virtual Desktops.
-  Citrix Managed Azure Capacity: allows the administrator to deploy a DaaS solution to be able to scale when needed.
-  Performance Analytics: This cloud-based service tracks, aggregates, and visualizes key performance indicators of the Citrix Virtual Apps and Desktops environment.

**Hosting Layer:** The hosting layer details how components are deployed on hardware, whether that is on-premises, cloud, or hybrid cloud.

-  CompanyA has a hybrid strategy to ensure that resources are always available to end-users when they need them.

The subsequent sections provide greater detail into specific design decisions for CompanyA's business continuity strategy reference architecture.

## User Layer

### Citrix Workspace

[![Citrix Workspace](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_02.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_02.png)

Users need to be able to access their resources and have a consistent experience regardless of what device or what resources they are accessing. Through Citrix Workspace app and Citrix Workspace, users are able to access on-premises resources and cloud resources managed by Citrix within Citrix's Azure tenant. Although CompanyA can bring their own Azure subscription, they have decided to use Citrix's Azure tenant.

It is seamless for the user no matter where the resources are hosted. Workspace app can either be installed in the user's endpoints, or they can use the web-based workspace app that doesn't require them to install anything.

CompanyA uses their on-premises Active Directory and continues using their on-premises Active Directory for their business continuity resources. More information on how Citrix Workspace uses a secure primary identity to broker authorization to virtual apps and desktops can be found [here](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html).

## Access Layer

### Gateway Service

As a part of their migration to Citrix Cloud, CompanyA decided to migrate to [Gateway service](/en-us/tech-zone/learn/tech-briefs/gateway-hdxproxy.html) as an HDX proxy to provide users with secure remote access to their resources. This alleviates the infrastructure and management overhead of maintaining the gateway on-prem. To make the switch, CompanyA followed this [migration guide](/en-us/citrix-gateway-service/migrate-vpx-to-gateway-service-for-hdx-proxy.html).

### Authentication

CompanyA continues to use their existing methods of authentication that users are familiar with. The end users authenticate to Citrix workspace and get access to all their resources. Users get the same experience whether they are launching cloud or on-premises back-end resources. CompanyA decided to have both the machines and the users' accounts joined to the organization's on-premises Active Directory. The Citrix Virtual Apps and Desktops service Azure subscription (SD-WAN virtual appliance installed) and the customer's on-premises (SD-WAN branch appliance installed) locations are connected to each other using SD-WAN. The customer manages these appliances using the SD-WAN Orchestrator in Citrix Cloud.

[![SD-WAN](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_03.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_03.png)

## Resource Layer

### Citrix Virtual Apps and Desktops Service with Citrix Managed Azure

CompanyA migrated to Citrix Virtual Apps and Desktops service to deliver access to on-premises virtual apps and desktops. Due to the increased demand for virtual apps and desktop, CompanyA needed a solution that can easily scale when needed. Citrix Managed Azure is a simple, fast way to deliver Windows apps and desktops from Microsoft Azure. This new functionality is now available for Citrix Virtual Apps and Desktops service customers.

Citrix Virtual Apps and Desktops service allows customers the flexibility to deploy images in their own customer-managed Azure. However, CompanyA has decided to deploy in the Citrix Managed Azure because they wanted a business continuity solution that was fully managed by the vendor. While Citrix provides base images that CompanyA can use, they only include the operating system, and the Virtual Delivery Agent installed on them. Therefore, they decided to import their own images, which include the organization's apps and configurations. CompanyA currently has deployed Windows ten multi-session machines to reduce the number of machines needed with the "medium" workload, which supports ten concurrent sessions.

More in-depth information on Citrix Virtual Apps and Desktops Managed Azure can be found [here](/en-us/tech-zone/learn/tech-briefs/citrix-managed-desktops.html).

### SD-WAN

[![SD-WAN](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_04.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_04.png)

When end users access DaaS resources, those resources still need to be able to communicate with app data and file servers in CompanyA's data center. Although CompanyA can connect back to their datacenter using Express Routes, they have chosen to deploy SD-WAN instead to enhance the traffic performance while in transit. They also want to the flexibility to be able to deploy to multiple cloud sites in the future.

High availability and great user experience are important for CompanyA, and SD-WAN can help in both areas. Citrix SD-WAN does per-packet QOS and analyzes traffic in real-time on a per-packet basis, including latency, loss, and jitter. By bonding two relatively inexpensive internet connections (whether two landlines or a landline plus 4G/LTE), Citrix SD-WAN can deliver not only the reliability of much costlier MPLS circuits but also higher bandwidth, lowest possible latency, and various [Quality-of-Service features](/en-us/citrix-sd-wan/current-release/quality-of-service.html).

More information on SD-WAN can be found [here](/en-us/tech-zone/learn/tech-briefs/sdwan-workspace.html).

## Control Layer

### Citrix Virtual Apps and Desktops Service

CompanyA has chosen to use Citrix Virtual Apps and Desktops Service because it gives them the flexibility they need to deploy resources from multiple resource locations from a unified management console. It also reduces the administrative overhead to deploy and manage their virtual apps and desktop environment. It allows them to minimize hardware costs and deploy DaaS resources for a fully managed solution.

More in-depth information on Citrix Virtual Apps and Desktops service can be found [here](/en-us/tech-zone/learn/tech-briefs/cvads.html).

### Service Continuity

CompanyA has designed its environment to overcome on-prem issues when machines become overloaded or unavailable. Therefore, it is important that a contingency plan is in place in case the cloud services fail.

It is important for CompanyA that their users don't experience lost productivity due to outages or cloud issues. Therefore, they have turned on service continuity within Citrix cloud. Service Continuity allows users to connect to resources that are reachable during outages or when Citrix Cloud components are unreachable. This functionality has given CompanyA peace of mind to ensure that even in the rare occasion of a cloud outage, their user are still productive.
Service continuity improves the visual representation of published resources during outages by using Progressive Web Apps service worker technology to cache resources in the user interface. Service continuity indicates which resources are available during an outage.

Service continuity uses Workspace connection leases to allow users to access apps and desktops during outages. Workspace connection leases are long-lived authorization tokens.

More information on how Service Continuity works can be found [here](/en-us/tech-zone/learn/tech-briefs/citrix-cloud-resiliency.html).

### Citrix Analytics for Performance

[![Citrix Analytics for Performance](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_05.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_05.png)

CompanyA chose to use Citrix Analytics for Performance to be able to proactively see if their users were having a bad user experience. Through the machine-learning anomaly alerts, administrators are notified if there are a high number of users experiencing a poor experience and are given a potential root cause to troubleshoot. They use Citrix Analytics for reactive troubleshooting. Through its advanced search functionality, they can use Analytics to figure out what exactly is causing the issue.  Admins can also use it to identify black-hole or overloaded machines.

CompanyA can monitor both their on-premises and cloud sites and proactively resolve the end user's experience.

[![Citrix Analytics for Performance](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_06.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_06.png)

Administrators go into Analytics on a weekly basis. They click the users experiencing a poor experience and see what insights Analytics provides to troubleshoot.

[![Citrix Analytics for Performance](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_07.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_07.png)

They look at the failure insights to troubleshoot any black hole machines and to address any communication errors occurring in the environment.

[![Citrix Analytics for Performance](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_08.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_08.png)

Lastly, they check the machine availability and whether it has improved in the past week.

[![Citrix Analytics for Performance](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_09.png)](/en-us/tech-zone/design/media/reference-architectures_daas-for-business-continuity_09.png)

More information on Citrix Analytics can be found [here](/en-us/tech-zone/learn/tech-briefs/analytics.html).

### Autoscale

CompanyA chose to deploy Autoscale to optimize cloud costs. Autoscale allows you to intelligently use, allocate, and deallocate resources. To optimize costs, CompanyA uses the [Restrict Autoscale](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/restrict-autoscale.html) functionality. This functionality allows CompanyA to use the on-premises machines first and then once the on-premises machines are unavailable. The cloud machines are used. To do this CompanyA uses tag restriction with zone preference. Tag restrictions specify machines to be power managed by Autoscale. Zone preference specifies machines in the preferred zone to handle user launch requests.

CompanyA does the following:

-  Machine Catalogs: CompanyA creates two machine catalogs. One containing all machines that are on-premises and the other that contains all the machines that are in the cloud
-  Tag restriction: a tag is created and applied to all the machines in the cloud catalog
-  Zone configuration: two zones are created—one corresponding to the on-premises deployment and one corresponding to the cloud deployment
-  Delivery group configuration: delivery group contains machines from both the cloud and on-premises catalog
-  Autoscale configuration: Autoscale power manages only the machines with that are tagged as "cloud" and a capacity buffer of 10% is set
-  Desktop configuration: zone preference is configured to have the on-premises zone preferred over the cloud zone

More information about Autoscale can be found [here](/en-us/tech-zone/learn/tech-briefs/autoscale.html)
