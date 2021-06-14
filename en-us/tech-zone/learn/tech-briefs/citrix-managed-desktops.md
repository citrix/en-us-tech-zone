---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Kireeti Valicherla, Swaroop Joseph Varghese, Alan Goldman
description: Delivers Windows apps and desktops from Microsoft Azure based on Azure Virtual Desktop. Citrix Virtual Apps and Desktops Standard for Azure offers cloud-based management, provisioning, and managed capacity for delivering virtual apps and desktops to any device.
tz_title: Citrix Virtual Apps and Desktops Standard for Azure
tz_products: citrix-virtual-apps-and-desktops-standard-for-azure;
---
# Tech Brief: Citrix Virtual Apps and Desktops Standard for Azure

Citrix Virtual Apps and Desktops Standard for Azure is a turnkey Microsoft Azure hosted solution to deliver virtual desktops and apps. The admin can deliver Windows 10 multi-session desktops, Windows 10 Enterprise and Windows 7 ESU single session desktops. Also, Windows Server 2008 R2, 2012 R2, 2016, and 2019 OS sessions or apps running on any of the above OS using a GUI interface in just a few clicks.

Admins can extend the organization’s on-premises deployment to burst into Azure. Admins can provide access to contractors or third party users without having to bring up machines in their own environment. It can also be used for setting up training labs or dev test setups that need to be brought up on demand.

For companies undertaking a merger or acquisition, supporting the on-boarding of the employees is critical to success. Citrix Virtual Apps and Desktops Standard for Azure can help quickly provide access to the key applications and desktops that are needed to incorporate new employees and keep them productive.

As it is a Desktop-as-a-Service that can be subscribed to on a monthly subscription. Citrix provides a single bill for the service and the Azure resource consumption, if the organization chooses to use Citrix managed Azure for the workloads.

With this option, organizations in the U.S., the E.U., and Asia/Pacific can deploy the VMs in four Azure locations globally: U.S. East, U.S. West, West Europe, and Australia East (and more to come soon). The workload locations combined with [11 global points of presence of Citrix Gateway service in Azure](https://www.citrix.com/about/trust-center/privacy-compliance.html) helps optimize the experience of HDX delivery. Once the user reaches the Gateway PoP, then the traffic is redirected to the closest workload location over the super fast Azure backbone.

Citrix is now enabling customers and partners with the capability to use their own Azure subscription along with Citrix Virtual Apps and Desktops Standard for Azure. Now customers and partners alike have the flexibility to use any Azure region or VM type in addition to the option of applying reserved instance pricing from Microsoft.

The standard deployment model and authentication flow would be as follows:

[![Citrix Virtual Apps and Desktops Standard for Azure service Authentication Flow](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_1-authentication-flow-diagram.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_1-authentication-flow-diagram-large.png)

Users connect to their Workspace via their endpoint devices that have the corresponding Citrix Workspace app installed on them. Or use the Citrix Workspace app for HTML5 from a browser, by logging into the Workspace URL.

The authentication flows from the user’s device (where the credentials are provided) to the Gateway Service, which validates the same against the Azure Active Directory in the Customer’s Azure subscription. (The identity might also be based on an on-premises Active Directory. Usually the admin would sync the on-prem AD with the Azure AD using Azure AD Connect.)

Once the user authenticates the Gateway service redirects the user to the appropriate Workspace. If the user then selects a resource from a CVAD Standard catalog, the user’s request is routed via the Managed Desktops service and the cloud connector to the appropriate VM. The user is then single signed-on to the VM in Azure and the user is logged in to the session. The session is redirected to the user using the HDX protocol.

## Deployment Scenarios

To support multiple topologies, organizations can choose an option from one of the many deployment scenarios, divided into two main categories:

1)  Non-domain joined workloads
2)  Domain joined workloads

## Non-domain joined workloads

In this category, the workloads (that is, Windows machines running in Azure) are not joined to a domain. This type of deployment is applicable for proof of concept, dev/test setups, or contractor desktops. Also, for smaller organizations that have not created an Active Directory at all and use the Azure AD identity.

With the user and machine identities not being on the same domain/workgroup, we need a way to have the user’s ID be mapped to the machine. The mapping allows for user profile mapping and so on. A wrapper token encapsulates user ID token and uses (Citrix Managed or the organizations) Azure AD or the organization’s AD. This wrapper token is used to create a mapped account for the user identity on the machine.

The user’s local mapped account is created by using the data that is stored in the Azure AD or organization's AD and the associated password is stored securely. This process is done by a privileged service. The service creates an account for the user on the machine, if it's the first time ever the user is logging on to the machine. When a user authenticates to the Workspace with the preferred identity, the local mapped account’s user name password information is retrieved. The retrieved credentials are in turn used to log in to the machine.

In a non-domain joined workload deployment model, there are 3 options for user accounts:

### 1) User accounts in Citrix Managed Azure Active Directory

In this scenario, the users’ accounts reside in the Azure Active Directory subscription that Citrix created for the particular deployment. The user accounts are administered by the admins in the organization via URL (that gives access to Azure AD). The URL is available in the **Identity and Access Management** section of the Citrix Cloud console. Users’ Azure account user names (managed by the organization) are used to log in to the Workspace. This deployment model helps in performing quick PoCs, where the entire environment can be stood up quickly. Used to showcase the ease with which an admin can set up the solution.

[![Deployment Scenario 1](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_2-deployment-scenario-1.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_2-deployment-scenario-1-large.png)

### 2) User accounts in Customer’s Azure Active Directory

In this scenario, the user accounts are in the Customer’s Azure AD subscription. This scenario is common in Banking, Financial Services, and Insurance sector and highly regulated industries. In this sector, the customer wants to give access to a contractor or a temporary third party user without utilizing the organization’s corporate domain, helping to create a barrier between the contractor environment and employee environment. Multi-factor authentication is enabled by using Azure MFA. The administration of the user’s accounts are done by the organization’s Azure AD admins.

[![Deployment Scenario 2](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_3-deployment-scenario-2.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_3-deployment-scenario-2-large.png)

### 3) User accounts in organization’s on-premises Active Directory

In this scenario, the users’ accounts are in the Customer’s Active Directory within their on-premises data center. To establish connectivity between the service and the organization’s AD, a Windows 2012 R2/2016 server virtual machine (deployed in an HA pair) is installed in the customer’s data center, called Citrix Cloud Connector. Software that allows for outbound TCP 443 based connection to Citrix Virtual Apps and Desktops Standard for Azure service is installed on it. In this scenario, the users are not able to access any profile data and file servers in the company’s on-premises data center. Native two-factor authentication is available using Time-based One Time Password.

[![Deployment Scenario 3](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_4-deployment-scenario-3.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_4-deployment-scenario-3-large.png)

## Domain joined workloads

In this category the machines (that is, Windows machines running in Azure) are joined to the organization’s domain. This type of setup enables typical virtual desktop infrastructure use cases, such as centralization of apps and desktops in a few locations, that are accessed by remote users from anywhere on any device. There are 2 deployment scenarios in this case:

### 1) Domain joined using Azure Active Directory Domain Services and user accounts in organization’s Azure Active Directory

Here the users’ accounts are in the organization’s Azure Active Directory and the machines are joined to the Azure Active Directory Domain Services (AADDS) within the customer’s Azure subscription. For the machines to be able to connect to the AADDS, the customer needs to set up Azure VNet Peering from the network in Citrix Virtual Apps and Desktops Standard's Azure subscription to their own Azure network in their subscription. Admins can manage the user and machine accounts via the organization’s Azure Active Directory.

[![Deployment Scenario 4](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_5-deployment-scenario-4.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_5-deployment-scenario-4-large.png)

### 2) Domain Joined to organization’s on-premises Active Directory via Azure Active Directory Domain Services and users’ accounts in organization’s on-premises Active Directory

Here the users’ accounts are in the organization’s on-premises Active Directory. The Active directory is synced with the Azure AD in the customer’s Azure subscription using Azure AD Connect. This setup allows the user’s identity to be authenticated from the synced Azure AD. For the machines to be able to connect to the on-premises AD, the customer needs to set up Azure VNet Peering from the network in Citrix Virtual Apps and Desktops Standard's Azure subscription to their own Azure network in their subscription. Another connection to the data center for access to profile and app data and file servers is needed. The second connection requires SDWAN or a site-to-site VPN or an Express Route. We recommend the use of SDWAN as it is a more reliable and cost effective solution.

[![Deployment Scenario 5](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_6-deployment-scenario-5.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_6-deployment-scenario-5-large.png)

### 3) Domain Joined to and users’ accounts in organization’s on-premises Active Directory

Here both the machines’ and users’ accounts are in the organization’s on-premises Active Directory. The Citrix Virtual Apps and Desktops Standard's Azure subscription (SD-WAN virtual appliance installed) and the customer’s on-premises (SD-WAN branch appliance installed) locations are connected to each other using SD-WAN. These appliances are managed by the customer using the SD-WAN Orchestrator in Citrix Cloud. This deployment is the simplest (as there is no need for syncing the on-prem Active Directory with the customer’s Azure AD) and utilizes the optimizations built into SD-WAN to help ensure that the user gets the best experience possible.

[![Deployment Scenario 6](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_7-deployment-scenario-6.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_7-deployment-scenario-6-large.png)

Now that we have seen the various deployment options, let’s look at the other main concepts.

## Image Management

Machine Creation Services (MCS) is used to provision the workload VMs in the console. MCS configures, starts, stops, and deletes virtual machines (VMs). MCS uses copies of the master image, called linked clones, to provision virtual desktops quickly. These clones can be updated easily by updating the image and then using that image as the master for the catalog. At the time of writing this brief, master images available are for Win 10 multi-session, Win 10, and Windows Server 2012 R2 and 2016.

1)  Windows 10 and Windows 7 ESU are the standard single session desktop operating systems. They are used to give access to an entire Windows desktop to a user. The compute resources may or may not be fully consumed by the single user.

2)  The Windows 10 multi-session is new OS made available in Azure that allows more than one user to log in to a Windows 10 machine. This OS helps with reducing the number of machines that must be brought up in Azure to serve the same set of users. This OS also helps with fully utilizing the compute resources of the machines that are deployed. This type of machine does not require RDS CALs for allowing multi-user access.

3)  The Windows Server 2008 R2 / 2012 R2 / 2016 / 2019 are server operating systems that allow multiple users to connect to a single machine. One of these OSs can be used to serve applications to users or provide access to desktop sessions (which can be skinned to look like desktop operating system sessions). These OSs are a cheaper option to deliver desktops than option 1.
Note: Each user connecting to this machine requires an RDS CAL or RDS SAL.

Note: For Windows 7 ESU and the Windows Server 2008 R2 operating systems, the version of the Virtual Delivery Agent to be installed on the image must be Citrix Virtual Apps and Desktop 7.15 LTSR.

![Master_Images_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_7-master-images-ss.png)

These prebuilt images have just the operating system and the Virtual Delivery Agent (Citrix software used to manage the system) installed on them. They would not have everything the organization needs to provide a usable desktop to the users.

The admin can import their own image of one of these OSes (with all the organizations apps and configurations done it). Or build an image from the console using one of these images as the base to get the master image in the desired configuration.

![Build_Image_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_8-build-your-image.png)

Visit this [link](/en-us/citrix-managed-desktops/security.html) to understand how the responsibility of the upkeep is shared between Citrix and the customer. The images created can then be updated and the updated image can be used to spawn new VMs.

Each of these templates can be applied to the 4 VM sizes that are available at launch

![Machine_Performance_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_9-machine-performance-ss.png)

The admin can choose the **VM size based** on the workload and the number of users expected to connect to each machine. Machines with fewer CPUs and lesser RAM are more suited to lighter workloads or to serve a single session at a time or both options. The machines with a larger number of CPUs and more memory can support more intensive workloads or support a larger number of sessions simultaneously or both.

Admins have a choice of two methods for creating the catalogs – Quick Create and Custom create.

**Quick Create** creates a static VM (the data on the VM persists across session launches) from a Citrix Managed Win 10 master image that is joined to the Citrix Managed Azure AD. This catalog has no connection to the organization's corporate network (so corporate apps can't access the data that is hosted there). This type of catalog is more suitable for the PoCs. The admin can only choose the size and number of machines and which region they are to be brought up in.

![Quick_Create_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_10-quick-create-ss.png)
  
 **Custom Create** provides admins with various options, so the required catalog can be created.

Admins have the options for the catalog type, the subscription where the VMs are being created, setting up connectivity with the corporate network, the Azure region, the storage type, workload, and number of machines in the catalog and which master image to be used in addition to the **Autoscale** settings for the catalog.

![Custom_Create_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_11-custom-create-ss.png)
  
Catalog types available are:
**Multi-session** - This type is for Windows 10 Multi-session or Windows Server 2016 OS machines. More than one user is expected to log in to a single machine. Giving admins the benefit of maximizing usage of machine resources and reducing the no of machines needed to serve a particular number of users.

**Static (personal desktops)** – This type is for Windows 7 ESU, Windows 10 and Windows 2008 R2 / 2012 R2 / 2016 / 2019 (server VDI) OS machines. The machines are to be assigned to a specific individual and retain their data and state across reboots. The machine is intended to be used over time by the same user.

**Random (pooled desktops)** – This type is for Windows 7 ESU, Windows 10 and Windows 2008 R2 / 2012 R2 / 2016 / 2019 (server VDI) machines. The machines can be assigned to any user who requests a desktop. These machines are reset to their master image defaults after the session is logged off. So they can be used to deliver a repeatable desktop to the next user that logs in, for example to a shift worker, who needs the same environment as the previous shift but not their data.

The options for **Azure Subscription** allow the admin to choose where the VMs are to be located. The location might be in the Citrix managed Azure subscription or the organization's Azure subscription.

The **Network Connection** option allows the admin to select the Azure VNet-Peer that they have configured to be used to connect the machine into the Azure subscription.

The **Region** option allows the admin to choose one of the 4 Azure regions that we currently support to host the VMs in.

The **Storage** option gives admins the capability to choose between Standard Disks (HDDs) or Premium Disks (SSDs).

In the **Select a machine** section, the options for selecting machines is different for Multi-session and single-session catalog types.
The Multi-session catalog would provide the workload drop-down list, where we can estimate the number of sessions each VM can serve.

The **Workload** option allows admins to choose from 4 options, for the workload, that gives admins an idea of the kind of workloads that would be served by each of the machines. They are:

![Workload_Options_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_12-workload-options-ss.png)
  
**Light:** The expected workload is light for each user and the machine supports 16 such sessions.

**Medium:** The expected workload is medium for each user and each machine supports 10 such sessions.

**Heavy:** The expected workload is medium for each user and each machine supports 4 such sessions.

**Custom:** option gives admins a drop-down list to choose from the 4 available VM sizes in Citrix Virtual Apps and Desktops Standard for Azure that were discussed earlier.

For Static and Random (single session catalogs) in the **Machine Performance** drop-down list, admins can choose from the 4 machine sizes discussed earlier.

## VNet Peering

Customers who want to connect their Azure hosted VMs in Citrix’s subscription need to connect them back to their own Azure subscriptions that host Azure AD or app and profile data or both. As seen in the deployment scenarios for domain joined workloads the Citrix Managed Azure subscription (resource location) needs to have VNet peering with the organizations Azure subscription (hosts Azure AD). The connection can be made by using the **Network Connections** item in the right menu.

![VNet_Peering_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_13-vnet-peering-ss.png)
  
Existing VNet peered networks are listed here. The admin can add a new VNet peer by clicking + Add Connection. Then click the link for the Easy setup for Azure customers. Simply sign in with the Subscription Owner account and agree to provide the following permissions. The list of networks in the subscription is retrieved and displayed. The admin can choose which of the retrieved connections they need to peer with the Citrix Managed network.

![VNet_Peering_Permissions_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_14-vnet-peering-perms-ss.png)

## User Defined Routes

Some organizations using the Citrix Virtual Apps and Desktops Standard for Azure service can have requirements for outgoing Internet traffic to originate from a known Static IP address. Due to the default routing mechanisms built into Azure, traffic originating from a deployed machine originates from a random set of public IPs. The solution is to configure an Azure Network Virtual Appliance (NVA) that has a statically assigned public IP on the NVA’s WAN interface. For this to work, the machine must be joined to the domain on the Azure subscription of the organization (Domain joined deployment scenarios 1 and 2) and VNet peering is enabled between Citrix Managed Azure and the customer’s Azure subscription. The Azure subscription network must have 2 subnets, one that contains all the Azure resources (call it LAN) and the one containing the external facing (outgoing) IP address (call it WAN). The WAN subnet can have a small network address space as it only is used for external routing.

For the outgoing IP to be static the admin must assign it to an Azure NVA. With the NVA in place there are extra features that are available including but not limited to URL filtering, content / SSL inspection, threat detection such virus scan and so on

Configure the Azure NVA to NAT the LAN IP to the WAN IP. In the example found in this link, a Windows Server 2016 data center edition VM is used for the outgoing IP.
Then add a route in the Citrix Virtual Apps and Desktops Standard for Azure UI, **Network Connections** > **Azure VNet Peering** previously created > **Routes** tab. Add a new route that points to the LAN IP of the router.

![User Defined Routes](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_20-user-defined-routes-ss.png)

Once the route is added it shows up in the listing of routes for the VNet peering.

## Autoscale

With this feature the admin can ensure workload availability for their users while controlling costs. Costs are lowered by powering machines down with load-based or schedule-based power management, or a combination of both (when the machines are not expected to be in use). Autoscale helps reduce the cost of running the workloads in the cloud. And also makes sure that there are enough VMs booted up to take care of session launch requests when needed.

![Autoscale_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_15-autoscale-ss.png)
  
Admins have the ability to power manage the machines that are in a catalog based on the time of the day. Admins can also control the actions that are to be performed on a session that is idle or disconnected and the time-out for those actions to occur.

An admin can set the working hours for a catalog (based on time-zone) and then define how many VMs are needed in off-work hours. Then we can shut down the rest of the VMs to save on the Azure consumption cost. Autoscale also works to bring up the required number of machines to serve the session when the work hours are resumed. Resulting in a great experience when users try to log in again.

Admin can set timeouts for when idle sessions should be disconnected, logged off, and powered off.

The capacity buffer is the percentage of current session demand. The minimum number of running machines is used to determine how many machines we would want to keep switched on to service new session requests. The no of machines to be powered up can be set differently for during work-hours and after-hours. Admins can also have a power off delay to keep machines in a powered on state until the time configured in this setting is reached. The power off delay ensures that machines are not continuously being powered-on and powered off due to Autoscale.

A few preset schedules are available for the admin to use, or the admin can create a custom one.

## Monitoring

Admins are provided visibility into their Citrix Virtual Apps and Desktops Standard for Azure deployment via the **Monitor** tab. Admins gain the ability to know what is going on in the environment, to see consumption patterns. Admins can detect which resources are being consumed more than others to balance capacity with demand. Visibility into the number of VMs running at a point in time guides the configuration of Autoscale settings. Resulting in the optimal number of machines being available when load is expected to rise or fall.

The admin can view desktop usage, sessions, and machines.

![Monitoring_Usage_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_16-monitoring-usage-ss.png)
  
The default view is the Desktop Usage page. The page contains a real time active machine and session status with a total number of machines in one or more selected catalogs. The following graph is of the machines and sessions that are active for one or more selected catalogs for the selected time period. If the admin hovers over any of the points in the graph, a pop-up reveals the counts at the point in time.
The time ranges available here are 1 day, 1 week, 1 month, and 3 months going back from the current time.

![Monitoring_Top_10_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_17-top-10-ss.png)

There are two graphs that show the Top 10 frequent users and the Top 10 active catalogs. These can be filtered based on the type of catalog (application, multi-session, and single session). The time ranges available here are 1 week, 1 month, and 3 months going back from the current time.

![Monitoring_Desktop_Launch_Activity_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_18-desktop-launch-activity-ss.png)

The admin can also download the Desktop Launch Activity report for the last month for the subscription which results in a csv file.

## Troubleshoot and Support

Admins can use the **Troubleshoot and Support** section in the right side menu to resolve issues if they arise in their deployments. Issues can occur when an admin is trying to create a catalog or when users are trying to access their apps or desktops. Admins can also open support tickets from here itself.

We provide options for different issue scenarios:

![TroubleShoot_SS](/en-us/tech-zone/learn/media/tech-briefs_citrix-managed-desktops_19-troubleshoot-ss.png)
  
In case the admin runs into an issue with the setup, the admin can use a machine called a Bastion host, to troubleshoot the issue. The Bastion host has tools preloaded on it. The Bastion host can be created in the resource location (for machine creation issues). Or the admin can RDP into the machine in question (if it’s a session launch issue) to resolve it.

Watch this video to see Citrix Virtual Apps and Desktops Standard for Azure in action: [Tech Insight Video](/en-us/tech-zone/learn/tech-insights/citrix-managed-desktops.html)

To learn more on the best practices, read our [reference architecture for Citrix Virtual Apps and Desktops Standard for Azure](/en-us/tech-zone/design/reference-architectures/citrix-managed-desktops.html)
