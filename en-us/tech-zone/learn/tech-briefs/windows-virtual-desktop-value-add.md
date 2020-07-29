---
layout: doc
description: Learn about the value add Citrix provides to your Windows Virtual Desktop environment running in Microsoft Azure. Citrix Virtual Apps and Desktops service and Citrix Managed Desktops service provide a cloud-based management, provisioning, and capacity management solution for delivering virtual apps and desktops to any device. See how cost savings can be achieved while delivering a superlative user expereince and enhanicng the security posture of our deployment. 
---
# Enhancing Windows Virtual Desktop

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** [Daniel Feller](https://twitter.com/djfeller)

## Overview

Windows Virtual Desktop (WVD) is a platform in Microsoft Azure to host and manage virtual machines. It is not just a set of operating systems that can be run in Azure but a set of services that can be used to deliver the virtual desktops to users. Staying true to Citrix’s track record, from the earliest days of our partnership with Microsoft, our solutions add unique value to this platform.

Let us first look at the WVD architecture:

[![Windows Virtual Desktop architecture](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_1-wvd-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_1-wvd-architecture.png)

It consists of the core level compute, networking, and storage that makes up the physical infrastructure that the Azure cloud runs on (which are managed by Microsoft). Then there are the virtual machines that run on the cloud hardware – Windows single-session and multi session desktops / server OS machines and remote apps. And the shared storage exposed to the machines like Azure Files and Azure AD and related services. The expectation is that this layer is managed by the customer. And finally, the services that run to manage and provide access to the desktops and applications.

For organizations to utilize the value add that Citrix provides, the bottom two layers of the WVD platform are retained. The top management layer is replaced by Citrix virtualization cloud services, including Citrix Virtual Apps and Desktops (CVAD) service and Citrix Managed Desktops service.

[![Windows Virtual Desktop Citrix Value Add architecture](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_2-wvd-architecture-citrix-value-add.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_2-wvd-architecture-citrix-value-add-large.png)

The on-premises data center is now included in the deployment and Remote PC Access enables connectivity to physical machines from the same environment / leveraging existing security. Citrix virtualization cloud services unify external access and identity management.

The Citrix value add is tabulated in the following table:

|   |Theme   |Feature   |Value Add   |
|---|---|---|---|
| 1  | Experience  | [HDX](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#HDX)  | Over 30 years of expertise connecting remote hosts to endpoints over latent networks. Reduces data on the wire and enables several optimizations and peripherals. Citrix sessions connecting directly to the session host. Adaptive display technologies are customizable for individual apps. 3D Optimizations for CAD and manufacturing use cases.  |
| 2  | Choice  | [Hybrid Platform Management](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Management_of_the_environment)  | Consume Windows Virtual Desktop as needed (burst and Disaster Recovery / Business Continuity) while continuing to manage on-premises workloads from a single management plane. Remote PC Access is also managed and secured similarly from the same console. Support of Non-domain joined users  |
| 3  | Experience  | [Profile Management](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Profile_Management), [WEM](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#WEM), and [Azure Files Integrations](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Azure_Files_Integrations)  | Extend FSLogix profile containers for multi-session access using Citrix Profile Management. Workspace Environment Managements helps control compute costs by automatically managing applications. Accelerates logon to WVD and increases single server scalability  |
| 4  | Security  | [Session Watermarking](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Session_Watermarking) and [Session Recording](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Session_Recording), [Multifactor authentication](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Multifactor_authentication) & Smart cards   | Compliance and regulatory requirements met. MFA extended to several IDPs natively and others via SAML. Smart card support. Endpoint Analysis scans and granular policy control over the content and the user can access  |
| 5  | Management  | [Provisioning](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Provisioning)  | Automated GUI based provisioning with versioning and rollback support. Autoscale helps reduce compute cost in the cloud. Machine Creation Services including MCS I/O optimization and On Demand Provisioning, reduce premium disk costs. Zone preference helps with identifying on-prem or reserved instances to be used ahead of pay-as-you-go instances  |
| 6  | Monitoring  | [Director / Monitoring and Citrix Analytics](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Monitoring_and_Analytics) | A user centric monitoring system that helps pinpoint and resolve user/application issues (Shadows user session, send messages, disconnect / logoff sessions, logon duration drill-down) from one place in addition to alerting (Session / app launch failures, resource consumption, and predictive analysis) and help desk integration with ITSM. Citrix Analytics enables advanced performance and security issue drill-down with automated real-time remediation  |
| 7  | Choice  | [Collaboration Platforms and Content redirection](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Resource_Optimization)  | Unified communications optimization extends beyond Teams to Skype for Business, Zoom, Jabber and so on. Browser Content redirection reduces data ingress and egress costs while offloading media rendering to the client, increasing server scalability  |
| 8  | Networking Integrations  | [Citrix Gateway and SD-WAN](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Networking_Solutions)  | Citrix Gateway POPs improve performance by connecting through the nearest gateway POP. Citrix SD-WAN allows the WVD environment to connect back to the on-premises data / environment and enables break out of Internet based traffic and HDX content optimizations to reduce  data ingress and egress costs and improving user experience  |
| 9  | Management  | Delegated Administration and Configuration Logging  | Granular control over administrative rights from help desk staff to IT owner coupled with full tracking of environment changes with date/time/admin action  |
| 10  | Workspace Experience  | [Citrix Workspace](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Citrix_Workspace)  | Citrix Workspace adds intelligent capabilities to organize, guide, and automate work in a single place, using microapps, universal search, Citrix assistant, relevant notifications and so on.  |

This tech brief will showcase the value add provided by various feature sets in Citrix products through all the phases of setting up a workspace and using the WVD resources hosted Azure.

### Management of the environment

Effective management of the Windows Virtual Desktops environment is important to be able to reduce cost while ensuring the best possible user experience. The simpler it is to manage the environment and the quicker it is to remediate user issues; the easier the administrator’s job is. To that end Citrix provides several features:

*  [Hybrid Cloud Management](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Mybrid_Cloud_manageent) from a single console
*  [Image management and brokering](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Image_Manaement_and_Brokeing)
*  Cost optimization using [active power management](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Autoscale), [Azure On-Demand provisioning](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Azure_On-Demand_Provisioning), and [MCS I/O optimization](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#MCS_I/O_optimization)

#### Hybrid Cloud management

Any organization that is not creating their environment from scratch in the cloud, is going to have a transition period where some existing resources will reside in the on-premises data center, while WVD resources are in Azure. Some on-premises resources may never move to the cloud due to security / compliance / business / data affinity reasons. This is where CVAD service with the ability to seamlessly manage any deployment, whether it be the on-premises data center or the Azure based WVD resources. And that from the same CVAD console, makes the life of the administrator so much easier.

CVAD service can help admins to manage their resources in other clouds as well, all from the same console. The console is functional and very easy to use. Everything is within reach without overwhelming the admin with choices. The console is designed to allow for the admin to deploy Azure based workloads without being an Azure expert. It includes searchability for users and groups when assigning workloads.

Let’s dig right into it with the first step needed to be performed in the process of getting a cloud-based Desktop environment running.

#### Image management and brokering

A well tested imaging solution allows administrators to easily manage VMs throughout their lifecycle and rapidly scale their environment whether dedicated or non-persistent.

To get several virtual machines running in the cloud, a Golden Image must be created. This can be done with either SCCM or App Layering or by simply downloading a template from Azure and making the necessary changes to the image, including installing the apps that must be delivered to all users.

The admin faces difficulties when there are a few or several groups of users that need different applications. This requirement results in the proliferation of the number of images that need to be managed with redundant apps within them. Updating the OS and the apps in each of these images would get cumbersome. App Layering can be used to make the process of updating this image and its contents much easier by splitting the image up into individual layers that can be stacked as needed.

![App Layering - Layered Image](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_3-layered-image.png)

App Layering also enables admins to make desktops available to different user groups (who require different apps) from the same set of layers. Consider the golden images of two pools in the following diagram.

![App Layering - 2 Pool Golden Images](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_4-Two-Pool-Master-Images.png)

Pool A for the Finance team requires Chrome and Accounting software while Pool B for the legal team requires Firefox and a PDF writer. Using a normal image management solution, the admin needs 2 different golden images and then must manage updates to the OS, the hypervisor driver, and Office 2016 in both the images, as and when an update occurs.

![App Layering - 2 Pool Layered Images](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_5-Two-Pool-Layered-Images.png)

With App Layering the same layers for the OS, Hypervisor driver and Office 2016 and the required apps can be chosen, and the images created. The 3 layers are reused for both pools. This method also has no performance impact on the virtual machines and the admin can update any layer that has a change once and push the change to both pools.

Additional benefit of using App Layering is that the same image with the platform driver being swapped for a different hypervisor or cloud allows the admin to target a totally new platform for the same image.

Elastic layers give an admin the ability to push an application to a particular user during login by mounting a folder onto the machine that contains the application and redirecting the writes to the profile.

![App Layering - Elastic App Layers](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_6-Elastic-App-Layers.png)

Expanding the scope of an Elastic layer to be able to hold all the writes (changes) the user is making during a session (which gets redirected to a share) is the functionality called user personalization layer. Learn more about this a little later when Profile Management is discussed.

Once ready with the image (prepared in any way the admin chooses to), the admin needs to be able to replicate it quickly and efficiently to make it available to the user.

#### Machine Creation Services

Citrix MCS clones the golden image (using a snapshot). Using that clone in conjunction with an identity disk and a writeable delta disk has many benefits.

Taking a snapshot of the golden image allows an admin to easily test future changes and rollback a change if it is found to have an issue. This provides great flexibility to admins for issue remediation with a quick turnaround.

![MCS - Stages](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_7-MCS-Stages.png)

The use of a delta disk gives an admin the flexibility to use the image for non-persistent desktops as the delta disk can be discarded easily to revert the virtual machine back to its original state after the end of the session. This also helps with resetting desktops to keep the disk from increasing to a very large size.

With Citrix Managed Desktops, Citrix has made the creation of a catalog (using MCS) very simple.

![CMD - Catalog Creation](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_8-CMD-create-catalog.png)

Admins have the option to choose the type of catalog, select the Azure subscription that the VMs are going to be created in, the network connectivity back to the corporate network, the region the VMs are to be hosted in and whether or not an Azure Hub is to be used. For the machine, admins get to choose the Storage type, the anticipated workload profile, and the number of machines. Then an admin selects the master image that is to be used to create the VMs and provide a name. Lastly the admin can set a power schedule, which will power on and power-off the VMs in the catalog using our Autoscale technology.

Going forward the day to day management of these machines is simplified with the CVAD service console, which not only gives admins a feature rich brokering console to manage the resources, but also apply granular policy controls.

#### Autoscale

Autoscale helps to reduce cost in the cloud by shutting down machines that are not needed during off peak times or when there is low load on the catalog. On the flip side Autoscale helps to power on the required machines before a shift or workday morning (logon storms) to handle the anticipated load so that user experience is not affected by long logon times when VMs boot as the users log in during these times. Admins can also define disconnection and logoff time outs to ensure idle machines can be shut down.

![Autoscale configuration dialog](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_9-CMD-Autscale-config-dialog.png)

Another advantage is the ability to identify primary (lower cost resources, such as on-prem desktops or reserved instances) and secondary (pay-as-you-go instances) resources. Resources in the primary zone, are booted first before trying to consume resources from the more expensive secondary zone. Secondary zone resources can be used for burst usage or during a business continuity or disaster recovery event. Hosts in the secondary zone would be turned off first as well when demand drops.

To learn more about Autoscale read the [Autoscale Tech Brief](/en-us/tech-zone/learn/tech-briefs/autoscale.html)

#### Azure On-Demand Provisioning

With Autoscale the machines can be shut down to reduce compute costs. But the machine still exists in Azure and therefore the organization would be charged for the all the static costs including storage costs. Another feature that is available in only the CVAD service is the Azure On-Demand Provisioning. This feature allows MCS to create the virtual machine only when a power on command is given and deletes the virtual machine when it is shut down. Resulting in no components of a machine being present when the machine is shut down. This includes the storage disks. Requires the use of Azure Managed disks.

#### MCS I/O optimization

Another line item in the organization's Azure bill are I/O operations (IOPS), if using standard disks. The Windows operating system assuming that a physical disk is locally available, performs a lot of small random write operations. In a virtualized environment, this means increased load on the drives that host the OS. This would lead customers to opt for the more expensive SSD options to host their VMs.

MCS I/O optimization – a RAM cache-based solution, is designed to offload random write operations to high-speed RAM. This reduces the number of writes to the disk and improves session responsiveness. As the RAM cache is consumed, MCS consolidates the writes and places large blocks of RAM cache onto disk. By creating larger, sequential blocks of data, MCS can provide better disk utilization allowing organizations to reduce costs by utilizing standard disks instead of high-performance disks.

![MCS I/O optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_10-MCS-IO-opt-diag.png)

Also, all reads are performed from the cache and only the data that is not in the cache is fetched from the disk. This also reduces the I/O operations that the organization is charged for in Azure.

Results from tests performed with the LoginVSI knowledge worker workload show, from the first an HDD with no cache, the user density comes out at 61, add MCSIO (4th and 5th bar), and with a small cache disk, user density increases to 76 or 77 users with the same workload.

[![MCS I/O optimization LoginVSI graph](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_11-MCS-IO-opt-LoginVSI-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_11-MCS-IO-opt-LoginVSI-graph.png)

Compare it to SSD disks, which are more expensive, and it is observed that the user density improved slightly from 74 to 77.

Consider a scenario where 1,000 users need desktops. These same 1,000 users, can be hosted on 12 D5_v2 instances instead of 13 without MCS I/O, with all of the VMs running on standard HDDs instead of Premium SSDs. This results in monthly cost savings (West US 2 pay-as-you-go monthly pricing as of writing this document) of approximately:

= Price of 1 VM running in Azure + Price of Premium SSD

= [Price of 1 D5_v2 VM + price of 1 Premium SSD managed disk

= USD [1366.56 + 9.29]

= ~USD 1375.85

Effectively saving of approximately USD 1375 per month for every 1,000 users in the environment.

In addition to the cost savings, the user’s application response times are also improved, which directly impacts user experience scores. Read more about the testing and results [here](/en-us/tech-zone/design/design-decisions/azure-instance-scalability.html)

### Personalization

In a non-persistent VDI environment, which includes WVD, users often require some level of personalization. Without persistent personalization, users are required to configure OS/app settings with every session.

Microsoft incorporates FSLogix containers into a WVD environment. This helps solve several personalization challenges. However, Citrix is able to extend the capabilities to include items like

*  User-Installed Applications
*  Multi-Session Support
*  Azure Files Support

#### User-Installed Applications

There are several scenarios where the admin would want to provide a desktop to power users and knowledge workers that want to be able to install apps on the desktops. These would not be persisted across session logouts with most solutions.

User personalization layer is a VHD based solution just like FSLogix Profile Containers but has the additional benefit of being able to persist user installed applications in the user personalization layer.

![App Layering - user personalization layer](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_12-App-Layering-User-Layers.png)

The user personalization layer VHD is mounted on the VMs at logon over the network. The changes a user makes are redirected to the individual user’s user personalization layer, which resides in the shared storage location. Note that it is not necessary for the entire OS image to be layered using App Layering to utilize this feature.

To learn more about the user personalization layer feature see the [Tech Insight video](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html)

#### Multi-session support for FSLogix containers

Another advanced use case, one which is more prevalent when users are remote, is when a user wants to have multiple, concurrent sessions.

With a VHD based profile solution, such as the FSLogix Profile Container or the user personalization layer alone, which requires the VHD to be mounted on the desktop that has the first session running on it, there is an inherent limitation. This approach allows only one session to write back changes. I.e. the first session that the user logs into has the VHD mounted in Read/Write mode, while the next session onwards the VHD can only be mounted as Read-Only.

So, when a user has a desktop session running and launches an app which writes to their profile, the changes made in the app will be discarded.

![Multi Session FSLogix](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_13-Multi-session-FSLogix.png)

For example, consider a physician working from home, she launches Outlook from her corporate Windows 10 device and a few minutes later launch’s the hospital’s EMR system on an iPad, to quickly check up on a few patients. That’s two simultaneous sessions, the first session that the physician logged into has the VHD mounted in Read/Write mode, while the EMR app session would have the VHD mounted as Read-Only. The changes made to the setting of the EMR app (default schemes, macros, favorites, and so on), will not be written to the profile when she logs off from it.

Citrix profile manager helps to synchronize files and settings that are changed in a Read-Only session. The centralized profile storage (user store) acts as the temporary storage for writes in the Read-Only sessions.

![Multi Session Citrix Profile Management](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_14-Multi-session-Citrix-Profile-Management.png)

This allows each session to write changed files back and those changes are merge using the last writer win strategy at file level. Changes to registry hive files like NTUSER.DAT, are merged at registry key level. More information about this can be found at this [link](/en-us/profile-management/current-release/configure/enable-multi-session-write-back-for-fslogix-profile-container.html)

#### Azure Files Integration

Citrix has built integrations to use Azure Files’ SMB based storage to host Citrix user personalization layer and Citrix Profile Management data. Azure Files support for on-premises Active Directory Domain Service Authentication (AD DS) enables these integrations. Azure Files shares can be mounted simultaneously in the cloud and on-premises, on Windows, Linux, or macOS. The benefit of having the personalization repositories in Azure, is that the data can be fetched very quickly from the shares, making session launch and application data access much faster.

Azure Files alleviates the need to set up, manage, and update the SMB store infrastructure (physical or virtual windows machines). Azure Files can be used to move the data that is hosted in your data center to the cloud and then be accessed by machines in your hybrid environment.

Learn more about how to set up the same [here](/en-us/tech-zone/build/deployment-guides/citrix-azure-files.html)

### HDX

For over three decades, Citrix has been in the business of remote delivery of desktops and applications. That experience led to various technologies designed to specifically come under the umbrella of HDX (**H**igh **D**efinition e**X**perience). These include the graphics remoting (the encoding, decoding, and rendering of the session window), the compression technologies for various data types, the transport protocol used to send the data and our broad support for the peripherals, including support for various keyboards, printing, scanners, mice, audio and video peripherals, security / authentication devices and many more.

Some of the great features that must be highlighted are:

*  [Data transport technology](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#HDX_Technologies) optimizations
*  Graphics optimizations
    *  [HDX 3D Pro](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#HDX_3D_Pro)
    *  [Multi Monitor layout](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Multi_Monitor_-_Virtual_Display_Layout) for single large screen
*  [Policies and Peripherals](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Policies_and_Peripherals) and peripheral connectivity

#### HDX Technologies

HDX is designed around three technical principles:

1.  Intelligent redirection
1.  Adaptive compression
1.  Data de-duplication

Applied in different combinations, they optimize the user experience, decrease bandwidth consumption, and increase user density per hosting server.

1.  **Intelligent redirection** - Intelligent redirection examines screen activity, application commands, endpoint device, and network and server capabilities to instantly determine how and where to render an application or desktop activity. Rendering can occur on either the endpoint device or the hosting server.

1.  **Adaptive compression** - Adaptive compression allows rich multimedia displays to be delivered even on constrained network connections. HDX first evaluates several variables, such as the type of input (text, video, voice, and multimedia), device, and display quality. It chooses the optimal compression codec and the best proportion of CPU and GPU usage. It then intelligently adapts based on each unique user and session characteristics basis.

1.  **Data De-Duplication** - De-duplication of network traffic reduces the aggregate data sent between client and server. It does so by taking advantage of repeated patterns in commonly accessed data such as bitmap graphics, documents, print jobs, and streamed media. Caching these patterns allows only the changes to be transmitted across the network, eliminating duplicate traffic. HDX also supports multicasting of multimedia streams, where a single transmission from the source is viewed by multiple subscribers at one location, rather than a one-to-one connection for each user.

#### HDX Adaptive Throughput

HDX adaptive throughput intelligently fine-tunes the peak throughput of the ICA session by adjusting output buffers. The number of output buffers is initially set at a high value. This high value allows data to be transmitted to the client more quickly and efficiently, especially in high latency networks. Providing better interactivity, faster file transfers, smoother video playback, higher framerate, and resolution results in an enhanced user experience.

Session interactivity is constantly measured to determine whether any data streams within the ICA session are adversely affecting interactivity. If that occurs, the throughput is decreased to reduce the impact of the large data stream on the session and allow interactivity to recover.

![HDX Adaptive Throughput](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_15-HDX-Adaptive-Throughput.gif)

Read more about optimizing HDX bandwidth over high latency connections [here](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/bandwidth-connections.html)

#### HDX 3D Pro

Hardware encoding and optimization for CAD based use cases. Utilization of GPU enabled hosts in WVD can make the experience of 3D based use cases a lot better. In conjunction with these technologies in HDX policies such as Optimized for 3D graphics workloads, Build to Lossless, Progressive display and so on make not just the image rendering but also the user interaction with the session as near native as possible, even in challenging network conditions.

**Build to Lossless** – when interacting with a model the quality may be reduced to increase interactivity but when the user stops interacting with the model a lossless image is rendered. The following side by side images showcases this feature.

![HDX - Build to Lossless](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_16-Build-to-Lossless.png)

#### Multi Monitor - Virtual Display Layout

The Virtual Display layout feature lets the user define a virtual monitor layout to split up a single 4K display into up to eight monitors. The user can create various monitor layouts and apps can be easily moved between monitors.

![Multi Monitor Layout](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_17-Mulit-Monitor-Display-Layout.png)

Windows display settings show the placement and scaling of each of the monitors. If the user wants to change the layout, they can define a new one by splitting the screen up by percentages of the client monitor resolution either horizontally or vertically. The user can set the DPI for each monitor individually.

In the current era, where new hires are being onboarded from their homes and it is taking organizations longer to ship systems to their employees. Having the ability to work on multiple screens, even though the user has just one physical one is a great productivity booster.

![Multi Monitor Layout config](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_18-Mulit-Monitor-Display-Layout-config.png)

To learn more see the video [here](https://www.youtube.com/watch?v=aVhhy4Ms4r0)

#### Policies and Peripherals

The rich set of policies available to fine-tune the delivery of the app or desktop are extremely useful as they can be applied based on a variety of criteria, such as group of users, a set of resources, a set of tagged objects and many more. The policies ensure the widest range of supported endpoints and devices while giving maximum control over the delivery of the session and what can and cannot be accessed by the user. Some examples are

1.  QoS for managing multiple connections on supported routers (see [Multi-stream connections policy settings](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/multistream-connections-policy-settings.html) for details)
1.  Managing the image quality on the endpoint like settings for Visual Quality, Target frame rate and display memory limit (video buffer size in kilo bytes).
1.  [Session reliability](en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-reliability-policy-settings.html) and [Auto client reconnect](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/auto-client-reconnect-policy-settings.html) settings that ensure smooth session reconnection when a network interruption may occur.
1.  Various inputs devices including but not limited to specialty keyboards, mice, recording equipment, identification devices, and so on.
1.  A plethora of clipboard settings, for example:
    1.  Restriction on text and types of files that can be copied to the clipboard.
    1.  No clipboard or one way redirection (client to server or vice versa)

### Resource Optimization

Most important to the user is a short logon time and session responsiveness. See how with Citrix technologies admins can reduce resource consumption by managing the applications running inside a WVD desktop while reducing logon times and making apps and desktops more responsive. This is a win-win as there are cost savings to be achieved as well.

WVD desktop while reducing logon times and making apps and desktops more responsive. This is a win-win as there are cost savings to be achieved as well.

Some of the technologies of note are:

*  [Audio and Video processing offload](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Audio_Video_Processing_optimizations)
    *  [Browser content redirection](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Browser_content_redirection)
    *  [Microsoft Teams Optimization](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Teams_Optimization)
*  [CPU and Memory consumption optimization](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#CPU_and_Memory_Optimization)
*  [Logon time optimization](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Logon_Time_Optimization)

#### Browser content redirection

With BCR the media encoding, and decoding is done on the client, which helps alleviate the load of rendering media heavy websites on the session host (running in Azure). Let’s look at how this feature makes WVD desktops more performant while increasing server scalability.

Browsing websites even casually, can use up a lot of CPU and Memory resources, as can be seen from this graph where 5 mins were spent browsing followed by a couple minutes of idle time.

![BCR RAM and CPU graph](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_19-BCR-RAM-CPU-Graph.png)

Enabling Browser content redirection, not only offloads the media rendering from the CPU and reduces Memory requirements for the browser on each cloud hosted machine, which leads to higher single server scalability but also helps reduce ingress and egress data costs as these large video files are not being sent up to your cloud.

The following were screenshots taken during our tests

[![BCR comparison screenshot](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_20-BCR-comparison-screenshot-large.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_20-BCR-comparison-screenshot.png)

To learn more about Browser Content Redirection, visit this [link](/en-us/citrix-virtual-apps-desktops/multimedia/browser-content-redirection.html)

#### Teams Optimization

Building on top of the Browser content redirection technology, Citrix in partnership with Microsoft (since the days of optimizing Skype for Business), gives admins a Microsoft Teams optimization solution. The ability to have the endpoint perform the encoding and decoding of the audio, video, and screen sharing bits of a call, increases the server scalability and call quality, by a significant margin.

Consider a scenario in a global company, where two users in different locations want to have a Teams call with each other and the desktops are hosted in a different location. The below map depicts such a situation with one user in the Netherlands and another in India.

![Teams User map without optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_21-Teams-user-map-without-optimization.png)

The WVD resource location is US East and there was significant latency between these locations with the Azure region in the middle (880ms round trip). The optimization reduces the latency by ensuring that in a 1:1 call, the users are connecting directly to each other rather than having to hair pin to the WVD server in the East US region hosting the call.

![Teams User map with optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_22-Teams-user-map-with-optimization.png)

When Teams optimization is enabled, the improvement in the image in the call is evident.

[![Teams image and CPU usage comparison with and without optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_23-Teams-optimization-comparison.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_23-Teams-optimization-comparison.png)

And CPU consumption on the machines hosting the Teams call is also drastically reduced.
Microsoft has released their own Teams optimizations solution, currently it only supports Windows 10 on the endpoints, Citrix's value add is that, not only are Windows OS based clients supported, but also Linux endpoints.

To learn more about Teams optimization, see the [Tech Insight video](/en-us/tech-zone/learn/tech-insights/microsoft-teams-optimization.html) or read the [Proof of Concept guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html)

#### CPU and Memory optimizations

Workspace Environment Management (WEM) uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops deployments.

**Resource management** - To provide the best experience for users, Workspace Environment Management monitors and analyzes user and application behavior in real time, then intelligently adjusts RAM, CPU, and I/O in the user workspace environment. The following graph shows the amount of memory being consumed by a set of sessions, with and without WEM.

**RAM optimization** - When a new process is launched, it takes up more RAM than it needs for its normal running. But generally, processes will not relinquish these resources once they are allocated to them.

WEM in real time detects which processes are in the focus of the user. A portion of the RAM working set of apps that are not in focus can then be reclaimed. It is observed that even if these apps come back into focus, they do not need the smaller subset of the amount of RAM that was reclaimed from them. This optimizes RAM consumption in the cloud and increases single server scalability.

![WEM RAM optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_24-WEM-RAM-optimization.png)
**Graph of RAM consumed by an app without and with WEM**

**CPU Optimization** - If a process is detected to be hogging CPU resources, this might negatively affect not only the session that it is running in, but also slow down other sessions running on the same machine and even impact logon times for other users.

CPU optimization with WEM, involves real-time monitoring of the process running on each VM. When a process is detected to be hogging CPU resources (for a defined amount of time), it automatically reduces the priority of the process, allowing other processes to use the CPU and alleviate the server load. When the process is seen to have returned to low CPU consumption overtime, then its priority is reset back to normal.

![WEM CPU Optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_25-WEM-CPU-Optimization.gif)

#### Logon Time Optimization

To deliver the best possible logon performance, Workspace Environment Management replaces commonly used Windows Group Policy Object objects, logon scripts, and preferences with an agent which is deployed on each virtual machine or server. The agent is multi-threaded and applies changes to user environments only when required, ensuring users always have access to their desktop as fast as possible.

![Log on process with and without optimization](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_26-Logon-process-without-optimization.png)

Find more information about WEM and its benefits [here](/en-us/workspace-environment-management/current-release.html)

### Security

Security provided by Microsoft is enhanced by adding more layers to it. Features such as Session Watermarking, Session Recording, clipboard redirection policies and many more services from Citrix enhance the security stance. Extensions to authentication mechanisms with peripheral connectivity and the inclusion of various 3rd party identity providers, helps leverage existing investments, when extending the resource footprint in the cloud.

Delegated administration and configuration logging don’t just enhance change tracking and audit capabilities but also help remain compliant.

The following are some features of note to bolster the security of your WVD deployment:

*  [Session Watermarking](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Session_Watermarking)
*  [Session Recording](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Session_Recording)
*  [Multifactor authentication extension](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#Multifactor_authentication)
*  [Secure Browsing](/en-us/tech-zone/learn/tech-briefs/windows-virtual-desktop-value-add.html#ecure_Browsing)

#### Session Watermarking

For sessions that have sensitive data being accessed by the user, a great deterrent to having the data be stolen is a watermark. Especially if the watermark can uniquely identify the user. Citrix enables admins to configure what to display. This includes use logon name, client IP address, VDA IP address, VDA host name, login timestamp, and even customized text. Being a server-side feature it is applicable to all sessions (not just on specific endpoints) and is immune to process termination at the end point by the user as a workaround.

![Session Watermarking](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_27-Session-Watermarking.png)

Learn more about session watermarking [here](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-watermark-policy-setting.html). Watch a short video to see the feature in action [here](https://youtu.be/XHEQlvpAZKs)

#### Session Recording

Citrix provides the ability to record all or part of a desktop or app session. The recording can be intelligently stopped when sensitive information is being displayed. The admin is also able to initiate recording of a session from the Manage tab in the CVAD service console to be able to help troubleshoot issues being experienced by users.

Session Recording provides flexible policies to trigger recordings of application and desktop sessions automatically. This enables regulatory compliance and an audit trail of what was done during a session. Playback can have events inserted in them to allow for easy seeking of the recording.

![Session Recording player](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_28-Session-Recording-player.png)

Security of the session recordings can be enhanced by encrypting them to ensure only authorized users can view them or digitally signing the recordings. The admin can even define the size of the recording file based on file size or duration. Citrix Analytics can automatically turn on Session Recording when a user’s risk score is above the configured threshold.

#### Multifactor authentication

Citrix extends the Azure multifactor authentication capabilities with support for the following:

1.  MFA for CVAD service Administrators (TOTP)
2.  Citrix Gateway Service Active Directory + Token (Citrix SSO, MS Authenticator, Google Authenticator)
3.  Citrix Gateway Service with AAD MFA
4.  Citrix Gateway Service with Okta
5.  Citrix Gateway Service with Google IdP (in preview)
6.  Citrix Gateway Service with SAML IdP (Ping, ONeLogin, Auth0) (in preview)
7.  On-Premises Citrix Gateway and existing MFA configurations
8.  Smart Card / Proximity Card support
    *  Supported (Imprivata, Gemalto, and so on)

### Networking solutions

#### Citrix Gateway and Citrix Access Control

As discussed in the preceding section the Citrix Gateway service and the on-premises Citrix Gateway are a great addition to the security of the WVD environment by adding additional capabilities to Azure MFA. It is also one of the best reverse proxy solutions in the market.

Citrix Gateway Service has a number of points of Presence (POP) globally, this improves performance by connecting through the nearest Citrix Gateway POP.
With CVAD service and Citrix Gateway service enterprises now may provide remote access to apps and desktops without those additional requirements along with other benefits:

*  Multiple sites are implemented and maintained globally by Citrix
*  Public IP addresses are implemented and maintained by Citrix
*  Predictive DNS provides better user experience
*  Certificates are implemented and maintained by Citrix
*  Elastic scalability and High Availability are provided and managed by Citrix
*  Enterprises pay as they grow and reduce operating expenses
*  Faster onboarding for new customers

Furthermore, Citrix Access Control combines the capabilities of instant secure access to SaaS and web applications through single sign-on (SSO), along with browser and cloud-based app controls, web-filtering policies and integrated user-behavior analytics to add an extra layer of security.

Endpoint Point Access scans and contextual access help to determine the level of security that a URL would need. Based on the result the system automatically determines whether the user can access the URL directly or the user must be redirected to a Secure Browser session where the URL is opened safely, or the URL needs to be blocked.

Citrix Analytics service using all these services as data sources provides comprehensive insights into user behavior. It uses machine learning algorithms to detect anomalous user behavior, troubleshoot user sessions and view operational metrics for users in an organization.

#### SD-WAN

Citrix SD-WAN optimizes all the network connections needed by machines running in WVD. Working in concert with the HDX technologies, Citrix SD-WAN provides quality-of-service and connection reliability for ICA and out-of-band Citrix Virtual Apps and Desktops or Citrix Managed Desktops traffic. It also helps bridge the WVD deployment in Azure with the on-premises data center and other office locations.

![SD-WAN Architecture](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_29-SD-WAN-Architecture.png)

Citrix SD-WAN supports the following network connections:

*  Multi-stream ICA connection between users and their virtual desktops
*  Internet access from the virtual desktop to websites, SaaS apps, and other cloud properties
*  Access from the virtual desktop back to on-premises resources such as Active Directory, file servers, and database servers
*  Real-time/interactive traffic carried over RTP from the media engine in the Workspace app to cloud-hosted Unified Communications services such as Microsoft Teams
*  Client-side fetching of videos from sites like YouTube and Vimeo

Citrix SD-WAN provides optimized connectivity including:

*  Deep HDX visibility - to prioritize delivery based on class of service
*  VoIP packet duplication for reliability
*  Security over the Internet
*  Simple, integrated admin workflow
*  Low latency and congestion avoidance

The breakout of internet traffic directly from the end point reduces the latency and ensure faster load times. Optimization of Unified Communications traffic including Microsoft Teams, make the conference audio and video the best-in-class. Citrix SD-WAN helps reduce the amount of data that needs to be sent to the WVD resources, thereby reducing the ingress and egress data to your Azure subscription.

Integrations with Citrix Managed Desktops can provision the SD-WAN instances in the Azure Tenant, by the using the Citrix SD-WAN Orchestrator service.

### Monitoring and Analytics

The setup and design of an environment is just the first part of getting VDI infrastructure for an organization. For that reason, it gets the most focus from cloud providers. But the Day 2 and onwards of running and maintaining a large environment in my opinion is as important. The cloud obfuscates the running of the machines, but the admin must still ensure that the user experience is great day in and day out.

Citrix gives full help desk visibility into user sessions, for both real-time debugging of specific user issues and performance, and broad visibility into the environment and trends.

[![Citrix Monitor console](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_30-Citrix-Monitor-console.png)](/en-us/tech-zone/learn/media/tech-briefs_windows-virtual-desktop-value-add_30-Citrix-Monitor-console-large.png)

As seen in the preceding screenshot, the admin has a view of what’s going on in the session, can shadow the session and can remediate from the same console.

The following is a list of some of the great features that Citrix brings to bear for the administrator and the help desk:

*  Full featured integrated monitoring, alerting, and help desk support console
*  Real time Session Details with drill-down for support actions
    *  Task Manager, process management, latency, user-perceived interaction latency, applied policy set, granular virtual channel status, client details, remoting protocol inspection, and so on.
*  Always on monitoring of  
    *  User Logons
    *  Connection Failures
    *  Server Resources
    *  Application Failures
*  Breakdown of Logon Times
*  Connection probing - synthetic app launch attempts to proactively test availability
*  Alerting
*  Trending
*  Predictive capabilities (for example capacity planning projection)
*  Configuration Logging
    *  Allows for full tracking of environment changes with date/time/admin action
*  Delegated Administration
    *  Granular controls over administrative rights from help desk staff to IT owner
*  Diagnostics
    *  Citrix Director utilized to diagnose and resolve user/application issues  
        1.  Shadow Users
        2.  Send messages to users
        3.  Disconnect/restore desktop and application sessions
        4.  Session Recording
*  Help desk integration
    *  ITSM Adapter for ServiceNow allows for API-level integration
*  Advanced analytics for performance and security
    *  Enterprise-wide view of environment with ability to drill down to specific locations or individual users
    *  Monitor session responsiveness and latency
    *  Multi-product user behavior scoring to help admins evaluate security risks

Read more about the monitoring features available in the CVAD service [here](/en-us/citrix-virtual-apps-desktops-service/monitor.html), and in the Citrix Managed Desktops monitor [here](/en-us/citrix-managed-desktops/monitor.html)

Learn about the analytics capabilities, visit the Analytics Tech Brief [here](/en-us/tech-zone/learn/tech-briefs/analytics.html), or visit the website [here](https://www.citrix.com/analytics/)

### Citrix Workspace

The Citrix Workspace extends the capabilities that WVD sessions provide by encompassing them with a holistic workspace. It is not just the place to provide single sign-on access to apps and desktops.
The Workspace helps the user by organizing their work, guiding them to the important tasks and automating the repetitive tasks to enable them to complete tasks without having to leave the workspace.

Citrix Workspace with Intelligence organizes work by funneling in notifications from various sources and then enables users to action on them by integrating actions and workflows into the Workspace by using microapps. Learn more about Workspace Microapps [here](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html)

Citrix Workspace adds universal search capabilities to all the data sources the workspace has been tied into including your on-premises SharePoint or SMB share, all the way to OneDrive in the cloud and everything in between. The Citrix Assistant is designed to learn and help users to information and allows them to interact with back-end applications to complete simple requests. It allows users to have natural language interaction and do queries for the data on the systems of records within Citrix Workspace. Learn about the Citrix Assistant [here](/en-us/tech-zone/learn/tech-briefs/virtual-assistant.html)

### Summary

The value-add Citrix provides to Microsoft Windows Virtual Desktop is multifaceted and can help expand the capabilities of your Microsoft WVD based VDI environment into a full-fledged Workspace.

Citrix adds value in almost all the steps of the VDI deployment lifecycle in WVD in reducing cost and delivering the best possible user experience. All the while helping with the day to day running of the environment with monitoring, hands on help desk capabilities and performance and security analytics.

The following is a list of the features that were discussed in the detail in the preceding sections, grouped into categories:

1.  Setup and VM consumption cost management – Citrix App Layering, MCS, MCS I/O optimization, Azure On-demand provisioning, and Autoscale. Hybrid platform management allows the admin to move the environment into the cloud as needed.
1.  User experience – HDX technologies allow for the most optimized and customizable delivery of remote sessions with support for the richest set of peripherals. SD-WAN traffic optimization for UCE, Web, and SaaS apps delivers the best experience possible.
1.  Performance, Single server scalability and compute consumption optimization – Workspace Environment Manager, Optimization for UCE solutions including Microsoft Teams, Browser Content Redirection, and SD-WAN based network optimization.
1.  Security – Session Watermarking, Session Recording, expanded multifactor authentication capabilities, Security Analytics, Citrix Gateway service, and Citrix Access Control service all add to layers of extra security to your environment.
1.  Management – Profile Management extension for multi session scenarios, full-fledged user centric help desk solution that can remediate issues in the same console.
1.  Networking solutions – Enhance security with SSO and MFA, reduces latency to the resources and increase the resiliency of the environment.
1.  Workspace – Enhances the user experience by integrating the WVD based resources into a Workspace that helps, organize, guide, and automate work for the user.

Call to action:
For a trial of Citrix Virtual Apps and Desktops service click [here](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/form/inquiry/)
For a trial of Citrix Managed Desktops service click [here](https://www.citrix.com/products/citrix-managed-desktops/form/inquiry/)
