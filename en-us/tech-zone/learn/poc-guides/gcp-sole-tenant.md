---
layout: doc
h3InToc: true
contributedBy: Matthew Harper
specialThanksTo: Josh Penick, Elaine Welch
description: Learn how to configure zone selection on Google Cloud Platform to enable sole tenancy in Citrix Virtual Apps and Desktop Service.
tz_title: Google Cloud Platform (GCP) Zone Selection Support with Citrix Virtual Apps and Desktops service
tz_products: google-cloud-platform;
---
# PoC Guide: Google Cloud Platform (GCP) Zone Selection Support with Citrix Virtual Apps and Desktops service

## Overview

The Citrix Virtual Apps and Desktops Service service (CVADS) now supports zone selection on Google Cloud Platform (GCP) to enable sole-tenant node functionality. You specify the zones where you want to create VMs in Citrix Studio. Then use Sole-tenant nodes allow to group your VMs together on the same hardware or separate your them from the other projects. Sole-tenant nodes also enable you to comply with network access control policy, security, and privacy requirements such as HIPAA.

This document covers:

-  Configuring a Google Cloud environment to support zone selection on the Google Cloud Platform in Citrix Virtual Apps and Desktops Service environments.

-  Provisioning Virtual Machines on Sole Tenant nodes.

-  Common error conditions and how to resolve them.

## Prerequisites

You must have existing knowledge of Google Cloud and the Citrix Virtual Apps and Desktops service for provisioning machine catalogs in a Google
Cloud Project.

To set up a GCP project for CVADS, follow the instructions [here](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html#configure-the-google-cloud-service-account).

## Google Cloud sole tenant

Sole tenancy provides exclusive access to a sole tenant node, which is a physical Compute Engine server that is dedicated to hosting only your project's VMs. Sole Tenant nodes allow you to group your VMs together on the same hardware or separate your VMs. These nodes can help you meet dedicated hardware requirements for [Bring Your Own License (BYOL)](https://cloud.google.com/compute/docs/instances/windows/bring-your-own-license) scenarios.

Sole Tenant nodes enable customers to comply with network access control policy, security, and privacy requirements such as HIPAA. Customers can create VMs in desired locations where Sole Tenant nodes are allocated. This functionality supports Win-10 based VDI deployments. A detailed description regarding Sole Tenant can be found on the [Google documentation site](https://cloud.google.com/compute/docs/nodes).

## Reserving a Google Cloud sole tenant node

To reserve a Sole Tenant Node, access the Google Cloud Console menu and select **Compute Engine** and then select **Sole-tenant-nodes**:

![Select tenant nodes](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_select-nodes.png)

### Sole-tenant nodes screen

Sole tenants in Google Cloud are captured in Node Groups. The first step in reserving a sole tenant platform is to create a node group. In the **GCP Console**, select **Create Node Group**:

![Sole tenant nodes](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_nodes.png)

### Creating a node group

Start by configuring the new node group. Citrix recommends that the **Region and Zone** selected for your new node group allows access to your domain controller and the subnets utilized for provisioning catalogs. Consider the following:

-  Fill in a name for the node group. In this example we used `mh-sole-tenant-node-group-1`.

-  Select a Region. For example, `us-east1`.

-  Select a Zone where the reserved system resides. For example, `us-east1-b`.

All node groups are associated with a *node template*, which is used to indicate the performance characteristics of the systems reserved in the node group. These characteristics include the number of virtual CPUs, the quantity of memory dedicated to the node, and the machine type used for machines created on the node.

Select the drop-down menu for the **Node template**. Then select **Create node template**:

![Select node group template](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_select-create-node-template.png)

### Create a node template

Enter a name for the new template. For example, `mh-sole-tenant-node-group-1-template-n1`.

The next step is to select a **Node Type**. In the drop-down menu, select the **Node type** most applicable to your needs.

  >**Note:**
  >
  >You can refer to this [Google documentation page](https://cloud.google.com/compute/docs/nodes) for more information on different node types.

Once you have chosen a node type click **Create**:

![Create node group template](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_create-node-template.png)

### Finish creating the node group

After creating the node template, the **Create node group** screen reappears. Click **Create**:

![Create node group](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_create-node-group.png)

## Creating the VDA master image

For the catalog creation process to deploy machines on the sole-tenant node extra steps must be performed when creating and preparing the machine image from the provisioned catalog.

Machine Instances in Google Cloud have a property called *Node affinity labels*. Instances that are used as master images for catalogs deployed to sole-tenant environments need to have a *Node affinity label* that matches the name of the **target node group**. There are two to apply the affinity label:

1.  Set the label in the Google Cloud Console when creating an Instance.

1.  Using the **gcloud** command line to set the label on instances that already exist.

An example of both approaches follows.

### Set the node affinity label at instance creation

This section does not cover all the steps necessary for creating a GCP Instance. It provides sufficient information and context for you to understand the process of setting the Node affinity label. Recall that in the examples above, the node group was named `mh-sole-tenant-node-grouop-1`. This is the node group we need to apply to the **Node affinity label** on the Instance.

#### New instance screen

The new instance screen appears. A section for managing settings related to management, security, disks, networking, and sole tenancy appears at the bottom of the screen.

To set a new Instance:

1.  Click the section once to open the **Management settings** panel.

1.  Then click **Sole tenancy** to see the related settings panel.

![Select management options](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_select-management-options.png)

#### Sole tenancy settings

The panel for setting the **Node affinity label** appears. Click **Browse** to see the available **Node Groups** in the currently selected Google Cloud project:

![Assign an affinity label](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_set-affinity-label.png)

#### Select node group screen

The Google Cloud Project used for these examples contains one node group, the one that was created in the earlier example.

To select the node group:

1.  Click the desired node group from the list.

1.  Then click **Select** on the bottom of the panel.

![Select node group](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_select-node-group.png)

#### Set the affinity label

After clicking **Select** in the previous step you are returned to the **Instance creation screen.** The **Node affinity labels** field contains the needed value to ensure catalogs created from this master image are deployed to the indicated node group:

![Select affinity label](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_assign-affinity-label.png)

### Set the node affinity label for an existing instance

To set the **Node affinity label** for an existing Instance, access the Google Cloud Shell and use the `gcloud compute instances` command.

More information about the *gcloud compute instances* command can be found on the Google Developer Tools [page](https://cloud.google.com/sdk/gcloud/reference/beta/compute/instances/set-scheduling).

Include three pieces of information with the `gcloud` command:

-  Name of the VM. This example uses an existing VM named `s2019-vda-base`.

-  Name of the Node group. The node group name, previously defined, is `mh-sole-tenant-node-grouop-1`.

-  The Zone where the Instance resides. In this example the VM resides in the `us-east-1b` zone.

#### How to open the Google Cloud shell

The buttons appearing in the following image are present at the top right of the **Google Cloud Console** window. Click the **Cloud Shell** button:

![Google Cloud Shell icon](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_open-google-cloud-shell-icon.png)

#### Fresh Cloud shell window

When the Cloud Shell first opens it looks similar to:

![Google Cloud Shell terminal window](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_google-cloud-shell.png)

#### Use the `gcloud` command to set the node affinity label

Run this command in the **Cloud Shell** window:

`gcloud compute instances set-scheduling "s2019-vda-base" --node-group="mh-sole-tenant-node-group-1" --zone="us-east1-b"`

#### Verify the instance affinity label setting

Finally, verify the details for the `s2019-vda-base` instance:

![VM instance details](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_vm-instance-details.png)

## Google shared VPCs

If you intend to use Google Sole-tenants with a Shared VPC, refer to the [GCP Shared VPC Support with Citrix Virtual Apps and Desktops](/en-us/tech-zone/learn/poc-guides/gcp-shared-vpc.html) document. Shared VPC support requires extra configuration steps related to Google Cloud permissions and service accounts.

## Create a machine catalog

After performing the previous steps in this document, you can create a machine catalog. Use the following steps to access Citrix Cloud and navigate to the Citrix Studio Console.

### Citrix Studio main page

In Citrix Studio, select **Machine Catalogs**:

![Select machine catalogs](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-machine-catalogs.png)

### Create the machine catalog

Select **Create Machine Catalog**:

![Create machine catalog](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-sole-tenant.png)

### Introduction screen

Click **Next** to being the configuration process:

![Studio Introduction screen](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-intro.png)

### Operating system

Select an operating system type for the machines in the catalog. Click **Next**:

![Select OS](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-os.png)

### Machine management

Accept the default setting that the catalog utilizes power managed machines. Then select MCS resources. In this example case we are using the Resources named `MyTestResources`. Click **Next**:

![Select machine management](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-machine-management.png)

>**Note:** These resources come from a previously created host connection, representing the network and other resources like the domain controller and reserved sole tenants. These elements are used when deploying the catalog. The process of creating the host connection is not covered in this document. More information can be found on the [Connections and resources page](/en-us/citrix-virtual-apps-desktops/manage-deployment/connections.html).

### Master image

The next step is to select the master image for the catalog. Recall that to utilize the reserved **Node Group** we must select an image that has the **Node affinity** value set accordingly. For this example we use the image from the previous example, `s2019-vda-base`.

Click **Next**:

![Select master image](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-master-image.png)

### Virtual machines

This screen indicates the number of virtual machines and the zones to which the machines are deployed. In this example we have specified three machines in the catalog. When using Sole-tenant node groups for machine catalogs it is imperative that you only select Zones containing reserved node Groups. In our example we have a single node group and it resides in Zone `us-east1-b`, so that is the only zone selected.

Click **Next**:

![Select VM](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-vm.png)

### Active Directory computer account

During the provisioning process MCS communicates with the domain controller to create host names for all the machines being created:

1.  Select the Domain into which the machines are created.

1.  Specify the **Account naming scheme** when generating the machine names.

Since the catalog in this example has three machines, and we have specified a naming scheme of `MySTVms-\#\#` the machines are named:

-  `MySTVms-01`

-  `MySTVms-02`

-  `MySTVms-03`

Click **Next**:

![Select computer account](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-computer-accounts.png)

### Domain credentials

Specify the credentials used to communicate with the domain controller, mentioned in the previous step:

1.  Select **Enter Credentials**.

1.  Supply the credentials, then click **Next**.

![Select domain credentials](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-select-domain-credentials.png)

### Summary

This screen displays a summary of key information during the catalog creation process. The final step is to enter a catalog name and an optional description. In this example the catalog name is *My Sole Tenant Catalog*.

Enter the catalog name and click **Finish**:

![Studio Summary screen](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-summary.png)

When the catalog creation process finishes, the Citrix Studio Console resembles:

![Studio Console showing machine catalogs](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-machine-catalogs.png)

### Verify in Google Cloud console

Use the Google Console to verify that the machines were created on the node group as expected:

![Verify machines](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_console-verify-machines.png)

## Migrating non-sole tenant catalogs

Currently migrating machine catalogs from Google Cloud general/shared space to sole tenant nodes is not possible.

## Commonly encountered issues and errors

Working with any complex system containing interdependencies results in unexpected situations. A few of the common issues and errors encountered when performing the setup and configuration to utilize CVAD and GCP Sole-tenants appears in this section.

### Catalog created successfully but machines are not provisioned to reserved node group

If you have successfully created a catalog the most likely reasons are:

-  The node affinity label was not set on the master image.

-  The node affinity label value does not match the name of the Node group.

-  Incorrect zones were selected in the **Virtual Machines** screen during the catalog creation process.

### Catalog creation fails with ‘Instance could not be scheduled due to absence of sole-tenant nodes in specified project and zone’

This situation presents itself with this error when **View details** is selected in the Citrix Studio dialog window:

`System.Reflection.TargetInvocationException: One or more errors occurred.
Citrix.MachineCreationAPI.MachineCreationException: One or more errors occurred.
System.AggregateException: One or more errors occurred.
Citrix.Provisioning.Gcp.Client.Exceptions.OperationException:
Instance could not be scheduled due to absence of sole-tenant nodes in
specified project and zone.`

One or more of the following are the likely cause of receiving this message:

-  You are attempting to provision a new catalog to a zone in which there is no reserved Sole Tenant Node. Ensure the zone selection is correct on the [Virtual Machines](#virtual-machines) screen.

-  You have a Sole Tenant Node reserved but the value of the VDA Master Node Affinity Label does not match the name of the reserved Node. Refer to these two sections; [Set Node Affinity Label at Instance Creation](#set-the-affinity-label) and [Setting the Node affinity label for an existing Instance](#set-the-node-affinity-label-for-an-existing-instance).

### Upgrading an existing catalog fails with ‘Instance could not be scheduled due to absence of sole-tenant nodes in specified project and zone’

There are two cases in which this occurs:

1.  You are upgrading an existing sole tenant catalog that has already been provisioned using Sole Tenancy and Zone Selection. The causes of this are the same as those found in the earlier entry [Catalog creation fails with ‘Instance could not be scheduled due to absence of sole-tenant nodes in specified project and zone’](#catalog-creation-fails-with-instance-could-not-be-scheduled-due-to-absence-of-sole-tenant-nodes-in-specified-project-and-zone).

1.  You are upgrading an existing **non-sole tenant catalog** and do not have a sole tenant node reserved in each zone already provisioned with machines for the catalog. This case is considered a *migration,* where the intent is to *migrate* machines from Google Cloud Common/Shared runtime space to a Sole Tenant Group. As noted in [Migrating Non-Sole Tenant Catalogs](#migrating-non-sole-tenant-catalogs), this is not possible.

### Unknown errors during catalog provisioning

If you encounter a dialog like this when creating the catalog:

![Image prep error](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-error-image-prep.png)

And selecting **View details** produces a screen resembling:

![Image prep error details](/en-us/tech-zone/learn/media/poc-guides_gcp-sole-tenant_studio-error-image-prep-details.png)

There are a few things you can check:

-  Ensure that the Machine Type specified in the Node Group Template matches the Machine Type for the master image Instance.

-  Ensure that the Machine Type for the master image has 2 or more CPUs.

## Test plan

This section contains some exercises you may want to consider trying to get a feel for CVAD support of Google Cloud Sole-tenants.

### Single tenant catalog

Reserve a group node in a single zone and provision both a persistent and a non-persistent catalog to that zone. During the steps below monitor the node group using the Google Cloud Console to ensure proper behavior:

1.  Power off the machines.

1.  Add machines.

1.  Power all machines on.

1.  Power all machines off.

1.  Delete some machines.

1.  Delete the machine catalog.

1.  Update the catalog

1.  Update the catalog from Non-Sole Tenant template to Sole Tenant Template

1.  Update the catalog from Sole Tenant template to Non-Sole Tenant Template

### Two zone catalog

Similar to the exercise above but reserve two Node Groups and provision a persistent catalog in one zone and a Non-Persistent catalog in another zone. During the steps below monitor the Node Group using the Google Cloud Console to ensure proper behavior:

1.  Power off the machines.

1.  Add machines.

1.  Power all machines on.

1.  Power all machines off.

1.  Delete some machines.

1.  Delete the machine catalog.

1.  Update the catalog.

1.  Update the catalog from non-sole tenant template to sole tenant template.

1.  Update the catalog from sole tenant template to non-sole tenant template.
