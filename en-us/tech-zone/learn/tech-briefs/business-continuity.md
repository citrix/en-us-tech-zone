---
layout: doc
description: Copy & paste description from TOC here
---
# Business Continuity

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

## Business Continuity

Most organizations have defined business continuity plans. The success of a business continuity plan is based on how much it impacts the user experience, how well it scales to overcome global issues, and how well it maintains corporate security policies.

## Overview

Business continuity allows organizations to enable seamless workforce productivity during any kind of planned or unplanned disruption. The success of a business continuity plan is often based on the following user and business requirements:

User Requirements

*  Ability to access all apps and data to perform job
*  User experience remains the same
*  Ability to be productive with varying network conditions

Business Requirements

*  Ability to rapidly scale to support unexpected need
*  Protect corporate resources from untrusted endpoint devices
*  Easy to integrate with current infrastructure
*  Must not bypass security rules and policies

### VPN Risks

Most organizations will need a way to provide users with secure remote access to corporate resources without relying on the deployment of VPN-based solutions. VPN-based of solutions are risky because they:

*  VPN Risk 1: Are difficult to install and configure
*  VPN Risk 2: Require users install VPN software on endpoint devices, which might utilize an unsupported operating system
*  VPN Risk 3: Require the configuration of complex policies to prevent an untrusted endpoint device from having unrestricted access to the corporate network, resources, and data.
*  VPN Risk 4: Difficult to keep security policies synchronized between VPN infrastructure and on-premises infrastructure.

### Business Continuity Options

Depending on the state of the current infrastructure, an organization can opt for one of the following solutions to provide secure remote access to users during a business continuity event:

[![Business Continuity Options](/en-us/tech-zone/learn/media/tech-briefs_business-continuity_options.png)](/en-us/tech-zone/learn/media/tech-briefs_business-continuity_options.png)

## Remote PC Access

For many users, the work environment centers on a physical Windows 10 PC sitting under their desk. Remote PC Access allows a remote user to log into their physical Windows office PC using virtually any device (tablets, phones, and laptops using iOS, Mac, Android, Linux, and Windows).

Adding Remote PC Access to the business continuity strategy assumes the following:

*  A user’s workspace is based on domain-joined Windows PCs
*  User’s authenticate with Active Directory
*  There is minimal extra data center hardware capacity to accommodate a large virtual desktop (VDI) style deployment

When the user remotely accesses their work PC, the connection utilizes the ICA protocol, which dynamically adjusts to changing network conditions and content. This provides the best possible experience.

### New Deployment

Organizations can easily deploy Citrix Virtual Apps and Desktops to provide Remote PC Access to their environment with a minimal deployment footprint, as seen in the following.

[![Remote PC Access - New Deployment](/en-us/tech-zone/learn/media/tech-briefs_business-continuity_remote-pc-access-new.png)](/en-us/tech-zone/learn/media/tech-briefs_business-remote-pc-access-new.png)

To add a new environment, the administrator must deploy the following components:

*  Gateway: Secures connections between internal Windows PCs and untrusted end point devices via a reverse-proxy.
*  StoreFront: Provides users an enterprise app store used to launch sessions to authorized resources.
*  Delivery Controller: Authorizes and audits user sessions to Windows PCs.

These three components should be deployed with redundancy to overcome any single point of failure.

With a new infrastructure deployed, the administrator can perform the following to enable Remote PC Access:

*  Deploys the virtual desktop agent to physical Windows PCs
*  Creates a new Remote PC Access catalog
*  Assigns users to PCs

Although the virtual desktop agent can be installed on each physical PC manually, to simplify deployment of the virtual desktop agent, it is recommended to use electronic software distribution such as Active Directory scripts and Microsoft System Center Configuration Manager.

### Expanded Deployment

An organization can also easily add Remote PC Access to a current Citrix Virtual Apps and Desktops environment. This process effectively expands the conceptual architecture as follows:

[![Remote PC Access - New Deployment](/en-us/tech-zone/learn/media/tech-briefs_business-continuity_remote-pc-access-add.png)](/en-us/tech-zone/learn/media/tech-briefs_business-remote-pc-access-add.png)

To add Remote PC Access to a current Citrix Virtual Apps and Desktops environment, the administrator simply does the following:

*  Deploys the virtual desktop agent to physical Windows PCs
*  Creates a new Remote PC Access catalog
*  Assigns users to PCs

Because users simply access their physical work PC, the organization only needs to consider additional hardware for the access and control layers. These components must be able to accommodate the influx of new users’ requests during a business continuity event.

### Results

Because Remote PC Access allows users to connect to their standard Windows PC during a business continuity event, organizations are able to:

*  Provide users access to all apps and data to perform job. Everything on the user’s Windows PC is accessible with Remote PC Access.
*  Maintain the user experience between normal operations and business continuity events. Users continue to use the same Windows PC in all situations.
*  Remain productive, regardless of location and network conditions. The ICA protocol connecting the user’s end point device to the Windows PC dynamically adjusts based on network conditions to provide the most responsive experience possible.
*  Rapidly scale to support unexpected need. Once the agent gets deployed to the Windows PCs, the administrator can simply enable the Remote PC Access capability.
*  Protect corporate resources from untrusted endpoint devices. Gateway creates a reverse proxy between the end point and work PC. With session policies, administrators can block users from transferring data to/from the work PC and corporate network.
*  Easily integrate with the current infrastructure. Remote PC Access is simply a different type of virtual desktop within the Citrix Virtual Apps and Desktops solution.
*  Maintain the same security profile during a business continuity event. Remote PC Access connects users to their office-based Windows PC. Users have the ability to access the same resources, the same way like they were physically in the office.
