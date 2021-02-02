---
layout: doc
description: Copy & paste description from TOC here
---
# Citrix Virtual Apps and Desktops Service – GCP Implementation with Managed Microsoft Active Directory for CSPs

## Contributors

**Author:** [JP Alfaro](https://www.linkedin.com/in/jp-alfaro-b2bb03b2/)

## Architecture

GCP’s Managed Service for Microsoft Active Directory is a fully managed service on the Google Cloud Platform. The service automatically deploys and manages highly available Active Directory domain controllers on your GCP project in an isolated VPC network. Domain controller access is restricted, and you can only manage your domain by deploying management instances with Remote Server Administration tools. A VPC peering is deployed automatically with the service for your AD-dependent workloads to reach Active Directory. Additionally, Google Cloud DNS is configured to forward all DNS queries to the Managed Microsoft AD.
For this implementation, we are following Google's Active Directory resource forest architecture with a Citrix Virtual Apps and Desktops service multitenant environment.

## Citrix and GCP Services

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_001.png)

## GCP Resource Hierarchy and Billing

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_002.png)

### Initial Assumptions

#### Google Cloud Platform

*  A GCP subscription is available and billing has been configured
*  GCP Cloud Identity has been configured (pre-requisite to deploy a GCP organization)
*  An organization has been configured along with a folder to deploy the test/dev GCP projects to be utilized. Check this link to learn more about:

[GCP Resource Hierarchy](https://cloud.google.com/resource-manager/docs/cloud-platform-resource-hierarchy)

*  The following GCP APIs are enabled:
    *  Compute Engine API
    *  Cloud Resource Manager API
    *  Identity and Access Management (IAM) API
    *  Cloud Build API
*  A GCP project is deployed with 2 subnets:
    *  Resources subnet: resources subnet to deploy the Managed Microsoft AD service, and the Citrix Cloud Connectors, Master Images, and VDAs. Most of the configurations is performed on this project
    *  AD management subnet:  management subnet dedicated to instances utilized to manage Active Directory through the Remote Server Administration Tools
*  Managed Microsoft AD service is deployed:
    *  Completely managed by GCP
    *  Deploys its own subnet, which is not viewable through the GCP console
    *  A VPC peering is deployed automatically with the service for connectivity from your GCP projects
    *  Google Cloud DNS is configured to forward all DNS queries to the managed domain controllers

### Citrix Cloud

*  A Citrix Cloud subscription is available
*  Citrix Cloud Connector is deployed
*  VDA master image is deployed
*  Google Cloud Platform hosting connection is configured
*  Machine Catalog and Delivery Group is configured

### GCP Terminology

The following are the most common GCP terms you need to understand, as described on the GCP documentation:

*  Organizations: Organizations represent the root node in the GCP resource hierarchy, to create an organization, GCP Cloud Identity, or G-Suite is required. An organization is not required for this implementation, but it is highly recommended to deploy one for your production environments to properly organize your resources.
*  Folders: Folders are utilized to organize resources within GCP, they can contain more folders, or projects. For example, you can create folders to separate projects by department, environment type, etc. A folder is not required for this implementation, but in general terms, they are recommended for proper resource organization.
*  Projects: A project provides an abstract grouping of resources within GCP, all resources in this deployment must belong to a GCP project. Under normal circumstances, VM instances from one project cannot communicate with VM instances on another project, unless Shared VPC is utilized. Shared VPCs are not currently supported by Citrix Cloud.
*  Billing Accounts: Billing accounts represent the payment profile to be utilized to pay for GCP consumption. A billing account can be linked to multiple projects, but a project can only be linked to a single billing account.
*  IAM: GCP’s Identity and Access Management platform. It is utilized to grant user permissions to perform actions on GCP resources. This platform is also utilized to deploy and manage Service Accounts. We utilize IAM to configure various permissions.
*  Service Account: a service account is a GCP account that is not connected to an actual user, but instead represents a VM instance or an application. Service accounts can be granted permissions to perform different actions on the various GCP APIs. A service account is created to connect Citrix Cloud to GCP and enable Machine Creation Services.
*  GCE: Google Compute Engine, this is the GCP platform in which you deploy compute resources, including VM instances, disks, instance templates, instance groups, etc.
*  GCE Instance: A VM deployed on the GCE platform. We deploy GCE instances for Cloud Connectors, master image, the AD management VM, etc. All machines created through a Citrix machine catalog are deployed as GCE instances.
*  Instance Template: A “baseline” resource you can utilize to deploy VMs and instance groups in GCP. The Citrix MCS process copies the master image into an instance template, which is utilized to deploy catalog machines.
*  VPC Network: GCP’s virtual network object. VPCs in GCP are global, meaning you can deploy subnets to a VPC from each GCP region. You can deploy VPCs in auto-mode, which creates all subnets and CIDR ranges automatically, or custom-mode, which lets you create subnets and CIDR ranges manually. Non-overlapping VPCs from different projects can be connected through a VPC peering.
*  Shared VPC: A shared VPC can be spanned across multiple projects, eliminating the requirement to create separate VPCs for each project, or the utilization of VPC peerings.
*  VPC Peering: A VPC peering allows you to connect VPCs which would otherwise be disconnected. In this implementation, the GCP Managed Microsoft AD service creates a VPC peering automatically to connect our VPC to the managed AD service VPC.
*  Cloud DNS: GCP service utilized to manage DNS zones and records. With the creation of the Managed Microsoft AD service, Cloud DNS is automatically configured to forward DNS queries to the managed domain controllers.

## Implementation

## Google Components

### Create the GCP main project

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_003.png)

*  On the GCP console, click Select a project.

Considerations:

*  A project might already be selected, so the menu shows the name of your current project instead of Select a project.
*  As recommended by Google, for production environments you should be protecting your projects against accidental deletion.
*  On the projects window, click NEW PROJECT.

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_004.png)

*  On the New Project screen, enter the following information:
    *  Project name: must be unique
    *  Billing account: the billing account to use for this project
    *  Organization: your GCP organization
    *  Location: Folder under which to deploy the project

    [![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_005.png)

*  Delete the main project default VPC
*  On the navigation menu, go to VPC Network > VPC Networks and select your default VPC.

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_006.png)

#### Considerations

*  When a new project is created, a default VPC (auto-mode) is automatically created, we will create a new VPC soon.
*  On the VPC network details screen, click DELETE VPC NETWORK and then DELETE.

[![CSP-Image-7](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_007.png)

#### Create a new custom-mode VPC

*  On the navigation menu, go to VPC Network > VPC Networks and click Create VPC network.

[![CSP-Image-8](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_008.png)

*  On the Create a VPC network screen, enter the following initial information:
    *  Name: VPC network name
    *  Description: VPC network description
    *  Subnet creation mode: Custom

[![CSP-Image-9](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_009.png)

Considerations:

*  Auto-mode VPCs automatically create a subnet on each GCP region and assign an RFC 1918 CIDR range.
*  Custom-mode VPCs allow you to create your subnets and CIDR ranges as required.
*  On the Subnets section, create 2 subnets, these is utilized by the Citrix instances, and the AD management servers.

[![CSP-Image-10](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_010.png)

*  Check all the details and click Create.

[![CSP-Image-11](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_011.png)

#### Create Firewall Rules

*  On the navigation menu, go to VPC Network > Firewall Rules, select CREATE FIREWALL RULE.

[![CSP-Image-12](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_012.png)

Considerations:

*  Since this is a testing environment, we are allowing inbound RDP, SSH, and ICMP from any external network, for production environments, set up the firewall rules appropriately to ensure only authorized users / networks can access the instances.
*  On the Create a firewall rule screen, enter the following information:
    *  Name: firewall rule name
    *  Description: firewall rule description
    *  Network: shared VPC network
    *  Priority: rule priority

[![CSP-Image-13](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_013.png)

*  Scroll down and enter the following information:
    *  Direction: Ingress
    *  Action: Allow
    *  Targets: as appropriate for your environment
    *  Source filter: as appropriate for your environment

[![CSP-Image-14](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_014.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_014.png)

Considerations:

*  Targets and Source filter are configured under the assumption that this is a testing environment, production environments should be configured accordingly to ensure access is restricted to authorized targets and sources only.
*  Under Protocols and ports, enter the following information:
    *  Specified protocols and ports
    *  TCP: 22, 3389
    *  Other protocols: ICMP
*  Click CREATE.

[![CSP-Image-15](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_015.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_015.png)

#### Deploy the Managed Microsoft AD Service

*  On the navigation menu, go to Security > Managed Microsoft AD and click CREATE NEW AD DOMAIN.

[![CSP-Image-16](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_016.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_016.png)

Considerations:

*  Managed domain controllers are deployed with the ADDS and DNS roles.
*  As explained previously, the domain controllers are not accessible or appear on the instance list on Compute Engine.
*  We will create a management VM for Active Directory shortly.
*  On the Create a new domain screen, enter the following information:
    *  FQDN: Domain FQDN
    *  NetBIOS: is automatically populated
    *  Select networks: networks that will have access to the service, in this case we are choosing our shared VPC
    *  CIDR Range: a /24 CIDR range for the VPC where the domain controllers is deployed, it must not overlap with your current subnets

[![CSP-Image-17](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_017.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_017.png)

Considerations:

*  The VPC that is deployed as part of the service cannot be managed from the GCP console.
*  Scroll down and enter the following information:
    *  Region: GCP regions to which the service is available
    *  Delegated Admin: name of the delegated administrator account
*  Click CREATE DOMAIN.

[![CSP-Image-18](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_018.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_018.png)

Considerations:

*  The delegated admin account is utilized to manage AD objects, add additional administrators, etc.
*  The delegated administrator account resides on the Users container within AD, you can reset its password directly in the ADUC console, but you cannot move the object to another OU.
*  The delegated administrator account has access to the following OUs:
    *  Cloud: Full control
    *  Cloud Service Objects: very limited update permissions
*  When joining a computer to the domain, the AD account is created under the Cloud > Computers OU, not the default Computers container.
*  Service creation can take up to 60 minutes.
*  Once creation is finalized, select your domain and click SET PASSWORD.

[![CSP-Image-19](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_019.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_019.png)

*  On the Set password window, click CONFIRM.

[![CSP-Image-20](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_020.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_020.png)

*  On the New password window, copy the password and click DONE.

[![CSP-Image-21](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_021.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_021.png)

#### Create the management instance

*  On the navigation menu, goto: Compute Engine > VM Instances and click Create.

[![CSP-Image-22](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_022.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_022.png)

*  On the instance creation window, enter a Name and select the Region and Zone.

[![CSP-Image-23](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_023.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_023.png)

Considerations:

*  Region and zone must match those of your management subnet.
*  Scroll down to Boot disk and click Change.

[![CSP-Image-24](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_024.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_024.png)

*  On the Boot disk window, select the following:
    *  Operating System: Windows Server
    *  Version: select your preferred Windows Server version
*  Click Select.

[![CSP-Image-25](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_025.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_025.png)

*  Scroll down and click Management, security, disks, networking, sole tenancy.

[![CSP-Image-26](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_026.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_026.png)

*  Select Networking and configure the following:
    *  Shared subnetwork: your management subnet.
    *  Primary internal IP: Create IP address
    *  External IP: Create IP address
*  Click Done.

[![CSP-Image-27](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_027.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_027.png)

*  Scroll to the bottom and click Create.

[![CSP-Image-28](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_028.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_028.png)

#### Configure the management

*  Still on the Compute Engine > VM Instances screen, under Connect, click the arrow and select Set windows password.

[![CSP-Image-29](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_029.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_029.png)

Considerations:

*  This configures the local windows admin user name and password, not a domain user password.
*  This process assumes basic windows administration knowledge.
*  On the Set new Windows password window, enter a user name and click SET.

[![CSP-Image-30](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_030.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_030.png)

*  On the New Windows password window, copy the password and click CLOSE.

[![CSP-Image-31](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_031.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_031.png)

*  Connect to the instance via RDP and start by installation the following Windows features from server manager:
    *  Role Administration Tools
    *  ADDS and AD LDS Tools
    *  Active Directory module for Windows PowerShell
    *  AD DS Tools
    *  AD DS Snap-ins and Command-Line Tools
    *  Group Policy Management Console (GPMC)
    *  DNS Manager

    [![CSP-Image-32](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_032.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_032.png)

*  Join the instance to the domain and restart it.

[![CSP-Image-33](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_033.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_033.png)

Considerations:

*  Remember that the Managed Microsoft AD service deployment configures GCP Cloud DNS to forward DNS queries to the managed domain controllers, DNS resolution should automatically work when joining the instance to the domain.
*  Optionally, reconnect to the instance and launch any management tool to perform specific configurations like creating OUs, users, configuring DNS records, etc.

#### Create a GCP service account

*  On the navigation menu, go to IAM & Admin > Service Accounts, then click CREATE SERVICE ACCOUNT.

[![CSP-Image-34](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_034.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_034.png)

Considerations:

*  This service account is utilized to configure a hosting connection in Citrix Cloud.
*  On the Service account details screen, enter the following information:
    *  Service account name: enter a name for your service account
    *  Service account ID: enter an ID for the service account (can be the same as the name)
    *  Service account description: optional description
*  Click CREATE.

[![CSP-Image-35](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_035.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_035.png)

Considerations:

*  Service account names must be unique.
*  On the Grant this service account access to project screen, click CANCEL.

[![CSP-Image-36](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_036.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_036.png)

Considerations:

*  Service account permissions is configured shortly.
*  On the Service accounts screen, click the Actions menu (three dots) for your service account and select Create key.

[![CSP-Image-37](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_037.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_037.png)

*  On the Create private key screen, choose JSON and click CREATE.

[![CSP-Image-38](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_038.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_038.png)

Considerations:

*  The private key is downloaded to your computer, we will utilize the contents of the key file when creating a hosting connection in Citrix Cloud.
*  On the Private key saved to your computer window, click CLOSE.

[![CSP-Image-39](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_039.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_039.png)

*  On the navigation menu, go to IAM & Admin > IAM, and click ADD.
*  On the Add members screen, enter the following information.
    *  New members: the new service account.
    *  Roles:
        *  Compute Admin
        *  Storage Admin
        *  Cloud Build Editor
        *  Service Account User
        *  Cloud Datastore User
    *  Click SAVE.

[![CSP-Image-40](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_040.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_040.png)

*  Back on the IAM screen, search for the Cloud Build Service Account and click the edit pencil icon on the far right.

[![CSP-Image-41](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_041.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_041.png)

*  On the Edit permissions page, enter the following information:
    *  Roles:
        *  Cloud Build Service Account
        *  Compute Instance Admin
        *  Service Account User
    *  Click SAVE.

[![CSP-Image-42](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_042.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_042.png)

### CITRIX COMPONENTS

#### Create the Cloud Connector VM

*  On the VM instances page, repeat steps Create the management instance and Configure the management VM to create a Windows instance and join it to the domain, this is utilized as the Citrix Cloud Connector.

[![CSP-Image-43](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_043.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_043.png)

Considerations:

*  The Cloud Connector instance must be created on the GCP resources subnet.
*  On a production deployment, at least 2 Cloud Connectors must be deployed to avoid possible service disruption.
*  Once the instance is created, connect to it via RDP and use a web browser to navigate to <https://citrix.cloud.com.>

[![CSP-Image-44](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_044.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_044.png)

*  Enter your Citrix Cloud credentials and click Sign-in.

[![CSP-Image-45](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_045.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_045.png)

*  Under Domains, click Add New.

[![CSP-Image-46](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_046.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_046.png)

*  On the Domains tab under Identity and Access Management, click +Domain.

[![CSP-Image-47](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_047.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_047.png)

*  On the Add a Cloud Connector window click Download.

[![CSP-Image-48](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_048.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_048.png)

*  Save the cwcconnector.exe file to the instance.

[![CSP-Image-49](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_049.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_049.png)

*  Right-click the cwcconnector.exe file and select Run as administrator.

[![CSP-Image-50](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_050.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_050.png)

*  On the Citrix Cloud Connector window, click Sign-in.

[![CSP-Image-51](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_051.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_051.png)

*  On the sign-in window, enter your Citrix Cloud credentials and click Sign-in.

[![CSP-Image-52](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_052.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_052.png)

*  When the installation finishes, click Close.

[![CSP-Image-53](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_053.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_053.png)

#### Considerations

Cloud Connector installation will take up to 5 minutes

### Create the VDA master image VM

*  On the VM instances page, repeat steps 2.1.10 and 2.1.11 to create a Windows instance and join it to the domain to be utilized as the Citrix VDA.

[![CSP-Image-54](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_054.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_054.png)

Considerations:

*  This process assumes previous Citrix administration knowledge.
*  Once the instance is created, connect to it via RDP and use a web browser to navigate to <https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/> and download the latest Citrix VDA version.

[![CSP-Image-55](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_055.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_055.png)

Considerations:

*  Citrix credentials is required to download the VDA software.
*  Either the LTSR or CR version can be installed.
*  Right-click the VDA installer file and select Run as administrator.

[![CSP-Image-56](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_056.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_056.png)

*  On the Environment page, select Create a master MCS image.

[![CSP-Image-57](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_057.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_057.png)

*  On the Core Components page, click Next.

[![CSP-Image-58](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_058.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_058.png)

*  On the Additional Components page, select the components that best apply to your requirements and click Next.

[![CSP-Image-59](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_059.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_059.png)

*  On the Delivery Controller page, enter the following information:
    *  Select “Do it manually”
    *  Enter the FQDN of each Cloud Connector
    *  Click Test Connection and if you get a green checkmark, click Add
*  Click Next.

[![CSP-Image-60](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_060.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_060.png)

*  On the Features page, check the boxes of the features you want to enable based on your deployment needs, then click Next.

[![CSP-Image-61](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_061.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_061.png)

*  On the Firewall page, select Automatically and click Next.

[![CSP-Image-62](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_062.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_062.png)

*  On the Summary page, ensure all the details are correct and click Install.

[![CSP-Image-63](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_063.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_063.png)

*  The instance will need to be restarted during installation.

[![CSP-Image-64](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_064.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_064.png)

*  After the installation finishes, on the Diagnostics page, select the option that best fits your deployment needs and click Next.

[![CSP-Image-65](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_065.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_065.png)

*  On the Finish page, make sure Restart machine is checked and click Finish.

[![CSP-Image-66](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_066.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_066.png)

### Create a GCP hosting connection

*  Use a web browser to navigate to <https://citrix.cloud.com.>

*  Enter your Citrix Cloud credentials and click Sign-in.
*  On the upper left hamburger menu, click My Services and select Citrix Virtual Apps and Desktops.

*  Under Manage, select Full Configuration.
*  In Citrix Studio, navigate to Citrix Studio > Configuration > Hosting and select Add Connection and Resources.

[![CSP-Image-67](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_067.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_067.png)

*  On the Connection page, click the radio button next to Create a new connection and enter the following information:
    *  Connection type: Google Cloud Platform
    *  Service account key: Enter the contents of your GCP service account key created earlier
    *  Service account ID: is auto populated when entering the service account key
    *  Zone name: Select your Citrix zone
    *  Connection name: enter a name
    *  Create virtual machines using: Studio tools (Machine Creation Services)
*  Click Next.

[![CSP-Image-68](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_068.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_068.png)

Considerations:

*  Citrix Studio Zones are created when you create a resource location and install at least one Cloud Connector.
*  On the Region page, ensure the appropriate GCP project was populated, select the region where your Cloud Connector and VDA were deployed and click Next.

[![CSP-Image-69](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_069.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_069.png)

*  On the Region page, enter a name for the resources, select the appropriate Virtual Network and Subnet and click Next.

[![CSP-Image-70](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_070.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_070.png)

*  On the Scopes page, select the appropriate scope and click Next.

[![CSP-Image-71](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_071.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_071.png)

Considerations:

*  A scope represents a collection of objects. Scopes are used to group objects in a way that is relevant to your organization. Objects can be in more than one scope.
*  On the Summary page, ensure all the information is correct and click Finish.

### Create a Machine Catalog

*  In Citrix Studio, navigate to Citrix Studio > Machine Catalogs and select Create Machine Catalog.

[![CSP-Image-72](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_072.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_072.png)

*  On the Introduction page, click Next.
*  On the Operating System page, select Multi-session OS and click Next.

Considerations:

*  At the moment of this writing, Citrix does not support deploying Windows desktop OS catalogs to GCP. This
*  On the Machine Management page, select the following information:
    *  The machine catalog will use: machines that are powered managed
    *  Deploy machines using: Citrix Machine Creation Services (MCS)
    *  Resources: select your GCP hosting connection

[![CSP-Image-73](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_073.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_073.png)

*  Click Next.
*  On the Master Image page, select the master image, the functional level (VDA version), and click Next.

[![CSP-Image-74](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_074.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_074.png)

*  On the Virtual Machines page, configure the number of virtual machines to deploy and click Next.

Considerations:

*  For GCP, the CPU and RAM for the machines created by MCS is the same as the master image. The master image is utilized to create an instance template in GCP.
*  Catalog machines is deployed without a public IP address on GCP.
*  On the Active Directory Computer Accounts page, configure the following:
    *  Account option: Create new AD accounts
    *  Domain: select your domain
    *  OU: the OU where the computer accounts is stored
    *  Naming scheme: naming convention to be utilized

[![CSP-Image-75](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_075.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_075.png)

Considerations:

*  Pound signs is replaced by numbers on the naming scheme.
*  Be mindful of the NetBIOS 15 character limit when creating a naming scheme
*  On the Domain Credentials page, click Enter credentials.
*  On the Windows Security pop-up, enter your domain credentials and click OK.
*  Back on the Domain Credentials page, click Next.
*  On the Scopes page, select a scope and click Next.

[![CSP-Image-76](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_076.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-gcp_076.png)

*  On the Summary page, enter a name and description and click Finish.
2.2.5 Create a Delivery Group
*  In Citrix Studio, navigate to Citrix Studio > Delivery Groups and select Create Delivery Group.

*  On the Getting started page, click Next.
*  On the Machines page, select your machine catalog, the number of machines, and click Next.

*  On the Users page select an authentication option and click Next.
*  On the Applications page, click Add.
*  On the Add Applications page, select which applications you want to publish and click OK.

Considerations:

*  Whilst most applications should show through the start menu, you can also optionally add applications manually.
*  This step can be skipped if you do not need to publish seamless applications.
*  Back on the Applications page, click Next.
*  On the Desktops page, click Add.
*  On the Add Desktop page, configure the Desktop and click OK.

Considerations:

*  This step can be skipped if you do not need to publish full shared desktops.
*  Back on the Desktops page, click Next.
*  On the Scopes page, select a scope and click Next.
*  On the Summary page, enter a name, a description, and click Finish.
