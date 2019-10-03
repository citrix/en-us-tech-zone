---
layout: doc
---
# Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud

## Contributors

**Author:** [Vivekananthan Devaraj](Vivekananthan.Devaraj@Citrix.com)

**Special thanks:**  [Allen Furmanski](https://twitter.com/tekguyallen), [Harsh Gupta](harsh.gupta@citrix.com), [Nitin Mehta](nitin.mehta@citrix.com) & [Victor Cataluna](victor.cataluna@citrix.com)

## Audience

This document is intended for Citrix technical professionals, IT decision-makers, partners, and security consultants who want to explore the migration strategies from a customer-hosted on-premises environment to the Virtual Apps and Desktops Service on Citrix Cloud. The reader should have a basic understanding of Citrix products, security, and Citrix Cloud frameworks. For more information on Citrix Cloud and its services, see the official product documentation for [Citrix Cloud](/en-us/citrix-cloud.html).

## Objective of this document

The purpose of this document is to describe a set of procedures to migrate an on-premises Citrix Virtual Apps and Desktops environment to the Citrix Cloud Virtual Apps and Desktops Service (CVADS). It is intended for an audience planning to migrate the Control Layer components from a local data center, private, or public cloud to Citrix Cloud. The actual resources that users connect to (Virtual Delivery Agents) can remain on-premises and/or in a public cloud.

## Prerequisites and Assumptions

It is important that the reader has background knowledge on Citrix FlexCast Management Architecture (FMA), Citrix Cloud Architecture, design concepts and terminologies to execute the migration strategies. As a prerequisite, Citrix strongly recommends reviewing the [product documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service.html) and [reference architecture](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) materials to gain adequate knowledge in preparation for the migration approaches.

## Planning and Design

At a high-level, the solution is based on a unified and standardized layer model.

*  User Layer – Defines the unique user groups, endpoints, and locations.
*  Access Layer – Defines how a user group gains access to the resources.
*  Resource Layer – Defines the applications, desktops, and data provided to each user group.
*  Control Layer – Defines the underlying infrastructure components required to securely access the resources.
*  Platform Layer – Defines the hardware components, a private, public, and hybrid cloud that will be used for the Citrix environment.
*  Operations Layer – This layer explains the procedures and tools that support the core product and solution.

As part of the planning strategy, assess the existing Citrix environment and prepare a new design with Citrix Cloud.

Review and capture the existing configuration details for

*  User Layer
    *  User Types and Groups
    *  Citrix Workspace app or Receiver version on endpoints
    *  Combability of Citrix Workspace app or Receiver on thin clients
    *  TLS 1.2 support
*  Access Layer
    *  Authentication point and provider
    *  StoreFront configurations
    *  Resource aggregation sites and groups
    *  Citrix Gateway Configuration for each site or data center
    *  HDX optimal gateway routing
*  Control Layer
    *  Number of Citrix Apps and Desktops sites or data centers
    *  Active Directory Functional Level
    *  Active Directory Forest / Domain structure
    *  Authentication flow and methods
    *  Federated Authentication Service configuration (if any)
    *  Delivery Controller configuration
    *  Citrix administrators and their roles for each Site
    *  Citrix HDX policies and assignments
    *  Local Host Cache Configurations
    *  Power Management policies
*  Resource Layer
    *  Golden image(s) for each Machine Catalog
    *  Types of Machine Catalogs and Delivery Groups
    *  Tagging details
    *  Published application and desktop details
    *  User or AD Group mapping details for published applications and desktops
    *  Provisioning methods
    *  Zone mappings
    *  App Groups
*  Platform Layer
    *  Hosting connections and respective credentials
    *  Third-party cloud hosting connections (Azure, AWS, and GCP)
    *  Network / Traffic Flow between sites or data centers
*  Operations Layer
    *  Required reports from Citrix Director
    *  Existing backup/disaster recovery approach

## A Sample Customer Use Case

An enterprise organization that has traditional on-premises Citrix Virtual Apps and Desktops environment to enable secure remote access to internal resources for their end-users. Users access this traditional environment by accessing the On-Premises Gateway URL for example, `https://CVAD.Customer.com`. This environment is due for a tech refresh since the hardware is five years old and the Citrix components are running with legacy version. The following diagram depicts the existing on-premises deployment

[![cloud-migration-strategies-Image-1](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_001.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_001.png)

The CTO of the organization decides to upgrade and migrate the environment to Citrix Cloud by moving the Control Infrastructure and Access Layer components to Citrix Cloud, keeping the workloads within their on-premises data center.

Let’s study the different strategies in which customers can migrate their on-premises environment to Citrix Cloud:

## Migration Design and Strategies

## #1 – Migrating the Control and Access Layer components to Citrix Cloud, integrating with Citrix Gateway Service

In this approach, the Control Layer components named Delivery Controllers, Database Servers, License Servers, and the Access Layer components named StoreFront Servers and Gateway are provisioned on Citrix Cloud and integrated with Citrix Gateway Service. The Cloud Connectors are deployed at customer-hosted locations known as resource locations. Users can securely access the Citrix Cloud environment through the Citrix Gateway Service from anywhere. The Gateway Service provides a secure remote access solution with diverse identity and access management capabilities to deliver a unified experience for apps and desktops. After the migration, the users need to access the Citrix Cloud Workspace URL for example, `https://examplecustomer.cloud.com` to access the Citrix environment.

[![cloud-migration-strategies-Image-2](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_002.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_002.png)

## #2 - Migrating the Control Layer components integrating with On-Premises StoreFront and Gateway

In this approach, the Control Layer components are migrated to Citrix Cloud, but the Access Layer components remain at the on-premises environment. This approach provides extended access utilizing existing StoreFront and Citrix Gateway deployments to retain their access methodology. Users access the environment through on-premises Gateway and StoreFront which communicate with Citrix Cloud for resource enumeration. After the migration, users can access the Citrix environment with the existing On-Premises Gateway URL `https://CVAD.Customer.com`.

[![cloud-migration-strategies-Image-3](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_003.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_003.png)

## #3 - Migrating the Control Layer components and Storefront Services to Citrix Cloud and integrating Citrix Workspace with On-Premises Gateway

In this approach, the Control Layer components and StoreFront services are migrated to Citrix Cloud and Citrix Workspace is configured with On-Premises Gateway for accessing the workloads. This approach provides extended and optimal access utilizing existing On-Premises Citrix Gateway deployments to enable remote access for the workloads. Users access the environment through Citrix Workspace URL for the resource enumeration. When users launch a desktop or application, the ICA connection is then established via the on-premises Gateway to the workloads(VDA). After the migration, the users need to access the Citrix Cloud Workspace URL for example, `https://examplecustomer.cloud.com` to access the Citrix environment and the On-Premises Gateway for ICA proxy.

[![cloud-migration-strategies-Image-4](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_004.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_004.png)

## Migration Overview

Let’s go through the migration steps involved in all three migration strategies.

As a pre-requisite, customers must ensure the network connectivity to establish communication from the customer hosted on-premises, public, or private cloud environments to Citrix Cloud.  Please refer to the [Citrix documentation](https://docs.citrix.com/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html) for complete requirements.

### Migration steps for Strategy #1 - Migrating the Control and Access Layer components to Citrix Cloud, integrating with Citrix Gateway Service

[![cloud-migration-strategies-Image-2](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_002.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_002.png)

1.  Login to your Citrix Cloud subscription at `https://citrix.cloud.com` and select the region for Citrix to provision the control layer components and Workspace on the selected region.

1.  Create a new resource location – A customer-hosted location where Cloud Connectors are installed and contain the workloads and resources required for users
    *  Install the Cloud Connectors at each customer-hosted location.
    *  Citrix strongly recommends installing two or more Cloud Connectors at each site. Refer to the sizing guide to deploy the required number of Cloud Connectors at each customer-hosted location.
    *  Refer to the Cloud Connector Technical Details for the system requirements and deployment scenario for different types of Active Directory implementations.
    *  Configure Primary Resource Location - a resource location that you designate as “most preferred” for communications between your domain and Citrix Cloud that has the best performance and connectivity to your domain.
    *  In the Citrix Virtual Apps and Desktops Service, zones are created automatically when you create a resource location and add a Cloud Connector to it. Zones help to logically organize the structure of a site and allow policies to control how users connect.
    *  It is recommended to conduct a failover test for Cloud Connectors on each domain and resource location to ensure stability during one of the Cloud Connector failures.

1.  Secure Cloud Connector Communication - Configure SSL on Cloud Connectors to secure XML traffic from the StoreFront server [CTX221671](https://support.citrix.com/article/CTX221671).

1.  Identity and Access Management - define the identity providers and accounts used for administrators and subscribers to Citrix Cloud and its offerings
    *  Configure the identity provider, for example, your on-premises Active Directory or Azure Active Directory. Refer to [Citrix Documentation](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html) for more information.
    *  Validate the authentication on each identity provider by accessing the Citrix Cloud Page or Workspace URL.
    *  Create and Download the Secure Client Key - Secure clients can be used to authenticate with the Citrix Cloud APIs and manage the cloud services. This enables administrators to create fully automated scripts and scheduled tasks.
    *  Add new cloud administrator accounts for the administration of Citrix Cloud Services.
    *  Customize the administrator roles with delegated permissions.

1.  Zone Creation and Mappings
    *  When you create a resource location in Citrix Cloud and then add a Cloud Connector to that resource location, the Citrix Virtual Apps and Desktops Service automatically create a zone.
    *  You can place host connections, machine catalogs, users, and applications in a zone. A zone can also contain Citrix Gateway and StoreFront servers.
    *  Placing items in a zone affects how the service interacts with them and with other objects related to them.
    *  After you create more than one resource location (and the zones are created automatically), you can move resources from one zone to another. This flexibility comes with the risk of separating items that work best in proximity. For example, moving a catalog to a different zone than the connection (host) that creates the machines in the catalog, can affect performance. So, consider potential unintended effects before moving items between zones. Keep a catalog and the host connection it uses in the same zone.

1.  Configure the hosting connections
    *  Export the existing hosting connections from customer hosted locations.
    *  Import or manually create the hosting configurations into Citrix Cloud Studio.
    *  PowerShell scripts with SDK commands can be used to export the existing hosting connections to import them on Citrix Cloud.
    *  Each hosting connection gets associated with a zone by default.
    *  When a hypervisor connection is added to a zone, all the virtual machines on hypervisors managed through that connection also reside in that zone.

1.  Create Machine Catalogs
    *  Before you create an MCS or PVS-based catalog, you first use the hypervisor or cloud service tools to create and configure the master image. This process includes installing a Virtual Delivery Agent (VDA) on the image. Then you create the machine catalog in the Studio management console. You can also point to the existing master image on the hypervisor to create the machine catalog.
    *  You can also use your own tools to provision machines. If your machines are already created using your own tools, you must still create one or more machine catalogs for those machines.
    *  Citrix Cloud Studio guides you to create the first machine catalog and uses a default functional level that can be earlier than the most current.
    *  When you create a catalog of VMs, you need to specify how to provision those VMs. You can use Citrix tools such as Machine Creation Services (MCS) or Citrix Provisioning (PVS).
        *  If you use Citrix Provisioning to create machines, see the [Citrix Provisioning documentation](https://docs.citrix.com/en-us/provisioning.html) for instructions.
        *  If you use MCS to provision VMs, you provide a master image (or snapshot) to create identical VMs in the catalog using the hypervisor or cloud service tools. You select that image (or a snapshot of an image), specify the number of VMs to create in the catalog, and configure additional information.
        *  Refer to the [Citrix documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html) which gives more detailed information on how to create a master image and machine catalog on the hypervisor or cloud service.
    *  When a machine catalog is placed in a zone, it is assumed that all VDAs in the catalog are in the same zone. If you move a machine catalog to another zone (using Studio), the VDAs in that catalog re-register with Cloud Connectors in the zone where you moved the catalog. When you move a catalog, ensure you also move any associated host connection to the same zone.

1.  Create Delivery Groups
    *  Creating a Delivery Group is the next step in your migration after creating a machine catalog.
    *  At least one machine must remain unused in a selected machine catalog.
    *  A Delivery Group can use machines from more than one catalog; however, those catalogs must contain the same machine types (Server OS, Desktop OS, or Remote PC Access). In other words, you cannot mix machine types in a Delivery Group. Similarly, if your deployment has catalogs of Windows machines and catalogs of Linux machines, a Delivery Group can contain machines from either OS type, but not both.

1.  Publish Applications and Desktops
    *  When you create a Delivery Group, a page appears to choose the delivery type. Choose either Applications or Desktops if you chose the machine catalog containing single session desktop OS machines.
    *  If you selected machines from a server OS or desktop OS random (pooled) catalog, the delivery type is assumed to be applications and desktops. You can deliver applications, desktops, or both.
    *  PowerShell scripts with SDK commands can be used to export the existing on-premises applications to import them in Citrix Cloud with respective Delivery Groups.
    *  To configure a mandatory home zone for a user, on the Delivery Group Wizard users page, select the “Sessions must launch in a user’s home zone” checkbox. All sessions launched by a user in that Delivery Group must launch from machines in that home zone. If a user in the Delivery Group does not have a configured home zone, this setting has no effect.
    *  An application’s home zone is specified in the application’s properties. You can configure application properties when you add the application to a group or later. Configuring a home zone for an application is also known as adding an application to a zone.
    *  A user or an application can have only one home zone at a time. (An exception for users is that they can have multiple zone memberships because of a user group membership. However, even in this case, the broker uses only one home zone). Although zone preferences for users and applications can be configured, the broker selects only one preferred zone for a launch. The default priority order for selecting the preferred zone is: application home > user home > user location. Refer to the [Citrix documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/zones.html) for more information on zone preferences and how it works.

1.  User Access (Library -> Subscriber) to Resources
    *  While creating the Delivery Group, on the users’ wizard specify the users and user groups who can use the applications and desktops in the Delivery Group.
    *  As an alternative to specifying applications in the Delivery Group wizard, you can configure them through the Citrix Cloud library as subscribers.
    *  PowerShell scripts with SDK commands can be used to export the user mappings for each application from the existing environment and import those details on Citrix Cloud

1.  Citrix Policies
    *  Configure Citrix policies to control user access and session environments. Citrix policies are the most efficient method of controlling connection, security, and bandwidth settings. You can create policies for specific groups of users, devices, or connection types. Each policy can contain multiple settings.
    *  As part of the migration, you need to convert existing Citrix Policies into Templates.
    *  Create a new Active Directory GPO (if there are no existing GPO with Citrix Policies) and link the GPO to the Organization Unit where the VDA’s are placed. Use the existing policy if there is an existing GPO with Citrix setting.
    *  Edit the GPO and select the Citrix Policies under Computer Configuration.
    *  Create a new policy and select the exported template name to import the template settings.
    *  Repeat the above steps for GPO User configuration and import Citrix Policies for users.
    *  Ensure there are no conflicts with Citrix settings from any other GPO settings.
    *  Refer to the [Citrix Documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/policies.html) for more information about Citrix Policies on Citrix Cloud.

1.  App Groups and Tags
    *  Application Groups let you manage collections of applications. Application Groups are optional; they offer an alternative to adding the same applications to multiple Delivery Groups. Delivery Groups can be associated with more than one Application Group, and an Application Group can be associated with more than one Delivery Group.
    *  Tags are strings that identify items such as machines, applications, desktops, Delivery Groups, Application Groups, and policies. You can use the tag restriction feature to publish applications from an Application Group, considering only a subset of the machines in selected Delivery Groups. With tag restrictions, you can use your existing machines for more than one publishing task, saving the costs associated with deploying and managing additional machines. A tag restriction can be thought of as subdividing the machines in a Delivery Group. Using an Application Group or desktops with a tag restriction can be helpful when isolating and troubleshooting a subset of machines in a Delivery Group.
    *  Review the Application Groups and Tags from the on-premises environment and configure those App Groups and Tags on Citrix Cloud.

1.  Workspace Configuration
    *  The evolution of Citrix Storefront in Citrix Cloud is known as Citrix Workspace and it is enabled by default when you subscribe to Citrix Cloud. Citrix Workspace does not require updates or maintenance from a customer perspective. Customers have a similar configuration and customize options with Citrix Workspace compared to traditional StoreFront.
    *  When using Workspace users will browse to `https://example.cloud.com`, the certificate and FQDN is owned by Citrix.
    *  In Citrix Cloud > Workspace Configuration > Access tab shows the Workspace URL which is ready to use. You can enable the individual service resources to your users from the Service Integrations tab. By default, the Virtual Apps and Desktops Service and the Secure Browser Service are enabled after you subscribe to them.
    *  Change authentication to workspaces - Change how subscribers authenticate to their workspace in Workspace Configuration > Authentication > Workspace Authentication.
    *  Access the workspace URL and verify the resource enumeration.
    *  Customize workspace preferences - Customize how subscribers interact with their workspace in Workspace Configuration > Customize and preferences tab.
    *  Customize the workspace URL - The first part of the workspace URL is customizable. You can change the URL from, for example, `https://example.cloud.com`, to `https://newexample.cloud.com`.

1.  Gateway Service Configuration
    *  To provide secure access for your remote subscribers, enable the Citrix Gateway service to the resource locations. You can select the Traditional Gateway or Gateway Service from Workspace Configuration > Access > External Connectivity or from Citrix Cloud > Resource Locations.
    *  When you create a resource location, you are offered the option to add a Citrix Gateway. When a Citrix Gateway is associated with a zone, it is preferred for use when connections to VDAs in that zone are used. Ideally, Citrix Gateway in a zone is used for user connections coming into that zone from other zones or external locations. You can also use it for connections within the zone.
    *  Gateway Service is a replacement of your on-premises Gateway to access the Citrix environment using the Workspace URL, for example `https://examplecustomer.cloud.com`.

1.  VDA Registration with Cloud Connector
    *  During the VDA install you would normally configure the ListOfDDCs registry flag containing the FQDNs of the controllers. Instead, now when you install the VDAs, you'll configure the Citrix Cloud Connector addresses. The VDAs will talk to the connector, which will then proxy all the traffic up to the Delivery Controllers that are managed in the cloud.
    *  Configure the GPO or Citrix Policies and apply it to the existing VDAs to establish the communication with Cloud Connectors for registration. Specify all the relevant Cloud Connectors for the Resource Location or Domain.
    *  Update the Golden Images / virtual disks with new Cloud Connector information
    *  Validate the VDA registration with Cloud Connector
    *  If you add or remove a Cloud Connector in a zone (using the Citrix Cloud management console), and auto-update is enabled, VDAs in that zone receive updated lists of available local Cloud Connectors, so they know with whom they can register and accept connections from.

### Migration steps for Strategy #2 - Migrating the Control Layer components integrating with On-Premises StoreFront and Gateway

[![cloud-migration-strategies-Image-3](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_003.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_003.png)

In this approach, all the above 1 to 15 steps are also applicable and are required to be followed in addition to the below Step for enabling the remote access.

*  Configure On-Premises StoreFront and Gateway
    *  You can use the on-premises StoreFront and Gateway along with Citrix Cloud. This allows you to enable access to Citrix hybrid environment using the existing URL configured on your on-premises Gateway and StoreFront.
    *  The biggest consideration points for customers that are new to the cloud is having the authentication point hosted in the cloud. The multi-factor authentication solution supported in Workspace Experience is Azure MFA and Okta, for all other integration Citrix recommends deploying StoreFront and Citrix ADC on-premises.
    *  Customer-managed StoreFront offers greater security configuration options and flexibility for deployment architecture, including the ability to maintain user credentials on-premises.
    *  On-premises Citrix ADC offers customization flexibility. On-premises Citrix ADC passes the user credentials to on-premises StoreFront, which uses Cloud Connectors as the XML Server. With on-premises Citrix Gateway and StoreFront, the user credentials never traverse into the Citrix Cloud.
    *  Some basic customization can be done in the Workspace, but for more advanced customization customers can use the CSS capabilities of StoreFront and Citrix ADC. For customers wanting to use their own FQDN and certificates, Citrix recommends deploying StoreFront and Citrix ADC on-premises.
    *  When using Workspace Experience, customers cannot limit certain IP addresses or user groups from authenticating. When using StoreFront and Citrix ADC on-premises, customers gain a bit more flexibility and security, such as LDAP group extraction and DoS prevention.
    *  Local Host Cache - Local Host Cache (LHC) allows the Citrix Virtual Apps and Desktops Service deployment to continue connection brokering operations in a Site when the Cloud Connector fails to connect to Citrix Cloud. Any users connected when LHC engages continue to stay connected, uninterrupted. Local Host Cache only works with an on-premises StoreFront deployment. It does not compliment the Workspace Experience. During an outage, one of the Cloud Connectors is elected to be the primary broker for the resource location. Local Host Cache is supported for Server-hosted applications and desktops and Static (assigned) desktops.
    *  StoreFront on-premises does not support delegating authentication to the Delivery Controllers in a cloud deployment.
    *  When using on-premises StoreFront and Citrix ADC, a customer can choose to integrate both the Virtual Apps and Desktops by pointing at the Cloud Connectors. Additionally, customers can also add their on-premises Delivery Controllers as a separate Site. Doing this will aggregate all the resources available for users between both the on-premises site and Citrix Cloud.

### Migration steps for Strategy #3 - Migrating the Control Layer components and Storefront Services to Citrix Cloud and integrating Citrix Workspace with On-Premises Gateway

[![cloud-migration-strategies-Image-4](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_004.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_004.png)

In this approach, all the above 1 to 15 steps are applicable and are required to be followed in addition to the below Steps for enabling ICA proxy access integrating with Citrix Workspace.

*  A customer-hosted Citrix Gateway acts as ICA-proxy only to enable remote access to the published resources. If you do not want to use the Citrix Gateway as a Service and to provide optimal connection to resources, you can deploy Citrix Gateways or Citrix ADCs at on-premises locations and register with the resource location. This is done within the Workspace Configuration.
    *  The Workspace URL is still used to access the Citrix environment and it remains the same for this deployment.
    *  HDX Optimal Routing is not available as a feature in Citrix Cloud. However, you can define Traditional on-premises Citrix Gateways per resource location and route the connections through those specific Citrix Gateways at the time of launch.
    *  Managing an on-premises Citrix ADC deployment requires a specialized networking skillset. If an organization decides to use an on-premises Citrix ADC or Citrix Gateway it is always recommended to have all components in pairs to avoid a single point of failure.
    *  To set up on-premises Citrix Gateway as an ICA Proxy, there are no authentication or session policies are needed. In Citrix Cloud > Resource Locations, select Gateway for the resource location you want to use. Select Traditional Gateway and enter the external FQDN URL configured on your on-premises Gateway. Do not add a protocol. Ports are optional. Bind Citrix Cloud Connectors as Secure Ticket Authority (STA) servers to Citrix Gateway. For more information, see CTX232640.

## Detailed Migration Steps

Let’s review the migration processes more in detail with screenshots

`Migration steps for Strategy #1 - Migrating the Control and Access Layer components to Citrix Cloud, integrating with Citrix Gateway Service`

### Step-1: Login to your Citrix Cloud Subscription and select the Region

If an administrator logs in to Citrix Cloud for the first time, they will get an option to select a home region. It is important to select the appropriate region for your deployment. This cannot be changed later. Refer to the [Citrix Docs Page](https://docs.citrix.com/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/geographical-considerations.html) for more information. You can access the Studio management console to configure and manage connections, machine catalogs, and Delivery Groups. Studio launches when you select Manage in the Citrix Cloud console.

### Step-2: Add administrators to a Citrix Cloud account and Delegate Administration

#### Citrix Cloud Administrators

The first Citrix Cloud Administrator is created during the subscription onboarding process. This Administrator has full rights to the full subscribed services. This first Administrator can add existing administrators using an invite from the Citrix Cloud Console.

Citrix Cloud sends an invitation to the user-specified and adds the administrator to the list. The email is sent from `cloud@citrix.com` and explains how to access the account. When the new administrator receives the email, they click the Join link to accept the invitation. Also, a browser window opens, displaying a page where they can create their password.

When the first Citrix Cloud Administrator invites additional administrators, their permissions can be configured to delegate access appropriate to their administrative role. Only full access administrators can add delegated administrators and define their level of access.

#### Delegated Administration

With Delegated Administration in Citrix Cloud, the first administrator can configure the access permissions that a new administrator needs, in accordance with their role in their organization. Refer to the [Citrix Docs Page](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/delegated-administration.html) for complete information on Delegated Administration in Citrix Cloud.

### Step-3: Create a Resource Location

In a Citrix Virtual Apps and Desktops Service deployment, a resource location contains items from the access layer and resource layer:

*  Cloud Connectors
*  An Active Directory Domain Controller
*  Virtual Delivery Agents (VDAs)
*  Hypervisors that provision VDAs and store their data, if used
*  Citrix Gateway (optional)
*  StoreFront servers (optional)

Let’s create the first Resource Location by installing the Cloud Connectors on a VM within the customer on-premises environment. We can install the Cloud Connector software interactively or by using the command line. These connectors essentially replace the on-premises delivery controllers.

During installation, the Cloud Connector requires internet access to Citrix Cloud to authenticate the user performing the installation, validate the installer’s permission(s), and configure the services. The installation occurs with the privileges of the user who initiates the install.

Refer to the [Citrix Docs Page](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html) for more information on how to install the Cloud Connector in interactive installation mode and command-line mode.

Once we successfully install the first Cloud Connector, the first resource location is created on  Citrix Cloud.

[![cloud-migration-strategies-Image-5](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_005.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_005.png)

**Note:** It is highly recommended to have two or more Cloud Connectors in a resource location to maintain high availability of services during the upgrade or maintenance with any one of the Cloud Connectors.

[![cloud-migration-strategies-Image-6](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_006.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_006.png)

Also, the installation of the Cloud Connectors registers your on-premises domain to Citrix Cloud under the Identity and Access Management section.

[![cloud-migration-strategies-Image-7](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_007.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_007.png)

#### Active Directory Domain Considerations

Cloud Connectors cannot traverse domain-level trusts. If deploying resources in a separate domain, also install Cloud Connectors in each user domain.

#### Multi-Domain Support

Each domain where Cloud Connectors are deployed will appear in the Domains list. Citrix Cloud supports multiple domains and forests. Launching resources in the same domain or forest does not require any trust relationships to be configured. When launching resources from another domain or forest, trust relationships between the domains or forests must be configured.

[![cloud-migration-strategies-Image-8](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_008.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_008.png)

Azure Active Directory domain service is also supported. Refer to the [Citrix Docs Page](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory) for more deployment scenarios for Cloud Connectors with Active Directory domains and forests.

**Key Points:**

*  The Cloud Connector needs to be installed on a dedicated domain-joined machine.
*  It is recommended to keep all the Connectors powered on for proper operation and maintenance.
*  The number of Connectors to install should at minimum be N+1 where N is the capacity needed to support the load within your resource location. Also, it is important to consider the Local Host Cache resource requirement during an outage. While 2 x Cloud Connectors are technically enough to facilitate HA under normal operations, having 3 x Cloud Connectors would facilitate continued HA in place during any maintenance operations and handle the resource requirements of LHC. Refer to the Citrix [LHC documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html) for the scalability details.
*  Having highly available Cloud Connectors minimizes the risk of a single point of failure and allows users to continue to use their resources even in the event of failure on a single Cloud Connector.
*  Cloud Connectors automatically distribute the load for Workspace and Gateway Service access. If all the Cloud Connectors at a customer-hosted location lose connectivity to Citrix Cloud and if there is no StoreFront configuration available for LHC, all brokering, and power management will cease to function. Configuring on-premises StoreFront helps during the outage by enabling the Local Host Cache. Existing HDX connections within the resource location will continue to run unless they are made through the Cloud Hosted Gateway Service.
*  Cloud Connector Updates - Periodically, Citrix releases updates to increase the performance, security, and reliability of the Cloud Connector. To install these updates timely without unduly affecting your users’ Citrix Cloud experience, you can choose when these updates are installed. Refer to the [Citrix documentation](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/cloud-connector-updates.html) for more information.
*  Refer to [CTX221535](https://support.citrix.com/article/CTX221671) for troubleshooting Cloud Connector issues.

In the Citrix Virtual Apps and Desktops Service, zones are created automatically when you create a resource location and add a Cloud Connector to it. Unlike an on-premises deployment, a service environment does not classify zones as primary or satellite. You can place machine catalogs, hypervisors, host connections, users, and applications in a zone. A zone can also contain Citrix Gateway and StoreFront servers. To use the Local Host Cache feature, a zone must have a StoreFront server.

### Step-4: Deploy Certificates for Cloud Connectors

The Cloud Connector runs the XML and the STA services on port 80 by default as these communications are typically INTERNAL. To configure encryption for these traffic types, certificates should be deployed on the Cloud Connectors and these two services should be bound to those certificates.

Both public and self-signed certificates can be used as the customer-hosted StoreFront and on-premises Citrix ADCs need to trust the certificates. This is only necessary if you are using on-premises StoreFront or Citrix ADCs. If using a cloud-hosted Workspace and Citrix Gateway as a Service, the communication will be encrypted by default. The Cloud Connector use HTTPS/SSL for all outbound communication to Citrix Cloud.

[![cloud-migration-strategies-Image-9](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_009.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_009.png)

#### Enable TLS on Cloud Connectors to secure XML Traffic

Refer to the [Citrix Support Article](https://support.citrix.com/article/CTX221671) for detailed information on how to enable SSL on Cloud Connectors to secure the XML traffic. The XML Service is used for application and desktop resource enumeration including handling user name and password data from StoreFront to Cloud Connectors, therefore, it must be encrypted.

#### Using XML and STA services on Cloud Connector

When StoreFront and Citrix Gateway are deployed at the customer-hosted location and integrated with Citrix Cloud, the XML and the STA services are being used. Since on-premises StoreFront and Citrix Gateway cannot talk directly to the Cloud Delivery Controller function directly, the Remote Broker Service on the Cloud Connectors helps to supply XML data and STA tickets.

### Step-5: Zone Creation and Mappings

When you create a resource location in Citrix Cloud and then add a Cloud Connector to that resource location, the Citrix Virtual Apps and Desktops Service automatically create a zone.

You can place host connections, machine catalogs, users, and applications in a zone. Placing items in a zone affects how the service interacts with them and with other objects related to them.

After you create more than one resource location (and the zones are created automatically), you can move resources from one zone to another. This flexibility comes with the risk of separating items that work best in proximity. For example, moving a catalog to a different zone than the connection (host) that creates the machines in the catalog, can affect performance.

If the connection between a zone and Citrix Cloud fails, the Local Host Cache feature enables a Cloud Connector in the zone to continue brokering connections to VDAs in that zone. The zone must have StoreFront installed.

[![cloud-migration-strategies-Image-10](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_010.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_010.png)

### Step-6: Configure the Hosting Connections

A Hosting Connection is required in Citrix Cloud to enable the communication with hypervisors and public cloud platforms as well as power management of VDAs. Hypervisor commands are executed by the HCL Service in Cloud Connector when doing MCS operations in Citrix Cloud. To communicate with a customer-hosted hypervisor, we need to configure the hosting connections on Cloud Studio.

#### Creating a hosting connection on Citrix Cloud Studio

Refer to the [Citrix Docs Page](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html) for the step-by-step process on how to create the hosting connection on Citrix Cloud. It is recommended to manually create the public cloud hosting connections on Citrix Cloud.

#### Additional Hypervisor Requirements for Cloud Connectors

1.  For a VMware vCenter that has self-signed certificates installed, the certificate needs to be added to the Citrix Cloud Connector
2.  For Hyper-V and System Center Virtual Machine Manager (SCVMM), the SCVMM Console must be installed on the Citrix Cloud Connector.
3.  For Citrix Hypervisor, consider deploying a certificate on the hosts and trusting it on the Cloud Connectors.

### Step-7: Create Machine Catalog

After adding the hosting connections, we need to Create Machine Catalogs and Delivery Groups and then assign Users and Groups to resources. Machine Creation Services and Citrix Provisioning is the same process as on-premises except that the Cloud Connector is communicating with hypervisors, Citrix Provisioning Servers, and Active Directory.

You can create a machine catalog manually by accessing the Cloud Studio. During this process, select the Master image from the hypervisor which will be communicated through Cloud Connectors.

[![cloud-migration-strategies-Image-11](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_011.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_011.png)

Select the **Resource Location** and **Zone** to establish a connection with an on-premises hypervisor.

[![cloud-migration-strategies-Image-12](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_012.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_012.png)

Select the **master image** and follow the remaining steps

[![cloud-migration-strategies-Image-13](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_013.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_013.png)

Enter how many virtual machines need to be created and Select the **OU** and **Naming scheme** for creating the VMs.

[![cloud-migration-strategies-Image-14](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_014.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_014.png)

Enter the domain credentials which has permissions to create AD accounts

[![cloud-migration-strategies-Image-15](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_015.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_015.png)

Name the Machine catalog and click **Finish** to complete the process

[![cloud-migration-strategies-Image-16](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_016.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_016.png)

Machine Catalog was created successfully with the required number of VMs.

[![cloud-migration-strategies-Image-17](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_017.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_017.png)

### Step-8: Create Delivery Group and Publish the resources

The next step is to create the delivery group and add the machines. During this process, you will also have the option to publish applications and desktops to the users.

Delivery Groups can use machines from more than one machine catalog that contains the same machine types. In other words, you cannot mix machine types in a Delivery Group. Similarly, in a deployment that has catalogs of Windows machines and catalogs of Linux machines, a Delivery Group can contain machines from either OS type, but not both.

Click on the **Create Delivery Group** option to begin the process and select the appropriate machine catalog.

[![cloud-migration-strategies-Image-18](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_018.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_018.png)

Select **Leave user management to Citrix Cloud** or other options based on the requirement.

[![cloud-migration-strategies-Image-19](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_019.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_019.png)

Add the applications which are required to publish from this delivery group.

[![cloud-migration-strategies-Image-20](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_020.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_020.png)

Name the **delivery group** and complete the process of creating a delivery group and publishing the applications

[![cloud-migration-strategies-Image-21](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_021.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_021.png)

#### Pooled Desktops

`The above-shown processes (Step 7 & 8) for creating machine catalogs and delivery groups apply for Pooled Desktops.`

### Step-9: Create Machine Catalogs (Static)

Let’s review how to Create Machine Catalogs and Delivery Groups and then assign Users and Groups to statically assigned machines. Machine Creation Services and Citrix Provisioning is the same process as on-premises except that the Cloud Connector is communicating with hypervisors, Citrix Provisioning Servers, and Active Directory.

Cloud Studio guides you through the process of creating the Machine catalog. Select **Single session OS** for Desktop provisioning:

[![cloud-migration-strategies-Image-22](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_022.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_022.png)

Since we are migrating the static machines that contain the user data and user-specific applications, we continue to use them as it is. Select **Other technology** on machine management so that we can add those VMs directly from hypervisors.

[![cloud-migration-strategies-Image-23](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_023.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_023.png)

Select the Desktop Experience as **Static Desktops**

[![cloud-migration-strategies-Image-24](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_024.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_024.png)

Add the VMs and respective users from the VM list retrieved from the hypervisor

[![cloud-migration-strategies-Image-25](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_025.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_025.png)

Name the machine catalog and complete the process of creating the machine catalog.

[![cloud-migration-strategies-Image-26](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_026.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_026.png)

With that, the machine catalog for permanent machine allocation is successfully created.

[![cloud-migration-strategies-Image-27](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_027.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_027.png)

### Step-10: Create Delivery Group (static) and assign users

Let’s create the delivery group for statically assigned machines. Select the **static machine catalog** which was created from the above step.

[![cloud-migration-strategies-Image-28](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_028.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_028.png)

The **Machine allocation** tab displays the machine names and users assigned to them

[![cloud-migration-strategies-Image-29](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_029.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_029.png)

Select the Delivery type as **Desktop** and continue the remaining process to complete the Delivery Group creation.

[![cloud-migration-strategies-Image-30](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_030.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_030.png)

We have successfully completed the delivery group creation for statically-assigned machines.

[![cloud-migration-strategies-Image-31](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_031.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_031.png)

**Note:** We have not migrated the virtual machines but instead just created the Machine Catalog and Delivery Groups. Let’s migrate the Policies, Tags and App Groups before migrating the VMs from the on-premises Delivery Controllers to Cloud Connectors.

### Step-11: Migrate Citrix Policies

The next step on the migration process is to export the Citrix HDX Policies into Active Directory Policies to apply them via the Organization Unit. You can configure and apply Citrix policies either within Citrix Studio console or via Active Directory.

Navigate to on-premises Studio and select **Policies**. Right-click on the Citrix policy which needs to be migrated and select **save as Template**.

[![cloud-migration-strategies-Image-32](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_032.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_032.png)

Provide a name for the template and save the settings as a template.

[![cloud-migration-strategies-Image-33](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_033.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_033.png)

A new **custom template** is created from the policy and the respective settings

[![cloud-migration-strategies-Image-34](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_034.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_034.png)

Now, we need to access the **Group Policy Management Console** from the Delivery Controller, if this feature is not installed, it is recommended to install this feature and proceed with next steps

[![cloud-migration-strategies-Image-35](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_035.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_035.png)

Open the **Group Policy Management console**, and select the **OU** where the VDAs reside

[![cloud-migration-strategies-Image-36](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_036.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_036.png)

Create a new GPO and edit the new policy to import the Citrix policy settings

[![cloud-migration-strategies-Image-37](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_037.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_037.png)

On the left pane, select either the **computer or user configuration** > Policies > Citrix Policies. On the right pane, select the templates. You will see the template which was created earlier

[![cloud-migration-strategies-Image-38](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_038.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_038.png)

Right-click the template and select **New policy** and proceed with the naming the policy.

[![cloud-migration-strategies-Image-39](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_039.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_039.png)

Select **Use the settings from template** to import those exported settings from the template

[![cloud-migration-strategies-Image-40](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_040.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_040.png)

Apply the filters by selecting either a delivery group or user group based on your on-premises configuration.

[![cloud-migration-strategies-Image-41](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_041.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_041.png)

Select Next and Finish to complete the policy creation.

[![cloud-migration-strategies-Image-42](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_042.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_042.png)

Change the **priority** to apply those settings. Enable the policy if it was not already enabled. You can verify the settings from the Group Policy Management Console.

[![cloud-migration-strategies-Image-43](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_043.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_043.png)

### Step-12: Create App Groups and Tags

Review the App Groups and Tags configured in your on-premises environment and create them on Citrix Cloud. Let’s begin the App Group creation process and select the Delivery Group to which the App Groups needs to be created

[![cloud-migration-strategies-Image-44](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_044.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_044.png)

Leave the user configuration as default settings and add the existing or new applications into App Group

[![cloud-migration-strategies-Image-45](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_045.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_045.png)

Name the App Group as per the requirement to complete the App Group creation

[![cloud-migration-strategies-Image-46](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_046.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_046.png)

To create the Tags, review the existing configuration from the on-premises environment and configure those settings on Citrix Cloud.

Select the Applications to which the Tag needs to be configured.

[![cloud-migration-strategies-Image-47](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_047.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_047.png)

Select the appropriate tag and complete the process

[![cloud-migration-strategies-Image-48](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_048.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_048.png)

### Step-13: Citrix Cloud Workspace and Gateway Service Configuration

To access the Workspace configuration, navigate to Citrix Cloud > Workspace Configuration. The Access tab shows the Workspace URL which is ready to use by the end-users. The first part of the workspace URL is customizable. You can change the URL from, for example, `https://example.cloud.com`, to `https://newexample.cloud.com`.

[![cloud-migration-strategies-Image-49](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_049.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_049.png)

You can enable the External Connectivity by selecting the **Resource Location** and configure **connectivity option**

[![cloud-migration-strategies-Image-50](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_050.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_050.png)

This option allows you to select the Gateway Service

[![cloud-migration-strategies-Image-51](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_051.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_051.png)

The **Authentication tab** allows you to configure the authentication method for user access

[![cloud-migration-strategies-Image-52](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_052.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_052.png)

The **Customize Tab** on Workspace Configuration allows you to customize the Workspace Appearance and Preferences

[![cloud-migration-strategies-Image-53](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_053.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_053.png)

Access the **Workspace URL** and Login to verify the authentication

[![cloud-migration-strategies-Image-54](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_054.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_054.png)

### Step-14: Migrate existing VDAs to register with Citrix Cloud

As a final step in migration, we need to migrate the Virtual Delivery Agents (VDAs) which are currently registering with on-premises Delivery Controllers to Cloud Connectors to register with Citrix Cloud.

For the MCS and Citrix Provisioning (Pooled and Server OS) machines, we need to update the **ListOfDDCs registry flag** on the Master Image.

Open the **Registry**, and navigate to **HKLM\Software\Citrix\VirtualDeliveryAgent** key, and update the **ListOfDDCs** with Cloud Connector details

[![cloud-migration-strategies-Image-55](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_055.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_055.png)

Shutdown the master image for MCS or virtual disk for PVS and update the machine catalog details on the ListofDDCs registry.

For Static virtual machines, we can use the Active Directory Group Policy to update the controller details. The following are the steps:

Open the **Group Policy Management Console** and create a **new GPO**.

Edit the Policy, select **Computer Configuration** and then **Citrix Policies**. On the right pane, select **new policy** under **Citrix computer policies**

[![cloud-migration-strategies-Image-56](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_056.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_056.png)

Name the Policy as **VDA Migration** and select the Controller settings in the list

[![cloud-migration-strategies-Image-57](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_057.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_057.png)

Select **Controllers** and click on **Add** to update the Cloud Connector details

[![cloud-migration-strategies-Image-58](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_058.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_058.png)

Provide the **Cloud Connector details** as controllers name

[![cloud-migration-strategies-Image-59](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_059.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_059.png)

Next, add the **Enable auto update of Controller** option and select **Prohibited**. This will allow VDAs not to update any other controller information.

[![cloud-migration-strategies-Image-60](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_060.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_060.png)

On the **filters** page, select the **delivery group** on which this policy needs to be applied.

[![cloud-migration-strategies-Image-61](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_061.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_061.png)

Enable the policy and click on create to complete the policy creation.

[![cloud-migration-strategies-Image-62](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_062.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_062.png)

Increase the **priority** of the policy to apply the settings

[![cloud-migration-strategies-Image-63](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_063.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_063.png)

Once the policy settings are updated on the virtual machines, they will start registering with Cloud Connectors. Users can access those resources from the Citrix Workspace URL or through the on-premises Gateway and StoreFront.

`Migration steps for Strategy #2 - Migrating the Control Layer components integrating with On-Premises StoreFront and Gateway`

In this approach, all the above 1 to 14 steps are also applicable and are required to be followed in addition to Step 15 for enabling the remote access.

### Step-15: Configure On-Premises StoreFront and Gateway

Customers can use an existing StoreFront or install a set of StoreFront servers to aggregate applications and desktops in Citrix Cloud.

The multi-factor authentication solution supported in Citrix Workspace is Azure MFA and Okta, for all other integration, Citrix recommends deploying StoreFront and Citrix ADC at customer-hosted locations. It also allows customers to customize their domain names and URLs. This deployment type is recommended for any Citrix Virtual Apps and Desktops customers who already have StoreFront deployed.

The StoreFront servers need to communicate with Cloud Connectors for resource enumeration from Citrix Cloud. The Cloud Connectors are installed with SSL certificates to ensure that the XML and STA traffic is encrypted and secure.

Configure the **store** on StoreFront Servers with **Cloud Connectors**. If you are creating a new store add the **Cloud Connectors** on the **Delivery Controllers** page. If you are using the existing store, select the **Manage Delivery Controllers** option and update the FQDNs of the Cloud Connectors.

[![cloud-migration-strategies-Image-64](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_064.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_064.png)

Enable the **Remote Access** option to integrate the Store with Citrix ADC or Gateway to enable external access for this store.

[![cloud-migration-strategies-Image-65](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_065.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_065.png)

Configure the **trusted domain** for authentication and apply the customizations required as per the organization requirements. Access the StoreFront URL internally and verify the access.

[![cloud-migration-strategies-Image-66](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_066.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_066.png)

For external access, we need to verify the **Security Ticket Authority** details. If not added in previous steps add those details now

[![cloud-migration-strategies-Image-67](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_067.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_067.png)

Configure the **CallBack URL** if required and finish the StoreFront configuration.

Let’s configure the **Citrix Gateway** to enable external access. Connect to Citrix ADC. In the Integrate with Citrix Products section, click **XenApp and XenDesktop**.

[![cloud-migration-strategies-Image-68](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_068.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_068.png)

Follow the Wizard and provide the required details for **FQDN and SSL Certificate** for the configuration.

[![cloud-migration-strategies-Image-69](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_069.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_069.png)

Provide the details of **StoreFront Servers, Store, and STA** details

[![cloud-migration-strategies-Image-70](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_070.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_070.png)

Configure the **Active Directory Domain** details for the authentication.

[![cloud-migration-strategies-Image-71](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_071.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_071.png)

Configure the **Session Policies** to complete the ADC configuration. Also, apply the necessary themes with the required customization.

[![cloud-migration-strategies-Image-72](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_072.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_072.png)

On-premises StoreFront and Gateway configuration are successfully completed.

`Migration steps for Strategy #3 - Migrating the Control Layer components and Storefront Services to Citrix Cloud and integrating Citrix Workspace with On-Premises Gateway`

In this approach, all the above 1 to 14 steps are applicable and are required to be followed in addition to Step 16 for enabling ICA proxy access integrating with Citrix Workspace.

### Step-16: Configure On-Premises Gateway as ICA Proxy

Now it’s time to configure the Citrix Cloud Workspace to enable the on-premises ADC as ICA proxy Access. Access the **Citrix Cloud Workspace page**, select the **Resource Location**, and Select **Gateway**

[![cloud-migration-strategies-Image-73](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_073.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_073.png)

Select the **Traditional Gateway** to integrate the on-premises ADC with Citrix Cloud and Save the configuration.

[![cloud-migration-strategies-Image-74](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_074.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_074.png)

Users still access the environment via the **Workspace URL** and when they launch a resource, the ICA connection is then established via the on-premises gateway.

Follow the traditional approach to configure the on-premises gateway except that there are no authentication or session policies are needed. Bind Citrix Cloud Connectors as Secure Ticket Authority (STA) servers to Citrix Gateway. For more information, see [CTX232640](https://support.citrix.com/article/CTX232640).

## Appendix

### Citrix Cloud API and SDK

Install the Citrix Virtual Apps and Desktops Service Remote PowerShell SDK on a dedicated VM or system.

Citrix SDKs help automate complex and repetitive tasks. The Remote PowerShell SDK can access the Control Plane the same way an on-premises Delivery Controller can be administered using PowerShell.

In an on-premises deployment, the Citrix PowerShell SDK is automatically installed on Delivery Controllers. With Citrix Cloud, customers must download and install the Remote SDK on a dedicated administrative machine. The Remote SDK **should not** be deployed on Cloud Connectors. The SDK’s operation does not involve Cloud Connectors. The PowerShell cmdlets utilize memory for execution hence it is not recommended to execute those commands from Cloud Connectors.

The Remote SDKs for Citrix Virtual Apps and Desktops can be downloaded from the downloads page on Virtual Apps and Desktops page on Citrix Cloud.

The Remote SDKs for Citrix Virtual Apps and Desktops require authentication before it can access Citrix Cloud. Once authenticated, the remote access remains valid in the current PowerShell Session for up to 24 hours. Authentication to use Remote PowerShell SDKs can be either done using:

**CloudMC (My Citrix):** Prompt for username and password for each PowerShell session (GUI).

**CloudAPI:** Customer ID and API key & secret stored in the users’ Windows profile. This method is used to run unattended scripts as it provides silent authentication.

To create a CloudAPI profile that bypasses the manual Citrix Cloud authentication dialog, you must first create a Citrix Cloud API Access Secure Client.  This operation can be found in the Citrix Cloud console, under “Identity and Access Management.”

[![cloud-migration-strategies-Image-75](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_075.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_075.png)

Provide the name and create a client

[![cloud-migration-strategies-Image-76](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_076.png)](/en-us/tech-zone/build/media/deployment-guides_cvad-cvads-migration_076.png)

Downloading your Secure Client saves a file named secureclient.csv, which should be kept in a safe location. For more information, refer to the [Citrix Docs page](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/sdk-api.html) on how to install and use the Remote PowerShell SDK

#### Using the credential file and setup a credential profile

The following command creates a default credential profile for customer “citrixdemo” that will bypass manual authentication in the current and all subsequent PowerShell sessions.

`Set-XDCredentials -CustomerId “xaxdpm” -SecureClientFile “c:\temp\secureclient.csv” -ProfileType CloudAPI –StoreAs “default”`

To verify the successful authentication using SecureClientFile, execute the Get-XDAuthentication cmdlet. If there are no errors, the authentication was successful and it is ready to execute the remote PowerShell commands on Citrix Cloud for Customer ID “xaxdpm”.

## References

[Citrix Product Documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service.html)

[Reference Architecture for Citrix Cloud Virtual Apps and Desktops Service](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html)

[Cloud Connector Connectivity requirements](https://docs.citrix.com/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html)

[Cloud Connector Sizing Guide](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/install-cloud-connector/cc-scale-and-size.html)

[Cloud Connector Technical Details](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details)

[Deployment Scenarios for Cloud Connector with Active Directory domains](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory)

[Cloud Connector Installation](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html)

[Citrix Cloud Identity and Access Management](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html)

[Citrix Provisioning documentation](https://docs.citrix.com/en-us/provisioning.html)

[Hosting Connections](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/connections.html)

[Citrix Cloud SDK and API Access](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/sdk-api.html)

[Machine Catalog creation and types](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html)

[Citrix Policies](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/policies.html)

[Local Host Cache](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html)

[Cloud Connector Updates](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/cloud-connector-updates.html)

[How to install SSL Certificate on Cloud Connectors](https://support.citrix.com/article/CTX221671)
