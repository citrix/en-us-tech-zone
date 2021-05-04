---
layout: doc
h3InToc: true
contributedBy: Matthew Harper
specialThanksTo: Josh Penick, Elaine Welch
description: Learn to deploy provisioned Windows 10 catalogs to GCP Sole Tenant nodes in Citrix Virtual Apps and Desktop Service.

tz_title: Google Cloud Platform (GCP) Windows 10 Sole Tenant with Optional Shared VPC Catalog Creation
tz_products: google-cloud-platform;
---
# PoC Guide: Google Cloud Platform (GCP) Windows 10 Sole Tenant with Optional Shared VPC Catalog Creation

## Overview

Support for Google Shared VPCs and Zone Selection by Citrix Virtual Apps and Desktops Service is available as an early access release (EAR). Citrix offers this support in response to customer requests for a way to provision a Windows 10-based catalog on Google Cloud using virtual networks shared across the enterprise. This document describes the steps required to create an MCS Machine Catalog by using a Windows 10 VDA, Google Cloud Shared VPC, and Google Cloud Sole Tenant Nodes.

## Prerequisites

-  Citrix Virtual Apps and Desktops Service and Google Cloud. For details, see the [product documentation](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html#configure-the-google-cloud-service-account).

-  GCP Zone Selection Support with Citrix Virtual Apps and Desktops.

-  GCP Windows 10 VDA with Citrix Virtual Apps and Desktops.

The following prerequisite is for users who want to use a Shared VPC in addition to using Sole Tenancy.

-  GCP Shared VPC Support with Citrix Virtual Apps and Desktops.

Once you meet all prerequisites, you must set up and configure the following environment and technical items:

-  Google Cloud Service Project with permissions to use the Shared VPC

-  Sole Tenant Node Group Reservation that resides in the Service Project

-  Windows 10 VDA

-  (Optional) Google Cloud Host Project with a Shared VPC and required firewall rules

## Example environment

Creating the desired Windows 10-based MCS Machine Catalog in Google Cloud is similar to creating other catalogs. You can do more sophisticated work after you complete the full prerequisites as described in the preceding section. Then, you select the proper VDA and network resources.

For this example, the following elements are in place:

### Host Connection

The Host Connection in this example uses Google Cloud Shared VPC resources. This is **not** mandatory when using Zone Selection, a standard Local VPC-based Host Connection, can be used.

| Connection Name | Shared VPC Resources Connection |
|----------------------------|-----------------------------|
| Resources                  | SharedVPCSubnet            |
| Virtual Shared VPC Network | `gcp-test-vpc`               |
| Shared VPC Subnet          | `subnet-good`                |

### Sole Tenant Node Reservation

A Sole Tenant Node Group named *mh-windows10-node-group* located in Zone
*us-east1-b*.

![Sole Tenant Node Group](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_001.png)

### Windows 10 VDA Image

A Windows 10-based VDA that resides in a local project named
*‘windows10-1909-vda-base’,* also in zone *us-east1-b*.

![Windows 10 based VDA](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_002.png)

## Catalog Creation

The following steps cover creation of the Windows 10-based Machine Catalog that uses a Google Cloud Shared VPC and Zone Selection. The final steps describe how to validate that the resulting machines are using the desired resources.

1.  Start with Citrix Studio.

    ![Citrix Studio](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_003.png)

1.  Select **Machine Catalogs**.

1.  The **Machine Catalogs** screen opens. One Multi-session (Windows Server-based) catalogs named **Shared VPC Catalog** already exists.

    ![Machine Catalogs screen](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_004.png)

1.  Click **Create Machine Catalog**.

    The standard **Catalog Creation Introduction** screen may appear.

    ![Introduction screen](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_005.png)

1.  Click **Next**.

    On the screen that appears, you specify the type of operating system the catalog will be based upon:

    -  **Multi-Session OS**, which indicates a Windows Server-based catalog

    -  **Single-Session OS**, which indicates a Windows Client-based catalog

    -  **Remote PC Access**, which indicates a catalog that includes physical machines

    ![Remote PC Access](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_006.png)

    This will be a Windows 10-based catalog, in which a **Single-Session OS** is used.

1.  Select **Single-Session OS** and then click **Next**.

    The next screen is used to indicate if the machines are power managed. The machines are power managed in this example. The screen also indicates the technology used to deploy the machines. Because MCS is being used, you must indicate the network resources to be used when deploying the machines. Note that in the following case, the Shared VPC
*SharedVPCSubnet* noted in [Example Environment](#example-environment) has been selected for the resources to be used.

1.  Select the resources associated with your Shared VPC on the following screen and then click **Next**.

    ![Select resources](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_007.png)

    Consider if users connect to a random desktop each time they log in or the same (static) desktop. Here we choose the **Random desktop** type. This option means that all changes that users make to the machine are discarded.

    ![Random desktop type](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_008.png)

1.  Click **Next**.

1.  Select the image to be used as the base disk in the catalog. Here, we select **windows10-1909-vda-base** as noted in the [Example
Environment](#example-environment).

    ![Selecting image for base disk](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_009.png)

1.  Click **Next**

    The **Virtual Machine** is another critical screen. Zone Selection is what enabled MCS to use the reserved Sole Tenant Node for placement of the provisioned Windows 10 virtual machine. The [Example Environment](#example-environment) section noted that both the Sole Tenant Node resides in Zone *us-east1-b*. Because we have a single Sole Tenant Node reserved, this is the only zone that should be selected. To distribute your machines across zones, reserve a Sole Tenant in each zone to be used.

    ![Reserving a Sole Tenant in each zone](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_010.png)

1.  Click **Next**

    The key thing to ensure on the **Active Directory Computer Accounts** screen is that the AD Domain you select is the correct domain for provisioning machines in the Shared VPC network.

    ![Selecting the correct AD domain](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_011.png)

1.  Select **The desired AD Domain**, enter **Account naming scheme** and then click **Next**.

    On the **Domain Credentials** screen, enter credentials with sufficient privileges to create and delete computer accounts in the domain.

    ![Enter credentials](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_012.png)

1.  Enter **Credentials** and then click **Next**.

    The **Catalog Summary and Name** screen shows a summary of the catalog to be created. You can also provide a name for the catalog. In this case, the catalog name is **Windows 10 Shared VPC and Sole Tenant**.

    ![Summary](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_013.png)

1.  Click **Finish**

It may take a few minutes for the catalog creation to complete. Then, you can view machines in the catalog through the **Search** node on the tree.

You can see that the expected three machines have been created using the expected Connection.

![Three machines created](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_014.png)

  >**Note:**
  >
  >Google Cloud starts Instances as part of the creation process. As a result, newly provisioned machines are initially **Power On**, as shown above.

## Validate Resource Utilization

To validate resource utilization and ensure that the newly provisioned machines are using the expected resources, check the following:

-  Are the machines running on the reserved Sole Tenant Node?

-  Are the machines on the desired Shared VPC subnet?

    Remember that use of a Shared VPC is optional, so this validation step may not be applicable to your configuration.

## Machines Running on Sole Tenant Node

The following figure shows that the three newly provisioned machines are running on the reserved Sole Tenant Node.

![Three newly provisioned machines](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_015.png)

## Instance Details

The details for the first Instance confirm the following:

-  The proper **Node Affinity Label** tag is in place.

-  The correct network `gcp-test-vpc` is being used.

-  The correct subnet `subnet-good` is being used.

![Instance details](/en-us/tech-zone/learn/media/poc-guides_gcp-win10-catalog-creation_016.png)
