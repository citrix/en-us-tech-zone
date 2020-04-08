---
layout: doc
---
# Baseline Policy Design

## Contributors

**Author:** [Floris van der Ploeg](https://www.linkedin.com/in/fvdploeg/)

## Overview

Policies provide the basis to configure and fine-tune Citrix Virtual Apps and Desktops environments. This allows organizations to control connection, security, and bandwidth settings based on various combinations of users, devices, or connection types.

When making policy decisions, consider both Microsoft and Citrix policies to ensure that all user experience, security, and optimization settings are included. For a list of all Citrix-related policies, refer to the [Citrix Policy Settings Reference](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/policies/reference.html).

## Decision: Preferred Policy Engine

Organizations can configure Citrix policies either via Citrix Studio or through Active Directory group policy. Active Directory policies use Citrix ADMX files, which extend group policy and provide advanced filtering mechanisms.

Using Active Directory group policy allows organizations to manage both Windows policies and Citrix policies in the same location, and minimizes the administrative tools required for policy management. Group policies are automatically replicated across domain controllers, protecting the information and simplifying policy application.

Use the Citrix administrative consoles if Citrix administrators do not have access to Active Directory policies. Select the method which is most appropriate for the organization’s needs and use that method consistently. Using a single method avoids confusion with multiple Citrix policy locations.

It is important to understand how the aggregation of policies, known as policy precedence flows to understand how a resultant set of policies is created. With Active Directory and Citrix policies, the precedence is as follows:

| Policy Precedence | Policy Type |
| :--- | :--- |
| Processed first (lowest precedence) | Local machine policies |
| Processed second | Citrix policies created using the Citrix administrative consoles |
| Processed third | Site level AD policies |
| Processed fourth | Domain level AD policies |
| Processed fifth | Highest level OU in domain |
| Processed sixth and subsequent | Next level OU in domain |
| Processed last (highest precedence) | Lowest level OU containing object |

Policies from each level are aggregated into a final policy that is applied to the user or computer. In most enterprise deployments, Citrix administrators do not have permissions to change policies outside their specific OUs, which is typically the highest level for precedence. Use the "block inheritance" and "no override" settings to manage policy settings applied from higher up the OU tree, in cases where exceptions are required. Block inheritance stops settings from higher-level OUs (lower precedence) from being incorporated into the policy. However, if a higher-level OU policy is configured with no override, the block inheritance setting is not applied. For this reason, care must be taken in policy planning. Use available tools such as the "Active Directory Resultant Set of Policy" or the "Citrix Group Policy Modeling Wizard" to validate the observed outcomes with the expected outcomes.

**Note:** *some Citrix policy settings, if used, need to be configured through Active Directory group policy. For example, Delivery Controllers and Delivery Controller registration port, are required for registration of the VDAs.*

## Decision: Policy Integration

When configuring policies, organizations often require both Active Directory policies and Citrix policies to create a fully configured environment. With the use of both policy sets, the resultant set of policies can become confusing to determine. Sometimes, particularly concerning Windows Remote Desktop Services (RDS) and Citrix policies, similar functionality can be configured in two different locations. For example, it is possible to enable client drive mapping in a Citrix policy and disable client drive mapping in an RDS policy. The ability to use the desired feature may be dependent upon the combination of RDS and Citrix policy. It is important to understand that Citrix policies build upon functionality available in Remote Desktop Services. If the required feature is explicitly disabled in the RDS policy, the Citrix policy is not able to affect a configuration.

To avoid this confusion, it is recommended that RDS policies only be configured where required and there is no corresponding policy in the Citrix Virtual Apps and Desktops configuration. Only configure RDS policies if the configuration is needed for RDS use within the organization. Configuring policies at the highest common denominator simplifies the process of understanding the resultant set of policies and troubleshooting policy configurations.

## Decision: Policy Scope

Once policies have been created, they need to be applied to groups of users, computers, or both, based on the required outcome. Policy filtering allows policies to be applied to the required user or computer groups. With Active Directory-based policies, a key decision is whether to apply a policy to computers or users within the site, domain, or organizational unit (OU) objects. Active Directory policies are broken down into user configuration and computer configuration. By default, the settings within the user configuration apply to users who reside within the OU at logon. Settings within the computer configuration are applied to the computer at system startup and affect all users who log on to the system. One challenge of policy association with Active Directory and Citrix deployments revolves around three core areas:

-  **Citrix environment-specific computer policies**  
  Citrix servers and virtual desktops often have computer policies that are created and deployed specifically for the environment. Applying these policies is easily accomplished by creating separate OU structures for the servers and the virtual desktops. Specific policies can then be created and confidently applied to only the computers within the OU and below and nothing else. Based on requirements, virtual desktops and servers may be further subdivided within the OU structure based on server roles, geographical locations, or business units.
-  **Citrix specific user policies**  
  When creating policies for Citrix Virtual Apps and Desktops there are several policies specific to user experience and security that are applied based on the user’s connection. However, the user’s account might be located anywhere within the Active Directory structure, creating difficulty with simply applying user configuration based policies. It is not desirable to apply the Citrix specific configurations at the domain level as the settings would be applied to every system any user logs on to. Applying the user configuration settings at the OU where the Citrix servers or virtual desktops are located also doesn't work. The user accounts are not located within that OU, therefore the settings not applied to the users. The solution is to apply a loopback policy. A loopback policy is a computer configuration policy that forces the computer to apply the assigned user configuration policy of the OU to any user who logs on to the server or virtual desktop. The user’s location within Active Directory does not affect the applied settings in a loopback policy. Loopback processing can be applied to either merge or replace mode. Using replace mode overwrites the entire user GPO with the policy from the Citrix server or virtual desktop OU. Merge mode combines the user GPO with the GPO from the Citrix server or desktop OU. As the computer GPOs are processed after the user GPOs when merge mode is used, the Citrix related OU settings will have precedence. This means that Citrix related OU settings are applied in the event of a conflict. For more information, refer to the Microsoft support article [Loopback processing of Group Policy](https://support.microsoft.com/en-us/help/231287/loopback-processing-of-group-policy).
-  **Active Directory policy filtering**  
  In more advanced cases, there may be a need to apply a policy setting to a small subset of users such as Citrix administrators. In this case, loopback processing does not work, as the policy should only be applied to a subset of users, not all users who log on to the system. Active Directory policy filtering can be used to specify specific users or groups of users to which the policy is applied. A policy can be created for a specific function. Additionally, a policy filter can be set to apply that policy only to a group of users such as Citrix administrators. Policy filtering is accomplished using the security properties of each target policy.

Citrix policies created using Citrix Studio have specific filter settings available, which may be used to address policy-filtering situations that cannot be handled using group policy. Citrix policies may be applied using any combination of the following filters:

| Filter Name | Filter Description | Scope |
| :--- | :--- | :--- |
| Access control | Applies a policy based on the access control conditions through which a client connects. For example, users connecting through a Citrix NetScaler Gateway can have specific policies applied. | User settings |
| Client IP address | Applies a policy based on the IPv4 or IPv6 address of the user device used to connect to the session. Care must be taken with this filter if IPv4 address ranges are used to avoid unexpected results. | User settings |
| Client name | Applies a policy based on the name of the user device from which the session is connected. | User settings |
| Delivery Group | Applies a policy based on the delivery group membership of the desktop or server running the session. | User and computer settings |
| Delivery Group type | Applies a policy based on the type of machine running the session. For example, different policies can be set depending on whether a machine is private or shared. | User and computer settings |
| NetScaler SD-WAN | Applies a policy based on whether a session is launched through NetScaler SD-WAN. | User settings |
| Organizational Unit (OU) | Applies a policy based on the Organizational Unit (OU) of the desktop or server running the session. | User and computer settings |
| Tag | Applies a policy based on any tags applied to the machine running the session. Tags are strings that can be added to items, such as machines, in Citrix Virtual Apps and Desktops environments. These tags can be used to search for or limit access to desktops.  | User and computer settings |
| User or group | Applies a policy based on the user or group membership of the user connecting to the session. | User settings |

**Note:** *Policies in a Citrix Virtual Apps and Desktops environment provide a merged view of settings that apply at the user and computer level. In the previous table, the **Scope** column identifies whether the specified filter applies to user settings, computer settings, or both.*

## Decision: Baseline Policy

A baseline policy contains all common elements required to deliver a high-definition experience to most users within the organization. A baseline policy creates the foundation for user access and any exceptions that are needed to address specific access requirements for groups of users. To create the simplest policy structure possible, configure the policy settings in the baseline to be comprehensive enough to accommodate as many use cases as possible and set the priority to lowest priority, for example, 99 (a priority number of "1" is the highest priority). Use Citrix's unfiltered policy collection as the default policy for establishing the baseline policy, as it refers to all users and connections. Enable all Citrix policy settings in the baseline configuration, even if those settings are configured with the default value to explicitly define desired behavior and avoid confusion if the default settings change. Use Citrix policy templates to configure Citrix policies to effectively manage the end-user experience within an environment. Citrix Policy templates are a solid initial starting point for a baseline policy. Templates consist of pre-configured settings that optimize performance for specific environments or network conditions. The built-in templates included in Citrix Virtual Apps and Desktops are shown in the following table:

| Template name | Description |
| :--- | :--- |
| High Server Scalability | Includes settings to provide an optimal user experience while hosting more users on a single server. |
| High Server Scalability-Legacy OS | Equal to the "High Server Scalability" template, apply this policy to VDAs running a legacy Operating System like Windows 2008R2 or Windows 7 and earlier. |
| Optimized for NetScaler SD-WAN | Includes settings to provide an optimized experience to users working from branch offices with NetScaler SD-WAN deployed.  |
| Optimized for WAN | Includes settings for providing an optimized experience to users with low-bandwidth or high-latency connections. |
| Optimized for WAN-Legacy OS | Equal to the "Optimized for WAN" template, apply this policy to VDAs running a legacy Operating System like Windows 2008R2 or Windows 7 and earlier. |
| Security and Control | Includes settings for disabling access to peripheral devices, drive mapping, port redirection, and Flash acceleration on user devices. |
| Very High Definition User Experience | Includes settings for providing high-quality audio, graphics, and video to users. |

For more information on Citrix policy templates, refer to Citrix Docs - [Policy Templates](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/policies/policies-templates.html).

Include Windows policies in a baseline policy configuration. Windows policies reflect user-specific settings that optimize the user experience and remove features that are not required or desired in a Citrix Virtual Apps and Desktops environment. For example, one common feature turned off in these environments is Windows Update. In virtualized environments, particularly where desktops and servers are streamed and non-persistent, Windows Update creates processing and network overhead. Changes made by the update process will not persist after a restart of the virtual desktop or application server. In many cases, organizations use Windows Software Update Service (WSUS) to control Windows Updates. In these cases, updates are applied to the master disk and made available by the IT department on a scheduled basis.

In addition to the preceding considerations, an organization’s final baseline policy includes settings to address the organization's requirements. These requirements can be related to security, common network conditions, or to manage user devices or user profiles.
