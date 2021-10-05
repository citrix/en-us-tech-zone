---
layout: doc
h3InToc: true
contributedBy: Rick Dehlinger, Jill Fetscher, Allen Furmanski, Faubricio Gutierrez, David Johnson, Joanne Lei
specialThanksTo: Nick Czabaranek, Josh Fleming, Matt Lull, Ryan McLure, Rich Meesters, Arnaud Pain, James Richards, Neil Spellings
description: Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on an Amazon Web Services cloud platform.
tz_title: Virtual Apps and Desktops Service - AWS
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: Virtual Apps and Desktops Service - AWS

## Audience and Objective

This document is intended to help Citrix partners and customers understand the most critical design decisions necessary to successfully deploy Citrix virtualization technologies on Amazon's public cloud. It is not meant to be a "How-To" reference - Citrix considers those [Deployment Guides](/en-us/tech-zone/build/deployment-guides.html), and they are now delivered and maintained separately from [Reference Architectures](/en-us/tech-zone/design/reference-architectures.html). In this document, we use the Citrix Architectural Design Framework to organize and present the leading practices, recommendations, and design patterns which are used by Citrix and select **Citrix consulting partners**.

## Overview and Executive Summary

Citrix virtualization and networking technologies have been successfully serving the needs of enterprises large and small for nearly three decades. There are many ways in which these technologies can be licensed, deployed, integrated, and managed. This flexibility allows Citrix technologies to serve various different use cases, business types, integration requirements, and operational models. This paper is written for the Citrix customer or partner who's considering or planning to deploy these technologies on AWS' public cloud infrastructure.

For both existing customers looking to modernize their infrastructure and new customers looking to deploy Citrix virtualization and networking technologies, there are several key high and low level decisions which must be made along the way to help facilitate a successful deployment. To help customers and partners understand these decision points, we have introduced and defined some specific terminology to set the appropriate context, then used this context to highlight the critical decisions and implications to consider as you plan your deployment.

We start by defining two primary technology adoption models:
**Customer Managed** and **Cloud Services**. We then break the [components of a Citrix virtualization system](#citrix-virtualization-system-components) down into multiple subsystems, and categorize them by adoption model:

| Adoption Model / Subsystem function | Customer Managed (installed from downloaded binaries) | Cloud Service (delivered via Citrix Cloud) |
|-----------------|-------------------------------|-------------------------------|
| **Session brokering and administration** | Citrix Virtual Apps and Desktops (**CVAD**) | Citrix Virtual Apps and Desktops Service (**CVADS**) |
| **User interface (UI) services** | Citrix StoreFront | Citrix Workspace (the service, consumed via Citrix Workspace app or web browser) |
| **Authentication** | Citrix StoreFront (plus Citrix ADC/Gateway for most use cases) | Citrix Workspace (plus Citrix ADC/Gateway for certain use cases) |
| **HDX session proxy** | Citrix ADC/Gateway | Citrix Gateway Service |

We take a stand for **cloud services**, recommending that **most organizations use or plan to use cloud services** where feasible. We don't offer this stand blindly - while we do believe cloud services, in the end, offer overwhelmingly positive benefits for our customers, we recognize that **not all organizations/deployments are able to use cloud services for all subsystems today**. Sometimes, use case requirements (with technical attributes or shortcomings in a currently available/specific cloud service) need adopting a combination of cloud services and a customer managed component: we focus on these in this paper. In other cases, non-technical items (politics, budgetary/contractual considerations, cloud readiness deficiencies, regulatory/compliance considerations, and such) may discourage the usage of cloud services. In these instances, we recommend working with your Citrix partner/sales/engineering team to help overcome them. Through the rest of this paper, we go to great lengths to identify key capabilities, features, or attributes that help customers decide which adoption model to use for which subsystem and when.

Next, we define and examine three common deployment scenarios, highlighting which adoption model is used for each subsystem:

-  **Greenfield**/**Cloud only** - uses cloud services for all Citrix virtualization system subsystems, plus AWS public cloud services.
-  **Hybrid** (not to be confused with a 'hybrid cloud') - the most common deployment model, the hybrid model uses CVADS for session brokering and administration, with both customer managed and cloud service options for the remaining subsystems.
-  **Lift and Shift** - as the name states, this model uses existing, customer managed CVAD, StoreFront, and ADC/Gateway and either migrates these components to AWS as is, or installs them into AWS as part of a workload migration to AWS public cloud services. While this is a valid deployment model for certain specific use cases, it comes with substantial caveats.

Finally, we use the well documented **Citrix Architectural Design Framework** to organize and present the key design decisions to be considered when deploying Citrix virtualization technology on AWS. We keep our focus on "what's different about Citrix on AWS" for clarity, providing links to other resources for more detailed information as needed.

We ultimately recommend that most customers use the **Hybrid deployment model** from day one, using the CVAD service for **session brokering and administration**. This provides the customer with the key capabilities necessary to cost-effectively run a Citrix virtualization system on AWS, substantially reduces the cost and complexity, provides access to the latest features and capabilities available, and simplifies the migration to and usage of other cloud services in the future. Either cloud services OR customer managed components can be used for the remaining subsystems (depending upon the customers' specific needs), though we recommend customers are clear as to why they're using customer managed components and have a plan to move to cloud services in the future once the cloud services meet their specific needs.

For more insights into leading practices for Citrix on AWS, readers can reference the following Cloud Guidepost articles:

-  [Leading practices for Citrix Cloud on AWS - Part 1](https://www.citrix.com/blogs/2019/09/23/cloud-guidepost-leading-practices-for-citrix-cloud-on-aws-part-1/)
-  [Leading practices for Citrix Cloud on AWS - Part 2](https://www.citrix.com/blogs/2019/11/21/cloud-guidepost-leading-practices-for-citrix-cloud-on-aws-part-2/)
-  [Leading practices for Citrix Cloud on AWS - Part 3](https://www.citrix.com/blogs/2020/01/21/cloud-guidepost-leading-practices-for-citrix-cloud-on-aws-part-3/)

## Key Concepts and Deployment Scenarios

In this section, we describe some key concepts and deployment scenarios before we dive into specific considerations for each layer of the **Citrix Architectural Design Framework**.

### Technology Adoption Models

Citrix Virtual Apps and Desktops technology can be 'consumed' or implemented many different ways. The most common methods can be described generally as **Customer Managed** and **Cloud Services**. A third model is also worth mentioning - **Partner Managed**. We describe this model here for clarity, but from an architectural design perspective, the first two are the most relevant.

Why are we discussing technology adoption models in a reference architecture? The choice of adoption or 'consumption' model has a substantial impact upon the system design, capabilities, cost, and delineation of management responsibilities. This section will define and explore these models, and provide some general guidance to help customers make informed choices as they design a Citrix virtualization system.

#### Customer Managed

For many years, businesses wanting to consume technology had no choice but to buy, install, configure, and maintain the entire technology stack required to build a Citrix virtualization system. Citrix's virtualization software was only available as installable binaries.
Customers who bought Citrix's virtualization software would download these binaries (often in the form of a downloadable ISO disk image or self-extracting executables) then install, configure, and maintain the software themselves. The Citrix software (and networking hardware) was most commonly installed into/on infrastructure the customer owned and maintained, in data centers they also owned and maintained.

Conceptually speaking, a Citrix virtualization system is made up of various different Citrix components, many of which we describe in detail in this paper. They also require different layers of third-party components which must be provided before you can do anything with the Citrix bits. Ultimately, they all come together to create a functional Citrix virtualization system. For the sake of clarity, this document refers to this technology adoption model as **"Customer Managed"**.
We use this term to describe various different components in a Citrix virtualization system, including the requisite third party components in the layers underneath, next to, and 'above' the Citrix components. This model can also be called "Self-Managed."

Today, customers have compelling alternatives to a customer managed adoption model, yet some still adopt elements of their technology stack using this model for various reasons. While this model provides customers with the **most control over each component**, it comes at a cost: the customer takes on the responsibility to manage and maintain the component, including securing, operating, patching, upgrading, and maintaining high availability. This model is also **commonly deployed for 'air gapped' systems** (those without any
Internet access, and hence are limited in their ability to use cloud services, which are commonly and securely accessed over public networks).

Here's an example of the architecture of a Citrix virtualization system that's using 100% customer managed components deployed on AWS using basic AWS IaaS services such as Elastic Compute Cloud (EC2) and Virtual Private Cloud (VPC) networking. We are discussing some of the details of this architecture in later sections of this document, though you can immediately notice the similarities to the much simpler greenfield/cloud only deployment model:

![Diagram 1: 100% Customer Managed, Lift/Shift deployment using AWS as IaaS only](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_001.png)
*Diagram 1: 100% Customer Managed, Lift/Shift deployment using AWS as IaaS only.*

#### Cloud Services

Over the last 15 years, technological advancements across many different fields have given rise to hyper scale public clouds, sophisticated cloud services, microservice architectures, DevOPS/Agile delivery frameworks, subscription licensing models, and 'evergreen' software and systems.
These advancements have revolutionized the way technology is acquired, adopted, delivered, and maintained across almost every industry in the world.

Today, many of the components or layers that comprise a Citrix virtualization system are available "as a Service." In contrast to the Customer Managed adoption model (where customers buy technology as a corporate asset and build/maintain systems themselves), customers "subscribe" to various services, and the service provider takes on the responsibility for delivering and managing these services. These services are often consumed over public networks (that is, the Internet) leading some to call this "Cloud Service" or "Web Service" adoption model. In this paper we're going to refer to this type of adoption model as "Cloud Managed Services," or simply the **"Cloud Service"** model.

Citrix offers many of its traditional products 'as a Service', using its platform partners' latest technological advancements to simplify and streamline adoption, accelerate the pace of innovation, improve quality, and deliver more incremental value to their customers over time. Citrix calls this service delivery platform "Citrix Cloud," and it represents the current and future state of the art from Citrix.

Here's an example of the architecture of a system that's using 100% cloud service components for a Citrix virtualization system on AWS.
We are discussing the details of this design in a later section of this document:

![Diagram 2: 100% Cloud Services on AWS with AWS Managed Services](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_002.png)
*Diagram 2: 100% Cloud Services on AWS with AWS Managed Services*

#### Partner Managed

While many organizations choose to build their own Citrix virtualization system, some customers seek to get out of the business of managing IT so they can focus resources and attention on serving their own customers and markets. To serve these customers, Citrix works with integration partners who use Citrix technologies to provide a 'finished goods' service to their customers.

Defining and differentiating the different integration partners/types and offerings available are outside of the scope of this document. However, Citrix partners face the same choices customers face when designing a Citrix virtualization system. The Citrix partner can choose to use one or more services from Citrix Cloud, or they can choose to build and manage some components of the system for their customers' specific needs. As such, the guidance provided in this document is relevant to both the Citrix partner and their customers, just for different reasons.

### Citrix Virtualization System Components

To understand the implications of the design decisions we detail later in this document, we're going to put the components of a Citrix virtualization system into higher level 'buckets' we'll then use to guide your decision-making process. Every Citrix virtualization system, regardless of how it is deployed and licensed, needs these components available to function and provide the best, most secure user experience. It is not uncommon for customers to mix and match self-managed components and cloud services, especially if they've got complex use case requirements, third party integration requirements, or extreme control or availability needs.

The following table highlights these key components for clarity. Details and recommendations on when/where you'd use one option vs another is covered later in this document:

| Adoption Model / Subsystem function | Customer Managed (installed from downloaded binaries) | Cloud Service (delivered via Citrix Cloud) |
|---|---|---|
|**Session brokering and administration** - The core of any Citrix virtualization system: without this core subsystem, you don't have any apps or desktops, and you can't manage them! This subsystem is where you define, provision, and update Machine Catalogs (collections of Citrix Virtual Delivery Agent or "VDA" VM instances). It is also where you create Delivery Groups, assigning apps/desktops to users, and administer the environment and user sessions. | **CVAD - Citrix Virtual Apps and Desktops**, either Long Term Service Release (LTSR) or Current Release (CR) versions. If you install and configure a delivery controller in your environment, this is what you're running. It also means you're installing and managing your own Microsoft SQL Server infrastructure. Administrative functions in a customer managed (CVAD) deployment include Citrix Director and Citrix Studio. You install, configure, and manage these yourself using LTSR/CR binaries. Director also requires Microsoft SQL Server infrastructure. Citrix licensing is also a part of this subsystem, with customers installing/configuring/ managing Citrix License Servers and license files. | **CVADS - Citrix Virtual Apps and Desktops Service**, delivered via Citrix Cloud. If you're logging into Citrix Cloud and installing and registering Cloud connectors in your environment, you're using this Citrix Cloud service. You install and register Cloud Connectors on a Windows instances you manage, and then Citrix keeps them evergreen and available. Citrix Cloud also provides and maintains most administrative functionality via a web browser through the Citrix Cloud console. This includes cloud service versions of Citrix Studio and Citrix Director. There is no additional infrastructure for the customer to maintain, keep highly available, or patch/update: Citrix owns this administrative responsibility. |
|**UI (user interface) services** - Native Citrix Workspace apps (and web browsers for clientless access) ultimately connect to a URL. The subsystem behind the URL is configured by IT administrators to match corporate requirements for authentication, and to present virtualized apps/desktops, SaaS apps, and possibly much more for users to access. | **Citrix Storefront**. Also installed/ configured from CVAD LTSR or CR binaries, this role provides extreme flexibility for the most complex deployment scenarios. Typically deployed in pairs, with Citrix ADC/Gateway instances in front of them for high availability. Can aggregate and present apps and desktops from both customer managed/brokered environments (CVAD) and Citrix Cloud managed/brokered environments (CVADS). | **Citrix Workspace** (the service, not the Citrix Workspace app). Provided as a cloud service through Citrix Cloud, and includes many next generation capabilities that are only available with this service. Can aggregate and present apps and desktops from both customer managed/brokered environments (CVAD) and Citrix Cloud managed/brokered environments (CVADS). |
| **Authentication** - In this context, we're referring to how users authenticate before accessing Citrix virtualized apps/desktops, files, SaaS apps, and more. Authentication in a Citrix environment is typically configured at the UI services layer, though Citrix ADC/Gateway can also be used for authentication in all deployment models. Each of the UI service provider options (Citrix StoreFront or Citrix Workspace) has different authentication options available, some requiring a customer managed Citrix ADC/Gateway. | **Citrix StoreFront** (plus **Citrix ADC/Gateway** for most use cases). User authentication services can be provided various different ways, though ultimately require an Active Directory instance and valid user accounts. The customer typically manages the AD instance. Citrix ADC/Gateway instances can also be used to provide authentication services, and provide a ton of advanced capabilities that are commonly used for more complex environments. Citrix Federated Authentication Services (FAS) can also be installed and used to provide session SSO for complex use cases. | **Citrix Workspace** (plus **Citrix ADC/Gateway** for certain use cases). With Citrix Workspace (the service), user authentication sources and requirements are configured once for the Citrix Cloud tenant and used by all users using this URL. It is configured for Active Directory out of the box, but for advanced use cases, can be configured to support other authentication providers. Examples include Azure AD, Okta, customer managed Citrix Gateway, Google Cloud ID, or other SAML/OpenID/RADIUS providers. Some scenarios require customer managed Citrix ADC/Gateways and Citrix Federated Authentication Services (FAS) for the best user experience. |
| **HDX session proxy** - The ability to securely and seamlessly connect users/devices outside the private corporate network to CVAD/CVADS delivered apps and desktops on the inside. | **Citrix ADC/Gateway** appliances - these appliances (or instances) often provide a ton of extra functionality for a Citrix virtualization system. Their core job, however, is to securely proxy HDX sessions to your VDAs when clients are on public networks. Requires installation, configuration, SSL certificates, and such. Compatible with both StoreFront (customer managed UI services) and Workspace cloud service. Also compatible with both Citrix managed and customer managed session brokering options. | **Citrix Gateway Service** - provided by Citrix Cloud, this service proxies HDX session traffic to your VDAs, and it uses Citrix managed infrastructure to get the job done. Requires no public IP addresses, SSL certs, or ingress firewall rules to operate. Compatible with the Citrix Workspace service and both Citrix Cloud managed and customer managed session brokering options (CVAD and CVADS). |

#### Leading Practices and Recommendations

Whether you manage the Citrix virtualization system yourself or you use Citrix or an authorized partner to do it, consider using **cloud services** wherever possible. For use cases/environments where the cloud service doesn't meet your needs, customer managed components can be used. That said - Citrix encourages customers to be clear on why they are deploying self-managed components, and be prepared to migrate to cloud services once the cloud service meets their specific needs. The cloud services provided by Citrix through Citrix Cloud are evolving rapidly. Over time you can expect them to provide all the functionality required to serve all but the most complex use cases. Citrix Cloud services ultimately minimize the amount of infrastructure the customer is responsible for managing and maintaining. Citrix Cloud also provides highly available, pre-integrated services, and ensures customers always have access to the latest, most secure, and feature-rich services.

### Common Deployment Models for Citrix Virtualization on AWS

As a cloud provider with the most functionality, largest community of customers, unmatched experience, and maturity, AWS sees a wide range of customers from various industries moving systems and infrastructure to their clouds. Over time they've seen common deployment scenarios/migration patterns develop. In this section, we explore these patterns/scenarios, discuss when and where you may want to consider using them to bring a Citrix Virtual Apps and Desktops workload to AWS, and provide some recommendations for which patterns to consider for common migration scenarios.

The three most common scenarios for delivering Citrix Apps and Desktops on AWS are:

-  **Greenfield/Cloud Only** deployment, using Citrix Cloud services with "resource locations" on Amazon EC2 (Amazon Elastic Compute Cloud) service. This scenario is commonly used when customers prefer to go to a subscription model and outsource control plane infrastructure and management responsibility to Citrix, or they're looking to experience/evaluate the capabilities provided by Citrix Cloud services.
-  **Hybrid** deployment/workload migration to AWS, using Citrix Cloud services for session brokering and administration, Workspace UI or StoreFront for content aggregation/session presentation/session launching, and can also use customer managed Citrix ADC/Gateways for HDX session proxying, complex authentication scenarios, or both.
-  **Lift and shift**. With this scenario, customers essentially move or redeploy their self-managed Citrix infrastructure into AWS, treating the deployment on AWS just like their existing customer managed deployment. With this scenario, customers use Citrix ADC/Gateway and Citrix StoreFront to aggregate resources from on-premises and AWS hosted sites. This facilitates the migration of workloads to AWS, though customers can keep their on-premises workloads around and simply add another site in AWS. The new site can be used for new workloads or to support disaster recovery (DR) and failover use cases. This model is characterized by the use of customer managed components for session brokering and administration, UI services, authentication, and HDX session proxy.

This section defines these scenarios in more detail, including architectural overviews of how each scenario is commonly designed.
You find that the **leading practice is to use Citrix Cloud services**, and as such this document will focus on the Citrix Cloud brokered deployment models ("Greenfield" and "Hybrid").

#### Greenfield Deployment

The most common example of the green field deployment model is trial or proof of concept deployments of Citrix virtualization technology on the AWS cloud. Since you're essentially starting from scratch, the power of 'infrastructure as code' can be experienced since you're not trying to integrate with a bunch of existing 'stuff'. It is also a fantastic opportunity to 'play with' various cloud services and evaluate their suitability to your or your customers' needs.

A green field deployment is also the quickest and easiest Citrix virtualization system you can build. You can simply tear it down when the system is no longer needed, and you stop paying for the resources it consumed. All you need for this type of deployment is an active AWS account and either a trial or paid subscription to Citrix Cloud and the Virtual Apps and Desktops Service. Armed with these two resources, you can use AWS' QuickStart CloudFormation templates to build a reference deployment. Citrix and AWS have collaborated on the [Citrix Virtual Apps and Desktops Service on AWS](https://aws.amazon.com/quickstart/architecture/citrix-virtual-apps/) quick start template, which helps you to either build an entire Citrix virtualization system from scratch, or create a Citrix Cloud "Resource Location" in an existing VPC with an existing Active Directory. When deploying the entire Citrix virtualization system from scratch, the resulting system on AWS is built closely matching the following reference architecture diagrams:

![Diagram 3: Deployed system architecture detail using the CVADS on AWS QuickStart template and default parameters. Citrix Cloud Services not shown](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_003.png)
*Diagram 3: Deployed system architecture detail using the CVADS on AWS QuickStart template and default parameters. Citrix Cloud Services not shown.*

![Diagram 4: Greenfield/Cloud Only deployment conceptual architecture with optional AWS services and Citrix Cloud Services](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_004.png)
*Diagram 4: Greenfield/Cloud Only deployment conceptual architecture with optional AWS services and Citrix Cloud Services.*

It is worth noting that this deployment model (actually, all three deployment models) use [AWS Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) to provide a highly available design. See [Availability Zones](#infrastructure-as-code-and-the-aws-object-model) later in this document for more context.

As mentioned previously, this is a great place to start when learning about AWS and Citrix Cloud services. Many of the design patterns depicted in the preceding diagram are used for hybrid and even lift and shift deployment types, so learning these design patterns suits a Citrix on AWS architect well, regardless of the deployment model.

To summarize, the **green field deployment model** uses all cloud services, at least as a starting point:

| Citrix virtualization system component | Provided by: |
|---|---|
| Session brokering and administration | Citrix Virtual Apps and Desktops Service ("CVADS," via Citrix Cloud) |
| UI services | Citrix Workspace service (via Citrix Cloud) |
| Authentication | Citrix Workspace service (via Citrix Cloud) |
| HDX session proxy | Citrix Gateway Service (via Citrix Cloud) |
| VMs compute, networking, and storage | Amazon Elastic Compute Cloud (Amazon EC2), Amazon Virtual Private Cloud (Amazon VPC), Amazon Elastic Block Store (Amazon EBS) |
| Active Directory and file systems | [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/) and [Amazon's FSx for Windows File Server](https://aws.amazon.com/fsx/windows/) (optional) |

We mentioned earlier that the green field deployment model is often used as a starting point for proof of concept and technology trial systems.
If you start with this model and then drop in StoreFront or Citrix ADC/Gateway VPXs in, you're ostensibly creating our next type of deployment model: hybrid.

#### Hybrid Deployment

With the hybrid deployment model, customers may choose to install/configure/manage some of the [Citrix virtualization system components](#citrix-virtualization-system-components) themselves, but not the **session brokering and administration subsystem**. In a hybrid deployment model, this subsystem is provided as a cloud service called "Citrix Virtual Apps and Desktops Service" (CVADS for short), and it is delivered as a subscription from Citrix Cloud.

The hybrid deployment model is the most common deployment seen today, and is the model Citrix recommends for most customers. Here are some of the primary reasons why we take this position:

-  **Simplicity** - With Citrix Cloud services, simplicity is a foundational design tenet. When multiple cloud services are used, they come pre-configured to work together, and when configuration is necessary, workflows and options are dramatically simplified.
-  **Infrastructure and licensing cost savings** - customer managed Citrix virtualization services often require extra hardware and software to support them, and these have costs associated with them.
    One good example is Microsoft SQL Server: customer managed brokering and administration services require databases, and if you're going to build/manage your own, you must provide them. An alternative is using the AWS Relational Database Service (Amazon RDS) for SQL Server.
-  **Autoscaling** - Citrix's managed brokering service (CVADS) includes the [**Citrix Autoscale feature**](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html), which provides built-in VDA capacity and cost management functionality. This feature can save customers a substantial amount of money on infrastructure when they're only paying for what they use. When running a Citrix virtualization workload on AWS, this often means the difference between paying for committed use discounts or paying for VM usage as you go. The cost savings can be dramatic for many use cases, and the Citrix Autoscale feature helps ensure you're only consuming what you need.   **Important note:** *This feature is only available to Citrix Cloud customers (CVADS). It is NOT available to customer managed brokering infrastructure (CVAD LTSR or CR releases).* -  **Management savings** - With cloud services, Citrix shoulders the responsibility for keeping the services highly available, performant, secure, and up to date. You still build and manage your VDAs regardless, but don't underestimate the value of delegating these responsibilities. Cloud services help free up IT resources, allowing them to focus on providing unique value to their businesses instead of these critical but tedious (and often time-consuming) tasks.
-  **"Free" upgrades and continuous innovation** - with customer managed infrastructure, the onus is on the customer to upgrade and patch the components in their care. With Cloud services, most of those work efforts go away. The service providers (Citrix or AWS for example) tend to be constantly innovating, and they bring those innovations to the customers who consume the services, often without requiring any work on behalf of the customer.
-  **Access to more features, functionality, and services** - modern service delivery platforms (such as Citrix Cloud and AWS EC2) give technology providers a powerful, cost-effective way to bring new features, capabilities, and services to market that would not otherwise be possible. Vendors such as Citrix are committed to meeting the customer wherever they are at in their digital transformation journey, but sometimes the only way to cost-effectively deliver new capabilities is to deliver them as a cloud service.
-  **Flexibility** - with CVADS as the foundation of this deployment model, customers can mix and match customer managed or cloud service components of the Citrix virtualization system. This allows the system to meet various different use cases and support complex enterprise requirements for a Citrix virtualization system. We explore these choices in depth in a later section of this paper.

To summarize, the **hybrid deployment model** uses the following:

| Citrix virtualization system component | Provided by: |
|---|---|
| Session brokering and administration | Citrix Virtual Apps and Desktops Service ("CVADS," via Citrix Cloud) |
| UI services | Citrix Workspace service (via Citrix Cloud) **OR** Citrix StoreFront on Amazon EC2 (customer managed) |
| Authentication | Citrix Workspace service (via Citrix Cloud) **OR** Citrix StoreFront on EC2 (Citrix ADC/Gateway optional but common) |
| HDX session proxy | Citrix Gateway Service (via Citrix Cloud) **OR** Citrix ADC/Gateway VPX on Amazon EC2 (Citrix ADC/Gateway optional but common) |
| VMs compute, networking, and storage | Amazon Elastic Compute Cloud (Amazon EC2), Amazon Virtual Private Cloud (Amazon VPC), Amazon Elastic Block Store (Amazon EBS) |
| Active Directory and file systems | [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/) and [Amazon's FSx for Windows File Server](https://aws.amazon.com/fsx/windows/) (optional) |

Given the options a customer can choose in the hybrid deployment model, and the flexibility provided by customer managed components, there isn't one, succinct architecture that fits all customers. There are, however, some common design patterns that can also be mixed/matched to suit the customers' needs. The foundational pattern, however, is the pattern for a Citrix Cloud "Resource Location" on AWS. It is also the pattern built by the [Citrix Virtual Apps and Desktops Service on AWS](https://aws.amazon.com/quickstart/architecture/citrix-virtual-apps/) QuickStart template, and it looks similar to the following architectural diagram:

![Diagram 5: Conceptual Architecture, CVADS - Hybrid Deployment Model on AWS](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_005.png)
*Diagram 5: Conceptual Architecture, CVADS - Hybrid Deployment Model on AWS.*

It is worth noting that this deployment model also uses [AWS Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) to provide a highly available design. See [Availability Zones](#infrastructure-as-code-and-the-aws-object-model) later in this document for more context.

It is also worth noting that the hybrid deployment model (a CVADS resource location on AWS) can be combined with a hybrid cloud model, connecting customer managed data centers/resources to AWS using AWS Direct Connect, AWS VPN, Citrix SD-WAN, or other networking tools. With this model, the customers' existing Active Directory is often extended into AWS, and customers create more Citrix Cloud 'Resource Locations' which deliver apps, desktops, and resources from the customer managed data center. The resulting conceptual architecture looks something like the following diagram:
![Diagram 6: Conceptual Architecture, CVADS: Hybrid Deployment/Hybrid Cloud Model](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_006.png)
*Diagram 6: Conceptual Architecture, CVADS: Hybrid Deployment/Hybrid Cloud Model.*

#### Lift and Shift

Referring to our definition of the [Citrix virtualization system components](#citrix-virtualization-system-components), when we're talking about a lift and shift deployment scenario, the key component is the **session brokering and administration** subsystem and associated infrastructure. If you're using **self-managed brokering infrastructure** (you're deploying Delivery Controllers instead of Cloud Connectors) then for the purposes of this paper **you're lifting and shifting.**

##### Lift and Shift - why

Despite Citrix guidance against this model, some customers still choose to go with this model and deploy/manage the Citrix virtualization system components themselves. Per [CTX270373](https://support.citrix.com/article/CTX270373), the use of public clouds including AWS is only supported with LTSR product versions. For customers who do choose the lift and shift (self-managed) deployment model, we often find that non-technical reasons are behind it. Politics, time pressures, fear of the unknown, perceived skills deficits, loss of control, and license acquisition fall into this category. There are, however, a few technical reasons why this model is appealing. These include:

-  **System Isolation** - some use cases, such as air-gapped systems with no Internet access, often make the lift and shift model appealing. Since cloud services require outbound Internet access to function, in a strictly air-gapped deployment, cloud services won't function. This mainly applies to the Cloud Connectors (primary component of managed session brokering services) as they need outbound Internet connectivity to communicate with and utilize the Citrix Cloud services. Some customers can consider utilizing a secure, outbound proxy for Cloud Connectors (while keeping all other infrastructure strictly air-gapped). This is often a suitable concession which allows the managed brokering services to be utilized, but even this may not be an option for some customers and use cases.
-  **Configuration flexibility** - one person's 'complex' is another person's 'flexible', and flexibility has been a strong suite of customer managed Citrix virtualization infrastructure for more than two decades. Over the years the technology has gained a ton of features that support some very niche use cases and third-party integrations. The Citrix Cloud services focus on simplicity and pre-integration. In doing so, some of these niche features and integrations are not available. Therefore, some edge cases are still best served by a customer managed stack. That said, given the rapid pace of innovation coming to the Citrix Cloud services, these edge cases are becoming increasingly rare.
-  **Control** - some organizations, cultures, and business models demand as much control as possible. With customer managed Citrix virtualization components, customers can completely own their destiny. This control comes at a cost (infrastructure, complexity, personnel, and such) but "control at all cost" is a thing for some customers.

To summarize, the **lift and shift deployment model** uses the following:

| Citrix virtualization system component | Provided by: |
|---|---|
| Session brokering and administration | Citrix Virtual Apps and Desktops (customer managed using LTSR or CR downloadable) on Amazon EC2 |
| UI services | Citrix StoreFront on Amazon EC2 (customer managed) |
| Authentication |Citrix StoreFront on EC2 (Citrix ADC/Gateway optional but common) |
| HDX session proxy | Citrix ADC/Gateway VPX on Amazon EC2 (customer managed) |
| VMs compute, networking, and storage | Amazon Elastic Compute Cloud (Amazon EC2), Amazon Virtual Private Cloud (Amazon VPC), Amazon Elastic Block Store (Amazon EBS) |
| Active Directory and file systems | Customer managed Windows Server instances on EC2; [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/) and [Amazon's FSx for Windows File Server](https://aws.amazon.com/fsx/windows/) (optional) |

In its simplest form, a lift and shift deployment of Citrix virtualization technology onto AWS resembles a traditional customer managed deployment on-premises. It uses a CVAD 'site' deployed into an AWS region and uses basic AWS IaaS services such as EC2 virtual machines and VPC networking at a minimum. As mentioned previously, it requires the customer to build/configure/maintain all system components, plus supporting services such as SQL databases. The following diagram depicts this deployment model:
[Diagram 1: Conceptual Architecture, CVAD: Lift and Shift Deployment Model on AWS](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_001.png)
*Diagram 1: Conceptual Architecture, CVAD: Lift and Shift Deployment Model on AWS.*

It is worth noting that this deployment model also uses [AWS Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) to provide a highly available design. See [Availability Zones](#infrastructure-as-code-and-the-aws-object-model) later in this document for more context.

A lift and shift deployment model is often combined with a hybrid cloud infrastructure model, using AWS Direct Connect, AWS VPN, Citrix SD-WAN, or similar networking technology to connect a customer managed data center and resources to AWS. Customers can optionally adopt some of AWS' more advanced cloud services (to provide a measure of simplification with the transition), and they may also choose to host some services (such as SQL databases, Citrix licensing, Citrix StoreFront, and Citrix ADC/Gateway) either on AWS, in a customer managed data center, or both depending upon their existing investments, use case requirements, and such. A conceptual architecture of this deployment model (using AWS RDS for SQL Server or on-premises SQL server) is shown in the following diagram. Only one active instance of Citrix Licensing is needed, but we've shown multiple to depict available options:
[Diagram 8: Conceptual Architecture, CVAD: Lift and Shift Deployment Model with Hybrid Cloud infrastructure model and AWS managed cloud services](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_008.png)
*Diagram 8: Conceptual Architecture, CVAD: Lift and Shift Deployment Model with Hybrid Cloud infrastructure model and AWS managed cloud services.*

#### Lift and Shift - why not

By now you've gathered that the Citrix leading practice/recommendation is to NOT do a full lift and shift. You can be wondering why, or where this is coming from. Referring to our breakdown of [Citrix virtualization system components](#citrix-virtualization-system-components), the **session brokering and administration** subsystem is the most critical component you want to consider NOT lifting and shifting. We strongly recommend customers consider using Citrix's cloud services for session brokering and administration (deploy Cloud Connectors only, vs. deploying Delivery Controllers + SQL databases + Director servers + Citrix Licensing servers). Here are some of the primary reasons why we take this position (and they might sound familiar):

-  **Simplicity** - While customer-managed session brokering services
    provide the ultimate in control and configuration flexibility, it comes at the cost of complexity and ongoing maintenance requirements. With Citrix Cloud services, simplicity is a foundational design tenet. When multiple cloud services are used, they come pre-configured to work together, and when configuration is necessary, workflows and options are dramatically simplified.
-  **Infrastructure and licensing cost savings** - customer managed
    Citrix virtualization services often require extra hardware and software to support them, and these have costs associated with them.
    One good example is Microsoft SQL Server: customer managed brokering services require databases, and if you're going to build/manage your own, you must provide them.
-  Speaking of infrastructure cost savings - this brings up a critical
    differentiator between the two session brokering options:
    **Autoscaling**. Citrix's managed brokering service (CVADS) includes the [**Citrix Autoscale feature**](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html), which provides built-in VDA capacity and cost management functionality. This feature can save customers a substantial amount of money on infrastructure when they're only paying for what they use. When running a Citrix virtualization workload on AWS, this often means the difference between paying for committed use discounts or paying for VM usage as you go. The cost savings can be dramatic for many use cases, and the Citrix Autoscale feature helps ensure you're only consuming what you need. **Important note:** *This feature is only available to Citrix Cloud service (Citrix Virtual Apps and Desktops service) customers - it is not available to customer managed brokering infrastructure (Citrix Virtual Apps and Desktops LTSR or CR releases).* -  **Management savings** - With cloud services, Citrix (and AWS in this case) shoulders the responsibility for keeping the services highly available, performant, secure, and up to date. You still build and manage your VDAs regardless, but don't underestimate the value of delegating these responsibilities. Cloud services help free up IT resources, allowing them to focus on providing unique value to their businesses instead of these critical but tedious and often time-consuming tasks.
-  **"Free" upgrades and continuous innovation** - with customer
    managed infrastructure, the onus is on the customer to upgrade and patch the components in their care. With cloud services, most of those work efforts go away. The service providers (Citrix and AWS in this case) tend to be constantly innovating, and they bring these innovations to the customers who consume the services, often without requiring any work on behalf of the customer.
-  **Access to more features, functionality, and services -**
    modern service delivery platforms (such as Citrix Cloud and Amazon EC2) give technology providers a powerful, cost-effective way to bring new features, capabilities, and services to market that wouldn't otherwise be possible. Vendors such as Citrix are committed to meeting the customer wherever they're at in their digital transformation journey, but sometimes the only way to cost-effectively deliver new capabilities is to deliver them as a cloud service.

#### Lift and Shift - more resources

Before Citrix Cloud services were born, customers were already successfully deploying Citrix virtualization technologies on AWS. In those days Citrix called the Virtual Apps and Desktops products XenApp and XenDesktop. Extensive work went into creating and publishing reference architectures and deployment guides for this deployment scenario. A good portion of the detail in these aging resources still applies to deployments who must go down this road today.

For customers who MUST go down this route, the following published resources provide you with useful background detail you can use to help you be successful. We recommend reviewing these materials before you continue on with this document, as we are highlighting important design decisions that have changed since these works were completed:

-  [Citrix XenApp and XenDesktop 7.6 on AWS Reference Architecture](https://www.citrix.com/blogs/2015/06/18/xenapp-and-xendesktop-7-6-reference-architecture-on-aws/) (blog)
-  [Citrix XenApp and XenDesktop 7.6 on Amazon Web Services Reference Architecture](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/citrix-xenapp-and-xendesktop-7.6-on-amazon-web-services-reference-architecture.pdf) (guide)
-  [Using AWS Directory Service and Amazon RDS with Citrix Virtual Apps and Desktops](https://aws.amazon.com/blogs/apn/using-aws-directory-service-and-amazon-rds-with-citrix-virtual-apps-and-desktops/) (blog)
-  [Deploying Citrix Virtual Apps and Desktop with AWS Directory Service and Amazon RDS Version 1.0](https://s3-us-west-2.amazonaws.com/apnblog.awspartner.com/Citrix+Virtual+Apps+and+Desktops/Citrix+Ready-Amazon+RDS+Deployment+Guide_v1.pdf) (deployment guide)

## Design Decisions

This section explores key design decisions to consider as you're architecting your Citrix virtualization system on AWS. We walk down through each layer of the Citrix Architectural Design Framework, exploring key areas for you to consider.

### About the Citrix Architectural Design Framework

Citrix's Virtual Apps and Desktops solution (the product family name collectively referring to Citrix's virtualization technologies) enables organizations to create, control and manage virtual machines, deliver applications and desktops, and implement granular security policies. The Citrix Virtual Apps and Desktops solution provides a unified framework for developing a complete digital workspace offering. This offering enables Citrix users to access applications and desktops independent of their device's operating system and interface.

The Citrix architectural design framework is based on a unified and standardized layer model. It provides a consistent and easily accessible framework for understanding the technical architecture for most of the common Virtual Apps and Desktops deployment scenarios. These layers are depicted in the following conceptual diagram:
[Diagram 9: Conceptual Architecture, Citrix Virtual Apps and Desktops Service](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_009.png)
*Diagram 9: Conceptual Architecture, Citrix Virtual Apps and Desktops Service.*

-  ***User Layer*** - This layer defines user groups and locations of
    the Citrix environment.
-  ***Access layer*** - This layer defines how users access the
    resources.
-  ***Control layer*** - This layer defines the components that control
    the Citrix solution.
-  ***Resource layer*** - This layer defines provisioning of Citrix
    workloads and how resources are assigned to the given users.
-  ***Platform layer*** - This layer defines the physical elements
    where the hypervisor components and cloud service provider framework run to host the Citrix workloads.
-  ***Operations Layer*** - This layer defines the tools that support
    the delivery of the core solutions.

### User Layer Considerations

In the Citrix Architectural Design Framework, the User layer describes the user groups, their locations, specific requirements, and more. The user layer appropriately sets the overall direction for each user group's environment. This layer incorporates the assessment criteria for business priorities and user group requirements to define effective strategies for endpoints and Citrix Workspace App. These design decisions affect the flexibility and functionality for each user group.

When designing and deploying a Citrix virtualization system on any platform, the decisions and strategies adopted after careful assessment set the foundation for many other decisions that customers ought to consider as they work their way down through the other layers in the Citrix Architectural Design Framework. As such, this is a critical layer to understand thoroughly and get right.

Fortunately, these considerations are already well documented, and don't differ substantially between systems deployed on AWS and any other hardware/cloud platform. For a thorough primer on the design considerations for this layer, see [Citrix VDI Handbook and Best Practices for XenApp and XenDesktop 7.15 LTSR](/en-us/xenapp-and-xendesktop/7-15-ltsr/downloads/handbook-715-ltsr.pdf) document, and the [Design methodology user layer](/en-us/xenapp-and-xendesktop/7-15-ltsr/citrix-vdi-best-practices/design/design-userlayer1.html) page in particular, for details.

### Access Layer Considerations

In the Citrix Architectural Design Framework, the Access layer defines how users access AWS resources. The design of your access layer is critical to the functionality delivered by any Citrix virtualization system. It controls how users authenticate to the system. It also controls how users view and launch virtualized applications and desktops, plus what type of applications and content are available to them. Also, the Access layer controls how and when sessions are securely proxied or directly connected.

In the context of the [Citrix virtualization system components](#citrix-virtualization-system-components) we defined earlier, the access layer contains the following components and choices:

| Citrix virtualization system component | Provided by: |
|---|---|
| UI services | Citrix Workspace (provided by Citrix Cloud) **OR** Citrix StoreFront on Amazon EC2 (customer managed) |
| Authentication | Citrix Workspace service (Citrix ADC/Gateway optional) **OR** Citrix StoreFront on EC2 (Citrix ADC/Gateway optional but common) |
| HDX session proxy | Citrix Gateway Service (provided by Citrix Cloud) **OR** Citrix ADC/Gateway VPX on Amazon EC2 (customer managed) |

The following table contains critical decision points when determining which access layer component to deploy, but the choice is not binary. Citrix supports various different access methods that can be customized to suit your needs.

#### UI Service and Authentication Considerations

Consider the following when choosing how you want to provide UI services for your Citrix virtualization system on AWS:

| Attribute / Capability | Customer Managed (installed from downloaded binaries) | Cloud Service (delivered via Citrix Cloud) |
|---|---|---|
| Ability to present and launch virtualized apps and desktops from multiple "Citrix Farms." | **YES** - Both legacy environments (XenApp and XenDesktop 6.5/7.x, Citrix Virtual Apps and Desktops CR/LTSR) and Citrix Virtual Apps and Desktops Service. | **YES** - Both legacy environments (XenApp and XenDesktop 6.5/7.x, Citrix Virtual Apps and Desktops CR/LTSR) and Citrix Virtual Apps and Desktops Service. See [this article](/en-us/citrix-cloud/workspaces/add-on-premises-site.html) for more details. |
| Ability to create multiple 'Stores' with different settings for different use cases, including authentication requirements. | **YES** - StoreFront can be configured with multiple different Stores, and when combined with Citrix ADC/Gateway VPX, can apply sophisticated rules to direct certain devices or user groups to different stores. For more information, see [How SmartAccess Works for Citrix Virtual Apps and Desktops](/en-us/citrix-gateway/current-release/integrate-citrix-gateway-with-citrix-products/integrate-web-interface-apps/ng-smartaccess-wrapper-con/ng-smartaccess-how-it-works-con.html). One common scenario requiring two StoreFront Stores would be when users require published applications from inside a published desktop. Another common scenario would be a requirement of having an internal only Store (no Citrix Gateway access) for a specific use case and another Store configured for both internal and remote access. See [Configure and manage stores](/en-us/storefront/current-release/configure-manage-stores.html) for more information. | **NO** - the Workspace Service is essentially a single store, on a single URL. All users use the same store and Workspace settings. Authentication requirements are set up once, and apply to all users of the Workspace tenant. |
| Ability to enumerate, launch, and SSO into SaaS and web apps using the Citrix Secure Private Access service, taking advantage of web-filtering and enhanced security control policies, plus advanced, ML enhanced analytics. | **NO** - Requires use of Citrix Workspace. | **YES** - With the Citrix Gateway Service, it is as simple as 'turning on' the integration in the Citrix Cloud Console. SaaS apps are defined simply from a web based wizard, and admins can use a substantial list of pre-defined apps as a starting point. |
| Ability to access/index/search Citrix Files (formerly ShareFile) content through the Citrix Workspace app and web browsers (HTML). | **NO** - StoreFront does not have the ability to integrate file-based content into either Workspace App or StoreFront HTML UIs. | **YES** - Enabled by default depending upon the subscription to Citrix Cloud. Brings users' file-based content from various sources (including on-premises file shares) into the Workspace UI, both HTML and Workspace App. |
| Ability to present and launch connections to physical desktops using the [Citrix Remote PC Access](/en-us/citrix-virtual-apps-desktops/install-configure/remote-pc-access.html) feature. | **YES** - Regardless of whether brokering is handled by CVAD or CVADS. | **YES** - Regardless of whether brokering is handled by CVAD or CVADS. |
| Ability to guide users' work and automate repetitive tasks by using microapps integrated with various different SaaS and custom applications. | **NO** - The microapps feature is not currently available with Citrix StoreFront. | **YES** - Presents users with a 'feed' in the Workspace UI that intelligently surfaces notifications, tasks, and workflows defined by in-box and custom microapps and the low code Microapp builder console. See [Workspace Intelligence Features - Microapps](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html) and [Microapps](/en-us/citrix-microapps) on Citrix Docs for more information. |
| For multi-site and DR use cases, ability to granularly control session launch behavior using Zone Preference and Failover. | **YES** - Using Citrix Zones for deployments across multiple AWS Regions and Availability Zones is a great way to expand east-west and limit the affected user base in the case of an outage, and allows for Region preference and failover to the Primary Zone seamlessly. See [CVAD Zones documentation](/en-us/citrix-virtual-apps-desktops/manage-deployment/zones.html). | **Partial** - Workspace service doesn't include the fully featured zone preference and failover functionality, but a similar effect can be implemented using home zones for users or apps. See [CVADS Zones documentation](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html) for details. |
| Ability to broker new and existing connections when a connection between a resource location/zone and Citrix Cloud fails, or when the databases underneath Citrix Delivery Controllers are unavailable. | **YES** - Uses the Local Host Cache feature on both Cloud Connectors and Delivery Controllers to provide resiliency for these two potential failure scenarios. **For environments with extensive resiliency requirements, Citrix recommends deploying StoreFront with Local Host Cache.** For more information see [Local Host Cache (CVAD)](/en-us/citrix-virtual-apps-desktops/manage-deployment/local-host-cache.html). | **YES** - Cloud Connectors use the Local Host Cache feature to broker resource connections in the event of Citrix Cloud communication failure. This requires passive StoreFront servers accessible by your resource locations to handle failover scenarios. For more information see [Local Host Cache (CVADS)](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html). |
| Ability to configure and utilize a customized 'vanity URL' for end-user consumption. | **YES** - Customer has full control of the URL's and certificates used and presented to users. Does require SSL certificates, DNS alias creation/management, and Citrix ADC/Gateway instances for ingress over public networks. | **Partial** - All Workspaces are delivered from the cloud.com domain, though customers can [configure their own customized prefix](/en-us/citrix-cloud/workspace-configuration.html) (customername.cloud.com). |
| Ability to intelligently route on-network users directly to VDAs and off-network users through Citrix ADC/Gateway VPX or Citrix Gateway Service. | **YES** - StoreFront uses administrator defined 'beacons', which the Citrix Workspace app uses to determine if a user is on or off-network. | **Coming Soon** - This feature is expected to be available on Citrix Workspace with the release of the Network Location Service once it is generally available. For more information, see [Network Location Service preview](/en-us/citrix-cloud/workspace-network-location.html). |
| Ability to use Citrix Gateway Service for simple, pre-configured HDX session proxy services. | **NO** - If off-network access to Citrix virtualized apps is a requirement (and it is in 99% of deployments), StoreFront requires the use of customer managed Citrix ADC/Gateway for HDX session proxy functionality. | **YES** - This feature is provisioned and enabled by default for all new Citrix Workspace tenants. |
| Includes built-in multifactor authentication via Active Directory and TOTP. | **YES** - Citrix ADC includes built-in TOTP functionality for use with third-party authenticators, and also supports third-party apps/devices/services. | **YES** - Citrix Workspace includes this feature, including self-service OTP device recovery and automatic push notifications to end users. Supports both Citrix and third-party authenticator apps. |
| SSO capabilities (Virtualized, SaaS, and Web apps) | **Partial** - SSO to virtualized apps out of the box. | **YES** - SSO to virtualized, SaaS, and web apps natively available with Citrix Workspace. Gateway Service and Secure Private Access include Web Filtering and Policy Controls. |
| Ability to choose from multiple pre-defined authentication methods and have the chosen method apply to all users on the system. | **YES**  - with more options and flexibility. Citrix StoreFront allows you to create multiple Stores, and authentication methods are configured on a per store basis. One or more options can be configured per store, and admins can select from Active Directory username/password, SAML authentication, domain pass-through, Smart Card, HTTP Basic, and Pass-through from Citrix Gateway options. Self service password reset can also be enabled. See [Configure the authentication service](/en-us/storefront/current-release/configure-authentication-and-delegation/configure-authentication-service.html) for more information. When Citrix ADC/Gateway (customer managed) is deployed and used with StoreFront, a various authentication options can be configured, along with extra logic to direct users to a specific Store as needed to support almost any use case.  Citrix StoreFront and Citrix ADC/Gateway are recommended where complex integrations and different authentication methods are required for different use cases. | **YES** - Currently, Active Directory, Azure AD, Active Directory + TOTP Token, Azure AD, and Citrix Gateway are currently supported options. Okta and Google Cloud ID options are in preview or coming soon. See [Secure workspaces](/en-us/citrix-workspace/secure.html) for more information. Except for Citrix Gateway, your authentication choice applies to all users and all services provided through the Citrix Workspace tenant/URL. With the Citrix Gateway option, customers can support various authentication options (RADIUS MFA, smart card, federation, conditional access policies, and more) and flexibly apply them to different groups of users and use cases. For more information, see [Connect an on-premises Citrix Gateway as an identity provider to Citrix Cloud](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/connect-ad-gateway.html). |
| Ability to SSO to sessions on VDAs when launching virtualized Windows apps/desktops using federated identity providers | **YES** - Citrix [Federated Authentication Service](/en-us/federated-authentication-service.html) (FAS) enables SSO to VDAs when using a federated identity provider such as SAML (Security Assertion Markup Language). | **Coming Soon** - By using Citrix [Federated Authentication Service](/en-us/federated-authentication-service.html) (FAS) with Citrix Workspace. This feature is in preview as of this writing. See [Enable SSO for Workspaces with Citrix FAS](/en-us/citrix-workspace/workspace-federated-authentication.html) for more information. |

#### HDX Session Proxy Considerations

Consider the following when choosing how you want to provide HDX session proxy functionality for your Citrix virtualization system on AWS:

| Attribute / Capability | Customer Managed (Citrix ADC/Gateway VPX on AWS) | Cloud Service ([Citrix Gateway Service](/en-us/citrix-gateway-service.html) provided by Citrix Cloud) |
|---|---|---|
| Simple, pre-configured service, providing HDX proxy with no administrative overhead | **NO** - As a customer managed component, these appliances require licensing, installation, configuration, and maintenance. | **YES** - [Citrix Gateway Service](/en-us/citrix-gateway-service.html) is a complete HDX proxy solution, managed by Citrix, delivered as a cloud service. |
| Ability to use Citrix HDX's EDT (UDP) based transport protocol. For more information, see [Adaptive Transport](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/adaptive-transport.html) and [How to Configure HDX Enlightened Data Transport Protocol](https://support.citrix.com/article/CTX220732). | **YES** - This feature optimizes traffic from high-latency sites and is available for customer managed ADC/Gateway instances. | **Not Yet** - This feature is in preview as of this writing. The Gateway Service currently only supports TCP based connections to VDAs. |
| Ability to provide load balancing, health checking, SSL offload, and various other advanced networking and application delivery services for customer managed infrastructure. | **YES** - Citrix ADC/Gateway VPX appliances provide sophisticated, industry leading capabilities, many of which can be enabled by simply applying the appropriate type of license to the appliance. | **NO** - For Citrix CVAD and CVADS brokered environments, the Gateway Service provides simple, secure access to virtualized applications running either in customer's AWS or on-prem environments. |
| Support for customer configurable Global Server Load Balancing (GSLB) between data centers, zones, and regions. | **YES** - Customer managed Citrix ADC/Gateway instances can be set up for GSLB, though the customer is responsible for setup and management. | **NO** - ...however there is no real need for it: the Gateway Service uses [14 or more POP's worldwide plus integrated GSLB](https://www.citrix.com/blogs/2019/07/02/new-citrix-gateway-pops-now-available-in-india-and-south-africa/) to ensure users get the best possible session performance regardless of where in the world they are. |
| Requires use of Citrix Workspace UI for HDX session presentation and launching. | **NO** - It is possible to use customer managed Citrix ADC/Gateway VPX instances with both Workspace UI and StoreFront. | **YES** - The Gateway Service is only configurable through the Citrix Workspace UI for HDX proxy - it does NOT provide HDX proxy capabilities for Citrix StoreFront. |
| Requires extra resources on Cloud Connector instances to proxy sessions into secured networks. | **NO** - While Cloud Connectors perform STA ticket validation for customer managed Citrix ADC/Gateway VPX instances, no additional resources are needed since all HDX sessions are proxied through the VPXs. | **YES** - Today the Gateway Service uses long lived, outbound TCP connections from the Cloud Connector instances to Citrix Cloud to proxy HDX traffic back into private networks. This requires extra resource considerations when sizing and configuring Cloud Connector instances. See [this article](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-cloud-connector/cc-scale-and-size.html) for more details. **Note** - this requirement is moot for most use cases once the Gateway Service and VDAs can use the Rendezvous protocol/feature. This requires Citrix VDA 1912 or newer. |
| Ability to be used with Citrix Cloud Government tenants. | **YES** - Both on-prem and AWS EC2-based ADC/Gateway/StoreFront deployments are supported. | **YES** - Citrix Workspace is available in Citrix Cloud Government. |
| Ability to support air-gapped AWS clouds/environments with no outbound internet connectivity. | **YES** - Customer-managed deployments of ADC/Gateway (and StoreFront) are supported for both on-prem and AWS EC2-based instances. | **NO** - Air-gapped AWS environments have no access to Citrix Cloud or Citrix Cloud Government, therefore Gateway Service and Workspace Service are currently not available. |

#### Summary, Recommendations, and Leading Practices

Now that we've reviewed some of the attributes/features/capabilities that help drive your customer managed vs. cloud service decisions for the Access Layer subsystems, let's examine the top level decisions in the context of the [deployment models](#common-deployment-models-for-citrix-virtualization-on-aws) we defined earlier.

##### Access Layer: Greenfield/Cloud Only Deployment

Since the green field or cloud only deployment model use cloud services across the board, the AWS specific implications on the design of your Citrix virtualization system are simple: there aren't any.
It's not necessary build or configure anything on AWS since everything required for both UI and HDX proxy services is provided for you, configured and ready to go 'out of the box'.

The Access Layer of a Citrix deployment is a key requirement for delivering virtual apps and desktops to users. If an access point is unreachable or fails, users cannot access their resources. Network design and implementation can be complicated, but with Citrix Gateway Service and Citrix Workspace, redundancy, failover, maintenance, and global presence are all part of the package - with no networking knowledge required.Using the Citrix Gateway Service and Citrix Workspace can reduce your infrastructure footprint substantially. By moving the access layer to a cloud services model, users can securely access network resources from anywhere in the world. This approach requires the least deployment and maintenance efforts, so it is a great option if you want to get up-and-running quickly, have a limited IT staff, or if infrastructure is not your focus. With everything pre-configured, this deployment model is the least customizable, but for deploying a simple, secure, fully functional, globally accessible system, using Citrix Workspace and Gateway Service for your access layer is the way to go.

##### Access Layer: Hybrid Deployment

With the hybrid deployment model, you're going to be building/managing some of the Citrix virtualization system components, otherwise it is a green field or cloud only deployment by definition. With the hybrid model, you're possibly deploying Citrix ADC/Gateway VPXs on AWS or even on-premises, and depending upon your requirements, you might also be deploying Citrix StoreFront on AWS or on-premises. Customers who have made significant investments in their on-premises Gateway and identity solutions can benefit from the ability to use [Citrix Gateway as the identity provider for Workspace](/en-us/citrix-adc/current-release/aaa-tm/use-citrix-gateway-as-idp-for-citrix-cloud.html).

This deployment model is common for security-focused deployments, deployments with current on-prem infrastructure (ADC or StoreFront), and for DR/failover sites for existing customer managed data centers. One of the key considerations for this model is keeping your users, resources, and access points as close together as possible.
Choose AWS regions near the on-prem resource locations in which to deploy your Access Layer. Where possible, keep your ADCs and StoreFront servers as close as possible to each other. This is where things can get tricky. Consider the [Citrix Virtual Apps and Desktops launch sequence](https://www.basvankaam.com/2016/12/19/demystifying-the-citrix-xenapp-logon-enumeration-and-launch-steps-new-details-included/) when designing your hybrid deployment, noting especially that all traffic is routed through the Citrix ADC.

With Citrix ADC/Gateway and StoreFront as EC2-based instances in AWS, there is also much more potential for customization. In addition to the multiple StoreFront stores, multifactor authentication, and various industry-leading ADC features, hybrid deployments can also use native AWS services such as the Relational Database Service (RDS) and AWS Directory Services. Hybrid deployments lend well to a more gradual cloud transition and leave room for adjustments to the architecture along the way, as opposed to lift, and shift methods.

The hybrid approach does require a higher level of expertise and increased lead time to deploy than the greenfield/cloud only model, but can serve as a solid transition state between a traditional customer managed/on-prem deployment and cloud only state.

##### Access Layer: Lift and Shift Deployment

With the legacy lift and shift deployment model, you're deploying both Citrix ADC/Gateway VPXs and Citrix StoreFront on AWS, or potentially reusing existing on-premises deployments of these technologies for the same purpose. This type of deployment tends to have the least lead time for customers with existing on-prem Citrix virtualization environments, and is also the easiest transition from an Operations and Maintenance perspective. Staff with experience managing an on-prem environment has a shorter ramp-up time with the lift and shift deployment model, as the Citrix infrastructure remains largely unchanged. For the access layer specifically, this method is straightforward and allows for many customizations. The lift and shift is a great first step for existing deployments going into the cloud or for new or air-gapped AWS regions, but may be a hindrance to adopting a cloud-forward architecture in the future.

### Citrix ADC/Gateway VPX on AWS

Deploying the Citrix ADC/Gateway on AWS is different than deploying it on-premises, though in the end you're managing them yourself.
Fortunately deploying Citrix ADC/Gateway on AWS is thoroughly documented, so we recommend reviewing the following resources before you solidify your design and begin implementation:

-  [Citrix ADC VPX on AWS in Citrix Docs](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws.html): Provides a comprehensive overview of Citrix ADC on AWS, including supported VPX models, AWS regions, EC2 instance types, and extra resource references.
-  [Citrix ADC and Amazon Web Services Validated Reference Design](/en-us/advanced-concepts/design-guides/netscaler-and-amazon-aws.html) in Citrix Docs/Advanced Concepts - includes more details and deployment guidance.
-  [Citrix ADC for Web Applications Quick Start CloudFormation Template](https://aws.amazon.com/quickstart/architecture/citrix-adc-vpx/): functional and relevant for Citrix Gateway deployments with CVAD/CVADS also, though Citrix and AWS are working on an extra Quick Start for this specific use case.
-  [Read.me](https://github.com/citrix/citrix-adc-aws-cloudformation/blob/master/templates/README.md) from Citrix ADC on AWS QuickStart CloudFormation template - an excellent reference for available Citrix Networking products on AWS Marketplace, and also provides AMI IDs for all available regions/instance types/firmware models.

While there are potential variants for a Citrix ADC/Gateway VPX architecture on AWS, the following diagram (from the [Citrix ADC for Web Applications Quick Start Deployment Guide](https://aws-quickstart.s3.amazonaws.com/quickstart-citrix-adc-vpx/doc/citrix-adc-vpx-for-web-applications-on-the-aws-cloud.pdf)) depicts a multi-AZ Citrix HA pair deployment as deployed by the Quick Start template (with default subnets/CIDR blocks):

[Diagram 10: Conceptual Architecture, Citrix ADC/Gateway VPX on AWS with HA across Availability Zones](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_010.png)
*Diagram 10: Conceptual Architecture, Citrix ADC/Gateway VPX on AWS with HA across Availability Zones.*

As discussed in [Citrix ADC VPX on AWS](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws.html) on Citrix Docs, there are two primary deployment options available. They are:

-  **[Standalone](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws/launch-vpx-for-aws-ami.html)**: Individual instances of Citrix ADC/Gateway can be deployed and managed as separate entities. This is commonly used for smaller
    scale or POC deployments where high availability is not a requirement.

-  **[High Availability](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws/how-aws-ha-works.html)**: This is the most commonly deployed model for production
    environments: pairs of Citrix ADC/Gateway VPX instances can be deployed using native Citrix HA mode on AWS. With older firmware versions, the pair is deployed in the same AWS Availability Zone. [Starting with Citrix ADC 12.1](/en-us/advanced-concepts/design-guides/netscaler-and-amazon-aws.html) firmware, highly available pairs of VPX appliances can be deployed across Availability Zones (AZ). [How high availability on AWS works](/en-us/citrix-adc/current-release/deploying-vpx/deploy-aws/how-aws-ha-works.html) explains the difference between deploying a pair of ADCs within the same AZ and across AZs. We dig into this option more deeply later in this section.

While Citrix ADC VPX generally supports single, dual, or multiple NIC deployment types, Citrix recommends using at least three subnets for each ADC when deployed on AWS, with a network interface in each subnet for optimum throughput and data separation. When deployed to support Citrix Virtual Apps and Desktops, the NSIP is typically attached to the "Private Citrix Infrastructure Subnet," the SNIP is attached to the "Private Citrix VDA Subnet," and the Citrix Gateway VIP to the "Public Subnet." The following simplified conceptual diagram depicts this configuration. It shows a single VPX instance in a single AZ - this design pattern would be duplicated (likely in a second AZ) for a High Availability configuration:

[Diagram 11: Citrix ADC VPX instance interface mapping for CVAD/CVADS deployments](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_011.png)
*Diagram 11: Citrix ADC VPX instance interface mapping for CVAD/CVADS deployments.*

#### ADC High Availability across Availability Zones

As mentioned earlier, this is the most common deployment model for Citrix virtualization systems. This model uses a pair of Citrix ADC VPXs deployed across Availability Zones by either using Citrix ADC's native HA (active/passive) feature or a combination of Citrix ADC's native Global Service Load Balancing (GSLB) and IPSet features.
The latter option (which became feasible in early 2020) allows for an active/active configuration across AZs, and functions by allowing the ADC to act as an authoritative DNS source. This new option/architecture is expected to be popular for public cloud deployments, so we focus on that here.

The Domain Based Services for cloud load balancers allow for AutoDiscovery of dynamic cloud services. By deploying Citrix ADCs across multiple AZs in an active-active configuration, you can use cloud resources in different AZs to optimize High Availability/Disaster Recovery. Each AZ can contain cloud resources in the familiar [Pod Infrastructure](https://www.citrix.com/blogs/2017/09/11/better-than-ever-xenappxendesktop-site-design-v2017/), to allow for easily managed updates, patching, and scalability for expansion. For detailed information about setting up GSLB between AWS AZs, see [Citrix Documentation](/en-us/advanced-concepts/design-guides/netscaler-and-amazon-aws.html).

[Diagram 12: Traffic flow before and after HA failover in multi-AZ HA deployment](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_012.png)
*Diagram 12: Traffic flow before and after HA failover in multi-AZ HA deployment.*

In the preceding diagram, we can see that each ADC has a different Gateway virtual IP (VIP). This is characteristic of an [Independent Network Configuration (INC)](/en-us/citrix-gateway/current-release/high-availability/ng-ha-routed-networks-con.html).
When VPXs in an HA pair reside in different Availability Zones, the secondary ADC must have an INC, as they cannot share mapped IP addresses, virtual LANs, or network routes. The NSIP is different for each ADC in this configuration, while SNIPs and Load Balancing VIPs utilize a special [Citrix ADC feature called IPset](/en-us/citrix-adc/current-release/load-balancing/load-balancing-customizing/multi-ip-virtual-servers.html), or Multi-IP virtual servers, which can be used for clients in different subnets to connect to the same set of servers. With IPset, you can associate a private IP to each of the primary and secondary instances. A public IP can then be mapped to the primary ADC in the pair. In the case of failover, the public IP mapping changes dynamically to the new primary. For GSLB deployments in AWS, the service IP can be part of the IPset for both IPv4 and IPv6 traffic.

For more information on adding a remote node to an ADC to create an INC-based HA pair, see [Citrix docs](/en-us/citrix-gateway/current-release/high-availability/ng-ha-routed-networks-con/ng-ha-add-remote-node-tsk.html) and watch [this video](https://www.youtube.com/watch?v=HhBxA_5Y2Cc&feature=youtu.be) from the Citrix YouTube channel.

### Citrix StoreFront on AWS

Deploying Citrix StoreFront on AWS is not much different than deploying it on-premises, and in the end, you're also managing all the components of StoreFront yourself too. See [Plan your StoreFront deployment](/en-us/storefront/current-release/plan.html) for general considerations which apply to all deployments including StoreFront on AWS. The main difference is that you typically deploy multiple StoreFront instances in a StoreFront server group across multiple AWS availability zones. It is important to note that the features enabled with this design are **dependent upon latency between AZs**. Per [Plan your StoreFront Deployment/Scalability](/en-us/storefront/current-release/plan.html#scalability), StoreFront server group deployments are only supported where links between servers in a server group have latency of less than 40 ms (with subscriptions disabled) or less than 3 ms (with subscriptions enabled).
Make sure you measure latencies between instances in all AZs you plan to host StoreFront and enable/disable subscriptions accordingly.

We already called this out in the [UI Service and Authentication Considerations](#ui-service-and-authentication-considerations) table earlier in this document, but it is worth calling out again: **for Citrix Virtual Apps and Desktops Service environments with extensive resiliency requirements, Citrix strongly recommends a StoreFront implementation to fully benefit from the Local Host Cache feature** (available in both CVAD and CVADS session brokering infrastructure types). For CVAD, this provides resiliency if there is a database outage.
For CVADS, this architecture provides resiliency in case Cloud Connectors cannot reach Citrix Cloud. In either case, disconnected users will still be able to connect to new and existing sessions during an outage scenario. For more details, limitations, and implications of Local Host cache activation, see [Local Host Cache (CVADS)](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html) and [Local Host Cache (CVAD)](/en-us/citrix-virtual-apps-desktops/manage-deployment/local-host-cache.html).

While we're on the topic of resilience, Citrix also recommends that your StoreFront implementation span multiple AZs (if the AWS region includes multiple AZs), but remember to take the ADC design into account.
Citrix ADC is often used in front of StoreFront instances to provide load balancing and extra service resiliency.

By utilizing [Citrix Zones](/en-us/xenapp-and-xendesktop/7-15-ltsr/manage-deployment/zones.html), StoreFront redundancy can be built in by spreading satellite zones across two or more AZs in a VPC with a single site. Using Zones is a great way to have resources as close to the users as possible and highly available. Satellite Zones contain StoreFront servers, Delivery Controllers, and app/desktop resources, leaving the Primary Zone with the full infrastructure setup, including the license server and SQL.
This allows for scalability of StoreFront web UI and Zone creation/destruction can be orchestrated. Keeping the Zones smaller will allow for optimal east-west scalability and reduce the impact in the case of an outage.

StoreFront on AWS is fully customizable, including Featured App Groups, splash page, coloring and logo, and apps and desktops can be arranged in the best way for your specific needs. StoreFront on AWS also requires knowledgeable administration and engineering to be kept up, but can provide a powerful web UI, especially when integrated with the Citrix ADC.

## Resource Layer Considerations

The Resource Layer design focuses on personalization, applications, and image design. The Resource Layer is where users interact with desktops and applications. When deploying a Citrix virtualization system on AWS, the key things to keep in mind (aside from all the 'normal' stuff we won't cover here) are:

-  **CIFS storage and data replication** - Regardless of the tooling
    you use for managing user personalization settings (the users' Windows profile and redirected folders) you've got to have Windows file shares to store them on. If you've got VDAs in multiple regions (and users can access apps/desktops in more than one) then you've also got to deal with data replication. Many applications also use Windows file shares, so CIFS storage and data replication are important for these too.
-  **Image design** - Citrix App Layering and Citrix Provisioning
    Services (PVS) do not currently support Amazon EC2 - customers hosting a resource location in AWS use Machine Creation Services for VDA fleet creation, management, and updating.

### CIFS Storage and Data Replication

Most Citrix virtualization systems on AWS require at least basic access to a Windows compatible file share to persist user settings, user data, and application data. When these shares are not available, the user experience and application functionality suffer, so it is important to ensure that whatever solution you choose to provide Windows compatible file shares is highly available and data is regularly backed up.

For multi-site deployments, reliable and performant data replication may also be necessary to meet availability, RPO, and RTO needs. This is especially true for environments where users may connect to desktops/apps in 2 or more regions, and application data/user settings must be available in the region where the apps/desktops run. The following section describes some solutions to consider for providing CIFS storage and data replication services on AWS.

While non-Windows solutions for providing Windows file shares exist, most of these solutions cannot deliver the indexing capabilities required for search functionality inside a Windows desktop or applications such as Microsoft Outlook running on Windows. As such, most customers turn to Windows-based file server solutions, at least for storing user profiles and persistent application data. Fortunately, both customer managed and cloud service options are available for use when Citrix virtualization systems are run on AWS.

#### Customer Managed: Windows File Servers on Amazon EC2

The first solution many customers consider for providing Windows compatible file services on AWS is building their own Windows file servers on EC2 to serve each resource location on AWS. Since Windows file servers are needed by various different types of applications and workloads, many IT shops may gravitate towards building and managing their own since this is something they know how to do. At the most basic level, the customer spins up one or more Windows EC2 instances, attach extra Amazon Elastic Block Store (EBS) volume, join the instance's to their Active Directory, and get busy configuring and setting up Windows File Services.

This option, as you might imagine, provides customers with the most control and flexibility. While this is very appealing to certain types of customers and certain verticals, it also comes at a cost: the responsibility to size, scale, build, manage, patch, secure, and maintain everything from the Windows OS up. Customers electing to go this route ought to also ensure these file servers are highly available.
This is often accomplished using file servers in multiple Availability Zones, and using Windows DFS-N/DFS-R, though it's easy to end up in an unsupported configuration (per Microsoft) if you're not careful.

***Note:** Customers considering this option ought to review [Microsoft's support statement](https://support.microsoft.com/en-ca/help/2533009/information-about-microsoft-support-policy-for-a-dfs-r-and-dfs-n-deplo) regarding using DFS-R and DFS-N for roaming profile shares and folder redirection shares.*
One more point to consider since the Citrix virtualization system will be running on AWS: a new deployment or migration event may provide an excellent opportunity to evaluate using a cloud service for Windows file services instead of building your own. Fortunately, Amazon has some cloud service options worth considering. We touch on some of these now.

#### Cloud Service: Amazon FSx for Windows File Server

[Amazon's FSx for Windows File Server](https://aws.amazon.com/fsx/windows/) is a cloud service which customers can consume on AWS. FSx for Windows File Server provides a fully managed, native Windows file system, and SSD-based storage with consistent submillisecond performance. Since FSx is built on Windows Server, it delivers a fully native, Windows compatible file system that provides storage and protection for Citrix virtualization systems on AWS. FSx for Windows File Server is also Citrix Ready Verified, meaning this AWS supported solution has been validated by Citrix to be compatible with Citrix Virtual Apps and Desktops. While it is not officially supported by Citrix, the service IS fundamentally native Microsoft Windows file server - it is just managed by AWS instead of the customer. For more information, see [Amazon FSX for Windows File Server on Citrix Ready](https://citrixready.citrix.com/amazon-com/amazon-fsx-for-windows-file-server.html).

For IT teams, this is an excellent option that removes many of the more mundane or low-value tasks around deploying and managing storage. Most importantly, using FSx offloads security, data protection/backup, compliance, software updating/patching tasks, and the monitoring of storage infrastructure to make sure it meets required service levels. IT teams can treat the entire FSx file service as a single operational platform instead of managing a Windows operating system file server, storage, networking, and such. Also, FSx supports all the common management tools it already uses, such as Active Directory (AD) integration, Windows DFS Namespaces, DFS Replication, and others.

Each FSx managed file system you create essentially becomes a highly available and durable file server in a specific Availability Zone. For servicing a Citrix virtualization system, customers ought to ensure these "file systems" are highly available. This can be accomplished by provisioning FSx managed file systems in multiple availability zones, and using Windows DFS-N/DFS-R to create highly available Windows file shares, though it's easy to end up in an unsupported configuration (per Microsoft) if you're not careful.

***Note:** Since FSx is a Windows file server, customers considering this option ought to review [Microsoft's support statement](https://support.microsoft.com/en-ca/help/2533009/information-about-microsoft-support-policy-for-a-dfs-r-and-dfs-n-deplo) regarding using DFS-R and DFS-N for roaming profile shares and folder redirection shares.*

#### More Cloud Service Options

Besides Amazon's first party managed Windows file service, AWS supports many more expansive and feature rich options, some of which integrate with traditional on-premises storage technologies. While these other options are outside the scope of this document, there are many options to choose from. A good place to start exploring options is on the [AWS Marketplace](https://aws.amazon.com/marketplace/). These types of solutions can be especially relevant for more complex, multi-region use cases where reliable, and resilient data replication is needed.

#### CIFS Storage and Data Replication: Summary and Conclusions

Customers can manage their own highly available DFS file share, benefit from this as an AWS service (FSx) to save on management effort, or use third party storage appliance solutions to extend on an on-premises environment. Citrix recommends that customers analyze the pros and cons of each to determine a solution that is right for them.

### Image Design and Management

In a Citrix virtualization system on AWS, applications and desktops are delivered via EC2 instances called "VDAs" (named after Citrix's Virtual Delivery Agent software, which is installed into Windows or Linux instances containing the applications being delivered by the Citrix virtualization system). A group of identical VDAs are provisioned and maintained in "Machine Catalogs," a management construct defined and maintained through the session brokering and management subsystem (both CVADS and CVAD). The creation, sizing, and management of these instances is key, as many systems have large numbers of VDAs and the software stack in a VDA changes frequently as hotfixes, service packs, and software updates are applied. We discuss some of the higher-level considerations in this section.

#### VDA Provisioning and Image Management

On AWS EC2, Citrix virtualization systems use Citrix's Machine Creation Services (MCS) provisioning technology for VDA deployment and image management. MCS utilizes an IAM service account on EC2 to orchestrate the mastering process (turning a snapshot of a template VM's system disk into a generalized AMI), the cloning process (creating and managing a fleet of VDA instances based on the AMI created from the snapshot of the template VM), autoscaling Delivery Groups, updating deployed images, and more. We discuss MCS on AWS in much more detail in the [Control Layer Considerations](#control-layer-considerations) sections of this document.

**Note:** customers already using MCS for their on-premises
environments can notice some differences between the options available to them when provisioning machines in AWS. MCS managed VDA instances on EC2 have two disks attached: the system disk (a read/write copy of the template image AMI created during the mastering process) and a 1GB personality disk. Depending on the machine catalog type and hosting connection options configured, the system disk (and sometimes the VM instance) will be deleted at shutdown and recreated at 'power on' (for pooled or shared catalogs) or they are retained (for persistent catalog types). See [CTX234562](https://support.citrix.com/article/CTX234562) for more information.

#### Delivery and Persistence Models

Choosing the right delivery models is critical and has broad implications beyond just cost. Citrix virtualization technology supports three main delivery models, which can be mixed and matched and used in combination to support many different use cases. The three delivery models are:

-  **Hosted shared:** The hosted shared model most commonly utilizes a
Windows Server OS with the RDSH role installed, though Linux instances can provide the same functionality for compatible apps. With this model, a single VDA instance can support multiple simultaneous users, each running either a full desktop or connecting to one or more published applications. When using Windows Server OS/RDSH with the Desktop Experience and related components installed, desktops and apps look and feel like they're running on a Windows desktop OS. Since every user on a given instance shares OS instance, administrators typically pre-install and configure the mix of applications on hosted shared instances, and users do not have local administrator rights to the OS.
Hosted shared instances can also run on shared infrastructure, and can be consumed using both on-demand and reserved instance pricing models.
Administrators usually deploy a fleet of instances to support the hosted shared model, and both customer managed and cloud service types of Citrix brokering subsystems provide sophisticated load balancing capabilities to ensure every user experiences adequate performance.
Hosted shared instances can also use GPU backed instance types on AWS to increase performance for graphically intensive workloads that can benefit from a GPU, though the GPU vendor can require extra licenses. Both Windows Server OS and RDS CAL licenses can be 'rented' under Microsoft's SPLA licensing model, though customers can avoid these additional costs by using Linux as the OS. *This model is, hands down, the most cost effective to run on AWS.*
-  **Server VDI:** The "Server VDI" (Virtual Desktop Infrastructure) model also uses a Windows Server OS, and with the Desktop Experience and related components installed, it looks and feels to the user just like a Windows Desktop OS. The RDSH role is not installed with this model, so one instance supports one user at a time, and users are sometimes provided with elevated rights to the Server OS so they can install their own applications. Like hosted shared instances, server VDI instances can also run on shared infrastructure, can be consumed using both on-demand and reserved instance pricing models, can use GPU backed instance types, and Microsoft OS and RDS CALs can be 'rented' under Microsoft's SPLA licensing model. Given the tools available today, 99+% of Windows applications can be installed and ran on the Windows Server OS, and though sometimes software vendors don't explicitly support their applications on Windows Server, most Windows apps run as well on Windows Server as they do on a Windows Desktop OS. It's also worth noting that server VDI instances can also use GPU backed instance types on AWS to increase performance for graphically intensive workloads that can benefit from a GPU, though the GPU vendor may require another licenses. *This is the second most cost-effective delivery model to run on AWS.*
-  **Client VDI:** The client VDI delivery model typically uses a Windows desktop OS such as Windows 10 or Windows 7, although a supported Linux OS version can be used as well. Client VDI is a 1:1 model, meaning each unique user requires their own OS instance. Customers who are new to Citrix virtualization technology often come into these types of projects asking for client VDI, even though more cost-effective models are available. Their vernacular can also have been influenced by other virtualization vendors whose technology stacks don't support hosted shared or server VDI deployment models. The client VDI model, while looking 'simpler' on the surface, gets much more complicated the deeper you get into it, though most of the complexity can be avoided by using Linux as the OS. Most of this complication is driven by Microsoft's licensing requirements for the Windows Desktop OS which, unlike Windows Server, is not available via Microsoft's SPLA licensing program. As such, customers must bring their own licensing for these products.
Also - Windows desktop based client VDI instances cannot run on shared infrastructure. This means that client VDI instances must run in either AWS dedicated instances or on AWS dedicated hosts. This substantially increases the cost and complexity of managing the infrastructure required, reduces flexibility and cost control options, and gets expensive quickly. As you might expect, client VDI instances can also use GPU backed instance types on AWS to increase performance for graphically intensive workloads that can benefit from a GPU, though the GPU vendor can require more licenses. *Client VDI is the most expensive delivery model to run on AWS.*
For both VDI models, another important consideration is the persistence model. VDI instances can be randomly assigned to users with no persistence (pooled) or users can have assigned machines that persist and are personalized (dedicated). Pooled instances can be easier to manage over time since all instances in a given pool are identical.
Citrix's MCS can update the system disks attached to pooled instances with a few clicks, and capacity/cost management is more effective since an idle pool of instances can serve many users. Pooled instances are a bit less flexible than dedicated since end-user changes to pooled instances don't usually persist between reboots, though technologies such as Citrix App Layering's User Layer or Personalization Layer released in CVAD 1912 can be used to minimize the impact on the user experience. Dedicated instances can also be tougher to manage from a cost perspective too - since it is often tough to predict when a user will log on, the user must either wait while their instance is started, or administrators must keep them running during time windows where each user is expected to log on.

While we've mentioned it previously, we're going to mention it again here for clarity: various flavors of Linux can be used in a Citrix virtualization system, as long as one or more applications run on Linux. Citrix's virtualization technology supports both hosted shared and VDI delivery models, persistent and pooled models, and GPU backed instance types. The user and administrator experiences are different than with Windows based instances, but Linux based VDAs are often much less expensive to run since they don't require Microsoft licenses.

Finally, let's revisit the consideration of GPU acceleration. All three delivery models (for both Linux and Windows) can use NVIDIA accelerated GPU instances on AWS. G-series instances can be used for graphics accelerated use cases, but are not yet commercially viable for general purpose usage. Note that Citrix doesn't support the AWS Elastic GPU today, but since Elastic GPU only works for OpenGL, its impact on typical graphics workloads in the enterprise is minimal.

So - which delivery models do you use? It is worth noting that you can mix and match delivery models in the same system to meet the needs of different user groups or use cases. **The most cost-effective delivery model from an infrastructure perspective is hosted-shared**.
The combination of server OS with multi-user concurrency is highly efficient, and the number of users per virtual machine can be sized accordingly depending on user type (for example - task worker vs. knowledge worker vs. power user) to ensure a great experience. For situations where users and apps require extra capabilities that aren't satisfied by hosted-shared, VDI is the way to go. Server VDI ought to be evaluated first: it is substantially more cost-effective to run than Windows 10 VDI for Windows workloads, and Server VDI can deliver a desktop that looks and feels similar to Windows 10. Also, Server VDI doesn't have the Microsoft EULA requirement to use dedicated instances/hosts - Client VDI (deploying Windows 10 or sometimes Windows 7) does. For Windows based workloads on AWS, Client VDI ought to be considered as a last resort, and deployed only when hosted shared and Server VDI delivery models are not possible.

To help with the decision making process, the following decision tree compares [Hosted Shared Desktops (Server OS multi-user desktops) to VDI Desktops](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_003.png).
The tree doesn't explicitly differentiate between client VDI and server VDI models. When a use case suggests VDI is the appropriate delivery model for your workload, Server VDI ought to be considered wherever possible for running on AWS as it is substantially more cost effective and easier to manage.

#### AWS Instance Billing Model

Once you've decided which delivery model to use (hosted-shared, server VDI, or client VDI), the next step is to plan for an hourly on-demand billing model or a reserved billing model. Ideally, as many VDAs as possible are to be paid for by the hour with the on-demand billing model, and use the [Citrix Autoscale](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html) feature to control costs. By using Citrix Autoscale (a feature exclusive to the CVADS cloud service brokering subsystem) VMs are powered on as needed with anticipation for peak hours. During off peak hours, however, VMs are shut down, so it's important to consolidate loads with the hosted-shared model and for all models ensure that users save their work and ideally log off gracefully from their sessions.
Reserved instance capacity can be used for infrastructure components like the Cloud Connectors (which remain on 24/7) and a predetermined number of VDAs that will always remain on (for example, 10% of peak). Besides providing significant discount compared to On-Demand pricing, Reserve Instances also provide a capacity reservation when used in a specific Availability Zone.

#### VDA Instance Sizing and Cost Management

When running a fleet of VDAs on AWS, choosing the right instance type for your different workloads (VDAs) is a key decision, with substantial performance, manageability, and cost considerations. Choose too small of an instance and performance can suffer. Choose too large of an instance, and you're paying for resources you're not using. Choosing the right instance type ends up being a balancing act, and often requires fine-tuning for each specific workload.

Which AWS EC2 instance type to choose for your VDAs depends heavily upon the specific workload and delivery type. However, as a general guideline, "M" series instances are often most suitable for hosted-shared whereas "T" series instances are suitable for VDI. "M" series has balanced CPU and RAM designed for the mostly predictable resource consumption across multiple sessions on a host. "T" series are "burstable" in nature designed for the mostly unpredictable characteristics of VDI (for example - one minute a user is idling and the next they are running a macro calculation). For extra details on instance type selection and pricing, readers can refer to the [Citrix on AWS cost estimation presentation](#sources) (in sources section).

For more information regarding instance selection (especially as it applies to the hosted shared delivery model) sees [Citrix Scalability in a Cloud World â€“ 2018 Edition](https://www.citrix.com/blogs/2018/08/16/citrix-scalability-in-a-cloud-world-2018-edition/).
This article, while slightly dated, discusses leading practices regarding instance selection based on performance, manageability, cost, reserved vs. on demand pricing models, and LoginVSI scalability testing.
These concepts and considerations are still valid today, even though instance choices and pricing have likely changed since its initial publication.

***Note:** Some newer AWS instance types will not show up by default in
the Machine Catalog creation wizard in Studio (either CVAD or CVADS).
The UI is populated with instance types from a static XML file which resides on Delivery Controllers (CVAD) or Cloud Connectors (CVADS). This XML can be modified to include newer instance types, but this file is overwritten with default values during upgrades (both Citrix initiated Cloud Connector updates or customer-initiated Delivery Controller upgrades). See [CTX139707](https://support.citrix.com/article/CTX139707?_ga=2.25237309.1017824996.1576508025-674023236.1532570336) for more details on how to update the list of available AWS instance types.*
To get a better idea of how instances are commonly sized on AWS (or any other platform) see this [detailed AWS LoginVSI blog](https://www.loginvsi.com/blog-alias/login-vsi/901-citrix-virtual-app-user-density-on-aws).
During this round of testing (a point in time reference) the M5.2Xlarge instance type (8vCPU, 32 GB RAM) turned out to be the winner in terms of $/user/hour (with an industry standard sample workload). Your numbers - given your specific workload characteristics and available AWS pricing - can vary, but the process and tooling can be used to approximate your monthly IaaS costing more accurately. Regardless of how you determine the instance types you start with, it is important to monitor usage over time and adjust as needed to keep the balance between resource availability, consumption, and cost. Customers ought to consider using services such as [Citrix Analytics for Performance](/en-us/tech-zone/learn/tech-insights/performance-analytics.html) - the information such services provide can play a key role in keeping performance up and costs down.

#### Application Design

An extra consideration includes application design. As customers plan to migrate workloads to a cloud platform such as AWS, they must ensure that app performance is not impacted. A rule of thumb which has applied for over 20 years is that the data ought to reside as near as possible to the workload. This means more complex applications architectures ought to respect this rule.
An example of this includes apps with a front-end and back-end (database).
To avoid adding latency which will impact application performance, both the front-end and back-end are to be migrated. An alternative would be a hybrid approach using a mix of on-premises (for complex apps) and cloud hosted workloads (for simple applications). It is important to always consult with application vendors for compatibility. The linked Tech Zone decision matrix compares the different [Hosted Shared delivery methods](/en-us/tech-zone/design/media/design-decisions_application-delivery-methods_004.png), which include Hosted Shared Applications (single and multi-use) and Hosted Shared Desktops. The workload segmentation decision-making process these article outlines can be used as a guide for the workload design process.

One final word on application design, the Enterprise Layer Manager appliance does not currently run on AWS, and does not currently support exporting layered images into an immediately consumable disk format for use on AWS. If App Layering support on AWS is critical for your migration or deployment, email [aws@citrix.com](mailto:aws@citrix.com) with information about your project. You’ll be added to the list to be an early adopter candidate for future releases, and your voice will be heard. For more information on Citrix App Layering, refer to the [product documentation](/en-us/citrix-app-layering/4.html) and [App Layering reference architecture](/en-us/tech-zone/design/reference-architectures/app-layering.html).

## Control Layer Considerations

In the Citrix Architectural Design Framework, the Control layer defines the components that control the Citrix solution. This includes components like Active Directory (forest/domain, OU, and user group structure, group policies, and such), Microsoft SQL database usage, Citrix licensing, session brokering and administration, load management, and VDA provisioning/image management. As with previous sections of this document, here we focus on the considerations which are most important for Citrix virtualization systems on AWS, and provide links to existing documentation/guidance on others.

One of the most impactful decisions you are making for control layer components is the session brokering and administration choice. This decision is critical, with substantial implications on cost, complexity, availability, and ongoing maintenance efforts. We start by reviewing the deployment models we introduced earlier in this document, then dig more deeply into the AWS specific considerations.

### Control Layer: Greenfield/Cloud Only Deployment

The green field or cloud only deployment model uses cloud services across the board. n). The AWS specific implications on the design of your Citrix virtualization system are minimal, but we walk you through them anyway. Since Citrix Cloud provides most of the infrastructure and administrative components as a service, you won't have to worry about SQL databases, Citrix License Servers, Citrix Director servers and more.

### Control Layer: Hybrid Deployment

Remember that with the hybrid deployment model, you're going to be building/managing some of the Citrix virtualization system components, otherwise it is a green field or cloud only deployment by definition.
The interesting thing here is that, in the context of the Control layer, they're almost identical.

### Control Layer: Lift and Shift Deployment

With the legacy lift and shift deployment model, you're deploying all key control layer components (including Active Directory and all Citrix session brokering/management components) on AWS. If you have to go down the "lift and shift" path, it is both a blessing and a curse. It is a blessing in that most of these considerations have been thoroughly documented in various published works that are already available. It is a curse in that you'll have a lot more work to do both up front and over time to build, manage, secure, and maintain these components.

If you're a "lift and shift"er, you want to review and reference the following before you continue: collectively, they cover most of the design decisions you must consider to be successful deploying Citrix on AWS using the lift and shift deployment model:

-  [Citrix XenApp and XenDesktop 7.6 on AWS Reference Architecture](https://www.citrix.com/blogs/2015/06/18/xenapp-and-xendesktop-7-6-reference-architecture-on-aws/) (blog)
-  [Citrix XenApp and XenDesktop 7.6 on Amazon Web Services Reference Architecture](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/citrix-xenapp-and-xendesktop-7.6-on-amazon-web-services-reference-architecture.pdf) (guide)
-  [Using AWS Directory Service and Amazon RDS with Citrix Virtual Apps and Desktops](https://aws.amazon.com/blogs/apn/using-aws-directory-service-and-amazon-rds-with-citrix-virtual-apps-and-desktops/) (blog)
-  [Deploying Citrix Virtual Apps and Desktop with AWS Directory Service and Amazon RDS â€“ Version 1.0](https://s3-us-west-2.amazonaws.com/apnblog.awspartner.com/Citrix+Virtual+Apps+and+Desktops/Citrix+Ready-Amazon+RDS+Deployment+Guide_v1.pdf) (deployment guide)
-  [Citrix VDI Handbook and Best Practices for XenApp and XenDesktop 7.15 LTSR](/en-us/xenapp-and-xendesktop/7-15-ltsr/downloads/handbook-715-ltsr.pdf) (document)

### Active Directory Considerations

All deployment models for Citrix virtualization systems on AWS require Microsoft Active Directory. For a compelling user experience, functional Active Directory services must be available in every AWS region where you've got VDAs deployed. The structure and complexity of your Active Directory implementation must be carefully considered, but fortunately Citrix virtualization can flexibly integrate with various different AD designs and servicing models.

When deploying Active Directory on AWS, customers can build/maintain their own Active Directory Domain Controllers using Windows Server instances, use [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/), or a combination of the two. Active Directory trusts can also be used to connect two or more AD forests/domains depending upon the customer's needs.

For customers looking to minimize the administrative overhead required to build and maintain functional Active Directory services, the [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/) (also known as AWS Managed Microsoft AD) is an option worth considering. This service provides you with a fully functional Active Directory forest/domain without the overhead of building and maintaining Windows Server VM instances. AWS Managed Microsoft AD is built on highly available, AWS-managed infrastructure. Each directory is deployed across multiple Availability Zones, and monitoring automatically detects and replaces domain controllers that fail. In addition, data replication and automated daily snapshots are configured for you. You do not have to install software, and AWS handles all patching and software updates.
With AWS Managed Microsoft AD, you can use native Microsoft administrative tools, manage Windows machines and users with Microsoft Group Policy, join EC2 instances and AWS RDS for SQL Server instances to it, and even setup Active Directory trusts with existing AD instances to support various complex Enterprise scenarios.

Customers who choose to use the AWS Managed Microsoft AD service with Citrix virtualization technologies can expect these technologies to work with this AWS service, though there are a few important considerations to consider before doing so. For starters - you won't have Domain Administrator, Enterprise Administrator, or other 'super user' type access to the AD instance. You do, however, have full control of your own container at the root of the directory where you can create users, computers, groups, OU's, and group policies.

A few other things you CAN NOT do:

-  Create AD objects in any of the default containers (such as /Computers): they're read-only. This brings up a common mistake some customers make when using Citrix's MCS provisioning technology: you must choose to create the machine accounts for your MCS managed VDAs in a container/OU that's writeable - if you don't choose such a location, MCS won't be able to create the machine accounts.
-  Install and configure some AD integrated features such as Certificate Services. As such, this impacts customers who will be using Citrix's Federated Authentication Services ("FAS") technology (which requires AD integrated Certificate Services): these customers must build and manage their own Active Directory on AWS using EC2 Windows Server instances.
-  Have local Server Administrator equivalence by default. In an 'out of the box' Active Directory installation, the Domain Administrators group is added to the local Server Administrators group by default. If you're using the AWS Managed Microsoft AD service, you must create your own server administrators' group, add your own users to it, create and apply a group policy to add your group to the built-in Server Administrators group on member servers/workstations.

While trust relationships, site/service configuration, replication, and other AD related topics will not be covered in this paper, Citrix has provided extensive documentation on these topics applicable to all three deployment models.

***Note:** [AWS Directory Service for Microsoft Active Directory](https://aws.amazon.com/directoryservice/) is a "Citrix Ready Verified" offering. While not officially supported by Citrix, the service IS fundamentally native Microsoft Active Directory - it is just managed by AWS instead of the customer. This AWS service does have some limitations imposed upon it to deliver it as a service at scale, and the currently known/most impactful limitations for a Citrix environment are listed here.*
For more information on Active Directory requirements for green field and hybrid deployments (environments using Citrix Cloud and the CVAD Service for session brokering and administration) see [Citrix Cloud Connector Technical Details](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html).
Besides covering [supported Active Directory functional levels](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#supported-active-directory-functional-levels), this article also covers [deployments scenarios for Cloud Connectors in Active Directory](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory).

For more information on Active Directory requirements for lift and shift deployments (environments using customer managed session brokering and administration via Citrix Virtual Apps and Desktops LTSR or CR versions) see [CVAD Technical Overview, Active Directory](/en-us/citrix-virtual-apps-desktops/technical-overview/active-directory.html) and [CVAD System Requirements, Active Directory Functional Levels](/en-us/citrix-virtual-apps-desktops/system-requirements.html#active-directory-functional-levels).

### Session Brokering and Administration Considerations

As you've probably already gathered by now, the choice of how you provide session brokering and administration services is critical, and has broad reaching implications on overall cost, manageability, maintenance, and available capabilities for your Citrix virtualization system. As we've already discussed, Citrix recommends the use of the Citrix Cloud service (CVADS) for this critical component, but for certain requirements and scenarios, deploying a customer managed session brokering and administration subsystem (via CVAD LTSR or CR releases) can be necessary or recommended. The following table highlights some of these requirements and scenarios for your consideration:

| Attribute/Capability | Customer Managed CVAD (Citrix Virtual Apps and Desktops, LTSR, or CR versions) | Cloud Service CVADS (Citrix Virtual Apps and Desktops Service, provided by Citrix Cloud) |
|---|---|---|
| Requires outbound Internet connectivity to Citrix Cloud. | **NO** - Delivery Controllers don't require outbound Internet connectivity, though they must be able to communicate to AWS infrastructure for MCS provisioning to function. | **YES** - Cloud Connectors communicate over the Internet to Citrix Cloud, though these connections can be proxied. See [How to Set up a Proxy Server for Citrix Cloud Connector](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/proxy-firewall-configuration.html) for more details. For strictly air gapped deployments, this is often a show stopper. |
| Requires the customer to provide highly available Microsoft SQL database services. | **YES** - CVAD (in both LTSR and CR release types) requires the customer to provide and highly available Microsoft SQL database services. These can be provided by building SQL Servers on EC2 instances, or by using the AWS RDS for SQL Server service. | **NO** - CVADS does not require customers to touch the SQL server: highly available database services are provided by the Citrix Cloud delivery platform and are transparent to customers. |
| Requires the customer to apply patches and upgrades to Citrix software over time to maintain security and supportability, and to get access to new features and capabilities. | **YES** - customers are responsible for installation, configuration, patching, securing, and upgrading both Citrix software and underlying operating system for all components in a CVAD based session brokering and administration system. They're also responsible for maintaining high availability of each component, including Citrix Delivery Controllers, Studio installations, Director, and Citrix Licensing. | **NO** - Cloud Connectors (the only session brokering and administrative component that resides in the customer's VPC) are automatically updated and maintained by Citrix. Customers are responsible for patching and maintaining the Windows **Server operating system** on the EC2 Cloud Connector instances, and new features and capabilities are available immediately, without requiring the customer to manually update the Cloud Connectors. |
| Ability to use advanced services provided by Citrix Cloud, including the [Citrix Autoscale feature](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html). | **Sometimes** - not all advanced services are available to customer managed CVAD deployments, and when they are, can require the installation and configuration of extra components. The Autoscale feature is not available for CVAD environments. | **YES** - CVADS is designed to work 'out of the box' with other Citrix Cloud services, and these services are typically pre-configured so the customer simply turns them on. The [Autoscale feature](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html), which provides the ability to granularly control the quantity and power state of VDAs, is impactful for VDA deployments on public cloud. It can provide substantial infrastructure cost savings in scenarios where you're paying for only the capacity you need. |
| Ability to have complete control over all subsystem components, including timing of upgrade and maintenance activities. | **YES** - since every component is installed, configured, and maintained by the customer, the customer has complete control over the versioning, configuration, and availability of each component (albeit at substantially increased cost of infrastructure, complexity, and administrative overhead). | **NO** - with CVADS, customers give up some measure of control, but gain simplicity, reduced infrastructure costs, and substantially reduced administrative overhead. |
| Ability to license based on concurrent users vs. named users. | **YES** - CVAD can be licensed by CCU. | **YES** - CCU licensing is available. See [this blog](https://www.citrix.com/blogs/2020/04/08/concurrent-licensing-is-here-for-citrix-virtual-apps-and-desktops-service/) for details.|

### Cloud Connectors, Delivery Controllers, and Resource Locations

Since both green field and hybrid models use cloud services (CVADS) for session brokering and administration, you deploy Cloud Connectors to create a [resource location](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html) in each region where you plan to host VDAs. When you create a resource location in a region, you build a highly available configuration by deploying **n+1** Cloud Connector instances and spreading the Cloud Connectors across Availability Zones in that region. Cloud Connectors are typically placed in separate private subnets from the VDAs to simplify security policy application, and the Cloud Connector instances must have outbound Internet access to facilitate connecting to Citrix Cloud. Placing them in a separate subnet from VDAs allows administrators to apply different routing policies to the two different resource types.

[Diagram 13: CVADS Resource Location design pattern with separate subnets for VDAs and Cloud Connectors](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_013.png)
*Diagram 13: CVADS Resource Location design pattern with separate subnets for VDAs and Cloud Connectors.*

The same general concepts apply when we're talking about Delivery Controllers (CVAD), though we use the term zone vs. resource location in the customer managed brokering subsystem. Also note that Cloud Connector instances on EC2 are great candidates for reserved pricing since they are running anytime the system needs to be up. See [this article](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-cloud-connector/cc-scale-and-size.html) for more information about sizing Cloud Connector instances.

### Citrix Virtual Apps and Desktops Site Design Considerations

#### Resource Locations and Zones

Using [Citrix zones](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html) (not to be confused with Availability Zones) can help users in remote regions connect to resources without necessarily forcing their connections to traverse large segments of the WAN. In a Citrix Virtual Apps and Desktops Service environment, each resource location is considered a zone. When you create a resource location and install a Cloud Connector, a zone is automatically created for you. Each zone can have a different set of resources, based on your unique needs and environment. For more information on zones, see the following [link](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html).

#### Machine Catalogs, Delivery Groups, and Resource Locations

Citrix administrators ought to ensure that VDAs are also spread across Availability Zones (AZ). An AWS Availability Zone (AZ) is one or more discrete data centers with redundant power, networking, and connectivity in an AWS Region - a physical location around the world where AWS cluster data centers. A virtual private cloud (VPC) is a virtual network which spans Availability Zones in the Region. Subnets are a required subcomponent of a VPC, and virtual network interfaces are each attached to a single subnet. Each subnet must reside entirely within one Availability Zone and cannot span zones. By launching VDAs in separate Availability Zones, you can protect your applications from the failure of a single location.
See [What is an Amazon VPC?](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html) for more information. To ensure VDAs are spread between AZs you can create a Machine Catalog per AZ (using one Host Connection per AZ) which then can map to a single Delivery Group.

#### Provisioning in AWS: Machine Creation Services

Starting with the release of CVAD 1811, role-based authentication can be used when creating a host connection for [MCS provisioning in AWS](https://www.citrix.com/blogs/2019/02/04/role-based-authentication-for-citrix-virtual-apps-and-desktops-in-aws/).
An IAM role or IAM user account associated with a Delivery Controller or Cloud Connector on an EC2 instance can be used in the place of a user's secret key and API key, enabling increased security, delegated administrative rights, and PKI-based environments with temporary credentials and session tokens. To configure a host connection using role-based authentication, first create an IAM role with the permissions described in [CTX140429](https://support.citrix.com/article/CTX140429).
[Associate this role with an EC2 instance](https://aws.amazon.com/premiumsupport/knowledge-center/assign-iam-role-ec2-instance/) with a CVAD 1811+ Delivery Controller or a Cloud Connector.
On versions of CVAD earlier than 1811, admins must provide the API Key (Access Key) and Secret Key of an IAM user to create a host connection. The following [article](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/flexing-to-the-cloud-with-citrix-xendesktop-and-amazon-web-services.pdf) describes how to configure a host connection this way.

After creating the host connection, create a machine catalog as described [here](/en-us/citrix-virtual-apps-desktops/install-configure/machine-catalogs-create.html) using an AMI created from the master VDA image in AWS. For more details about MCS in AWS, see the following articles:
[Citrix MCS on AWS Deep Dive 1](https://support.citrix.com/article/CTX241160) and [How MCS works after pooled VMs are created in AWS](https://support.citrix.com/article/CTX234562).

Another item that ought to be considered when deploying VDAs in AWS using MCS is [EBS Volume initialization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-initialize.html) (also known as pre-warming or hydration). For volumes that were restored from snapshots, the storage blocks must be pulled down from Amazon S3 and written to the volume before you can access them. This preliminary action takes time and can cause a significant increase in the latency of I/O operations the first time each block is accessed. Volume performance is achieved after all blocks have been downloaded and written to the volume. See [Initializing Amazon EBS Volumes on Windows](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ebs-initialize.html) for AWS recommended steps to Initialize Amazon EBS Volumes on Windows instances and see [Initializing Amazon EBS Volumes on Linux](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-initialize.html) for Linux instances.

See [Infrastructure (or Platform) Layer Considerations](#operations-layer-considerations) for details on VPC design as it relates to MCS.

#### Troubleshooting Machine Creation Services

This section lists some common issues and associated recommendations/resolution links.

-  Some newer AWS instance types will not show up by default in the Machine
Catalog creation wizard in Studio (either CVAD or CVADS). The UI is populated with instance types from a static XML file which resides on Delivery Controllers (CVAD) or Cloud Connectors (CVADS). This XML can be modified to include newer instance types, but this file is overwritten with default values during upgrades (both Citrix initiated Cloud Connector updates or customer-initiated Delivery Controller upgrades).
See [Updating AWS Instance Types for XenDesktop](https://support.citrix.com/article/CTX139707) for more details on how to update the list of available AWS instance types.
-  [MCS Provisioning fails on AWS EC2 when using Dedicated instances](https://support.citrix.com/article/CTX222527)
-  [Unable to create AWS hosting connection on Citrix Cloud DDC when proxy is configured on connector server](https://support.citrix.com/article/CTX248735)
-  [When Creating Machines with MCS and AWS an Error "XDDS:2367399e" Occurs](https://support.citrix.com/article/CTX219734)
-  [Configure the Volume Worker instance to use the Machine Catalog VPC and not the default VPC](https://support.citrix.com/article/CTX219734)
-  [How to Ensure Region Compatibility When Using XenApp and XenDesktop MCS in AWS](https://support.citrix.com/article/CTX225276)
-  [Troubleshooting tips from the field for Machine Creation Services in AWS](https://www.citrix.com/blogs/2019/10/16/troubleshooting-tips-from-the-field-for-machine-creation-services-in-aws/)

## Infrastructure (or Platform) Layer Considerations

In the Citrix Architectural Design Framework, the Infrastructure (or Platform) layer defines the physical elements where the Citrix workloads run. In this document, that of course refers to AWS. AWS provides many cloud services (165+) and is both the oldest and largest of the Hyperscale Cloud providers in existence today. It was also the first public cloud supported by Citrix virtualization technology, and is a compelling option for new or existing Citrix customers looking to move existing or run new Citrix virtualization workloads in 'the Cloud'.

### Infrastructure as Code and the AWS Object Model

To understand how Citrix virtualization technologies are integrated with and run on top of AWS, it is useful to start with a basic understanding of the object model behind some of their key/relevant services. This also allows us to describe the AWS platform in terms that are familiar to most IT professionals. To facilitate this understanding, we refer to the following diagram which represents the design pattern for a CVADS resource location on AWS:

[Diagram 14: Deployed "Resource Location" architecture/design pattern from the Citrix Virtual Apps and Desktops Service on AWS Quick Start CloudFormation template](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_014.png)
*Diagram 14: Deployed "Resource Location" architecture/design pattern from the [Citrix Virtual Apps and Desktops Service on AWS](https://aws.amazon.com/quickstart/architecture/citrix-virtual-apps/) Quick Start CloudFormation template.*

This design pattern is the foundation of most Citrix virtualization system architectures on AWS. It is also not just one massive pattern - it is built on various different, well maintained and documented design patterns for Enterprise IT on AWS. These patterns are represented, documented, and reproduced using [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates. AWS provides a library of [Quick Start templates](https://aws.amazon.com/quickstart/) which can be run as-is, layered together ('nested') with other templates, and even duplicated and customized for your own specific needs. This highlights a couple of the other major advantages of public cloud infrastructure:
infrastructure as code, and the 'pay as you go' nature of many cloud services. We dig more deeply into infrastructure as code in the Citrix virtualization world shortly, but we emphasize the point with a quick touchpoint that will likely resonate for the expected readers of this paper: for many enterprise IT architects, having access to such a vast library of services, design patterns, and technology tools at your fingertips is awesome. Combined with the ability to pay for resources as you consume them and simply remove them when you're done? This is a powerful way to learn about or evaluate new stuff, and it makes the ROI for at scale investments much easier to understand and communicate.

Back to the AWS object model for a moment: the top level object in diagram 14 is the [AWS Region](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/).
You can think of AWS regions as clusters of well-connected but strategically separated data centers called [Availability Zones](https://aws.amazon.com/about-aws/global-infrastructure/regions_az/).
Each region will typically include 2 or more Availability Zones, which consist of one or more physical buildings with redundant power, networking, and connectivity. As of the time of this writing, AWS has 23 regions globally, which consist of 69 availability zones, but it is important to note that they're constantly investing in new regions and AZs. These numbers, while staggering to most of us, are likely already outdated by the time you read this. This highlights one of the other benefits of moving to public cloud infrastructure on AWS: you continue to benefit from the investments they're making (on a scale well beyond the reach of most IT organizations or even governments) over time. This continuous evolution/improvement, while daunting for change-averse IT organizations and business cultures, provides a broad reaching set of empowering benefits for Enterprise IT as it adapts to this 'new' model.

AWS region adoption choices are often based on proximity, services available, cost, compliance, or SLA. While choosing one or more right regions for your Citrix virtualization system is beyond the scope of this document, consider at least the following when making your choices:

-  If you have one or more existing, customer-managed data centers
    you are connecting to AWS, consider one or more regions which provide the lowest latency network connectivity to your data centers and major offices.
-  All regions can not have the AWS services or instance types you're
    looking for. AWS deploys new services or instance types initially to a few main regions, then expands to the rest over time.
    Also, newer regions cannot have older instance types - do your research before you build whenever possible.
-  CVAD sites and CVADS resource locations are bound to a
    specific region. High availability for individual components of a site/resource location (such as cloud connectors, StoreFront servers, and ADC/Gateway VPX instances) is accomplished by placing resources in multiple availability zones in a given region.
-  Don't go overboard spreading your infrastructure across regions:
    while it is easy to do on AWS, consider cost and complexity relative to the payoff you expect before you scale any system. You do end up paying for network traffic and storage traffic as well sometimes. The costs can be trivial for traffic while it is local to a region, but goes up when the traffic traverses regions or the Internet.

Stepping up a layer in Diagram 14 now, let's look at some of the networking constructs in this design pattern. The primary network construct on AWS is the VPC or "Virtual Private Cloud." VPCs are a regional construct (they span AZs) - you have at least one VPC in each region you deploy Citrix virtualization tech into. VPCs have a CIDR block of IP addresses defined, which must be unique if your network design routes traffic between multiple VPCs. VPCs are further broken down into subnets, and subnets are tied to an AZ (that is they do NOT span AZs in a region).

Subnets also have different attributes and objects associated with them, including routing policies and security policies. This is why the design patterns highlighted in this document (and other Citrix documentation) recommend putting VDAs in separate subnets from Cloud Connectors - so you can assign different routing and security policies to VDAs and Cloud Connectors.

Outbound Internet access from any subnet in a VPC (a regional construct) can be handled many different ways, but a common method is using [NAT Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat-gateway.html) to provide Internet connectivity to private subnets. Public subnets are often served by [Internet Gateways](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html), which facilitate the routing of inbound connections to services you make accessible from the Internet.

Subnets are also commonly labeled as 'public' and 'private'. A public subnet is a subnet with Internet routable IP addresses assigned (in addition to the private IP addresses) and is associated with a route table that has a route to an Internet Gateway (IGW) for both inbound and outbound Internet traffic. A private subnet is a subnet with only private IP addresses assigned, and is associated with a route table that has a route for outbound internet access through a NAT Gateway or NAT Instances which reside in a public subnet. In a Citrix virtualization system, the Gateway virtual server (VIP) usually resides in a public subnet since it accepts inbound connections from client devices over the Internet and is used to securely proxy Citrix virtualization traffic into private subnets in a VPC.

There are many ways to build networks on AWS, with many innovative features and techniques available that you can't get elsewhere. We're not going to introduce you to them all here, but two tools/techniques worth looking into are [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html) and [transit gateways](https://aws.amazon.com/transit-gateway/). These two constructs help introduce you to routing traffic between VPCs simply ([VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html)) or in a more Enterprise ready, hybrid cloud friendly model ([transit gateways](https://aws.amazon.com/transit-gateway/)).

There's much more we can dig into here, and for the curious and motivated, there's a mountain of public domain knowledge available at your fingertips to learn more. For now, let's bring this back around to design patterns underneath all the diagrams you've seen in this paper.

One of the compelling attributes of the AWS platform is that it has been built on publicly consumable APIs. Why is this compelling? For one thing, this means that much any type of infrastructure component you can run on AWS can be **reproducibly built from code**. When combined with a powerful and comprehensive deployment service such as [AWS CloudFormation](https://aws.amazon.com/cloudformation/), customers have a powerful framework for learning about, customizing, deploying, and managing IT systems. The concept of [Infrastructure as Code](https://infrastructure-as-code.com/) can be new or perplexing for many traditional Enterprise focused technologists, but it can be transformational once adopted and practiced.

As we mentioned earlier, AWS provides a library of CloudFormation based [Quick Start templates](https://aws.amazon.com/quickstart/) which can be run as-is, layered together ('nested') with other templates, and even duplicated and customized for your own specific needs. This library of templates is managed and maintained by AWS, in cooperation with technology partners such as Citrix, and these templates are often open-sourced (meaning they can be duplicated and modified as needed). As of the time of writing, the following Quick Start templates are available for Citrix technologies on AWS:

-  [Citrix Virtual Apps and Desktops Service on AWS](https://aws.amazon.com/quickstart/architecture/citrix-virtual-apps/) - deploys a highly available Citrix Cloud Virtual Apps and Desktops
    Service "Resource Location" on AWS.
-  [Citrix ADC for Web Applications](https://aws.amazon.com/quickstart/architecture/citrix-adc-vpx/) - deploys highly available Citrix ADC VPX instances on AWS. While
    the use case focus differs slightly, this design pattern is functional and relevant for Citrix Gateway deployments with CVAD/CVADS also. Citrix and AWS are working on an extra Quick Start for this specific use case.

We've already discussed how the "Citrix ADC for Web Applications" design pattern relates to the CVADS resource location design pattern in the "Citrix Virtual Apps and Desktops Service on AWS" template, so let's break down a couple foundational templates that underlie both. For simplicity, we call it the "CVADS template."

If we open the **CVADS QuickStart template** in the CloudFormation Designer, we get a visual representation of this template. From this visual, we can see that this template utilizes three separate "nested" stacks or templates:

[Diagram 15: CVADS template in CloudFormation Designer with nested stacks](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_015.png)
*Diagram 15: CVADS template in CloudFormation Designer with nested stacks.*

In Diagram 15, CitrixResourceLocationStack is 'on top', and the other three stacks nested underneath it are foundational stacks, managed and maintained by AWS.

[The "VPCStack" template](https://aws.amazon.com/quickstart/architecture/vpc/) lays down the bulk of the networking foundation underneath the CVADS template. VPCStack is responsible for building the following components of the CVADS stack diagram. All other stacks build on top of these resources and parameters:
[Diagram 16: VPCStack sample build results](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_016.png)
*Diagram 16: VPCStack sample builds results.*

On top of VPCStack comes the RDGWStack and ADStack. The [RDGWStack template](https://aws.amazon.com/quickstart/architecture/rd-gateway/) lays down the infrastructure to provide administrative remote console access to the network built by VPCStack. It is depicted in Orange in Diagram 17. The [ADStack template](https://aws.amazon.com/quickstart/architecture/active-directory-ds/), which runs in parallel to RDGWStack, creates the Active Directory infrastructure for a system. It includes options for creating AD on IaaS instances and also using the AWS Active Directory service. For our example design pattern, it is building the AD DS Service object in red:
![Diagram 17: VPStack + RDGWStack + ADStack sample build results](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_017.png)
*Diagram 17: VPStack + RDGWStack + ADStack sample build results.*

Finally, CitrixResourceLocationStack (the 'master' stack, which calls the three nested stacks) runs, building on top of the three foundation stacks. It is responsible for creating the Cloud Connectors, and VDA on AWS, and it uses the Citrix Cloud APIs to create the resource location, hosting connection, machine catalog, and delivery group in your Citrix Cloud tenant. The end result? A fully functional Citrix Cloud resource location running on AWS:
[Diagram 18: CitrixResourceLocationStack (plus nested stacks) sample build results](/en-us/tech-zone/design/media/reference-architectures_citrix-virtual-apps-and-desktops-on-aws_018.png)
*Diagram 18: CitrixResourceLocationStack (plus nested stacks) sample build results.*

### Summary - Understanding Design Patterns for Citrix on AWS

Confused yet? If so, don't be alarmed: this can well be the start of your Citrix on AWS public cloud journey, and we've merely skimmed the surface of many deep topics here. Hopefully, however, we've successfully illustrated the following salient points:

-  Infrastructure as Code is a powerful concept that can revolutionize the way complete systems are designed, built, and maintained.
-  When deploying systems on AWS' public cloud, different components of any given solution can be represented by code, and built on-demand using AWS CloudFormation and other technologies.
-  These components are represented by stack templates when using AWS CloudFormation, and templates can be copied and modified, as needed, to achieve the desired results.
-  Templates can be nested, building complete systems (such as a fully functioning CVADS resource location on AWS) from the individual design patterns (templates).
-  The [Citrix Virtual Apps and Desktops Service on AWS](https://aws.amazon.com/quickstart/architecture/citrix-virtual-apps/) Quick Start template is built upon three AWS managed/maintained foundation templates, which are well documented. Start with the following links to learn more about each:
    -  The VPCStack template: [https://aws.amazon.com/quickstart/architecture/vpc/](https://aws.amazon.com/quickstart/architecture/vpc/)
    -  The RDGWStack template: [https://aws.amazon.com/quickstart/architecture/rd-gateway/](https://aws.amazon.com/quickstart/architecture/rd-gateway/)
    -  The ADStack template: [https://aws.amazon.com/quickstart/architecture/active-directory-ds/](https://aws.amazon.com/quickstart/architecture/active-directory-ds/)
    -  By using templates and performing trial builds, an Enterprise technologist can learn about, evaluate, and design systems that meet the specific needs of their organization or customer.

### AWS Infrastructure Layer - extra Resources

The following resources can be used to help more about Citrix virtualization on AWS requirements and leading practices:

-  [Communication Ports Used by Citrix Technologies](/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html): a good global reference for the communication ports used by different components of the Citrix virtualization stack.

## Operations Layer Considerations

This section defines the operational activities that administrators perform on a periodic basis. Many of these are not specific to AWS, and are detailed in existing published documentation. In the following tables, we've summarized some of the more important or AWS specific tasks. Readers can refer to the [Monitor topic](/en-us/xenapp-and-xendesktop/7-15-ltsr/citrix-vdi-best-practices/monitor.html) in Citrix product documentation for more information.

### On-Demand Tasks

The following table outlines the tasks that are expected to be performed on-demand based on application requirements and troubleshooting efforts.

| Component | Task | Description |
|-----|-----|-----|
| Generic | Update Knowledge Base | When the Citrix Team troubleshoots issues related to the environment, they are to identify solutions to problems. KBA ought to be created for each issue to help support future troubleshooting activities. |
| Citrix Virtual Apps and Desktops | Modify Image | Images are to be updated as required to support requests. The updates will likely be monthly, but more frequent updates may be required for testing. |
| Citrix Virtual Apps and Desktops | Publish Image | When images are modified, they are tested and published. |
| AWS | Verify instance launch | When a new instance is launched via MCS, verify that the instance has been created in the AWS console, and that there are available IPs in the pool for the given VPC. MCS-provisioned machines will not be created if there are no available IPs in the VPC pool. |
| AWS | Verify on-prem image efficacy | An instance created from any on-prem image ought to be tested for launchability and viability before being used to update production instances. |
| AWS | Modify IAM user/ group permissions | As needed, IAM user and group permissions ought to be reviewed to reduce the number of users with administrative access and to implement the "least privilege" methodology. |
| AWS | Modify Security Groups | As needed, Security Groups are to be reviewed to grant or remove access for different traffic protocols from various IPs or IP ranges. Ingress and egress rules are to be modified to implement network traffic lockdowns. |
| AWS and Citrix Virtual Apps and Desktops | Update machines in a Machine Catalog   | As needed, update machine images to include any necessary modifications. A new AMI must be created of the modified image, and used to update the Machine Catalog. See the Update and Upgrade Process section of this document for more details. |
| AWS and Citrix Virtual Apps and Desktops | Roll back updates to a Machine Catalog | As needed, in the case that a machine image must be rolled back, a previous AMI with the last known working configuration can be used to update machines in the Machine Catalog. |

### Daily Periodic Tasks

The following table outlines the tasks that ought to be performed daily.

| Component | Task | Description |
|-----|-----|-----|
| Generic | Review Citrix Director, Windows Performance Monitor, Event Log, and other monitoring software alerts | Check for warnings or alerts within Citrix Director, event logs, or other monitoring software. Investigate the root cause of the alert if any. **Note:** A computer and monitor can be set up to display the Citrix Director dashboard to create a heads-up display for the Citrix department so that the status of the environment is clearly visible. Monitoring recommendations for Citrix Virtual Apps and Desktops are included in the [Monitoring](/en-us/xenapp-and-xendesktop/7-15-ltsr/citrix-vdi-best-practices/monitor.html) section of the Virtual Apps and Desktops Best Practices guide. |
| Generic | Verify backups completed successfully | Verify all scheduled backups have been completed successfully. This can include but is not limited to user data (user profiles / home folders), application data, Citrix databases, Citrix StoreFront configuration, Citrix license files. |
| Generic | Test environment access | Simulate a connection both internally and externally to validate that desktop and application resources are available before most users log on for the day. This connection is to be tested throughout the day and can even be automated. |
| Citrix Virtual Apps and Desktops | Virtual machine power checking | Verify that the appropriate number of idle desktops and application servers are powered on and registered with the Delivery Controllers to confirm availability for user workloads. |
| AWS | Perform checks for instance health | Check the AWS console to verify the state of the instances and underlying hardware. All instances ought to pass the two health checks when powered on. |
| Citrix Virtual Apps and Desktops | Perform incremental backup of Citrix-related databases | Perform incremental-data backups of the following Citrix databases: Site Database, Configuration Logging Database, Monitoring Database |

### Weekly Periodic Tasks

The following table outlines the tasks that are to be performed on a weekly basis.

| Component | Task | Description |
|-----|-----|-----|
| Generic | Review the latest hotfixes and patches | Review, test, and deploy the latest Citrix [hotfixes](https://support.citrix.com/product/xd/) and ascertain whether the Delivery Controllers and Server-Based OS / Desktop-Based OS virtual machines require them. For Microsoft updates deployed via SCCM or WSUS to machines in AWS, all machines receive these updates when powered on. If Citrix Power Management is employed, there can be machines in the Machine Catalog that are not regular turned on. When performing image updates, it is best to use a dynamic master instance that is powered on during all update cycles. AMIs can then be created from this instance and include all necessary patches. **Note:** Any required hotfixes are to be tested using the recommended testing process before the implementation in Production. |
| Generic | Create Citrix environment status report | Create a report on overall environment performance (server health, resource usage, user experience) and number of Citrix issues (close rate, open issues, and so on). |
| Generic | Review status report | Review the Citrix status report to identify any trends or common issues. |
| Generic | Maintain internal support knowledge base | Create KBA and issue resolution scripts to address Level-1 and Level-2 support requests. Review KBA and issue resolution scripts for accuracy, compliance, and feasibility. |
| Citrix Virtual Apps and Desktops | Check **Configuration Logging reports** | Confirm that Citrix Site-wide changes implemented during the previous week were approved through change control. |
| Citrix Virtual Apps and Desktops | Perform full backup of Citrix-related databases | Perform full-data backups of the following Citrix databases: Site Database, Configuration Logging Database, Monitoring Database. |
| AWS | Perform snapshots of all EBS volumes | All Elastic Block Storage volumes are to be snapshotted on a periodic basis. Snapshots can be managed and groomed in the AWS EC2 console. |

### Monthly Periodic Tasks

The following table outlines the tasks that are to be performed on a monthly basis.

| Component | Task | Description |
|-----|-----|-----|
| Generic | Perform capacity assessment | Conduct environment performance and capacity assessment of the Citrix environment to determine environment utilization and any scalability requirements. Review monthly reports from monitoring tools to assess environment performance and capacity, including, but not limited to: Virtual server computes (CPU and RAM) allocation, Licensing, Network bandwidth. Procure software and or licenses and build extra servers as needed. **Note:** Recommendations for performing a capacity assessment are included in [Decision: Capacity Management](/en-us/xenapp-and-xendesktop/7-15-ltsr/citrix-vdi-best-practices/monitor.html#decision-capacity-management) in the **Monitoring** section of the Citrix Virtual Apps and Desktop Best Practices guide.|
| Generic | Review elevated privilege access | Review which users and groups have elevated permissions to the environment and assess whether ongoing elevated access is required. Remove any accounts that no longer require these administrative rights. Primarily only IAM users and roles that are to be used to assign elevated privileges, with tightly restricted access to individual user, local, or root accounts. |

### Yearly Periodic Tasks

The following table outlines the tasks that are to be performed on a yearly basis.

| Component | Task | Description |
|-----|-----|-----|
| Generic | Conduct Citrix policy assessment | Review Citrix policies and determine whether new policies are required and existing policies must be updated. |
| Generic | Review software upgrades | Review and assess the requirement for new Citrix software releases or versions. |
| Generic | Perform Business Continuity Plan (BCP)/ Disaster Recovery (DR) test | Conduct functional BCP/DR test to confirm DR readiness. This plan is to include a yearly restore test to validate the actual restore process from backup data is functioning correctly. |
| Generic | Perform application assessment | Review the usage of applications outside and within the Citrix environment. Assess the validity of adding more applications to the Citrix Site, removing applications that are no longer required, or upgrading the applications to the latest version. |
| AWS | Assess Network Security Group Accesses | As features or applications are added or removed from the Citrix infrastructure servers or application servers, the Network Security Groups associated with those instances are to also be assessed and modified if necessary, to add or remove any ports or protocols. |

## Sources

Goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sda01a8cdca14f04a).
