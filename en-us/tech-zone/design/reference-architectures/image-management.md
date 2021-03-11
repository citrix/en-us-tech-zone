---
layout: doc
h3InToc: true
contributedBy: Nagaraj Manoli
specialThanksTo: Martin Zugec, Allen Furmanski, James Hsu
description: Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering, and maintaining virtual machine images in your environment.
---
# Citrix Virtual Apps and Desktops Image Management

## Audience

This document is intended for Citrix technical professionals, IT decision makers, partners, and architects who want to explore image management services with Citrix Virtual Apps and Desktops either in on-premises or cloud environments. The reader must have a basic understanding of Citrix products, hypervisors, and cloud frameworks.

## Objective of this document

This document provides an overview of product functionality and design architecture for an image management environment to ensure efficient delivery of application and desktop workloads for an organization. The document is focused on Citrix image management services with conceptual deployment scenarios.

Regarding image management, there are two provisioning models that enable Citrix administrators to manage the Citrix environment efficiently:

*  Machine Creation Services (MCS)
*  Citrix Provisioning (PVS)

## Architectural Design Framework

Virtualization solutions from Citrix enable organizations to create, control and manage virtual machines, deliver applications and implement granular security policies. The Citrix Virtual Apps and Desktops solution provides a unified framework for developing a complete digital workspace offering. This offering enables Citrix users to access applications and desktops independent of their device’s operating system and interface.

The Citrix architectural design framework is based on a unified and standardized layer model. The framework provides a foundation to understand the technical architecture for most of the common Virtual Apps and Desktops deployment scenarios. These layers are depicted in the conceptual diagram.

*  ***User Layer*** - This layer defines the user groups and locations of the Citrix environment.
*  ***Access layer*** - This layer defines how users access the resources.
*  ***Resource layer*** - This layer defines the provisioning of Citrix workloads and how resources are assigned to the given users.
*  ***Control layer*** - This layer defines the components that control the Citrix solution.
*  ***Platform layer*** - This layer defines the physical elements where the hypervisor components and cloud service provider framework run to host the Citrix workloads.
*  ***Operations Layer*** - This layer defines the tools that support the delivery of the core solutions.

[![IM-Image-1](/en-us/tech-zone/design/media/reference-architectures_image-management_001.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_001.png)

Image management services, fit into the Control, Platform, and Operations layers to manage virtual machines in the Resource layer. The following sections go through the Citrix Machine Creation Services (MCS) and Citrix Provisioning (PVS) concepts as these are the basic building blocks of image management in a Citrix Virtual Apps and Desktops environment.

## Why Image Management is necessary?

Image Management is an approach of creating a master or golden image that contains the operating systems and all the required applications to deliver that single virtual image to multiple target virtual machines. The key concept behind image management is reusability and simplified management, which allow the Citrix administrator to deliver the necessary operating systems with the required set of applications to appropriate users based on their needs.

[![IM-Image-2](/en-us/tech-zone/design/media/reference-architectures_image-management_002.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_002.png)

The Citrix Virtual Apps and Desktops solution has two provisioning models for image management: Citrix Machine Creation Services and Citrix Provisioning.

### Machine Creation Services (MCS)

Citrix Machine Creation Services is a component of the Citrix Virtual Apps and Desktops solution that is coupled within the Delivery Controller. Using application programming interfaces (APIs) from the underlying hypervisor or cloud provider, MCS builds intelligent linked clones from a master image to provision multiple virtual desktops. The clones include a differencing disk and an identity disk linked from a base disk.

Machine Creation Services (MCS) configures, starts, stops, and deletes virtual machines using the hypervisor APIs. MCS is a disk-based provisioning approach that works with major hypervisors and leading cloud platforms.

#### Why MCS?

Citrix Machine Creation Services offers a simplified approach for image management through the following features:

*  Simple to deploy and manage using Citrix Studio
*  Technology embedded into core product and no additional infrastructure required
*  Better suited for Cloud provisioning
*  Ideal for both persistent and non-persistent workloads

### Citrix Provisioning (PVS)

Citrix Provisioning streams a single shared disk image to multiple individual machines rather than copying images to them. Citrix Provisioning enables organizations to reduce the number of disk images that they have to manage, even when the number of machines continues to grow.

Also, machines are streaming from a single shared image in real time, machine image consistency is ensured, at the same time large pools of machines can completely change their configuration, applications, and even operating systems all within the time it takes to reboot. This best-in class approach enables organizations to install and update the security and application patches to a single shared image in minimal time while meeting business objectives.

#### Why PVS?

The proper use of Citrix Provisioning allows for more efficient image management through the following features:

*  High scalability
*  Reduced storage requirements
*  Increased IOPS efficiency
*  Integrated version management capabilities
*  Supports for both physical and virtual targets

## Choosing the right provisioning model

Citrix MCS and PVS are mature provisioning platforms proven at scale. However, there are considerations when deciding on which one to use or if both are appropriate for a given environment. Citrix MCS is embedded into the delivery controller and is managed from within the Citrix Studio Console. Citrix Provisioning requires separate servers, network considerations, a database and it has its own management console.

The following table compares both the Citrix MCS and PVS models.

|                   **Capability**                   |    **MCS**    |    **PVS**    |
|:--------------------------------------------------:|:-------------:|:-------------:|
|    Support for virtual RDS and VDI workloads       |       X       |       X       |
|    Support for pooled and personal VDI desktops    |       X       |       X       |
|    Support for physical machines                   |               |       X       |
|    Support for Microsoft Azure machines            |       X       |               |
|    Support for Amazon Web Services (AWS) machines                 |       X       |               |
|    Support for Google Cloud Platform | X |  |

You can read more about image management decision factors in our article [Choosing the Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html)

## Machine Creation Services

Citrix Machine Creation Service (MCS) plays a vital role in image management for Citrix Virtual Apps and Desktops environments. Citrix MCS services are coupled with the Citrix Delivery Controller and Cloud Connector hence it does not require any additional servers or infrastructure. With MCS, IT administrators simply access the Citrix Studio Console to create and deliver the virtual desktops and server images to the enterprise users either on-premises or with Citrix Cloud.

Citrix Machine Creation Services uses Application Programming Interfaces (APIs) from the underlying hypervisor or public cloud platform that enables Citrix MCS to create, configure, start, stop, and delete virtual machines to the on-premises, hybrid, private, and public cloud environments.

The administrator creates a virtual machine with the required OS, installs the necessary applications and Citrix Virtual Delivery Agent on the hypervisor or in the cloud. The IT administrator selects this as the master virtual machine to provision a group of virtual desktops or servers using the Citrix Studio management console. Citrix MCS creates a snapshot of master the VM and it copies the full snapshot to the storage repository to serve as the master image (base disk).

When provisioning multiple virtual desktops or servers, MCS includes two types of disks: a differencing disk and an identity disk for each virtual machine.

Citrix MCS supports both server and desktops OS environment.

For desktop OS environments, Citrix administrators can create three types of virtual desktops using Citrix Machine Creation Services

*  **Pooled-random desktops** are non-persistent virtual desktops assigned to users randomly every time they start a VDI session. These desktops erase any user-specific changes each time they reboot. With the Citrix Profile Management solution, the user specific data and settings can be stored on centralized file servers.

*  **Pooled-static desktops** are assigned to a specific user and only the assigned user will be able to use that desktop unless changed by an IT admin. The user's personal data and settings do not carry over from session to session. With the Citrix Profile Management solution, the user specific data and settings are stored on centralized file servers.

*  **Dedicated desktops** are assigned to individual users and the data and settings persist on the desktops. Optionally, the Citrix Profile Management solution can be used to store the user profile and data on central file servers. For dedicated desktops there is a new option available under Desktop OS Catalogs virtual machine copy mode, "Use full copy for better data recovery and migration support, with potentially reduced IOPS after the machines are created".

For the Server OS environment, Citrix administrators can deploy multiple hosted shared virtual machines for a Virtual Apps environment using the master VM image (base disk).

### MCS High-level VM and Disk Architecture

The first step when using Citrix MCS is to provision a master VM that serves as a template to create clones. The IT administrator can provision the VM with the required amount of CPU, RAM and disk space, and then install an operating system and required applications. Using the Citrix Studio Console, the admin creates a machine catalog of clone VMs using the base image. Those VMs live in a data store, which is different from PVS.

Citrix MCS is completely relying on storage. When the VM is provisioned two types of disks are created for each VM: a differencing disk and an identity disk.

Citrix MCS creates the number of VMs specified in the create catalog wizard with two disks defined for each VM on the storage. A copy of the master image is also stored in the same storage repository. If there are multiple storage repositories defined, then each one gets the following types of disks.

Each storage repository gets one full snapshot of the master VM image, which is read-only and shared across the VMs.

A unique identity disk (16 MB) for VM identity will also be created. The Delivery Controller creates the identity disks for each VM.

Each VM also gets a difference disk. A unique difference disk used to store any writes made to the VM. The disk is thin provisioned (if supported by the storage) and will increase to the maximum size of the base disk if necessary.

#### Full Clone

Sometimes, it is not desirable to create VMs with delta disks. A few reasons are mentioned below:

*  Some of the backup solutions don’t backup VMs that contain a delta structure
*  Storage migration becomes more complicated
*  VM migration does not work on all hypervisors
*  Deltas grows over time which leads to load on storage

For these reasons, MCS added a new capability in addition to creating the existing delta structure called full clones. When using persistent VMs, Citrix MCS allows admins to select **VMs** to be created with a full clone of the master image.

[![IM-Image-3](/en-us/tech-zone/design/media/reference-architectures_image-management_003.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_003.png)

There is no special requirement for full clones. Citrix MCS uses its identity technology to change the identity of the full clone. Full clone machines have two disks, one for the actual VM, and one for identity including machine name, computer account and password. Full clone VMs can be moved to a different datastore or cluster which is not possible with linked clones.

While provisioning machines through Citrix Studio, full clones is only an option for desktop OS and not for server OS.

#### Machine Catalog

Collections of physical or virtual machines are managed as a single entity called a Machine Catalog in Citrix environments. While creating Machine Catalogs administrators have the option to select ways to provision VMs, and which Citrix image management tools such as Citrix Machine Creation Services or Provisioning Services.

Reference: [Citrix docs: Machine Catalogs](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html)

#### Host Connection

In Citrix Virtual Apps and Desktops, before creating the machine catalog it is important to create connections to hosting resources while creating a site to integrate an underlying platform including hypervisor or cloud providers. Configuring a connection includes selecting the connection type among the supported hypervisors and cloud services. The system requirements page lists the supported hypervisors and cloud options.

Reference: [Citrix docs: System requirements](/en-us/citrix-virtual-apps-desktops-service/system-requirements.html)

#### Host storage

A storage product is supported if it can be managed by a supported hypervisor. Provisioning machines, data is classified by type:

*  Operating system (OS) data, which includes master images

*  Temporary data, which include all non-persistent data written to MCS-provisioned machines, Windows page files, user profile data, and any data that is synchronized with Content Collaboration (formerly ShareFile). This data is discarded each time the machine restarts

Provisioning separate storage for each data type can reduce load and improves IOPS performance on each storage device.

#### Storage shared by hypervisors

Shared storage stores data that is retained for longer periods and provides centralized backup and management. This storage holds the OS disks and other disks associated with virtual machines. When using shared storage, local storage to the hypervisor used for temporary data cache, aids to reduce traffic for main OS storage. The disk is cleared after every machine restart, using local storage for temporary data. The provisioned VDA is tied to a specific hypervisor host. If the host goes down, the VM cannot start.

The hypervisor provides optimization technologies through read caching of the disk images locally. For example, Citrix Hypervisor (formerly XenServer) offers IntelliCache. This reduces network traffic to the central storage.

Citrix Hypervisor also supports read caching using the host’s free memory. The performance improvement can be seen whenever data is read from disk more than once, as it gets cache in memory.

Both read-caching and IntelliCache can be enabled simultaneously. In this case, IntelliCache caches the reads from the network to a local disk. Reads from that local disk are cached in memory with read caching.

Reference: [Citrix docs: storage read-caching](/en-us/citrix-hypervisor/storage/read-cache.html)

#### Local storage

Local storage stores data locally on the hypervisor local data store. This includes, master images and other OS data that are, transferred to all of the hypervisors in the site. This method increases network traffic along with management traffic.

When this method is selected, the option to choose whether to use shared storage to provide resilience and support for backup and disaster recovery systems is available.

### How MCS works with on-premises hypervisors

Below is the pictorial flow diagram and workflow depicting how Citrix Machine Creation Services works with on-premises hypervisors.

[![IM-Image-5](/en-us/tech-zone/design/media/reference-architectures_image-management_005.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_005.png)

[![IM-Image-4](/en-us/tech-zone/design/media/reference-architectures_image-management_004.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_004.png)

Citrix Machine Creation Services uses hypervisor APIs to provision virtual machines. Each virtual machine is assigned an identity disk that gives the machine a unique identity and a differencing disk that handles the writes for the virtual machine.

***Instruction Disk:*** This small instruction disk contains the steps of the image preparation to run and is attached to that VM. The preparation virtual machine is then started, the image preparation process begins, and the virtual machine is shut down.

***Identity Disk:*** A unique Identity Disk used to provide each virtual machine with a unique identity. The functionality within the Delivery Controller creates the identity Disks. This disk is 16 MB in size.

***Differencing Disk:*** A unique Difference Disk is used to store any writes made to the VM. The disk is thin provisioned and will increase to the maximum size of the base VM as required.

#### Cache for Temporary Data

For the pooled (not dedicated) machines in a machine catalog, administrators can enable the use of temporary data cache on the machine. To enable this feature, the VDA on each machine in the catalog must be minimum version 7.9 and above. This feature is also referred to as MCS-IO.

The administrator must specify the storage type for temporary data that the catalog uses. Enabling temporary cache in the catalog includes Memory allocated to Cache (MB) and Disk cache size (GB).

[![IM-Image-6](/en-us/tech-zone/design/media/reference-architectures_image-management_006.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_006.png)

*  The temporary data is written to the memory cache until it reaches the limit, when the temporary data reaches the configured limit, the cold data is moved to temporary data cache disk.

*  Memory cache is part of total memory on each machine, before enabling this option, consider increasing the total amount of memory of each machine.

*  Enabling only disk cache size, temporary data is written directly to the cache disk, using a minimal amount of memory cache.

*  Disabling both options, temporary data is not cached and it is written to the differencing disk of each VM.

#### Citrix MCS with Linux VDAs

Citrix Machine Creation Services offers administrators the ability to create Linux VMs starting from Citrix XenApp and XenDesktop 7.18 and later. Prepare a master virtual machine on the hypervisor or cloud provider and install the Linux Virtual Delivery Agent on this template VM. Create a machine catalog in Citrix Studio using the template VM and then create a delivery group to provision the Linux VMs to enterprise users.

Reference: [Citrix docs: Linux Virtual Delivery Agent](/en-us/linux-virtual-delivery-agent/current-release/installation-overview.html)

## Citrix Cloud and Machine Provisioning

Citrix Cloud manages the operation of the control plane for Citrix Virtual Apps and Desktops Service environments. Delivery controllers, management consoles, SQL database, License Server, StoreFront, and Citrix Gateway are all delivered on Citrix Cloud and managed by Citrix.

The workloads hosting the apps and desktops for users remain under the customer control in the data center of their choice, either cloud or on-premises. These components are connected to the cloud service using an agent called the Citrix Cloud Connector.

Citrix Machine Creation Service uses APIs from underlying hypervisors. These resources can come from the customer’s data center or cloud. Citrix Cloud Connector acts as a bridge between the Citrix Cloud plane and underlying resources. The control plane has access to metadata, such as login details, machine names and application shortcuts, restricting access to the customer’s intellectual property from the control plane.

Data flowing between the cloud and customer premises uses secure TLS connections over port 443.

[![IM-Image-7](/en-us/tech-zone/design/media/reference-architectures_image-management_007.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_007.png)

While provisioning VMs using the MCS method ensure that the hypervisor or cloud service has enough processors, memory and storage to accommodate virtual machines.

Installing the latest hypervisor tools on the golden image is required so that applications and desktops function normally. It is advised to not run Sysprep on master images as MCS handles machine identity itself.

Reference: [Citrix docs: Citrix Cloud and Machine provisioning](/en-us/provisioning/current-release/configure/cloud-connector.html)

### Common use case

Citrix MCS is a best fit for production environments which meet the criteria as follows:

*  Deploying in a cloud environment
*  Intending to deploy NFS storage or clustered shared volumes
*  Availability of high IOPS storage (MCS directs more read activity to the shared storage)

Citrix MCS offers a simple management plane through Citrix Studio and easy to provision workloads from a single UI. There is no extra infrastructure needed. Let’s assume running mixed workloads using the Citrix MCS provisioning method. This includes hosted shared desktops, Linux VMs, full clone VMs, GPU-based workloads and a few Windows apps.

[![IM-Image-8](/en-us/tech-zone/design/media/reference-architectures_image-management_008.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_008.png)

The above diagram is a conceptual deployment scenario for running mixed workloads that support task workers and power users in the same environment. The task worker workload is deployed through hosted shared desktops while the power users are using dedicated VMs deployed and separated into different datastores, as these VMs are the highest in IOP usage.

In the above deployment scenario, there is more than one delivery controller deployed in the environment to achieve high availability and load balancing. Delivery controllers are equipped with adequate processing power and memory to handle the user traffic. Microsoft SQL Servers are deployed in a high availability model so that if any one database server goes down, delivery controller operations like fetching the user details, responding to StoreFront requests are not impacted.

As the number of applications increases, resource consumption also goes up in the environment. It is best practice to pre-calculate the number of users and types of workloads that are going to run in the environment. Citrix MCS is simple to manage from Citrix Studio, there is no extra infrastructure required hence it is easy to deploy on leading hypervisors and cloud platforms.

## Best practices for MCS with Citrix Virtual Apps and Desktops

There are several aspects that must be taken into account before provisioning virtual machines using Citrix Machine Creation Services. Citrix MCS is capable of delivering virtual RDS and VDI workloads on Citrix Hypervisor, Hyper-V, vSphere, AHV and also with leading cloud providers.

The following infrastructure considerations should be addressed before provisioning virtual machines using Citrix MCS

*  Storage
*  Temporary Cache
*  OS optimization
*  Delivery controller
*  Scalability

### Storage

Storage configuration and sizing’s are the deciding factor when using Citrix Machine Creation Services.

**Capacity Considerations:** When VMs are created using Citrix MCS a minimum of two disks are created: one is the delta disk containing the OS as copied from the master image and the other one is the identity disk (16 MB) containing Active Directory identity data for each VM. Extra disks can be added to satisfy certain use cases.

The Citrix Hypervisor IntelliCache feature, creates a read-only disk of the master VM on local storage on each host. It is recommended to pre-calculate storage before provisioning the end user machines.

**Hypervisor overhead:** Different hypervisors create specific sets of files that generate overhead on a per VM basis. For example, log files, hypervisor specific configuration files and snapshot files are also saved on the storage.

**Process overhead:** Initial catalog creation requires the base disk to be copied to each storage repository. Adding a new machine to a catalog does not require copying the base disk to each storage repository. The catalog updates process creates an extra base disk on each storage repository and can also experience temporary storage peak.

**Others:** RAM sizing and thin/thick provisioning approaches are also considered for provisioning the virtual machines.

### Temporary Cache / MCS storage I/O optimization

Temporary cache in a catalog includes two options, the first one with memory and the second one on disk. With memory or disk, part of the resource is consumed for temporary cache operations, hence it is recommended to check available memory and disk space from the host where VMs are running. In the the case using disk for temporary cache for better performance, SSD disks or high IOPS storage solutions are recommended.

### OS optimization

For better performance and to minimize the consumption of resources on the host, it is recommended to optimize the operating system by running the Citrix Optimizer Tool.

**Citrix Optimizer:** By default, Microsoft Windows desktop images contain numerous features that aren’t needed in a VDI environment. The Citrix Optimizer is a Windows tool developed by Citrix to help administrators optimize various components in their environment. The tool is PowerShell based, but also includes a graphical UI.

Citrix Optimizer provides various templates for optimization. Choose the right template for the operating system so that unnecessary services, configuration entries, and applications are disabled or removed. Admins can expect to realize fairly significant performance gains after optimization.

To download and install the latest Citrix Optimizer visit: [https://support.citrix.com/article/CTX224676](https://support.citrix.com/article/CTX224676).

### Delivery Controller

In Citrix MCS deployments, the Delivery Controller is the core component of the infrastructure. It is recommended to deploy delivery controllers and Microsoft SQL Server in high availability mode so that if any one delivery controller goes down normal operations will not be impacted.

In medium or large-scale deployments, delivery controllers must have enough memory and computing power so that there will not be any CPU and memory bottlenecks in the environment.

While connecting to hosting resources, make sure to check the compatible version of the hypervisor so that there will be no issues while provisioning. It is recommended to keep the master copy in the high IOPS data store /LUN on SSD or NVMe drives so that maximum efficiency and performance can be achieved.

### Scalability

The Machine Creation Services functionally is bundled within the delivery controller and this interacts with the underlying hypervisor and cloud provider framework APIs. When expansion of the environment storage, it can become a bottleneck so it is recommended to have extra storage clusters available so that scalability will not be impacted.

In medium and large-scale deployment, resource consumption is more as end user demand grows, it is recommended to deploy optimized images so that unwanted applications will not consume excessive resources.

## Citrix Provisioning (PVS)

Citrix Provisioning is different from traditional imaging solutions, fundamentally changing the relationship between hardware and the software that runs on it. A shared disk image is streamed over the network rather than copied to individual virtual machines. Citrix Provisioning enables enterprises to reduce the number of images that they have to manage and also provides centralized management with distributed processing.

A Provisioning Server is a server that has the Citrix Provisioning Soap and Citrix Stream Services installed. The Stream Service is used to stream software from virtual disk images, or vDisks to target devices. The Soap Service is used when accessing the console. Provisioning Servers are used to stream the contents of a vDisk file (containing a machine image) to target devices. vDisk files can reside directly on the Provisioning Server local hard disk or Provisioning Servers can access the vDisks from a shared-storage device on the network.

The Citrix Provisioning solution requires a SQL database to store all system configuration settings that exist within a farm. Provisioning Server advanced configuration options are available to ensure high availability and load-balancing of target device connections between PVS Servers.

## Overview of Citrix Provisioning

The below diagram depicts an overview of Citrix Provisioning and product infrastructure.

[![IM-Image-9](/en-us/tech-zone/design/media/reference-architectures_image-management_009.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_009.png)

### PVS Farm

A farm represents the top level of a Provisioning Services infrastructure. A farm also includes the SQL Database and a Citrix License Server, local and/or network shared storage, and collections of target devices.

### PVS Site

A site provides a method of representing and managing logical groupings of Provisioning Servers, Device Collections, and local shared storage. One or more sites can exist within a farm. The first site is created with the Configuration Wizard and is run on the first Provisioning Server in the farm.

### Device Collection

Device collections provide the ability to create and manage logical groups of target devices. Creating device collections simplifies device management by performing actions at the collection level rather than at the target-device level. A target device can only be a member of one device collection.

### Target Devices

A device, such as a desktop computer or a virtual machine, that boots and gets its OS image from a PVS virtual disk on the network, is considered a target device. A device that is used to create the base Personal vDisk image is considered a master target device.

### vDisks

vDisks acts like a hard disk for a target device and exists as disk image files on storage that is accessible by the PVS Servers. A virtual disk consists of a VHDX base image file, any associated properties files (.pvp), and optionally a chain of versioned VHDX differencing disks (.Avhdx).

Citrix Provisioning provides support for a full image lifecycle that takes a virtual disk from initial creation, through deployment and subsequent updates, and finally to retirement. The lifecycle of a virtual disk consists of four stages:

1)  Creating
2)  Deploying
3)  Updating
4)  Retiring

#### Creating a virtual disk

Creation of a virtual disk requires preparing the master virtual machine for imaging, creating and configuring a virtual disk store where the vDisks will reside, and then imaging the master target device (VM) to that file that results in a new base virtual disk image. This process is performed by the Citrix administrator using the Imaging Wizard.

#### Deploying a virtual disk

After the base virtual disk image is created, it is deployed by assigning it to one or more target devices. When the target device starts, it boots from an assigned virtual disk. There are two boot mode options. Private Image mode (single device access, read/write), and standard image mode (multiple device access, read only with write cache options).

#### Updating a virtual disk

It is necessary to update a base virtual disk image over its lifecycle so that the image contains the most current software and patches. Updates can be made manually, or the update process can be automated using virtual disk Update Management features. Each time a virtual disk is updated a new version is created. Different devices can access different versions based on the joint classification of the target device and virtual disk version: test, maintenance, or production.

A maintenance device has exclusive read/write access to the newest maintenance version, test devices have shared read-only access to test versions, and production devices have shared read-only access to production versions.

Updating a virtual disk involves the following:

*  Create a version of the virtual disk, manually or automatically
*  Boot the newly created version from a device (Maintenance device or Update device), install and save any changes to the virtual disk, then shut down the device
*  Validate with a test target device, then promote to Production and reboot all the production target devices

#### Retiring a virtual disk

Retiring a virtual disk is the same as deleting it. The entire VHDX chain including differencing and base image files, properties files, and lock files are deleted after being unassigned.

### Virtual disk Store

A store is the logical name for the physical location of the folder containing vDisks. This folder exists on a PVS server or on shared storage. When virtual disk files are created in the PVS Console, they are assigned to a store. Within a PVS site, one or more Provisioning Servers are given permission to access that store to serve vDisks to target devices.

### Write Cache

When the virtual disk is in private/maintenance mode, all data is written back to the virtual disk file. When the virtual disk is in standard mode or shared mode data, cannot be written back to the base virtual disk. Instead, it is written to a write cache file in one of the following locations:

*  Device RAM
*  Device RAM with overflow on hard disk
*  PVS Server

This write cache file is deleted on the next boot cycle so that when a target is rebooted or starts up it has a clean cache and contains nothing from the previous sessions, thus guaranteeing the consistency of the image.

By default, the PVS target software redirects the system page file to the same disk as the write cache file so that the pagefile.sys is allocating space on the cache drive unless it is manually set up to be redirected on a separate volume.

#### Cache in device RAM

Write cache can exist as part of the non-paged pool in the target device’s RAM. This functionality provides the fastest method of disk access since memory access is always faster than disk access.

This mode is useful when the server has enough physical memory and it is faster than other cache modes. It is important to pre-calculate workload requirements and set the appropriate RAM size, otherwise the target device may bluescreen due to insufficient space before the write cache is exhausted.

#### Cache on device RAM with overflow on hard disk

This method has moderate consumption of RAM and hard disk. Citrix recommends using this cache type for Citrix Provisioning because it combines the best of RAM with the stability of hard disk cache. The cache uses non-paged pool memory for the best performance. When RAM utilization has reached its threshold, the oldest of RAM cache data will be written to the local disk.

Better performance and easy to scale, achieving target device reliability in high demand workloads.

#### Cache on PVS server

The write cache can exist as a temporary file on a Provisioning Server disk. This generally results in increased network traffic as disk writes are redirected to a remote location from the target device.

This cache type is not recommended for a production environment as it is slower than the other options.

### High Availability of Citrix Provisioning

The key to establishing a highly available Citrix Provisioning environment is to identify the critical components, create redundancy for the critical components, and ensure automatic failover to the secondary component if the active component fails. Critical components for Citrix Provisioning include:

*  SQL Database
*  Provisioning Servers
*  vDisks and Storage

Citrix Provisioning provides several options to consider when configuring for a highly available implementation, including:

***Offline Database Support*** - This allows Provisioning Servers to use a local snapshot of the database if the connection to the database is lost to allow continued functionality.

***SQL AlwaysOn*** - Citrix Provisioning supports the SQL Always On high availability and disaster recovery solution.

***Database mirroring*** - A high availability solution for SQL Server implemented at the database level.

***Provisioning Server Failover*** - If one of the PVS servers becomes unavailable, another server within the site can handle the active target device connections with the virtual disk. Load balancing is enabled so the load is automatically balanced between the target devices and the remaining servers.

***vDisks and Storage*** - Provisioning Servers are configured to access a shared storage location. Citrix Provisioning supports various shared storage configurations including Windows shared storage and SANs.

Reference: [Citrix Docs: Managing for highly available implementations](/en-us/provisioning/current-release/advanced-concepts/managing-high-availability.html)

#### SQL Database for Citrix Provisioning

It is best practice to install the SQL database on a separate server or cluster other than where the PVS Server is located to avoid poor distribution during load balancing. Refer to the PVS system requirements for details on the SQL versions supported.

#### Database Sizing

Estimating the size of a database helps to determine the hardware configuration. This helps in achieving the performance, and storage allocation to store the data and indexes.

Reference: [Citrix Docs: Database sizing](/en-us/provisioning/current-release/install/pre-install.html)

#### Citrix License Server

The Citrix License Server is installed on a Windows server within the Citrix environment to communicate with all Citrix PVS servers to activate the licenses for PVS Servers. The License Server connectivity outage grace period is 30 days (720 hours). If connectivity to the Citrix License Server is lost, Citrix Provisioning continues to provision systems for 30 days. To achieve scalability, reliability and increase availability of the Citrix License Server, Microsoft clustering functionality can be used to create clustered License Servers.

#### New license type for Citrix Cloud

Citrix introduced a new license type (PVS_CCLD_CCS) that provides a traditional PVS license entitlement to customers of the Virtual Apps and Desktops Service in Citrix Cloud. Citrix Provisioning license options for Citrix Cloud are controlled by the options associated with Citrix Provisioning license types, on-premises, or Citrix Cloud. Using a License Server with Citrix Provisioning, Citrix Cloud licenses are consumed if the Cloud option is selected during initial setup. Conversely, an on-premises license is consumed if on-premises is selected when setting up Citrix Provisioning.

**Note**: This new Citrix Cloud license type replaces the existing on-premises Citrix Provisioning license for Desktops and Provisioning for Data Centers. It possesses the same license acquiring precedence as the on-premises licenses when bundling Citrix licenses.

The on-premises trade-up feature does not apply to Citrix Cloud licenses. Each Citrix Provisioning target device checks out a single Citrix Cloud license regardless of the operating system type.

#### Microsoft Volume Licensing

When running the PVS imaging wizard to create the virtual disk, configure the Microsoft Key Management Service (KMS) or Multiple Activation Key (MAK) volume licensing option that enables the Citrix Provisioning Server to activate the operating system of each target device.

KMS volume licensing utilizes a centralized activation server that runs in the data center, and serves as a local activation point (opposed to having each system activate with Microsoft over the internet).

A MAK corresponds to some purchased OS licenses. The MAK is entered during the installation of the OS on each system, which activates the OS and decrements the count of purchased licenses centrally with Microsoft. Alternatively, a process of ‘proxy activation’ is done using the Volume Activation Management Toolkit (VAMT). This allows activation of systems that do not have network access to the internet. Citrix Provisioning uses this proxy activation mechanism for Standard Image Mode vDisks that have MAK licensing mode selected when the virtual disk is created.

#### Active Directory Integration and Target Device Management

Integrating Citrix Provisioning and Active Directory allows administrators to select the Active Directory Organizational Unit (OU) in which Citrix Provisioning should create a target device computer account. It also allows it to take advantage of Active Directory management features, such as delegation of control and Group policy. Finally, configure the Provisioning Server to automatically manage the computer account passwords of target devices.

Before integrating Active Directory within the farm, verify that the following prerequisites are met:

*  The Master Target Device was added to the domain before building the virtual disk
*  The Disable Machine Account Password Changes option was selected when the image optimization wizard was run during imaging

Reference: [Citrix docs: Configuring vDisks for Active Directory management](/en-us/provisioning/7-15/active-directory-management.html)

### Citrix Provisioning Accelerator

Citrix Provisioning Accelerator acts as a provisioning proxy in Dom0 on a Citrix Hypervisor’s host, streaming data from the virtual disk is cached at the proxy before being forwarded to the virtual machine. This cache accelerates the boot time of other VMs residing on the same host since it’s not necessary to stream large amounts of data from the PVS Server over the network. Citrix Hypervisor’s local resources are consumed but this improves overall performance on the network.

Reference: [Citrix docs: Citrix Provisioning Accelerator](/en-us/provisioning/current-release/configure/configure-accelerator.html)

## Target Device Boot Process

When a target device is powered on, it needs to be able to find and contact a Provisioning Server to stream down the appropriate virtual disk. This information is stored in a so-called bootstrap file named ARDBP32.BIN. It contains everything that the target device needs to contact a Citrix PVS server so that the streaming process can be initialized.

The bootstrap file is delivered through a TFTP server, this also partly applies to the alternative BDM (Boot Device Manager) approach. There are some distinct differences between TFTP and BDM.

### TFTP

When using TFTP, the target device needs to know how and where it can find the TFTP server to download the bootstrap file before connecting to the PVS Server. TFTP can be configured in HA through a Citrix ADC to avoid a single point of failure. Provisioning Services has its own built-in TFTP server.

One of the most popular approaches in delivering the TFTP server address to target devices is through DHCP (though there are other options).

### BDM (Boot Device Manager)

There are two different methods to make use of the Boot Device Manager.

PVS offers a quick wizard which generates a relatively small .ISO (around 300 KB) file. Next, the administrator configures the target devices to boot from this .ISO file, using their virtual DVD drive. This method uses a two-stage process where the PVS server location is hardcoded into the bootstrap file generated by BDM. The rest of the information like the PVS device drivers is downloaded from the PVS server using a TFTP protocol (UDP port 6969), here TFTP will still be used.

When using the Virtual Apps and Desktops Setup Wizard to provision target devices, administrator can create and assign a small BDM hard disk partition, which will be attached to the virtual machine as a separate virtual disk. Using this method, the above mentioned two-stage approach is no longer needed because the partition already contains all the PVS drivers. This way all the information needed will be directly available without the need of PXE, TFTP & DHCP.

[![IM-Image-10](/en-us/tech-zone/design/media/reference-architectures_image-management_010.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_010.png)

The above diagram illustrates the high level boot steps. PXE is used for getting the TFTP server IP and bootstrap file name details by the clients and TFTP is used for downloading the bootstrap program file.

Reference: [Citrix article: CTX227725](https://support.citrix.com/article/CTX227725)

## Citrix Provisioning managed by Citrix Cloud

Citrix PVS and Citrix Cloud integration is essential when an admin wants to manage their deployments from anywhere using the Citrix Cloud portal. The **Citrix Cloud Connector plays a** key role and it enables the communication with provisioned VDAs to be used in the Citrix Cloud Virtual Apps and Desktops Service providing proxy functionality for commands to remote hypervisors and clouds.

There are a few elements to be considered when using Citrix Provisioning with Citrix Cloud.

*  Citrix Virtual Apps and Desktops Delivery Controller in Citrix Cloud
*  Citrix Cloud Connector located in one or more resource locations
*  Provisioning Server located on-premises (v7.18 or later)
*  Remote PowerShell SDK used by Citrix Virtual Apps and Desktops Setup Wizard to push VDA records to the Delivery Controller in Citrix Cloud.

To connect an existing Citrix Provisioning deployment to Citrix Cloud:

*  Add Cloud Connector servers
*  Upgrade Citrix Provisioning to version 7.18 or later
*  Install the Remote PowerShell SDK to be used on the Citrix Provisioning Console with Citrix Virtual Apps and Desktops.

[![IM-Image-11](/en-us/tech-zone/design/media/reference-architectures_image-management_011.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_011.png)

Citrix Cloud integration enables Citrix Provisioning to add the newly provisioning VDAs to a machine catalog in the Citrix Cloud Virtual Apps and Desktops Delivery Controller located in Citrix Cloud. This process follows one of these two methods:

*  Add new devices using the Virtual Apps and Desktops Setup Wizard in the Citrix Provisioning Console
*  Import the existing Citrix Provisioning target devices using the Machine Catalog creation in Studio

Citrix Studio uses the PvsPsSnapin to communicate with the PVS Server. This snap-in has been extended to enable communications from the Citrix Virtual Apps and Desktops Service to the PvsMapiProxyPlugin (in Citrix Cloud Connector). Communication happens over HTTPS (TCP 443). The PVS administrator credentials are sent over this secure channel. The credentials are then used by the proxy to emulate the PVS administrator before contacting the PVS server.

Reference: [Citrix docs: Citrix Provisioning managed by Citrix Cloud](/en-us/provisioning/current-release/configure/cloud-connector.html)

### Common Use Case

Citrix Virtual Apps and Desktops addresses a wide-ranging set of business requirements and use cases.

For example, in finance, marketing or any medical field, users are considered as normal office workers, knowledge workers or a power user.

Citrix Provisioning eases administrator work and provides the following capabilities

*  Fast provisioning of machines
*  Centralized and secure data
*  Consistency and a more dynamic environment based on user groups

Citrix Provisioning enables administrators to create multiple vDisks with various business-oriented applications based on user groups and their needs. For office workers, they typically require only a limited number of Windows applications for day-to-day work. For multimedia workers, who require running animation software, medical scan reports and so on hardware-accelerated systems with virtual GPUs from AMD, Intel or NVIDIA can be used.

| **Type of workloads** | **Descriptions** |
| --- |--- |
| Homogeneous workers | Typically, in a call center scenario,users accessing Microsoft Office and other day-to-day applications. Deploying multiple virtual machines using single master image that contains Microsoft Office and other required application. |
| Deploying Hosted Shared Desktops or Streamed Desktops | In a large user environment, multiple non-persistent user groups have access to desktops and applications. Scale ranges from tens of thousands of desktops with Citrix Provisioning helping to quickly deliver the required workloads.|
| If environment IOPS constrained | Citrix Provisioning is the best fit for such environments using iSCSI or less bandwidth network channel where IOPS is constrained. |
| Large number of applications | Citrix Provisioning helps to create multiple server OS instances to run all the necessary departmental applications. |
| High performance workloads | This is similar to power workers requiring more CPU, RAM, and benefiting from GPU. |

### Deployment scenario for education sector

In education sectors, IT has become a part of their system. Growing demand and delivering applications and data for thousands of unique users is the challenge. Secure remote access to applications such as Hyper chemistry, MATLAB, SAS, Mathematica, Office and so on are also required.

Virtualize and stream dozens or hundreds of applications to end-user’s on any device at scale. Citrix Provisioning Server helps administrators to overcome provisioning hurdles with the “Do more with less” concept. Assuming different sets of workloads needs to be run and provisioned through Citrix Provisioning.

[![IM-Image-12](/en-us/tech-zone/design/media/reference-architectures_image-management_012.png)](/en-us/tech-zone/design/media/reference-architectures_image-management_012.png)

The above diagram depicts multiple workloads to run on different sections/labs in the university. vDisks contain different operating system and applications stored in the shared storage and with the help of Citrix Provisioning, vDisks are streamed to different labs with different sets of workloads over the network. It is easy to scale on demand with rapid deployments.

Citrix Provisioning enables a blended delivery strategy that results in supporting mixed workloads and satisfies various use cases. Key highlights are,

*  Enables students and faculties to learn and teach anytime
*  Cuts costs while increasing IT services
*  Enhances competitive edge in higher education, where technology is a key differentiator

In all deployments, the PVS Servers must have enough processing power and should meet all the networking needs including NIC teaming, better bandwidth, etc.

## Best Practices for Citrix Provisioning with Citrix Virtual Apps and Desktops

While designing a Citrix Virtual Apps and Desktops solution, it is important to consider Provisioning Servers to align with business needs. The components included in designing are Active Directory Services, network and security architecture, server hardware types, storage infrastructure, the virtualization platform and operating systems.

This section provides generic best practices in the following areas

*  Networking
*  Storage
*  Delivery Controller
*  Network switches
*  Virtual desktops images/Target devices
*  Scalability

### Networking

**Domain Name System:** Dynamic updates are a key feature in DNS. This eliminates the need of manual entries of names and IP addresses into the DNS database. Securing dynamic updates will verify with Active Directory machines, which are requesting updates to the DNS. This means only computers that have joined the Active Directory domain can dynamically update the DNS database.

**Network Interfaces:** Citrix recommends using multiple NICs in Provisioning Server machines. A teamed pair of NICs has to be configured for streaming the vDisks via the PVS Stream Service and for network access to enterprise storage systems or file shares. A dedicated network or VLANs is also suggested for deployment.

### Storage

Storage requirements for PVS Servers depend upon the number of virtual disk images to be created and maintained. The size of vDisks depends on the number of applications to be installed and the operating system.

To minimize the storage space required, Citrix recommends minimizing applications on each virtual disk and minimize the number of vDisks. Each target machine contains a volatile write cache file. The size of the cache file for each VM depends on types of applications used, user workloads and reboot frequency.

**SAN/NAS:** In a high availability deployment, a shared volume is required and a volume must be accessible from multiple hosts. A read-only volume is used for storing vDisks in standard mode. Private image mode requires read/writes access.

### Delivery Controller

As a best practice, production sites should always have at least two controllers on different physical servers in on-premises deployments (within Citrix Cloud this is managed automatically). Each Controller communicates directly with the site database.

### Network Switches

**Disable spanning tree and enable port fast:** With Spanning Tree Protocol (STP) or Rapid Spanning Tree Protocol, the ports are placed into a blocked state while the switch transmits Bridged Protocol Data Units (BPDUs) and listens to ensure the BPDUs are not in a loopback configuration.

The amount of time it takes to complete this convergence process depends on the size of the switched network, which might allow the Pre-boot Execution Environment (PXE) to time out, preventing the machine from getting an IP address.

To resolve this issue, disable STP on edge-ports connected to clients or enable PortFast or Fast Link depending on the managed switch brand. Refer to the following table:

| **Switch Manufacturer** | **Fast Link Option Name** |
|-------------------------|---------------------------|
| Cisco                   | PortFast or STP Fast Link |
| Dell                    | Spanning Tree Fast Link   |
| Foundry                 | Fast Port                 |
| 3COM                    | Fast Port                 |

**Stream Service Isolation:** If security is of primary concern, Citrix recommends isolating or segmenting the PVS stream traffic from other production traffic.

**NIC teaming:** Teaming two NICs for throughput provides the server with the maximum bandwidth in turn increasing network performance helping to alleviate this potential network bottleneck.

#### Optimize Virtual Desktop images/Target devices

Virtual disk images play an important role in delivering successful vDisks to target devices. Before creating an image, it is important to clear unwanted applications and optimize as per the requirement.

**Citrix Optimizer:** By default, Microsoft Windows desktop images contain numerous features that aren’t necessary in a VDI environment. The Citrix Optimizer is a Windows tool developed by Citrix to help administrators optimize various components in their environment, most notably the operating system with the Virtual Delivery Agent (VDA). The tool is PowerShell-based, but also includes a graphical UI.

Citrix Optimizer provides various templates for optimization. Choose the right template for the operating system so that unnecessary services, configuration entries, and applications are disabled or removed. Admins can expect to realize fairly significant performance gains after optimization.

Reference: [Citrix blogs: Citrix Optimizer](https://www.citrix.com/blogs/2017/12/05/citrix-optimizer/)

**Preparing a vDisk image:** Virtual disk preparation is a key step in Citrix Virtual Apps and Desktops Service deployment. A few important steps need to be taken care when preparing the master image:

*  Remove unused files and features from the master image
*  Run Citrix Optimizer to improve performance and take care to select the correct OS
*  Test the connectivity between controllers and VMs
*  Run Optimization within the Imaging Wizard

Reference: [Citrix docs: Preparing a master target device for imaging](/en-us/provisioning/current-release/install/target-image-prepare.html)

**Citrix Provisioning Antivirus Best Practices:** Servers and targets may experience common issues if the antivirus is not properly tuned for the environment. It is recommended to limit antivirus definition updates to only the master target device. Avoid scanning the virtual disk write cache file and streaming disk I/O makes up the operating system for a given target.

Upgrading antivirus client software requires uninstalling PVS client software and reinstalling it. Check antivirus software specific instructions on configuring scanning exceptions. Obtaining a performance baseline may helps in the event of troubleshooting.

Reference: [Citrix article: CTX124185](https://support.citrix.com/article/CTX124185?_ga=2.53303340.521481839.1542115658-1035754598.1540193386)

#### Scalability

Scalability is an important factor while designing Citrix Virtual Apps and Desktops solutions. It is important to plan for the scalability of the constituent parts of the solution rather than just viewing it in generalized ways. Scalability observed here on the Delivery Controller, Citrix Provisioning Server and for the virtual machine infrastructure.

The number of target devices that is supported per PVS Server depends on the size of the virtual disk, storage solution for virtual disk placement, write cache type and work flow of end users. The most common bottlenecks that impact scalability of PVS Servers are network I/O of the PVS Server, disk I/O of the virtual disk storage location and cache file location. Organizations have to take care of these key factors according to their use cases and infrastructure.

Citrix recommends each organization performs scalability tests according to their environment based on infrastructure use cases. Adding extra PVS Servers to the existing infrastructure help in distributing the load and provide redundancy and high availability.

## Summary

Delivering virtual applications and desktops to end users has been a challenge for many IT administrators due to demands in end user experience and their work style to gain freedom of anywhere, anytime and any device access to resources.

This document demonstrates the imaging technology being used in Citrix Virtual Apps and Desktops. Image management encompasses core components to deal with meeting end user needs providing customized and optimized virtual desktops and application delivery.

A few important points to be considered:

*  Image management is not only just settings for the infrastructure but it is the basic building blocks for a solution design either in on-premises or cloud

*  Optimizing resource consumption and providing different deployment models in terms of scalability

*  Application virtualization using provisioning models gives flexibility to administrators and minimizes complexity

*  Ensure generic best practices are addressed to make use of resources efficiently

We have gone through a holistic view of both provisioning models (Citrix Machine Creation Services and Citrix Provisioning) from Citrix. Organizations have the option to use one of these or both provisioning models depending on the requirement.

## References

[Resources for Citrix Provisioning](/en-us/provisioning/)

[Resources for Citrix Virtual Apps and Desktops](/en-us/citrix-virtual-apps-desktops.html)

[Endpoint Security and Antivirus Best Practices](/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html)

[For Best Practices and Design hand book](/en-us/xenapp-and-xendesktop/7-15-ltsr/downloads/handbook-715-ltsr.pdf)
