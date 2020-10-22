---
layout: doc
h3InToc: true
contributedBy: Sarah Steinhoff
description: Learn about the different decisions involved when aggregating and de-duplicating applications and desktops from multiple sites.
---
# Designing StoreFront Multi-Site Aggregation

A core functionality of StoreFront is the ability to aggregate and de-duplicate "common" application and desktop resources from multiple Citrix Virtual Apps and Desktops (CVAD) Sites. This functionality is commonly referred to as multi-site aggregation. Duplicate applications and desktops are identified based on matching `Application Display Name` and `Application Category` properties. This functionality has been available in the console as of version 3.5 and was previously a config file edit. The purpose of multi-site aggregation is to allow Citrix administrators to build redundant CVAD Sites (for scalability or failure domain reasons), yet present a single application or desktop icon to users instead of per-Site duplicates (as would be displayed without this feature). StoreFront then controls how application and desktop launches are distributed across the multiple Sites supporting that resource.

This article reviews how those settings can be implemented in an enterprise environment and how they integrate with other related settings, such as application keywords and subscriptions, to further control how user sessions are routed and resources are presented.

## Overview

The configuration of multi-site settings is done through the “Manage Delivery Controllers” wizard in the StoreFront console, with various configurations detailed [in product documentation](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html). If more than one Site is configured for the Store, the **Configure** button under “User Mapping and Multi-Site Aggregation Configuration” becomes enabled as shown in the following.

![Configuration Options](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_1.png)

Once selected, an administrator is prompted to configure user mappings and resource aggregation as shown in the following. The “Map users to Controllers” link prompts the administrator to configure a user group to which these settings are to be applied. The “Aggregate resources” link is where the actual multi-site aggregation settings are specified.

![User Mapping and Resource Aggregation](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_2.png)

Multi-site aggregation cannot be configured until at least one user group has been defined to apply the settings to. The built-in “Everyone” group can be used if a single set of aggregation settings is to be applied to all users who connect to the Store. Once these settings are configured, if a user is not a member of one of the groups specified here, no applications or desktops are enumerated for that user. The next two sections review the available configurations in detail, starting with the “Aggregate resources” options.

## Aggregation options

In the Aggregate resources section, an administrator is presented with all Sites that are enumerated and prompted to select which have overlapping resources and are aggregated, as shown in the following.

![Aggregate Resources settings](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_3.png)

For the aggregated Sites, there are two more configurations that control how resources are enumerated and sessions are launched across those Sites. These settings apply to all Sites marked for aggregation:

-  **Controllers publish identical resources**: This setting controls enumeration. If two Sites are marked as “identical,” StoreFront places the farms in the same “equivalent farm set,” which means that enumeration requests are load balanced (round robin) across the Sites as the resource sets are assumed to be equivalent, saving enumeration time. If the two Sites are not marked as “identical,” then StoreFront sends an XML enumeration request to all and de-duplicates the common application and desktops from the resulting resource sets.
-  **Load balance resources across controllers**:  This setting controls session launch. Launch requests are either load balanced across the Sites or distributed in failover order, regardless of whether the Sites are “identical” or not. Session sharing takes precedence over a load balancing decision. Therefore, if a user already has a session in Site B and launches another applications or desktop, that session will also launch in Site B (assuming the application or desktop is available there).

Some use cases that are not handled well through the GUI are combinations of load balancing and failover or combinations of identical and non-identical sites. For instance, if there are two production sites that need to be load balanced and one DR site that is only be used if both production sites are down, the GUI cannot be used and the web.config file must be manually modified instead (refer to [Citrix Docs](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html) for the proper format for this file).

### Design Takeaways

To summarize the previous section:

-  Only one of a set of “identical” Sites is used to enumerate applications and desktops when the Sites are aggregated through StoreFront
-  Session launches can either be load balanced or failed over across aggregated Sites

## User Farm Mapping

The “Map users to controllers” settings are commonly referred to as “User Farm Mapping” as they are used to control which Sites a given group of users are allowed to enumerate against, whether those Sites are aggregated or not. There are two primary use cases for this functionality:

1.  **Limiting Enumeration**: even without multi-Site aggregation, assigning a group of users in StoreFront to a subset of Sites means that StoreFront will only send XML enumeration requests to those Sites when a user from that group authenticates. In a global deployment, this situation can have a significant impact on enumeration time as it prevents StoreFront from attempting to communicate with Sites that a given user does not need to be accessing anyway. For instance, these settings can be used to map US users to US-based CVAD Sites such that StoreFront can not reach other globally dispersed sites when those users log in.
2.  **Assigning Aggregation Settings**: if multi-Site aggregation is configured, it can be desirable to assign different configurations to different groups of users, such as different failover configurations or different combinations of Sites.

The “Map users to controllers” first prompts administrators to specify a user group and then the Sites that the user group is assigned to (although the dialog says “Controllers,” it is Sites as defined for the Store). There is a column that denotes whether the selected Sites have been previously configured for aggregation or not as shown in the following. Also, if the aggregated Sites were configured for failover (“Load balance resources across controllers” was left cleared), the order of the Sites can be specified here via the arrows on the right.

![User Mapping settings](/en-us/tech-zone/design/media/design-decisions_storefront-multisite-aggregation_4.png)

It is possible for a user to be part of multiple user group mappings. When StoreFront reads the list of configured user groups, it does not stop processing after finding a match for a user. All configured groups are processed and all returned Sites enumerated against. This scenario can be expected in the case of Citrix administrators, who typically have access to all applications and desktops and are members of multiple user groups to be able to test and reproduce reported user issues. This just means that administrators (or other users that are members of multiple groups) might experience different launch behavior that users in one of the groups because they have multiple configurations applied to them.
Configuration-wise, the only consideration is if two user groups have access to the same set of Sites with different aggregation settings. In the context of an individual user, a Site can only belong to one aggregation group or an error will be displayed. This is resolved by naming the aggregation groups the same between the two user group mappings, which is done by default through the GUI, but can be a consideration if the web.config file is being manually modified for a more advanced set of configurations as previously referenced.

### Design Takeaways

To summarize the previous section:

-  Assigning user groups to Sites, even without aggregation configured, can be used to help limit enumeration traffic and thus reduce the time to completion for this process
-  Failover order for aggregated Sites is specified in the user group assignment wizard
-  When multiple user group mappings are configured containing some of the same aggregated Sites and users belong to multiple groups, the aggregation groups must be named the same

## Application Keywords

Another way to control resource display and launch is by using keywords in the application description field. Within Store settings, display can be filtered based on custom keywords, as documented [in article CTX223451](https://support.citrix.com/article/CTX223451). There are also a few special, pre-defined keywords that have unique functions. Two of these are “primary” and “secondary,” which influence session launch across multiple Sites. When there are two instances of the same application published, the one with the keyword “primary” specified will always be preferred over the one with the keyword “secondary.”  This setting overrides any Site aggregation settings covered in the previous sections, which means these settings are frequently used together.

For instance, with two CVAD Sites, Site A and Site B, almost all applications need to be launched out of Site A (based on the back-end application architecture) and failed over to Site B, but there are a couple of applications that are primarily hosted out of Site B. Multi-Site aggregation would be configured for all users in failover order with Site A listed first. The exceptional applications that primarily are hosted out of Site B would be configured with the keyword primary in Site B and secondary in Site A.

### Design Takeaways

To summarize the previous section:

•  The “primary” and “secondary” keywords can be used to further control application launch across multiple Sites and will take precedence over Site aggregation settings

## Subscriptions

Subscriptions in StoreFront are how users are allowed to “favorite” applications and are stored in a local database on each StoreFront server, replicated automatically across a server group. By default, subscription records are stored in the format `<SiteName>.<DisplayName>`. This is because StoreFront will by default enumerate resources from all CVAD Sites and display them individually. If two resources happen to be published with the same name in different Sites, subscriptions to them are tracked individually. Therefore, we need some kind of Site tag to differentiate those subscriptions.

This changes with Site aggregation where identical resources across Sites are not displayed and tracked individually, but aggregated behind the same subscription shortcut. The Site tag is no longer meaningful because a single subscription record must cover the same resource from multiple Sites. Therefore, when Site aggregation is configured, the subscription records for aggregated resources are stored in the format `<AggregationGroup>.<DisplayPath>\<DisplayName>`.

This means that if StoreFront multi-Site aggregation is configured after the Sites were previously available individually (with subscriptions enabled), any user subscriptions for newly aggregated resources will disappear because of the change in subscription record format. The previous subscription record is no longer valid as the application is no longer tied to a Site, but an aggregation group. A workaround for this is to export the subscription database via PowerShell, replace all records for resources that will be aggregated with the proper format, and reimport the subscription database after the configuration of Site aggregation. Otherwise, users have to resubscribe to their applications.

### Design takeaways

To summarize the previous sections:

-  Multi-Site aggregation changes the format of user subscriptions in the subscription database, which must be considered if aggregation is implemented post go-live

## References

[Product Documentation: StoreFront Multi-Site Settings](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html)

[StoreFront Keywords Usage](https://support.citrix.com/article/CTX223451)
