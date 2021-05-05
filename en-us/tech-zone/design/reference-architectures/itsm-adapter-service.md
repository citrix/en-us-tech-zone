---
layout: doc
h3InToc: true
contributedBy: Nagaraj Manoli
specialThanksTo: Arvind SankaraSubramanian, Kiran Kumar
description: Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.
tz_title: ServiceNow with Citrix Virtual Apps and Desktops
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: ServiceNow with Citrix Virtual Apps and Desktops

## Audience

This document is intended for technical professionals, IT decision-makers, partners, and system-integrators. This document also allows the administrator to explore, and adopt a Citrix ITSM adapter service with ServiceNow. The ITSM service helps to automate, monitor, and manage Citrix Virtual Apps and Desktops environments seamlessly and easily. The reader should have a basic understanding of ITSM and ServiceNow workflows.

## Objective of this document

This document covers a technical overview, architectural concepts, and capabilities of the Citrix ITSM Adapter Service for ServiceNow. This document covers a few examples of how administrators and users can benefit from integrating ServiceNow with their Citrix environment.

## Introduction to ITSM

IT Service Management (ITSM) is a holistic approach to IT delivery in an organization. Both Managing and implementing IT services are part of ITSM to meet the needs of an organization. The most common perception of ITSM among employees is used solely for IT support – tickets are raised, triggered, and then resolved by IT. ITSM actually goes for beyond just resolution of day-to-day issues in an organization. This adoption, in turn, enhances employee productivity and is good for the business overall.

There are many reasons to adopt ITSM in an organization. Implementing ITSM can help to smooth the flow of processes through a structured delivery methodology. In Citrix Virtual Apps and Desktops environment resource consumption, resource provision and cost calculation that are financially focused can be optimized with proper predictive methodologies.

Adopting ITSM also helps in saving costs by building a predictable IT organization. The ITSM brings an actionable IT insight into the business that helps the IT decision-makers.

As a business grows, the IT demand also grows often requiring the same number of employees to do more. To successfully solve this challenged, management must adhere to mature and efficient processes to reap the greatest benefit from the IT resources they own. Here are some common benefits of bringing ITSM into a Citrix Virtual Apps and Desktops environment:

*  Reduces IT costs

*  Increases competitive advantage

*  Improves employee satisfaction

*  Improves the Quality of Service

*  Automates workflows saving time and money

To perform all IT service management activities required, teams use an ITSM tool. This tool is used to manage the delivery of IT services. This tool can be standalone software or a suite of applications. It can be hosted in the cloud or installed on-premises depending on the needs of an organization.

The ITSM tool performs multiple functions including, incident management, service request handling, problem management, and change management, to name a few.

[![ITSM-Image-1](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_001.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_001.png)

Between customers and the service provider, a service desk acts as a single point of contact. They are responsible for monitoring services and providing support. A few of the known tasks managed by a service desk are incident handling, service requests, and software licensing among other things.

### Overview of ServiceNow

ServiceNow is a company that provides service management Software as a Service (SaaS). The company specializes in IT operations management (ITOM), IT business management (ITBM), and IT services management (ITSM).

ITSM is catered through the SaaS model by ServiceNow. Later, ServiceNow ventured into the PaaS cloud model. The entire company business processes can be managed through a single system.

ServiceNow provides the infrastructure required to perform data collection, storage, and application development all on a single platform. ServiceNow does not provide an in-house IaaS deployment model. It provides IaaS integration capabilities with other leading IaaS models including Microsoft® Azure.

The benefits of ServiceNow ITSM include business-IT alignment, predictable IT performance, and cost savings. The organization can manage its IT processes efficiently; customers can spend less time on firefighting and focus on strategic initiatives.

There are several categories for IT processes that are defined by ITIL but appear in various forms in the other ITSM frameworks.

*  Change management

*  Asset management

*  Project management

*  Knowledge management

*  Incident management

*  Problem management

ServiceNow ITSM typically is associated with the service lifecycle outlined in ITIL v3. The goal of the framework is to ensure that the right ITSM processes, workforce, and technology places so that the organization can focus on business goals.

## Citrix ITSM Adapter Service

The ITSM Adapter Service in Citrix Cloud automates the provisioning and management of Citrix resources within an organization. Out-of-the-box workflows are provided by the ServiceNow administrator as catalog via a self-service portal for resource management.

The Citrix ITSM Adapter Service is available for Citrix Virtual Apps and Desktops customers and includes deployment that is on-premises or cloud. This service is hosted on Citrix Cloud and available through the Citrix Cloud platform.

Citrix ITSM Adapter Service is entitled to Citrix Virtual Apps and Desktops Premium and Citrix Virtual Apps and Desktops service customers.

## Citrix ITSM Connector

Citrix ITSM Connector is a plug-in installed in ServiceNow that enables communications between ServiceNow and Citrix Cloud. ServiceNow administrator has to download and install the Citrix ITSM connector from the ServiceNow store.

## Key benefits of integrating ServiceNow with Citrix Cloud

Citrix and ServiceNow solutions are integrated to deliver IT services for the organization to enhance end-user productivity and focus on business strategies.

The ITSM Adapter automates day-to-day IT administrator tasks around digital workspace delivery for both IT teams and their workforce. In turn, it enhances the overall efficiency throughout the organization.

Citrix ITSM Adapter is a value-add service to an organization, where IT teams can build workflows to automate, monitor, and manage Citrix Virtual Apps and Desktops environments easily and seamlessly.

Citrix ITSM Adapter is a cloud-enabled service to automate the provisioning of Citrix resources and employee onboarding and offboarding with ServiceNow. This integration helps to alleviate the IT team from spending time on manual and one-off processes.

The key benefits of Citrix ITSM Adapter are as follows:

*  Growing demand across new employees and business units in organization scalability of digital workspace become easy for an organization

*  Enhancing the user experience for the organization workforce and their IT team

*  Optimizes IT utilize their time on strategic initiatives, rather than manual processes

*  Automating the provisioning of Citrix Virtual Apps and Desktops during onboarding or offboarding

*  Automating common help desk requests

### Technical Concepts

This section covers the ITSM Adapter and ServiceNow high-level architecture. Intelligent workflow and integration of the solution are also included.

## High-level architecture

Citrix Virtual Apps and Desktops and ServiceNow ITSM are integrated through the ITSM Adapter. In other words, the Citrix ITSM Adapter acts as a bridge between the ITSM tool and the resource provisioning platform. The administrator has the power to create workflows that automate the end-to-end process thereby reducing the SLA and increasing productivity. IT administrators can automate, monitor, and manage their Citrix environments seamlessly and easily.

[![ITSM-Image-2](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_002.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_002.png)

The preceding diagram depicts the components and communication for the Citrix ITSM Adapter feature. Organizations with Citrix and ServiceNow solutions can easily leverage these benefits. The Citrix ITSM Adapter is a cloud service that both on-premises customers and Citrix Cloud customers can use.

The Citrix ITSM Adapter itself runs on Citrix Cloud regardless of resource locations. Customers use their Citrix Cloud account to access the platform (or sign-up for a new Citrix Cloud account if needed). The administrator works with the ITSM team to install the Citrix ITSM connector plug-in from the ServiceNow store.

The plug-in within ITSM console communicates with the workspace service eliminating wait times associated with provisioning and managing virtual assets. Assets are managed using out-of-the-box workflows that are available in ServiceNow.

Learn more about Citrix ITSM and ServiceNow integration at this [link](/en-us/advanced-concepts/downloads/itsm-adapter-service-user-guide.pdf).

## Citrix ITSM Adapter Service and ServiceNow integration

Many organizations face challenges when they have multiple silos to manage their employees’ desktops and apps. Imagine the impact of an organization having multiple sites and managing all of their workloads from a single control center or IT help desk. During onboarding or offboarding of contract workers and permanent workers, the strain on IT resources will likely result.

The idea of automating the workflows using the ITSM tool becomes an easy task for the IT help desk and reduces the time. In turn, it provides quick deployment of desktops and applications needed for production.

[![ITSM-Image-3](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_003.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_003.png)

The preceding design diagram depicts communication between the Citrix ITSM Adapter and ServiceNow plug-in. Also, it includes the interaction between the Workspace Automation Service and Desktop Delivery Controller.

The earlier diagram can be carved into three parts - ServiceNow plug-in, ITSM Adapter, and on-premises connector that communicates with Citrix Virtual Apps and Desktops resources.

ServiceNow ITSM Connector plug-in’s UI displays service catalog items and the workflows that are related to Citrix. The back end component makes actual calls to the Workspace automation service using REST APIs over HTTPS.

The Citrix ITSM Adapter is hosted on Citrix Cloud. It has two components that include a front end and back end service which provides communication with the Citrix Cloud Connector and delivery controller. The Cloud Connector that runs on customer premises provides a channel for the ITSM Adapter to execute the workflow. These connectors run on Windows Servers at the customer resource location.

### Citrix ITSM Adapter Service on-boarding

Configuring the ITSM Adapter for ServiceNow involves these three steps:

*  Subscribing to the Citrix Cloud service

*  Configuring the ITSM Connector for ServiceNow

*  Adding sites in the ITSM Adapter

The Citrix ITSM Adapter Service runs on Citrix Cloud. The administrator logs in to the Citrix Cloud portal and finds the available service on the landing page. The administrator has to make sure that cloud connectors are installed at the customer resource location. During the connector on-boarding, the login credentials are entered for authentication. Later the administrator has to select the customer ID.

For ServiceNow on-boarding, the ServiceNow administrator has to install the application from the ServiceNow App Store. During the onboarding, the administrator provides the Client Key and Client Secret from the Citrix Cloud console.

[![ITSM-Image-4](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_004.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_004.png)

The preceding picture shows the Citrix ITSM Adapter management console. The Citrix Virtual Apps and Desktops Service can be enabled or disabled for use with the feature on this page. The ITSM Adapter can connect to multiple ServiceNow instances.

Learn about configuring Citrix ITSM Adapter with ServiceNow at this [link](/en-us/citrix-cloud/itsm.html).

## Service Catalog

ServiceNow offers self-service opportunities to customers. ServiceNow administrator can create service catalogs that support the Citrix ITSM Adapter to manage Citrix Virtual Apps and Desktops environment. Citrix end-users are enabled with the self-service capability and faster request fulfillment.

ServiceNow administrator and catalog administrators can define and manage multiple service catalogs. The ServiceNow administrator can further extend the service catalog to provide more powerful features, configuration options, and scripting functions.

[![ITSM-Image-5](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_005.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_005.png)

The preceding picture shows multiple catalogs created in ServiceNow. Multiple service catalogs enable the organization to offer different sets of services. End-users can access multiple catalogs from a single home page. From the search option, all catalogs are accessed or directly within each catalog.

## Citrix ITSM Adapter Service for public cloud resource locations

Citrix ITSM Adapter Service and ServiceNow capabilities can be extended to the public cloud resource location. The Citrix Virtual Apps and Desktops Service with resource locations running on any leading public cloud platform can make practical and effective use of ServiceNow. Citrix Cloud Connectors are playing a vital role in executing commands initiated by ServiceNow admins. The Citrix ITSM Adapter is hosted on Citrix Cloud and all the connectors are hosted on the public cloud platform with communication over HTTPS.

[![ITSM-Image-6](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_006.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_006.png)

The preceding diagram shows a holistic view of the ServiceNow and Citrix ITSM service framework. Customers can host their resources either on-premises or in the cloud with all the communications initiated through the Citrix Cloud Connector from the customer side.

For higher availability, it is recommended to have a minimum of two Citrix Cloud Connectors at the resource location. Citrix Virtual Apps and Desktop Service support all the major public cloud platforms to provision workloads. It is easy for an administrator to adopt Citrix ITSM service in any green field or brownfield environment.

Reference: [CVAD Reference Architecture](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html)

## Citrix ITSM Adapter Service and ServiceNow Workflow

The ServiceNow and Citrix ITSM Adapter workflow with Citrix Virtual Apps and Desktops is explained in the following diagram.

[![ITSM-Image-7](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_007.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_007.png)

ServiceNow initiates the request command and all the tasks are handled in the delivery controller. In most of the deployment scenarios, the delivery controller installed on the customer premises and all the communication happens through the Citrix Cloud Connectors.

The customer has a Citrix Cloud hosted delivery controller and the commands are initiated from Citrix Cloud to a specific resource location. Resource locations could be on on-premises hypervisors or public cloud platforms.

|      | **Workflow for on-premises Delivery Controller**             | **Workflow for Citrix Cloud Delivery Controller**            |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1    | User requests virtual apps and desktops creating a service ticket on ServiceNow portal | User requests virtual apps and desktops creating a service ticket on ServiceNow portal |
| 2    | API call from ServiceNow to Citrix ITSM Adapter initiated    | API call from ServiceNow to Citrix ITSM Adapter initiated    |
| 3    | Citrix ITSM Adapter forwards requests to a specific site or resource location. | Citrix ITSM Adapter executes the required script             |
| 4    | From the Delivery Controller script is executed              | Citrix ITSM Adapter sends a request to close the service ticket |
| 5    | Delivery Controller sends results back to ITSM Adapter and script execution is acknowledged. | ServiceNow closes ticket                                     |
| 6    | From Citrix Cloud Connector acknowledgment is passed to the Citrix ITSM Adapter. |                                                              |
| 7    | The Citrix ITSM Adapter sends a request to close the service ticket |                                                              |
| 8    | ServiceNow closes ticket                                     |                                                              |

## General PowerShell Scripting from ServiceNow

The ServiceNow administrator can build and execute workflows as per the needs. PowerShell scripts commonly used in ServiceNow that help to automate various tasks that are executed manually by the IT help desk. Some of the examples are discussed as follows:

### Creating desktops based on user geography

In given organization employees are spread across multiple locations and single IT help desk has to struggle in terms of handling multiple tasks. Including employee onboarding and assigning the resources for respective users with location. ServiceNow and Citrix Cloud can help in automating this task and reduce the workload on the help desk.

[![ITSM-Image-8](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_008.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_008.png)

The preceding flowchart depicts virtual desktop creation on two different sites with intelligent workflow from ServiceNow. Assuming that the customer has two different sites and employees spread across different locations. When user requests virtual desktop or app, the request is analyzed based on the user’s location and resources are provisioned. Decision making in the intelligent workflow varies from customer’s environment (For example, the decision can be made based on OU level, domain level and so on) ServiceNow with Citrix ITSM Adapter service provides an intelligent workflow that handles the user request, resulting in resources are provisioned on that particular site.

### Self-service password reset from ServiceNow

Resetting login passwords of domain users is always a bottleneck for an IT help desk. In most of the organization’s IT help desk teams spending a lot of time resetting the user’s password. A self-service password reset workflow that helps the end-user to self-reset domain password with few simple clicks from ServiceNow console.

[![ITSM-Image-9](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_009.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_009.png)

The preceding diagram depicts the self-service password workflow from ServiceNow. When any Citrix end-user requests for a password reset or unlock the user account then a standard workflow written on PowerShell script helps to resolve the issues.

From the end-user's ServiceNow console, user enters the details with unique security code then ServiceNow creates a ticket. Further it is filtered with decisive steps to verify the account. Finally, user asked to enter the new password with pre-defined condition set on ServiceNow password policies. Once user entries are confirmed, later they are updated on Windows Active Directory service with the closure of the ticket.

The earlier steps are automated without the help of the IT service desk. This automated task reduces help desk costs and employee wait time to access resources including apps and desktops.

### Track resources running on a particular site

Asset management is important for an organization to aid in financial responsibility. Many departments, including IT, finance, services, and end-users are involved in IT asset management.

[![ITSM-Image-10](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_010.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_010.png)

From the ServiceNow portal, the administrator can run the PowerShell commands to fetch the details from an existing Citrix Virtual Apps and Desktops environment. A sample script as follows:

`Get-BrokerMachine | select MachineName, DesktopGroupName | Group-Object DesktopGroupName | Select Name, Count`

The preceding sample script shows the number of delivery groups and names. The administrator can build their own PowerShell script to fetch more details from the delivery controller including the number of machines, users, as so on.

## Citrix ITSM Adapter Service and ServiceNow Integration

Citrix ITSM Adapter integration with ServiceNow provides an easy way to extend ServiceNow capabilities into Citrix infrastructure. The Citrix Virtual Apps and Desktops service allows the organization to be more flexible and enables its workforce to be more productive with anywhere, any device access.

Citrix ITSM plug-in for ServiceNow enhances the capabilities of Citrix Workspace with out-of-the-box workflows, along with the ability to create custom workflows.

Primary customer use cases are:

*  User onboarding and off-boarding

*  Request for Citrix resources from a service-desk portal

*  The incident and problem management for provisioned resources from the service-desk portal

[![ITSM-Image-11](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_011.png)](/en-us/tech-zone/design/media/reference-architectures_itsm-adapter-service_011.png)

The preceding diagram describes the usage of the Citrix ITSM Adapter implemented for ServiceNow integration with a Citrix Virtual Apps and Desktops environment. ServiceNow provides a space for creating the catalogs which are relevant to the Citrix Virtual Apps and Desktops environment. Out-of-the-box workflows and orchestration of workflows are handled by ServiceNow. Apart from this task creating and updating CI records are handled by the ServiceNow platform.

Citrix ITSM Adapter provides brokering service between ServiceNow and Citrix Virtual Apps and Desktops environments. It reads ServiceNow catalog requests from end-users and routes them to the delivery controller. Once the request is forwarded to the delivery controller apps and desktops are provisioned on a particular resource location.

In the upcoming section all the use cases are discussed:

### Use case #1 Employee on-boarding and off-boarding

One of the common use cases of Citrix ITSM Adapter involves automating resource creation and deletion. In any given organization hiring contract workers often require them needing business apps and desktops to accomplish their job. This task is challenging for the IT help desk team as the number of on-boarding and off-boarding employee figures increases.

ServiceNow offers a pre-built integration kit as a catalog offering for supporting staff and user accounts. Citrix solutions are made easy with the Citrix Workspace automation service and ServiceNow.

For IT help desk, facilities, and security systems tasks are made easy by automating asset provisioning and de-provisioning of resources. Along with this offering, user accounts for Active Directory and HR management systems tasks are automated. In this process, account creation and management tasks are handled by the customer made catalogs.

For employee off-boarding, similar catalogs can be used. This revocation step includes rescinding all access rights, accounts, permissions, and provisioned resources. Less manual intervention in employee onboarding and offboarding that requires less personnel in the IT help desk team. This integration tends to increase employee productivity and focus on other strategies.

### Use case #2 Request for Citrix resources from a service-desk portal

Many organizations adopt digital workspaces for their workforce to realize the benefits including enhanced productivity and flexibility. In the production environment, a digital workspace is perceptible when the infrastructure is automated. Citrix Workspace provides resources for employees (apps and desktops) when the IT team or help desk provision them. Citrix ITSM Adapter offers complete automation of the provisioning of Citrix resources. In the automation environment, it is possible by integrating ServiceNow with Citrix Virtual Apps and Desktops.

ServiceNow offers a few out-of-the-box catalogs to provision MCS based resources on Citrix Virtual Apps and Desktops environments. The few samples are:

*  Request for a managed/shared virtual desktop

*  Request for a persistent virtual desktop

*  Request for virtual applications

*  Request for virtual application on behalf of other users

### Use Case #3 Incident, problem management for provisioned resources from a service-desk portal

A truly automated infrastructure offers the power to end users to restore a normal service operation. In the ITSM process, the goal of the incident management process is to restore normal service operation as quickly as possible. The incident management minimizes the impact on business operations, and in turn, service quality and availability are maintained.

In the same way, ServiceNow and Citrix ITSM Adapter offer an automated session reset and turn off the resources when not in use. For example, ServiceNow administrator initiates “reset a session” instruction from the catalogs and that has been carried through Citrix ITSM Adapter service. The ITSM Adapter, in turn, sends a command to the respective Cloud Connector to reset the session.

Here Citrix ITSM Adapter plays a key role, the workforce in the organization has the power to reset their own user sessions without intervention of the IT help desk team. This reduces the downtime on Citrix Virtual Apps and Desktops environments.

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s3be8c9250b641eb8).

## References

The following resources are referenced for a better understanding of Citrix ITSM Adapter Service:

[Citrix ITSM Adapter](/en-us/citrix-cloud/itsm.html)

[MCS PowerShell script](https://www.citrix.com/blogs/2012/03/06/using-powershell-to-create-a-catalog-of-machine-creations-services-machines/)

[CVAD Reference Architecture](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html)
