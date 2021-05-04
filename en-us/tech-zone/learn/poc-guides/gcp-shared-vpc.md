---
layout: doc
h3InToc: true
contributedBy: Matthew Harper
specialThanksTo: Josh Penick, Elaine Welch
description: Learn how to use Machine Creation Services (MCS) to provision machines in a Shared VPC on Google Cloud Platform (GCP). Then, learn to manage the machines using Citrix Studio.

tz_title: Google Cloud Platform (GCP) Shared VPC Support with Citrix Virtual Apps and Desktops
tz_products: google-cloud-platform;
---
# PoC Guide: Google Cloud Platform (GCP) Shared VPC Support with Citrix Virtual Apps and Desktops

## Overview

The Citrix Virtual Apps and Desktops Service supports Google Cloud Platform (GCP) Shared VPC. This document covers:

-  An overview of Citrix support for Google Cloud Shared VPCs.

-  An overview of terminology related to Google Cloud Shared VPCs.

-  Configuring a Google Cloud environment to support use of Shared VPCs.

-  Use of Google Shared VPCs for host connections and machine catalog provisioning.

-  Common error conditions and how to resolve them.

## Prerequisites

This document assumes knowledge of Google Cloud and the use of Citrix Virtual Apps and Desktops Service for provisioning machine catalogs in a Google Cloud Project.

To set up a GCP project for Citrix Virtual Apps and Desktops Service, see the [product documentation](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html#configure-the-google-cloud-service-account).

## Summary

Citrix MCS support for provisioning and managing machine catalogs deployed to Shared VPCs is functionally equivalent to what is supported in Local VPCs today.

There are two ways in which they differ:

-  A few more permissions must be granted to the Service Account used to create the Host Connection to allow MCS to access and use the Shared VPC Resources.

-  The site administrator must create two firewall rules, one each for ingress and egress, to be used during the image mastering process.

Both of these will be discussed in greater detail later in this document.

## Google Cloud Shared VPCs

GCP Shared VPCs are comprised of a Host Project, from which the shared subnets are made available, and one or more service projects that utilize the resources.

Use of Shared VPCs is a good option for larger installations because they provide for more centralized control, usage, and administration of shared corporate Google cloud resources. Google Cloud describes it this way:

"Shared VPC allows an [organization](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy) to connect resources from multiple projects to a common [Virtual Private Cloud (VPC) network](https://cloud.google.com/vpc/docs/vpc), so that they can communicate with each other securely and efficiently using internal IPs from that network. When you use Shared VPC, you designate a project as a host project and attach one or more other service projects to it. The VPC networks in the host project are called Shared VPC networks. [Eligible resources](https://cloud.google.com/vpc/docs/shared-vpc#resources_that_can_be_attached_to_shared_vpc_networks_from_a_service_project) from service projects can use subnets in the Shared VPC network."

The above paragraph was taken from the [Google Documentation site](https://cloud.google.com/vpc/docs/shared-vpc).

## New Permissions Required

When working with Citrix Virtual Apps and Desktops and Google Cloud, a GCP Service Account with specific permissions must be provided when creating the Host Connection. As noted above, to utilize GCP Shared VPCs some additional permissions must be granted to any service accounts used to create Shared VPC based Host Connections.

Technically speaking, the permissions required are not “new”, since they are already necessary to utilize Citrix Virtual Apps and Desktops with GCP and Local VPCs. The change is that the permissions must be granted so as to allow access to the Shared VPC resources. This is accomplished by adding the Service Account to the IAM Roles for the Host Project and will be covered in detail in the “How To” section of this document.

  >**Note:**
  >
  >To review the permissions required for the currently shipping Citrix Virtual Apps and Desktops product, see the Citrix documentation site [describing resource locations](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html).

In total, a maximum of four additional permissions must be granted to the Service Account associated with the Host Connection:

1.  compute.firewalls.list - Mandatory

    This permission is necessary to allow Citrix MCS to retrieve the list of firewalls rules present on the Shared VPC (discussed in detail below).

1.  compute.networks.list - Mandatory

    This permission is necessary to allow Citrix MCS to identify the Shared VPC networks available to the Service Account.

1.  compute.subnetworks.list – *May be* Mandatory (see below)

    This permission is necessary to allow MCS to identify the subnets within the visible Shared VPCs.

    > **Note:**
    >
    > This permission is already required for using Local VPCs but must also be assigned in the Shared VPC Host project.

1.  compute.subnetworks.use - *May be* Mandatory (see below)

    This permission is necessary to utilize the subnet resources in the provisioned machine catalogs.

    >**Note:**
    >
    >This permission is already required for using Local VPCs but must also be assigned in the Shared VPC Host Project.

The last two items are noted as “*May be* Mandatory” because there are two different approaches to be considered when dealing with these permissions:

-  **Project-level** permissions

    -  Allows access to all Shared VPCs within the host project.

    -  Requires that the permissions #3 and #4 must be assigned to the Service Account.

-  **Subnet-level** permissions

    -  Allow access to specific subnets within the Shared VPC.

    -  Permissions #3 and #4 are intrinsic to the subnet level assignment and therefore do not need to be assigned directly to the Service Account.

Examples of both approaches are provided below in the “How To” section of this document.

Either approach will work equally well. Select the model more in tune with your organizational needs and security standards. More detailed information regarding the difference between Project-Level and Subnet-level permissions can be obtained from the [Google Cloud documentation](https://cloud.google.com/vpc/docs/shared-vpc#svc_proj_admins).

## Host Project

To use Shared VPCs in Google Cloud, first designate and enable one Google Cloud Project to be the Host Project. This Host Project contains one or more Shared VPC Networks utilized by other Google Cloud Projects within the organization.

Configuring the Shared VPC Host Project, creating subnets, and sharing either the entire project or specific subnets with other Google Cloud Projects are purely Google Cloud related activities and not included within the scope of this document. The Google Cloud documentation related to creating and working with
Shared VPCs can be found [here](https://cloud.google.com/vpc/docs/provisioning-shared-vpc#enable-shared-vpc-host).

## Firewall Rules

A key step in behind-the-scenes processing that occurs when provisioning or updating a machine catalog is called *mastering*. This is when the selected machine image is copied and prepared to be the master image system disk for the catalog. During mastering, this disk is attached to a temporary virtual machine, the *prep* machine, and started up to allow preparation scripts to run. This virtual machine needs to run in an isolated environment that prevents **all**
inbound and outbound network traffic. This is accomplished through a pair of **Deny-All** firewall rules; one for ingress and one for egress.

When using GCP Local VPCs, MCS creates this firewall rule pair on the fly in the local network, applies them to the machine for mastering, and removes them when mastering has completed.

Citrix recommends keeping the number of new permissions required to utilize Shared VPCs to a minimum, because Shared VPCs are higher level corporate resources and typically have more rigid security protocols in place. For this reason, the site administrator must create a pair of firewall rules (one ingress, and one egress) on each Shared VPC with the highest priority and apply a new **Target Tag** to each of the rules. The Target Tag value is:

`citrix-provisioning-quarantine-firewall`

When MCS is creating or updating a machine catalog, it will search for firewall rules containing this Target Tag, examine the rules for correctness, and apply them to the *prep* machine.

If the firewall rules are not found, or the rules are found, but the rules or priority are incorrect, a message of this form will be returned:

`Unable to find valid INGRESS and EGRESS quarantine firewall rules for VPC \<name\> in project \<project\>. Please ensure you have created deny all firewall rules with the network tagcitrix-provisioning-quarantine-firewall and proper priority. Refer to Citrix Documentation for details.`

## Cloud Connectors

When using a Shared VPC for Citrix Virtual Apps and Desktops machine catalogs, you will create two or more Cloud Connectors to access the Domain Controller that resides within the Shared VPC. The recommendation in this case is to create a GCP machine instance in your local project and add an additional network interface to the instance. The first interface would be connected to a subnet in the Shared VPC. The second network interface would connect to a subnet in your Local VPC to allow access for administrative control and maintenance via your Local VPC Bastion Server.

Unfortunately, you cannot add a network interface to a GCP instance after it has been created. It is a simple process and is covered below in one of the How To entries.

## How To Section

The following section contains a series of instructional examples to help you understand the steps to perform the configuration changes necessary to use Google Shared VPCs with Citrix Virtual Apps and Desktops.

The examples presented in screenshots of Google Console will all take place in a hypothetical Google Project named *Shared VPC Project 1*.

### How To: Create a New IAM Role

Some additional permissions need to be granted to the Service Account used when creating the Host Connection. Since the intent of deploying to Shared VPCs is to allow multiple projects to deploy to the same Shared VPC, the most efficient approach is to create a new role in the Host Project with the desired permissions and then assign that role to any Service Account that requires access to the Shared VPC.

Below we will create the Project-Level role named *Citrix-ProjectLevel-SharedVpcRole.* The Subnet-Level role simply follows the same steps for the first two sets of permissions assigned.

#### IAM & Admin in the Google Console

Access the **IAM & Admin** configuration option in the Google Cloud Console:

![IAM and Admin configuration option](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_001.png)

#### Create Role

Select **Create Role**:

![Create Role option](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_002.png)

#### Empty Create Role Screen

A screen resembling the following appears:

![Empty Create Role screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_003.png)

#### Fill in the Name and ADD PERMISSION

Specify the **Role Name**. Click **ADD PERMISSIONS** to apply the update:

![Role name specification](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_004.png)

#### Add Permissions Dialog

After clicking, **ADD PERMISSIONS**, a screen resembling the once below appears.

Note that in this image the “*Filter Table”* text entry field has been highlighted:

![Highlighted Filter Table text entry](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_005.png)

#### Add compute.firewalls.list Permission

Clicking the **Filter Table** text entry field displays a contextual menu:

![Contextual menu](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_006.png)

Copy and paste (or type) the string **compute.firewalls.list** into the text field, as shown below:

![Adding string to a text field](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_007.png)

Selecting the **compute.firewalls.list** entry that has been filtered out of the table of permissions results in this dialog:

![Permission dialog box](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_008.png)

Click the toggle box to enable the permission:

![Enable permission](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_009.png)

Click **ADD**.

The **Create Role** screen reappears. Note that the *compute.firewalls.list* permission has been added to the role:

![Create Role screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_010.png)

#### Add compute.networks.list Permission

Using the same steps as above, add the *compute.networks.list* permission. However, make sure to select the proper rule. As you can see below, when the permission text is entered into the **filter table** field, two permissions are listed. Choose the *compute.networks.list* entry:

![Choosing the correct permission](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_011.png)

![Correct permission entry](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_012.png)

Click **ADD**.

The two Mandatory permissions added to our role:

![Mandatory permission role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_013.png)

#### Project Level or Subnet-Level

Determine what level of access the Role has, such as Project-Level access, or a more restricted model using Subnet-Level access. For the purposes of this document, we are currently creating the role named `Citrix-ProjectLevel-SharedVpc Role`, so we will add the *compute.subnetworks.list* and *compute.subnetworks.use* permissions using the same steps used above. The resulting screen looks like this, with the four permissions granted, just before clicking **Create**:

![Four permissions granted](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_014.png)

Click **CREATE**.

  >**Note:**
  >
  >If the Subnet-Level Role were created here, we would have clicked **CREATE** rather than add the two additional *compute.subnetworks.list* and *compute.subnetworks.use* permissions.

#### Citrix-ProjectLevel-SharedVpc Role Created

![Subnet-level role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_015.png)

### How To: Add Service Account to Host Project IAM Role

Now that we have created the new `Citrix-ProjectLevel-SharedVpc Role`, we need to add a Service Account to it within the Host Project. For this example, we will use a Service Account named `citrix-shared-vpc-service-account`.

#### Navigate to IAM & Admin

The first step is to navigate to the **IAM & Roles** screen for the project. In the console, select **IAM and Admin**. Select **IAM**:

![IAM selection](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_016.png)

#### Project Permissions Screen

Add members with the specified permissions. Click **ADD** to display the list of members:

![Display list of members](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_017.png)

#### Add Members Panel

Clicking **ADD** displays a small panel as shown in the image below. Data will be entered in the following step.

![Add Members panel](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_018.png)

#### Add Service Account

Start typing the name of your Service Account into the field. As you type, Google Cloud will search the projects you have permissions to access and present a narrowed list of possible matches. In this case, we have one match (displayed directly below the fill-in), so we select that entry:

![Service Account entry](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_019.png)

#### Role Selection

After specifying the **Member Name** (in our case the Service Account), select a role for the Service Account to function as in the Shared VPC Project. Start this process by clicking the indicated list:

![Select role for Service Account](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_020.png)

#### Selecting a Role

Note that the **Select a Role** process is similar to the ones used in the previous How To - Create a New IAM
Role. In this case, several more options are displayed as well as the fill-in.

![More Select a Role option](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_021.png)

#### Specify the Role

Since we know the Role we want to apply, we can start typing. Once the intended Role appears, select the role:

![Role selection](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_022.png)

#### Select and Save

After selecting the Role, click **Save**:

![Save the role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_023.png)

We have now successfully added the Service Account to the Host Project.

### How To: Subnet-Level Permissions

If you have chosen to use Subnet-Level access rather than Project-Level access, you must add the Service Accounts to be used with the Shared VPC as members for each subnet representing the resources to be accessed. For this How To section, we are going to provide the Service Account named `sharedvpc-sa\@citrix-mcs-documentation.iam.gserviceaccount.com` with access to a single subnet in our Shared VPC.

#### Navigate to VPC > Shared VPC

The first step is to navigate to the Shared VPC screen in Google Console:

![Shared VPC screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_024.png)

#### Initial Shared VPC Screen

This is the landing page for the Google Cloud Console Shared VPC screen. This project shows five subnets. The Service Account in this example requires access to the second *subnet-good* subnet (the last subnet on the list below).

Select the check box next to the second *subnet-good* subnet:

![Subnets](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_025.png)

#### Select the subnet for Service Account access

Now that the check box for the last subnet has been selected, note that the **ADD MEMBER** option appears on the upper right of the screen.

It is also useful for this exercise to take note of the number of users this subnet has been shared with. As indicated, one user has access to this subnet.

Click **ADD MEMBER**:

![Add Member option](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_026.png)

#### Fill in New member name

Similar to the steps required to add the Service Account to the Host Project in the preceding How To section, the New member name needs to be provided here as well. After filling in the name, Google Cloud lists all related items (as before) so that we can select the relevant Service Account. In this case, it is a single entry.

Double-click the **Service Account** to select it:

![Service Account option](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_027.png)

#### Select a Role for the new Member

After a Service Account has been selected, a Role for the new Member needs to be chosen as well:

1.  In the list, click **Select a role**.

1.  Double-click the *Compute Network User* Role.

![Computer Network Users Role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_028.png)

#### Role Selected

The image shows that the Service Account and Role have been specified. The only remaining step is to click **SAVE** to commit the changes:

![Select Role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_029.png)

#### User has been added to the subnet

After the changes have been saved, the main Shared VPC screen appears. Observe that the number of users who have access to the last subnet has, as expected, increased to two:

![Added members](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_030.png)

### How To: Add Project CloudBuild Service Account to the Shared VPC

Every Google Cloud Subscription has a service account that is named after the project ID number followed by *cloudbuild.gserviceaccount*. A full name example (using a made-up project ID) is:

*705794712345\@ cloudbuild.gserviceaccount.*

This *cloudbuild Service Account* also needs to be added as a member of the Shared VPC in the same way the Service Account you use for creating Host Connections was in Step 3 of [How To: Add Service Account to Host Project IAM Role](#how-to-add-service-account-to-host-project-iam-role).

You can determine what the Project ID number is for your project by selecting **Home** and **Dashboard** in the Google Cloud Console menu:

![Project ID number](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_031.png)

Find the **Project Number** under the **Project Info** area of the screen.

Enter the project number/cloudbuild.gserviceaccount combination into the **Add Member** field. Assign a Role of **Computer Network User**:

![Assigning a role](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_032.png)

Select **Save**.

### How To: Firewall Rules

Creating the needed firewall rules is a bit easier than creating the Roles.

#### Select Host Project

As noted previously in this document, the two firewall rules need to be created in the Host Project.

Make certain you have selected the **Host Project**.

#### VPC Network > Firewall

From the Google Console menu, navigate to **VPC > Firewall**, as shown below:

![Firewall options](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_033.png)

#### Create Firewall Rule Button

The top of the Firewall screen in Google Console includes a button to create a new rule.

Click **CREATE FIREWALL RULE**:

![Create Firewall rule](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_034.png)

#### Create New Firewall Screen

The screen used to create a new firewall rules is shown below:

![Create new firewall rule](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_035.png)

#### Ingress Rule: Fill in Data

First, create the needed **Deny-All Ingress** rule by adding or changing values to the following fields:

-  **Name**

    Give your Deny-All Ingress firewall rule a name. For example,
    `citrix-deny-all-ingress-rule`.

-  **Network**

    Select the Shared VPC network this ingress firewall rule will be applied against. For example, `gcp-test-vpc`.

-  **Priority**

    The value in this field is critical. In the world of firewall rules, the *lower* the priority value is the *higher* the rule is in priority. This is why all the default rules have a value of 66536, so that any custom rules, which will have a value lower than 65536, will take priority over any of the default rules.

    For these two rules we need them to have the highest priority rules on the network. We will use a value of 10.

-  **Direction of traffic**

    The default for creating a new rule is **Ingress**, which should already be selected.

-  **Action on match**

    This value will default to **Allow**. We need to change it to **Deny**.

-  **Targets**

    This is the other very critical field. The default type of targets is **Specified target tags**, which is precisely what we want. In the text box labeled **Target Tags** enter the value
    *citrix-provisioning-quarantine-firewall.*

-  **Source Filter**

    For the source filter we will retain the default filter type of **IP ranges** and enter a range that will match **all** traffic. For that we use a value of 0.0.0.0/0.

-  **Protocols and ports**

    Under Protocols and ports, select **Deny All.**

The completed screen should look like this:

![Final screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_036.png)

Click **CREATE** and generate the new rule.

#### Egress Rule: Fill in Data

The egress rule is almost identical to the previously created ingress rule. Use the **CREATE FIREWALL RULE** again as was done above and fill in the fields as detailed below:

-  **Name**

    Give your Deny-All Egress firewall rule a name. Here we will call it *citrix-deny-all-egress-rule.*

-  **Network**

    Select the same Shared VPC network here that was used when creating the ingress firewall rule above. For example, `gcp-test-vpc`.

-  **Priority**

    As noted above, we will use a value of 10.

-  **Direction of traffic**

    For this rule, we need to change from the default and select **Egress**.

-  **Action on match**

    This value will default to **Allow**. We need to change it to **Deny**.

-  **Targets**

    Enter the value *citrix-provisioning-quarantine-firewall* into the **Target Tags** field.

-  **Source Filter**

    For **source filter**, we will retain the default filter type of **IP ranges** and enter a range that will match **all** traffic. For that we use a value of 0.0.0.0/0.

-  **Protocols and ports**

    Under **Protocols and ports**, select **Deny All**.

The completed screen should look like this:

![Completed rule screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_037.png)

Click **CREATE** to generate the new rule.

Both of the necessary firewall rules have been created. If multiple Shared VPCs will be used when deploying machine catalogs, repeat the steps performed above. Create two rules for each of the identified Shared VPCs in their respective Host Projects.

### How To: Add Network Interface to Cloud Connector Instances

When creating Cloud Connectors for use with the Shared VPC, an additional network interface needs to be added to the instance **when it is being created**.

Additional network interfaces cannot be added once the instance exists. To add the second network interface:

#### The initial Network settings panel for a GCP instance

This is the initial panel for network settings presented when first creating a network instance

![Network settings panel](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_038.png)

Since we want to use the first network instance for the Shared VPC, click the **Pencil** icon to enter **Edit** mode.

The expanded network settings screen is below. A key item to note is that we can now see the option for **Networks shared with me (from host project: citrix-shared-vpc-project-1)** directly beneath the **Network Interface** banner:

![Networks shared with me options](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_039.png)

The **Network Settings panel with Shared VPC selected** panel shows:

-  Selected the Shared VPC network.

-  Selected the **subnet-good** subnet.

-  Modified the setting to an external IP address to **None**.

![Network Settings panel](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_040.png)

Click **Done** to save the changes. Click **Add Network Interface**.

#### Add Second Network Interface

We now have our first interface connected to the Shared VPC (as indicated). We can configure the second interface using the same steps as would normally be used when creating a new Cloud Connector. Select a network, subnet, make a decision on an external IP address, and then click **Done**:

![Second network interface](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_041.png)

### How To: Creating Host Connection and Hosting Unit

Creating a Host Connection for use with Shared VPCs is not much different than it is for creating one for use with a Local VPC. The difference is with selecting the resources to be associated with the Host Connection. When creating a Host Connection to access Shared VPC resources, use Service Account JSON files related to the project where the provisioned machines reside.

#### Credentials and Connection Name

Creating a Host Connection for using Shared VPC resources is similar to creating any other GCP related Host Connection:

![Host connection](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_042.png)

#### Select Project and Region

Once your project, shown as **Developer Project** in the following figure, has been added to the list that can access the Shared VPC, you may see both your project **and** the Shared VPC project in Studio. It is important to ensure you select the project where the deployed machine catalog should reside and **not** the Shared VPC:

![Selecting project and region](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_043.png)

#### Select Resources

Select the resources associated with the Host Connection.

Consider the following:

-  The Name given for the resources is *SharedVPCResources.*

-  The list of virtual networks to choose from includes those from the local project, as well as those from the Shared VPC, as indicated by **(Shared)** appended to the network names.

>**Note:**
>
>If you do not see any networks with **Shared** appended to the name, click the **Back** button and verify you have chosen the correct Project. If you verify the project chosen is correct and still do not see any shared VPCs, something is misconfigured in the Google Cloud Console. See the [Commonly Encountered Issues and Errors](#commonly-encountered-issues-and-errors) later in this document.

![Possible errors](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_044.png)

#### Resources Selected

The following figure shows that the **gcp-test-vpc (Shared)** virtual network was selected in the previous step. It also shows that the subnet named **subnet-good** has been selected. Click **Next**:

![Selected resources](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_045.png)

#### Summary Screen

After clicking **Next,** the **Summary screen** appears. In this screen, consider:

-  The Project is **Developer Project**.

-  The virtual network is **gcp-test-vpc**, one from the Shared VPC.

-  The subnet is **subnet-good**.

![Summary screen](/en-us/tech-zone/learn/media/poc-guides_gcp-shared-vpc_046.png)

### How To: Creating a Catalog

Everything from this point forward, such as catalog creation, starting/stopping machines, updating machines, and so on, is performed exactly the same as is done when using Local VPCs.

## Commonly Encountered Issues and Errors

Working with any complex system with interdependencies may result in unexpected situations. A few common issues and errors that may be encountered when performing the setup and configuration to use Citrix Virtual Apps and Desktops and GCP Shared VPCs can be found below.

### Missing or Incorrect Firewall Rules

If the firewall rules are not found, or the rules are found but the rules or priority are incorrect, a message of this form will be returned:

"Unable to find valid INGRESS and EGRESS quarantine firewall rules for VPC
\<name\> in project \<project\>. "Please ensure you have created 'deny all'
firewall rules with the network tag ‘citrix-provisioning-quarantine-firewall'
and proper priority." "Refer to Citrix Documentation for details.");

If this message is encountered, you must review the firewall rules, their priorities, and what networks they apply to. For details, Refer to the section [How To - Firewall
Rules](#how-to-add-project-cloudbuild-service-account-to-the-shared-vpc).

### Missing Shared Resources When Creating Host Connection

There are a few reasons this situation may occur:

#### The incorrect project was selected when creating the Host Connection

For example, if the Shared VPC Host Project was selected when creating the Host Connection instead of your project, you will still see the network resources from the Shared VPC **but** they will not have **(Shared)** appended to them.

If you are seeing the shared subnets without that extra information, the Host Connection was likely made with the wrong Service Account.

See [How To -Creating Host Connection and Hosting
Unit](#how-to-creating-host-connection-and-hosting-unit).

#### Wrong Role was assigned to the Service Account

If the wrong role was assigned to the Service Account, you may not be able to access the desired resources in the Shared VPC. See [How To - Add Service Account To Host Project IAM
Role](#how-to-add-service-account-to-host-project-iam-role).

#### Incomplete or incorrect permissions granted to Role

The correct Role may be assigned to the Service Account, but the Role itself may be incomplete. See [How To – Create a New IAM
Role](#how-to-create-a-new-iam-role).

#### Service Account not added as subnet Member

If you are using Subnet-Level access, ensure the Service Account was properly added as a Member (User) of the desired subnet resources. See [How To -Subnet-Level Permissions](#how-to-subnet-level-permissions).

### Cannot find path error in Studio

If you receive an error while creating a catalog in Studio of the form:

`Cannot find path “XDHyp:\\Connections …” because it does not exist`

It is most likely that a new Cloud Connector was not created to facilitate use of the Shared VPC resources. It is a simple thing to overlook after going through all the above steps to configure everything. Refer to [Cloud Connectors](#cloud-connectors) for important points on creating them.
