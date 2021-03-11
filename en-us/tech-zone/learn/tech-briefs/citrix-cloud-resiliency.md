---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Fernando Klurfan, Rick Feijoo, Kenneth Bell, Ashish Gujrathi, Himanshu Agarwal, Dan Feller
description: Learn how Citrix Cloud services are architected and built for resiliency. Understand how Service Continuity features enable users to connect to the resources that are accessible even if some or all of the cloud services are unreachable.
---
# Citrix Cloud Resiliency

## Introduction

Digital transformation initiatives are currently top of mind for a large section of enterprises. Moving their apps and desktops delivery infrastructure to the cloud is one of the major considerations of CIOs. Cloud providers are bringing cloud only solutions or cloud-based resources bundled with attractive pricing, with the added benefit of simplified operations and lower management costs. A couple of the first things to evaluate for the IT departments, when considering a move to the cloud, are the uptime and the fault tolerance of the proposed cloud-based solution. This consideration equally applies to a prospective or existing Citrix customer, who wants to utilize Citrix Cloud as their Workspace solution.

This brief is designed to address:

1.  These reliability considerations and lay out the various ways in which Citrix is working towards making Citrix Cloud and the services offered progressively more resilient to failure.
1.  How Citrix Cloud is deployed in a way that in the rare case a service(s) is unreachable, the end user continues to have access to resources that are not affected by the unavailability of the service(s).
1.  How Citrix Cloud is using the technologies exposed by cloud providers, that Citrix Cloud services run on to make the services highly available and fault tolerant.

The important thing to note is that all the items covered in the document work together as a combined solution to form multiple layers of a net that protects the organization from an outage and enables access during one.

## High availability

Let’s first look at improvements being made to the Cloud services and the underlying infrastructure to ensure that services don’t fail. This resiliency is achieved by building highly available services, that can be scaled up easily to meet customer demand.

Utilization of Azure Availability zones ensures that the broker and the associated databases are resilient to a cloud outage.

### Platform high availability and geographic node deployment model

The cloud services delivered by Citrix are built on top of various platforms that work together to provide the users with access to Workspace and the resources made available from it. Each of these platforms’ architecture is continuously being improved to be highly available and fault tolerant. A design decision has been made to have the platforms be deployed based on the [Azure geode pattern](https://docs.microsoft.com/en-us/azure/architecture/patterns/geodes). Each of the platforms is in different stages of the journey to adopt this pattern.

![Citrix Cloud Resiliency - Geographic nodes based deployment model](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_1-geographic-nodes-deployment-model.png)

In a geographic node pattern, platforms are spread across multiple deployments in different nodes that are globally diverse and utilize the storage replication in other deployments. Within each geode the fully self-contained platform services are deployed in different Azure fault domains. Most critical services can failover between these deployments.

Even if an entire Azure region is not reachable, the platform services would be able to service requests from another geode from a different region. For organizations that have regulatory requirements concerning data sovereignty, the platforms are deployed so that servers are deployed within the required geographic location. Organizations can choose between different Citrix Cloud instances to guarantee their data sovereignty needs.

![Citrix Cloud Resiliency - Deployment options for data sovereignty](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_2-data-sovereignty-deployment-options.png)

## Citrix Virtual Apps and Desktops Service

For the Citrix Virtual Apps and Desktops (CVAD) service, there are many different features implemented to make it resilient and fault tolerant. Some of the features are:

### Rendezvous protocol

To ensure greater scalability of the Citrix Cloud Connectors, the HDX protocol was modified in CVAD version 1912. The Rendezvous protocol enables HDX sessions to bypass the Citrix Cloud Connector and connect directly and securely to the Citrix Gateway service. Bypassing the HDX traffic, frees up bandwidth and compute resources on the Cloud Connector. Enabling the Connector to handle more connection requests and increase the number of resources that a Cloud Connector can manage.

Learn more about the Rendezvous Protocol and how to enable it via policies [here](/en-us/citrix-virtual-apps-desktops-service/hdx/rendezvous-protocol.html).

### Fall back to StoreFront

  >**Tech Preview Disclaimer**
  >
  >**This feature is currently in tech preview.** The information in this article is for information purposes only, provided “AS IS” without warranty, and is subject to change at Citrix’s discretion without notice. Any terms governing Citrix products or services shall be contained in a written agreement ran by the parties. Contact your account manager to request access to this tech preview.

The Workspace platform allows customers with a hybrid deployment, to display the URL of the on-premises StoreFront (when they are unable to launch sessions via the Workspace service). So, users accessing their Citrix Workspace via a browser can access resources in the on-premises environment through the StoreFront URL.

Admins, using PowerShell, can define a fallback StoreFront HTTP(S) URL, that resolves to an internal StoreFront or an external Citrix Gateway server. Required steps are included in the private tech preview documentation.

![Citrix Cloud Resiliency - Fall back to StoreFront](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_3-fallback-to-storefront.png)

When a user attempts to log in or launches a resource and the Workspace Platform returns an error, the Workspace app presents the StoreFront URL to the users. This occurs when the Workspace service is not reachable, there was a CVAD launch failure, or authentication failure occurs resulting in resource launch failure.

  >**Note:**
  >
  >If you have Service Continuity configured, it is going to ‘win’ and show the offline UI, as opposed to the error screen with fallback URLs you would see with fallback enabled.

The user can then use the StoreFront URL to connect directly to the on-premises store and access resources that are published from it.

## Service Continuity

Having seen the different ways in which Citrix is ramping up the supported scale limits to make your Cloud deployment resilient, let us shift our attention to how Citrix ensures, in an unlikely event of a cloud service failure or outage, the user is able to continue accessing their resources from multiple resource locations.
With a goal to make resources available even when Citrix Cloud services are not reachable, teams at Citrix have implemented many improvements, from the ground up, under the umbrella of Service Continuity.

The set of functionalities implemented under Service Continuity include but are not limited to:

1.  A mechanism to create and distribute Workspace connection leases to each client endpoint, that has at least authenticated once to the Workspace. The Workspace connection lease files tell that endpoint (per-user) what apps and desktop resources are available from which locations managed by which Cloud connector. Essentially eliminating the single resource location limitation.

1.  The implementation of the Progressive Web App (PWA) within the Citrix Workspace app that can detect when certain service(s) are not reachable and based on that show the user what resources are currently accessible. It displays a banner to let the user know that there has been a loss in connectivity as well.

1.  The ever-increasing footprint to global Citrix Gateway Points of Presence (PoP), with built-in HA and multi-cloud presence, ensures that the user is always able to reach the Citrix Gateway service.

Before the deep dive into how these solutions work, let’s look at how different scenarios affect the Citrix Cloud service availability and what resources are available or not, when such a scenario occurs.

1.  Workspace URL / `Cloud.com` store

    With Service Continuity, when the user is unable to reach the Workspace URL, a banner on top of the Workspace app informs the user that a subset of the resources is unavailable. Citrix Workspace app modifies the resource icons to easily distinguish resources that are still available.

1.  Citrix Virtual Apps and Desktops / Broker Service only

    In the scenario, only the CVAD or Broker Service is experiencing an outage, the user would continue to have access to the Web and SaaS apps, in additions to the virtual apps and desktops resources. The Workspace app UI (empowered by PWA) aggregates all the resources from the service feeds and relies on Workspace connection leases for CVAD apps only (see per-feed in-app cache diagram).

    ![Citrix Cloud Resiliency - Workspace unreachable banner](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_4-workspace-unreachable-banner.png)

    Citrix Cloud Connectors now take over all the brokering responsibilities for the Resource Location – VDA re-registrations to the primary Cloud Connector are triggered. Load management and other operations are handled by the Cloud Connector as well. Read more about VDA power management during outages [here](/en-us/citrix-workspace/service-continuity.html#power-management-of-vdas-during-outages).

1.  Identity Service

    In the scenario where the identity service is not reachable, the user would continue to have access to all the CVAD service resources reachable from the Cloud Connectors. The connection leases (explained a little later) contain the authorization tokens for the resources and the user. The user will be asked by the VDA to authenticate and log in at the desktop or app, (unless the machine is sharing a session for the same user and they are already authenticated from a previous connection lease based launch).

1.  Internet – (Only internal resources are accessible if there is connectivity)

    In the scenario where the client endpoints are not able to reach the internet, but still have network connectivity to Cloud Connectors and VDAs (on the internal network). All virtual apps and desktop resources accessible on the internal network are available during the loss of Internet access.

1.  Gateway or Secure Workspace Access service – Fail over to a different PoP

    The Citrix Gateway service is built to be highly available with multiple instances of the service, deployed on multiple Points of Presence (PoP) across various locations in the world. Also, the service is hosted on different cloud providers. Find the list of PoPs [here](https://support.citrix.com/article/CTX270584).

### Progressive Web app

A progressive web app differs from a normal web app by including background processing of cached elements that are to be displayed in the event of an inaccessible web server, using the cache to populate the UI of the app. For service continuity, the progressive web app behaves in the same way when either the Workspace service is not reachable or is unable to return the list of resources that are assigned to the user.

![Citrix Cloud Resiliency - Progressive Web app per feed cache](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_5-pwa-per-feed-cache.png)

The Citrix Workspace app uses the new CLXMTP (Connection Lease Exchange & Mutual Trust Protocol) to talk to the Cloud Connector or Gateway service. For internal users it runs on Citrix Common Gateway Protocol (CGP) TCP port 2598 and for external users on CGS TCP port 443. This entire process is transparent to the user.

### Changes in user experience

After Service Continuity Is enabled, when the Citrix Workspace app attempts to connect to Workspace and is unable to reach the store. Provided the user has valid Connection Leases, we introduce support for cancelable Auth UI and fallback to cashed assets in the native Workspace app.

![Citrix Cloud Resiliency - Progressive Web app cancel button](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_6-pwa-cancel-button.png)

A banner is displayed to let the user know that there has been a loss in connectivity. The resources are displayed from the cache. Depending on the impact, (as discussed in the previous section) it dynamically grays out the resources which are currently not accessible.

![Citrix Cloud Resiliency - Unable to connect to resources Workspace app view](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_7-unable-to-connect-to-resources-workspace-app.png)

The list of accessible resources is obtained from the connection lease (discussed in the next section) stored on the endpoint.

Once connectivity is reestablished, the Workspace app gives the user, the ability to reconnect to the Workspace. The user can click the “Reconnect to Workspace” link in the banner to restore access to the resources that were previously grayed out.

![Citrix Cloud Resiliency - Unable to connect to resources banner](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_8-unable-to-connect-to-resources-banner.png)

For the user to be able to access the resources, when the Workspace identity service is not accessible for authentication, the connection lease behaves like a long-lived authorization token. The user is required to authenticate with the resource when launched to get access to the apps or desktops, using their AD credentials or SmartCard PIN.

If Citrix Workspace app and the VDA are joined to the same domain and the SSO pass-through plug-in is configured in Workspace app, SSO is achieved. Session sharing is also supported, so if a subsequent app is launched via Workspace Connection Lease from the same VDA where the user has an existing session also launched via Workspace Connection Lease, SSO is achieved.

![Citrix Cloud Resiliency - Desktop authentication needed with Service Continuity](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_9-authentication-dialog.png)

### Workspace connection leases and how they work

When all services are reachable, a `.ICA` file is generated that enables the client to connect to the app or desktop by providing a one-time short-lived access ticket. This ticket contains information of the resource that the user would connect to. The `.ICA` file is generated by Workspace, by retrieving the list of resources that are assigned to the user (a.k.a enumeration) and then identifying which VDA can host the session (a.k.a resolution).

[![Citrix Cloud Resiliency - .ICA file creation process](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_10-ica-file-creation.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_10-ica-file-creation-large.png)

In the case of any of the required services being unreachable the `.ICA` file cannot be generated. To overcome this per launch/reconnect requirement, the Workspace connection lease was designed.

A Workspace connection lease by default is valid for 7 days (configurable from 1 to 30 days). It is a long lived authorization token for ALL the resources the user is assigned to. It contains a list of Cloud Connectors that can be used to reach the resources (either directly or via the Citrix Gateway Service). In case CGS is involved, the Workspace connection lease also contains a global Citrix Gateway service FQDN address that can be used to connect to the resources.

A Workspace connection lease is a per user, per endpoint set of tokens which contain a cache of all the resources that the user is entitled to. They contain information of the Citrix Gateway service, and all Cloud Connectors that can service the connection request. As the connection lease is bound to the user and endpoint, it cannot be copied over to another device and used to launch sessions.

For Workspace connection leases to be created, the user must sign in at least once into Citrix Workspace using the Citrix Workspace app natively (and not via browsers). One the user signs in, the Connection Lease Issuing Service (CLIS) in Citrix Cloud receives a request to generate the leases for the user and endpoint combination. The Connection Lease Issuing service queues this request and forward it to the cloud broker (to not overload it). As the connection lease contains all combinations of the Cloud Connectors and resources that can service a user’s session request this process is done asynchronously and once the leases are generated, they are pushed onto the endpoint. This process can take up to 10 mins depending on the load on the broker.

The Workspace connection lease is made up of three concatenated files that are encrypted and signed by Citrix Cloud and stored securely on the endpoint. The three files are Metadata, Common Parameters and Resource location. On a windows endpoint, they are stored in each user’s AppData\Local folder. The path is `%LOCALAPPDATA%\Citrix\SelfService\ConnectionLeases\<StoreName>\<SiteName>\<username>\leases`

As seen in the following image the user is entitled to 6 resources and so there are 18 files, in the folder.

![Citrix Cloud Resiliency - Workspace connection leases](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_11-workspace-connection-leases.png)

The connection leases are tamper-proof. A bad actor would be unable to edit the lease, to say extend the duration of the lease. Soon as any tampering is detected, the connection lease hash is invalidated. The connection lease can be blocked by the IT Admin using the CVAD remote PowerShell SDK, in situations such as closed or compromised user account, stolen endpoint or device deallocation. Read about how to apply these policies [here](/en-us/citrix-workspace/service-continuity.html#what-makes-it-secure).

The following is the process diagram for the creation of the connection leases, when the user authenticates for the first time from an end point and launches a session.

[![Citrix Cloud Resiliency - Citrix Cloud reachable - Workspace connection leases creation process](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_12-leases-creation-process.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_12-leases-creation-process-large.png)

The following is the process that occurs in the scenario when some of the Citrix Cloud services are not reachable from the Workspace app.

[![Citrix Cloud Resiliency - Citrix Cloud unreachable - Workspace connection leases based launch](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_13-connection-lease-based-launch-external-user.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_13-connection-lease-based-launch-external-user-large.png)

To enable a larger subset of the current customer base, with VDA versions as far back as the Citrix Virtual Apps and Desktops 7.15 LTSR, the Cloud Connector with Local Host Cache (LHC) is used when the Cloud broker is not accessible. Else changes to enable Service Continuity would have required the VDAs to be upgraded. To know more read the [LHC Tech Brief](/en-us/tech-zone/learn/tech-briefs/local-host-cache-ha-cvads.html).

Note: In the scenario where the Citrix Cloud Broker Service is reachable (regardless of whether the Workspace store is accessible or not), the Cloud Connector always relies on the Broker Service for brokering.

The process is the same when the Resource Location is configured as “Internal only”, except in this case the Workspace app directly talks to the Cloud connector in the Resource location where the desired app is hosted.

[![Citrix Cloud Resiliency - Citrix Cloud unreachable - Workspace connection leases based launch](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_14-connection-lease-based-launch-internal-user.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-cloud-resiliency_14-connection-lease-based-launch-internal-user-large.png)

**Note: Even with the presence of connection leases, the default mechanism to launch resources is to attempt to obtain a `.ICA` file. When this is not possible, the Citrix Workspace app falls back to rely on the Workspace connection lease, transparently and silently to the user.**

The Workspace connection leases are cleared from the system when a user logs out of the Workspace app. The Workspace connection leases are retained if the user exits the Citrix Workspace app.

This behavior can be modified via remote PowerShell commands against their cloud broker instance by the IT Admin.

#### System requirements

For a list of current requirements visit the [docs page](/en-us/citrix-workspace/service-continuity.html#requirements-and-limitations).

*  Customer is using Citrix Virtual Apps and Desktops service and Workspace experience.

*  Workspace UI must be accessed via native Citrix Workspace app for Windows 2012 and for Mac 2011.X or later only, and not via Browsers (a.k.a Workspace app for Web)

*  Citrix Virtual Delivery Agent (VDA) version 7.15 and above, LTSR or one of the current CRs

*  Up-to-date Cloud Connector. Since service continuity relies on Local Host Cache functionality (but without requiring any on-prem access layer), we recommend sizing the Connectors with the same size we recommend for LHC. Review [LHC scale and size considerations](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache-size-and-scale.html) for more info.

*  Workspace Identity: AD (or with Azure AD Connect sync); AD + Token (OTP); AD + RADIUS, Azure AD; Citrix Gateway on-prem where the primary claim is based on AD based authentication. Policy based authentication is not compatible with Service Continuity and they are mutually exclusive.

*  Customer’s VDAs are online: not impacted by an AWS/Azure/DC outage or powered off by Autoscale.
Caveat: if the Resource Location is using Citrix Hypervisor or vSphere, the [Cloud Connector can now perform power management actions even if the Cloud broker is offline](https://www.citrix.com/blogs/2020/08/31/tech-preview-power-operations-during-local-host-cache-lhc-mode/).

*  Supported workloads: Hosted Shared apps or desktops, static/dedicated desktops, random/pooled desktops, Remote PC Access

*  Layer 3 network connectivity between the endpoint with CWA and Connector and VDA running the resource, either via

    *  Direct (LAN) (in this case, Connector and VDA must be reachable over TCP 2598)

    *  Citrix Gateway Service (TCP 443)

The procedure to configure Service Continuity can be found [here](/en-us/citrix-workspace/service-continuity.html#configure-service-continuity).

**Service Continuity is currently in public Tech Preview and you can sign up to try it using this [link](https://podio.com/webforms/25148648/1854298).**

### Monitor scalability improvements

The Day 2 and onward upkeep of a CVAD service environment relies on the ability of admins to easily manage the environment and resolve the user issues quickly and effectively. To ensure that Monitor can support large organizations, partners and resellers, who create CVAD service based solutions supporting with large multitenant deployments, Monitor included these features to be more resilient.

1.  **Read replicas being used to increase performance**

    Director has implemented mechanisms such that most of the read queries to the DB are rerouted to read replicas of the databases. A read replica is a read-only copy of the Database. This enables the write operations to complete much faster on the source database increasing its scale and resilience.

2.  **Database deployment following best practices**

    Splitting the Site DB and the Configuration logging and Monitor DBs, supports higher number of queries and auto scaling of the DB.

3.  **Session admin role introduced**

    For larger deployments, the CVAD service offers the Session Administrator role. This role provides permissions in Monitor typical of the Level 1 help desk role. With the session administrator role, the same CVAD service instance offers a higher concurrency rate for help desk administrators.

4.  **Increased supported admin count**

    Large organizations and resellers also require a larger number of admins that can manage their vast deployments. This requires the DB to scale and handle a higher number of concurrent queries. The number of supported admins has been increased to support these customers.

## Citrix Gateway service

The Citrix Gateway service is built to be highly available with multiple instances of the service, deployed on multiple Points of Presence (PoP) across various locations in the world. Also, the service is hosted on different cloud providers. Find the list of PoPs [here](https://support.citrix.com/article/CTX270584).

Within a Citrix Gateway service PoP, the micro services and tenants are deployed in a fully redundant active-active model. This functionality allows any component to switch over to the standby if there is a failure. Only in rare cases, if all the services of a component within a PoP fail, does the Gateway service mark itself as down.

Citrix uses the Intelligent Traffic Manager to monitor PoP health and automatically uses DNS to switch traffic to an alternate PoP if necessary.

Combined with the resiliency of the Gateway service, Service Continuity enables access to virtual apps and desktops resources, as long as the Gateway service and / or the Resource Location can be reached from the clients.
