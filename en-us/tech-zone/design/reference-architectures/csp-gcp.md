---
layout: doc
h3InToc: true
contributedBy: JP Alfaro
specialThanksTo: Darren Harding, Selma Wei, Rick Dehlinger, Neir Benyamin
description: Citrix Virtual Apps and Desktops Service Google Cloud Platform (GCP) Architecture with the Managed Service for Microsoft Active Directory for Citrix Service Providers (CSPs) aligns with the use cases described in the CSP Citrix Virtual Apps and Desktops Reference Architecture to provide guidance and design considerations to leverage GCP Managed AD Service.
tz_title: Citrix Virtual Apps and Desktops Service - GCP Architecture with the Managed Service for Microsoft Active Directory for CSPs
tz_products: citrix-service-providers;
---
# Reference Architecture: Citrix Virtual Apps and Desktops Service - GCP Architecture with the Managed Service for Microsoft Active Directory for CSPs

## Introduction

The purpose of this document is to provide design and architectural guidance for Citrix Service Providers (CSPs) looking to use the Google Cloud Platform (GCP) as a Citrix Virtual Apps and Desktops Service resource location with the **Managed Service for Microsoft Active Directory**.

This document does not intend to provide step-by-step guidance on how to deploy the Citrix Virtual Apps and Desktops service for CSPs. It assumes understanding of the [CSP Virtual Apps and Desktops Reference Architecture](/en-us/tech-zone/design/reference-architectures/csp-cvads.html), which provides in-depth design and deployment considerations for a Citrix Virtual Apps and Desktops service environment for CSPs.

On the other hand, for detailed guidance on design considerations to architect the Citrix Virtual Apps and Desktops Service on GCP, refer to the [Citrix Virtualization on Google Cloud](/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html) reference architecture.

We start this document by reviewing the most common GCP elements you need to understand to comfortably utilize this document. Google has created their own ways to name and organize components in GCP, so understanding them is vital for a successful design and deployment.

Next, we review the details of the Managed AD Service, its similarities and differences with the traditional Microsoft Active Directory, and the deployment models you can use as a CSP to provision your GCP managed resource locations.

Finally, we cover the steps required to deploy a Managed AD Service domain in GCP.

## Terminology

While several services provide a similar functionality across the different public cloud providers, the terminology can be different. The following are the most common GCP elements that you need to understand as you follow through this reference architecture, as described on the GCP documentation.

*  **VPC Network:** GCP's virtual network object. Virtual Private Cloud networks (VPCs) in GCP are global, meaning you can deploy subnets to a VPC from each GCP region. You can deploy VPCs in auto-mode, which creates all subnets and CIDR ranges automatically, or in custom-mode, which lets you create subnets and CIDR ranges manually. Non-overlapping VPCs from different projects can be connected through VPC peering.
*  **VPC Peering:** A VPC peering allows you to connect VPCs which would otherwise be disconnected. In this case, the GCP Managed AD Service creates a VPC peering automatically to connect our VPC to the Managed AD Service VPC.
*  **Shared VPC:** A shared VPC can be spanned across multiple projects, eliminating the requirement to create separate VPCs for each project, or the utilization of VPC peering.
*  **GCP Organization:** An organization represents the root node in the GCP resource hierarchy. To create an organization, GCP Cloud Identity or Google Workspace (formerly G-Suite) are required. An organization is not required, but it is highly recommended to deploy one for your production environments to better organize and manage your resources.
*  **Folders:** A folder is utilized to organize resources within GCP, and they can contain more folders, or projects. For example, you can create folders to separate projects by department, environment type, or any other criteria. A folder is not always required, but same as with organizations, they are recommended for better resource organization.
*  **Project:** A project provides an abstract grouping of resources within GCP, and all resources in GCP must belong to a project. Under normal circumstances, VM instances from one project cannot communicate with VM instances in another project, unless a VPC peering or a Shared VPC are utilized.
*  **Billing Account:** A billing account represents the payment profile to be utilized to pay for GCP consumption. A billing account can be linked to multiple projects, but a project can only be linked to a single billing account.
*  **IAM:** GCP's Identity and Access Management platform is utilized to grant user permissions to perform actions on GCP resources. This platform is also utilized to deploy and manage Service Accounts.
*  **Service Account:** A service account is a GCP account that is not connected to an actual user, but instead represents a VM instance or an application. Service accounts can be granted permissions to perform different actions on the various GCP APIs. A service account is required to connect the Citrix Virtual Apps and Desktops Service to GCP and enable Machine Creation Services.
*  **GCE:** Google Compute Engine is the GCP platform in which you deploy compute resources, including VM instances, disks, instance templates, instance groups, and more.
*  **GCE Instance:** A GCE instance is any VM deployed on the GCE platform. Cloud Connectors, golden images, the AD management VM, and any other virtual machine in the environment are considered GCE instances.
*  **Instance Template:** A "baseline" resource you can utilize to deploy VMs and instance groups in GCP. The Citrix MCS process copies the golden image into an instance template, which is then utilized to deploy catalog machines.
*  **Cloud DNS:** The GCP service utilized to manage DNS zones and records. With the creation of the Managed AD Service, Cloud DNS is automatically configured to forward DNS queries to the managed domain controllers.

## Managed AD Service on GCP

GCP's [Managed Service for Microsoft Active Directory](https://cloud.google.com/managed-microsoft-ad) is a fully managed Active Directory service on the Google Cloud Platform. This service provides you with a fully functional Active Directory forest/domain without the overhead of building and maintaining Windows Server VM instances.

The Managed AD Service is built on highly available, Google-managed infrastructure, and delivered as a managed service. Each directory is deployed across multiple GCP zones and monitoring automatically detects and replaces domain controllers that fail. You do not have to install software, and Google handles all patching and software updates.

The Managed AD Service automatically deploys and manages highly available Active Directory domain controllers on an isolated GCP project and VPC network. A VPC peering is deployed automatically with the service for your AD-dependent workloads to reach Active Directory. Additionally, Google Cloud DNS is automatically configured to forward all DNS queries to the Managed AD Service.

### Managed AD Service Considerations

While there are many similarities between a traditional Microsoft Active Directory environment and the Managed AD Service in GCP, a few considerations must be kept in mind when deploying GCP's Managed AD Service.

1.  Domain controller access is restricted, and you can only manage your domain by deploying management instances and installing the Remote Server Administration tools.
2.  A shared VPC must be deployed before adding new customers / projects on the GCP shared resource location. Projects must belong to a shared VPC to be able to reach the managed AD domain. Resources deployed on a project with a VPC that is peered to a shared VPC are not able to reach the Managed AD Service domain. For more details, check the GCP [VPC peering requirements](https://cloud.google.com/vpc/docs/vpc-peering#restrictions) page and the [Google Cloud Platform (GCP) Shared VPC Support with Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/poc-guides/gcp-shared-vpc.html) tech-zone PoC guide.
3.  Domain Administrator / Enterprise Administrator account permissions are not available, these accounts are only used by GCP to manage the domain for you.
4.  AD objects cannot be created in any of the default containers (such as /Computers), they're read-only. This limitation brings up a common mistake when using Citrix's MCS provisioning technology, you must create the machine accounts for your MCS managed VDAs in a container/OU that's writeable. If you don't choose such a location, MCS is not be able to create the machine accounts.
5.  Some AD integrated features such as Certificate Services cannot be installed. As such, this limitation impacts CSPs who need to utilize Citrix's Federated Authentication Services (FAS) technology (which requires AD integrated Certificate Services). These customers must build and manage their own Active Directory on Google Cloud using Windows Server VM instances.
6.  Two main organizational units (OUs) are created by the service. The **"Cloud"** OU, which hosts all your managed AD resources. You have full control in this OU and any of its children. And the **"Cloud Services Object"** OU, which is used by GCP to manage the domain. Resources and the OU itself are read-only, except for some attributes being writable.
7.  The service automatically creates [several AD user groups](https://cloud.google.com/managed-microsoft-ad/docs/objects#groups) to allow for different AD administrative functions. You can manage the membership of these user groups.
8.  An account is created at service creation with a default name of **"setupadmin"**. This account is utilized to manage the domain. [Check this page](https://cloud.google.com/managed-microsoft-ad/docs/objects#delegated_administrator) for the full list of permissions for the **"setupadmin"** account.
9.  Trusts can be configured as one-way, outbound trusts to an on-premises Active Directory environment. With this configuration, the Managed AD Service domain is the *"trusting"* domain hosting the computer accounts, and the on-premises domain is the *"trusted"* domain hosting the user accounts. This model is commonly utilized with the Resource Forest deployment, which is explained in the following section.

### Managed AD Service Forest Design Considerations

In the context of a Citrix Service Provider, the Managed AD Service can be deployed under two different Active Directory forest design models.

The first and simpler way to deploy the Managed AD Service is by using the organizational forest design model. In this model, the GCP managed AD service hosts both the user accounts and resources (computer accounts), plus any administrative accounts.

Under normal circumstances, an organization forest allows for a trust to be configured to establish a relationship with another organizational forest. However, keep in mind that the GCP managed AD service only supports one-way outbound trusts.

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_001.png)

The second type of forest design model is the resource forest. In this model, a one-way outbound trust is configured to establish a relationship with an on-premises Active Directory environment.

As explained before, in this deployment model, the Managed AD Service is the *"trusting"* forest hosting the resources, and the on-premises AD is the *"trusted"* forest where user identities reside. In other words, the Managed AD Service domain allows for users in the on-premises domain to access its resources.

| NOTE:                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Keep in mind the [Citrix Cloud Connector Technical Details](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory) when designing your Active Directory forest models. Cloud Connectors cannot traverse forest trusts, user accounts from an on-premises Active Directory are not visible in Citrix Cloud unless a set of Cloud Connectors is deployed in that forest. |

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_002.png)

## Architecture

We understand that not all CSPs are at the same stage on their cloud adoption journey. For an in depth explanation of the various design patterns for Citrix on GCP, check [this section](/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html#design-patterns-for-citrix-virtualization-on-google-cloud) of the Citrix Virtualization on Google Cloud reference architecture.

Also, we're assuming full understanding of the Citrix Cloud multitenancy and customer management features available to CSPs. Those features are covered in depth on the [CSP Virtual Apps and Desktops Reference Architecture](/en-us/tech-zone/design/reference-architectures/csp-cvads.html).

### Managed Service for Microsoft Active Directory for CSPs Design Pattern

The Managed Service for Microsoft Active Directory for CSPs design pattern focuses on the combination of the different architecture models available to CSPs utilizing GCP managed resource locations, while using the Managed AD Service.

Partners deploying their managed DaaS offerings with Citrix Cloud can use the exclusive customer management and multitenancy features available to CSPs. These multitenancy features allow CSPs to deploy multiple customers on a shared Citrix Cloud control plane / tenant, or provide them with their dedicated control plane / tenant.

Citrix Cloud can be deployed with shared or dedicated resource locations on GCP. Different metrics can help a CSP determine which model better aligns to the specific requirements of each customer, and they can be based on end customer size, security and compliance requirements, cost savings, or more.

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_003.png)

While being an optional component, GCP Organizations (**1**) can be used to manage the hierarchy of the different projects and folders on the CSPs GCP subscription. Also, notice that the GCP subscription and resources utilized for a specific end customer can potentially belong to the end customer and not the CSP.

A shared VPC network (**2**) is deployed on a resource location where multiple customers share components like the Managed AD Service domain, golden images, and Citrix Cloud Connectors. Other customers can be hosted on dedicated VPCs and resource locations (**3**) under the same GCP organization. These customers have their own Managed AD Service domain, golden images, and Citrix Cloud Connectors.

The Managed AD Service can be deployed on the shared resource location (**4**) or in a dedicated resource location (**5**). This process creates a project (which cannot be accessed) and a network peering from your VPC to the VPC hosting the Managed AD Service.

As explained before, whenever you deploy new customers / projects on the shared resource location, they must belong to the shared VPC to be able to reach the Managed AD Service domain. Resources deployed on a project with a VPC that is peered to your shared VPC are not able to reach the Managed AD Service domain. This limitation has to do with VPCs not being transitive. The Managed AD Service domain on the shared resource location is different from the one on dedicated resource locations.

A separate GCP project is recommended to host each customer's resources (VDAs) on a shared resource location (**6**). This consideration allows for easier resource management and IAM permission application for the administrators in charge of supporting the different environments.

Also, per leading practices, the shared VPC host project will not host any resources (**7**). This project is only used to deploy the shared VPC and the Managed AD Service domain.

| NOTE:                                                                                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| While the Managed AD Service domain is deployed from the host project, the actual resources (domain controllers and VPC network) belong to a project managed by Google. You do not have access to this project. |

A shared Citrix Cloud tenant (**8**) is provisioned to deploy and manage the resources of multiple customers. These customers share the Citrix Virtual Apps and Desktop Service components (like Delivery Controllers, Databases, Director, Studio, Licensing, and APIs).

A dedicated [Citrix Workspace Experience](/en-us/citrix-workspace/experience.html) (**9**) is deployed for each customer. The dedicated Workspace Experience allows CSPs to brand the login page, along with customizing the access URL for each customer. Each customer uses the Citrix Gateway Service for authentication and HDX connections to their resources.

A dedicated Citrix Cloud tenant (**10**) can be provisioned for the bigger, most complex customers. This dedicated environment provides an isolated Citrix Virtual Apps and Desktop service, along with all of its components, and a dedicated Citrix Workspace Experience. There is no additional Citrix licensing costs to deploy a dedicated Citrix Cloud tenant

## Deploying the Managed Service for Microsoft Active Directory

In this section, we cover the steps required to deploy the Managed AD Service domain. This section assumes that a GCP subscription is available, and resources such as projects, VPC networks, firewall configurations, and other GCP components have already been deployed.

1- On the navigation menu, go to **IDENTITY & SECURITY > Identity > Managed Microsoft AD**.

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_004.png)

2- On the **Managed Service for Microsoft Active Directory** screen, click **CREATE NEW DOMAIN**.

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_005.png)

| NOTES:                                                                  |
| ----------------------------------------------------------------------- |
| *  Managed domain controllers are deployed with the ADDS and DNS roles. |
| *  Management VMs must be created separately.                           |

3- On the **Create a new domain** screen, enter the following information:

*  **Fully qualified domain name**: domain FQDN, for example, customer.com
*  **NetBIOS**: this is automatically populated
*  **Select networks**: networks that will have access to the service,
*  **CIDR Range**: a /24 CIDR range for the VPC where the domain controllers are be deployed

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_006.png)

| NOTES:                                                                                     |
| ------------------------------------------------------------------------------------------ |
| *  The VPC that is deployed as part of the service cannot be managed from the GCP console. |
| *  CIDR range must not overlap with your current subnets.                                  |

4- Scroll down and enter the following information:

*  **Region**: GCP regions in which to deploy the Managed AD service domain
*  **Delegated Admin**: name of the delegated administrator account
*  Click **CREATE DOMAIN**

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_007.png)

| NOTES:                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *  The delegated administrator account resides on the Users container. While you can reset its password directly in the ADUC console, you cannot move the object to a different OU. |
| *  When joining a computer to the domain, its AD account is created under the **Cloud > Computers** OU, not the default Computers container.                                        |
| *  Service creation can take up to 60 minutes.                                                                                                                                      |

5- Once creation is finalized, select your domain and click **SET PASSWORD**.

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_008.png)

6- On the **Set password** window, click **CONFIRM**.

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_009.png)

7- On the **New password** window, copy the password and click **DONE**.

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-gcp_010.png)

Once the service has been created and is ready for use, you can start deploying other instances and join them to the domain. You can also complete your Citrix Virtual Apps and Desktops Service site configuration.
