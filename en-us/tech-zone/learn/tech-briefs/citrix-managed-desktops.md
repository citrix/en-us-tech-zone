---
layout: doc
---
# Citrix Managed Desktops

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** Kireeti Valicherla and Swaroop Joseph Varghese

Citrix Managed Desktops is a turnkey Microsoft Azure hosted solution to deliver virtual desktops and apps. You can deliver Windows 10 multi-session desktops, Windows 10 Enterprise single session desktops or Windows Server 2012 R2 and 2016 OS sessions or apps running on any of these using a GUI interface in just a few clicks.

You can extend your on-premises deployment to burst into Azure or provide access to contractors or 3rd party users without having to bring up machines in your own environment. It can also be used to setup training labs or dev test setups that need to brought up on demand.
For companies undertaking a merger or acquisition, supporting the on-boarding of the employees is critical to success. Citrix Managed Desktops can help quickly provide access to the key applications and desktops that are needed to incorporate new employees and keep them productive.

As it is a Desktop-as-a-Service that can be subscribed to on a monthly subscription. Citrix will provide a single bill for the service as well as the Azure resource consumption.

Organizations in the U.S., the E.U., and Asia/Pacific can deploy the VMs in four Azure locations globally: U.S. East, U.S. West, West Europe, and Australia East (and more to come soon). The workload locations combined with [11 global points of presence of Citrix Gateway service in Azure](https://www.citrix.com/blogs/2019/07/02/new-citrix-gateway-pops-now-available-in-india-and-south-africa/) helps optimize the experience of HDX delivery. Once the users reaches the Gateway PoP, then the traffic is redirected to the closest workload location over the superfast Azure backbone.

The standard deployment would be as follows:

<Authentication Flow diagram>

Users connects to their Workspace via their endpoint devices that have the corresponding Citrix Workspace App installed on them (or use the Citrix Workspace App for HTML5 from a browser), by logging into the Workspace URL.

The authentication flows from the user’s device (where the credentials are provided) to the Gateway Service, which validates the same against the Azure Active Directory in the Customer’s Azure subscription. (The identity could also be based on an on-premises Active Directory, in most cases the admin would sync the on-prem AD with the Azure AD using Azure AD Connect.)'

Once the user authenticates the Gateway service redirects the user to the appropriate Workspace. If the user then selects a resource from a Citrix Managed Desktop catalog, the user’s request is routed via the Managed Desktops service and the cloud connector to the appropriate VM. The user is then single signed-on to the VM in Azure and the user is logged in to the session. The session is redirected to the user using the HDX protocol.

## Deployment Scenarios

In order to support multiple topologies, organizations can choose an option for one of many deployment scenarios, divided into two main categories: 
1)	Non-domain joined workloads
2)	Domain joined workloads

## Non-domain joined workloads

In this category, the workloads (i.e. Windows machines running in Azure) are not joined to a domain. This type of deployment is applicable for proof of concept, dev/test setups or contractor desktops or for smaller organizations that have not created an Active Directory at all and use Azure AD identity.

With the user and workload machine identities not being on the same domain/workgroup, we need a way to have the user’s ID be mapped to the machine to allow for user profile mapping etc. This is achieved by using wrapper token that encapsulates user ID token (which could be any ID type such as the Google / Facebook /Azure AD) and uses (Citrix Managed or the organizations) Azure AD or the organization’s AD to create a mapped account for the identity on the workload machine.

The user’s local mapped account is created by using the data that is stored in the Azure AD or organizations AD and the associated password is stored securely. This is done by a privileged service which creates an account for the user on the machine, if this is the first time the user is ever logging onto the machine. When a user authenticates to the Workspace with the preferred identity, the local mapped account’s username password information is retrieved and that is in turn used to login to the workload machine.

In a non-domain joined workload deployment model, there are 3 options for user accounts:

**1)	User accounts in Citrix Managed Azure Active Directory**

In this scenario, the users’ accounts reside in the Azure Active Directory subscription that is created by Citrix for the particular deployment. The user accounts are administered by the admins in the organization via URL (that gives access to Azure AD) available in the Identity and Access Management section of the Citrix Cloud console. Users’ Azure account usernames (managed by the organization) are used to login to the Workspace. This deployment model helps in performing quick PoCs, where the entire environment can be stood up quickly to showcase the ease with which an admin can setup the solution.

<Deployment 1 Diag>

**2)	User accounts in Customer’s Azure Active Directory**

In this scenario, the user accounts are in the Customer’s Azure AD subscription. This scenario is common in Banking, Financial Services and Insurance sector and highly regulated industries where the customer wants to give access to a contractor or a temporary 3rd party user without utilizing the organization’s corporate domain, helping to create a barrier between the contractor environment and employee environment. Multi factor authentication is enabled by leveraging Azure MFA. The administration of the user’s accounts are done by the organization’s Azure AD admins.

<Deployment 2 Diag>

**3)	User accounts in organization’s on-premises Active Directory**

In this scenario, the users’ accounts are in the Customer’s Active Directory within their on-premises datacenter. To establish connectivity between the service and the organization’s AD, a Windows 2012 R2/2016 server virtual machine (generally deployed in an HA pair) is installed in the customer’s in the datacenter, called Citrix Cloud Connector. Software that allows for outbound TCP 443 based connection to Citrix Managed Desktops service is installed on it. In this scenario, the users will not be able to access any profile data and file servers in the company’s on-premises datacenter. Native 2 factor authentication is available using Time-based One Time Password.

<Deployment 3 Diag>

## Domain joined workloads

In this category the workload machines (i.e. Windows machines running in Azure) are joined to the organization’s domain. This enables typical virtual desktop infrastructure use cases, such as centralization of apps and desktops in a few locations, that can be accessed by remote users from anywhere on any device. There are 2 deployment scenarios in this case:

**1)	Domain joined using Azure Active Directory Domain Services and user accounts in organization’s Azure Active Directory**

Here the users’ accounts are in the organization’s Azure Active Directory and the workload machines are joined to the Azure Active Directory Domain Services (AADDS) within the customer’s Azure subscription. For the machines to be able to connect to the AADDS, the customer needs to setup Azure VNet Peering from the network in Citrix Managed Desktops’ Azure subscription to their own Azure network in their subscription. You can administer the user and machine accounts via the organization’s Azure Active Directory.

<Deployment 4 Diag>

**2)	Domain Joined to and users’ accounts in organization’s on premises Active Directory**

Here the users’ accounts are in the organization’s on-premises Active Directory, which is synced with the Azure AD in the customer’s Azure subscription using Azure AD Connect. This allows the user’s identity to be authenticated from the synched Azure AD. For the machines to be able to connect to the on-premises AD, the customer needs to setup Azure VNet Peering from the network in Citrix Managed Desktops’ Azure subscription to their own Azure network in their subscription. An additional connection to the data center for access to profile and app data and file servers is needed, this requires SDWAN or a site-to-site VPN or an Express Route. We recommend the use of SDWAN as this is a more reliable and cost effective solution.

<Deployment 5 Diag>

Now that we have seen the various deployment options, let’s take a look at the other main concepts.

## Image Management

Machine Creation Services (MCS) is used to provision the workload VMs in the console. MCS configures, starts, stops and deletes virtual machines (VMs). MCS uses copies of the master image, called linked clones, to provision virtual desktops quickly and these can be updated easily by updating the image and then using that image as the master for the catalog. At the time of writing this brief, master images available are for Win 10 Multi-session, Win 10 Pro and Windows Server 2012 R2 and 2016.

1)	Windows 10 Pro is the standard single user desktop operating system, it can be used to give access to an entire Windows 10 desktop to a user. The compute resources may or may not be fully consumed by the single user. 

2)	The Windows 10 multi-session is new OS made available in Azure that allows more than one user to login to a Windows 10 machine. This will help with reducing the number of machines that need to be brought up in Azure to serve the same set of users. This also helps with fully utilizing the compute resources of the machines that are deployed. This type of machine does not require RDS CALs for allowing multi user access.

3)	The Windows Server 2016/ 2012 R2 is a server operating system that allows multiple users to connect to a single machine. This can be used to serve applications to users or provide access to desktop sessions (which can be skinned to look like desktop operating system sessions). This is a cheaper option to deliver desktops than option 1. Note: Each user connecting to this machine will require an RDS CAL or RDS SAL

<Master Images Screenshot>

These pre built images just have the operating system and the virtual desktop agent (Citrix software used to manage the system) installed on them. They would not have everything the organization needs to provide a usable desktop to the users.

The admin has the option to import their own image of one of these OSes (with all the organizations apps and configurations done it) or build an image from the console using one of these as the base to get the master image in the desired configuration.

<Build Image Screenshot>
  
 Please go through this [link](https://docs.citrix.com/en-us/citrix-managed-desktops/security.html) to understand how the responsibility of the upkeep is shared between Citrix and the customer. The images created can then be updated and the updated image can be used to spawn new VMs.
 
Each of these templates can be applied to the 4 VM sizes that are available at launch

<Machine Perf Screenshot>
  
As the admin you will choose the VM size based on the workload and the number of users that are expected to connect to each machine. Machines with fewer CPUs and lesser RAM are more suited to lighter workloads and/or for machines that are expected to serve a single session at a time. The machines with larger number of CPUs and more memory can support more intensive workloads and/or support a larger number of sessions simultaneously.

Admins are given two methods of creating the catalogs – Quick Create and Custom create.

**Quick Create** creates a static VM (the data on the VM persists across session launches) from a Citrix Managed Win 10 master image that is joined to the Citrix Managed Azure AD. This catalog will not have any connection to your corporate network (so corporate apps will not be able to access the data that is hosted there). This type of catalog is more suitable for the PoCs. You can only choose the size and number of machines and which region it has to be installed.

<Quick Create Screenshot>
  
 **Custom Create** provides you with a number of options so you get the catalog the way you want it.
 
Admins have the options for the catalog type, the subscription where the VMs are being created, setting up connectivity with the corporate network, the Azure region, the storage type, workload and number of machines in the catalog and which master image to be used as well as the AutoScale settings for the catalog.

<Custom Create Screenshot>
  
Catalog types available are: 
**Multi-session** - these are for Windows 10 Multi-session or Windows Server 2016 OS machines where more than one user is expected to login to a single machine. Giving you benefit of maximizing usage of machine resources and reducing the no of machines needed to serve a particular number of users.

**Static (personal desktops)** – these are for Windows 10 and Windows 2012 R2 or Windows 2016 (server VDI) OS machines that are to be assigned to a specific individual and retain their data and state across reboots. The machine is intended to be used over time by the same user.

**Random (pooled desktops)** – these are for Windows 10 and Windows 2012 R2 or 2016 (server VDI) machines that can be assigned to any user who requests a desktop. These machines are reset to their master image defaults after the session is logged off. So they can be used to deliver a repeatable desktop to the next user that logs in, for example to a shift workers, who need the same environment as the previous shift but not their data.

The options for **Azure Subscription** allow the admin to choose where the VMs are to be located, these could be Citrix Managed Azure subscription or your own Azure subscription (this feature is not yet available). 

The **Network Connection** option allows you to select the Azure VNet-Peer that you have configured to be used to connect the machine into the Azure subscription.

The **Region** option allows you choose one of the 4 Azure regions that we currently support to host the VMs in.

The **Storage** option gives you the capability to choose between Standard Disks (HDDs) or Premium Disks (SSDs).

In the **Select a machine** section, the options for selecting machines is different for Multi-session and single-session catalog types. 
The Multi-session catalog would provide the workload dropdown, where we can estimate the number of sessions each VM can serve. 

The **Workload** option allows you to choose from 4 options, for the workload machine, that give you an idea of the kind of workloads that would be served by each of the machines. They are:

<Workload Options Screenshot>
  
**Light:** The expected workload is light for each user and the machine supports 16 such sessions.

**Medium:** The expected workload is medium for each user and each machine supports 10 such sessions.

**Heavy:** The expected workload is medium for each user and each machine supports 4 such sessions.

**Custom:** option gives you a drop down to choose from the 4 available VM sizes in Citrix Managed Desktops that we discussed earlier.

For Static and Random (single session catalogs) in the **Machine Performance** dropdown, you can choose from the 4 machine sizes discussed earlier.

## VNet Peering

Customers who want to connect their Azure hosted VMs in Citrix’s subscription need to connect them back to their own Azure subscriptions that host Azure AD and/or app and profile data. As we have seen in the deployment scenarios for domain joined workloads the Citrix Managed Azure subscription (resource location) needs to have VNet peering with the organizations Azure subscription (hosts Azure AD). This can be accomplished by using the Network Connections item in the right menu.

<VNet Peering Screenshot>
  
Existing VNet peered networks are listed here and you can add a new VNet peer by clicking + Add Connection and then clicking the link for the Easy setup for Azure customers, where you simply sign in with the Subscription Owner account and provide the following permissions and then the list of networks in the subscription are retrieved. You can choose which of those you need to peer with the Citrix Managed network.

<Network Peering Perms Screenshot>

## AutoScale

With this feature the admin can ensure workload availability for their users while controlling costs by powering machines down with load-based or schedule-based power management, or a combination of both, (when the machines are not expected to be in use). This helps reduce the cost of running the workloads in the cloud and makes sure that there are enough VMs booted up to take care of session launch requests when needed.

<AutoScale Screenshot>
  
We provide the admin the ability to power manage, the machines that are in a catalog based on the time of the day, as well as control the actions that are to be performed on the session that is idle or disconnected and the time out for those actions to occur.

An admin can set the working hours for a catalog (based on time-zone) and then define how many VMs are needed in off-work hours, so that the rest of the VMs can be shutdown to save on Azure consumption. This also works to bring up the required number of machines to serve the session when the work hours is resumed. Resulting in a great experience when user try to login again.

Admin can set timeouts for when idle sessions should be disconnected, logged off and powered off.

The capacity buffer is the percentage of current session demand and the minimum running machines number is used to determine how many machine we would want to keep switched on to service new session requests. These can be set differently for during work-hours and after-hours separately. We also have a power off delay setting to keep machines in a powered on until the time configured in this setting is reached. This ensures that we don’t have machines that are continuously being powered on and powered off due to AutoScale.

A few preset schedules are available for you the admin to use, or you can create a custom one.

## Monitoring

We provide the admin with visibility into their Citrix Managed Desktops deployment via the Monitor tab. This gives admins the ability to know what is going on in the environment, to see consumption patterns, detect which resources are being consumed more than others to balance capacity with demand. Visibility into the number of VMs running at a point in time would guide the configuration of AutoScale settings to have the optimal number of machines available when load is expected to rise or fall. 

The admin can view desktop usage, sessions, and machines.

<Monitoring Machines and Sessions Screenshot>
  
The default view is the Desktop Usage page. This contains a real time active machine and session status with a total number of machines in the selected catalog(s).  The graph below is of the machines and sessions that are active for the selected catalog(s) for the selected time period. If the admin hovers over any of the points in graph, a pop up reveals the counts at the point in time. 
The time ranges available here are 1 day, 1 week, 1 month and 3 months going back from the current time.

<Monitoring Top 10 Screenshot>

There are two graphs below that show the Top 10 frequent users and the Top 10 active catalogs. These can be filtered based on type of catalog (application, multi-session and single session). The time ranges available here are 1 week, 1 month and 3 months going back from the current time.

<Monitoring Report Screenshot>
  
 The admin can also download the Desktop Launch Activity report for the last month for the subscription which results in a csv file.
 
 ## Troubleshoot and Support
 
Admins can use the Troubleshoot and Support section in the right side menu to resolve issues that may arise in their deployments. Issues may occur when you are trying to create a catalog or when your users are trying to access their apps or desktops. You can also open support tickets from here itself.

We provide options for different issue scenarios:

<Troubleshoot Screenshot>
  
In case you run into an issue with your setup, you can use a machine (called a Bastion host, which has a tools pre loaded on it) that can be created in the resource location (for machine creation issues) or RDP into the machine in question (if it’s a session launch issue) to resolve it.
