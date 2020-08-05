---
layout: doc
description: Learn about different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.
---
# Designing StoreFront Multi-Site Aggregation

## Contributors

**Author:** [Sarah Steinhoff](https://www.citrix.com/blogs/author/sarahst)

A core functionality of StoreFront is the ability to aggregate and de-duplicate "common" application and desktop resources from multiple Citrix Virtual Apps and Desktops (CVAD) Sites, commonly referred as multi-site aggregation. Duplicate applications and desktops are identified based on matching `Application Display Name` and `Application Category` properties. This functionality has been available in the console as of version 3.5 and was previously a config file edit. The purpose of multi-site aggregation is to allow Citrix administrators to build redundant CVAD Sites (for scalability or failure domain reasons), yet present a single application or desktop icon to users instead of per-Site duplicates (as would be displayed without this feature). StoreFront then controls how application and desktop launches are distributed across the multiple Sites supporting that resource.

This article reviews how those settings can be implemented in an enterprise environment and how they integrate with other related settings, such as application keywords and subscriptions, to further control how user sessions are routed and resources are presented.

## Overview

The configuration of multi-site settings is done through the “Manage Delivery Controllers” wizard in the StoreFront console, with various configurations detailed [in product documentation](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html). If more than one Site is configured for the Store, the **Configure** button under “User Mapping and Multi-Site Aggregation Configuration” becomes enabled as shown below.

![Configuration Options](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_1.png)

Once selected, an administrator is prompted to configure user mappings and resource aggregation as shown below. The “Map users to Controllers” link prompts the administrator to configure a user group to which these settings should be applied. The “Aggregate resources” link is where the actual multi-site aggregation settings are specified.

![User Mapping and Resource Aggregation](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_2.png)

Multi-site aggregation cannot be configured until at least one user group has been defined to apply the settings to. The built-in “Everyone” group can be used if a single set of aggregation settings should be applied to all users who connect to the Store. Once these settings are configured, if a user is not a member of one of the groups specified here, no applications or desktops are enumerated for that user. The next two sections review the available configurations in detail, starting with the “Aggregate resources” options.

## Aggregation options

In the Aggregate resources section, an administrator is presented with all of the Sites that are enumerated and prompted to select which have overlapping resources and should be aggregated, as shown below.

![Aggregate Resources settings](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_3.png)

For the aggregated Sites, there are two additional configurations that control how resources are enumerated and sessions are launched across those Sites. These settings apply to all Sites marked for aggregation:

-  **Controllers publish identical resources**: This setting controls enumeration. If two Sites are marked as “identical,” StoreFront places the farms in the same “equivalent farm set,” which means that enumeration requests are load balanced (round robin) across the Sites as the resource sets are assumed to be equivalent, saving enumeration time. If the two Sites are not marked as “identical,” then StoreFront sends XML enumeration request to all and de-duplicates the common application and desktops from the resulting resource sets.
-  **Load balance resources across controllers**:  This setting controls session launch. Launch requests are either load balanced across the Sites or distributed in failover order, regardless of whether the Sites are “identical” or not. Session sharing takes precedence over a load balancing decision. Therefore, if a user already has a session in Site B and launches another applications or desktop, that session will also launch in Site B (assuming the application or desktop is available there).

Some use cases that are not handled well through the GUI are combinations of load balancing and failover or combinations of identical and non-identical Sites. For instance, if there are two production Sites that should be load balanced and one DR site that should only be used if both production Sites are down, the GUI cannot be used and the web.config file must be manually modified instead (refer to [eDocs](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html) for the proper format for this).

### Design Takeaways

To summarize the previous section:

-  Only one of a set of “identical” Sites is used to enumerate applications and desktops when the Sites are aggregated through StoreFront
-  Session launches can either be load balanced or failed over across aggregated Sites

## User Farm Mapping

The “Map users to controllers” settings are commonly referred to as “User Farm Mapping” as they are used to control which Sites a given group of users are allowed to enumerate against, whether those Sites are aggregated or not. There are two primary use cases for this functionality:

1.  **Limiting Enumeration**: even without multi-Site aggregation, assigning a group of users in StoreFront to a subset of Sites means that StoreFront will only send XML enumeration requests to those Sites when a user from that group authenticates. In a global deployment, this can have a significant impact on enumeration time as it prevents StoreFront from attempting to communicate with Sites that a given user should not be accessing anyway. For instance, these settings can be used to map US users to US-based CVAD Sites such that StoreFront will not reach out to other globally dispersed Sites when those users log in.
2.  **Assigning Aggregation Settings**: if multi-Site aggregation is configured, it may be desirable to assign different configurations to different groups of users, such as different failover configurations or different combinations of Sites.

The “Map users to controllers” first prompts administrators to specify a user group and then the Sites that the user group will be assigned to (although the dialog says “Controllers,” it is Sites as defined for the Store). There is a column that denotes whether the selected Sites have been previously configured for aggregation or not as shown below. Additionally, if the aggregated Sites were configured for failover (“Load balance resources across controllers” was left unchecked), the order of the Sites can be specified here via the arrows on the right.

![User Mapping settings](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_4.png)

It is possible for a user to be part of multiple user group mappings. When StoreFront reads the list of configured user groups, it does not stop processing after finding a match for a user. All configured groups are processed and all returned Sites enumerated against. This scenario should be expected in the case of Citrix administrators, who typically have access to all applications and desktops and are members of multiple user groups to be able to test and reproduce reported user issues. This just means that administrators (or other users that are members of multiple groups) might experience different launch behavior that users in one of the groups because they have multiple configurations applied to them.
Configuration-wise, the only consideration is if two user groups have access to the same set of Sites with different aggregation settings. In the context of an individual user, a Site can only belong to one aggregation group or an error will be displayed. This is resolved by naming the aggregation groups the same between the two user group mappings, which is done by default through the GUI, but may be a consideration if the web.config file is being manually modified for a more advanced set of configurations as previously referenced.

### Design Takeaways

To summarize the previous section:

-  Assigning user groups to Sites, even without aggregation configured, can be used to help limit enumeration traffic and thereby reduce the time to completion for this process
-  Failover order for aggregated Sites is specified in the user group assignment wizard
-  When multiple user group mappings are configured containing some of the same aggregated Sites and users belong to multiple groups, the aggregation groups must be named the same

## Application Keywords

Another way to control resource display and launch is by using keywords in the application description field. Within Store settings, display can be filtered based on custom keywords, as documented [in article CTX223451](https://support.citrix.com/article/CTX223451). There are also a few special, pre-defined keywords that have unique functions. Two of these are “primary” and “secondary,” which influence session launch across multiple Sites. When there are two instances of the same application published, the one with the keyword “primary” specified will always be preferred over the one with the keyword “secondary.”  This setting overrides any Site aggregation settings covered in the previous sections, which means these settings are frequently used together.

For instance, with two CVAD Sites, Site A and Site B, almost all applications should be primarily launched out of Site A (based on the back-end application architecture) and failed over to Site B, but there are a couple of applications that are primarily hosted out of Site B. Multi-Site aggregation would be configured for all users in failover order with Site A listed first. The exceptional applications that should primarily be hosted out of Site B would be configured with the keyword primary in Site B and secondary in Site A.

### Design Takeaways

To summarize the previous section:

•  The “primary” and “secondary” keywords can be used to further control application launch across multiple Sites and will take precedence over Site aggregation settings

## Subscriptions

Subscriptions in StoreFront are how users are allowed to “favorite” applications and are stored in a local database on each StoreFront server, replicated automatically across a server group. By default, subscription records are stored in the format `<SiteName>.<DisplayName>`. This is because StoreFront will by default enumerate resources from all CVAD Sites and display them individually. If two resources happen to be published with the same name in different Sites, subscriptions to them should be tracked individually. Therefore, we need some kind of Site tag to differentiate those subscriptions.

This changes with Site aggregation where identical resources across Sites should not be displayed and tracked individually, but aggregated behind the same subscription shortcut. The Site tag is no longer meaningful because a single subscription record must cover the same resource from multiple Sites. Therefore, when Site aggregation is configured, the subscription records for aggregated resources are stored in the format `<AggregationGroup>.<DisplayPath>\<DisplayName>`.

This means that if StoreFront multi-Site aggregation is configured after the Sites were previously available individually (with subscriptions enabled), any user subscriptions for newly aggregated resources will disappear because of the change in subscription record format. The previous subscription record will no longer be valid as the application is no longer tied to a Site, but an aggregation group. A workaround for this is to export the subscription database via PowerShell, replace all records for resources that will be aggregated with the proper format, and reimport the subscription database after the configuration of Site aggregation. Otherwise, users have to resubscribe to their applications.

### Design takeaways

To summarize the previous sections:

-  Multi-Site aggregation changes the format of user subscriptions in the subscription database, which must be considered if aggregation is implemented post go-live

## References

[Product Documentation: StoreFront Multi-Site Settings](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html)

[StoreFront Keywords Usage](https://support.citrix.com/article/CTX223451)

## Other Tech Zone content

[Learn -> PoC Guides -> Citrix Virtual Apps and Desktops with Windows Virtual Desktop Hybrid Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html) - Learn how to deliver Windows Virtual Desktop (WVD) based desktops and apps and your on premises resources to your users in a single place. Manage both the WVD environment in Azure and your on-premises environment from a single place in Citrix Cloud with Citrix Virtual Apps and Desktops service.

[Learn -> PoC Guides -> Microsoft Teams optimization in Citrix Virtual Apps and Desktops environments Proof of Concept Guide](/en-us/tech-zone/learn/poc-guides/microsoft-teams-optimizations.html) - Learn how to deliver the Citrix® HDX™ Optimization for Microsoft® Teams in a Citrix environment. The optimization offers clear, crisp high-definition video calls, audio-video or audio-only calls to and from other Teams users, optimized Teams’ users and other standards-based video desktop and conference room systems. Support for screen sharing is also available.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops-service.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop deployment in Citrix Cloud.

[Learn -> Diagrams and Posters -> Virtual Apps and Desktops On-Prem](/en-us/tech-zone/learn/diagrams-posters/virtual-apps-and-desktops.html) - Conceptual architecture drawing for Citrix Virtual Apps and Desktop on-premises deployment.

[Learn -> Tech Insight -> App Layering - User Layers](/en-us/tech-zone/learn/tech-insights/app-layering-user-layers.html) - User layers persist user profile settings, data, and user-installed applications in non-persistent VDI environments.

[Learn -> Tech Insight -> Remote PC Access](/en-us/tech-zone/learn/tech-insights/remote-pc-access.html) - Remote PC Access allows users to access their physical, office-based Windows PC from remote locations.

[Learn -> Tech Insight -> Virtual Apps and Desktops Service](/en-us/tech-zone/learn/tech-insights/virtual-apps-and-desktops-service.html) - Citrix Virtual Apps and Desktops Service provides a fast, low-impact deployment option for on-premises/cloud-hosted, Windows/Linux, desktops/apps.

[Learn -> Tech Insight -> Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) - Single Sign-on to Windows-based virtual apps and desktops when using a non-Active Directory based Citrix Workspace identity.

[Learn -> Tech Insight -> HDX - Display and Adaptive Technologies](/en-us/tech-zone/learn/tech-insights/hdx-adaptive-display.html) - Technologies that ensure an unparalleled user experience when connecting to remote Citrix resources.

[Design -> Reference Architectures -> Remote PC Access](/en-us/tech-zone/design/reference-architectures/remote-pc.html) - Discover the use cases and learn about the detailed architecture of Citrix Remote PC Access solution with the layered approach for on-premises and Citrix Cloud deployments.

[Design -> Reference Architectures -> Optimizing Unified Communications Solutions](/en-us/tech-zone/design/reference-architectures/optimizing-unified-communications-solutions.html) - Learn how to optimize voice, video, and other capabilities of unified communication solutions in virtualized Citrix environments.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) - Learn the architecture and deployment considerations for this cloud-based service of secure app and desktop delivery.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - Azure](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-azure.html) - Learn the detailed architecture and deployment model of Citrix Virtual Apps and Desktops on Microsoft Azure with five key architectural principles.

[Design -> Reference Architectures -> ServiceNow with Citrix Virtual Apps and Desktops](/en-us/tech-zone/design/reference-architectures/itsm-adapter-service.html) - Learn how to integrate ServiceNow within your Citrix Virtual Apps and Desktops environment including key technical concepts and use cases.

[Design -> Design Decisions -> Provisioning Model for Image Management](/en-us/tech-zone/design/design-decisions/image-management.html) - Learn about the different decision factors involved in choosing the right provisioning model for image management. Learn more about Citrix Provisioning and Machine Creation Services solutions.

[Design -> Design Decisions -> Remote PC Access](/en-us/tech-zone/design/design-decisions/remote-pc-access.html) - When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.

[Design -> Design Decisions -> HDX Graphics Overview](/en-us/tech-zone/design/design-decisions/hdx-graphics.html) - To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.

[Design -> Design Decisions -> Disaster Recovery Planning](/en-us/tech-zone/design/design-decisions/cvad-disaster-recovery.html) - Learn more about different decision factors and recommendations for business continuity and disaster recovery planning.

[Design -> Design Decisions -> Delivery Model Comparison](/en-us/tech-zone/design/design-decisions/delivery-model-comparison.html) - A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.

[Design -> Design Decisions -> Single Server Scalability](/en-us/tech-zone/design/design-decisions/single-server-scalability.html) - Learn about the magic formula to calculate how many users you can have on a single server, what are the different variables that has impact on scalability and recommendations to improve it.

[Design -> Reference Architectures -> Virtual Apps and Desktops Service - AWS](/en-us/tech-zone/design/reference-architectures/citrix-virtual-apps-and-desktops-on-aws.html) - Learn the architecture and deployment considerations of Citrix Virtual Apps and Desktops on Amazon Web Services cloud platform.

[Design -> Reference Architectures -> Image Management](/en-us/tech-zone/design/reference-architectures/image-management.html) - Gain an understanding of Machine Creation Services (MCS) and Citrix Provisioning (PVS) offerings for building, delivering and maintaining virtual machine images in your environment.

[Design -> Reference Architectures -> App Layering](/en-us/tech-zone/design/reference-architectures/app-layering.html) - Gain a deep understanding of the Citrix Layering technology that simplifies the image management for VDI and hosted-shared environments including use cases and technical concepts.

[Design -> Design Decisions -> Designing StoreFront and Gateway Integration](/en-us/tech-zone/design/design-decisions/storefront-gateway-integration.html) - Learn about different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.

[Design -> Design Decisions -> VDI Model Comparison](/en-us/tech-zone/design/design-decisions/vdi-model-comparison.html) - Selecting the best VDI model starts with properly defining user groups and  aligning the requirements with the capabilities of the VDI models. Learn how different factors plays a role in selecting the correct VDI model for a user group.

[Build -> Deployment Guides -> Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud](/en-us/tech-zone/build/deployment-guides/cvads-migration.html) - Learn how to migrate your on-premises Citrix Virtual Apps and Desktops environment to Citrix Cloud.

[Build -> Tech Papers -> Deploying Google Chrome](/en-us/tech-zone/build/tech-papers/google-chrome.html) - Tech Paper focused on installation, configuration, and various optimizations for Google Chrome browser running on Citrix Virtual Apps and Desktops.
