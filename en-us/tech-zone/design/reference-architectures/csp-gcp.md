---
layout: doc
h3InToc: true
contributedBy: JP Alfaro
specialThanksTo: Darren Harding, Selma Wei, Rick Dehlinger
description: Citrix Virtual Apps and Desktops Service GCP Architecture with the Managed Service for Active Directory for CSPs aligns with the use cases described in the CSP Citrix Virtual Apps and Desktops Reference Architecture to provide guidance and design considerations to leverage GCP's Managed AD Service.
---

# Citrix Virtual Apps and Desktops Service – GCP Architecture with the Managed Service for Active Directory for CSPs

## Introduction

The purpose of this document is to provide design and architectural guidance for Citrix Service Providers (CSPs) looking to successfully leverage Google Cloud Platform (GCP) in combination with the Managed Service for Microsoft Active Directory as a Citrix Cloud Virtual Apps and Desktops Service (CVADs) resource location.

This document does not intend to provide step-by-step guidance on how deploy the Citrix Virtual Apps and Desktops service for CSPs, and it assumes understanding of the [CSP Virtual Apps and Desktops Reference Architecture](/en-us/tech-zone/design/reference-architectures/csp-cvads.html), which provides in-depth guidance on design descisions and deployment considerations for a Citrix Virtual Apps and Desktops service environment for CSPs.

On the other hand, for detailed guidance on design considerations to architect the CVAD service on GCP, please refer to the [Citrix Virtualization on Google Cloud](/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html) reference architecture.

We will start by reviewing the most common GCP elements you need to understand to comfortably work with this document. Google has created their own ways to name and organize components in GCP, so understanding them is vital for a successful design and deployment.

Next, we will review the details of the Managed Service for Microsoft Active Directory, its similarities and differences with the traditional Microsoft Active Directory, and the different deployment models you can leverage as a CSP to provision your GCP managed resources locations.

Finally, we will cover the steps required to deploy your Managed Service for Microsoft Active Directory domain.

## Terminology

While several services will provide similar functionality across the different public cloud providers, the terminology can be very different. The following are the most common GCP elements that you will need to understand as you work with this reference architecture, as described on the GCP documentation.

*  **VPC Network:** GCP’s virtual network object. VPC in GCP are global, meaning you can deploy subnets to a VPC from each GCP region. You can deploy VPCs in auto-mode, which creates all subnets and CIDR ranges automatically, or custom-mode, which let you create subnets and CIDR ranges manually. Non-overlapping VPCs from different projects can be connected through VPC peering.
*  **VPC Peering:** A VPC peering allows you to connect VPCs which would otherwise be disconnected. For the purpose of this implementation, the GCP Managed Microsoft AD service will create a VPC peering automatically to connect our VPC to the managed AD service VPC.
*  **Shared VPC:** A shared VPC can be spanned across multiple projects, eliminating the requirement to create separate VPCs for each project, or the utilization of VPC peering.
*  **GCP Organzation:** An organizations represents the root node in the GCP resource hierarchy. In order to create an organization, GCP Cloud Identity or Google Workspace (formerly G-Suite) will be required. An organization is not required for this architecture, but it is highly recommended to deploy one for your production environments in order to properly organize and manage your resources.
*  **Folders:** A folder is utilized to organize resources within GCP, they can contain more folders, or projects. For example, you can create folders to separate projects by department, environment type, etc. A folder is not required for this implementation, but in general terms, they are recommended for proper resource organization.
*  **Project:** A project provides an abstract grouping of resources within GCP, all resources in this deployment need to belong to a GCP project. Under normal circumstances, VM instances from one project cannot communicate with VM instances in another project, unless VPC peering or Shared VPC are utilized. 
*  **Billing Account:** Billing accounts represent the payment profile to be utilized to pay for GCP consumption. A billing account can be linked to multiple projects, but a project can only be linked to a single billing account. 
*  **IAM:** GCP’s Identity and Access Management platform. It is utilized to grant user permissions to perform actions on GCP resources. This platform is also utilized to deploy and manage Service Accounts. We will utilize IAM to configure various permissions.
*  **Service Account:** A service account is a GCP account that is not connected to an actual user, but instead represents a VM instance or an application. Service accounts can be granted permissions to perform different actions on the various GCP APIs. A service account will be created to connect Citrix Cloud to GCP and enable Machine Creation Services.
*  **GCE:** Google Compute Engine is the GCP platform in which you deploy compute resources, including VM instances, disks, instance templates, instance groups, etc.
*  **GCE Instance:** A GCE instance is any VM deployed on the GCE platform. You will deploy GCE instances for Cloud Connectors, master image, the AD management VM, etc. All machines created through a Citrix machine catalog are deployed as GCE instances.
*  **Instance Template:** A “baseline” resource you can utilize to deploy VMs and instance groups in GCP. The Citrix MCS process will copy the master image into an instance template, which is utilized to deploy catalog machines.
*  **Cloud DNS:** GCP service utilized to manage DNS zones and records. With the creation of the Managed Microsoft AD service, Cloud DNS will be automatically configured to forward DNS queries to the managed domain controllers.

## Managed AD Service on GCP

GCP’s [Managed Service for Microsoft Active Directory](https://cloud.google.com/managed-microsoft-ad) is a fully managed Active Directory service on the Google Cloud Platform. This service provides you with a fully functional Active Directory forest/domain without the overhead of building and maintaining Windows Server VM instances.

The Managed Microsoft AD service is built on highly available, Google-managed infrastructure, and delivered as a managed service. Each directory is deployed across multiple GCP zones and monitoring automatically detects and replaces domain controllers that fail. You do not have to install software, and Google handles all patching and software updates.

The Managed Service for Microsoft Active Directory automatically deploys and manages the highly available Active Directory domain controllers on an isolated GCP project and VPC network, a VPC peering is deployed automatically with the service for your AD-dependent workloads to reach Active Directory; additionally, Google Cloud DNS is automatically configured to forward all DNS queries to the Managed Microsoft AD.

### Managed AD Service Considerations

While there are many similarities between a traditional Microsoft Active Directory environment and the Managed Service for Microsoft Active Directory in GCP, and resources in the domain are fundamentally managed via the MMC console, a few considerations must be kept when deploying GCP’s managed AD.

1.  Domain controller access is restricted, and you can only manage your domain by deploying management instances and installing the Remote Server Administration tools.
2.  A shared VPC must be deployed before adding new customers / projects on the GCP shared resource location. Projects must belong to a shared VPC in order to be able to reach the managed AD domain; resources deployed on a project with a VPC that is peered to a shared VPC will not be able to reach the managed AD domain. For more details, check the GCP [VPC peering requirements](https://cloud.google.com/vpc/docs/vpc-peering#restrictions) page and the [Google Cloud Platform (GCP) Shared VPC Support with Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/poc-guides/gcp-shared-vpc.html) tech-zone PoC guide.
3.  Domain Administrator / Enterprise Administrator account permissions are not available, these are only used by GCP to manage the domain for you.
4.  AD objects cannot be created in any of the default containers (such as /Computers): they're essentially read-only. This brings up a common mistake some customers make when using Citrix's MCS provisioning technology: you must choose to create the machine accounts for your MCS managed VDAs in a container/OU that's writeable - if you don't choose such a location, MCS won't be able to create the machine accounts.
5.  Some AD integrated features such as Certificate Services cannot be installed. As such, this impacts customers who will be using Citrix's Federated Authentication Services ("FAS") technology (which requires AD integrated Certificate Services): these customers must build and manage their own Active Directory on Google Cloud using Windows Server VM instances.
6.  Two main organizational units (OUs) will be created by the service. The **"Cloud"** OU, which will host all your managed AD resources. You will automatically have full control in this OU and any of its children. And the **"Cloud Services Object"** OU, which will be used by GCP to manage the domain. Resources and the OU itself will be read-only, with the exception of some attributes being writable.
7.  The service will automatically create [several AD groups](https://cloud.google.com/managed-microsoft-ad/docs/objects#groups) to allow for different AD administrative functions. You can manage the membership of this groups.
8.  An account will be created at service creation with a default name of **setupadmin**. This account will be utilized to manage the domain. [Check this page](https://cloud.google.com/managed-microsoft-ad/docs/objects#delegated_administrator) for the full list of permissions for the setupadmin account.
9.  Trusts can be configured as one-way, outbound trusts to an on-premises Active Directory environment. Under this configuration, the Managed Service for Active Directory domain will be the trusting domain hosting the computer accounts, and the on-premises domain will be the trusted domain hosting the user accounts. This will be commonly utilized with the Resource Forest deployment model, which will be explained on the following section.

### Managed AD Service Forest Design Considerations

In the context of a Citrix Service Provider, the Managed Service for Active Directory can be deployed under two different Active Directory forest design models.

The first and simpler way to deploy the Managed Service for Active Directory is by leveraging the organizational forest design model. In this model, the GCP managed AD service will host both the user accounts and resources (computer accounts), plus any administrative accounts.

Under normal circumstances, an organization forest allows for a trust to be configured to establish a relationship with another organizational forest; however, keep in mind that the GCP managed AD service only supports one-way outbound trusts.

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_001.png)

The second type of forest design model is the resource forest. In this model. the GCP managed AD service will host the resources (computer accounts), a one-way outbound trust will be configured to establish a relationship with an on-premises Active Directory environment, which will host the user accounts.

As explained before, in this deployment model, the GCP managed AD service will be the “trusting” forest where the resources reside, and the on-premises AD will be the “trusted” forest. In other words, the GCP managed AD domain will allow for users in the on-premises domain to access its resources.

| NOTE: |
|---|
| Keep in mind the [Citrix Cloud Connector Technical Details](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory) when designing your Active Directory forest models. Because Cloud Connectors cannot traverse forest-level trusts, user accounts from the on-premises Active Directory would not be visible in Citrix Cloud unless a set of Cloud Connectors is deployed in that forest. |

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_002.png)

## Architecture

We understand that different CSPs will be at a different stage on their cloud adoption journey. For a full, in depth explanation of the various design patterns for Citrix on GCP, please check the [Design Patterns for Citrix virtualization on Google Cloud](/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html#design-patterns-for-citrix-virtualization-on-google-cloud) section of the Citrix Virtualization on Google Cloud reference architecture.

Additionally, we’re also assuming that you understand the Citrix Cloud multi-tenancy and customer management features available to CSPs, those are covered in depth on the [CSP Virtual Apps and Desktops Reference Architecture](/en-us/tech-zone/design/reference-architectures/csp-cvads.html).

### Managed Service for Active Directory for CSPs Design Pattern

The Managed Service for Active Directory for CSPs design pattern focuses on the combination of the different architecture models available to Citrix Service providers in conjunction with GCP managed resource locations, while leveraging the Managed Service for Active Directory service.

Partners deploying their managed DaaS offerings with Citrix Cloud can leverage the exclusive customer management and multi-tenancy features available to CSPs. These multi-tenancy features allow CSPs to locate different customers on a shared Citrix Cloud control plane / tenant or provide them with their dedicated control plane / tenant. 

Citrix Cloud can be deployed in conjunction with shared or dedicated resource locations on GCP; different metrics can help a CSP determine which model better aligns to the specific requirements of each customer, and they can be based on end customer size, security and compliance requirements, or more.

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_003.png)

While an optional component, GCP Organizations (**1**) can be leveraged to manage the hierarchy of the different projects and folders on the CSPs GCP subscription, also do notice that the GCP subscription and resources utilized for a specific end customer can potentially belong to the end customer and not the CSP.

A shared VPC network (**2**) will be deployed on a resource location where multiple customers will share the different components like the Managed Service for Active Directory domain, master images, and Citrix Cloud connectors. Other customers can be hosted on dedicated VPCs and resource locations (**3**) under the same GCP organization, these customers will have their own Managed Service for Active Directory domain, master images, and Citrix Cloud connectors.

The Managed Service for Active Directory can be deployed on the shared resource location (**4**) or in any other dedicated resource location (**5**), this will create a project (which you cannot access) and a peering from your VPC to the VPC where the Managed Service for Active Directory belongs.

As explained before, whenever you deploy new customers / projects on the shared resource location, they must belong to the shared VPC in order to be able to reach the managed domain; resources deployed on a project with a VPC that is peered to your shared VPC will not be able to reach the managed domain, this is a limitation with GCP VPCs not being transitive. The Managed Service for Active Directory domain on the shared resource location is different from the one on dedicated resource locations.

Customer resources (VDAs) on a shared resource location should be deployed on a separate GCP project for each customer (**6**), this will allow for easier resource management and IAM permission application for the administrators in charge of supporting the different environments.

Additionally, no resources should be deployed on the host project (**7**), which will only be used to deploy the shared VPC and the Managed Service for Active Directory domain.

| NOTE: |
|---|
| While the Managed Service for Active Directory domain will be deployed and managed from the host project, the actual resources (domain controller VMs and the VPC network they belong to) will belong to a GCP project managed by Google. You do not have access to this project. |

A shared Citrix Cloud tenant (**8**) will be provisioned to deploy and manage the resources from multiple customers. These customers will share the Citrix Virtual Apps and Desktop Service components, like the Delivery Controllers, Databases, Director, Studio, Licensing, and APIs.

A dedicated [Citrix Workspace Experience](/en-us/citrix-workspace/experience.html) (**9**) will be deployed for each customer, this will allow for the CSP to customize the look of the workspace, along with a dedicated access URL for each customer. Each customer will leverage their own Citrix Gateway Service for authentication and HDX connections to their resources. 

A dedicated Citrix Cloud tenant (**10**) can be provisioned for the bigger, most complex customers. This dedicated environment will provide a completely isolated Citrix Virtual Apps and Desktop service, along with all of its components, and a dedicated Citrix Workspace Experience.

## Deploying the Managed Service for Microsoft Active Directory

In this section, we will cover the steps required to deploy the Managed Service for Microsoft Active Directory domain. This section assumes that a GCP subscription is available, and resources such as projects, VPC networks, firewall configurations, and other GCP components have already been deployed.

1- On the navigation menu, go to **IDENTITY & SECURITY > Identity > Managed Microsoft AD**.

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_004.png)

2- On the **Managed Service for Active Directory** screen, click **CREATE NEW DOMAIN**.

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_005.png)

| NOTES: |
|---|
|*  Managed domain controllers will be deployed with the ADDS and DNS roles. |
|*  Management VMs need to be created separately. |

3- On the **Create a new domain** screen, enter the following information:

*  **Fully qualified domain name**: domain FQDN, for example, customer.com
*  **NetBIOS**: will be automatically populated
*  **Select networks**: networks that will have access to the service,
*  **CIDR Range**: a /24 CIDR range for the VPC where the domain controllers will be deployed

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_006.png)

| NOTES: |
|---|
|*  The VPC that is deployed as part of the service cannot be managed from the GCP console. |
|*  CIDR range must not overlap with your current subnets. |

4- Scroll down and enter the following information:

*  **Region**: GCP regions in which the managed AD service will be available
*  **Delegated Admin**: name of the delegated administrator account
*  Click **CREATE DOMAIN**

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_007.png)

| NOTES: |
|---|
|*  The delegated administrator account resides on the Users container within AD, while you can reset its password directly in the ADUC console, you cannot move the object to a different OU. |
|*  When joining a computer to the domain, its AD account will be created under the Cloud > Computers OU, not the default Computers container. |
|*  Service creation can take up to 60 minutes. |

5- Once creation is finalized, select your domain and click on **SET PASSWORD**.

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_008.png)

6- On the **Set password** window, click **CONFIRM**.

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_009.png)

7- On the **New password** window, copy the password and click **DONE**.

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_010.png)

Once the service has been created and is ready for use, you can start deploying additional instances, join them to the domain, and complete your CVAD service site configuration.
