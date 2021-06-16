---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
specialThanksTo: Adam Lotz, Daniel Feller
description: Learn how Citrix Virtual Apps and Desktop service enables you to deliver virtual apps and desktops to your end users, while offloading the management plane to Citrix Cloud ensuring your environment is always up to date.
tz_title: Citrix Virtual Apps and Desktops service
tz_products: citrix-virtual-apps-and-desktops;
---
# Tech Brief: Citrix Virtual Apps and Desktops service

## Overview

Citrix Virtual Apps and Desktop service (CVADs) enables you to securely deliver a high-performance virtual apps and desktops experience to any device. Run by a Citrix-hosted management service, this solution delivers end user access to Windows and Linux applications and desktops securely from a central location, regardless of the operating system on which the endpoints run.

Users get flexibility and consistency. They are no longer tied to a specific endpoint.

IT gets benefits from this hybrid cloud offering as well. First, you get data security and IP protection since your workloads are hosted and managed by you. You can deploy and manage desktops and applications from Citrix Cloud services, even if all workloads are hosted on-premises. Our set of admin tools enables you to deploy to cloud and on-prem locations as your implementation evolves. You can conveniently host apps depending upon use cases. What’s more, you can move to the cloud as it suits your business needs.

Many on-premises Virtual Apps and Desktop environments are outdated because administrators don’t have the time to manually address patches and upgrades. Not only do outdated environments degrade the user experience, but also, they pose security threats to your organization. Citrix Virtual Apps and Desktops service enables you to maintain complete control over your applications, policies, and users. But you offload control plane maintenance, patching, and upgrades to Citrix. This ensures your environment is always up to date.

Customers who have made the switch to Citrix Cloud services can rapidly scale their environments, flex to address unexpected business conditions, and deploy new features easily. All without having to deploy new infrastructure.

## Why Migrate to Citrix Virtual Apps and Desktop service

Migrating to the Citrix Virtual Apps and Desktops service lets IT focus on business needs and end-users take advantage of the latest features and functionality. Citrix Cloud services help you transition to cloud at your own pace. With a hybrid, multi-cloud approach, you have the flexibility to choose the appropriate data center or cloud to host each application.

Citrix Virtual Apps and Desktop service is cloud and hypervisor agnostic. You avoid lock-in and give your organization true resource flexibility. Citrix Cloud services give your business the agility needed to respond to a disaster recovery or business continuity event without having to deploy new infrastructure. These services also allow you to adapt your workspace to address new use cases quickly and easily. You get enhanced visibility and control over all your Citrix infrastructure. Some of the reasons why to use Citrix Virtual Apps and Desktop service are listed below.

### Global Availability

Citrix Cloud is deployed in multiple regions worldwide and is designed using industry best practices to achieve a high degree of service availability (you can [read more about current SLA in product documentation](/en-us/citrix-cloud/overview/service-level-agreement.html)). To ensure the best user experience, Citrix Virtual Apps and Desktop service can be deployed with Citrix Gateway service. Citrix Intelligent Traffic Management (ITM) ensures fast and reliable sessions.

In a traditional on-prem environment, customers architect and design the environment to include multiple sites. This ensures end-users can access their applications and desktops even if there is an outage. Because Citrix Cloud is available in multiple points of presence (POPs) around the world, it delivers optimal uptime and eliminates the need for administrators to deploy in multiple sites.

Citrix Gateway service operates with ITM in multiple POPs throughout the world. If for any reason, a POP goes down or if the connectivity is degraded, Citrix ITM responds to subsequent domain name system (DNS) queries from the next closest POP. More information is available [here](/en-us/tech-zone/learn/tech-briefs/gateway-hdxproxy.html).

Citrix Cloud has a global presence. A global footprint enables you to choose a centrally located control plane region to serve your users. You can choose between United States, European Union, or Asia Pacific regions to ensure the best performance. Your published apps and desktops can be hosted in any location worldwide. Policy controls enforce data sovereignty and protect intellectual property.

### Hybrid Cloud

Citrix Virtual Apps and Desktop Service (CVADs) allows you to manage on-premises data center and public cloud workloads together in a hybrid deployment. CVADs allows you to connect to public clouds Microsoft Azure, Amazon (AWS), and Google Cloud, alongside on-premises hypervisors such as Citrix Hypervisor, Microsoft Hyper-V, and VMware vSphere. By allowing a hybrid, multi-cloud approach, Citrix gives you the flexibility to deploy different applications in different resource locations worldwide.

You can choose which hosting location or type makes the most sense for your business – by user group, geographic region, cost, or available capacity. A single Workspace end user can seamlessly access applications hosted on different back-end resource locations (for example: cloud or on-prem). The experience is the same regardless of the origin from which the apps launch.

### Image Management

A sophisticated provisioning system allows IT to scale up Citrix deployments to tens of thousands of users. Administrators can choose to use either Machine Creation Services (MCS) or Citrix Provisioning Services (PVS) to create machine catalogs. These tools help administrators maintain their images throughout their lifecycles and ensure that they remain consistent even as patches or new applications need to be installed. MCS operates by cloning a master image to create identical VMs within the catalog that your users access. Citrix Provisioning is a streaming technology that streams OS images to the virtual machines.

Citrix’s App Layering technology allows for the same image and applications to be used across different hypervisors or platforms. It reduces the time required to manage Windows applications and images. This time savings are achieved by separating the OS, the platform tools, and the applications in separate layers. When it comes time to update an individual component, it gets propagated across all the images that have that specific layer. App Layering significantly reduces image sprawl, and saves valuable time maintaining your environment.

There are more ways to realize the cost benefits of virtualization at scale. *User personalization layers* offer user-based customizations in non-persistent virtual environments. User layers provide users with an experience that mimics that of a dedicated desktop while offering the management and cost savings of a non-persistent Windows image.

In this type of deployment, users are provisioned a shared Windows instance for their desktops. As users log in, a user layer with their specific customizations is generated. This user layer allows their profile settings, data, and locally installed applications to persist in a portable container. When users log off, the desktop session is destroyed. However, individual data is retained and seamlessly applied at the next login. This happens regardless of whether users are assigned the same -- or different -- back-end virtual machines.

You can enable the following user layers:

-  **Full User Layer:** includes all of the user’s data, settings, and locally installed applications are stored within the user layer
-  **Office 365:** for desktop OS and it saves only the user’s Outlook data and settings
-  **Session Office 365:** for server OS and it saves only the user’s Outlook data and settings

Find more information on user layers [here](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html).

### Microsoft Azure Virtual Desktop (AVD) in Azure

If maximizing Microsoft entitlements is your goal, Citrix extends the Microsoft Azure Virtual Desktop platform with enhancements targeted at experience, security, and choice. Azure Virtual Desktop (AVD) includes new multi-user Windows 10 capabilities. End-users get a familiar Windows 10 experience, and IT can still take advantage of the scalability of a multi-user operating system (OS). Citrix administrators can use Azure Virtual Desktop while using Citrix’s robust management tools, optimized HDX, and networking capabilities. The long-standing Citrix partnership with Microsoft affords cost-effective ways for customers to shift capacity into the cloud. Technologies like Citrix Autoscale enable cost savings by balancing workloads between fixed-capacity data centers and consumption-based flexible clouds. Autoscale is designed to automate management of virtual machines in a Citrix environment to reduce cost. You can find more information on Autoscale [here](/en-us/tech-zone/learn/tech-briefs/autoscale.html).

### Agility for Business Continuity and Disaster Recovery

By using the Citrix Cloud control plane, administrators are able to rapidly scale and use existing images to extend to cloud hosted workloads. This can be done without having to invest in more infrastructure.

Preparation for business interruptions – whether planned or unplanned – often means that end users must work remotely for extended periods of time. With this in mind, business continuity plans need to be designed to scale quickly, to have little impact on the end-user experience, and to fit within the company’s security requirements. With Citrix Virtual Apps and Desktop Service, you can use the control plane and burst more users into a hybrid cloud without having to stand up additional infrastructure. More information on how Citrix can help you with your Business Continuity plans can be found [here](/en-us/tech-zone/learn/tech-briefs/business-continuity.html).

Citrix Cloud architecture is designed to handle multiple in-service failures during a business interruption. By using multiple regions and data centers, Citrix Cloud is “always ready” when availability is threatened by an unplanned event.

### Proactively Monitor your Environment

Citrix Analytics, an intuitive analytics service, is a component of Citrix Workspace. Citrix Analytics provides two kinds of functionality – performance analytics and security analytics. Machine learning and advanced algorithms enable this solution to provide actionable insights. Citrix Analytics collects indicators across users, endpoints, network traffic, and files. Both security and performance analytics are managed from a single console.

Security Analytics collects data from different Citrix products and generates actionable insights to protect your environment from malicious or compromised users. It continuously assesses the behavior of your end users and finds deviations from normal behaviors. Security Analytics ensures that your corporate data is always protected.

Performance Analytics enables you to track, aggregate, and see key performance indicators collected from end user sessions. By having these insights, you can proactively tweak your environment to make sure end-users have the best possible user experience (UX). More information about analytics can be found [here](/en-us/tech-zone/learn/tech-briefs/analytics.html).

### Simplifying Your Deployment

In a typical on-premises Virtual Apps and Desktop deployment, administrators find themselves managing multiple networking devices, configuring firewall rules, and juggling public IP addresses, all while replicating configurations across multiple sites. In many organizations, the Citrix team does not manage networking. This results in added complexity and the need for collaboration and cooperation between teams to deploy and track changes.

Migrating to Citrix Virtual Apps and Desktop service with Citrix Gateway service enables IT to deliver the same experience to all users. Regardless of whether they are employees, partners, third party suppliers, or contractors. The need for public IP addresses and SSL certificates is alleviated. So is the need for firewall rule changes.

There are no networking appliances to manage, and no need to worry about installs or upgrades. Overall, rolling out the initial environment in addition to maintaining it is simpler and less prone to error. Below is an illustration of certain aspects of the administration that get removed by offloading Citrix Gateway to Citrix Cloud.

[![Citrix Virtual Apps and Desktops service Deployment](/en-us/tech-zone/learn/media/tech-briefs_cvads_deployment.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_deployment.png)

In addition, migrating your environment to CVADs ensures that the Citrix technology in the environment is always up to date. As a result, IT admins focus on your business needs, but your users can still capitalize on the latest features and functionality. Citrix Cloud deploys new features, security enhancements, and performance optimizations on a continuous basis.

Below is a diagram of what is managed by Citrix and what is managed by the customer in a typical Citrix service deployment.

[![Citrix Virtual Apps and Desktops service Cloud](/en-us/tech-zone/learn/media/tech-briefs_cvads_cloud.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_cloud.png)

By hosting Virtual Delivery Agents (VDAs) in resource locations managed by the customer, the amount of data and information that is accessible to the Citrix Cloud control plane is limited. The only thing that the control plane has access to is metadata (machine names, application names, application shortcuts, and so forth). This keeps intellectual property secure. All communication between the customer’s resource location and the Citrix Cloud control plane is encrypted, using HTTPS port 443 through the TCP protocol. All connections are outbound. No inbound connections are accepted. Citrix only collects the necessary logs for troubleshooting any potential issues.

**Components Managed by Citrix:**
All components that Citrix manages are kept highly available.

-  **Studio:** management console that is used to configure your environment
-  **Director:** monitoring tool that enables IT support and help desk personnel to troubleshoot issues
-  **License Server:** component that manages licenses for the environment and provides usage statistics
-  **Workspace Configuration:** the collection of settings that allows you to create configurations and customizations for your users’ workspace
-  **Delivery Controllers:** communicates through the Cloud connectors to load balance applications and desktops, authenticate users, and broker or prioritize connections
-  **SQL:** Server database used to store the data from the controllers
-  **Cloud Connectors:** communication channel between Citrix Cloud and the customer’s resource location. These are hosted in each of the customer’s resource locations but are pushed scheduled updates from the Citrix Cloud services. *(the only parts handled by the customer are Cloud Connector Windows updates and patching)*

**Components Managed by Customer:**

-  **Virtual Delivery Agents:** VDAs register with the Cloud connectors to broker connections and provide users with the resources they need to access. CVADs can use either the current release (CR) VDA or the long-term service release (LTSR) VDA (7.15 or higher).
    -  CR VDAs reach end of maintenance six months after they are released and end of life 18 months after they have been released. CRs are not eligible for extended support programs.
    -  LTSR VDAs reach end of life five years after they are released. The end of extended support is 10 years after they have been released.
    -  The full lifecycle matrix can be found [here](https://www.citrix.com/support/product-lifecycle/product-matrix.html).
    -  An FAQ on LTSR can be found [here](https://support.citrix.com/article/CTX205549).
-  **Active Directory:** used for authentication and authorization, Active Directory authenticates users and ensures that they are getting access to appropriate resources. A subscriber’s identity defines the services to which they have access in Citrix Cloud. This identity comes from Active Directory domain accounts provided from the domains within the resource location.
-  **Identity Provider:** the final authority for the user’s identity. The following identity providers are supported: on-premises Active Directory, Active Directory plus token, Azure Active Directory, Citrix Gateway, and Okta. An in-depth look at how Citrix Workspace handles identity and authentication can be found [here](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html)
-  **App and Desktop Workloads:** The app and desktop instances published by Citrix customers can be on-prem, hosted in public clouds, or in a hybrid mixture of both. Citrix provides many tools to simplify and facilitate how these session hosts are built and maintained. By allowing customers to maintain their session hosts, intellectual property is protected.

**Components that can be managed either by Citrix or by Customer:**

-  **Citrix Gateway:** Used so external users can connect to internal resources. Citrix Gateway can either be deployed and managed within the customer’s resource location or deployed and managed in Citrix Cloud.
-  **StoreFront:** Used as the web interface for access to applications and desktops. StoreFront is an optional component that can be installed within a customer’s data center, or the cloud-hosted Workspace can be used for more functionality.

For more information on why you would host these components on-premises vs cloud look at [Conceptual Architecture and Process Flow](#conceptual-architecture-and-process-flow) section.

### Expand your Workspace

Citrix Cloud allows you to easily extend beyond Virtual Apps and Desktops to provide your end users with a complete workspace. Other Citrix Cloud services allow you to deploy, create, and manage workspaces with apps and data from a unified location. A description of these Citrix Cloud services can be found below:

-  **Microapps:** allows users to interact with content and complete workflows without context switching or launching the full application. Microapps presents a single use case from a business application
-  **Virtual Apps and Desktop service:** delivers Windows, Linux, web, and SaaS applications or full virtual desktops
-  **Endpoint Management:** manages endpoints through mobile device management (MDM) or mobile application management (MAM)
-  **Secure Browser:** isolates web browsing to protect your corporate network from browser-based attacks
-  **Gateway:** allows secure, contextual access to apps and data
-  **Analytics:** collects actionable insights in order to proactively handle security threats, improve app performance, and support operations of your environment
-  **Application Delivery Management:** manages Citrix networking deployments on-premises or in the cloud from a single centralized console
-  **Content Collaboration:** enables any device to be used to securely access files, share data and create time-saving workflows -- from the cloud or on-premises storage

The goal is to provide end-users with a unified place to get work done. This workspace follows them wherever they go so they can continue working even as they are switching endpoints or locations. Below is a picture of what a user’s workspace looks like.

[![Citrix Virtual Apps and Desktops Workspace](/en-us/tech-zone/learn/media/tech-briefs_cvads_workspace.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_workspace.png)

As you can see, Citrix Workspace is a unified, secure, and intelligent work platform that transforms the employee experience by organizing, guiding, and automating all activities people need to do their best work. With Citrix Workspace, employees are more productive and engaged, while IT receives more visibility and control for simplified management, security, and compliance.

All of the services needed to power are controlled by a unified admin console. This unified admin console provides administrators with an easier way to manage multiple Citrix services. It also allows them to gain valuable insight across the entire Citrix environment. Below is a picture of the administrative console used to manage Citrix Cloud services.

[![Citrix Virtual Apps and Desktops Console](/en-us/tech-zone/learn/media/tech-briefs_cvads_console.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_console.png)

## Conceptual Architecture and Process Flow

[![Citrix Virtual Apps and Desktops Architecture](/en-us/tech-zone/learn/media/tech-briefs_cvads_conceptual-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_conceptual-architecture.png)

As the diagram shows, Citrix Virtual Apps and Desktop service gives IT professionals the design flexibility they need to choose whether certain components are hosted and managed within Citrix Cloud or deployed by IT.

Using a cloud-hosted workspace and gateway service simplifies your deployment and the components that you need to manage. If you need advanced authentication methods, you can utilize the cloud hosted workspace with an on-premises gateway. If all authentication in your organization’s environment must occur on-premises, then on-premises StoreFront and Gateway are recommended for your organization’s deployment.

Each organization is different, so we’ve provided different options and associated process flows below for consideration.

### Cloud Hosted Workspace and Gateway Service

In a Cloud Hosted Workspace and Gateway service scenario, Citrix does the heavy lifting for you. Your team does not need to expend effort on deployment. Citrix keeps the environment evergreen.

The Gateway service must be used with Citrix Workspace. By deploying both the Workspace and Gateway service, the need to deploy Citrix Gateway appliance in the DMZ is alleviated and complexity is reduced. This scenario negates the need for public IP addresses, firewall changes, and networking devices. Resulting in fewer components to manage.

Also, Citrix Gateway service is a highly resilient solution. Multiple instances of Gateway service are deployed in different geographic locations. Citrix Gateway service allows users to be redirected to the nearest POP in case of a failure. Optimal Gateway Routing ensures that users are always connected to the closest POP.

Below you can find the associated process flow with this type of configuration.

[![Citrix Virtual Apps and Desktops Cloud](/en-us/tech-zone/learn/media/tech-briefs_cvads_workspace-gwservice.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_workspace-gwservice.png)

### Cloud Hosted Workspace with on-premises Gateway

Customers can take advantage of their current investment if they already have gateway deployed on-premises. Customers who are using the Gateway on-premises for use cases like full VPN or micro VPN and would like to continue using it for virtual apps and desktops are able to do so. On-premises Gateway allows for advanced authentication options as well such as multifactor authentication (MFA) with RADIUS.

Some use cases require customers to maintain Gateway on-premises, for example, customers with highly secure environments that require endpoint analysis (EPA). EPA scans end-user devices for certain security requirements. With this information, it determines if they are allowed to enter the environment and what policies must be applied.

Below you can find the associated process flow with this type of configuration.

[![Citrix Virtual Apps and Desktops Cloud](/en-us/tech-zone/learn/media/tech-briefs_cvads_onpremgwpng.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_onpremgwpng.png)

### On-premises StoreFront and Gateway

For organizations that want to retain control of authentication on-premises, a great option is StoreFront and Gateway deployed on-premises. In this scenario, end-users aren’t entering their passwords into a cloud service. Also, certain features such as Local Host Cache require StoreFront to be located on-premises. Local Host Cache allows users to continue working even when the Cloud Connectors have lost communication with Citrix Cloud.

If customers require more advanced authentication settings like federated authentication, they would require an on-premises StoreFront. Federated Authentication services (FAS) dynamically issue certificates for users, enabling users to log in as if they had smart cards. FAS allows StoreFront to use a broad range of authentication protocols like SAML.

Below you can find the associated process flow with this type of configuration.

[![Citrix Virtual Apps and Desktops Cloud](/en-us/tech-zone/learn/media/tech-briefs_cvads_storefront.png)](/en-us/tech-zone/learn/media/tech-briefs_cvads_storefront.png)

## Delivery Models

Virtual Apps and Desktop service can be used to deliver different types of desktop models. Here is a glossary of those models:

-  **Windows Apps:** a Windows app interface running a server based (multi-user) OS, accessible to many users.
-  **Linux Apps:** a Linux app interface running on a server-based OS, accessible to many users.
-  **Secure Browser:** an app, encapsulated within a compatible browser tab, delivered to the user’s preferred browser.
-  **VM-Hosted App:** a Windows app interface, running on a desktop-based (single-user) OS, accessible to a single user.
-  **Shared Windows Desktop:** a Windows desktop interface, running on a server-based OS, accessible to many users.
-  **Shared Linux Desktop:** a Linux desktop interface running on a sever-based OS, accessible to many users.
-  **Pooled Windows Desktop:** a randomly assigned desktop-based Windows OS, accessible to a single user.
-  **Pooled Linux Desktop:** a randomly assigned desktop-based Linux OS, accessible to a single user
-  **Personal Windows Desktop:** a statically assigned desktop-based Windows OS. (accessible to a single user)
-  **Personal Linux Desktop:** a statically assigned desktop-based Linux OS (accessible to a single user)
-  **Pro Graphics Desktop:** a virtual desktop using a hardware-based graphical processing unit (GPU), accessible to a single user
-  **Remote PC Access:** a traditional Windows PC available to a remote user
-  **Local VM:** a desktop running within a virtual container on the end point device

Take an in-depth look at evaluating VDI Models [here](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html).

## PoC Guides

There are multiple Proof of Concept (PoC) guides available that can help you get started with Citrix Virtual Apps and Desktops service:

-  [Getting Started with Citrix Virtual Apps and Desktop service](/en-us/tech-zone/learn/poc-guides/cvads.html)
-  [Citrix Virtual Apps and Desktops with Azure Virtual Desktop Hybrid](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html)
-  [Remote PC Access with Citrix Virtual Desktop service](/en-us/tech-zone/learn/poc-guides/remote-pc-access.html)
