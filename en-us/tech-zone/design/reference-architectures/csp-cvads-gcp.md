---
layout: doc
description: Copy & paste description from TOC here
---
# Citrix Virtual Apps and Desktops Service – GCP Implementation with Managed Microsoft Active Directory for CSPs

## Contributors

**Author:** [JP Alfaro](URL)

1 BEFORE YOU START
1.1 ARCHITECTURE
GCP’s Managed Service for Microsoft Active Directory is a fully managed service on the Google Cloud Platform. The service automatically deploys and manages highly available Active Directory domain controllers on your GCP project in an isolated VPC network. Domain controller access is restricted, and you can only manage your domain by deploying management instances with Remote Server Administration tools. A VPC peering is deployed automatically with the service for your AD-dependent workloads to reach Active Directory; additionally, Google Cloud DNS is configured to forward all DNS queries to the Managed Microsoft AD.
For this implementation, we are following Google's Active Directory resource forest architecture in combination with a Citrix Virtual Apps and Desktops service multi-tenant environment.
1.2 CITRIX AND GCP SERVICES

1.3 GCP RESOURCE HIERARCHY AND BILLING

1.4 INITIAL ASSUMPTIONS
1.4.1 Google Cloud Platform
• A GCP subscription is available and billing has been configured
• GCP Cloud Identity has been configured (pre-requisite to deploy a GCP organization)
• An organization has been configured along with a folder to deploy the test/dev GCP projects to be utilized. Check this link to learn more about GCP Resource Hierarchy
• The following GCP APIs are enabled:
o Compute Engine API
o Cloud Resource Manager API
o Identity and Access Management (IAM) API
o Cloud Build API
• A GCP projects will be deployed with 2 subnets:
o Resources subnet: resources subnet to deploy the Managed Microsoft AD service, and the Citrix Cloud Connectors, Master Images and VDAs. Most of the configurations will be performed on this project
o AD management subnet:  management subnet dedicated to instances utilized to manage Active Directory through the Remote Server Administration Tools
• Managed Microsoft AD service will be deployed:
o Completely managed by GCP
o Deploys its own subnet, which is not viewable through the GCP console
o A VPC peering is deployed automatically with the service for connectivity from our your GCP projects
o Google Cloud DNS is configured to forward all DNS queries to the managed domain controllers
1.4.2 Citrix Cloud
• A Citrix Cloud subscription is available
• Citrix Cloud Connector will be deployed
• VDA master image will be deployed
• Google Cloud Platform hosting connection will be configured
• Machine Catalog and Delivery Group will be configured

1.5 GCP TERMINOLOGY
The following are the most common GCP terms you will need to understand, as described on the GCP documentation:
• Organizations: Organizations represent the root node in the GCP resource hierarchy, in order to create an organization, GCP Cloud Identity or G-Suite will be required. An organization is not required for this implementation, but it is highly recommended to deploy one for your production environments in order to properly organize your resources.
• Folders: Folders are utilized to organize resources within GCP, they can contain more folders, or projects. For example, you can create folders to separate projects by department, environment type, etc. A folder is not required for this implementation, but in general terms, they are recommended for proper resource organization.
• Projects: A project provides an abstract grouping of resources within GCP, all resources in this deployment need to belong to a GCP project. Under normal circumstances, VM instances from one project cannot communicate with VM instances on another project, unless Shared VPC is utilized. Shared VPCs are yet to be supported by Citrix Cloud.
• Billing Accounts: Billing accounts represent the payment profile to be utilized to pay for GCP consumption. A billing account can be linked to multiple projects, but a project can only be linked to a single billing account.
• IAM: GCP’s Identity and Access Management platform. It is utilized to grant user permissions to perform actions on GCP resources. This platform is also utilized to deploy and manage Service Accounts. We will utilize IAM to configure various permissions.
• Service Account: a service account is a GCP account that is not connected to an actual user, but instead represents a VM instance or an application. Service accounts can be granted permissions to perform different actions on the various GCP APIs. A service account will be created to connect Citrix Cloud to GCP and enable Machine Creation Services.
• GCE: Google Compute Engine, this is the GCP platform in which you deploy compute resources, including VM instances, disks, instance templates, instance groups, etc.
• GCE Instance: A VM deployed on the GCE platform. We will deploy GCE instances for Cloud Connectors, master image, the AD management VM, etc. All machines created through a Citrix machine catalog are deployed as GCE instances.
• Instance Template: A “baseline” resource you can utilize to deploy VMs and instance groups in GCP. The Citrix MCS process will copy the master image into an instance template, which is utilized to deploy catalog machines.
• VPC Network: GCP’s virtual network object. VPC in GCP are global, meaning you can deploy subnets to a VPC from each GCP region. You can deploy VPCs in auto-mode, which creates all subnets and CIDR ranges automatically, or custom-mode, which let you create subnets and CIDR ranges manually. Non-overlapping VPCs from different projects can be connected through a VPC peering.
• Shared VPC: A shared VPC can be spanned across multiple projects, eliminating the requirement to create separate VPCs for each project, or the utilization of VPC peerings.
• VPC Peering: A VPC peering allows you to connect VPCs which would otherwise be disconnected. For the purpose of this implementation, the GCP Managed Microsoft AD service will create a VPC peering automatically to connect our VPC to the managed AD service VPC.
• Cloud DNS: GCP service utilized to manage DNS zones and records. With the creation of the Managed Microsoft AD service, Cloud DNS will be automatically configured to forward DNS queries to the managed domain controllers.

2 IMPLEMENTATION
2.1 GOOGLE COMPONENTS
2.1.1 Create the GCP main project
• On the GCP console, click Select a project.

Considerations:
• A project might already be selected, so the menu will show the name of your current project instead of Select a project.
• As recommended by Google, for production environments you should be protecting your projects against accidental deletion.
• On the projects window, click NEW PROJECT.

• On the New Project screen, enter the following information:
o Project name: must be unique
o Billing account: the billing account to use for this project
o Organization: your GCP organization
o Location: Folder under which to deploy the project
2.1.2 Delete the main project default VPC
• On the navigation menu, go to VPC Network > VPC Networks and select your default VPC.

Considerations:
• When a new project is created, a default VPC (auto-mode) is automatically created, we will create a new VPC soon.
• On the VPC network details screen, click DELETE VPC NETWORK and then DELETE.
2.1.3 Create a new custom-mode VPC
• On the navigation menu, go to VPC Network > VPC Networks and click Create VPC network.

• On the Create a VPC network screen, enter the following initial information:
o Name: VPC network name
o Description: VPC network description
o Subnet creation mode: Custom

Considerations:
• Auto-mode VPCs automatically create a subnet on each GCP region and assign an RFC 1918 CIDR range.
• Custom-mode VPCs allow you to create your subnets and CIDR ranges as required.
• On the Subnets section, create 2 subnets, these will be utilized by the Citrix instances, and the AD management servers.

• Check all the details and click Create.
2.1.4 Create Firewall Rules
• On the navigation menu, go to VPC Network > Firewall Rules, select CREATE FIREWALL RULE.

Considerations:
• Since this is a testing environment, we are allowing inbound RDP, SSH, and ICMP from any external network, for production environments, set-up the firewall rules appropriately to ensure only authorized users / networks can access the instances.
• On the Create a firewall rule screen, enter the following information:
o Name: firewall rule name
o Description: firewall rule description
o Network: shared VPC network
o Priority: rule priority
• Scroll down and enter the following information:
o Direction: Ingress
o Action: Allow
o Targets: as appropriate for your environment
o Source filter: as appropriate for your environment

Considerations:
• Targets and Source filter are configured under the assumption that this is a testing environment, production environments should be configured accordingly to ensure access is restricted to authorized targets and sources only.
• Under Protocols and ports, enter the following information:
o Specified protocols and ports
 TCP: 22, 3389
 Other protocols: ICMP
• Click CREATE.
2.1.5 Deploy the Managed Microsoft AD Service
• On the navigation menu, go to Security > Managed Microsoft AD and click CREATE NEW AD DOMAIN.

Considerations:
• Managed domain controllers are deployed with the ADDS and DNS roles.
• As explained previously, the domain controllers are not accessible or appear on the instance list on Compute Engine.
• We will create a management VM for Active Directory shortly.
• On the Create a new domain screen, enter the following information:
o FQDN: Domain FQDN
o NetBIOS: will be automatically populated
o Select networks: networks that will have access to the service, in this case we are choosing our shared VPC
o CIDR Range: a /24 CIDR range for the VPC where the domain controllers will be deployed, it must not overlap with your current subnets

Considerations:
• The VPC that is deployed as part of the service cannot be managed from the GCP console.
• Scroll down an enter the following information:
o Region: GCP regions to which the service will be available
o Delegated Admin: name of the delegated administrator account
• Click CREATE DOMAIN.

Considerations:
• The delegated admin account will be utilized to manage AD objects, add additional administrators, etc.
• The delegated administrator account resides on the Users container within AD, you can reset its password directly in the ADUC console, but you cannot move the object to another OU.
• The delegated administrator account has access to the following OUs:
o Cloud: Full control
o Cloud Service Objects: very limited update permissions
• When joining a computer to the domain, the AD account will be created under the Cloud > Computers OU, not the default Computers container.
• Service creation can take up to 60 minutes.
• Once creation is finalized, select your domain and click on SET PASSWORD.
• On the Set password window, click CONFIRM.
• On the New password window, copy the password and click DONE.
2.1.6 Create the management instance
• On the navigation menu go to Compute Engine > VM Instances and click Create.

• On the instance creation window, enter a Name and select the Region and Zone.

Considerations:
• Region and zone must match those of your management subnet.
• Scroll down to Boot disk and click Change.
• On the Boot disk window, select the following:
o Operating System: Windows Server
o Version: select your preferred Windows Server version
• Click Select.
• Scroll down and click Management, security, disks, networking, sole tenancy.
• Select Networking and configure the following:
o Shared subnetwork: your management subnet.
o Primary internal IP: Create IP address
o External IP: Create IP address
• Click Done.
• Scroll to the bottom and click Create.
2.1.7 Configure the management VM
• Still on the Compute Engine > VM Instances screen, under Connect, click the arrow and select Set windows password.

Considerations:
• This configures the local windows admin username and password, not a domain user password.
• This process assumes basic windows administration knowledge.
• On the Set new Windows password window, enter a username and click SET.
• On the New Windows password window, copy the password and click CLOSE.
• Connect to the instance via RDP and start by installation the following Windows features from server manager:
o Role Administration Tools
o ADDS and AD LDS Tools
o Active Directory module for Windows PowerShell
o AD DS Tools
o AD DS Snap-Ins and Command-Line Tools
o Group Policy Management Console (GPMC)
o DNS Manager
• Join the instance to the domain and restart it.

Considerations:
• Remember that the Managed Microsoft AD service deployment configures GCP Cloud DNS to forward DNS queries to the managed domain controllers, DNS resolution should automatically work when joining the instance to the domain.
• Optionally, reconnect to the instance and launch any management tool to perform specific configurations like creating OUs, users, configuring DNS records, etc.
2.1.8 Create a GCP service account
• On the navigation menu go to IAM & Admin > Service Accounts, then click CREATE SERVICE ACCOUNT.

Considerations:
• This service account will be utilized to configure a hosting connection in Citrix Cloud.
• On the Service account details screen, enter the following information:
o Service account name: enter a name for your service account
o Service account ID: enter an ID for the service account (can be the same as the name)
o Service account description: optional description
• Click CREATE.

Considerations:
• Service account names must be unique.
• On the Grant this service account access to project screen, click CANCEL.

Considerations:
• Service account permissions will be configured shortly.
• On the Service accounts screen, click the Actions menu (three dots) for your service account and select Create key.

• On the Create private key screen, choose JSON and click CREATE.

Considerations:
• The private key will be downloaded to your computer, we will utilize the contents of the key file when creating a hosting connection in Citrix Cloud.
• On the Private key saved to your computer window, click CLOSE.
• On the navigation menu, go to IAM & Admin > IAM, and click ADD.
• On the Add members screen, enter the following information.
o New members: the new service account.
o Roles:
 Compute Admin
 Storage Admin
 Cloud Build Editor
 Service Account User
 Cloud Datastore User
o Click SAVE.
• Back on the IAM screen, search for the Cloud Build Service Account and click the edit pencil icon on the far right.

• On the Edit permissions page, enter the following information:
o Roles:
 Cloud Build Service Account
 Compute Instance Admin
 Service Account User
o Click SAVE.

2.2 CITRIX COMPONENTS
2.2.1 Create the Cloud Connector VM
• On the VM instances page, repeat steps 2.1.6 and 2.1.7 to create a Windows instance and join it to the domain, this will be utilized as the Citrix Cloud Connector.

Considerations:
• The Cloud Connector instance must be created on the GCP resources subnet.
• On a production deployment, at least 2 Cloud Connectors must be deployed to avoid possible service disruption.
• Once the instance is created, connect to it via RDP and use a web browser to navigate to <https://citrix.cloud.com.>

• Enter your Citrix Cloud credentials and click Sign in.
• Under Domains, click Add New.
• On the Domains tab under Identity and Access Management, click +Domain.
• On the Add a Cloud Connector window click Download.
• Save the cwcconnector.exe file to the instance.
• Right-click the cwcconnector.exe file and select Run as administrator.
• On the Citrix Cloud Connector window, click Sign in.
• On the sign in window, enter your Citrix Cloud credentials and click Sign in.
• When the installation finishes, click Close.

Considerations:
• Cloud Connector installation will take up to 5 minutes.
2.2.2 Create the VDA master image VM
• On the VM instances page, repeat steps 2.1.10 and 2.1.11 to create a Windows instance and join it to the domain to be utilized as the Citrix VDA.

Considerations:
• This process assumes previous Citrix administration knowledge.
• Once the instance is created, connect to it via RDP and use a web browser to navigate to <https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/> and download the latest Citrix VDA version.

Considerations:
• Citrix credentials will be required to download the VDA software.
• Either the LTSR or CR version can be installed.
• Right-click the VDA installer file and select Run as administrator.
• On the Environment page, select Create a master MCS image.
• On the Core Components page, click Next.
• On the Additional Components page, select the components that best apply to your requirements and click Next.

• On the Delivery Controller page, enter the following information:
o Select “Do it manually”
o Enter the FQDN of each Cloud Connector
o Click Test Connection and if you get a green checkmark, click Add
• Click Next.
• On the Features page, check the boxes of the features you wish to enable based on your deployment needs, then click Next.

• On the Firewall page, select Automatically and click Next.
• On the Summary page, ensure all the details are correct and click Install.
• The instance will need to be restarted during installation.
• After the installation finishes, on the Diagnostics page, select the option that best fits your deployment needs and click Next.

• On the Finish page, make sure Restart machine is checked and click Finish.
2.2.3 Create a GCP hosting connection
• Use a web browser to navigate to <https://citrix.cloud.com.>

• Enter your Citrix Cloud credentials and click Sign in.
• On the upper left hamburger menu, click My Services and select Citrix Virtual Apps and Desktops.

• Under Manage, select Full Configuration.
• In Citrix Studio, navigate to Citrix Studio > Configuration > Hosting and select Add Connection and Resources.

• On the Connection page, click the radio button next to Create a new connection and enter the following information:
o Connection type: Google Cloud Platform
o Service account key: Enter the contents of your GCP service account key created earlier
o Service account ID: will be auto populated when entering the service account key
o Zone name: Select your Citrix zone
o Connection name: enter a name
o Create virtual machines using: Studio tools (Machine Creation Services)
• Click Next.

Considerations:
• Citrix Studio Zones are created when you create a resource location and install at least one Cloud Connector.
• On the Region page, ensure the appropriate GCP project was populated, select the region where your Cloud Connector and VDA were deployed and click Next.

• On the Region page, enter a name for the resources, select the appropriate Virtual Network and Subnet and click Next.

• On the Scopes page, select the appropriate scope and click Next.

Considerations:
• A scope represents a collection of objects. Scopes are used to group objects in a way that is relevant to your organization. Objects can be in more than one scope.
• On the Summary page, ensure all the information is correct and click Finish.
2.2.4 Create a Machine Catalog
• In Citrix Studio, navigate to Citrix Studio > Machine Catalogs and select Create Machine Catalog.

• On the Introduction page, click Next.
• On the Operating System page, select Multi-session OS and click Next.

Considerations:
• At the moment of this writing, Citrix does not support deploying Windows desktop OS catalogs to GCP. This
• On the Machine Management page, select the following information:
o The machine catalog will use: machine that are powered managed
o Deploy machines using: Citrix Machine Creation Services (MCS)
o Resources: select your GCP hosting connection
• Click Next.
• On the Master Image page, select the master image, the functional level (VDA version), and click Next.

• On the Virtual Machines page, configure the number of virtual machines to deploy and click Next.

Considerations:
• For GCP, CPU and RAM for the machines created by MCS will be the same as the master image. The master image will be utilized to create an instance template in GCP.
• Catalog machines will be deployed without a public IP address on GCP.
• On the Active Directory Computer Accounts page, configure the following:
o Account option: Create new AD accounts
o Domain: select your domain
o OU: the OU where the computer accounts will be stored
o Naming scheme: naming convention to be utilized

Considerations:
• Pound signs will be replaced by numbers on the naming scheme.
• Be mindful of the NetBIOS 15 character limit when creating a naming scheme
• On the Domain Credentials page, click Enter credentials.
• On the Windows Security pop-up, enter your domain credentials and click OK.
• Back on the Domain Credentials page, click Next.
• On the Scopes page, select an scope and click Next.
• On the Summary page, enter a name and description and click Finish.
2.2.5 Create a Delivery Group
• In Citrix Studio, navigate to Citrix Studio > Delivery Groups and select Create Delivery Group.

• On the Getting started page, click Next.
• On the Machines page, select your machine catalog, the number of machines, and click Next.

• On the Users page select an authentication option and click Next.
• On the Applications page, click Add.
• On the Add Applications page, select which applications you wish to publish and click OK.

Considerations:
• Whilst most applications should show through the start menu, you can also optionally add applications manually.
• This step can be skipped if you do not need to publish seamless applications.
• Back on the Applications page, click Next.
• On the Desktops page, click Add.
• On the Add Desktop page, configure the Desktop and click OK.

Considerations:
• This step can be skipped if you do not need to publish full shared desktops.
• Back on the Desktops page, click Next.
• On the Scopes page, select a scope and click Next.
• On the Summary page, enter a name, a description, and click Finish.
