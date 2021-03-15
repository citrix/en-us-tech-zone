---
layout: doc
h3InToc: true
contributedBy: Rick Dehlinger, Jeff Allen, Rich Meesters, Anthony Zepeda
specialThanksTo: JP Alfaro, Stefano Castelli, Gavin Connolly, Kishore Kunisetty, Dan Lazar, John Moody, Mads Petersen, Michael Shuster
description: Learn the architecture and deployment considerations for Citrix solutions on Google Cloud Platform.
---
# Citrix virtualization on Google Cloud

## Introduction

In this guide, we walk you through designing a Citrix virtualization system on GCP. As the journey progresses, we discuss the implications of the decisions you need to make, and curating more reference resources along the way. This guide is a living document. Be sure to bookmark it and check back periodically to see how things change over time.

We start by reviewing the common [design patterns](#design-patterns-for-citrix-virtualization-on-google-cloud) for Citrix virtualization technologies on Google Cloud. Some think of these 'design patterns' as ‘reference architectures', but when we're working with infrastructure as code and cloud services, ‘design patterns' make a lot more sense.

Next we explore the [Solution Components and Requirements](#solution-components-and-requirements). We lay out the solution prerequisites and give you an overview of what services/components are required to create a Citrix Cloud ‘[resource location](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)'.

We then revisit and dig more [deeply into the design patterns](#design-patterns---going-deeper), armed with a greater understanding of the services/components of a Citrix virtualization system on Google Cloud. Finally, we dive deeper into specific topic areas, including [Virtual Delivery Agent (VDA) Design and Management](#vda-design-and-management-considerations), [Citrix ADC/Gateway on Google Cloud](#citrix-adcgateway-vpx-on-google-cloud), and [Citrix StoreFront on Google Cloud](#citrix-storefront-on-google-cloud).

As you take this journey with us, we encourage you to share your comments, contributions, questions, suggestions, and feedback along the way. Drop us a line via email to [the Citrix on Google SME working group](mailto:caceb231.citrix.onmicrosoft.com@amer.teams.ms).

## Design Patterns for Citrix virtualization on Google Cloud

We recognize that different customers are at different stages on their journey to "the cloud". As such, we outline three design patterns that represent a spectrum from "we are all in" to "we will get there but it can take us a while". Observant technologists see the common elements between all three. They start to see how they can mix and match customer managed and cloud services to meet different business needs and environmental influences. We explore this modularity of subsystems when we revisit these [three design patterns later](#design-patterns---going-deeper) in this guide.

### The Cloud Forward Design Pattern

Organizations of all shapes and sizes are making the move to "the cloud" and subscription based managed services. For customers who are all in on "the cloud" (or interested in experiencing the best of what the cloud has to offer), the Cloud Forward design pattern is a great match. The Cloud Forward design pattern:

-  Uses state of the art, cloud-delivered services from Citrix and Google.
-  Is commonly used for new deployments, in addition to technology evaluation, proofing, training, and other use cases that value simplicity, flexibility, and speed of deployment.
-  Requires no existing infrastructure or licenses, and can be built in less than 30 minutes using Google Deployment Manager templates such as the [Citrix on GCP GitHub project](https://github.com/GoogleCloudPlatform/citrix-on-gcp).
-  Provides high availability of all critical services.
-  Creates a Citrix Cloud "[resource location](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)" - the foundation of the other two patterns outlined here.

All you need to get started is a GCP Project and access to a Citrix [Cloud Virtual Apps and Desktops service](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/) (CVADS) subscription. Evaluation subscriptions to Citrix Cloud are available through Citrix and Citrix authorized resellers. Google also offers new customers a [free trial](https://cloud.google.com/free) which includes $300 of Google Cloud credit.

> **Note:**
>
>The GCP free trial does not include the use of Windows Server images, as noted in the [Google Cloud Free Tier document](https://cloud.google.com/free/docs/gcp-free-tier#free-trial). To use Windows Server images you must upgrade to a paid account in GCP. Your free credits still apply when you upgrade to a paid account as noted in the [Upgrading to a paid account section](https://cloud.google.com/free/docs/gcp-free-tier#how-to-upgrade) Google Cloud Free Tier document.

![Cloud Forward Design Pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_cloud-forward-design-pattern.png)

This design pattern uses more than one of all key resources (➊) deployed in separate zones in a given Google Cloud region for high availability.

Citrix Cloud Connectors (❷) are responsible for communications to and from Citrix Cloud (❻), using outbound TLS connections to Citrix Cloud services over the Internet. Once installed on domain-joined Windows Server VM instances, the Cloud Connector software is automatically updated and maintained by Citrix Cloud.

Apps and desktops are provided by Windows or Linux VM instances, or both running Citrix's Virtual Delivery Agent (VDA) software (❸). The Citrix VDA software uses Citrix's sophisticated [HDX technology](https://www.citrix.com/glossary/what-is-hdx.html) to provide the best possible user experience with virtualized applications and desktops. VDAs register with Citrix Cloud Connectors, which are responsible for brokering HDX session connections to the VDAs. HDX sessions are proxied into the VPC through the Cloud Connectors by default, or optionally through the Citrix Gateway Service by configuring the 'rendezvous' policy. VM instances can be optionally backed by [Google Cloud GPUs](https://cloud.google.com/gpu) to create virtual workstations, in turn accelerating graphics intensive applications.

Citrix VDAs are most commonly deployed close to the infrastructure required by the applications being delivered (❹). As such, the application infrastructure is typically deployed or migrated into the same region as the Citrix VDAs.

End-users use the [Citrix Workspace app](https://www.citrix.com/downloads/workspace-app/) (❺) (CWA) to connect to and interact with virtualized applications and desktops using Citrix's innovative [HDX session remoting protocol](https://www.citrix.com/glossary/what-is-hdx.html). The CWA is available for almost any device or operating system, including Chrome OS, Windows, OSX, iOS, Android, and Linux.

This pattern uses the following cloud services (❻) from Citrix, which are secure and highly available by design:

-  [Citrix Virtual Apps and Desktop Service:](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/) provides session brokering, load management, single instance image management, monitoring, and cost/capacity management services.
-  [Citrix Workspace service:](https://www.citrix.com/products/citrix-workspace/) the user interface of the solution. This web service provides multifactor authentication, content presentation, and launching services for the Citrix Workspace app. This service consolidates access to virtualized applications and desktops, web, and SaaS applications, in addition to Enterprise file stores.
-  [Citrix Gateway Service:](https://www.citrix.com/products/citrix-gateway/) provides secure access to virtualized applications and desktops, in addition to Enterprise web applications, to devices on public networks.
-  [Citrix Analytics Service:](https://www.citrix.com/products/citrix-analytics-security/) uses advanced machine learning technologies to provide enterprise-grade security, performance, and user behavioral analytics and reporting.

This design pattern can also be paired with a Google Cloud VPN/Interconnect, or a purpose built solution like Citrix SD-WAN (❼) to extend existing Active Directory investments (❽) into Google Cloud or to provide access to traditional, on-premises, customer managed applications and infrastructure.

### The Hybrid Design Pattern

The Hybrid design pattern builds upon the Cloud Forward design pattern. It introduces customer-managed access layer components from Citrix (➊) to flexibly meet the needs of specific customer demographics and use cases. These customer-managed components include the following:

-  [Citrix ADC/Gateway](https://www.citrix.com/networking/)(❷): deployed as virtual appliances on GCP, this component is often used for use cases requiring one or more of the following:
    -  Advanced authentication scenarios, such as SAML/OAUTH 2/OpenID federation, RADIUS, smart card, and conditional access requirements.
    -  Highly optimized and flexible session access for end user devices on public networks.
    -  Advanced networking services such as content switching, web app firewall, integrated web caching, attack mitigation, application load balancing, and SSL offload.
    -  Ability to direct specific users/devices to specific ‘stores' based on advanced, highly flexible, and contextually aware policies. Policy decisions can be based on user profile attributes, location, device type, device health, authentication results, and more.
-  [Citrix StoreFront](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/citrix-storefront.html)(❸): The predecessor of the Citrix Workspace service, StoreFront is Citrix's ‘classic' provider of UI services. Installed on customer-managed Windows Server instances, StoreFront is often used for use cases requiring one or more of the following:
    -  Extreme high availability, capable of surviving a broader range of failure scenarios, particularly when deployed in a highly available configuration.
    -  Flexible session routing, with the ability to route internal user session traffic directly to VDAs while sending external users through Citrix Gateways.
    -  Single sign-on from customer-managed, on-premises devices.
    -  The need to provide multiple ‘stores' with different configuration properties to support diverse use cases on the same system.
    -  The need for highly customized or branded, HTML based user interfaces.

![hybrid-design-pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_hybrid-design-pattern.png)

With the Hybrid design pattern, Citrix access layer components are deployed in the customer's Google Cloud environment (➊). The components are typically deployed in pairs spread across multiple zones for high availability.

This pattern uses Citrix's ADC/Gateway VPX (virtual) appliances to securely proxy HDX sessions into the VDAs in the customer's environment (❷). Citrix ADC/Gateway appliances can be used with the Citrix Workspace service for simple session proxy services or complex authentication scenarios, or both (UI option A). It can also be paired with Citrix StoreFront (UI option B).

This pattern optionally uses Citrix StoreFront (❸) for UI services, allowing the system to meet the requirements for more complex use cases as outlined above. It pairs with Citrix ADC/Gateway, which handles authentication in addition to UI and HDX session proxy services.

### The Cloud Migration Design Pattern

The Cloud Migration design pattern further builds upon the Hybrid design pattern. It allows customers with existing investments in Citrix technologies to systematically modernize their infrastructure, while seamlessly migrating workloads to the cloud. This migration can be done at a pace that doesn't disrupt existing/critical systems and use cases. It allows technologically conservative customers to ‘wade' into the Cloud, workload by workload, mitigating risk along the way on the customer's terms. It also allows the customer to systematically reskill staff on the latest, most capable technologies from Citrix and Google, and build their organizational cloud readiness at a manageable pace while using their existing investments in technology, infrastructure, knowledge, processes, and operationalization procedures.

The cloud migration design pattern starts with the hybrid pattern described in the preceding section. The system built with the hybrid pattern becomes the new, state of the art environment where new workloads are deployed. The cloud migration pattern uses Citrix StoreFront(➊) or the Citrix Workspace (❷) user interface, or both to connect legacy Citrix environments (❸) to the new environment, as both UIs can present multiple brokering environments in a single view with a single login. This provides users with a single UI (❹) they can use to access both environments. This allows the customer to deploy new workloads onto Google Cloud (❺), while systematically migrating existing workloads to Google Cloud as the businesses opportunities and constraints dictate.

![cloud-migration-design-pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_cloud-migration-design-pattern.png)

Now that you've reviewed the design patterns, let's peel back the layers a bit. Let's explore what it takes to build a Citrix virtualization system on Google Cloud.

## Solution Components and Requirements

### Virtualization System Prerequisites

To build a Citrix virtualization system on Google Cloud, you need a minimum of two things to get started:

-  A Google Cloud Project
-  A Citrix Virtual Apps and Desktops Service subscription

> **Note:**
>
> The GCP free trial does not include the use of Windows Server images, as noted in the [Google Cloud Free Tier document](https://cloud.google.com/free/docs/gcp-free-tier#free-trial). To use Windows Server images, you must upgrade to a paid account in GCP. Your free credits still apply when upgrading to a paid account as noted in the [Upgrading to a paid account section](https://cloud.google.com/free/docs/gcp-free-tier#how-to-upgrade) within the Google Cloud Free Tier document.

Got your pre-requisites lined up? Good! Now let's introduce you to what you're building.

> **Tip:**
>
> The Citrix Virtual Apps and Desktops Service documentation is relevant as this service is at the core of the solution. Be sure to give it a read before you start building, and keep it handy for when you need more information. It can be found on [the Citrix docs site](/en-us/citrix-virtual-apps-desktops-service.html).

### The Citrix Cloud Resource Location

The foundation of a Citrix virtualization system on Google Cloud is a Citrix Cloud construct called a "resource location". A resource location, in Citrix speak, is roughly analogous to a region on GCP. It's a well-connected grouping of resources, on a private network with an Active Directory, Internet egress (to utilize secure cloud services from Citrix and Google), and some applications and data you want to virtualize and securely deliver to any device in the world. Virtualized apps and desktops are delivered from VM instances on GCP called "VDAs". These are Windows or Linux VM instances running Citrix's VDA software which enables the desktop or application user interfaces to be remoted to client devices using Citrix's HDX display protocol. VDAs register with Cloud Connectors, which securely proxy communications with Citrix Cloud Services.

> **Tip:**
>
> One key rule of thumb for delivering virtualized applications to be aware of. You want to put the apps (running on the Citrix VDAs) near the data (infrastructure required for the apps). Not having apps and data local to each other can result in increased latency and slower applications, which can ultimately cause a degraded user experience.

Resource locations are typically built to be highly available, meaning your key services are spread across zones in the GCP region. Where applicable, more than one VM instance for key services are deployed for availability and manageability purposes. The key services you need to have in a resource location are:

-  *Microsoft Active Directory
-  *Citrix Cloud Connectors
-  *A method for reliable Internet egress, such as Cloud NAT
-  *Citrix VDAs (GCP VM instances with Citrix's VDA software installed underneath the applications being delivered)
-  Other infrastructure, as needed, to support the applications being delivered

Let's further explore some of these services* in more detail as they're required to have a functional Citrix Cloud resource location on Google Cloud.

### Microsoft Active Directory

All design patterns for Citrix virtualization systems on Google Cloud require Microsoft Active Directory. For a compelling user experience, Active Directory services ought to be available in every GCP region where you've got VDAs deployed, hence it's considered a core component of a Citrix Cloud resource location. AD is used for configuration management (group policy) in addition to authentication, though as we learn later, AD does not need to be the identity provider for the system. Many customers extend their existing AD into Google Cloud, though Citrix virtualization can flexibly integrate into various AD designs and servicing models.

When deploying Active Directory on Google Cloud, customers can build/maintain their own Active Directory Domain Controllers using Windows Server instances, use Google's [Managed Service for Microsoft Active Directory](https://cloud.google.com/managed-microsoft-ad), or a combination of the two. Active Directory trusts can also be used to connect two or more AD forests/domains depending upon the customer's needs.

For customers looking to minimize the administrative overhead required to build and maintain functional Active Directory services, the Google [Managed Service for Microsoft Active Directory](https://cloud.google.com/managed-microsoft-ad) (Managed Microsoft AD for short) is an option worth considering. This service provides you with a fully functional Active Directory forest/domain without the overhead of building and maintaining Windows Server VM instances. The Managed Microsoft AD service is built on highly available, Google-managed infrastructure, and delivered as a managed service. Each directory is deployed across multiple GCP zones, and monitoring automatically detects and replaces domain controllers that fail. You do not have to install software, and Google handles all patching and software updates. With Google's Managed Service for Microsoft Active Directory, you can use native Microsoft administrative tools, manage Windows machines and users with Microsoft Group Policy. You can also join VM instances to it, and even setup Active Directory trusts with existing AD instances to support various complex Enterprise scenarios.

Customers who choose to use Google's Managed AD Service with Citrix virtualization technologies can expect these technologies to work, with a few important things to consider before doing so. For starters - you won't have Domain Administrator, Enterprise Administrator, or other 'super user' type access to the AD instance. You do, however, have full control of your own container at the root of the directory where you can create users, computers, groups, OU's, and group policies. You can also set up and use various types of trust relationships with other directories.

A few other things you CAN NOT do:

-  Create AD objects in any of the default containers (such as /Computers): they're read-only. This brings up a common mistake some customers make when using Citrix's Machine Creation Services (MCS) provisioning technology: you must choose to create the machine accounts for your MCS managed VDAs in a container/OU that's writeable. If you don't choose such a location, MCS is not able to create the machine accounts.
-  Install and configure some AD integrated features such as Certificate Services. As such, this impacts customers who plan to use Citrix's Federated Authentication Services (FAS) technology, which requires AD integrated Certificate Services. These customers must build and manage their own Active Directory on Google Cloud using Windows Server VM instances.
-  Have local Server Administrator equivalence by default. In an 'out of the box' Active Directory installation, the Domain Administrators group is added to the local Server Administrators group by default. If you're using the Google Managed Service for Microsoft AD, you may have to create your own server administrators' group, add your own users to it, create and apply a group policy to add your group to the built-in Server Administrators group on member servers/workstations.

While trust relationships, site/service configuration, replication, and other AD related topics are not covered here, Citrix has provided extensive documentation on these topics applicable to all three deployment models.

> **Note:**
>
> For more information on Active Directory requirements for Citrix virtualization on Google Cloud, see [Citrix Cloud Connector Technical Details](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html). Besides covering [supported Active Directory functional levels](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#supported-active-directory-functional-levels), this article also covers [deployments scenarios for Cloud Connectors in Active Directory](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory).

<!-- -->

> **Important:**
>
> DNS name resolution is important for a properly functioning system. DHCP on GCP always uses Google's name servers. For VM instances to ‘find' and join your Active Directory instance on GCP, you want to implement private managed DNS zones, though not necessary if using the Managed Microsoft AD service. See Google [Cloud DNS overview](https://cloud.google.com/dns/docs/overview) for more information.
>
> DNS name resolution is also important when implementing Citrix's Rendezvous feature for HDX session proxy, including usage of [EDT/Citrix adaptive transport](/en-us/citrix-virtual-apps-desktops-service/hdx/adaptive-transport.html). See [Rendezvous protocol](/en-us/citrix-virtual-apps-desktops-service/hdx/rendezvous-protocol.html) documentation for more details.

### Citrix Cloud Connectors

Citrix Cloud Connectors function as a secure, cloud managed proxy for the Citrix virtualization system. Cloud Connectors are dedicated, domain joined Windows Server instances in separate zones within a region. It also functions as an offline session broker if Internet access is interrupted ("local host cache mode') - useful for mission critical deployments with extreme availability requirements. We discuss this function in more detail as we get into the Hybrid design pattern later on in this document.

Cloud Connectors are typically deployed as an N+1 resource, using VM instances spread across multiple zones in a given region. This enables a resource location to scale and facilitates the automatic update of the Cloud Connector software.

> **Note:**
>
> For more information on instance sizing for Citrix Cloud Connectors, see [scale and size considerations for Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/cc-scale-and-size.html).

Cloud Connectors can sit on any VPC where they can reach Active Directory and the Citrix VDAs, and they need reliable Internet egress to function properly. One simple, highly available method for providing Internet egress is Google Cloud NAT, though many other methods are supported as well. For use cases with strict egress controls or auditing requirements, egress traffic from the Cloud Connectors to Citrix Cloud [can be proxied](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/proxy-firewall-configuration.html).

> **Note:**
>
> For more information on ports and protocols used by Citrix virtualization technologies, see [Communication Ports Used by Citrix Technologies](/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html). This document provides the foundation for the firewall rules you establish on Google Cloud.

### Citrix VDAs

The last resource type you need to have a functional Citrix virtualization system are called VDAs - and they're the actual workload you're delivering. As mentioned earlier, these are Windows or Linux VM instances running Citrix's "Virtual Delivery Agent" software, which enables the desktop or application user interfaces to be remoted to client devices using Citrix's HDX display protocol. VDAs can be created and managed outside of the system using any provisioning mechanism you'd like. For example, you can use Google Deployment Manager templates, but for any type of scale, you want to use Citrix's MCS technology.

MCS automates the creation of ‘machine catalogs' - groups of identically configured VDAs built from one or more ‘golden master' VM instances. MCS uses snapshots of the persistent disk to capture the state of the operating system and application stack installed. It also uses the instance attributes of your ‘golden master' VM instance to create instance templates, which apply these attributes to VDAs under MCS management. MCS also automates the updating of the system disk on the VDAs using similar snapshots of the ‘golden master' disk, and orchestrates the Autoscale feature's efforts to automatically manage cost and capacity.

There's a lot more to know about VDAs, but we dig deeper later on in this guide. Can't wait? Head straight to [VDA Design and Management Considerations](#vda-design-and-management-considerations).

> **Note:**
>
> See [Communication Ports Used by Citrix Technologies](/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html) to identify the firewall rules you establish on Google Cloud.

<!-- -->

> **Additional note:**
>
> VDAs do not have to have Internet egress - and for certain use cases they don't by design. If, however, the VDAs do have Internet egress, the "[rendezvous protocol](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/rendezvous-protocol.html)" feature can be [enabled via Citrix policy](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings.html#rendezvous-protocol), allowing client devices (which run the Citrix Workspace app) and the VDA to securely connect via the Citrix Gateway Service. This improves the scalability of the Cloud Connectors, who are often responsible for proxying the HDX connections into the VDAs. The other option for proxying HDX connections over public networks - deploy customer managed Citrix ADC/Gateway instances at the perimeter of the VPC.

## Design Patterns - Going Deeper

Earlier in this document we introduced three different yet related design patterns for Citrix virtualization on Google Cloud. These patterns build on top of each other, effectively serving more complex use cases along the way. We refer to them as:

-  The Cloud Forward Design Pattern
-  The Hybrid Design Pattern
-  The Cloud Migration Design Pattern

### The Cloud Forward Design Pattern

We mentioned that this is the foundational design pattern for Citrix virtualization on Google Cloud. It ought to be clear that the architecture of the cloud forward design pattern creates a Citrix Cloud resource location. This is the common foundation shared across all three patterns presented here. The differences between the patterns lie in which technologies are used to service the five components of a Citrix virtualization system outlined in the following table. With the cloud forward design pattern, cloud services are used for all five components:

| Session brokering and administration | Citrix Virtual App and Desktop Service \- (CVADS) \(cloud service\) |
|--------------------------------------|---------------------------------------------------------------------|
| User interface \(UI\) services       | Citrix Workspace service \(cloud service\)                          |
| Authentication                       | Citrix Workspace service, Active Directory as IdP                   |
| HDX session proxy                    | Citrix Gateway Service \(cloud service\)                            |
| Analytics                            | Citrix Analytics Service \(cloud service\)                          |

![Cloud Forward Design Pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_cloud-forward-design-pattern.png)

The cloud forward design pattern can be replicated, using the same Active Directory (or not) in different Google Cloud regions. This makes the pattern useful for deployments with geographically distributed applications and data. This pattern, for production deployments, is often extended by connecting the resource location in GCP to customer managed data centers using [Google Cloud VPN](https://cloud.google.com/network-connectivity/docs/vpn/concepts/overview), [Google Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect/concepts/overview), or a purpose built product like [Citrix's SD-WAN](https://www.citrix.com/products/citrix-sd-wan/). Such a private network connection allows you to extend key services (such as Microsoft Active Directory) up into Google Cloud. This can provide VDAs with access to applications and resources that have not yet been migrated. It can also act as a conduit to migrate apps and data up into Google Cloud. While not shown in the preceding diagram, the [Citrix Workspace Environment Management service](/en-us/workspace-environment-management/service.html) is commonly used, especially as systems make their way to production. The Workspace Environment Management service uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops deployments. See [User Environment/Settings Management](#user-environmentsettings-management) later in this guide for more details.

### The Hybrid Design Pattern

Earlier we also introduced the hybrid design pattern. A variant of cloud forward pattern, the hybrid design pattern introduces customer managed implementations of Enterprise proven Citrix access layer technologies to provide UI services, authentication, and HDX proxy functions. These options allow the virtualization system to service more complex use cases, many of which are common in Enterprises who are just beginning their journey to the cloud.

![the-hybrid-design-pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_the-hybrid-design-pattern.png)

To put the hybrid design pattern into the context of the five components of a Citrix virtualization system:

| Virtualization system function:      | Provided by:                                                                                                                                          |
|--------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| Session brokering and administration | Citrix Virtual App and Desktop Service (CVADS) \(cloud service\)                                                                                     |
| User interface \(UI\) services       | Citrix Workspace service \(cloud service\) OR Citrix StoreFront \(customer managed\)                                                                  |
| Authentication                       | Many combinations available to Citrix Workspace service \(cloud service\) OR Citrix StoreFront by introducing Citrix ADC/Gateway \(customer managed\) |
| HDX session proxy                    | Citrix Gateway Service \(cloud service\) OR Citrix ADC/Gateway \(customer managed\)                                                                   |
| Analytics                            | Citrix Analytics Service \(cloud service\)                                                                                                            |

Earlier we gave you the high points of when the introduction of Citrix ADC/Gateway and Citrix StoreFront is useful to introduce into the Citrix virtualization system on Google Cloud:

-  [Citrix ADC/Gateway](https://www.citrix.com/networking/)(❷): deployed as virtual appliances on GCP, this component is often used for use cases requiring one or more of the following:
    -  Advanced authentication scenarios, such as SAML/OAUTH 2/OpenID federation, RADIUS, smart card, and conditional access requirements.
    -  Highly optimized and flexible session access for end user devices on public networks.
    -  Advanced networking services such as content switching, web app firewall, integrated web caching, attack mitigation, application load balancing, and SSL offload.
    -  Ability to direct specific users/devices to specific ‘stores' based on advanced, highly flexible, and contextually aware policies. Policy decisions can be based on user profile attributes, location, device type, device health, authentication results, and more.
-  [Citrix StoreFront](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/citrix-storefront.html)(❸): The predecessor of the Citrix Workspace service, StoreFront is Citrix's ‘classic' provider of UI services. Installed on customer-managed Windows Server instances, StoreFront is often used for use cases requiring one or more of the following:
    -  Extreme high availability, capable of surviving Internet and cloud service outages.
    -  Flexible session routing, with the ability to route internal user session traffic directly to VDAs while sending external users through Citrix Gateways.
    -  Single sign-on from customer-managed, on-premises devices.
    -  The need to provide multiple ‘stores' with different configuration properties to support diverse use cases on the same system.
    -  The need for highly customized or branded, HTML based user interfaces.

These are the high points - there are many other functional items you may also find important to consider before choosing between the cloud service or customer managed components. We provide you with a deeper dive into Citrix ADC/Gateway and Citrix StoreFront on GCP in later sections. You can use different combinations of technologies at each layer to achieve specific outcomes or meet specific needs - at the expense of simplicity.

For example: Citrix ADC/Gateway VPX appliances can be added to a system and used for Authentication or HDX proxy functionality while using Citrix Workspace for UI services. This gives the system the ability to support almost any identity and authentication strategy (including federation scenarios), plus the ability to use HDX's [Enlightened Data Transport](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/adaptive-transport.html) for the best session performance over suboptimal networks.

You can also introduce Citrix StoreFront to use for UI services, in parallel to or instead of Citrix Workspace. StoreFront requires Citrix ADC/Gateway for most use cases, but this combination would serve use cases with extreme high availability requirements, heavy UI customization requirements, and the ability to create multiple different ‘stores', with different properties, for different groups of users, device properties, physical locations, and so on.

### The Cloud Migration Design Pattern

The cloud migration design pattern is designed to allow a customer to run a legacy Citrix virtualization system and Citrix Cloud on GCP in parallel. Both options for providing UI services (Citrix Workspace service and Citrix StoreFront) can aggregate resources from the legacy environment and Citrix Cloud into a single user interface. This provides the user with a consistent access experience regardless of where the resources are launched from.

One common example: an existing Citrix customer, with a substantial investment in a customer managed deployment of Citrix Virtual Apps and Desktops wants to begin running their app and desktop workloads on GCP. Perhaps they've also got multiple ‘Citrix farms' they currently manage on-premises. The customer has deployed Citrix StoreFront and most likely Citrix ADC/Gateway appliances to provide authentication, UI services, and HDX proxy services.

In this scenario, the customer is probably already using StoreFront's ability to aggregate apps and desktops from their multiple ‘Citrix farms' into one UI. To begin ‘moving in' to Google Cloud, they'd start by creating a Citrix Cloud resource location in a region of their choice. Assuming they're all on the same network, they can simply add the new ‘Citrix farm' to StoreFront and deploy their first virtualized workload on Google Cloud. This gives them the ability to run Citrix workloads in two environments, side by side - some on-premises, some on GCP - and migrate workloads to GCP as business priorities allow.

![cloud-migration-design-pattern](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_cloud-migration-design-pattern.png)

This same customer can also run Citrix Workspace side by side with StoreFront, and add the legacy ‘Citrix farms' to the Workspace UI using the Citrix Cloud [Site Aggregation](/en-us/citrix-workspace/add-on-premises-site.html) feature. Both UIs would provide access to the same resources for the same users. End-users can be gradually migrated to the Citrix Workspace service UI as business priorities allow.

![site-aggregation.png](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_site-aggregation.png)

The benefit of the Cloud Migration approach is that IT can migrate the app and desktop workloads from the legacy on-premises infrastructure to Google Cloud at a pace that suits them. Users can continue to access their applications and desktops in the same way, regardless of whether the workload is being delivered on-premises or from Google Cloud.

Using [Site Aggregation](/en-us/citrix-workspace/add-on-premises-site.html), customers are also able to use Citrix Analytics, giving them insights into security, performance, and operations of both their on-premises and cloud-hosted infrastructure. This can help in the decision-making process of when a workload ought to be moved from on-premises to Google Cloud Platform. Citrix Security Analytics can also be used to ensure that as workloads become distributed across on-premises infrastructure and Google Cloud, the customer's security posture can be enforced.

### Migration with Google VMware Engine

If you are considering the Cloud Migration design pattern, there's probably a good chance you're running Citrix on VMware today. For some customers, the option of extending their existing VMware based infrastructure to Google Cloud can be appealing. For these customers, this path promises to expedite workload migrations, using existing knowledge and process investments to get there sooner. With [Google Cloud VMware Engine](https://cloud.google.com/vmware-engine), customers can provision and run [VMware Cloud Foundation](https://www.vmware.com/products/cloud-foundation.html) (VCF) based Software-Defined data centers ([SDDCs](https://www.vmware.com/solutions/software-defined-datacenter/in-depth.html#compute)) on Google Cloud.

Citrix's Virtual App and Desktops service enables provisioning and image management of VDAs on VMware VCF-based public clouds. Before launch, Google Cloud VMware Engine underwent a comprehensive compatibility test to become [Citrix Ready Verified](https://www.citrix.com/blogs/2020/07/30/accelerate-your-move-to-cloud-with-citrix-and-google-cloud/). Both Citrix Provisioning platforms (MCS and PVS) were tested and functioned as expected. For more information, see [Google Cloud VMware Engine on Citrix Ready](https://citrixready.citrix.com/google-inc/google-cloud-vmware-engine.html).

When customers spin up a VCF based SDDC using Google VMware Engine, the SDDC (which includes compute, storage, and networking plus management) is peered to VPC networks on Google Compute Engine. This allows you to run workloads on the SDDC or Google Compute Engine, providing customers with options for various workloads. The following logical diagram depicts the relationship between Google Cloud, Citrix Cloud, and a managed SDDC instance:

![cloud](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_cloud.png)

> **Note:**
>
> Customers who have a firm requirement for full Citrix App Layering or PVS support ought to consider running their Citrix Cloud resource location on Google Cloud VMware Engine. Both Citrix App Layering and PVS are currently available on VMware-based platforms.

While a design and implementation of a Citrix virtualization solution on Google VMware Engine is outside the scope of this design guide, most of the concepts and components described in this guide still apply. For more information regarding setting up a Citrix Cloud resource location on VMware (Cloud Foundation), see [Citrix Virtual Apps and Desktops service documentation](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/vmware.html).

#### Summary

As you can see, these patterns provide maximum flexibility for existing Citrix customers, and allows them to journey to the cloud on their terms. There is no single prescriptive approach, instead, Citrix customers can design a solution and migration approach that best suits their business. To support this, customers who purchase Citrix Cloud Licensing also receive hybrid-use rights. Many customers also have a Citrix Customer Success Manager assigned to guide them through the process. Ask your Citrix Sales representative for more information.

Now that we've peeled back a couple layers in the context of the design patterns for Citrix virtualization on Google Cloud, let's dig more deeply into a few more topics.

## VDA Design and Management Considerations

The most dynamic part of a Citrix virtualization system is the VDA. Remember that VDAs are where the actual work is happening - the apps and desktops you provide users on a Citrix virtualization system run from VM instances on GCP. You want to make sure you get this layer right, but don't let perfection get in the way of progress! Do your homework up front. Set the expectation with users that the system will change over time. ...and build simple and effective processes to handle change: it's inevitable! With the power and flexibility of Citrix virtualization tech, managing change doesn't have to be a major burden.

In this section, we've attempted to logically break the topic up such that we can dive deep without losing context. We do our best to provide the details you need in each section and call out leading practices and recommendations along the way.

We start by examining the different VDA related options for delivering your mix of apps and desktops, and there are quite a few! We then dive into how to configure and use Citrix Cloud's VDA fleet and image management technologies, including MCS and the Autoscale feature. We then introduce user environment management (registry settings, drive/printer mappings, and so on) and user settings management (user profiles, personalization layers, home drives, and so on) options, dive into cost optimization and capacity management, and wrap up the section with more performance tuning considerations.

This is an ambitious amount of knowledge to distill - let's get after it!

### Workload Delivery Options and Considerations

As you begin your workload delivery journey, it's important we start it on the right foot. That means touching on a couple non-VDA specific elements that need to be considered first. One of the most important conversations a good Citrix consultant has with a new customer is about the use cases you're going to be servicing. These conversations (more than one, because customer needs, business climate, and technology evolve over time) typically lead to the definition of reasonably well-defined groups of users and apps - we call them Delivery Groups. Most of the options we're going to break down in this section are re-evaluated for each Delivery Group and use case. It's common for customers to have quite a bit of variation and even overlap between Delivery Groups. At the end of the day, however, the most foundational element of each Delivery Group is the mix of applications, data, and services to be provided. Once you have that defined, you can begin to evaluate the decisions laid out in this section.

> **Important:**
>
> For each use case/Delivery Group, start by defining the mix of apps, data, and services needed, then work your way through the following considerations to decide what delivery options can best serve each.

<!-- -->

> **Tip:**
>
> VDAs are managed in a resource grouping called [machine catalogs](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html). Machine catalogs are groups of virtual machine instances which serve a common use case for a group of users. They're typically based off the same ‘golden master' VM instance template, and inherit VM instance properties and a generalized copy of the persistent disk.

#### VDA Operating Systems

##### Windows or Linux?

Now that you've defined the apps, data, and services required for each Delivery Group/use case, you can begin to consider what operating system is best suited for your VDAs. The most basic question: do you need Windows or Linux? This decision is often forced by the requirements of the application or set of applications you're delivering. If the app only runs on Windows, then Windows it is! The same obviously applies if the app only runs on Linux.

Business applications are often built upon Windows, so a large percentage of Citrix virtualized apps run on Windows based VM instances on GCP. Sometimes Windows is chosen because it's what the IT team knows, and the cost of spinning up operational competencies on a new OS like Linux is deemed too high, so Windows is used even if the application set can run on Linux. If, however, the app set can be run on Linux, it's worth considering - much of the complexity and a good portion of the costs (Windows OS and client licenses) can be avoided.

##### Server or desktop OS?

If you can use Linux as the OS, the choice of ‘server or desktop' is relatively simple. You must pick a version that has a GUI, can run [on Google Cloud](https://cloud.google.com/compute/docs/images), and is supported by the [Citrix Linux VDA](/en-us/linux-virtual-delivery-agent/current-release/system-requirements.html).

If you deploy Windows, the choice of server vs. desktop OS gets a bit more complicated. Both options share a common GUI, and both can present users with a virtual desktop. In fact, most Windows applications run on both Windows Server and Windows 10 desktop operating systems, though often application vendors won't call out Windows Server support in their documentation. The most major implication of Windows Server vs. Windows 10 desktop is licensing - and it's a large one.

Microsoft's licensing policies are restrictive when running Windows 10 (or any other ‘desktop' operating system) on public clouds. These policy-based restrictions can make it more expensive to run a Windows desktop OS on any public cloud, including GCP. For more information on Microsoft's licensing policies, consult your Microsoft licensing specialist, but the following gets you started on this complex topic:

-  [Microsoft Volume Licensing Product Terms (April 1, 2020)](https://www.microsoftvolumelicensing.com/Downloader.aspx?DocumentId=16407) - for Windows Client, Windows Server, and Windows Services.
-  [Microsoft Office Licensing page](https://www.microsoft.com/en-us/licensing/product-licensing/office) - get the [licensing brief](https://download.microsoft.com/download/3/D/4/3D42BDC2-6725-4B29-B75A-A5B04179958B/Licensing_Office365_ProPlus_in_Volume_Licensing.pdf).

If you're running Windows on GCP, Windows Server serves most use cases and application mixes, and you simply pay for the license usage along with instance usage. It's often more cost effective than a Windows desktop and ends up being the choice for many virtualization systems on Google Cloud.

#### Shared OS (multi-user) or single user ("VDI")?

One common misconception is that Windows Server cannot serve desktop use cases, regardless of whether you are sharing the OS between multiple users or have one OS/VM instance per user. This misconception is false! When deployed in Multi-user mode (that is, RDSH role is installed), Windows Server can present users with a ‘hosted shared' desktop. Windows Server can also be used for "VDI" use cases, and while not as cost effective or scalable as the multi-user/shared OS option, it's a legitimate option for a single user desktop. We call this delivery model "Server VDI".

To summarize, the following combinations of options/operating systems can be used depending upon the use case:

| Delivery Model | Single or multi\-user | Common OS versions/components                                               | Relative cost to run on Google Cloud |
|----------------|-----------------------|-----------------------------------------------------------------------------|--------------------------------------|
| Hosted Shared  | Multi\-user           | Windows Server \(2016 or 2019\), RDSH role and Desktop Experience enabled\. | ⭐                                    |
| Server VDI     | Single user           | Windows Server \(2016 or 2019\), Desktop Experience enabled\.             | ⭐⭐⭐                                  |
| Desktop VDI    | Single user           | Windows 10 \(BYO licensing and STN required\)                               | ⭐⭐⭐⭐⭐                                |

Another common mis-conception is that Google Cloud's sole-tenant nodes (STN) are required to serve ‘desktop' use cases. Sole tenant nodes are required to comply with Microsoft's BYO licensing scenarios such as Windows 10 (desktop) OS. As mentioned above, Windows Server can be used to deliver a single-user desktop ("Server VDI") in addition to a multi-user desktop (Hosted Shared). Sole tenant nodes can also be used for Windows Server instances when you're bringing your own Windows Server licensing.

Most flavors of Linux are multi-user capable out of the box. As such, they can be deployed in either Hosted Shared or "Server VDI" models, with similar relative cost implications.

> **Note:**
>
> To help with the decision making process, the following decision tree compares [Hosted Shared Desktops (Server OS multi-user desktops) to VDI Desktops](/en-us/tech-zone/design/design-decisions/application-delivery-methods.html#hosted-shared-vs-vdi-desktop-overview). The tree doesn't explicitly differentiate between client VDI and server VDI models, but the decisions presented are valid for both. When a use case suggests VDI is the appropriate delivery model for your workload, Server VDI ought to be considered wherever possible for running on Google Cloud.

#### Published desktops or published apps?

At the end of the day, the virtualized apps you deliver to users in a Citrix virtualization system run on VDAs. You have options for how you present them, which determines how users interact with them. You can present the user with, or "publish" individual applications and files. You can also present them with a desktop on which they interact with applications and data.

![published-desktops-published-apps](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_published-desktops-published-apps.png)

Example: a hosted shared desktop, being presented as a windowed app in Citrix Workspace app for Windows.

There's more to this choice (and many customers use both), but here's an attempt to summarize:

**Published desktops** (both hosted shared and VDI):

|  |                                                                                                                                      |
|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \+ | Give users a familiar metaphor for interacting with the apps and data on the system. Can be simpler for users to grasp and get productive using. Great for delivering flexible environments with many applications. |
| \- | Users expect things to work like they do on a desktop\. You are working harder to balance security with access and functionality, and you are managing a Windows desktop\. User profiles, application settings, data storage, and desktop configuration management become critical\. Doubly so if users expect settings to roam between regions\. |
| \- | Require more VM instance resources \- the Windows Desktop services consume more resources for each user vs\. published apps\. |

**Published applications:**

|  |                                                                                                                                      |
|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \+ | Published apps are often easier to secure, require less resources to deliver, and can provide users with a simpler user experience. Citrix calls this "seamless windows". |
| \- | User experience can get complicated with larger numbers of published apps. |
| \+ | Still requires management of user profiles, application settings, and data storage, but often simpler and with more flexibility in execution relative to published desktops. |
| \+ | Require less VM instance resources vs. Windows Desktop presentation. Multiple published apps usually run inside the same session - a feature Citrix calls session sharing. |

#### Pooled or persistent?

This choice is another property of the machine catalog and is defined upon catalog creation. The hosted shared delivery model usually uses pooled/non-persistent VDAs, but both VDI models can use either pooled or persistent machine catalogs. With the pooled model, OS instances are reset by MCS on logoff or reboot of the VDA. This model ensures that users get a ‘clean' system image, which is in turn based on your template or ‘golden image' VM instance(s) and snapshots of it's persistent disk. They're referred to as ‘pooled' as a pool of VDAs are maintained and users are dynamically connected to an available/unused VDA in the pool. User settings and data can be managed several different ways. With pooled VDAs, they're handled such that the user gets the same configuration and experience regardless of which VDA they're logged into. See user environment/settings management in this document for more details on this topic.

Persistent machine catalogs contain VDA instances which are assigned to individual users, and they persist between reboots. This model is useful for scenarios where users need to install their own applications (such as developer environments) and use cases where necessary applications are not multi-user compatible.

Pooled instances tend to be the easiest to manage over time since Citrix's MCS can update the system disks attached to pooled instances with a few clicks. Capacity and cost management also tends to be more effective since an idle pool of instances can serve many users. Pooled instances are a bit less flexible than dedicated since changes to pooled instances don't usually persist between reboots. Technologies such as [Citrix User Personalization Layer](/en-us/citrix-virtual-apps-desktops-service/install-configure/user-personalization-layer.html) can be used to persist user initiated changes across sessions on different pooled VDAs, though it's only compatible with single user "VDI" use cases.

Persistent instances can be simpler to deploy, but tougher to manage over time since you have to handle OS/application patching, upgrading, and maintenance inside the VM. It can also be more expensive from a cost/capacity perspective as it is often tough to predict when a user will log on. This means that users must wait while their instance is started, or administrators must keep them running during time windows where each user is expected to log on.

#### Managed or unmanaged?

Catalogs created and managed by MCS can contain persistent or non-persistent clones of template (or ‘golden master') VM instances. Machine catalogs can also be provisioned with another process or technology. Either way, you want to make sure they're created as power managed:

![managed-unmanaged](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_managed-unmanaged.png)

If you don't use power managed machine catalogs, key features such as Citrix Autoscale will not work, and you won't have help managing costs and capacity. Using MCS for VDA fleet provisioning and management brings a host of useful benefits to administrators, but ‘unmanaged' VDAs - those provisioned outside of Citrix - can also be used. We cover those benefits in [Fleet and Image Management](#fleet-and-image-management) later in this guide.

#### GPU Acceleration?

Certain types of applications deployed on VDAs can benefit from GPU resources if they're available to the VM instance. All three delivery models (hosted shared, server VDI, and desktop VDI, for both Linux and Windows) can use NVIDIA accelerated [GPU instances for graphics workloads on Google Cloud](https://cloud.google.com/compute/docs/gpus#gpu-virtual-workstations). These Virtual Workstation GPUs can be attached to general-purpose N1 machine types for graphics intensive workloads such as 3D visualization, chip design, CAD/CAM, graphics and video editing, and include the required GRID license.

With the appropriate NVIDIA GRID driver installed on the instance, Citrix's VDA software detects GPU presence and configures itself appropriately.

> **Tip:**
>
> Citrix's HDX display protocol stack does lots of auto-detecting and adapting on the fly to provide the best possible user experience. However, it also tries to balance performance, responsiveness, and richness of the UX with bandwidth consumption. As such, graphics intensive workloads often benefit from some fine-tuning to get the balance right. See [HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) for more information. Note that Citrix does provide a policy template called "Very High Definition User Experience" (as outlined in [Baseline Policy Design](/en-us/tech-zone/design/design-decisions/baseline-policy-design.html)). This template can be used as the starting point for fine-tuning to your specific environment.

### Fleet and Image Management

Citrix virtualization on Google Cloud includes built in features which are designed to simplify VDA provisioning and image management at scale. These features are often referred to as "Machine Creation Services", or MCS for short. MCS uses IAM service accounts on Google Cloud to facilitate VDA management on GCP.

> **Tip:**
>
> Before you dive into setting up and using MCS on GCP, review the Citrix documentation on setting up and using MCS on [Google Cloud Platform virtualization environments](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html). This document walks you through enabling Google Cloud APIs, creating and configuring the IAM Service Account, creating Hosting connections and resources, and much more.

#### MCS Issues and Limitations

MCS has a couple of important limitations you need to know about before deploying a Citrix virtualization system on GCP today. As the MCS feature set is delivered as a cloud service, you can expect this list to change over time as the service evolves. In the interim, you need to know about them so you can adjust your design and execution plan accordingly.

The current known issues/limitations are:

-  **Provisioning of Linux VDAs**. MCS on GCP has not been fully tested for provisioning of Linux VDAs, so this option is not currently supported. Running Linux VDAs on Google Cloud, even with GPUs attached, is. To work around this issue: provision Linux VM instances outside of MCS using Google Deployment Manager templates or another mechanism, then add pre-provisioned instances to a power managed machine catalog for assignment and power management.

#### Importing VDA Images from On-Premises

Custom VDA images are something many customers may already have and want to use on GCP. While deploying a fresh new instance and configuring that from scratch is preferable, there are times where it may not be feasible. For example, there can be a base image that has been configured on-premises where application owners or application dependencies are not known but are relied upon for critical business operations. Fortunately, on GCP you can import your on-premises image into GCP. Importing is also necessary when deploying Windows client OS variants (i.e. Windows 10) as Windows client operating systems are not natively available in Google Cloud's image catalog. For more information, see [importing a virtual disk.](https://cloud.google.com/compute/docs/import/importing-virtual-disks)

> **Note:**
>
> Before importing an existing disk, be sure to read and understand the differences of [importing a virtual disk](https://cloud.google.com/compute/docs/import/importing-virtual-disks) from your on-premises environment. Where possible, it's preferable to deploy new instances from the public image library and create your master images from scratch.

#### Using Sole-Tenant Nodes on GCP

Google Cloud has a feature called sole-tenant nodes which are useful for various use cases, including [bringing your own license for Windows OS](https://cloud.google.com/compute/docs/instances/windows/bring-your-own-license/bringing-your-own-license-sole-tenant-nodes), and deploying Windows client VDAs on GCP. When configuring sole-tenant nodes, you can configure them in one or more zones within a GCP region. Citrix MCS fully supports provisioning VDAs on sole-tenant nodes, but is not automatically aware of which GCP zones your sole-tenant nodes are deployed in. If you intend to deploy VDAs to sole-tenant nodes, be sure to check out the [GCP Zone Selection](/en-us/tech-zone/learn/poc-guides/gcp-sole-tenant.html) documentation for details.

 > **Note:**
 >
 > For a good tutorial on deploying Windows 10 on GCP, see the [GCP Windows 10 Sole Tenant with Optional Shared VPC Catalog Creation POC](/en-us/tech-zone/learn/poc-guides/gcp-win10-catalog-creation.html) guide. The article covers both custom image import and sole-tenant node usage on GCP.

### Cost Optimization and Capacity Management

 When running VDAs on Google Cloud, you're paying for the compute, storage, and networking resources you use. This means there's a direct correlation between the amount of capacity you consume and the costs you incur. The choices you make and the practices you adopt have a direct correlation to the operational cost of the virtualization system.

First off, make sure you choose the right [workload delivery options](#workload-delivery-options-and-considerations) - the topics you just read through if you're reading this front to back. These can have a dramatic impact on the total cost of the solution! Here are a few other recommendations and topics to consider as you work to balance cost with capacity and optimize the user experience.

#### On-demand Provisioning

When you use MCS to create non-persistent machine catalogs in Compute Engine, MCS uses on-demand provisioning to reduce your storage costs, provide faster catalog creation, and provide faster instance power operations. With on-demand provisioning, Compute Engine instances are created only when the Citrix Virtual Apps and Desktops service initiates a power-on action. On-demand provisioning is used for non-persistent machine catalogs.

> **Note:**
>
> Some administrators find on-demand provisioning confusing initially, as VDA instances do not show up in the Google Cloud console until MCS powers them on. Also, since the instances receive a new virtual NIC and MAC address, it can take some time for Active Directory DNS entries to update/replicate successfully. VDA identity disks DO persist between reboots and creation/deletion events.

#### Rightsizing VDA Instances

Now that you've gotten some insight into the important decisions related to workload delivery options, let's dig into right-sizing your VDA instances. For [VDI based delivery models](#shared-os-multi-user-or-single-user-vdi), selecting the right instance type is straightforward. Assuming you've done some homework and have a decent understanding of the resource requirements of the OS, apps, and users of the VDI instances, you can simply map these requirements to the [instance types available in the Google Cloud region](https://cloud.google.com/compute/docs/regions-zones#available) of your choice. Don't worry if you don't have a perfect match between available instance types and workload requirements. Google Cloud supports [custom instance types](https://cloud.google.com/custom-machine-types), which allow you to tweak the shape of your VDA instances as you go. Sustained use and committed use discounts still apply to custom instance types, so don't let that deter you from adjusting as needed to get the right size up front.

Also - Google Cloud's [sizing recommendations](https://cloud.google.com/compute/docs/instances/apply-sizing-recommendations-for-instances) feature can be used to identify adjustments to VDA shapes you may want to make over time.

![Rightsizing VDA Instances](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_rightsizing-vda-instances.png)

> **Note:**
>
> One important thing to note - workload resource consumption can change over time. Sometimes events/activities reduce resource requirements - like when an administrator applies optimizations to the environment. Conversely, sometimes these requirements go up, like when an OS or app vulnerability is patched, or an update is applied. Find your baseline, but it's important to monitor consumption trends over time and adjust as necessary to find the optimal balance between user performance and costs.

When selecting the right instance size for hosted shared VDAs, things get a bit more complicated. What you're ultimately searching for is a moving target - the right balance of performance, cost, and manageability. To further complicate things, every workload is different. Variances between OS, applications, settings, tuning, and user expectations make it tough to nail down the right shapes for your VDAs. It also tends to change over time as well.

Fortunately, the tools and techniques for finding that ‘goldilocks' balance between performance, cost, and manageability are well known and thoroughly documented. One excellent article we'd recommend starting with is [Citrix Scalability in a Cloud World 2018 Edition](https://www.citrix.com/blogs/2018/08/16/citrix-scalability-in-a-cloud-world-2018-edition/). This article is still relevant today as it discusses leading practices regarding instance selection based on performance, manageability, cost, available pricing models, and LoginVSI scalability testing. These concepts and considerations are still valid today, even though instance choices and pricing have likely changed since its initial publication.

Another article worth mentioning is [Right-sizing Citrix on Google Cloud Platform](https://www.citrix.com/blogs/2018/07/23/right-sizing-citrix-xenapp-on-google-cloud-platform/). Although a bit dates, this article digs even deeper into the considerations and provides an example of how to find the most cost effective instance type based on single VDA scaling and your instance costs.

Finally, for extra insight into strategies for optimizing VDA costs, see this [Autoscale tech brief](/en-us/tech-zone/learn/tech-briefs/autoscale.html) on Citrix TechZone. It helps align your instance cost estimates with the capabilities of the [Citrix Autoscale](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html) feature, including the use of vertical load balancing.

Speaking of Citrix Autoscale - read up on it and use it: with a bit of thought and clever design, you are able to cost optimize your VDA fleet while ensuring you've got capacity available to handle expected and unexpected fluctuations in system demand.

Speaking of demand patterns - you want to invest some time and resources into understanding the unique patterns of each workload. Expect them to change and evolve over time, and be prepared to adjust your capacity management strategy and tactics to accommodate.

#### Choosing the right Pricing Models

Google Cloud offers various different [pricing](https://cloud.google.com/pricing) models customers can use for the different types of workloads you run there. Understanding the demand patterns for different use cases can help you choose the right model for each resource to balance cost and service availability/performance. In a Citrix virtualization system, customers commonly consider sustained use vs. committed use discount models for the resources that run on GCP. Sustained use discounts can vary between instance types (N1 vs. N2, for example) and some instance types (such E2) don't offer sustained use discounts. See [VM instance pricing](https://cloud.google.com/compute/vm-instance-pricing#general-purpose_machine_type_family) for more details.

The following is a simplified chart illustrating sustained use vs committed use discounts **for N1 instance types:**

![optimizing-cost](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_optimizing-cost.png)

Some resources are unique, highly scalable, and must be available for a Citrix virtualization system to function. As such, they're commonly run 24/7 and deployed N+1 for availability, and are great candidates for committed use discounting. This includes Active Directory, Citrix Cloud Connectors, Citrix ADC/Gateway VPX, and Citrix StoreFront VM instances.

For VDA instances, the choice isn't quite as simple, but the more clearly you understand your demand patterns, the clearer the choice is. It all boils down to how long the VDA needs to be powered on for. Consider the following chart (specific to **N1 instance types**), which is reproducible with a bit of back-of-the-envelope math:

![break-even](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_break-even.png)

This diagram shows that if a resource (running on an N1 instance type) will be on for over 50% of the time during a given billing cycle, you start saving money if you can apply 3 year committed use discounting. The break even point on a 1 year committed use discount is approximately 82%. If a resource is going to be powered on for more than that during a billing cycle, and 3 year committed use isn't available, then a 1 year committed use makes sense.

### Performance Tuning Considerations

Many factors can contribute to the perception of performance users get on your Citrix virtualization system. Besides selecting the right shape of VM instance for your VDAs, other key areas to investigate for performance optimization include the following:

**User environment and settings management choices**: Policies are your friend when managing and optimizing performance for your users. Policies control the configuration of Windows, applications, sessions, and much more. To further complicate things, there are multiple policy engines you can potentially use, each with their own impact on the user experience. Choosing the right policy engine and establishing consistent baselines are important, and fortunately are covered in depth in [Baseline Policy Design](/en-us/tech-zone/design/design-decisions/baseline-policy-design.html). Also, the [Citrix Workspace Environment Management service](/en-us/workspace-environment-management/service.html) can be used to optimize the user experience and simplify management in a diverse environment.

**Optimizing Windows**: Windows is a general purpose operating system, and is designed to cover a broad variety of use cases, hardware types, and so on. Many of the default settings in Windows are unnecessary in a Citrix virtualization environment. Fortunately [Citrix Optimizer](https://support.citrix.com/article/CTX224676) is available to help, and includes many comprehensive templates you can apply to your VDAs to get the best possible performance and lowest overall resource utilization.

**Antivirus tuning**: Running antivirus software on Citrix VDAs and supporting infrastructure is a solid and recommended practice. However if incorrectly installed/configured, it can wreak havoc on the user experience. See [Endpoint Security and Antivirus Best Practices](/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html) for a great primer on how to get it right.

**HDX Protocol Optimization**: Citrix's HDX display protocol stack does many auto-detecting and adapting on the fly to provide the best possible user experience. It attempts to balance performance, responsiveness, and richness of the UX with bandwidth consumption. For some use cases (such as graphics intensive workloads, or low/poor quality network connections) often benefit from some fine-tuning to get the balance right. See [HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) for more information. Citrix's SD-WAN can also help optimize and [measure the HDX User Experience](/en-us/tech-zone/design/reference-architectures/sdwan-hdx-experience.html).

Also, Citrix provides several pre-built session policy templates which can be used to flexibly match settings to your specific use cases. These policies are configured and managed in Citrix Cloud Studio, and can be applied using various filters. These filters allow you to make sure the right policy is applied to optimize specific scenarios.

![HDX Protocol Optimization](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_hdx-protocol-optimization.png)

### User Environment/Settings Management

Designing and managing user settings and data is one of the more complex, and important elements of a Citrix virtualization system. When done right, session logon/logoff is fast and reliable. Apps are pre-configured for users, and users get a consistent, predictable experience regardless of the VDA they log into. When done wrong or ignored, the user experience can suffer dramatically.

Fortunately user environment and settings management decisions are common across Citrix virtualization systems, regardless of the platform VDAs are deployed on. They're also thoroughly documented. These decisions are highly dependent on user location, end user connectivity, and security requirements. As such, we're not going to cover this topic in depth here, but we get you started with some references. There are many options available that can be tailored to your specific needs.

The following are some links to get you started:

|  |                                                                                                                                      |
|----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Workspace Environment Management service](/en-us/workspace-environment-management/service.html) | Workspace Environment Management service uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops deployments. |
| [Baseline Policy Design](/en-us/tech-zone/design/design-decisions/baseline-policy-design.html) | This article outlines various policy decisions to be made when using Citrix policies. |
| [Profile Management](/en-us/profile-management/current-release/profile-management-best-practice.html) | An article outlining some best practices when it comes to Profile Management. |
| [Plan your deployment](/en-us/profile-management/current-release/plan.html) | A great resource to help plan for Profile Management in a Citrix virtualization system. |
| [Citrix user personalization layer](/en-us/citrix-virtual-apps-desktops-service/install-configure/user-personalization-layer.html) | For VDI use cases (single user, single OS instance) Citrix's user personalization layer technology can be used to dramatically simplify system management. Citrix UPL allows administrators to use pooled/non-persistent machine catalogs while providing users with a consistent and personalized user experience across VDA instances. The user layer is retained as a virtual hard disk file on a Windows file share. It is mounted to the VDA at logon time, and captures user changes inside the session, including user installed applications. The user layer is mounted to the VDA at logon and detached at logoff. |

### File Storage and Data Replication

Most Citrix virtualization systems on GCP require at least basic access to a Windows compatible file share to persist user settings, user data, and application data. Windows file shares are also used to store [Citrix user personalization layers](/en-us/citrix-virtual-apps-desktops-service/install-configure/user-personalization-layer.html). When these shares are not available, the user experience and application functionality suffer. It is important to ensure that whatever solution you choose to provide, Windows compatible file shares are highly available and data is regularly backed up.

For multi-site deployments, reliable and performant data replication may also be necessary to meet availability, RPO, and RTO needs. This is especially true for environments where users can connect to desktops/apps in 2 or more regions, and application data/user settings must be available in the region where the apps/desktops run. The following section describes some solutions to consider for providing file storage and data replication services on GCP.

While non-Windows solutions for providing Windows file shares exist, most of these solutions cannot deliver the indexing capabilities required for search functionality inside a Windows desktop or applications such as Microsoft Outlook running on Windows. As such, most customers turn to Windows-based file server solutions, at least for storing user profiles and persistent application data. Fortunately, both customer managed and cloud service options are available for use when Citrix virtualization systems are run on GCP.

#### Customer Managed: Windows File Servers on Google Compute Engine

The first solution many customers consider for providing Windows compatible file services on GCP is building their own Windows file servers on Compute Engine to serve each resource location on GCP. Since Windows file servers are needed by various different types of applications and workloads, many IT shops can gravitate towards building and managing their own since this is something they know how to do. At the most basic level, the customer creates one or more Windows instances, attaches more persistent disks, joins the instances to their Active Directory, and finishes configuring Windows File Services.

This option, as you might imagine, provides customers with the most control and flexibility. While this is very appealing to certain types of customers and certain verticals, it also comes at a cost: the responsibility to size, scale, build, manage, patch, secure, and maintain everything from the Windows OS up. Customers electing to go this route ought to also ensure these file servers are highly available. This is often accomplished using file servers in multiple zones, and using Windows DFS-N/DFS-R, [Windows failover clusters](https://cloud.google.com/compute/docs/tutorials/running-windows-server-failover-clustering), or storage spaces direct. It's easy to end up in an unsupported configuration (per Microsoft) if you're not careful.

> **Note:**
>
> Customers considering this option ought to review [Microsoft's support statement](https://support.microsoft.com/en-ca/help/2533009/information-about-microsoft-support-policy-for-a-dfs-r-and-dfs-n-deplo) regarding using DFS-R and DFS-N for roaming profile shares and folder redirection shares.

#### Third Party

An alternative solution is using third party solutions such as [NetApp Cloud Volumes ONTAP](https://cloud.netapp.com/ontap-cloud) or the [Cloud Volumes Services for GCP](https://cloud.google.com/solutions/partners/netapp-cloud-volumes). Both solutions allow you to create compatible SMB shares that can be used for your storage needs. There are benefits to using third party storage solutions as opposed to managing your own Windows File Servers, such as less administrative overhead when managing storage. See [File servers on Compute Engine](https://cloud.google.com/solutions/filers-on-compute-engine) for more information.

## Citrix ADC/Gateway VPX on Google Cloud

Deploying the Citrix ADC/Gateway on GCP is different than deploying it on-premises, though in the end you're managing them yourself. Fortunately deploying Citrix ADC/Gateway on GCP is thoroughly documented. We recommend reviewing the following resources before you solidify your design and begin implementation:

-  [Citrix ADC VPX on GCP in Citrix Docs](/en-us/citrix-adc/13/deploying-vpx/deploy-vpx-google-cloud.html): Provides a comprehensive overview of Citrix ADC on GCP, including supported VPX models, GCP regions, Computer Engine instance types, and other resource references.
-  [Citrix ADC VPX GCP Marketplace Deployments](https://console.cloud.google.com/marketplace/partners/citrix-public): All available Citrix networking deployment solutions available in the GCP Marketplace. Functional and relevant for Citrix Gateway deployments with CVAD/CVADS also.
-  [Citrix ADC GDM Templates](https://github.com/citrix/citrix-adc-gdm-templates/blob/master/README.md): A GitHub repository for Citrix ADC GDM templates. This is an excellent reference for a repository that hosts Citrix ADC templates for deploying a Citrix ADC VPX instance on the Google Cloud Platform.

As discussed in [Citrix ADC VPX on GCP](/en-us/citrix-adc/13/deploying-vpx/deploy-vpx-google-cloud.html) on Citrix Docs, there are two primary deployment options available. They are:

-  [Standalone](/en-us/citrix-adc/13/deploying-vpx/deploy-vpx-google-cloud.html): Individual instances of Citrix ADC/Gateway can be deployed and managed as separate entities. This is commonly used for smaller scale or POC deployments where high availability is not a requirement.
-  [High Availability](/en-us/citrix-adc/13/deploying-vpx/deploy-vpx-google-cloud-ha.html): This is the most commonly deployed model for production environments: pairs of Citrix ADC/Gateway VPX instances can be deployed using an HA configuration within the same zone or across multiple zones in the same region. We dig into this option more deeply later in this section.

> **Best practice:**
>
> **When you deploy Citrix ADC/Gateway appliances on GCP, we recommend using Premium tier (regional) external IP addresses.** When using premium tier external IP's, traffic ingresses and egresses at the Edge network location nearest the user. Traffic then traverses Google's private network to get to the region where the resource is deployed. This provides better throughput, lower latency, and more consistent performance (lower jitter) as compared to Standard tier external IP addresses. For more information, see Google Cloud [Network Service tiers](https://cloud.google.com/network-tiers/docs/overview).

### ADC Standalone

While Citrix ADC VPX generally supports single, dual, or multiple NIC deployment types, Citrix recommends using at least three VPC networks for each ADC when deployed on GCP, with a network interface in each VPC for optimum throughput and data separation. When deployed to support Citrix Virtual Apps and Desktops, the management interface ([NSIP](/en-us/citrix-adc/current-release/networking/ip-addressing/configuring-citrix-adc-owned-ip-addresses/configuring-citrix-adc-ip-address.html)) is typically attached to the "Private Citrix Infrastructure Subnet," the subnet IP ([SNIP](/en-us/citrix-adc/current-release/networking/ip-addressing/configuring-citrix-adc-owned-ip-addresses/configuring-subnet-ip-addresses-snips.html)) is attached to the "Private Citrix VDA Subnet," and the Citrix Gateway virtual IP ([VIP](/en-us/citrix-adc/current-release/networking/ip-addressing/configuring-citrix-adc-owned-ip-addresses/configuring-and-managing-virtual-ip-addresses-vips.html)) to the "Public Subnet." The following simplified conceptual diagram depicts this configuration. It shows a single VPX instance in a single zone - this design pattern would be duplicated (likely in a second zone) for a High Availability configuration:

![ADC Standalone](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_adc-standalone.png)

The following is a table showcasing the purpose of each NIC along with the associated VPC network:

| NIC   | Purpose                                      | Associated VPC network                                       |
|-------|----------------------------------------------|--------------------------------------------------------------|
| NIC 0 | Serves management traffic \(NSIP\)           | \(❶\) Management network       |
| NIC 1 | Serves client\-side traffic \(VIP\)          | \(❷\) Public network           |
| NIC 2 | Communicates with back\-end servers \(SNIP\) | \(❸\) Back\-end server network |

> **Important:**
>
> Citrix ADC VPX instances with three NICs require a minimum of 4 vCPUs when running on GCP. See [maximum number of network interfaces](https://cloud.google.com/vpc/docs/create-use-multiple-interfaces#max-interfaces) for more information.

### ADC High Availability across Zones

As mentioned earlier, this is the most common deployment model for Citrix virtualization systems. This model uses a pair of Citrix ADC VPXs in a single region deployed across multiple zones. High availability (active/passive) can be achieved multiple ways. You can use a GCP HTTPS Load Balancer with the ADCs configured independent of each other or by using Citrix ADCs HA configured in Independent Network Configuration (INC) mode. The latter option/architecture is expected to be popular for public cloud deployments, so we focus on that here.

While there are potential variants for a Citrix ADC/Gateway VPX architecture on GCP, the following diagram depicts a three NIC Citrix ADC HA solution. This solution can be deployed by the [Google Deployment Manager template](https://github.com/citrix/citrix-adc-gdm-templates/tree/master/high-availability-templates/3nic) with pre-configured VPC networks and subnets:

![conceptual-architecture](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_conceptual-architecture.png)

When using the Google Deployment Manager template, you must configure the VPC networks before deploying the Citrix ADC appliances. The three VPC networks ought to consist of the (❶) management network, (❷) public network, and (❸) backend-server network and appropriate subnets within each VPC network.

![traffic-flow](/en-us/tech-zone/design/media/reference-architectures_citrix-google-virtualization_traffic-flow.png)

In the preceding diagram, we can see that each ADC has a different Gateway virtual IP (VIP). This is a characteristic of an [Independent Network Configuration (INC)](/en-us/citrix-gateway/13/high-availability/ng-ha-routed-networks-con.html). When VPXs in an HA pair reside in different zones, the secondary ADC must have an INC, as they cannot share mapped IP addresses, virtual LANs, or network routes. The NSIP and SNIP are different for each ADC in this configuration, while the Citrix Gateway VIP uses a [Citrix ADC feature called IPset](/en-us/citrix-adc/13/load-balancing/load-balancing-customizing/multi-ip-virtual-servers.html), or Multi-IP virtual servers. This feature can be used for clients in different subnets to connect to the same set of servers. With IPset, you can associate a private IP to each of the primary and secondary instances. A public IP can then be mapped to the primary ADC in the pair. In the case of failover, the public IP mapping changes dynamically to the new primary.

For more information on adding a remote node to an ADC to create an INC-based HA pair, see [Citrix docs](/en-us/citrix-gateway/13/high-availability/ng-ha-routed-networks-con/ng-ha-add-remote-node-tsk.html). For general HA deployment information for ADC on Google cloud, see [Deploy a VPX high-availability pair on Google Cloud Platform](/en-us/citrix-adc/current-release/deploying-vpx/deploy-vpx-google-cloud/deploy-vpx-google-cloud-ha.html).

## Citrix StoreFront on Google Cloud

Citrix StoreFront is a component of the Citrix virtualization stack that provides UI services and also plays a role in your authentication strategy. StoreFront can be seen as the customer managed predecessor to the Citrix Workspace service, and it's been used to front-end Citrix virtualization systems for over a decade. StoreFront is delivered as an MSI/EXE downloadable, and is installed on one or more Windows Server instances. Multiple StoreFront instances can share configuration (and optionally subscription) data by creating StoreFront server groups. Multiple instances, front-ended by a service aware load balancer like the Citrix ADC/Gateway appliance, are used to create a highly available subsystem.

Deploying Citrix StoreFront on Google Cloud is not much different than deploying it on-premises. In the end, you're also managing all the components of StoreFront yourself.

See [Plan your StoreFront deployment](/en-us/storefront/current-release/plan.html) for general considerations which apply to all deployments including StoreFront on Google Cloud.

The main difference with StoreFront on GCP is that you typically deploy multiple StoreFront instances in a StoreFront server group across multiple Google Cloud zones. It is important to note that the features enabled with this design are _dependent upon latency between zones_. Per [Plan your StoreFront Deployment/Scalability](/en-us/storefront/current-release/plan.html#scalability), StoreFront server group deployments are only supported where links between servers in a server group have latency of less than 40 ms (with subscriptions disabled) or less than 3 ms (with subscriptions enabled). While inter-zone latency on GCP is typically less than 1 ms, make sure you measure latencies between instances in all zones you plan to host StoreFront and enable/disable subscriptions accordingly.

We've already called this out a couple times earlier in this document, but it is worth calling out again: for environments with extensive resiliency requirements, Citrix strongly recommends a StoreFront implementation to fully benefit from the Local Host Cache feature of the Citrix Virtual Apps and Desktops service. This architecture provides resiliency in case Cloud Connectors cannot reach Citrix Cloud. If this happens, users are still be able to connect to new and existing sessions during an outage scenario. For more details, limitations, and implications of Local Host cache activation, see [Local Host Cache (CVADS)](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html).

As mentioned above, StoreFront implementations on Google Cloud ought to span multiple Google Cloud zones for high availability, but remember to take the Citrix ADC/Gateway design into account. Citrix ADC/Gateway is typically recommended to be used in front of StoreFront instances to provide load balancing, advanced monitoring, and more service resiliency.
