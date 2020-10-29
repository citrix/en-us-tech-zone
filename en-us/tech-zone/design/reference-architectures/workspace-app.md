---
layout: doc
h3InToc: true
contributedBy: James Hsu
description: Discover the technical aspects of Citrix's vision for the secure, modern digital workspace through the Citrix Workspace app - accessible on all your devices.
---
# Workspace App Reference Architecture

## Audience

This reference architecture document defines a set of architectural building blocks for delivering Citrix Workspace app and Citrix Workspace. The target audiences are technical professionals and architects seeking knowledge on how to deliver a unified user experience using Citrix Workspace services and Citrix Workspace app.

## Objective of this document

Citrix Workspace app works with Citrix Workspace Platform to deliver a unified user experience for single sign-on access to Windows, Linux, SaaS/ Web,and mobile applications through the service feeds in Citrix Workspace.
The scope of this document is limited to:

*  Architectural components of Citrix Workspace app and Workspace Platform solutions
*  High-level deployment procedures for the Workspace App
*  Example use case of Workspace App

## Citrix Workspace app and Citrix Workspace Platform Overview

Workspace is a device-specific environment for users to interact with their applications and data. Citrix Workspace app delivers capabilities including access to Citrix Virtual Apps and Desktops, comprehensive SaaS app control, and access to files and data via Citrix Content Collaboration. Citrix Workspace Experience UI (one of the service entitlements from Citrix Workspace services) can be delivered to the end user via HTML5 compatible browsers or native OS clients such as Windows, macOS, iOS, and Android operating systems. Citrix Workspace Premium Plus is the recommended Citrix Workspace Platform services to deliver the full unified user experience with Citrix Workspace app.

## Citrix Workspace app

The Citrix Workspace app unified user experience interface delivers application and data access. This unified user interface allows users to quickly access SaaS web applications and hosted virtualized Windows applications while browse and access aggregated cloud hosted and on-premises files together in one place. With this unified file and application access solution, customers can significantly improve user productivity and allow administrators to enforce granular security controls for SaaS and Internet policies.

[![WSAPP-Image-1](/en-us/tech-zone/design/media/reference-architectures_workspace-app_001.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_001.png)

Citrix Workspace app combines multiple engines in a single unified client app. There are 6 engines packaged in this unified workspace application simplifying user access to applications and data. The embedded browser allows administrators to provide native access to SaaS applications with enhanced security control. The Citrix HDX engine allows remote access to virtual desktops and apps. The Content Collaboration engine integrates access to all cloud and on-premises file storage repositories in one location. The Networking Engine provides secure and optimized connectivity to back-end resources. The Analytics Engine provides user and device behavior monitoring and enables risk analysis for user activities. The Management Engine offers auto-update and centralized management.

[![WSAPP-Image-2](/en-us/tech-zone/design/media/reference-architectures_workspace-app_002.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_002.png)

## Citrix Workspace Platform

The Citrix Workspace Platform provides a single platform for delivering a unified administration experience using a cloud-based management tool. Inside the Workspace Platform, customers can subscribe different cloud services to configure the desired user workspace experience. Examples of the services offered in the Workspace Platform are Virtual Apps & Desktops, Secure Browser, Gateway, Content Collaboration, Endpoint Management, Analytics and more. These services are updated and managed by Citrix which reduces deployment and system update effort by administrators and allows management staff to focus on other more strategic tasks. To learn more about the workspace platform services click [here](https://www.citrix.com/products/citrix-workspace/).

[![WSAPP-Image-3](/en-us/tech-zone/design/media/reference-architectures_workspace-app_003.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_003.png)

There are several Citrix Cloud Services that combine to enable the full workspace user experience. This reference architecture is based on the Citrix Workspace Premium Plus.

## Conceptual Service Deployment Architecture

Citrix Workspace app is a client app which consumes Citrix Workspace services. In this conceptual architecture, we are using the Citrix Workspace Premium Plus offering to deliver mobile, virtual, and web apps to Citrix Workspace app users.

The Citrix Workspace Platform is mostly managed and updated by Citrix as a SaaS service. Most of the configuration is performed in the cloud management portal. There are 7 conceptual steps on configuring and deploying a Citrix Workspace app and Citrix Workspace Experience UI to the end users. In the following section, we cover the system requirements and high-level steps needed to complete the setup using Citrix Cloud Services. This hybrid cloud approach can dramatically simplify the deployment and configuration process for customers while offering faster feature updates, new releases managed by Citrix, and a great centralized administrative experience. In this conceptual service architecture, we focus on the service architecture integration and provide resource links to more resources and details.

[![WSAPP-Image-4](/en-us/tech-zone/design/media/reference-architectures_workspace-app_004.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_004.png)

### Step1 Create Citrix Cloud account and deploy Citrix Cloud Connector

Citrix Workspace Platform Services is a service offering managed by Citrix in the cloud. To access Citrix Workspace Platform Services, customers need to request and activate a Citrix Cloud account through [https://onboarding.cloud.com/](https://onboarding.cloud.com/). After the required account is created, the administrator can download and deploy [Citrix Cloud Connectors](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html) to the resource location or locations containing Active Directory and the virtual machine hosting platform. Citrix Cloud Connector is a software package installed on a Windows Server 2012 R2 or Windows Server 2016 domain joined server. Cloud Connector authenticates and encrypts all communications between Citrix Cloud and the resource locations. All communications from the Citrix Cloud Connector to the Citrix Cloud Platform services are established via outbound SSL connections. See [Internet Connectivity Requirements](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html) for more info.

The Citrix Cloud control plane is hosted in the United States, European Union, and AsiaPac. Customers choose which control plane location they would like to use when they first sign up. No sensitive customer information is saved in the control plane. For more information on the Cloud Connector security related practice and extra considerations, see the [Secure Deployment Guide for the Citrix Cloud Platform](/en-us/citrix-cloud/overview/secure-deployment-guide-for-the-citrix-cloud-platform.html). For a list of Citrix Cloud Connector functions see [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html#cloud-connector-functions).

It is recommended to deploy two or more Citrix Cloud Connectors per resource location for high availability and scalability. For example, a set of three 4-vCPU Cloud Connectors is recommended for sites that host no more than 5,000 workstation VDAs. See [Scale and size considerations for Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-cloud-connector/cc-scale-and-size.html) for extra guidance.

The concept of a Resource Location is to define the location of the hypervisor or physical server/cloud location. There are no databases to install or management requirements for Citrix Cloud Connector.

[![WSAPP-Image-5](/en-us/tech-zone/design/media/reference-architectures_workspace-app_005.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_005.png)

### Step2 Configure and Publish Citrix Virtual Apps and Desktops

Configure Citrix Apps and Desktops using the admin console hosted by the Citrix Workspace platform. For a step-by-step configuration, see [here](/en-us/citrix-virtual-apps-desktops-service/install-configure.html). For a reference architecture on the Citrix Virtual Apps and Desktops Service, see [here](/en-us/citrix-virtual-apps-desktops-service/secure.html). For guidance on how to set up high availability in a resource location see [here](https://www.citrix.com/content/dam/citrix/en_us/documents/white-paper/ensuring-high-availability-xenapp-and-xendesktop-deployments-on-citrix-cloud.pdf). If the administrator wants to add an existing on-premises Citrix Virtual Apps and Desktops deployment in to Citrix Workspace, it is possible to use the Site Aggregation feature in the Citrix Workspace platform. See [Add an on-premises Site to Citrix Workspace](/en-us/citrix-cloud/workspaces/add-on-premises-site.html) for more information. To learn more about the concept of Site Aggregation, see this [video](/en-us/tech-zone/learn/tech-insights/site-aggregation.html).

[![WSAPP-Image-6](/en-us/tech-zone/design/media/reference-architectures_workspace-app_006.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_006.png)

### Step3 Configure Citrix Workspace and Enable Gateway Services

To enable secured user connections to the Citrix Workspace, the administrator needs to configure the Gateway Service for Virtual Apps and Desktops services or use the Citrix Gateway on-site. For more info, see [here](/en-us/netscaler-gateway/service/support-xenapp-xendesktop-service-apps.html).

Administrators can apply the existing Citrix Gateway server in the resource location which will be able to offer better control of network traffic in the resource location and allow extra Citrix Gateway network services to be enabled in the resource location. An example is Micro VPN services and more info can be found [here](/en-us/tech-zone/learn/tech-insights/micro-vpn.html).

For customers with small remote office or branch office locations, adding and managing a dedicated Citrix Gateway server might not be possible. In this case, administrators can use the hosted Citrix Gateway Service to address the requirement.

[![WSAPP-Image-7](/en-us/tech-zone/design/media/reference-architectures_workspace-app_007.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_007.png)

For customers who do not require external network user access to the Virtual Apps and Desktops in the resource location, it is possible to configure Citrix Workspace to deliver Virtual Apps and Desktops to internal users only. In this case, the user is connecting the Virtual Apps and Desktops via internal IP only.

### Step4 Enable Single Sign On (SSO) to Web/SaaS apps for publishing

Part of the Citrix Gateway Service (cloud services only) is single sign-on to SaaS applications. SSO delivers a unified user experience for Web-based SaaS application SSO and publishing to the Citrix Workspace Experience user interface. The administrator needs to provide the service URL to the users to access Citrix Workspace. To enable Single Sign-On user experience, the SaaS app trusts a SAML assertion provided by the Citrix Gateway Service. For more info on how to configure SSO see here. To enable single sign-on to SaaS applications, the process is now simplified in just a few web forms and clicking the configuration page greatly simplifies the deployment process. For a list of supported SaaS applications see here. For internal network hosted SaaS applications, it is possible for administrators to apply the Gateway Connector (in preview) to proxy the internal web-based SaaS application access to the external Workspace App user.

[![WSAPP-Image-8](/en-us/tech-zone/design/media/reference-architectures_workspace-app_008.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_008.png)

[![WSAPP-Image-9](/en-us/tech-zone/design/media/reference-architectures_workspace-app_009.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_009.png)

### Step5 Enable Secure Workspace Access service to monitor and block malicious, dangerous, or unknown websites

The Secure Workspace Access Service allows administrators to gain visibility of the user activities on the Workspace App and add enhanced securities to SaaS apps such as Watermark, copy-paste restriction, and preventing downloads.

Administrators can enable Enhanced Security when configuring a SaaS application SSO or publishing process. (Note: Enhanced Security setting needs to be configured in the “Add a Web/SaaS App” UI)

[![WSAPP-Image-10](/en-us/tech-zone/design/media/reference-architectures_workspace-app_010.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_010.png)

[![WSAPP-Image-11](/en-us/tech-zone/design/media/reference-architectures_workspace-app_011.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_011.png)

After configuring Gateway and SaaS application publishing, the administrator can extend the security control of the Workspace App SaaS app access by configuring web filtering to allow/block end user access and redirect them to the Citrix Secure Browser Service. Most importantly, combining Secure Workspace Access and Workspace App client software, administrators can review valuable user insights through Analytic services tracking user SaaS application usage and application usage pattern.
With Citrix Secure Workspace Access, administrators are able to control how a SaaS Application can be accessed by the end user via Citrix Workspace Experience web UI or native Citrix Workspace app client.

To learn more about how the Secure Workspace Access Service works with the Workspace app see this Video. For how to configure Secure Workspace Access see [here](/en-us/citrix-cloud/access-control/get-started.html).

[![WSAPP-Image-12](/en-us/tech-zone/design/media/reference-architectures_workspace-app_012.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_012.png)

### Step6 Link a Content Collaboration account and Endpoint Management service to Citrix Workspace services

To enable Content Collaboration in the Workspace App, the administrator must Create or link a Content Collaboration account to Citrix Cloud. For more information on how to configure Content Collaboration see [here](/en-us/citrix-cloud/create-content-collaboration-acct.html).

[![WSAPP-Image-13](/en-us/tech-zone/design/media/reference-architectures_workspace-app_013.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_013.png)

Enable Endpoint Management Service Integration under **Workspace Configuration > Service Integrations**.

[![WSAPP-Image-14](/en-us/tech-zone/design/media/reference-architectures_workspace-app_014.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_014.png)

Linking the Content Collaboration Service allows Workspace App users to access data in Storage Zones and External cloud file storage vendors with single sign-on. Enabling Citrix Endpoint Management integration will also improve device security management and allow administrators to encrypt selective company data on the mobile device.

[![WSAPP-Image-15](/en-us/tech-zone/design/media/reference-architectures_workspace-app_015.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_015.png)

### Step7 Deploy Citrix Workspace app or use HTML5 compatible browser to access Citrix Workspace Experience UI and enable Analytic services for Workspace App

Deploy Citrix Workspace app to mobile devices (iOS or Android), Mac, and Windows PCs. Download the Workspace App [here](https://www.citrix.com/downloads/workspace-app/).

Enable Analytics services by configuring required data sources. To learn more, see [here](/en-us/citrix-analytics/data-sources.html).

[![WSAPP-Image-16](/en-us/tech-zone/design/media/reference-architectures_workspace-app_016.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_016.png)

Administrator can also create more rules to automate some actions.

[![WSAPP-Image-17](/en-us/tech-zone/design/media/reference-architectures_workspace-app_017.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_017.png)

After completing these 7 steps, the administrator will be able to monitor user activities across multiple resource locations and increase security control on information access with Workspace App users.

End users are able to enjoy an integrated user experience from Local and Mobile apps delivered by the Endpoint Management and Workspace hub, Securely access SaaS and Web Applications through Secure Workspace Access and SSO too. Existing Citrix Virtual Apps and Desktops sites can be aggregated together along with Citrix Virtual Apps and Desktops resource locations through the Gateway Service. Reduce external web application access risks by redirect them to Secure Browser. Apply Content Collaboration with external users using local Storage Zones or public cloud storage platforms.

## Citrix Workspace app Use Cases

The modern work environment is rapidly changing from traditional client/server applications to cloud hosted SaaS applications. Users are demanding access to applications on any device, anywhere, anytime. IT needs a new security approach. One that provides a fine level of control over access and adjusts to contextual factors like location and user behavior. IT needs to be able to stay ahead of both internal threats and malicious external actors.

*Solution:*

*  Deliver Citrix Workspace using Citrix Cloud Services to maintain an evergreen environment.
*  Use the Gateway Service single sign-on to SaaS applications to reduce multiple user password complexities. Simplify support and improve user productivity.
*  Enable enhanced security policies to allow Workspace App to restrict web app and published applications and desktops.
*  Enable Advanced Secure Workspace Access to better protect SaaS application security and use web filtering to block access to blacklisted or redirect high risk URLs to Secure Browser to reduce risk.
*  Deliver the Content Collaboration Service to aggregate cloud content in one place.
*  Enable Endpoint Management to enforce endpoint device compliance and manage and deploy mobile apps on the endpoint.
*  Use Analytics services to improve visibility to user behavior activities and enforce policies when anomalies are detected.

[![WSAPP-Image-18](/en-us/tech-zone/design/media/reference-architectures_workspace-app_018.png)](/en-us/tech-zone/design/media/reference-architectures_workspace-app_018.png)

## Summary

Citrix Workspace app is the unified front-end application for the end user. To take full advantage of the Workspace App, one must also subscribe to the Citrix Workspace Premium Plus Service.

Citrix Virtual Apps and Desktops provides comprehensive Windows and Linux application and desktop virtualization.

Citrix Secure Workspace Access provides consolidation of point products that offer SSO, web filtering and secure browsing as separate offerings. It provides a single solution to meet your requirements for SSO and forward-proxy functionality with web filtering and secure browser services. For customers looking to implement just SSO, Citrix is the only vendor that offers SSO to Citrix Virtual Apps, Citrix Virtual Desktops, SaaS, and web applications. This helps consolidate your existing ICA Proxy with any third-party SSO solution you can be using today.

Content Collaboration allows users to share, sync, and secure content from the cloud and on-premises storage services.

Citrix Endpoint Management is a solution for managing endpoints, offering mobile device management (MDM) and mobile application management (MAM) capabilities. With Endpoint Management, you manage device and app policies and deliver apps to users. Your business information stays protected with strict security for identity, devices, apps, data, and networks. Combining Citrix Secure Hub Mobile productivity apps and Citrix Workspace app, the administrator can better control mobile devices.

Combining Workspace App and Workspace Premium Plus Service, the administrator can deliver digital workspace on any device while maintaining security control with great user experience.
