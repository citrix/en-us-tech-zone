---
layout: doc
description: Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.
---
# Citrix Virtual Apps and Desktops Single-Server Scalability

## Contributors

**Author:** [Nick Rintalan](https://www.citrix.com/blogs/author/nicholasr/)

## Overview

This article provides recommendations and guidance to estimate how many users or virtual machines (VMs) can be supported on a single physical host. This is commonly referred to as Citrix Virtual Apps and Desktops “single-server scalability” (SSS). In the context of Citrix Virtual Apps (CVA) or session virtualization, it is also commonly called “user density”. The idea is to ascertain how many users or VMs can be executed on a single piece of hardware running a major hypervisor such as Citrix Hypervisor.

In this article, we cover several of the variables or factors that influence Citrix Virtual Apps and Desktops (CVAD) SSS. We provide recommendations and simple guidelines to quickly estimate SSS for a given environment. We conclude by providing a few sizing examples using real-world scenarios.

**Warning!** This article contains guidance to estimate SSS. It should be noted that the guidance is high level and will not necessarily be specific to your unique situation or environment. The only way to truly understand CVAD SSS is to utilize a scalability or load testing tool such as Login VSI. Citrix recommends using this guidance and these simple rules to quickly estimate SSS. But Citrix recommends using Login VSI or the load testing tool of your choice to validate results, especially before purchasing hardware or making any financial decisions.

## Scalability Factors

There are many factors, parameters or variables that impact SSS. This is by no means an exhaustive list but the following are several of the major factors that impact SSS. While there are many more factors that influence performance & scalability such as antivirus and monitoring agents, graphics encoding, recent security vulnerabilities such as Spectre and L1 Terminal Fault, the factors detailed below typically have the greatest impact on CVAD SSS. Now let’s look at how to quickly estimate CVAD SSS using a simple formula.

### Workload

One of the primary factors that impacts performance & scalability is the workload itself. Some workloads might involve task workers performing simple data entry tasks on a CVA server. Other workloads might involve developers compiling code or engineers manipulating 3D models via Citrix Virtual Desktops (CVD). These are commonly referred to as “light” and “heavy” workloads, respectively. And as you’ll see later in this article, this type of workload variance can have a dramatic impact on SSS.

### Hardware

The physical hardware that the workloads run on have a direct impact on SSS. It probably goes without saying but a newer server equipped with 28 cores and 1 TB of RAM will be able to support more users than an older piece of hardware with only 12 cores and 256 GB of RAM running a similar workload. And the CPU plays an especially important part in CVAD SSS as you’ll see later.

### Activity Ratio

One of the often overlooked aspects of SSS is the activity ratio or how often users are working vs. idle. Many scalability testing tools take a conservative approach and might utilize a fairly high activity ratio like 80% (which effectively means users are active or working 80% of the time and idle 20% of the time). However, we often see in the real world that activity ratios are closer to 40-60%. And this additional idle time can dramatically impact SSS and how much hardware one needs to purchase to support a CVAD environment.

### CPU Over-Subscription Ratio

Most CVAD workloads are CPU-bound, meaning that the ultimate point of resource exhaustion is directly related to the number of physical cores available in the system. And since users may not be active 100% of the time and we have tools such as Intel Hyper Threading available (not to mention hypervisor CPU schedulers are becoming increasingly efficient), we can often “over-commit” or over-subscribe resources such as CPU. And the ratio that one over-subscribes the CPU can also impact SSS (in a positive or negative manner if not done carefully). Citrix has found that a 2:1 CPU over-subscription ratio is often optimal when it comes to CVA SSS. For example, if a physical server is equipped with dual socket 20 core chips (i.e. “2 x 20”), there are 40 total physical cores available. And with Hyper Threading enabled, there would be 80 virtual or logical cores available. By applying a 2:1 ratio to the number of physical cores (40), we’d effectively recommend utilizing 80 cores when sizing or estimating SSS. More examples are provided at the end of this article to illustrate this concept in further detail.

### Microprocessor Architecture and Features

The underlying chip and memory architecture can also play an important role in SSS. And Intel has recently made significant improvements when it comes to the underlying microprocessor architecture design. On older chips, such as Broadwell and Haswell, Intel connected processors using a ring-based architecture. But as the number of cores increased, access latency increased and bandwidth per core diminished so Intel would mitigate this by splitting the chip into two halves and adding a second ring to reduce distances. And this invisible split was something that needed to be factored into CVAD SSS to provide optimal results. This has been referred to in the past as “NUMA” or Non-Uniform Memory Access. And the leading guidance was to ensure that you are sizing CVA VMs as large as possible but not crossing NUMA nodes, sub-NUMA clusters or rings at the same time. If you sized your CVA VMs too large and they effectively spanned NUMA nodes or rings, it can lead to NUMA “thrashing” by accessing non-local resources and this would yield reduced SSS. Fast-forward to today and Intel has moved from a ring-based architecture to a mesh-based architecture. And this new mesh architecture introduced in Skylake does not have the same limitations as before where we have to split chips, divide cores or add rings. And this changes the way we size CVA servers in particular. So it’s important to understand the specific chip that is being used in the hardware you purchase and how the underlying microprocessor architecture is designed and constructed

## The Magic Multipliers

If you’d like to quickly gauge or estimate CVAD SSS, this guidance is pretty effective. It’s as easy as this – *take the number of physical cores in a server, multiply it by 5 or 10, and the result will be your SSS.* If you’re looking for the number of CVD VMs you can support, then you would use the “magic multiplier” of 5. If you’re trying to understand how many CVA users can run on a single piece of hardware, you’d use 10. Let’s look at a few real-world examples to see how this is applied in practice.

### Example 1: Citrix Virtual Desktops

Let’s assume you’re running Windows 10 with standard Office applications and a few custom applications. You’ve identified that a 2 vCPU / 4 GB RAM VM specification would work best given the workload/image. You’re considering purchasing Cisco blades with 36 physical cores (2x18) and 768 GB of RAM. And you’d like to understand what kind of density you can expect. Let’s utilize the Rule of 5 for CVD:

> 5 x 36 = 180 VMs per host

### Example 2: Citrix Virtual Apps (older hardware)

Let’s assume you’re running an application such as SAP on Windows Server 2012 R2 via CVA. You’re repurposing some older HP blades with 24 physical cores (2x12) and 256 GB of RAM. You’ve researched on Intel’s website that the underlying chip employs a ring buffer architecture and each socket is effectively split into 2 NUMA nodes with 6 cores each. Therefore, a 6 vCPU / 24 GB RAM VM specification seems optimal to maximize linear scalability and minimize NUMA thrashing. Using a 2:1 CPU over-subscription ratio, you utilize all 48 logical cores and deploy 8 XenApp servers on each host (48 / 6 = 8). Utilizing the Rule of 10 for CVA:

> 10 x 24 = 240 users per host

### Example 3: Citrix Virtual Apps (newer hardware)

Let’s assume you’re running a popular healthcare application on Windows Server 2016 via CVA. You’re considering purchasing Dell blades with 32 physical cores (2x16) and 256 GB of RAM. You’ve researched on Intel’s website that the underlying chip employs a mesh architecture and there is a business directive to decrease your VM footprint as much as possible. You decide on a 16 vCPU / 48 GB RAM VM specification. Using a 2:1 CPU over-subscription ratio, you utilize all 64 logical cores and deploy 4 XenApp servers on each host (64 / 16 = 4). Utilizing the Rule of 10 for CVA:

> 10 x 32 = 320 users per host

As mentioned previously, we realize there are many more variables or parameters that influence scalability versus just the number of physical cores in a server. And there may be certain situations where the CVAD workload is not CPU-bound so extra care is required when sizing. In addition, other factors we haven’t discussed such as CPU clock speed and logon storms also matter and further complicate sizing exercises. But we have found through years of field experience and hundreds of deployments that nothing matters as much as the number of physical cores. If you’d like to understand your exact SSS for your particular workload and unique environment, Citrix strongly recommends utilizing a tool such as Login VSI to properly test and/or validate results.

## References

[Intel Xeon Processor Scalable Family Technical Overview](https://software.intel.com/en-us/articles/intel-xeon-processor-scalable-family-technical-overview)

[Antivirus Best Practices](/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html)

[Login VSI Load Testing](https://www.loginvsi.com/products/application-and-capacity-load-testing)

## Other Tech Zone content

[Learn -> PoC Guides -> Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html) - Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and your on premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.

[Learn -> PoC Guides -> Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html) - Learn how to deliver the Citrix® HDX™ Optimization for Microsoft® Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops-service.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop deployment in Citrix Cloud.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops On-Prem](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop on-premises deployment.

[Learn -> Tech Insight -> App Layering - User Layers](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html) - User layers persist user profile settings, data, and user-installed applications in non-persistent VDI environments.

[Learn -> Tech Insight -> Remote PC Access](/en-us/tech-zone/learn/tech-insights/remote-pc-access.html) - Remote PC Access allows users to access their physical, office-based Windows PC from remote locations.

[Learn -> Tech Insight -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/tech-insights/virtual-apps-and-desktops-service.html) - Citrix Virtual Apps and Desktops Service provides a fast, low-impact deployment option for on-premises/cloud-hosted, Windows/Linux, desktops/apps.

[Learn -> Tech Insight -> Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) - Single Sign-on to Windows-based virtual apps and desktops when using a non-Active Directory based Citrix Workspace identity.

[Learn -> Tech Insight -> HDX - Display and Adaptive Technologies](/en-us/tech-zone/learn/tech-insights/hdx-adaptive-display.html) - Technologies that ensure an unparalleled user experience when connecting to remote Citrix resources.

[Design -> Reference Architectures -> Remote PC Access](/en-us/tech-zone/design/reference-architectures/remote-pc.html) - Discover the use cases and learn about the detailed architecture of Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.

[Design -> Reference Architectures -> Optimizing Unified Communications Solutions](/en-us/tech-zone/design/reference-architectures/optimizing-unified-communications-solutions.html) - Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Remote PC Access](/en-us/tech-zone/design/design-decisions/remote-pc-access.html) - When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Design Decisions -> Designing StoreFront and Multi-Site Aggregation](/en-us/tech-zone/design/design-decisions/storefront-multisite-aggregation.html) - Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.

[Design -> Design Decisions -> VDI Model Comparison](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html) - Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.

[Build -> Tech Papers -> Deploying Google Chrome](/en-us/tech-zone/build/tech-papers/google-chrome.html) - Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
