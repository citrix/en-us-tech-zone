---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Selecting the best VDI model starts with properly defining user groups and aligning the requirements with the capabilities of the VDI models. Learn how different factors play a role in selecting the correct VDI model for a user group.
tz_title: VDI Model Comparison
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: VDI Model Comparison

## Overview

Selecting the best VDI model starts with properly defining user groups and then aligning the requirements with the capabilities of the VDI models. Although there are multiple approaches towards defining user groups, it is often easiest to align user groups with departments. Typically most users within the same department or organizational unit consumes the same set of applications.

## User Segmentation

Depending on the size of the department, there might be a subset of users with unique requirements. Each defined user group is evaluated against the following criteria to determine if the departmental user group needs to be further divided into more specialized user groups.

*  Primary data center – Each user has a primary data center or cloud resource location assigned that hosts their virtual desktop, data, and application servers. Identify the data center that the user is assigned to rather than the data center they are currently using. Users are grouped based on their primary data center so that a unique design can be created for each one.
*  Personalization – Personalization requirements are used to help determine the appropriate VDI model for each user group. For example, if a user group requires complete personalization, a personal desktop is recommended as the optimal solution. There are three classifications available:

| Personalization | Requirement |
| :--- | :--- |
| None | User cannot modify any user or application settings, for example - a kiosk. |
| Basic | User can modify user-level settings of desktops and applications. |
| Complete | User can make any change, including installing applications. |

*  Security – Security requirements are used to help determine the appropriate desktop and policy (or policies) for each user group. For example, if a user group requires high security, a hosted pooled desktop or a local VM desktop is recommended as the optimal solution. There are three classifications available:

| Security Level | Description |
| :--- | :---|
| Low | Users are allowed to transfer data in and out of the virtualized environment. |
| Medium | All authentication and session traffic is be secured; users are not be able to install or modify their virtualized environment. |
| High | In addition to traffic encryption, no data leaves the data center (such as through printing or copy/paste); all user access to the environment is audited. |

*  Mobility – Mobility requirements are used to help determine the appropriate desktop model for each user group. For example, if a user group faces intermittent network connectivity, then any VDI model requiring an active network connection is not applicable. There are four classifications available:

| Mobility | Requirement |
| :--- | :--- |
| Local | Always uses the same device, connected to an internal, high-speed, and secured network. |
| Roaming Local | Connects from different locations on an internal, high-speed, secured network. |
| Remote | Sometimes connects from external variable-speed, unsecure networks. |

*  Desktop Loss Criticality – Desktop loss criticality is used to determine the level of high availability, load balancing, and fault tolerance measures required. For example, if there is a high risk to the business if the user’s resource is not available, the user is not allocated a local desktop. If that local desktop fails, the user is not able to access their resources. There are three classifications available:

| Desktop loss criticality | Requirement |
| :--- | :--- |
| Low | No major risk to products, projects, or revenue. |
| Medium | Potential risk to products, projects, or revenue. |
| High | Severe risk to products, projects, or revenue. |

*  Workload – Types and number of applications accessed by the user impacts overall density and the appropriate VDI model.  Users requiring high-quality graphics must utilize a local desktop implementation or a professional graphics desktop. There are three classifications available:

| User Type | Characteristics |
| :--- | :--- |
| Light | 1–2 office productivity apps or kiosk. |
| Medium | 2–10 office productivity apps with light multimedia use. |
| Heavy | Intense multimedia, data processing, or application development. |

**_Note:_** Performance thresholds are not identified based on processor, memory or disk utilization because these characteristics will change dramatically following the application rationalization and desktop optimization process. It is likely that the user’s management tools and operating system will change during the migration process. Instead, workload is gauged based on the number and type of applications the user runs.

| Type | Experience from the field |
| :--- | :--- |
|Utility Company | A large utility company collected data on every user in their organization. During the user segmentation process, it was realized that the organization’s existing role definitions were sufficiently well defined that all the users within a role shared the same requirements. This allowed a significant amount of time to be saved by reviewing a select number of users per group. |
| Government | A government organization discovered that there was significant deviation between user requirements within each role, particularly around security and desktop loss criticality. As such, each user needed to be carefully reviewed to ensure that they were grouped appropriately |

## Assign VDI Models

As with physical desktops, it is not possible to meet every user requirement with a single type of VDI. Different types of users need different types of resources. Some users may require simplicity and standardization, while others may require high levels of performance and personalization. Implementing a single VDI model across the entire organization inevitably leads to user frustration and reduced productivity.
Citrix offers a complete set of VDI technologies that have been combined into a single integrated solution. Because each model has different strengths, it is important that the right model is chosen for each user group within the organization.
The following list provides a brief explanation of each VDI model.

*  Hosted Apps – The hosted apps model delivers only the application interface to the user. This approach provides a seamless way for organizations to deliver a centrally managed and hosted application into the user’s local PC. The Hosted Apps model is often utilized when organizations must simplify management of a few line-of-business applications. Hosted apps include a few variants:
    *  Windows Apps – The Windows apps model utilizes a server-based Windows operating system, resulting in a many users accessing a single VM model.
    *  VM hosted apps – The VM hosted apps model utilizes a desktop-based Windows operating system, resulting in a single user accessing a single VM model. This model is often used to overcome application compatibility challenges with a multi-user operating system, like Windows 2008, Windows 2012 and Windows 2016.
    *  Linux Apps – The Linux apps model utilizes a server-based Linux operating system, resulting in a many users accessing a single VM model.
    *  Browser Apps – The browser apps model utilizes a server-based Windows operating system to deliver an app as a tab within the user’s local, preferred browser. This approach provides a seamless way for organizations to overcome browser compatibility challenges when users want to use their preferred browser (Internet Explorer, Microsoft Edge, Google Chrome, Mozilla Firefox and so on) but the web application is only compatible with a specific browser.
*  Shared Desktop – With the shared desktop model, multiple user desktops are hosted from a single, server-based operating system (Windows 2008, 2012, 2016, Red Hat, SUSE, CentOS, and Ubuntu). The shared desktop model provides a low-cost, high-density solution; however, applications must be compatible with a multi-user server based operating system. In addition, because multiple users share a single operating system instance, users are restricted from performing actions that negatively impact other users, for example installing applications, changing system settings, and restarting the operating system.
*  Pooled Desktop – The pooled desktop model provides each user with a random, temporary desktop operating system (Windows 7, Windows 8, and Windows 10). Because each user receives their own instance of an operating system, overall hypervisor density is lower when compared to the shared desktop model. However, pooled desktops remove the requirement that applications must be multi-user aware and support server based operating systems.
*  Personal Desktop – The personal desktop model provides each user with a statically assigned, customizable, persistent desktop operating system (Windows 7, Windows 8, Windows 10, Red Hat, SUSE, CentOS, and Ubuntu). Because each user receives their own instance of an operating system, overall hypervisor density is lower when compared to the shared desktop model. However, personal desktops remove the requirement that applications must be multi-user aware and support server based operating systems.
*  Pro Graphics Desktop – The pro graphics desktop model provides each user with a hardware-based graphics processing unit (GPU) allowing for higher-definition graphical content.
*  Local Streamed Desktop – The local streamed desktop model provides each user with a centrally managed desktop, running on local PC hardware
*  Local VM Desktop – The local VM desktop model provides each user with a centrally managed desktop, running on local PC hardware capable of functioning with no network connectivity.
*  Remote PC Access – The Remote PC Access desktop model provides a user with secure remote access to their statically assigned, traditional PC. The Remote PC Access desktop model is often the fastest and easiest VDI model to deploy as it utilizes already deployed desktop PCs.

Each user group is compared against the following table to determine which VDI model best matches the overall user group requirements. In many environments, a single user might utilize a desktop VDI model and an app VDI model simultaneously.

| User Segmentation Category | User Segmentation Characteristic | Hosted Apps | Hosted Shared Desktop | Hosted Pooled Desktop | Hosted Personal Desktop | Hosted Pro Graphics Desktop | Remote PC Access |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Workload**
| | **Light** | Best | Good  | Good  | Good  | Bad | Good |
| | **Medium** | Good | Best | Best | Good | Good | Good |
| | **Heavy** | Bad | Bad | Bad | Good | Best | Good |
| **Mobility**
| | **Local** | Best | Best | Best | Good | Good | Good |
| | **Roaming Local** | Best | Best | Best | Good | Good | Good |
| | **Remote** | Best | Best | Best | Good | Good | Best |
| **Personalization** |
| | **None** | Best | Best | Best | Bad | Good | Good |
| | **Basic** | Best | Best | Best | Bad | Good | Good |
| | **Complete** | Bad | Bad | Bad | Best |  | Best |
| **Security** |
| | **Low** | Good | Good | Good | Good | Good | Good |
| | **Medium** | Best | Best | Best | Good | Good | Good |
| | **High** | Good | Good | Best | Bad | Good | Bad |
| **Criticality** |
| | **Low** | Good | Good | Good | Good | Good | Good |
| | **Medium** | Best | Best | Best | Good | Good | Bad |
| | **High** | Best | Best | Best | Bad | Good | Bad |

Don’t forget to follow these top recommendations from Citrix Consulting based on years of experience:

Citrix Consulting Tips for Success

1.  Start with Windows apps, shared desktops, and pooled desktops – As you can see in the VDI capability table, the Windows apps, hosted shared desktop, and pooled desktop models can be used in most situations. By reducing the number of VDI models required, you help reduce deployment time and simplify management.
1.  Perfect match – It might not be possible to select a VDI model that is a perfect match for the user group. For example, you can’t provide users with a desktop that is highly secure and offers complete personalization at the same time. In these situations, select the VDI model which is the closest match to the organization’s highest priorities for the user group.
1.  Desktop loss criticality – There are only three VDI models that meet the needs of a high desktop loss criticality user group (backup desktops available) – none of which allow for complete personalization. If a high-desktop loss criticality user group also requires the ability to personalize their desktop they are provided with a pool of backup desktops (hosted shared, pooled) in addition to their primary desktop. Although these desktops would not include customizations made to their primary desktop, they would allow users to access core applications such as mail, Internet, and Microsoft Office.
1.  Consider Operations & Maintenance – The ongoing support of each VDI model should be factored in when deciding on a VDI model. For example, pooled desktops can be rebooted to a known good state which often leads to reduced maintenance versus a personal desktop where each desktop is unique.
