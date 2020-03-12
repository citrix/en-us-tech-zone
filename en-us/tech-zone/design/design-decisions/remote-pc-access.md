---
layout: doc
description: When deploying a Remote PC Access solution, learn about the impact it will have on authentication, performance, scalability, and deployment.
---
# Remote PC Access Design Decisions

## Contributors

**Author:** Matthew Greenbaum, Simon Jackson, Nick Rintalan, [Daniel Feller](https://twitter.com/djfeller)

## Overview

Remote PC Access is an easy and effective way to allow users access to their office-based, physical Windows PC. Using any endpoint device, users can remain productive regardless of their location. However, organizations want to consider the following when implementing Remote PC Access.

## Authentication

Users continue to authenticate with Active Directory, but this authentication happens when the user initiates a connection to the organization's public fully qualified domain (FQDN). The FQDN requests authentication from Citrix Gateway.

Because the site is external, organizations require stronger authentication than a simple Active Directory user name and password. Incorporating multifactor authentication, like a time-based one-time password token, can greatly improve authentication security.

Citrix Gateway provides organizations with numerous multifactor authentication options, which include:

*  [Time-based One Time Password](/en-us/citrix-gateway/13/authentication-authorization/configure-onetime-passwords.html)
*  [RADIUS](/en-us/citrix-gateway/13/authentication-authorization/configure-radius.html)
*  [TACACS+](/en-us/citrix-gateway/13/authentication-authorization/configure-tacacs.html)
*  [SAML](/en-us/citrix-gateway/13/authentication-authorization/configure-saml.html)

Some of these options, like the time-based one time password, requires the user to initially register for a new token. As this option requires access to the user's email, it would need to be done before the user attempts to work remotely.

## Session Security

Users are able to remotely access the work PC with an untrusted, personal device. Organizations can use integrated Citrix Virtual Apps and Desktops policies to protect against:

*  Endpoint Risks: Key loggers secretly installed on the endpoint device can easily capture a user's user name and password. Anti-keylogging capabilities protect the organization from stolen credentials by obfuscating keystrokes.
*  Inbound Risks: Untrusted endpoints can contain malware, spyware, and other dangerous content. Denying access to the endpoint device's drives prevents transmission of dangerous content to the corporate network.
*  Outbound Risks: Organizations must maintain control over content. Allowing users to copy content to local, untrusted endpoint devices places extra risks on the organization. These capabilities can be denied by blocking access to the endpoint's drives, printers, clipboard, and anti-screen-capturing policies.

## Infrastructure Sizing

***Note:** The following sizing recommendations are a good starting point, but each environment is unique, resulting in unique results. Please monitor the infrastructure and size appropriately.*

In many implementations, organizations enable thousands of physical PCs for Remote PC Access capabilities. Each PC must register with the Citrix Virtual Apps and Desktop controller. The controller must be able to accommodate the increase load. Also, Citrix Gateway must be sized appropriately to handle the ICA traffic for the new users.

As a general recommendation with regards to sizing

### Citrix Virtual Apps and Desktops

When using an on-premises deployment of Citrix Virtual Apps and Desktops, start with the following recommendations:

*  A 4 vCPU controller can handle 5,000 new sessions
*  A 4 vCPU StoreFront server supports 55,000 requests per hour

### Gateway

 When using an on-premises Citrix ADC for Citrix Gateway, consult the data sheet for the particular model and refer to the SSL VPN/ICA proxy concurrent users line item as a starting point. If the ADC is handling other workloads, validate that current throughput and CPU usage are not approaching any upper limits

### Gateway Service

When using the Gateway Service from Citrix Cloud, sizing is irrelevant as it is managed by Citrix. However, this type of implementation requires on-premises Citrix Cloud Connectors.

The rendezvous protocol enables the Virtual Delivery Agent installed on each physical PC to communicate directly with the Gateway Service instead of tunneling the session through the Cloud Connector. The rendezvous protocol requires the 1912 version of the Virtual Delivery Agent.

[![Rendezvous Protocol Policy](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)

It is disabled by default and can be enabled with a Virtual Apps and Desktops policy.

For Remote PC Access, the following can be used for initial planning estimations:

*  A 4 vCPU Citrix Cloud Connector can handle 1,000 concurrent sessions without using the rendezvous protocol. Without the rendezvous protocol, traffic funnels through the Cloud Connector on the way to the Gateway Service.
*  A 4 vCPU Citrix Cloud Connector sizing becomes irrelevant when using the rendezvous protocol as the ICA traffic flows directly between the Gateway Service and the PC.

## Availability

Remote PC Access supports Wake-on-LAN operations to enable powering on Windows PC that are currently powered off. However, this option requires the use of Microsoft Endpoint Configuration Manager.

If Configuration Manager is not an option, administrators can configure an Active Directory Group Policy object to remove the "Shut Down" option from the Windows PC. This prevents the user from powering down the physical PC, allowing them to connect remotely.

***Note:** Wake on LAN functionality is not available when using the cloud service - Citrix Virtual Apps and Desktops Service.*

If there is a power failure, the physical PC will be powered off. If available, modify the PC's BIOS setting to automatically power on in the event of a power failure.

## User Assignments

To have the remote experience match the local experience, users need to connect to their personal work PC. The user must get assigned to the correct physical PC.

User assignments can be automated. Once the Virtual Delivery Agent installs on the physical PC, the next user who logs on will get assigned to that PC within Citrix Virtual Apps and Desktops. This is an effective method for assigning thousands of users.

By default, multiple users can be assigned to a desktop if they have all logged into the physical PC, but this can be disabled via a [registry edit](https://support.citrix.com/article/CTX137805) on the Delivery Controllers.

The Citrix Virtual Apps and Desktops administrator can modify the assignments as needed within Citrix Studio or via PowerShell.

## Agent Deployment

To deploy the Virtual Delivery Agent to thousands of physical PCs, automated processes are required.

The install media for Citrix Virtual Apps and Desktops include a deployment script (InstallMedia\Support\ADDeploy\InstallVDA.bat) that can be leveraged by Active Directory Group Policy Objects.

The script can be used as a baseline for PowerShell scripts and Enterprise Software Deployments (ESD) tools. These approaches allow organizations to quickly deploy the agent to thousands of physical endpoints.

## VDA Registration

Depending on the network topology, the subnet containing the Virtual Apps and Desktops delivery controllers might not allow communication from the physical PCs. To properly register with the delivery controller, the VDA on the PC must be able to communicate with the delivery controller over ports:

*  TCP port 80 if communication unsecured
*  TCP port 443 if communication secured (preferred configuration)
