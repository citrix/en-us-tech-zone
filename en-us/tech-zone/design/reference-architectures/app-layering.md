---
layout: doc
h3InToc: true
contributedBy: Nagaraj Manoli
specialThanksTo: Rob Zylowski, Dan Morgan
description: Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.
tz_title: App Layering
tz_products: citrix-virtual-apps-and-desktops;
---
# Reference Architecture: App Layering

## Audience

This document is intended for technical professionals, IT decision makers, partners, and system-integrators. This will allow the administrator to explore, and adopt Citrix App Layering to aid with managing the delivery of images and applications to their end users. The reader must have a basic understanding of Citrix products, image management services, and application virtualization concepts. For more information on image management, refer to the reference architecture on Citrix [Tech Zone](/en-us/tech-zone/design/reference-architectures/image-management.html).

## Objective of this document

This document provides a technical overview, architectural concepts for managing and delivering applications using Citrix App Layering technologies. This document includes both how App Layering works and integration with Citrix Virtual Apps and Desktops on different platforms.

## Citrix App Layering Overview

Citrix App Layering is a flexible solution to provide a complex set of Windows applications to a diverse set of users on any non-persistent supported platform.

Citrix App Layering performs image management in a unique way. The operating system and applications are split into different containers called “Layers.” Layers are created and updated independently, then compiled into “published images” to be distributed using any supported provisioning system.

Once the libraries of applications are created, different sets of images are deployed to many platforms using any combination of layers.

[![AL-Image-1](/en-us/tech-zone/design/media/reference-architectures_app-layering_001.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_001.png)

The primary goal of Citrix App Layering is to simplify Windows application management using a single interface. It allows the administrator to create and manage enterprise applications regardless of the underlying hypervisors or cloud infrastructure.

The OS and applications are separated into discrete manageable units. Even if many images are required to properly control application access, each operating system and its applications are managed as a single instance. In this approach, updates don’t have to be performed on every image in the environment. Simplifies the environment while reducing management time, complexity, and costs associated with operating systems and app management.

The OS and applications are stored as virtual disk that contains files and registry entries for a specific layer. There are two ways to include applications within virtual machines:

*  **Published image:** This method combines the OS layer, a platform layer, and a set of application layers to create an image for provisioning systems like Citrix Provisioning or Citrix Machine Creation Services. App Layering can publish images to multiple provisioning systems and multiple hypervisors from the same set of layers.

Learn more about Citrix image management, refer the [reference architecture](/en-us/tech-zone/design/reference-architectures/image-management.html) document.

*  **Elastic Layers:** Few application layers are attached to a virtual machine during the log-on based on AD group membership and application assignment. Allows greater flexibility in application assignment by allowing dynamic delivery of applications to standardized images.

Separating the applications and personalization from the operating system provides a flexible and manageable solution to non-persistent image management.

## Why Citrix App Layering

Citrix App Layering provides a cost-effective solution to manage a complex set of applications across multiple environments with different packaging requirements. Almost all applications that are embedded with kernel drivers, OS and system service dependencies are compatible with App Layering technology.

There are many advantages to adopting the Citrix App Layering approach for image management:

*  **Simplifies master image management for PVS and MCS:** Citrix App Layering is a single image management solution that supports both provisioning models used with Citrix and third-party hypervisors. Using App Layering both management and upgrades for images are simplified with no direct editing or reverse imaging of images.

*  **Azure support:** Citrix App Layering supports Microsoft Azure and makes it simple for App Layering customers to migrate to an Azure platform.

*  **Decouple the apps and provisioning systems from the image:** Citrix App Layering separates packaging from the image. In normal image management updates to an image are performed for each image separately. Using App Layering a layer can be part of many images. To update all the images the layer would be updated once, and the images regenerated. Upgrades to components like hypervisor tools, Virtual Delivery Agents (VDA), and Citrix Provisioning tools become easy.

*  **Supports complex use cases:** Complex applications with kernel drivers, systems services, third-party drivers, and console access can all be supported using Citrix App Layering. Due to, App Layering’s two modes for app layer deployment almost all applications are compatible.

*  **Non-persistence desktop User Layer:** The User Layer is a writable Elastic Layer. A user logs on to a desktop most write operation goes into the User Layer. It allows a user to install applications and save configuration settings that are outside the user profile. Also it stores user's data files by making their desktop seem to be persistent with the benefits of a shared desktop model.

*  **Reduces the number of required images:** Elastic Layering can significantly reduce the number of required images by dynamically delivering applications to only assigned users at logon. Elastic Layers are compatible with both Citrix Virtual Apps and Citrix Virtual Desktops.

## Citrix App Layering Use Cases

App Layering is an important addition to the Citrix technology portfolio that provides many benefits listed above. Though it provides benefits, App Layering does not mean to use it for all the use cases. In this section, several of the most relevant use cases are outlined to show the benefits of App Layering technology.

### 1-Too Many Images to Manage

Many Citrix customers must support a significant number of applications and a complex set of user requirements. When using an image-based provisioning technology, meeting these requirements often requires a high number of images to manage. These images have to support different user groups or different sets of conflicting applications. Often there is some overlap in the applications deployed to each image as well.

Citrix App Layering simplifies the management of this complex scenario.

[![AL-Image-2](/en-us/tech-zone/design/media/reference-architectures_app-layering_002.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_002.png)

App Layering allows the administrators to manage the operating system and applications as individual entities using layers. For example, Windows needs to be patched, the patch is made to the OS layer once and all the images that use OS layer updated by the App Layering appliance. If Microsoft Office is used in 10 images, upgrading this Office is simpler by adding a version to the Office layer. Eventually all those 10 images are updated automatically.

When Elastic Layering is added, it is also possible to significantly decrease the number of required images. Elastic Layer allows the admins to dynamically deliver applications during log-on. For applications that are not used by everyone, Elastic Layers allow the image to be created more generically while providing customization for each user.

### 2-Support for Multiple Hypervisors and Azure

Many organizations are moving to a hybrid multi-cloud environment mixing on-premises and cloud resources to enhance the user experience. Citrix App Layering provides image portability by supporting most hypervisors plus Microsoft Azure simply by creating a different Platform Layer for each desired environment.

[![AL-Image-3](/en-us/tech-zone/design/media/reference-architectures_app-layering_003.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_003.png)

Normally a mixed hypervisor and hybrid cloud configuration would force the administrators to maintain multiple different sets of images and applications in multiple different management platforms. With App Layering technology, the same OS and applications used on-premises can be pushed to the cloud or to another hypervisor with little to no extra work.

To understand, on App Layering cross-platform functionality, refer to the **Citrix App Layering Cross-Platform Support** section below.

### 3- Hypervisor Portability for Migration

Organizations often get “stuck” with a vendor’s technology simply because it would be prohibitively expensive to migrate to new technology. Also, companies often merge with other organizations that have made different technology choices. One of the distinct advantages of App Layering is the ability to move from one platform to another simply by creating a different Platform Layer and migrating the App Layering appliance using import export to the new platform.

[![AL-Image-4](/en-us/tech-zone/design/media/reference-architectures_app-layering_004.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_004.png)

Allows for a seamless migration, for example, from vSphere to Citrix Hypervisor or from an on-premises hypervisor to Azure.

### 4-Persistence for shared VDI Desktops

Many organizations have several users that require a high level of persistence on desktops. Including power users in any group, developers, engineers, architects, and so on. App Layering User Layers provide a significant amount of persistence on top of a pooled desktop architecture.

The User Layer is mounted on logon and any subsequent writes on the desktop are written to the User Layer. Most applications can be installed in the User Layer. The rules for what works in the User Layer are the same as for Elastic Layering. As long as the application does not install kernel drivers, third-party drivers, and the services that are dependencies to other services during boot, they are the most likely work in the user Layer.

[![AL-Image-5](/en-us/tech-zone/design/media/reference-architectures_app-layering_005.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_005.png)

For use cases that require persistence, the User Layer is the best choice. For use cases to support just Microsoft Outlook OST files and index files, the User Layer may not be the best choice. The User Layer is intended to handle all writes to the VDI desktop after the user logs on. Other technologies like the Citrix Profile Management Outlook Container or FSLogix Profile or Office 365 Container handle the Outlook OST and indexes in a much more targeted manner so that the amount of I/O handled by the container is much smaller. All of these solutions now handle managing the Outlook OST, Outlook streaming files and Outlook index files so that supporting indexing is no longer a reason to choose one technology over another.

## Technical Overview of Citrix App Layering

### Types of Layers

A Layer is a virtual disk containing the files and registry entries that are changed or added during packaging. Excluding the first version of the Operating System layer, layers are created by the App Layering appliance integrated with the hypervisor. An administrator creates a layer, the appliance dynamically provisions a packaging machine with a boot disk and a layer disk. When the packaging machine boots, all changes on the packaging machine are written into the layer disk. When packaging is complete, this disk is copied back to the appliance and all the files and registry changes are written into the new layer and stored in the layer repository in VHD format.

App Layering uses these different types of layers:

*  Operating System Layers
*  Platform Layers
*  Application Layers
*  Full User Layers

[![AL-Image-7](/en-us/tech-zone/design/media/reference-architectures_app-layering_007.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_007.png)

**OS Layer:** The operating system like Windows 10 or Windows Server 2019. Built from a “Gold Image” VM in the hypervisor.

**Platform Layer:** The platform layer includes the software required to support a particular platform. Including the broker agent, provisioning system, and hypervisor tools if the hypervisor is different from the default hypervisor. The platform layer is also the highest priority layer and sometimes software is installed here so that it is compiled at the highest priority.

**App Layers:** The main layer type. Used for most application software.

**User layers:** An elastic (see next section) writable layer. User Layers are mounted at logon and once mounted almost all the desktop writes go to the user layer. This layer gives users the ability to significantly customize their VDI experience even though they are using a shared desktop model.

### Citrix App Layering appliance

The Citrix App Layering appliance (appliance) provides both the administrative interface for App Layering and the engine for all App Layering processes.

The App Layering appliance is deployed as a virtual machine into the data center where application packaging and image publish take place. The App Layering appliance is built on CentOS, configured with 4 vCPUs and 8 GB of RAM. These settings are not to be changed as the appliance is designed to work in that configuration.

The appliance is built with two disks. The first disk is a 30 GB boot disk for the operating system. The second disk is the 300 GB layer repository. This disk can be extended or expanded as necessary if more space is required.

During the process of layer creation and image publish, the Citrix App Layering appliance saves virtual disk files in VHD format to its layer repository within the appliance. The appliance interfaces with an SMB share to support the appliance upgrade process and for storing Elastic Layers.

The appliance is used only to manage layers, images, and Elastic Layer assignments. Virtual Desktops and Virtual App servers do not interface directly with the appliance. When a layer is assigned elastically, the appliance copies the layer to the Elastic Layer share. The share holds these VHD files plus a set of JSON files that provide the layer assignments to users.

The Citrix App Layering appliance also hosts the App Layering management console. Deploying the App Layering appliance is the first step in the installation process. After installing the appliance, the management console is accessed to complete the installation steps.

Learn more about installing the Citrix App Layering appliance, refer to the [product documentation](/en-us/citrix-app-layering/4/install-appliance.html).

[![AL-Image-6](/en-us/tech-zone/design/media/reference-architectures_app-layering_006.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_006.png)

The Citrix App Layering management console is a web-based application hosted on the App Layering appliance. The App Layering management console provides the interface to:

*  Create and manage Operating System, Platform, and Application layers
*  Create published image templates
*  Publish and manage Layered Images
*  Assign App Layering the administrator roles to users
*  Manage the appliance and system settings such as task and log retention, security settings, and network file shares

### Compositing Engines

New to version 1910 and later is a feature called Compositing Engines. Compositing Engines offload most of the packaging and publishing tasks that can also be performed by the App Layering Appliance. By offloading these tasks the packaging and publishing processes scale much better and due to the advantages of the technologies used, performance of the process is also significantly enhanced.

A Compositing Engine is built by a Hypervisor connector as a Windows PE virtual machine that carries out a set of publishing tasks, then reboots itself into a packaging machine or published image. The Compositing Engine is used to create cached layer disks, create packaging machines and publish images. At the time of writing of this reference architecture, there are Compositing Engines for HyperV and vSphere introduced in versions 1910 and 1911, respectively.

Compositing Engines have the following characteristics:

*  Lightweight, ephemeral appliance running Windows PE
*  Self compositing
*  Controlled via REST API
*  Hypervisor disk formats supported (VHD, VHDX and VMDK)
*  UEFI support (Gen2)
*  Secure boot on published images (only if no Elastic Layering or User Layers are used)
*  iSCSI is used to attach disks hosted on the App Layering Appliance
*  No Limit on the Number of simultaneous publishing jobs (there is a practical limit)

#### Advantages of Compositing Engines

Use of Compositing Engines is a choice. In the HyperV Connector and the vSphere Connector there is a checkbox to enable "Offload Compositing". This setting is also available in the Machine Creation for vSphere and the VMware Horizon View connectors. The significant advantage to the Compositing Engine is that it is running on a Windows device with direct access to hypervisor disks. This provides the mechanisms to support GEN2 machines in HyperV, native ESX VMDK formats with Thin Provisioning in vSphere and UEFI in both. Packaging and Publishing performance is enhanced because the large layer files are processed less and written directly into disks on the hypervisor by the Compositing Engine which attaches back to the App Layering Appliance to access the layers using iSCSI connections.

Note: PVS publishing cannot yet be performed using a Compositing Engine, therefore it is still not possible to directly publish a VHDX file or GEN2 virtual machine to PVS.

### Layer Delivery

Layers can be delivered in two ways. As part of a published image where the App Layering appliance combines all the layers into a single disk file and uploads that file to the provisioning system or where layers are mounted on logon and made available to users dynamically.

**Published Image:** Here an OS, Platform, and set of Application layers are combined into a single disk file that is published to a provisioning system like Citrix Provisioning or Machine Creation Services. The process starts by cloning the OS layer, then writing in the files from Application layers one at a time in priority order where the newer the layer the higher its priority and lastly writing in the platform layer. The merge process for creating published images is advanced. When an image is created the file systems, registry, and environment variables are merged the Windows Driver store is recreated merging all third-party drivers from different layers.

**Elastic layer:** These layers are App Layers that are assigned to users by Active Directory group membership and attached during or slightly after logon. Elastic layers are dynamic, allowing the administrators to customize the user experience while minimizing the number of images.

### Connectors and Connector Configurations

The App Layering appliance integrates with hypervisors and provisioning systems using connectors.

There are two types of connectors - hypervisor and provisioning:

*  **Hypervisor connectors** provide the mechanism to interface with a hypervisor. Currently, there are hypervisor connectors for Citrix Hypervisor, vSphere, Hyper-V, Nutanix AHV, Azure, and Azure Gov.

*  **Provisioning System connectors** allow the admin to publish an image to the provisioning system. Currently, there is Provisioning System Connector’s for Citrix Provisioning, Citrix Machine Creation Services on each hypervisor and Horizon View.

A single appliance can connect to any number of hypervisors or provisioning systems by having more connectors defined. Connector configurations define the settings required to integrate with the hypervisor or provisioning system. A configuration typically includes credentials for authentication, a storage location, a VM template, and any other information required to interface with the environment where the administrators are creating layers or publish images. The administrator can create multiple connector configurations, each configured to access a unique location in the environment. Connectors are used for:

*  Importing a Gold Image when creating an OS layer
*  Creating layers and layer versions
*  Publish layered images to a hypervisor or Provisioning Service. A connector configuration can also include a PowerShell script to run on a Provisioning Server or a system with a host agent to perform post-processing of a published image

Reference: [Connector Configurations](/en-us/citrix-app-layering/4/connect.html)

### Connector Types

This section defines the connector types available in Citrix App Layering.

### Hypervisor Connectors

Hypervisor connectors are used when creating layers or importing a Gold Image to create the OS layer. Hypervisor connectors when packaging dynamically creates a packaging machine on the storage and host defined by the connector configuration. A hypervisor connection can also be used to create a virtual machine in the hypervisor.

### Citrix Provisioning

Citrix Provisioning connector integrates with an App Layering Agent on a Citrix Provisioning server to publish an image directly to the Provisioning server as a virtual disk. The prerequisites for Citrix Provisioning are:

1.  The connector service account must be a domain account with the administrator permission within PVS.
1.  The connector service account must also be a local administrator on the Citrix Provisioning server.
1.  A Citrix App Layering Agent must be installed on each Citrix Provisioning server defined in a connector. Only one agent can be defined per connector.

Reference: [Citrix Provisioning](/en-us/citrix-app-layering/4/connect/citrix-provisioning.html)

### Machine Creation Services

The Citrix App Layering appliance can directly provision and publish layered images to hypervisors as virtual machines that is used as the Master Image for MCS. Connectors allow layered images to publish directly to the hypervisor. The MCS connector is almost identical to the Hypervisor connectors except that the MCS connector starts the published virtual machine which allows any scripts defined in layers to run on the Master Image before it is deployed by MCS. The Master Image shutdown and a VM snapshot taken as part of this process.

### Connector Scripts

This step is an optional and advanced feature available while creating a connector configuration. To use this feature, configure an optional PowerShell script to run on the defined Agent server on the connector configuration. This step is most often used with Citrix Provisioning, but it can be used with any other publishing connector by installing the App Layering Agent on a Windows system to run scripts against a published image. Copy the scripts to the machine the agent is installed on. This script runs after a successful deployment of a layered image. To use this functionality, with a Master Image on a hypervisor the script has to also interface with the management platform of the hypervisor. For example, for vSphere, the script would need to use PowerCLI against vCenter to run code against the Master Image virtual machine.

Some preset variables are available to enable scripts to be reusable with different template images and different connector configurations. The variables also contain information that identifies the virtual machine as part of the published image.

Learn more about the sample scripts, refer the following support articles [CTX226060](https://support.citrix.com/article/CTX226060) and [CTX226062](https://support.citrix.com/article/CTX226062).

### Published Image Templates

When using App Layering, OS, Platform, and Application layers are merged by the App Layering appliance to create published Images. Images are published and then created in a format required by the target provisioning system. For example, in case the administrators are publishing to Citrix Provisioning, the appliance creates a VHD and upload it to the defined provisioning server as a virtual disk. In case solution uses Machine Creation Services on Citrix Hypervisor, the appliance creates a VHD, upload it to Citrix Hypervisor and create a Master Image VM using the VHD.

To define the configuration of a published Image, an Image Template is used. The image template defines which OS, Platform, and Application layers are included in the image. It also defines the connector used to publish the image, how large the resulting image is in GB, whether the image has Elastic Layering enabled or one of the different types of User Layers. The image templates are a point-in-time snapshot of the image configuration, they do not support versioning. However, Image Templates can be cloned and modified if slightly different versions of the same image are required.

### App Layering Agent

The Citrix App Layering agent provides communications between the App Layering appliance and either a Citrix Provisioning server, a Hyper-V server, or just a Windows machine used as a scripting server. The App Layering Agent can also be used to automate scripting following publishing images to other hypervisors like Citrix Hypervisor or vSphere.
For Citrix Provisioning and Hyper-V, the App Layering Connector contacts the App Layering Agent and asks it to transfer the VHD the appliance has compiled to the agent’s server. This transfer is initiated from the agent using BITS and the file is download from the appliance.

Citrix App layering agent details can be found in the Citrix documentation.

Reference: [Install Agent](/en-us/citrix-app-layering/4/install-agent.html)

### Active Directory

The App Layering appliance connects to Active Directory for both authentications to the appliance and assignment of Elastic Layers. When an administrator logs into the appliance it attempts to logon to Active Directory with the same credentials. If that logon works, the user is allowed into the appliance.

Access to directory services is required for the following purposes:

*  Role-based access control (RBAC)
*  Assignment of Elastic and User Layer

During the initial binding with the directory service, the App Layering appliance is compatible with the SSL 3.0 Secure Socket Layer and TLS 1.1 and 1.2 transport layer security.

To create a directory junction, refer to the [product documentation](/en-us/citrix-app-layering/4/manage/users/directory-service.html).

## Layers

The following section describes the uses for each type of layer in more detail.

### OS Layer

An OS layer is one that contains the Windows operating system. It is a leading practice to include any components that would be updated with Windows Update in the OS layer so that all are updated by Windows Update. All operating system roles and components such as .NET and Visual C++ runtime libraries are included as part of the operating system image for this reason.

It is preferable not to install end-user applications into the OS layer because all application layers made with a particular OS layer are tied to that OS layer. If an application is installed in the OS layer, then every image using that layer to have that application included this process leads to a problem when the strategy is to make the OS layer universal. Separation of applications from the operating system is the key to limiting the number of OS images to manage. Even applications with drivers, services, and kernel devices are supported in application layers and do not need to be included in the OS layer.

During, the creation of the OS layer there are a few points to remember:

*  Check the supported operating systems from the link
*  The OS layer is not connected to the domain. The domain join is part of the platform layer
*  The hypervisor tools for the primary hypervisor are installed in the OS layer. The hypervisor where most packaging is performed
*  The hypervisor tools for alternate hypervisors are installed into the Platform Layer for that hypervisor
*  Install .NET components (from Microsoft) and updates in the OS layer
*  If Microsoft Runtimes are required, they are installed in the OS layer
*  Windows roles and features such as the RDS roles installed in the OS layer so that it can be updated with Windows Update
*  In case local user or group need to be added to the virtual machines, this task can only be done in the OS layer because the Windows Security Account Manager (SAM) captured from the OS layer

Reference: [Create an OS layer](/en-us/citrix-app-layering/4/layer/create-os-layer.html)

### Platform Layer

A Platform Layer is a layer that contains configuration settings, tools, and other software necessary to run a published image on a particular platform. Two platform layer types can be created:

**Publishing Platform Layer:** This type of Platform Layer is used to include the software required by a target provisioning system, broker, and hypervisor. For example, a publishing Platform Layer to support Provisioning Services on vSphere for Citrix Virtual Apps have the Citrix VDA, and PVS Device Drivers installed. If vSphere is not the same hypervisor where packaging is performed, then this layer also has VMware Tools installed.

**Packaging Platform Layer:** Packaging Platform Layers are used if a packaging layer is required to support packaging machines. These layers are not often required, but there are several instances where one has to be necessary including:

1.  If a layer has to be packaged on a different hypervisor than the standard. For example, if most layers are created on Hyper-V, but for some reason, a particular layer created within vSphere, a packaging platform layer with VMware Tools are used to support the packaging machine on vSphere.

1.  If access to the packaging machine is required using VDA software. This layer is most often required when installing drivers for USB peripherals that must see the device to install properly. By creating a packaging platform layer with the VDA software installed and adding the packaging machine to a manually provisioned catalog in Studio, this type of access can be supported.

### Creating a Platform Layer

For the creation of the Platform Layer a few points to be remembered:

*  For Citrix Provisioning, the target device software is installed without running the Imaging Wizard and then rebooted
*  Installation of Citrix VDA and Citrix Provisioning device drivers goes in the Platform Layer
*  Domain join is performed in the Platform Layer, that means multiple Platform Layers can be created to support different domains
*  The platform layer is also the highest priority layer, so some optional components can be installed including SSO software like Imprivata, hypervisor tools for alternate hypervisors, and the Citrix Workspace Environment Management Agent.

Learn more about the Platform Layer creation, refer to the article [CTX225997](https://support.citrix.com/article/CTX225997).

### App Layer

App Layers are used to package most applications. App Layers contain file system and registry objects for an application or group of applications. When creating or editing a layer, a packaging machine is dynamically created and all filesystem and registry changes are captured on that machine. The Packaging Machine contains the OS Layer and any included prerequisite App Layers. A second virtual disk called the Package Disk is attached to the packaging machine as the writable volume. That disk captures all the filesystem changes during packaging, and it also contains a registry hive (called an RSD hive) used to capture all the registry changes. When the packaging machine is finalized, only the Package Disk is downloaded. The Boot Disk is deleted.

Learn more about creating and editing App Layers refer to the [product documentation](/en-us/citrix-app-layering/4/layer/create-app-layer.html).

Most of the time applications are installed with the same configuration the administrator would normally use for the provisioning system that is supporting. While creating App Layers, it is always important to work out to disable automatic updates. Because the administrator usually does not want applications to automatically update after deployment. Citrix has documented the layering process for a few common applications. Theses App Layering “Recipes” can be found at the following link.

Reference: [App Layering Recipes](/en-us/citrix-app-layering/4/layer/app-layering-recipes.html)

### Elastic Layering

Elastic Layering is a method for dynamically deploying the application to a virtual session during user logon by mounting layers stored as VHD files on an SMB share and integrating them into the file system using the Citrix Composite File System (CFS). Elastic Layering is managed on the VDA by the Citrix Layering Service. The Elastic Layer Repository is an SMB share that contains both the layer VHD files and JSON configuration files which are used to define layer assignments for the Layering Service. The Layering Service reads the configuration files and then mounts layer VHD files using a Windows **SDK call**.

All applications are not supported by Elastic Layering technology because Elastic Layers are mounted during logon. The following application requirements exclude applications from being used within Elastic Layers:

1.  Applications with Kernel drivers
2.  Applications with services that are dependencies for other boot time services
3.  Applications that modify the Windows Driver store like third-party printer drivers

Applications with these requirements can be layered but the layers must be included in the published image.

### Configuration files (JSON)

As discussed above, Elastic Layer assignments are defined in a set of .JSON files store on the Elastic Layer Repository. These files are defined as follows:

*  **ElasticLayerAssignment.json:** This file contains information about the user and group mapping to application layers. This file contains an entry for each group or user ID that has assigned applications and under the SID for that AD object the layer assignments are listed.

*  **Layers.json:** This file defines the layers in the repository and metadata about the layer. This file is used to obtain the path used to mount the VHD.

*  **MachineAssociations.json:** As the name suggests it defines machine association with any AD group, the format uses machine name patterns containing wildcards to associate a set of computers with a defined AD group. When a user logs on to a machine that matches the pattern, they receive the Elastic Layers assigned to that group. These settings are defined in the App Layering Management Console in the **Users>Groups section**.

### User Layer

User Layers provide a more persistent experience for users while still supporting a shared desktop computing model. After, a User Layer is mounted most system writes are redirected to the User Layer. This layer allows support for the following:

*  Each user’s profile and data settings are stored in the User Layer
*  User installed applications are supported in the User Layer as long as the applications conform to the rules allowed by Elastic Layers

User Layers are assigned one to one. One user can have only one user writable layer per OS layer per domain. The user can therefore only log on to one delivery group or pool with a desktop using the same OS layer and Platform Layer combination with the User Layer enabled. This layer is created as a virtual disk on a file share when the user logs on for the first time.

As of version 1910 the Full user layer supports search index persistence between sessions. To support this, the Windows Search service is set to DISABLED (4) on VDAs when they boot. When the user logs on the Ulayer Service changes the start type of the Windows Search service to START_ON_DEMAND (3). Prior to enabling WSearch, the ULayer service must also ensure the indexer's "crawl-scope" registry settings are correct. The crawl-scope is a set of registry keys that determine the areas of the user's data to be indexed. Input into the crawl-scope comes from defaults built into the base image, but also can come from the elastic layers and the user's persistence layer settings too. These inputs are processed at logon time to provide the complete set of crawl-scope locations, and there is a modest but measurable overhead to building this for each logon. To avoid this overhead, the ULayer service generates a hash string to represent the base image deployment (e.g., the BIC instance) and the elastic layer assignments, and stores this string in the user's \Program Files\Unidesk\Etc\UserLayer.json file as "IndexerHash". On subsequent logons this string is compared to the recalculated IndexerHash and only when they differ is the crawl-scope rebuilt.

The following types of User Layers can be used:

*  **Full:** all of a user’s data, settings, and locally installed apps are stored in their user layer
*  **Office 365:** only the user’s Outlook data and settings are stored on their user layer. Search indexes are recreated every logon.
*  **Session Office 365:** Only the user’s Outlook data and settings are stored on their user layer. Search indexes are recreated every logon.

Note: In case of using the Office 365 layer it is recommended to use Citrix Profile Management to persist outlook settings.

The default max size of a User Layer is 10 GB. This size can be altered by defining a quota for the User Layer share. It is also possible to override the default User Layer max size using a registry entry on the VDAs. To change default max size, add the following registry override,

`HKLM\Software\Unidesk\ULayer\`

`DWORD: DefaultUserLayerSizeInGb`

`VALUE: <Size in GB>`

### Elastic Layer Share and User Layer Shares

Elastic Layers are VHD files mounted by client or server operating system over the VM network. Elastic Layers are mounted as read-only, and many machines can mount the exact same VHD file. The file server or share used for Elastic Layers are optimized for the read I/O.

User Layers are writable Elastic Layers. The User Layer is mounted read/write by only a single desktop. The file server or share used for User Layers are optimized for write I/O. When using User Layers performance of the storage is critical to the user experience. Flash storage arrays are highly recommended for the User Layer share.

The architecture for Elastic Layers is largely scalable. The Elastic Layer share repository path is defined in the VM HKLM registry for both VDI and RDS workloads. This design makes it possible to have an unlimited number of replicas to spread the load.

[![AL-Image-8](/en-us/tech-zone/design/media/reference-architectures_app-layering_008.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_008.png)

The Elastic Layer Repository and User Layer Shares are defined in the appliance. The Elastic Layer share is defined in the **System>Settings** and Configuration section. This path can be changed on VDAs using Group Policy by modifying the following registry values:

`HKLM\SOFTWARE\Unidesk\ULayer\RepositoryPath`

`Value = \\Server\Share`

For large VDI implementations, this registry value allows multiple Elastic Layer repositories to be used for different sets of desktops. Learn more about changing the Elastic Layer repository in the registry without reimaging refer to the Citrix article [CTX222107](https://support.citrix.com/article/CTX222107).

The location used for User Layers is assigned by Active Directory group and therefore it is also highly scalable because as many shares as desired can be used. These assignments are defined under **System>Storage Locations**.

For more information on the architecture of the Elastic Layers share to see the **Availability, Backup, and Recovery section**.

### User Layer File Share

During the login process when an image is configured for User Layers, the User Layer VHD file are created at the first time the user logs on after being assigned to the image. User Layer share settings are defined in the appliance by Active Directory Group. If a user is in multiple groups with assigned User Layer shares, there is a priority order to the share and their User Layer file created on the highest priority share. User Layer disks can only be used on one machine at a time.

User Layers are tied to both the domain and OS Layer for the desktop being logged into. The path to a particular User Layer as follows:

`\\Server\Share\Users\Domain_Username\OSLayerID_OSLayerName`

User Layers are write-intensive. It is recommended to use a file share that is optimized for writes. If the User Layer share is different from the Elastic Layer share, then user assignment defined by AD user groups.

[![AL-Image-9](/en-us/tech-zone/design/media/reference-architectures_app-layering_009.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_009.png)

User layer assignment is defined in the **System>Storage Locations** section within the App Layering Console. Enter the share and the group associated with the share.

For more information on the architecture of the Elastic Layers share, see the **Availability, Backup, and Recovery section**.

## App Layering Integration

The following section outlines the integration of Citrix App Layering with Provisioning Systems and Hypervisors.

## Citrix App Layering and Citrix Provisioning Services

App Layering is easy to integrate with Citrix Provisioning (PVS). Integration is facilitated by installing the App Layering Agent on one or more PVS servers. The Agent provides the connectivity between the appliance and Provisioning Services.

[![AL-Image-10](/en-us/tech-zone/design/media/reference-architectures_app-layering_010.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_010.png)

The preceding diagram depicts publishing layered images using Citrix Provisioning. Images are centrally managed and distributed for deployment by publishing from the App Layering appliance to a Citrix Provisioning server. There are several architectures that can be created to support Citrix Provisioning. The most often used architecture is to integrate a single production PVS server that is used as the integration point. Virtual disks are then downloaded to the PVS server store by the App Layering Agent when the image is published. The Agent uses the Windows BITS service to perform the transfer.

When images are published, it is possible to configure Connector PowerShell scripts to run against the image after the image is uploaded. When using this feature, if the scripts are the processor intensive, it is advisable to use a “pre-staging” Provisioning Server that is not used to perform the streaming. That way the publishing and script processing does not adversely affect streaming functions in the Provisioning Services Farm. To support this design, it is easiest way to create a “DEV” Citrix Provisioning farm with a single server. This “DEV” server can also be used to stream test images. Once, the image has been tested the virtual disk can be replicated to the Production Citrix Provisioning farm for further testing and deployment.

Learn more about this type of scripting refer to the following sample script from Citrix support article [CTX226060](https://support.citrix.com/article/CTX226060).

When an image is published to Citrix Provisioning, the image is named according to the Image Template Name with a date and time stamp for versioning. Virtual disks are managed in Provisioning Services the same way they would be managed without App Layering. Versioning is not used when App Layering is used. Instead, each time a new image is published a full virtual disk is created with a new date and time stamp. If an emergency change to the image is needed, Citrix Provisioning versioning can be used to quickly modify the image. Then the same change can be added to the appropriate layer after the issue has been quickly fixed.

### Elastic Layering’s Impact on the Citrix Provisioning Cache Disks

Citrix Provisioning uses reserved memory and a locally attached cache disk to temporarily store local filesystem changes during a session. While using App Layering and Citrix Provisioning, the cache disks size must be larger than without App Layering. Whenever a file is opened, for write operation using App Layering the entire file is copied into the writable volume of the virtual machine so that it can be edited. Causes the entire file to also be copied into the disk cache when Citrix Provisioning would normally only copy the modified blocks into the cache.

[![AL-Image-11](/en-us/tech-zone/design/media/reference-architectures_app-layering_011.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_011.png)

The above diagram depicts writes to the cache disk with and without the App Layering filter driver installed in the Provisioning Server targets. To know more about caching with Elastic Layers, refer to the article [CTX227454](https://support.citrix.com/article/CTX227454).

### User Layer and Citrix Provisioning Cache Disk

When User Layers are being used, only the Provisioning cache disk is used before a user logs on.

[![AL-Image-12](/en-us/tech-zone/design/media/reference-architectures_app-layering_012.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_012.png)

In this case, the local writable partition disk is only used during boot up. Once a user has logged in, all new file modifications are redirected to the User Layer disk on the file share and not to the streamed virtual disk, which means that the Provisioning cache no longer sees I/O on the writable partition.

## Citrix App Layering and Machine Creation Services

To publish Layered Images to Machine Creation Services a Machine Creation Services Connector created for the hypervisor being published to. The Connector configuration includes the service account credentials used to access the hypervisor, in addition to hosts, storage locations, templates, and so forth.

The connector is then used to publish a layered image as a virtual machine “Master Image” to the hypervisor.

[![AL-Image-13](/en-us/tech-zone/design/media/reference-architectures_app-layering_013.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_013.png)

The MCS connector starts the Master Image after it's published and run any layer scripts that have been defined in any layers. After all the scripts are run, the Master Image has to be shut down and the hypervisor will take a snapshot of the virtual machine. Once this process is complete, the Master Image can be deployed using Machine Creation Services. The naming of the virtual machine is similar to Citrix Provisioning. The virtual machine is named as the published image template name followed by a date and time stamp. When a new version of the image is published, it is a new virtual machine. The new virtual machine is then used to update the existing catalog to roll out changes. It is important when using MCS that the template used to create the Master Images must have been created from a real virtual machine. Machine has been booted in Windows and has the right time zone set. This task ensures that the virtual BIOS is properly configured. If this task is not done, the resulting booted image has its time-off by some hours based on UTC. The best way to create the template is to use a clone of the original gold image used to create the OS layer. This step ensures that the virtual hardware settings match from the OS layer through to the Master Image.

## Citrix App Layering Cross-Platform Support

The Citrix App Layering architecture is designed to support for many hypervisors and provisioning systems without creating layers specific to any one platform. As many organizations move towards multi-cloud or hybrid cloud environments, Citrix App Layering ease this migration.

[![AL-Image-14](/en-us/tech-zone/design/media/reference-architectures_app-layering_014.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_014.png)

App Layering supports multiple platforms on multiple hypervisors with the combination of connectors and Platform Layers.

**App Layering Connector -** The App Layering Connectors are developed in node.js and run from the App Layering appliance. The connectors provide integration to all supported platforms including both hypervisors and provisioning systems.

**Platform layer –** This layer is similar to an application layer except that always has the highest priority and when publishing images cleanup “recipes” are run differently against platform layers than app layers. Platform layers are where software drivers are installed for a defined “Platform.” For example, when using Citrix Provisioning the VDA software and PVS software are both installed into the Platform Layer.

App Layering Connectors and Platform Layers are combined to support available platforms. Learn more about the multiple hypervisors and cloud-platform deployment details and configurations, refer to the [product documentation](/en-us/citrix-app-layering/4/plan.html).

## App Layering Communication Flow

App Layering uses few connections and ports. These communication flows are documented following. For more information on the communications, requirements see the [product documentation](/en-us/citrix-app-layering/4/manage/firewall-ports.html).

### App Layering appliance

The App Layering Management Console is a Microsoft Silverlight based console accessed over TCP/IP on port 80 (HTTP) or 443 (HTTPS).

### Management Appliance Access

| **Protocol** | **Port** |
| ------------ | -------- |
| HTTP         | 80       |
| HTTPS        | 443      |
| SSH          | 22       |
| Log download | 8888     |

In addition to HTTP and HTTPS, the administrators are able to access the App Layering appliance virtual machine directly using SSH on port 22. Access is not permitted on unsecured Telnet or FTP ports. SSH is used to log on to the appliance as "root" for full access and an “administrator” account is used to access a limited menu to configure network settings. In Azure, "root" and "administrator" are both disabled. Instead, the administrators are prompted during the appliance provisioning for a local administrative user account.

When exporting logs from the management console, a download link is generated and presented in the task details. Port 8888 is used to download logs.

Port 8161 is used for ActiveMQ management and configuration, but access to this port is only available from within the App Layering appliance.

Optionally, the App Layering appliance can check for upgrades and download them from Citrix servers over the Internet using HTTPS/443 or HTTP/80. www.citrix.com and cdn.citrix.com are accessed if an Internet connection is available.

### Connector Configuration

Each connector type uses a different port. The current list of connectors is:

| **Connector**       | **HTTP port** | **HTTPS port** | **Internal Ports** |
| ------------------- | ------------- | -------------- | ------------------ |
| Azure               | 3000          | 3500           | 3001/3501          |
| Citrix Hypervisor   | 3002          | 3502           | 3003/3501          |
| vSphere             | 3004          | 3504           | 3005/3505          |
| Nutanix             | 3006          | 3506           | 3007/3507          |
| Citrix Provisioning | 3009          | 3509           | 3010/3510          |
| Hyper-V             | 3012          | 3512           | 3011/3511          |

Connectors open as a separate webpage within the management console. Each connector has both an HTTP and HTTPS listening port. When a connector is opened, the administrator is redirected to an HTML5 based interface in a new tab. The administrator’s browser must be able to access the App Layering appliance on the ports listed above for each connector.

### Miscellaneous Ports

The App Layering appliance talks to various network servers and services on their respective ports. The App Layering appliance connects to the following services on the following TCP ports:

| **Destination**         | **Protocol**          | **Port** |
| ----------------------- | --------------------- | -------- |
| Active Directory Server | LDAPS (LDAP over SSL) | 636      |
| Active Directory Server | LDAP                  | 389      |
| Active Directory Server | DNS                   | 53       |
| Windows File Servers    | SMB                   | 445      |
| Network Time Servers    | NTP                   | 123      |
| DHCP server             | DHCP                  | UDP/67   |
| App Layering appliance  | DHCP                  | UDP/68   |

### The Agent Service

The Agent Service can be used for three separate functions:

*  Integration with Citrix Provisioning
*  Integration with Hyper-V
*  Integration with a general scripting server

The Agent Service is accessed on the following ports.

| **Agent Server**    | **Destination**        | **Function or Protocol**                     | **Port** |
| ------------------- | ---------------------- | ----------------------------------------- | -------- |
| All                 | App Layering appliance | Registration or HTTPS                        | 443      |
| All                 | Agent Server           | Commands from App Layering appliance or SOAP | 8016     |
| All                 | App Layering appliance | Log export                                | 8787     |
| Citrix Provisioning | App Layering appliance | Disk Upload or HTTP                          | 3009     |
| Citrix Provisioning | App Layering appliance | Disk Upload or HTTPS                         | 3509     |

At initial installation of the App Layering Agent, the installer opens a connection on port 443 to the App Layering appliance to register the Agent Server. The App Layering appliance stores the FQDN and short name for the Agent Service Host, and the registry on the Agent server contains a record of the App Layering appliance.

Once the Agent and the App Layering appliance have exchanged identities, the App Layering appliance communicates directly to the Agent service on a secure SOAP channel on port 8016. Most communications between the appliance and the agent work as follows:

| **Host**               | **Action**                                                   |
| ---------------------- | ------------------------------------------------------------ |
| App Layering appliance | Hello to Agent on port 8016                                  |
| App Layering appliance | Command request to Agent opened                              |
| Agent                  | Runs a PowerShell command as the configured user account   |
| Agent                  | Transmits reply to App Layering appliance on existing request channel |
| Agent                  | Goodbye                                                      |

The specifics of the actual command being sent can often be seen in the Connector log on the App Layering appliance or the applayering.agent.log file on the Agent Service server.

When the appliance is asked to generate a log bundle, it transmits a request to every Agent Service that has ever been registered on the appliance requesting logs from the agent. Each Agent Service generates its own log bundle and transmit it back to the App Layering appliance on port 8787.

The main function of the Agent Service is to transfer files to Citrix Provisioning or Hyper-V. When transferring files, the agent uses the Windows Background Intelligent Transfer Service (BITS) on the Agent Service Server to pull the virtual disk to the server and place it in the store or upload or download a VHD from Hyper-V.

The process of transferring a file works like this:

| **Host**               | **Action**                                                   |
| ---------------------- | ------------------------------------------------------------ |
| App Layering appliance | Hello to Agent on port 8016                                  |
| App Layering appliance | Command request for file upload                              |
| Agent                  | Runs BITS as the configured user account                     |
| BITS                   | Opens a connection to the Citrix Provisioning connector on port 3009 on the App Layering appliance |
| BITS                   | Downloads the file to the specified repository location    |
| App Layering appliance | Command to get transfer status                               |
| Agent                  | Polls BITS for status and reports back to the App Layering appliance |
| BITS                   | Finishes                                                     |
| Agent                  | Reports complete status to the App Layering appliance      |

Normally, the file transfer runs on port 3009, which is unencrypted. This communication is done for performance reasons – the overhead of running on HTTPS connection significantly impacts throughput. However, the Agent can be configured to force HTTPS and use 3509 instead.

When the Agent runs BITS, it provides BITS two things: the URL for the file download, and the destination file path. BITS is executed as the user account that is configured in the Connector. Therefore, this user needs permissions to make an outgoing connection from the Provisioning server to port 3009 on the App Layering appliance; and also rights to write a file into the virtual disk store.

### Hypervisors

Hypervisor Connectors use the following ports.

| **Connector**     | **Destination**              | **Port** |
| ----------------- | ---------------------------- | -------- |
| Azure and Hyper-V | Azure Management             | 443      |
| Citrix Hypervisor | Citrix Hypervisor            | 5900     |
| vSphere           | Virtual Center               | 443      |
| vSphere           | ESX hosts for disk transfers | 443      |
| Nutanix           | Nutanix CVM                  | 9440     |

These ports are the same ports that the hypervisor browser-based management consoles also use. App Layering is using well-defined API calls through the hypervisor’s normal web service for communications with the hypervisor.

For vSphere file uploads, and downloads are not performed by communicating with vCenter. And handled by direct communication with an ESXi host. For this reason, the vSphere connectors require a host to be defined and the host firewall must be configured to allow access from the App Layering appliance on port 443.

### Compositing Engines

Compositing Engines connect back to the App Layering Appliance for iSCSI connections on port 3260 and they make API calls to the appliance on port 443. The App Layering Appliance performs API calls to the Compositing Engines also on port 443.

## Availability, Backup, and Recovery

App Layering can have several components where backups are appropriate. Including the appliance, Elastic Layer Shares, and User Layer Shares.

### Appliance Backups

The App Layering appliance, as stated earlier, is not required for end users to have full use of App Layering, therefore, it is not a requirement to make the appliance highly available. However, it has to be considered a requirement to back up the appliance regularly. Appliance backups ensure that all the layers are available even if the appliance is somehow destroyed or corrupted. Any virtual machine back-up technology can be used for the App Layering appliance. It is also possible to use two appliances and the import and export feature to keep them in sync. However, this step is a manual process now.

### Availability and Disaster Recovery for Elastic Layers

Elastic Layers are files mounted on virtual desktops agents (VDAs) during log-on using a Windows in-guest mount from an SMB share. The Layering Service connects the layers on logon, but it never reconnects a disconnected VHD file. Therefore, it's critical to ensure that the file server used for Elastic Layers is highly available by using some type of clustered file system. Using multiple DFS-R targets is not sufficient for this use case, because if a target fails, the mounted VHD files cannot be remapped to another target until another logon happens.

For Disaster Recover, there are two models that can be used to handle Elastic Layers; a replication model or a dual appliance model.

Reference: [App Layering 4.x availability and recovery concepts guide](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/resources/app-layering-4x-availability-recovery-guide.html)

### Replication Model

In this model, Elastic Layer shares can be replicated using any file-system replication technology. This model can include technologies like DFS-R, Microsoft Storage Replica, Veeam, NAS vendor replication, Zerto, VMware vSphere Replication, and even robocopy.

[![AL-Image-15](/en-us/tech-zone/design/media/reference-architectures_app-layering_015.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_015.png)

Then the VDAs in the DR data center can be pointed to the Elastic Layer share in that data center via GPO. A GPO template to configure the repository location can be found here: [CTX222107](https://support.citrix.com/article/CTX222107).

### Dual Appliance Model

In this model, an appliance is installed into each data center. The import and export functionality provided by the Appliance is used to keep the two appliances in sync from a layer perspective. Here the administrators manage DR layers separately and build images in DR from a local appliance.

[![AL-Image-16](/en-us/tech-zone/design/media/reference-architectures_app-layering_016.png)](/en-us/tech-zone/design/media/reference-architectures_app-layering_016.png)

If this option is chosen, then the sync transfers over the WAN from the SMB share defined during the import and export operation. Then Layers must be assigned on the DR appliance copies them out to the Elastic Layer share in DR. In this model, it is also possible to develop a solution that does not sync all layers but only desired layers. Since layers are chosen for export manually, only selected layers can be included in the process. Currently, the import and export process must be kicked off manually.

In the dual appliance model, connectors and permissions for elastic shares must be created on each side. The only objects that get imported are the actual layers themselves. However, it is possible in this model to have different layers in each site as needed. For example, if it is really an active-active site scenario.

## Availability and Disaster Recovery for User Layers

User Layers are similar to Elastic Layers in that they are VHD files mounted by Windows on logon using an in-guest mount. However, they are mounted as writable, and the files are locked by the Windows desktop. This task makes the options for backing up and replicating these disks much more difficult than normal Elastic Layers.

Due to this limitation, in case DFS-R or a robocopy script is used, the synchronization process has to be performed off hours (if there are times considered off hours). The process has to constantly check to see if the files are available to sync. For User Layers, which can be large files, robocopy would not work well as it would always copy the entire file rather than the blocks that have changed. DFS-R would be a better choice because it copies only modified blocks. However, replication at the storage level would be even better as it would synchronize more evenly as changes are made rather than waiting for the file locks to be removed.

There are other options that are supported here as well depending on the technology chosen to store the User Layer VHD files. If the file server supports the Volume Shadow Copy Service (VSS), then VSS snapshots are created to allow for backup and replication of the User Layers. By varying the frequency of the process User Layers can then also be rolled back to earlier points in time if corruption or mistake is made that adversely affects the user.

If a storage controller supports NDMP, this feature can also be used for User Layer backups. NDMP works at the storage level to back-up NAS storage directly to disk or tape utilizing NAS snapshots.

Due to the difficulty of replicating large disks and the expense for the bandwidth to do so, many customers choose to provide DR desktops for users without a replicated User Layer. In this model, there are two options. Users can be provisioned a separate delivery group in the DR site that is also User Layer enabled. They can then log on to the DR site and pre-configure that layer with what they need. Or users can be provisioned with use a normal non-persistent DR desktop. In the latter case, it is often beneficial to mix Citrix Profile Management with the User Layer so that user settings can be replicated to the DR site.

Reference: [Citrix App Layering backup and recovery](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/resources/app-layering-4x-availability-recovery-guide.html)

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this task easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-sa9ac47a833a43b38).

## References

The following resources are referenced for a better understanding of Citrix App Layering:

[App Layering Product Documentation](/en-us/citrix-app-layering/4.html)

[How to Create a Platform Layer](https://support.citrix.com/article/CTX225997)

[Understanding Elastic Layering](https://www.citrix.com/products/xenapp-xendesktop/resources/understanding-elastic-layering.html)

[App Layering Availability and Recovery Concepts Guide](https://www.citrix.com/products/xenapp-xendesktop/resources/app-layering-4x-availability-recovery-guide.html)

[Windows and App Layering Optimizations](https://www.citrix.com/blogs/2018/01/08/optimizing-windows-and-citrix-app-layering/)

[App Layering Antivirus Guide](/en-us/citrix-app-layering/4/layer/layer-antivirus-apps.html)

[Understand the Office Recipe](https://support.citrix.com/article/CTX224566)

[App Layering Best Practices](https://support.citrix.com/article/CTX225952)
