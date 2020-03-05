---
layout: doc
description: Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.
---

# Choosing the Provisioning Model for Image Management

## Contributors

**Author:** [Martin Zugec](https://www.twitter.com/martinzugec)

One of the most common design decision that needs to be done for every Citrix Virtual Apps and Desktops (CVAD) project is which provisioning model will meet the business and operational requirements. The goal of this article is to describe the most common decision factors, recommendations and different scenarios where certain provisioning model might be a better candidate. For image management, there are two provisioning models that are commonly used by Citrix administrators to manage their Citrix environment efficiently:

-  Machine Creation Services (MCS)
-  Citrix Provisioning (PVS)

It is also important to mention that Citrix App Layering is out of scope for current version of this document. Implementation of Citrix App Layering can influence many of these design decisions and we will include it in one of the future updates for this article.

This article is specifically focused on design decisions and factors involving image provisioning. If you are interested in more general reference architecture for Citrix PVS or MCS, we highly recommend to read [Citrix Virtual Apps and Desktops Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) reference architecture.

## Overview of Citrix Provisioning Services (PVS)

Citrix Provisioning is a software-based streaming technology that can deliver centralized, shared operating system image to multiple virtual or physical endpoints. For design purposes, it is important to understand that PVS is **active** component - it is actively involved in daily operations of image management and delivery. As an advantage, Citrix PVS can reduce the operational and storage costs, since it acts as a software-based storage offloading solution. This however means that environment needs to be properly designed and maintained. Citrix PVS requires dedicated set of streaming servers, database and needs to be included in high-availability planning. Design of PVS environment is mostly hypervisor agnostic and implementations are similar for different hypervisors.

## Overview of Citrix Machine Creation Services (MCS)

Citrix Machine Creation Services is an orchestration component of Citrix Virtual Apps and Desktops that can provide single image management for shared or dedicated machines. MCS is **passive** component - in majority of deployments, MCS is involved only in the image build orchestration process (telling hypervisor what and where to do) and not in daily operations and delivery of images. There are some exceptions to this rule, most notably for hypervisors that cannot automatically reset disks. From design perspective, environments built using MCS will mostly inherit behavior and characteristics of hypervisor or cloud provider that is hosting workloads. Design of MCS environment is therefore heavily influenced by combination of hypervisor and storage used.

## Provisioning Decision Factors

Each project and environment is unique and have different requirements and goals. For that reason, it is very common that a good architect will choose different provisioning models for different projects and not exclusive prefer only one. It is very common that different provisioning models are used even in a same environment - for example when providing a combination of dedicated and shared machines.

For purposes of this document, we are going to divide decision factors in two categories - factors where it is very clear which provisioning model should be preferred (or have to be used) and factors that are more open to interpretation and where personal preference / experience is playing much bigger role in decision making.

## Explicit Decision Factors

Explicit decision factors cover scenarios where PVS or MCS is not only preferred, but often there is only one possible option.

![Explicit Decision Factors](/en-us/tech-zone/design/media/design-decisions_image-management_diagram-explicit.png)

### Physical Machines

If your project involves provisioning to physical machines (typically classrooms or similar use cases), only Citrix PVS is supported (and functional). This model works well for standardized desktops such as those in lab and training environments, call centers, and “thin client” devices used to access virtual desktops.

**Recommended model:** PVS

### Cloud Deployment

If you are planning to run virtual apps and desktops in one of the public infrastructure as a service (IaaS) environments like Microsoft Azure, Amazon Web Services or Google Cloud Platform, be aware that such environments do not support Citrix PVS. There are technical limitations that prevent PVS from running in such environments - most notably the requirement for PXE firmware, which is required even when BDM boot partition is used. While this limitation prevents you from running Citrix PVS in true cloud environments, it is still possible to run them in hosted virtualized environments.

**Recommended model:** MCS

### Persistent Desktops

When deploying virtual desktops in persistent mode, these are most common approaches:

-  Full clones with MCS
-  Fast clones with MCS
-  User layers
-  Manual / ESD provisioning (SCCM)
-  Hypervisor templates

While it is theoretically possible to provide dedicated desktops with Citrix PVS using private image mode, this approach is not recommended and does not provide any operational or performance benefits. Without the use of separate technology for persistent user layers, Citrix PVS is not recommended model for persistent machines.

When using MCS for persistent machines, there are two possible approaches - using fast clones or full clones (introduced in version 7.11). While fast clones with MCS provides the benefit of small storage footprint and fast create and reset times (small delta disks), storage migration / backups / high-availability is more complicated in this deployment model. Since this is often a requirement for dedicated / persistent machines, full clones with MCS is usually recommended approach to persistent desktops. You can read more about difference between fast and full clones in [article CTX224040](https://support.citrix.com/article/CTX224040).

![Fast vs Full Clone](/en-us/tech-zone/design/media/design-decisions_image-management_fast-full-clones.png)

**Recommended model:** MCS

### License Entitlement

Citrix PVS is not available to all license editions - most notably, you are not eligible to use it with Virtual Apps Standard (formerly XenApp Advanced) and Virtual Apps Advanced (formerly XenApp Enterprise) licenses. It is a common misunderstanding that it is possible to use lower editions of Citrix Virtual Apps with purchase of Provisioning Services Datacenter Edition, however this is specifically excluded in license agreement. For more information, read [Products and License Models](https://www.citrix.com/buy/licensing/product.html).

**Recommended model:** MCS

## Variable Decision Factors

While previous set of decision factors are very straightforward, this second set is much more flexible, open to interpretation and personal preference / experience with technology plays a much bigger role in decision making. Citrix gives you flexibility to choose the best solution for your needs and your decision decisions for following factors might be different than our recommendations.

![Variable Decision Factors](/en-us/tech-zone/design/media/design-decisions_image-management_diagram-variable.png)

### Technical Skills

This is an important factor to consider, especially if you are a Citrix partner and you are building a green field environment for new customer. Consider the skills and capabilities of team that is going to manage this environment - if customer is new to Citrix technologies, is operating a static environment with minimum changes or if they have multiple roles and Citrix management is only subset of their responsibilities, it might be a good idea to minimize the complexity of environment and and reduce the number of moving parts. In that case, MCS might be a better solution.

**Recommended mode:** MCS if technical skills are concern

### Familiarity With Provisioning Model

Many of Citrix customers and partners are very familiar with Citrix PVS or MCS and have provisioned thousands of machines using this technology. This is very important factor when deciding which provisioning model to use - if you and the rest of your team is very familiar with procedures and technical aspects of one solution and that solution meets all your requirements and needs, use it for your new project. However, if you are a partner or build solution for 3rd party, it is important to consider learning curve and skill set of your customer.

Both PVS and MCS can support complex architectures and large environments - personal preference / experience is very often the most important decision factor involved in choosing one.

**Recommended mode:** PVS or MCS, whichever model is more familiar

### Complex Multi-Site Architecture

In certain type of environments, ability to quickly replicate images across multiple storage repositories is critical. With PVS, this replication is very simply and usually involves simple copy operation across different file shares. With enterprise MCS design, this requires replication of master image itself together with automated image provisioning using CVAD SDK / PowerShell.

While it is possible to automate multi-site deployments with MCS, PVS process is simpler and easier to use.

**Recommended mode:** PVS preferred

### Requires Frequent Changes

One of the biggest advantages of Citrix PVS is ability to almost instantly switch from one virtual disk (vDisk) to another and support for advanced versioning of virtual images. It is possible to achieve similar results with MCS using rolling catalogs and versioning on master image level, however this process is simpler with PVS.

For environments that require frequent changes (multiple image changes every week), PVS might offer a more flexible, out of the box solution. There are more factors involved in this decision - for example how long does it take to update images using MCS in your environment and how many storage repository replications are required, but as a general rule, you can expect PVS to be more flexible image management solution.

**Recommended mode:** PVS preferred

### Size of Environment

One factor that is **not** as important as many people believe is the scale of target environment. Both PVS and MCS are enterprise ready solutions that can scale to tens of thousands of machines.

When scale is a potential decision factor is if you are designing very small and simplistic environment - unless there are some other factors involved (such as provisioning to physical machines), MCS is preferred method for smaller environments (tens of machines).

**Recommended mode:** MCS for smaller environments, PVS/MCS for larger environments

### Network Bottleneck

Citrix PVS is sensitive to properly working network environment - whether it's proper routing/size of packets or stable network connection. Because it is using a hybrid UDP traffic, the impact of dropped packets can be substantial, as it requires a repeat of the whole sequence of packets. If network performance and/or stability is a concern, MCS (preferably not using NFS) might be a better approach.

**Recommended mode:** MCS if network stability is a concern

### Requires Persistent Disk

With PVS, it is possible to store persistent data on write cache disk. This capability is possible with MCS as well, however it requires additional scripting and automation skills. If you are not willing to automate this procedure or don't have required skills, using out-of-box functionality of PVS might be a better option.

**Recommended mode:** PVS preferred

### Optimized Hypervisor and/or Storage

As mentioned at beginning, PVS is mostly hypervisor agnostic solution, while performance, stability and flexibility of MCS has a very strong dependency on underlying hypervisor and storage.

If your underlying infrastructure is however optimized and designed to work properly with MCS, it is possible to achieve better results with MCS, as you are going to use hardware-acceleration instead of software acceleration.

The most notable candidate to mention here is Nutanix implementation of Shadow Clones, which is optimized for MCS provisioning. Another good examples are hypervisors that are optimized for virtual desktop workloads - for example Citrix Hypervisor with support for [In-memory Read Caching or IntelliCache](https://support.citrix.com/article/CTX201887).

**Recommended mode:** MCS if using hypervisor/storage optimized for MCS

## Summary

In this article, we have discussed most common decision factors when choosing provisioning method for your Citrix Virtual Apps and Desktops environment. Both Citrix PVS and MCS are enterprise-ready solutions that offer great performance and flexibility.

[![Decision Factors](/en-us/tech-zone/design/media/design-decisions_image-management_diagram-full.png)](/en-us/tech-zone/design/media/design-decisions_image-management_diagram-full.png)
