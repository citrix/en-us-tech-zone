---
layout: doc
description: Remote PC Access is easy to deploy. These design decisions help maintain security, availability and performance.
---
# Remote PC Access Design Decisions

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller), Nick Rintalan, Matthew Greenbaum, Simon Jackson

## Overview

Remote PC Access is an easy and effective way to allow users access their office-based, physical Windows PC. Using any endpoint device, users can remain productive regardless of their location. However, organizations should consider the following when implementing Remote PC Access.

## Authentication

Users continue to authenticate with Active Directory, but this authentication happens when the user initiates a connection to the organization's public fully qualified domain (FQDN). The FQDN requests authentication from Citrix Gateway.

Because the site is external, organizations require stronger authentication than a simple Active Directory user name and password. Incorporating multifactor authentication, like a time-based one-time password token, can greatly improve authentication security.

Citrix Gateway provides organizations with numerous multifactor authentication options, which include:

*  [Time-based One Time Password](/en-us/citrix-gateway/13/authentication-authorization/configure-onetime-passwords.html)
*  [RAIUS](/en-us/citrix-gateway/13/authentication-authorization/configure-radius.html)
*  [TACACS+](/en-us/citrix-gateway/13/authentication-authorization/configure-tacacs.html)
*  [SAML](/en-us/citrix-gateway/13/authentication-authorization/configure-saml.html)

Some of these options, like the time-based one time password, requires the user to initially register for a new token. As this option requires access to the user's email, it would need to be done before the user attempts to work remotely.

## Session Security

Users are able to remotely access the work PC with an untrusted, personal device. Organizations can use integrated Citrix Virtual Apps and Desktops policies to protect against:

*  Endpoint Risks: Key loggers secretly installed on the endpoint device can easily capture a user's username and password. Anti-keylogging capabilities protect the organization from stolen credentials by obfuscating keystrokes.
*  Inbound Risks: Untrusted endpoints can contain malware, spyware, and other dangerous content. Denying access to the endpoint device's drives prevents transmission of dangerous content to the corporate network.
*  Outbound Risks: Organizations must maintain control over content. Allowing users to copy content to local, untrusted endpoint devices places additional risks on the organization. These capabilities can be denied by blocking access to the endpoint's drives, printers, clipboard, and anti-screen-capturing policies.

## Infrastructure Sizing

***Note:** The following sizing recommendations are a good starting point, but each environment is unique, resulting in unique results.  Please monitor the infrastructure and size appropriately.*

In many implementations, organizations enable thousands of physical PCs for Remote PC Access capabilities.  Each PC must register with the Citrix Virtual Apps and Desktop controller. The controller must be able to accommodate the increase load.

As a general recommendation with regards to sizing

### Citrix Virtual Apps and Desktops

When using an on-premises deployment of Citrix Virtual Apps and Desktops, start with the following recommendations:

*  A 4 vCPU controller can handle 5,000 new sessions
*  A 4 vCPU StoreFront server supports 55,000 requests per hour

### Gateway Service

When using the Gateway Service from Citrix Cloud, start with the following recommendations:

*  A Citrix Cloud Connector can handle 1,000 sessions without using the rendezvous protocol
*  A Citrix Cloud Connector can handle 2,000 sessions when using the rendezvous protocol

The rendezvous protocol enables the virtual delivery agent installed on each physical PC to communicate directory with the Gateway Service instead of tunneling the session through the Cloud Connector.  The rendezvous protocol requires the 1912 version of the virtual delivery agent.

## Availability

Remote PC Access supports Wake-on-LAN operations to enable powering on Windows PC that are currently powered off.  However, this option requires the use of Microsoft Configuration Manager.

If Configuration Manager is not an option, administrators can configure an Active Directory Group Policy object to remove the "Shut Down" option from the Windows PC. This prevents the user from powering down the physical PC, allowing them to connect remotely.  

***Note:** Wake on LAN functionality is not available when using the cloud service - Citrix Virtual Apps and Desktops Service.*

If there is a power failure, the physical PC will be powered off.  If available, the PC's BIOS setting should include the configuration to automatically power on in the event of a power failure.  

## User Assignments

In order to have the remote experience match the local experience, users need to connect to their personal work PC. The user must get assigned to the correct physical PC.

User assignments can be automated. Once the virtual delivery agent installs on the physical PC, the next user who logs on will get assigned to that PC within Citrix Virtual Apps and Desktops. This is an effective method for assigning thousands of users.

The Citrix Virtual Apps and Desktops administrator can modify the assignments as needed within Citrix Studio.

## Agent Deployment

To deploy the virtual delivery agent to thousands of physical PCs, automated processes are required.

The install media for Citrix Virtual Apps and Desktops include a deployment script (InstallMedia\Support\ADDeploy\InstallVDA.bat) that can be leveraged by Active Directory Group Policy Objects.

The script can be used as a baseline for PowerShell scripts and Enterprise Software Deployments (ESD) tools. These approaches allows organizations to quickly deploy the agent to thousands of physical endpoints.
