---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Dan Feller
description: Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.
tz_title: Virtual Apps and Desktops Service
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: Virtual Apps and Desktops Service

## Overview

When Covid-19 occurred, it forced all of Worldwide Co. employees to work remotely. Worldwide Co. office users typically worked from a corporate owned PC associated with their cube/office. Worldwide Co. quickly deployed Citrix Virtual Apps and Desktops Service to allow users to securely access their work PC from home using Remote PC Access.

During the 2020 pandemic, Worldwide Co. realized that employees in certain roles were equally or more productive working from home than at the office. Therefore, they wanted to ensure their environment allowed for these new permanent remote workers.

Although many employees became permanent remote workers, a group of employees have roles requiring onsite, office work. However, Worldwide Co. wants to provide the office-based employees with the flexibility of working remotely as needed.

This reference architecture shows how Worldwide Co. planned their Citrix Virtual Apps and Desktops environment.

## Success Criteria

Worldwide Co. defined a list of success criteria that formed the basis for the overarching design.

| Success Criteria                              | Description                                                                                                                                                                              | Solution                                               |
|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| Flexible work style                            | Although many users have primary work environment, the solution supports work style flexibility allowing users to work from anywhere, as needed.                                    | Citrix Virtual Apps and Desktops Service               |
| Minimize hardware costs                       | A large percentage of users work in the office on traditional PCs. The solution allows users to work remotely while still having the same experience.                              | Remote PC Access                                       |
| Secure resources                              | Corporate resources must be secured for users who are accessing with untrusted endpoints or from unsecured locations.                                                                    | VPN-less access                                        |
| Minimize data center footprint                | Minimize data center footprint to have the flexibility and agility to scale as needed and decrease the amount of physical hardware and appliances that need to be managed.               | Citrix Virtual Apps and Desktops Service and Cloud VDI |
| Adaptive session                              | Due to the varying nature of the end-user's connection to the resource, the experience dynamically change as the end user's environment is changing.                              | HDX Adaptive Technologies                              |
| User experience reporting                     | As IT is unable to fully control the links between remote users and the virtual desktops, they need to be able to monitor the overall experience and identify the areas for improvement. | Performance Analytics                                  |
| Optimal Routing                               |  To decrease latency and improve the experience, the solution must use the best route possible.                                                                                  | Citrix Gateway Service                                 |
| Optimize cloud costs                          | Minimize cloud costs by automatically scaling workloads based on schedule and usage.                                                                                                     | Autoscale                                             |
| Multifactor authentication                   | With security being top of mind, MFA is required to ensure another layer of authentication and protection of corporate resources.                                                | Time-Based One-Time Password micro-service             |
| Optimal performance and app response time     | In a multi-user environment, avoid a situation where a single user can monopolize CPU resources, which negatively impacts other users.                                                   | Workspace Management- CPU optimization                 |
| Optimize the images provided to the end users | Easy tool to help administrators optimize their images                                                                                                                                   | Citrix Optimizer                                       |
| Business Continuity                           | Options for resiliency in case there is an outage with the Cloud services                                                                                                                | Citrix Service Continuity                              |

## Conceptual Architecture

Based on their requirements above, Worldwide Co. created the following architecture. This architecture will not only meet all of the above requirements, but it will give Worldwide Co. the foundation they need to expand to other use cases as they are identified in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image1.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image1.png)

The Citrix Virtual Apps and Desktops architecture is divided up into layers. This framework provides a foundation to understand the technical architecture for the most common virtual desktop/application deployment scenarios. All layers flow together to create a complete, end-to-end solution for an organization.

At a high-level:

-  **User Layer:** This layer describes the end-user environment and end-point devices that are used to connect to resources.
    -  External Users: Access Citrix Workspace to gain access to Azure Virtual Desktop hosted in Azure.
    -  Internal Users: When in the office, continue to utilize their physical PC. When working remotely, they access Citrix Workspace and Remote PC Access to connect to their office-based physical PC.
-  **Access Layer:** This layer describes details surrounding external and internal access to the Citrix environment.
    -  Citrix Workspace: A complete digital workspace solution that allows you to deliver secure access to the information, apps, and other content that are relevant to a person’s role in your organization.
    -  Gateway Service: This cloud-based service provides secure remote access with Identity and Access Management (IdAM) capabilities, delivering a unified experience to SaaS (Software as a Service) apps and virtual apps and desktops.
-  **Resource Layer:** This layer defines the virtual desktops, applications, and data provided to each user group.
    -  Remote PC Access: A traditional, local Windows desktop, assigned to a single user and can be physically accessed locally or accessed remotely.
    -  Azure Virtual Desktop: virtualized Windows 10 multi-session operating system for users to be able to access their desktops and applications remotely.
-  **Control Layer:** This layer describes details surrounding the components used to support the rest of the environment.
    -  Virtual Apps and Desktops Service: This cloud-based service manages the authorization and brokering to Azure Virtual Desktops and Remote PC Access.
    -  Workspace Environment Management Service: This cloud-based service uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times.
    -  Performance Analytics: This cloud-based service tracks, aggregates, and visualizes key performance indicators of the Citrix Virtual Apps and Desktops environment.
-  **Host Layer:** This layer describes the hardware components, private, public, and hybrid cloud that are used for the Citrix environment – hardware, storage, and virtualization details.
    -  Physical PC: They use the physical PCs they already own but allow users to access these remotely when needed
    -  Azure: to reduce their data center footprint, they deploy new virtual desktop resources on Azure.

In the sections below we go through each of the above architectural components and how they meet Company XYZ’s requirements.

## Detailed Architecture

### User Layer

Aligning the user requirements with an appropriate virtual desktop is the initial step in creating a complete, end-to-end solution. Worldwide Co. defined the user requirements below.

| Users need access to...                                             | Users include...               | Endpoints include...                                        | Common locations include...                                                | IT delivers...          |
|---------------------------------------------------------------------|--------------------------------|-------------------------------------------------------------|----------------------------------------------------------------------------|-------------------------|
| Standardized desktop environment with line of business applications | Engineers Designers Executives | At the office: Physical corporate PCs  Remote: Personal devices | Predominantly internal local network. Sometimes remote, untrusted network. | Remote PC Access        |
| Standardized desktop environment with line of business applications | Sales Marketing                | Personal devices Tablets Laptops                            | Remote untrusted network                                                   | Azure Virtual Desktop |

Office workers typically work from the office with their corporate owned PC. When the pandemic happened, they needed a way to securely work from home while still utilizing their PCs that were in the office. Worldwide Co. realized that office workers can be productive as remote workers and want to provide the flexibility of working remote. They continue to use their PCs locally when they work at the office and access them remotely through Citrix Virtual Apps and Desktops Remote PC Access when they work from home.

Employees are predominantly remote employees. Worldwide Co. doesn’t want to provide corporate owned devices, instead they want to provide these employees the choice to use whatever device they want. This can include devices such as personal laptops, smartphones, or tablets. Because Worldwide Co. wants to minimize their data center footprint, they have chosen to deploy Azure Virtual Desktop with Citrix Virtual Apps and Desktop service for this set of employees.

### Access Layer

Providing access to the environment includes more than simply making a connection to a resource. Providing the proper level of access is based on where the user is located in addition to the security policies defined by the organization. Worldwide Co. chose to do the following:

-  **Minimize Data Center Footprint:**
    -  Gateway Service: Worldwide Co. decided to deploy Gateway Service to align with their goal of reducing their data center footprint. Gateway service allows them to provide secure remote access for their external users, without having to deploy and maintain any physical hardware, public IP address, or firewall rules. They also don’t need to worry about architecting for redundancy since Citrix takes care of that for them—Gateway Service operates in multiple regions worldwide with integrated redundancy. Gateway service minimizes the infrastructure required which provides administrators the agility to scale rapidly when needed (M&A, DR, new users, or contractors). More information on Gateway Service can be found [here](/en-us/tech-zone/learn/tech-briefs/gateway-hdxproxy.html).
    -  Rendezvous Protocol: Worldwide Co. also turned on the [Rendezvous protocol](/en-us/citrix-virtual-apps-desktops-service/hdx/rendezvous-protocol.html) which allows the HDX session to bypass the Citrix Cloud Connector and connect directly to the Citrix Gateway Service. Rendezvous protocol reduces the load on the Cloud Connectors, which helps reduce the data center footprint.
-  **Multifactor Authentication:** Worldwide Co. decided to implement multifactor authentication to protect their intellectual property. They have chosen to do this through the [Time-based One-Time Password microservice](/en-us/tech-zone/learn/tech-insights/authentication-totp.html) within Citrix Workspace. They chose TOTP because it allows them to meet their security needs without having to deploy or maintain other third party systems. Additional information on TOTP and Workspace Identity can be found [here](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html).
-  **Optimal Routing- Gateway Service:** Because Gateway service is globally distributed, it allows the user to connect via the fastest access point which creates the best user experience.
-  **Secure Resources:** All users authenticate with Workspace and Gateway service provides vpn-less access to their physical PCs and cloud-hosted VDI desktops. While this Reference Architecture only shows the users accessing virtual apps and desktops, Workspace gives organizations the flexibility to provide end users with SaaS, web, mobile, files, and microapps all from one unified location. Also, it provides SSO so users don’t have to constantly reauthenticate over and over again.
-  **Business Continuity:** Worldwide Co. also has taken advantage of the latest Service Continuity functionality within Citrix Workspace. Service Continuity further expands the Citrix Virtual Apps and Desktop service resiliency in case there is an outage with any of the following:
    -  Citrix Workspace Portal
    -  Citrix Cloud platform
    -  Citrix Identity Provider Service
    -  Citrix Virtual App and Desktop control plane
    -  AWS and Azure platform

Worldwide Co. opted for this approach instead of Local Host Cache because Service Continuity doesn’t have any on-premises requirements. Essentially it utilizes long-lived connection tickets for workspace and connects users to their VDAs as long as there is a network connection between the endpoints and the VDA. More information on Service Continuity can be found [here](/en-us/tech-zone/learn/tech-briefs/citrix-cloud-resiliency.html).

### Resource Layer

Users need access to their resources, whether those resources are desktops or applications.  Resources are configured within resource locations that are managed by Worldwide Co. The configuration of the resources must align with the overall needs of the user groups. End users expect an experience that is similar or superior to a traditional PC environment. Resources can be located on-premises, in private cloud, public cloud or in a hybrid approach. This is seamless to the end-user. Cloud Connectors are located within each Resource Location to connect the resources with Citrix Cloud. Worldwide Co. chose to do the following:

-  **Minimize Hardware costs:**
    -  [Remote PC Access](/en-us/tech-zone/learn/tech-insights/remote-pc-access.html): Remote PC Access allows users to access their office-based, physical PCs.

    [![Remote PC Access](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image2.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image2.png)

    Users access through their own personal device and access through Workspace App. After authentication, users would have access to their physical windows desktops. Worldwide Co. followed the best practices found here (/en-us/tech-zone/design/design-decisions/remote-pc-access.html) for their Remote PC Access VDAs.
-  **Minimize Data Center Footprint:** Worldwide Co. has chosen Azure as their other resource location. This allows them to quickly spin up new resources as needed without having to add new infrastructure. It gives them the flexibility to scale quickly and easily.
[![Azure](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image3.png)](/en-us/tech-zone/design/media/reference-architectures_virtual-apps-and-desktops-service_image3.png)
Worldwide Co. used the following [Design Decision guide](/en-us/tech-zone/design/design-decisions/azure-instance-scalability.html) when considering which instance series to deploy. Ultimately, they have chosen a D13_v2 instance with standard HDD disks and a 2GB MCSIO cache with a Windows 10 multisession OS. Worldwide Co. has chosen to have these be domain joined to their on-premises Active Directory via Azure Active Directory Domain Services and users’ accounts in the organization’s on-premises Active Directory. More information can be found [here](/en-us/tech-zone/learn/tech-briefs/citrix-managed-desktops.html).
-  **Optimize the images proviced to the end users:** Worldwide Co. has chosen to use the Citrix Optimizer to optimize their VDAs. Information on the Citrix Optimizer can be found [here](https://support.citrix.com/article/CTX224676).
-  **Adaptive session:** Worldwide Co. used the [baseline policies](/en-us/citrix-virtual-apps-desktops/policies/policies-default-settings.html), however they turned on “Adaptive Transport”. Adaptive Transport allows the session to respond to changing network conditions. With remote workers, adaptive transport allows them to have an optimal user experience. They have also taken advantage of other [HDX technologies](/en-us/tech-zone/learn/tech-insights/hdx.html) to improve the overall experience.

### Control Layer

With Citrix Virtual Apps and Desktops Service, the delivery controllers, SQL Database, Studio, Director, and Licensing are the core components in the Control layer. These components are provisioned on Citrix Cloud by Citrix during the activation of the Virtual Apps and Desktop Service. Citrix handles the redundancy, the updates, and the installation of these components. This allows the environment to always have the latest features and security patches. More services within Citrix Cloud can be enabled to support the requirements of Worldwide Co. Worldwide Co. chose the following:

-  **User Experience Reporting:** Worldwide Co. chose to enable [Citrix Analytics for Performance](/en-us/tech-zone/learn/tech-insights/performance-analytics.html) which allows them to quantify the end user experience and proactively address any issues. This information can be seen across both of their resource locations. More information can be found [here](/en-us/tech-zone/learn/tech-briefs/analytics.html).
-  **Optimal performance and app respoonse time:** Worldwide Co. wanted to avoid a situation where a single user can monopolize CPU resources, which negatively impacts other users (noisy neighbor syndrome). Therefore, they used Workspace Environment Management service to enable CPU Management settings. More information on CPU management can be found [here](/en-us/workspace-environment-management/service/user-interface-description/system-optimization/cpu-management.html#cpu-management-settings).

Worldwide Co. chose to domain join their Azure Windows virtual desktops to organization’s on-premises Active Directory via Azure Active Directory Domain Services and keep their users’ accounts in organization’s on-premises Active Directory. The Active directory is synced with the Azure AD in the customer’s Azure subscription using Azure AD Connect. This setup allows the user’s identity to be authenticated from the synced Azure AD.

### Host Layer

Administrators have the flexibility to deploy on-premises, in a public cloud, or in a hybrid approach. Worldwide Co. has chosen to do the following:

-  **Optimize Cloud Cost:**
    -  Autoscale: Worldwide Co. chose to deploy Autoscale to optimize cloud costs. Autoscale allows you to intelligently utilize, allocate, and deallocate resources. More information about Autoscale can be found [here](/en-us/tech-zone/learn/tech-briefs/autoscale.html). Worldwide Co. will initially use the following schedule-based Autoscale parameters based on the typical workday:

| Day      | Peak Times | Off-Peak Times | Machines Active       |
|----------|------------|----------------|-----------------------|
| Weekdays | 7AM-5PM    | 5PM-7AM        | Peak: 50% Off peak: 5% |
| Weekends | None       | All day        | 5%                    |

To accommodate more users, Worldwide Co. also enabled load-based scaling with the following parameters:

| Day      | Capacity Buffer (Peak) | Capacity Buffer (Off-peak) |
|----------|------------------------|----------------------------|
| Weekdays | 20%                    | 5%                         |
| Weekends | 5%                     | 5%                         |

-  Azure sizing: Worldwide Co. chose to deploy D13_v2 instance with standard HDD disks and a 2GB MCSIO to provide the best user experience at the lowest cost. An in depth analysis on the scalability of Citrix Virtual Apps and Desktop services on Azure can be found [here](/en-us/tech-zone/design/design-decisions/azure-instance-scalability.html).
