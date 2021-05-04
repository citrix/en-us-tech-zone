---
layout: doc
h3InToc: true
contributedBy: JP Alfaro
specialThanksTo: Darren Harding, Selma Wei, Armando Rodriguez, Daniel Lugo
description: Citrix Virtual Apps and Desktops service implementation with Azure Active Directory Domain Services for CSPs Reference Architecture aligns with the use cases described in the CSP Citrix Virtual Apps and Desktops Reference Architecture to provide guidance and implementation steps to use Azure AD Domain Services.
tz_title: Virtual Apps and Desktops service with Google Cloud Platform for Citrix Service Providers
tz_products: citrix-service-providers;
---
# Reference Architecture: Virtual Apps and Desktops service with Google Cloud Platform for Citrix Service Providers

## Architecture

[Azure Active Directory Domain Services](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/overview) is a fully managed Active Directory service on Microsoft Azure. Not to be confused with Azure AD, which is a cloud-based identity and authentication service for Microsoft services, Azure AD Domain Services (ADDS) provides managed domain controllers. Azure ADDS includes enterprise features like domain-join and group policy. While Azure AD leverages modern authentication and authorization protocols like OpenID Connect and OAuth 2.0, Azure ADDS utilizes traditional protocols that rely on Active Directory, like LDAP and Kerberos. Azure AD Domain Services automatically synchronizes identities from Azure AD to your managed AD environment.

Azure ADDS automatically deploys and manages highly available Active Directory domain controllers on your Azure subscription. Domain controller access is restricted, and you can only manage your domain by deploying management instances with Remote Server Administration tools. Additionally, Domain Admin and Enterprise Admin permissions are not available under the managed service. The Azure ADDS instance is deployed directly to a Virtual Network (VNet) within your subscription, resources can be deployed to the same VNet, or in different VNets. If resources are deployed to a different VNet, it must be connected to the Azure ADDS VNet via a VNet peering.

Azure ADDS can be deployed as a [user forest](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/concepts-resource-forest), or a resource forest. For this implementation, we are deploying Azure ADDS as a user forest, without configuring a trust to an external on-premises AD environment. Also, the Citrix Virtual Apps and Desktops service resources are deployed based on our [CSP reference architecture](/en-us/tech-zone/design/reference-architectures/csp-cvads.html).

### Architecture Scenario 1

This deployment scenario implies the following considerations:

*  **Azure AD**:
    *  Shared Azure AD tenant for all customers
*  **Azure ADDS**:
    *  Shared Azure ADDS instance for all customers
*  **Subscriptions**:
    *  Shared Azure subscription for smaller customers
    *  Dedicated Azure subscriptions for larger customers
*  **Network Connectivity**:
    *  VNET Peering from dedicated subscriptions to the shared subscription for Azure ADDS connectivity

[![CSP-Image-001](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_001.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_001.png)

### Architecture Scenario 2

This deployment scenario implies the following considerations:

*  **Azure AD**:
    *  Shared Azure AD tenant for all customers
    *  Dedicated Azure AD tenant for larger customers
*  **Azure ADDS**:
    *  Shared Azure ADDS instance for all customers
*  **Subscriptions**:
    *  Shared Azure subscription for smaller customers
    *  Dedicated Azure subscriptions for larger customers
*  **Network Connectivity**:
    *  VNET Peering from dedicated subscriptions to the shared subscription for Azure ADDS connectivity

[![CSP-Image-002](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_002.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_002.png)

### Architecture Scenario 3

This deployment scenario implies the following considerations:

*  **Azure AD**:
    *  Shared Azure AD tenant for all customers
    *  Dedicated Azure AD tenant for larger customers
*  **Azure ADDS**:
    *  Shared Azure ADDS instance for small customers
    *  Dedicated Azure ADDS instance for larger customers
*  **Subscriptions**:
    *  Shared Azure subscription for smaller customers
    *  Dedicated Azure subscriptions for larger customers
*  **Network Connectivity**:
    *  No VNET peering from dedicated subscriptions to shared subscription

[![CSP-Image-003](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_003.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_003.png)

## Azure Resource Hierarchy

When designing and organizing your Azure subscription resources, take the following resource hierarchy in consideration:

[![CSP-Image-004](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_004.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_004.png)

## Initial Assumptions

### Azure ADDS

*  Azure AD tenant exists
*  Azure subscription exists
*  An Azure AD account with the following permissions is available:
    *  Azure AD: Global Admin
    *  Subscription: Contributor
*  Azure ADDS will be deployed as a standalone user forest, no trust will be configured
*  While it is a possibility, existing AD users will not be synchronized via Azure AD Connect
*  Self-service password reset will be deployed to force password resets for password hash synchronization

### Citrix Cloud

*  A Citrix Cloud subscription is available
*  Citrix Cloud Connector will be deployed
*  VDA master image will be deployed
*  Azure hosting connections will be configured
*  Machine Catalog and Delivery Group will be configured

## Terminology

The following are the most common Azure terms you need to understand, as described in the Azure documentation:

*  **Azure subscriptions:** Azure subscriptions are an agreement with Microsoft to use Azure services. Billing is tied to a subscription based on the resources consumed, and resources cannot be deployed without a subscription. Subscriptions allow you to organize access to resources. Subscription types include trial, pay as you go, Enterprise Agreement, and MSDN, and each one can have a different payment setup. Azure subscriptions must be tied to an Azure AD tenant.
*  **Azure AD:** Azure AD is Microsoft’s cloud-based identity management service for users, groups, and devices. Azure AD is not to be considered a replacement to traditional Active Directory Domain Services, as it does not support LDAP or Kerberos. Multiple Azure subscriptions can be tied to a single Azure AD tenant. Azure AD offers different types of licenses (Free, Premium 1, and Premium 2) which provide different functionality based on the license level.
*  **Management Groups:** Azure Management Groups are containers that allow you to manage access, policy, and compliance across multiple subscriptions. Management groups can contain subscriptions, or other management groups.
*  **Azure RBAC:** Azure RBAC is utilized to manage authorization for Azure resources. Azure RBAC contains over 70 built-in roles and allows you to create custom roles to manage authorization to resources based on your requirements. Permissions are cascaded from management groups to subscriptions, from subscriptions to resources groups, and from resource groups to resources. The Owner RBAC role provides the highest level of permissions over an Azure Resource and also allows you to manage resource permissions for other users.
*  **Azure AD Roles:** Azure AD roles are used to manage Azure AD related actions, like creating users, groups, app registrations, interaction with APIs, and more. The Global Administrator role grants the highest level of authorization in Azure AD, including access to all Azure AD features, manage roles and licensing for other users, and more. The Global Administrator role is automatically assigned to the user who first creates the Azure AD tenant.
*  **Custom Azure AD Domain:** All new Azure AD tenants are created under the onmicrosoft.com domain, custom domains can be configured by validating ownership with your domain registrar.
*  **Resource Groups:** Resource groups are logical containers utilized to organize resources within Azure and manage their permissions via RBAC. Typically, resources within a resource group share a similar lifecycle. A resource group cannot contain other resource groups, and Azure resources cannot be created unless you specify a resource group. While a Resource Group is deployed to an Azure region, it can contain resources from different regions.
*  **VNET:** An Azure VNET is a software defined network that allows you to manage and deploy resources under an isolated address space in Azure. VNETs allow resources to communicate with other resources on the same VNET, the internet, resources in other VNETs, or on-premises. Access to and from VNETs is secured via Network Security Groups and you can also configure routes by implementing User Defined Routes. Azure VNETs are a layer 3 overlays, so they do not understand any layer 2 semantics like VLANs or GARP. All VNETs contain a main address space and must contain at least one subnet with an address space within it. VM IPs in a VNET are not attached to the actual VM instance, they are assigned to the VM NIC, which is managed as an independent resource.
*  **VNET Peering:** A peering allows for 2 VNETs to connect and communicate via the Azure backbone, as opposed to the traditional VNET-to-VNET connection, which routes traffic through the public internet. Peerings allow for low latency and can be configured across different regions, different subscriptions, and even different Azure AD tenants. Peering connections are non-transitive by default, advanced configuration is required to change this behavior. In a hub and spoke architecture, a spoke VNET can only communicate with the hub, but it is unable to communicate with resources in other spokes.
*  **Network Security Group:** A Network Security Group (NSG) is a set of rules that enable you to control inbound and outbound access to resources inside a VNET, they can be attached to a subnet or a NIC. Inbound and outbound rules within a Network Security Group are managed independently, and all rules must have a priority from 100 and 4096. By default, Network Security Groups include a set of default rules that permit traffic between resources in the same VNET, outbound internet access, among others. Network Security Groups have no relationship whatsoever with OS level firewall configurations and as a rule of thumb, a zero-trust approach is recommended when designing your Network Security Groups.
*  **App Registration:** An app registration is an Azure AD account that allows an external application to interact with Azure APIs. When an app registration is created, Azure AD generates an app ID and a secret, which act as a user name and password. In this implementation, an app registration is created to allow Citrix Cloud to interact with Azure and perform machine creation and power management tasks.

## Azure ADDS Considerations

*  Azure ADDS automatically synchronizes user identities from Azure AD
*  Synchronization works from Azure AD to Azure ADDS, not the opposite way
*  It can leverage users created in the Cloud, or users synced via Azure AD Connect
*  Azure AD Connect cannot be installed on an Azure ADDS environment to sync objects back to Azure AD
*  LDAP write functions only work for objects created directly on ADDS, not for users synced from Azure AD
*  Azure ADDS can only be used as a standalone domain (one forest, one domain only), not as an extension of an on-premises domain
*  The service is deployed on Azure Availability Zones where available
*  Azure ADDS is deployed as a user forest by default, at the moment of this writing, the resource forest deployment model is on preview
*  For users synced from Azure AD, the password hash is not synchronized until the users reset their password, Azure Self Service Password Reset is utilized to help users reset their passwords.
*  The AAD DC Administrators group, which is created when the Azure ADDS instance is deployed, cannot be edited inside ADUC. AAD DC Administrators group can only be edited from within Azure AD groups in the Azure console
*  For users synced from Azure AD:
    *  The password cannot be reset from the ADUC console
    *  Cannot be moved to a different OU
    *  These users are typically used to manage the Azure ADDS instance as a CSP, end customer users can be created inside ADUC
*  GPOs can be created and linked to the pre-created AADDC Computers and AADC Users organizational units, not to other pre-created OUs
    *  You can create your own OU structure and deploy GPOs
    *  Domain and Site level GPOs cannot be created
*  OU lockdown is possible by utilizing the Delegation of Control Wizard on new OUs
    *  Does not work on pre-created OUs

## Logon Process Considerations

Azure ADDS synchronizes user accounts from the Azure AD tenant under which is created. It includes accounts created with a custom domain, accounts created with the initial onmicrosoft.com domain, and B2B accounts (external accounts added to Azure AD as guests). Based on the type of user account, users will have a different logon experience:

*  Custom domain accounts:
    *  Login using UPN (user@domain.com): Login successful
    *  Login using NetBIOS (domain\user): Login successful
*  Onmicrosoft domain accounts:
    *  Login using UPN (user@domain.onmicrosoft.com): Login unsuccessful _(1)_
    *  Login using NetBIOS (domain\user): Login successful _(2)_
*  B2B accounts (guests):
    *  Login using UPN (user@domain.com): Login unsuccessful
    *  Login using NetBIOS (domain\user): Login unsuccessful _(3)_

**_NOTE:_**  
_(1)_ Adding an alternate UPN name is not allowed on Azure ADDS, so these users cannot login via UPN.

_(2)_ This works properly because the NetBIOS name is the same for all users.

_(3)_ These users cannot authenticate against Azure ADDs, even though they are synchronized, Azure does not have access to their password hash.

## Implementation

### Azure Components

#### Step 1: Create a Resource Group for Azure ADDS

1- On the Azure portal menu, select Resource Groups, and click **Add**

[![CSP-Image-005](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_005.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_005.png)

**Considerations:**

*  This step assumes an Azure subscription has been created and is ready to deploy the resources.

2- On the Basics tab, enter the following information, and click **Review + Create**

*  Subscription
*  Resource group name
*  Resource group region

[![CSP-Image-006](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_006.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_006.png)

3- On the Review + create tab, click **Create**

[![CSP-Image-007](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_007.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_007.png)

**Considerations:**

*  Repeat these steps to create resource groups for customer resources, networks, and more.
*  Optionally, you can pre-create resource groups for Citrix Machine Creation Services to utilize. Machine Creation Services (MCS) can only utilize empty resource groups.

#### Step 2: Create the Azure ADDS VNet

1- On the Azure portal menu, select **Virtual Networks**, and click **Add**.

[![CSP-Image-008](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_008.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_008.png)

2- On the Basics tab, enter the following information, and click **Next: IP Addresses**:

*  Subscription
*  Resource group name
*  VNET name
*  VNET region

[![CSP-Image-009](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_009.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_009.png)

3- On the IP Addresses tab, enter the following information, and click **Next: Security**:

*  IPv4 address space
*  Add subnets

[![CSP-Image-010](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_010.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_010.png)

**Considerations:**

*  Add subnets as determined by your network design decisions. In this case, we are adding a subnet for the ADDS service, and a subnet for shared infrastructure resources, including Citrix Cloud Connectors, master images, and so forth and so on.

4- On the Security tab, configure **DDoS and Firewall** as required, and click **Review + create**

[![CSP-Image-011](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_011.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_011.png)

5- On the Review + create tab, click **Create**.

[![CSP-Image-012](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_012.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_012.png)

**Considerations:**

*  Repeat these steps to create customer networks, both in the same subscription, or any additional subscription.

#### Step 3: Configure VNet Peerings

1- On the Azure portal menu, select **Virtual Networks**, and select the **VNET** where ADDS will be deployed.

[![CSP-Image-013](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_013.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_013.png)

**Considerations:**

*  For this implementation, networking is designed in a hub and spoke architecture. A VNET peering will be configured from the Azure ADDS network (hub) to the customer networks (spokes).
*  By default, VNET peerings are not transitive, so spoke networks are not able to communicate with each other unless [intentionally configured](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/overview).
*  If peering networks on different Azure subscriptions and Azure AD tenants:
    *  Users must be added as guest users on the opposite subscription and be granted with RBAC permissions to peer networks.
    *  Network Security Groups must be properly configured on both sides.

2- On the VNET blade, click **Peerings** and **Add**.

[![CSP-Image-014](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_014.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_014.png)

3- On the Add peering blade, enter the following information:

*  Name of the peering from the source VNET to the destination VNET
*  Subscription
*  Destination virtual network
*  Name of the peering from the destination VNET to the source VNET

[![CSP-Image-015](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_015.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_015.png)

4- Scroll down and click **OK**

[![CSP-Image-016](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_016.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_016.png)

**Considerations:**

*  Repeat these steps to peer other customer (spoke) networks.

#### Step 4: Create the Azure AD Domain Services instance

1- On the Azure search bar, type **Domain Services**, and click **Azure AD Domain Services**

[![CSP-Image-017](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_017.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_017.png)

2- On the Azure AD Domain Services page, click **+ Add**.

[![CSP-Image-018](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_018.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_018.png)

3- On the Basics tab, enter the following information, and click **Next**:

*  Subscription
*  Resource group name
*  DNS domain name
*  Region
*  SKU
*  Forest type

[![CSP-Image-019](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_019.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_019.png)

**Considerations:**

*  The AAD DS instance region must match that of the network you pre-created on the previous steps.
*  A **User Forest** is the default type of forest on Azure ADDS, they synchronize all Azure AD user accounts to Azure ADDS so that they authenticate against the Azure ADDS instance. This model assumes user password hashes can be synced.
*  A **Resource forest:** is a recently supported type of forest, which is on preview. Under this deployment model, Azure ADDS is used to manage machine accounts. A one-way trust is configured from Azure ADDS (trusting domain) to an on-premises AD environment (the trusted domain). With this configuration, user accounts from the on-premises environment can log in to resources hosted in Azure which are joined to the Azure ADDS domain. This type of forest assumes network connectivity to the on-premises domain is configured.

4- On the Networking tab, enter the following information, and click **Next**:

*  Virtual Network
*  Subnet

[![CSP-Image-020](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_020.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_020.png)

5- On the Administration tab, click **Manage group membership**

[![CSP-Image-021](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_021.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_021.png)

6- On the Members blade, click **+ Add members**

[![CSP-Image-022](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_022.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_022.png)

7- On the Add members blade, search for the accounts that you want to add as members of the AAD DC Administrators group.

[![CSP-Image-023](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_023.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_023.png)

8- Once the users have been added, click **Select**

[![CSP-Image-024](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_024.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_024.png)

9- Back on the Administration tab, click **Next**

[![CSP-Image-025](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_025.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_025.png)

**Considerations:**

*  The AAD DC Administrators group membership can only be managed from Azure AD, it cannot be managed from the ADUC console in the Azure ADDS instance.

10- On the Synchronization tab, click **Next**

[![CSP-Image-026](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_026.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_026.png)

**Considerations:**

*  This page can be optionally utilized to select which Azure AD objects to synchronize to Azure ADDS by selecting the Scoped sync type.

11- On the Review tab, click **Create**

[![CSP-Image-027](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_027.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_027.png)

12- On the confirmation pop-up, click **OK**

[![CSP-Image-028](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_028.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_028.png)

**Considerations:**

*  The process to create the Azure ADDS instance can take up to 1 hour.

#### Step 5: Configure DNS for the Azure ADDS VNET

1- Once the Azure ADDS instance has been created, under **Update DNS server settings for your virtual network**, click **Configure**

[![CSP-Image-029](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_029.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_029.png)

**Considerations:**

*  This step automatically configures the DNS settings of the VNET where the Azure ADDS instance was created (Hub network). Once configured, all DNS queries are forwarded to the managed domain controllers.
*  Customer networks (spokes) must have their DNS settings updated manually.

#### Step 6: Configure DNS for the customer networks

1- On the Azure portal menu, select **Virtual Networks**, and select your customer (spoke) VNET.

[![CSP-Image-030](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_030.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_030.png)

2- On the VNET blade, click **DNS Servers**, select **Custom**, enter the IP address of the managed domain controllers and click **Save**

[![CSP-Image-031](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_031.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_031.png)

**Considerations:**

*  Repeat these steps for every customer (spoke) VNET, and any other external VNET that is peered to the VNET hosting the Azure ADDS instance.

#### Step 7: Configure Self Service Password Reset (SSPR)

1- On the Azure portal menu, select **Azure Active Directory**, and click **Password reset**

[![CSP-Image-032](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_032.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_032.png)

**Considerations:**

*  When Azure AD users are initially synced to Azure ADDS, their password hash is not synced, therefore, users must reset their password for this to occur. SSPR is utilized to allow for users to reset their passwords in a simple and secure manner.
*  User authentication against Azure ADDS does not work until this step is performed.
*  The step to enable SSPR is only required if it has not been previously configured.
*  This step is only required if Azure AD users are being managed from the Azure portal (not users synced from on-prem AD via Azure AD Connect). For users synced from on-prem AD via Azure AD connect, follow these steps.

2- On the Properties blade, select **All**, and click **Save**.

[![CSP-Image-033](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_033.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_033.png)

**Considerations:**

*  You can optionally choose Selected to enable SSPR only to a subset of users.
*  Next time the users login, they will be forced to register to SSPR.

#### Step 8: SSPR User Registration Process

1- When a user logs in, they are redirected to the **SSPR registration** screen and configure their authentication methods.

[![CSP-Image-034](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_034.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_034.png)

**Considerations:**

*  SSPR authentication methods can be selected on the SSPR configuration blade in the Azure portal.
*  For this example, SSPR has been enabled with the basic settings, which requires for a Phone an Email to be configured.

2- Once the users enter their authentication information, the SSPR enrollment process is complete.

[![CSP-Image-035](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_035.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_035.png)

3- Users can now navigate to [Self-Service Password Reset](https://aka.ms/sspr) to reset their password.

[![CSP-Image-036](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_036.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_036.png)

**Considerations:**

*  Once this step is complete and the users reset their password, the password hash is synced from Azure AD to Azure ADDS.
*  For synced users, the ADUC cannot be utilized to reset their password.

#### Step 9: Create the AD management VM

1- On the Azure portal menu, select **Virtual Machines**, and click **Add**

[![CSP-Image-037](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_037.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_037.png)

2- On the Basics tab, enter the following information, and click **Next: Disks**:

*  Subscription
*  Resource group
*  VM Name
*  Region
*  Availability options
*  Image
*  Size
*  Admin account details

[![CSP-Image-038](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_038.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_038.png)

[![CSP-Image-039](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_039.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_039.png)

3- On the Disks tab, enter the **OS Disk Type**, and click **Next: Networking**

[![CSP-Image-040](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_040.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_040.png)

4- On the Networking tab, configure the following information, and click **Next: Management**:

*  Virtual network
*  Subnet
*  Public IP (if applicable)
*  Network security group

[![CSP-Image-041](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_041.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_041.png)

5- On the Management tab, configure the following information, and click **Next: Advanced**:

*  Monitoring
*  Auto-shutdown
*  Backup

[![CSP-Image-042](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_042.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_042.png)

[![CSP-Image-043](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_043.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_043.png)

6- On the Advanced tab, leave the default settings, and click **Next: Tags**

[![CSP-Image-044](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_044.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_044.png)

7- On the Tags tab, create any required tags for the VM instance, and click **Next: Review + create**

[![CSP-Image-045](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_045.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_045.png)

8- On the Review + create tab, make sure all information is correct, and click **Create**

[![CSP-Image-046](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_046.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_046.png)

**Considerations:**

*  Repeat the previous steps to create all additional VMs: Cloud Connectors, Master Images, and so forth and so forth and so on.

#### Step 10: Join the Management VM to the domain

1- Connect to the instance via RDP and open Server Manager, and click **Add Roles and Features**

[![CSP-Image-047](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_047.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_047.png)

2- On the Add Roles and Features Wizard, add the following features:

*  Role Administration Tools
*  ADDS and AD LDS Tools
*  Active Directory module for Windows PowerShell
*  AD DS Tools
*  AD DS Snap-ins and Command-Line Tools
*  Group Policy Management Console (GPMC)
*  DNS Manager

[![CSP-Image-048](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_048.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_048.png)

3- When the installation finishes, join the VM to the Azure ADDS domain.

[![CSP-Image-049](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_049.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_049.png)

**Considerations:**

*  Repeat the previous steps to join all other VMs to the Azure ADDS domain.
*  RSAT tools installation is only required for the VMs used to manage the Azure ADDS instance.
*  Make sure the password of the user account utilized to join the VMs to the Azure ADDS domain has been reset before attempting these steps.

#### Step 11: Create an Azure AD App Registration

1- On the Azure portal menu, select **Azure Active Directory > App registrations > + New registration**

[![CSP-Image-050](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_050.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_050.png)

2- On the Register an application blade, enter the following information, and click **Register**:

*  App name
*  Supported account types
*  Redirect URL
    *  Web
    *  "https://citrix.cloud.com"

[![CSP-Image-051](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_051.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_051.png)

3- On the Overview blade, copy the following values to a notepad:

*  Application (client) ID
*  Directory (tenant) ID

[![CSP-Image-052](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_052.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_052.png)

**Considerations:**

*  The application ID and Directory ID values will be utilized later on when creating a hosting connection for Citrix MCS to manage Azure resources.

4- Click on **Certificates & secrets** and then **+New client secret**

[![CSP-Image-053](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_053.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_053.png)

5- On the Add a client secret pop-up, enter a **Description and Expiration**, and click **Add**

[![CSP-Image-054](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_054.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_054.png)

6- Back on the Certificates & secrets screen, copy the value of the client secret

[![CSP-Image-055](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_055.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_055.png)

**Considerations:**

*  While the Client ID acts as a user name for the app registration, the Client Secret acts as the password.

7- Click on **API permissions** and then **Add a permission**

[![CSP-Image-056](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_056.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_056.png)

8- On the Request API permissions blade, under **APIs my organization uses** search for **Windows Azure**, and select **Windows Azure Active Directory**

[![CSP-Image-057](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_057.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_057.png)

9- On the Azure Active Directory Graph API blade, select **Delegated Permissions**, assign the **Read all users’ basic profiles** permission, and click **Add permissions**

[![CSP-Image-058](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_058.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_058.png)

10- Back on the Request API permissions blade, under **APIs my organization uses** search for **Windows Azure** again, and select **Windows Azure Service Management API**

[![CSP-Image-059](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_059.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_059.png)

11- On the Azure Service Management API blade, select **Delegated Permissions**, assign the **Access Azure Service Management as organization users** permission and click **Add permissions**

[![CSP-Image-060](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_060.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_060.png)

12- On the Azure portal menu, click **Subscriptions** and copy the value of your **Subscription ID**

[![CSP-Image-061](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_061.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_061.png)

**Considerations:**

*  Copy the value of all subscriptions utilized to manage resources via Citrix MCS. The hosting connection for each Azure subscription must be configured independently.

13- Select your subscription, and select **Access Control (IAM) > +Add > Add role assignment**

[![CSP-Image-062](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_062.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_062.png)

14- On the Add role assignment blade, assign the **Contributor** role to the new app registration, and click **Save**

[![CSP-Image-063](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_063.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_063.png)

**Considerations:**

*  Repeat this step to add Contributor permissions to the registration on any additional subscription.
*  If utilizing a secondary subscription belonging to a separate Azure AD tenant, a new app registration must be configured.

### Citrix Components

#### Step 1: Install the Cloud Connector

1- Connect to the Cloud Connector VM via RDP and use a web browser to navigate to [Citrix Cloud](https://citrix.cloud.com). Enter your Citrix Cloud credentials and click **Sign in**

[![CSP-Image-064](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_064.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_064.png)

2- Under Domains, click **Add New**

[![CSP-Image-065](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_065.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_065.png)

3- On the Domains tab under Identity and Access Management, click **+Domain**

[![CSP-Image-066](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_066.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_066.png)

4- In the Add a Cloud Connector window click **Download**

[![CSP-Image-067](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_067.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_067.png)

5- Save the cwcconnector.exe file to the instance.

[![CSP-Image-068](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_068.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_068.png)

6- Right-click the **cwcconnector.exe** file, and select **Run as administrator**

[![CSP-Image-069](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_069.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_069.png)

7- On the Citrix Cloud Connector window, click **Sign in**

[![CSP-Image-070](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_070.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_070.png)

8- On the sign-in window, enter your **Citrix Cloud** credentials, and click **Sign in**

[![CSP-Image-071](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_071.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_071.png)

9- When the installation finishes, click **Close**

[![CSP-Image-072](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_072.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_072.png)

**Considerations:**

*  Cloud Connector installation can take up to 5 minutes.
*  At a minimum, 2 Cloud Connectors must be configured per resource location.

#### Step 2: Configure the VDA Master Image

1- Connect to the Citrix VDA master image VM via RDP and use a web browser to navigate to [Citrix Downloads](https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/) and download the latest **Citrix VDA version**

[![CSP-Image-073](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_073.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_073.png)

**Considerations:**

*  Citrix credentials are required to download the VDA software.
*  Either the LTSR or CR version can be installed.
*  A separate VDA installer must be downloaded for Server and Desktop OS machines.

2- Right-click the VDA installer file, and select **Run as administrator**

[![CSP-Image-074](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_074.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_074.png)

3- On the Environment page, select **Create a master MCS image**

[![CSP-Image-075](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_075.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_075.png)

4- On the Core Components page, click **Next**

[![CSP-Image-076](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_076.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_076.png)

5- On the Additional Components page, select the components that best apply to your requirements, and click **Next**

[![CSP-Image-077](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_077.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_077.png)

6- On the Delivery Controller page, enter the following information, and click **Next**:

*  Select “Do it manually”
*  Enter the FQDN of each Cloud Connector
*  Click **Test Connection** and then **Add**

[![CSP-Image-078](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_078.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_078.png)

7- On the Features page, check the boxes of the features you want to enable based on your deployment needs, then click **Next**

[![CSP-Image-079](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_079.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_079.png)

8- On the Firewall page, select **Automatically**, and click **Next**

[![CSP-Image-080](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_080.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_080.png)

9- On the Summary page, ensure all the details are correct, and click **Install**

[![CSP-Image-081](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_081.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_081.png)

10- The VM will be restarted during installation

[![CSP-Image-082](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_082.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_082.png)

11- After the installation finishes, on the Diagnostics page, select the option that best fits your deployment needs, and click **Next**

[![CSP-Image-083](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_083.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_083.png)

12- On the Finish page, make sure **Restart machine** is checked, and click **Finish**

[![CSP-Image-084](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_084.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_084.png)

#### Step 3: Create an Azure Hosting Connection

1- On the Citrix Cloud hamburger menu, navigate to **My Services > Virtual Apps and Desktops**

[![CSP-Image-085](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_085.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_085.png)

2- Under Manage, select **Full Configuration**

[![CSP-Image-086](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_086.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_086.png)

3- In Citrix Studio, navigate to **Citrix Studio > Configuration > Hosting**, and select **Add Connection and Resources**

[![CSP-Image-087](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_087.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_087.png)

4- On the Connection page, click the radio button next to **Create a new connection**, enter the following information, and click **Next**:

*  Connection type
*  Azure environment

[![CSP-Image-088](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_088.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_088.png)

5- On the Connection Details page, enter the following information, and click **Use Existing**:

*  Subscription ID
*  Connection name

[![CSP-Image-089](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_089.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_089.png)

6- On the Existing Service Principal page, enter the following information, and click **OK**:

*  Active Directory ID
*  Application ID
*  Application Secret

[![CSP-Image-090](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_090.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_090.png)

7- Back on the Connection Details page, click **Next**

[![CSP-Image-091](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_091.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_091.png)

8- On the Region page, select the region where your Cloud Connector and VDA were deployed, and click **Next**

[![CSP-Image-092](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_092.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_092.png)

9- On the Network page, enter a name for the resources, select the appropriate Virtual Network and Subnet, and click **Next**

[![CSP-Image-093](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_093.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_093.png)

10- On the Summary page, ensure all the information is correct, and click **Finish**

[![CSP-Image-094](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_094.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_094.png)

#### Step 4: Create a Machine Catalog

1- In Citrix Studio, navigate to **Citrix Studio > Machine Catalogs**, and select **Create Machine Catalog**

[![CSP-Image-095](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_095.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_095.png)

2- On the Introduction page, click **Next**

[![CSP-Image-096](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_096.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_096.png)

3- On the Operating System page, select the appropriate OS, and click **Next**

[![CSP-Image-097](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_097.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_097.png)

**Considerations:**

*  Subsequent screens will slightly vary depending on the OS type selected in this page.

4- On the Machine Management page, select the following information, and click **Next**:

*  The machine catalog will use: machines that are powered managed
*  Deploy machines using: Citrix Machine Creation Services (MCS)
*  Resources: select your Azure hosting connection

[![CSP-Image-098](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_098.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_098.png)

5- On the Desktop Experience page, select the options that best adjust to your requirements, and click **Next**

[![CSP-Image-099](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_099.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_099.png)

6- On the Master Image page, select the master image, the functional level (VDA version), and click **Next**

[![CSP-Image-100](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_100.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_100.png)

7- On the Storage and License Types, select the options that best adjust to your requirements, and click **Next**

[![CSP-Image-101](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_101.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_101.png)

8- On the Virtual Machines page, configure the number of virtual machines to deploy, the machine size, and click **Next**

[![CSP-Image-102](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_102.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_102.png)

9- On the Write Back Cache page, select your write cache options, and click **Next**

[![CSP-Image-103](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_103.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_103.png)

10- On the Resource Groups page, select between creating new resource groups for the Citrix MCS resources or using pre-created resource groups.

[![CSP-Image-104](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_104.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_104.png)

**Considerations:**

*  Only empty resource groups appear on the list of existing resource groups.

11- On the Network Interface Cards page, add **NICs** as required, and click **Next**

[![CSP-Image-105](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_105.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_105.png)

12- On the Active Directory Computer Accounts page, configure the following options, and click **Next**:

*  Account option: Create new AD accounts
*  Domain: select your domain
*  OU: the OU where the computer accounts will be stored
*  Naming scheme: naming convention to be utilized

[![CSP-Image-106](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_106.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_106.png)

**Considerations:**

*  Numbers will replace the pound signs on the naming scheme
*  Be mindful of the NetBIOS 15-character limit when creating a naming scheme

13- On the Domain Credentials page, click **Enter credentials**

[![CSP-Image-107](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_107.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_107.png)

14- On the Windows Security pop-up, enter your domain credentials, and click **OK**

[![CSP-Image-108](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_108.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_108.png)

15- On the Summary page, enter a name and description, and click **Finish**

[![CSP-Image-109](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_109.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_109.png)

#### Step 5: Create a Delivery Group

1- In Citrix Studio, navigate to **Citrix Studio > Delivery Groups**, and select **Create Delivery Group**

[![CSP-Image-110](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_110.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_110.png)

2- On the Getting started page, click **Next**

[![CSP-Image-111](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_111.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_111.png)

3- On the Machines page, select your machine catalog, the number of machines, and click **Next**

[![CSP-Image-112](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_112.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_112.png)

4- On the Users page select an authentication option, and click **Next**

[![CSP-Image-113](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_113.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_113.png)

5- On the Applications page, click **Add**

[![CSP-Image-114](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_114.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_114.png)

6- On the Add Applications page, select which applications you want to publish, and click **OK**

[![CSP-Image-115](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_115.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_115.png)

**Considerations:**

*  While most applications will show through the start menu, you can also optionally add applications manually.
*  This step can be skipped if you do not need to publish seamless applications.

7- Back on the Applications page, click **Next**

[![CSP-Image-116](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_116.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_116.png)

8- On the Desktops page, click **Add**

[![CSP-Image-117](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_117.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_117.png)

9- On the Add Desktop page, configure the Desktop, and click **OK**

[![CSP-Image-118](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_118.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_118.png)

**Considerations:**

*  This step can be skipped if you do not need to publish full desktops.

10- Back on the Desktops page, click **Next**

[![CSP-Image-119](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_119.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_119.png)

11- On the Summary page, enter a name, a description, and click **Finish**

[![CSP-Image-120](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_120.png)](/en-us/tech-zone/design/media/reference-architectures_csp-cvads-aad_120.png)
