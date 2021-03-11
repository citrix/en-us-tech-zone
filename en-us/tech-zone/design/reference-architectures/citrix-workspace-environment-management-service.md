---
layout: doc
h3InToc: true
contributedBy: Jian Luo, Cheng Zhang, Jack Zhou
description: Learn about the architecture and deployment considerations for this cloud-based service of Workspace Environment Management.
---
# Reference Architecture for Citrix Workspace Environment Management Service

## Introduction

Workspace Environment Management (WEM) service uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops deployments. It is a software-only, driver-free solution.

**Resource management**. To provide the best experience for users, WEM service monitors and analyzes user and application behavior in real time. It then intelligently adjusts RAM, CPU, and I/O in the user workspace environment.

**Profile Management**. To deliver the best possible logon performance, WEM service replaces commonly used Windows Group Policy Object objects, logon scripts, and preferences with an agent, which is deployed on each virtual machine or server. The agent is multi-threaded and applies changes to user environments only when required, ensuring that users always have access to their desktop as quickly as possible.

**Simplified setup and configuration**. WEM service eliminates most of the setup tasks required by the on-premises version of WEM. You can directly use the web-based administration console to tune WEM behavior.

## Cloud deployment overview

WEM service has the following architecture:

![WEM architecture](/en-us/tech-zone/design/media/reference-architectures_citrix-workspace-environment-management-service_002.png)

The following components are hosted in Citrix Cloud and administered by Citrix as part of the service:

**Infrastructure services**. The infrastructure services are installed on a multi-session OS. They synchronize various back-end components (SQL Server and Active Directory) with front-end components (administration console and agent). We ensure that sufficient, robust infrastructure services are available in Citrix Cloud.

**Administration console**. You use the administration console, available on the service’s **Manage** tab, to manage your user environments. You access the console using your web browser. The console is hosted on a Citrix Cloud-based Citrix Virtual Apps server, with a Citrix Workspace app for HTML5 connection to the console on the Citrix Virtual Apps server.

**Azure SQL Database**. WEM service settings are stored in a Microsoft Azure SQL Database service deployed in an elastic pool. This component is managed by Citrix.

The following components are installed and managed in each resource location by the customer or partner:

**Agent**. The WEM service agent connects to the WEM infrastructure services and enforces settings that you configure in the administration console. All communications are over HTTPS using the Citrix Cloud Messaging Service. You can deploy the agent on a Virtual Delivery Agent (VDA). Doing so lets you manage single-session or multi-session environments. You can also deploy the agent on a physical Windows endpoint.

All agents use local caching. This behavior ensures that agents can continue using the latest settings if network connection is interrupted.

> **Note:**
>
> The Transformer feature is not supported on multi-session operating systems.

**Microsoft Active Directory Server**. WEM service requires access to your Active Directory to push settings to your users. The infrastructure service communicates with your Active Directory using the Citrix Cloud identity service.

**Cloud Connector**. The Citrix Cloud Connector is required to let machines in your resource locations communicate with Citrix Cloud. Install Citrix Cloud Connector on at least one machine in every resource location you are using. For continuous availability, install multiple Cloud Connectors in each of your resource locations. We recommend at least two Cloud Connectors in each resource location to ensure high availability. If one Cloud Connector is unavailable for any period of time, the other Cloud Connectors can maintain the connection.

## Communications between agent and infrastructure services

### Agent connection overview

WEM uses WCF for communications between the WEM agent and the WEM infrastructure server.

WCF is a framework created by Microsoft, and it is part of .Net Framework. WCF can use different underlying protocols, such as SOAP over TCP or SOAP over HTTP.

For on-premises deployments, WCF connections between the agent and the infrastructure service use TCP binding. For cloud deployments, WCF uses HTTP binding to suit various HTTP proxies and firewall configurations. Communications on public networks use HTTPS on port 443.

### Agent connection insights

![Agent connection insights](/en-us/tech-zone/design/media/reference-architectures_citrix-workspace-environment-management-service_001.png)

In the diagram, there are two different agent-facing WCF services:

**Agent cache sync service** – A proxy service for `Dotmim.Sync` framework, used by agents with SQLite.

**Agent broker service –** A general purpose WCF service for the agent to download and upload data.

All the services are session based. When a session is established, WEM infrastructure server validates the service key first. If service key validation fails, the session is terminated.

Each session contains multiple HTTP requests and responses.

Agent services basically function as a proxy layer between the agent and the remote database. Agent services receive agent requests and run the corresponding database queries. Database query results are then communicated to the agent.

### Agent cache sync service

Agent cache sync service is intended for the WEM agents with SQLite to synchronize agent local cache. The agent cache sync service relies on `Dotmim.Sync`, an open source sync framework. Agent cache sync service is for SQLite local cache synchronization only. Agent cache sync service does not query agent AD information because the information is not needed for cache synchronization. Cache synchronization is simply a matter of replicating Azure database to agent local cached database.

### Agent infrastructure service

Agent infrastructure service serves the following purposes:

#### Retrieving agent service settings

The WEM agent service settings are WEM settings not assigned to users, for example, advanced settings and optimization settings. The WEM agent retrieves agent service settings if any one of the following conditions is met:

-  Agent service starts (because of machine startup)
-  Periodical agent service settings refresh (by default 15 minutes)
-  Agent network resumes
-  WEM administrators trigger a refresh of service settings

#### Retrieving user assigned actions

When a user logs on or reconnects, the WEM agent retrieves the following information on demand from the infrastructure services:

-  WEM user list
-  WEM filter rules and conditions
-  WEM actions assigned to users (for example, printers, net drives, applications)
-  User-related settings (for example, USV settings, environmental settings)
-  AppLocker settings
-  Other settings (for example, transformer settings)

Starting with the February 2020 release, the WEM agents use local cache by default to retrieve the information above. (The **Use Cache to Accelerate Actions Processing** option is enabled by default.)

In general, retrieving user-assigned actions consumes more resources because that process involves more requests and responses. In the cloud service, retrieving user-assigned actions rarely occurs (less than 4%) after the introduction of the **Use Cache to Accelerate Actions Processing** option.

#### Uploading statistical data

The WEM agent uploads the following statistical data:

-  Agent information (for example, IP, device name)
-  Agent statistics (for example, machine start time, cache sync time)
-  User information (for example, SID)
-  User statistics (for example, logon time)
-  CEM group policy processing results
-  Agent callback information (how the Cloud Connector connects back to agents)

If the WEM agent fails to upload some data, those data are saved to the agent local database. The agent will attempt to upload those data the next time.

#### Miscellaneous

Agent infrastructure service also functions in the following scenarios:

-  Manually check agent upgrade
-  Download Citrix Optimizer templates
-  Retrieve assigned agent tasks

## WEM service onboarding

### Onboarding preparation

Customers who have the following Citrix Cloud entitlements are able to use WEM as a service:

-  Citrix Virtual Apps and Desktops
-  Citrix Virtual Apps
-  Citrix Endpoint Management service
-  Citrix Workspace Premium
-  Citrix Workspace Premium Plus

> **Notes:**
>
> Customers using the above-mentioned services as trials get the WEM service trial as well. WEM trial is available for Cloud use only. With the trial, customers get full access to WEM service functionalities.

Before the onboarding, customers also need to make sure that their resource locations and Cloud Connectors are set up and able to connect to Citrix Cloud. To understand how to prepare the connection, read [Connect to Citrix Cloud](/en-us/citrix-cloud/citrix-cloud-resource-locations/resource-locations.html) for more details.

### Onboarding to the WEM service

To use the WEM service, you need to sign in to Citrix Cloud and then launch the WEM service. Following the steps described in **Get started with your Workspace Environment Management service**, you can onboard to WEM and start the use.

## Migrating on-premises WEM to the WEM service

The migration can be performed by running a toolkit. With the toolkit, you can migrate your existing on-premises WEM database into the WEM service. The toolkit includes a wizard to generate an SQL file containing the content of your WEM database. You can then upload the SQL file to the WEM service Azure database. For more information, see
[Migrate](/en-us/workspace-environment-management/service/migrate.html).

## Scale and size considerations for Cloud Connectors

The WEM service is designed for large-scale enterprise deployments. On the server side, WEM service monitors the communication flow between front- and back-end components and scales up or down dynamically based on data in transit.

When evaluating WEM service for sizing and scalability, you need to consider only the number of Cloud Connectors and the Cloud Connector machine specification. A Cloud Connector with the following machine specification can support up to 10,000 agents:

-  4 vCPUs, 8 GB RAM, and 80 GB of available disk space.

To ensure high availability, we recommend at least two Cloud Connectors in each resource location. The WEM agent balances the load among Cloud Connectors automatically. If the Citrix Cloud Connectors in place are not for WEM service only, consider deploying more Cloud Connectors.

For information about Cloud Connectors, see [Citrix Cloud Connector](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html).

## The recommended configuration

We can use three types of configurations to deploy WEM. Use the type that suits the actual environment the best.

### Single domain in a single forest

This configuration can be used in an environment that has only a single domain in a single forest. Normally, the single domain contains all the resources and user objects. So, in this configuration, you only need to deploy one set of Cloud Connectors to enable all your devices to connect to the WEM service. Below is the overview of this configuration.

![Single domain in a single forest setup](/en-us/tech-zone/design/media/reference-architectures_citrix-workspace-environment-management-service_004.png)

### Multiple domains in a single forest

This configuration can be used in environments where multiple domains in a single forest exist. As the domains in the forest can communicate with each other, in this configuration, you only need to deploy one set of Cloud Connectors to enable all your devices to connect to the WEM service.

![Multiple domains in a single forest setup](/en-us/tech-zone/design/media/reference-architectures_citrix-workspace-environment-management-service_003.png)

### Users and resources in separate forests (with trust)

In this use case, users and resources reside in different domain forests for management purposes. A trust exists between the two forests that allows the users to log on to resources in another forest. In this deployment, customers need to deploy Cloud Connectors into each domain forest to complete the WEM deployment.

![Users and resources in separate forests setup](/en-us/tech-zone/design/media/reference-architectures_citrix-workspace-environment-management-service_005.png)

To learn more, visit the [WEM product documentation](/en-us/workspace-environment-management/service.html). For the latest updates about WEM, check out [What’s new](/en-us/workspace-environment-management/service/whats-new.html).
