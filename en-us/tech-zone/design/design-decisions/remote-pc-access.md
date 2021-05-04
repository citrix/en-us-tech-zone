---
layout: doc
h3InToc: true
contributedBy: Matthew Greenbaum, Simon Jackson, Nick Rintalan, Daniel Feller
description: Remote PC Access is easy to deploy. These design decisions help maintain security, availability, and performance.
tz_title: Remote PC Access
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: Remote PC Access

## Overview

Remote PC Access is an easy and effective way to allow users to access their office-based, physical Windows PC. Using any endpoint device, users can remain productive regardless of their location. However, organizations want to consider the following when implementing Remote PC Access.

## Deployment Scenarios

There are multiple ways to connect a PC to a user, each applicable to different scenarios.

### Office Workers

In many deployments, Remote PC Access gets deployed in an office worker scenario where one user is permanently assigned to one PC.

[![Office Workers](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_office-workers.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_office-workers.png)

This is the most common deployment scenario. To implement this use case, the administrator can use the Remote PC Access machine catalog type.

[![Office Workers](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-rpca.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-rpca.png)

### Computing Labs

In certain situations, users need to share a set of computing resources, often found in computing labs at schools, colleges, and universities. Users are randomly assigned to an available physical PC.

[![Computing Lab Users](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_computing-lab-users.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_computing-lab-users.png)

This type of configuration uses an unmanaged, randomly assigned, single-session OS, which is configured as follows:

*  In the Machine Catalog Setup Wizard, use **Single-session OS**

[![Office Workers](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-pooled.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-pooled.png)

*  Select **Machines that are not power managed**
*  Select **Another service or technology**

[![Office Workers](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-management.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-management.png)

*  Select **I want users to connect to a new (random) desktop each time they log on.**

[![Office Workers](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-assignment.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_machine-catalog-assignment.png)

Because there are often more users than PCs in a computing lab, [policies limiting session time](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-limits-policy-settings.html) are recommended.

## Authentication

Users continue to authenticate to their office-based PC with their Active Directory credentials. However, as they are accessing over the Internet from outside the office premises, organizations typically require stronger levels of authentication than just user name and password.

Citrix Workspace supports different authentication options to be selected including Active Directory + Token and Azure Active Directory. [Current Supported Authentication Options](/en-us/citrix-workspace/secure.html)

If Citrix Gateway is configured as the authentication option for Citrix Workspace, or if a customer chooses to use Citrix Gateway + Citrix StoreFront as an alternative to Citrix Workspace, then the much wider selection of [Citrix Gateway authentication options](/en-us/citrix-gateway/13/authentication-authorization.html) becomes available.

Some of these options, like the time-based one-time password, require the user to initially register for a new token. As token registration requires the user to access their email to validate their identity, it may need to be completed before the user attempts to work remotely.

## Session Security

Users can remotely access their work PC with an untrusted, personal device. Organizations can use integrated Citrix Virtual Apps and Desktops policies to protect against:

*  Endpoint Risks: Key loggers secretly installed on the endpoint device can easily capture a user name and password. [Anti-keylogging capabilities](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html) protect the organization from stolen credentials by obfuscating keystrokes.
*  Inbound Risks: Untrusted endpoints can contain malware, spyware, and other dangerous content. Denying access to the endpoint device's drives prevents transmission of dangerous content to the corporate network.
*  Outbound Risks: Organizations must maintain control over content. Allowing users to copy content to local, untrusted endpoint devices places extra risks on the organization. These capabilities can be denied by blocking access to the endpoint's drives, printers, clipboard, and anti-screen-capturing policies.

## Infrastructure Sizing

***Note:** The following sizing recommendations are a good starting point, but each environment is unique, resulting in unique results. Monitor the infrastructure and size appropriately.*

As users are accessing existing office PCs there is minimal extra infrastructure needed to support adding Remote PC Access, however it is important that the Control layer and Access layer infrastructure are sized and monitored correctly to ensure that they do not become a bottleneck.

### Control Layer

The Virtual Delivery Agent (VDA) on each Office PC must register with Citrix Virtual Apps and Desktops. For on-premises deployments VDA registration happens directly with a Delivery Controller, for Citrix Virtual Apps & Desktops Service in Citrix Cloud this registration happens via a Citrix Cloud Connector.

Sizing of Delivery Controllers or Cloud Connectors for Remote PC Access workloads is similar to VDI workloads. Citrix Consulting recommends at least N+1 availability. Guidance for Cloud Connector scaling including conditions where Local Host Cache may be required is available here

*  [Scale and size considerations for Cloud Connectors](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/cc-scale-and-size.html)
*  [Scale and size considerations for Local Host Cache](/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/local-host-scale-and-size.html)

### Access Layer

When a user establishes an HDX session to their office PC, the ICA traffic needs to be proxied to the VDA. ICA Proxy can be provided via Citrix Gateway appliances or Citrix Gateway Service.

When using an on-premises Citrix ADC for Citrix Gateway, consult the data sheet for the particular model and refer to the **SSL VPN/ICA proxy concurrent users line** item as a starting point. If the ADC is handling other workloads, validate that current throughput and CPU usage are not approaching any upper limits.

Ensure that there is adequate available Internet bandwidth where the Gateway appliance is located to support the expected concurrent ICA sessions.

When using the Gateway Service from Citrix Cloud, the ICA traffic flows between the Resource Location (where the VDAs and Cloud Connectors are located) directly to the Gateway service. The traffic is either proxied by the Cloud Connectors (default) or can flow directly from the VDA, bypassing the Cloud Connectors if the conditions to use the rendezvous protocol can be met.

When Cloud Connectors are used to proxy ICA traffic to the Gateway Service then this can be a bottleneck and careful monitoring of CPU & memory on the Cloud Connector VMs is advised. For initial planning estimates a 4 vCPU Citrix Cloud Connector VM can handle a maximum of 1000 concurrent ICA Proxy sessions.
Rendezvous protocol (when configured) enables the Virtual Delivery Agent installed on each physical PC to communicate directly with the Gateway Service instead of tunneling the session through the Cloud Connector.

When using Gateway Service, Citrix recommends using rendezvous protocol to mitigate the issue of Cloud Connectors being a bottleneck for ICA Proxy.

[![Rendezvous Protocol Policy](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_rendezvous-protocol-policy.png)

There are certain prerequisites to allow the [rendezvous protocol](/en-us/citrix-virtual-apps-desktops/technical-overview/hdx/rendezvous-protocol.html) to function, including:

*  Citrix Virtual Apps & Desktops Service
*  VDA version 1912 or higher
*  An enabled HDX Policy
*  DNS PTR Records for all VDAs
*  Specific SSL Cipher Suite Order
*  Direct (non-proxied) Internet connectivity from VDA to Gateway Service

## Availability

If the office PC is not powered on with the VDA registered, the user’s session cannot be brokered. Citrix recommends putting in place processes to ensure the machines that users need to connect to are powered-on.

If available, modify the PC’s BIOS setting to automatically power on in the event of a power failure. Administrators can also configure an Active Directory Group Policy object to remove the “Shut Down” option from the Windows **PC**. This helps prevent the user from powering down the physical PC.

Remote PC Access also supports [Wake-on-LAN](/en-us/citrix-virtual-apps-desktops/install-configure/remote-pc-access.html#wake-on-lan) operations to enable powering on Windows PCs that are currently powered off. This option requires the use of Microsoft System Center Configuration Manager 2012, 2012 R2 or 2016.

***Note:** The Microsoft Configuration Manager Wake-on-LAN hosting connection functionality is not available when using the Citrix Virtual Apps and Desktops Service in Citrix Cloud*

## User Assignments

It is important that users are each brokered to their own office PC. Once the VDA has been installed and the catalog and delivery group defined, users are automatically assigned when they next logon locally to the PC. This is an effective method for assigning thousands of users.

By default, multiple users can be assigned to a desktop if they have all logged into the same physical PC, but this can be disabled via a registry edit on the Delivery Controllers.

The Citrix Virtual Apps and Desktops administrator can modify the assignments as needed within Citrix Studio or via PowerShell. To get started with using PowerShell for adding Remote PC Access VDAs to a Site and assigning users, Citrix Consulting produced a reference script which can be found on the [Citrix GitHub page](https://github.com/citrix/remote-pc-load-script).

## Virtual Delivery Agent

This section reviews key considerations for handling the Virtual Delivery Agent package.

### Versions

Citrix Virtual Apps and Desktops administrators can use either the VDAWorkstationCoreSetup.exe package or the VDAWorkstationSetup.exe package with the /remotePC flag. The VDAWorkstationCoreSetup.exe package is smaller and only includes the core components required for Remote PC Access, but notably, in version 1912 and earlier, excludes the components required for content redirection (see the [Microsoft Teams](#microsoft-teams) section for further guidance).

#### Windows 7 and 8.1

Though Windows 7 is no longer supported, many organizations still have legacy Windows 7 desktop machines. For deployment on Windows 7 and Windows 8.1, customers should use the XenDesktop 7.15 LTSR VDA.

#### Windows 10

For deployment on Windows 10, customers should use the Citrix Virtual Apps and Desktops 1912 LTSR VDA or a supported Current Release VDA. Citrix version compatibility for Windows 10 builds can be found in [CTX224843](https://support.citrix.com/article/CTX224843).

### Helpful Command-line Options

There are several VDA installer command-line options to consider when deploying Remote PC Access that can enable useful functionality.

#### /remotePC

Used with the full VDA package, VDAWorkstationSetup.exe, to install only the core components required for Remote PC Access.

#### /enable_hdx_ports

Opens ports in the Windows firewall required by the Cloud Connector and enabled features (except Windows Remote Assistance), if the Windows Firewall Service is detected, even if the firewall is not enabled.

#### /enable_hdx_udp_ports

Opens UDP ports in the Windows firewall that are required by HDX adaptive transport, if the Windows Firewall Service is detected, even if the firewall is not enabled.

To open the ports that the VDA uses to communicate with the Controller and enabled features, specify the /enable_hdx_ports option, in addition to the /enable_hdx_udp_ports option.

#### /enable_real_time_transport

Enables or disables use of UDP for audio packets (RealTime Audio Transport for audio). Enabling this feature can improve audio performance.

To open the ports that the VDA uses to communicate with the Controller and enabled features, specify the /enable_hdx_ports option, in addition to the /enable_real_time_transport option.

#### /includeadditional "Citrix User Profile Manager","Citrix User Profile Manager WMI Plug in"

In a Remote PC Access deployment, most implementations do not require profile management. However, Citrix User Profile Manager also captures performance metrics, which are useful for administrators to identify and fix performance-related issues. User Profile Manager does not have to be configured, it just needs to be deployed to capture metrics.

When installed, the Citrix User Profile Manager allows administrators to run reports on the user experience, session responsiveness, and insights into logon performance within Citrix Director and Citrix Analytics for Performance.

[![Logon Performance Chart](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_logon-performance-chart.png)](/en-us/tech-zone/design/media/design-decisions_remote-pc-access_logon-performance-chart.png)

#### /logpath path

Log file location. The specified folder must exist as the installer does not create it. The default path is "%TEMP%\Citrix\XenDesktop Installer," but if the install is conducted via SCCM, then depending on the context, the log files can be in the system temp folder instead.

#### /optimize

Do NOT use this flag as it is intended mainly for MCS deployed machines.

### Deployment

To deploy the Virtual Delivery Agent to thousands of physical PCs, automated processes are required.

#### Via Scripting

The install media for Citrix Virtual Apps and Desktops includes a deployment script (`%InstallMedia%\Support\ADDeploy\InstallVDA.bat`) that can be used via Active Directory Group Policy Objects.

The script can be used as a baseline for PowerShell scripts and Enterprise Software Deployments (ESD) tools. These approaches allow organizations to quickly deploy the agent to thousands of physical endpoints.

#### Via SCCM

If you are automating the VDA installation with an ESD tool such as SCCM or Altiris, creating separate packages for the prerequisites and VDA tends to work best. You can find more information about VDA deployment with ESD tools in the [product documentation](/en-us/citrix-virtual-apps-desktops/install-configure/install-vdas-sccm.html).

## Microsoft Teams

If users access Microsoft Teams for voice and video calls, content redirection functionality is required to create a positive user experience.

For content redirection to be available when using VDA 1912 or older, it is required to deploy the VDA on the physical PCs using the single-session full VDA installer (standalone `VDAWorkstationSetup.exe`) with the `/remotepc` command line option.

For example: `VDAWorkstationSetup.exe /quiet /remotepc /controllers “control.domain.com” /enable_hdx_ports /noresume /noreboot`

If deploying VDA 2003 or newer, the single-session core VDA installer can be used instead (standalone `VDAWorkstationCoreSetup.exe`).

For example: `VDAWorkstationCoreSetup.exe /quiet /controllers “control.domain.com” /enable_hdx_ports /noresume /noreboot`

## Common Network Ports

Similar to any other Citrix VDA, there are a handful of key network ports to be mindful of opening for the system to function. As a reminder, ICA traffic needs to reach the Remote PC Access from the Citrix ADC hosting the external Citrix Gateway. A comprehensive list of ports can be found in [Communication Ports Used by Citrix Technologies](https://docs.citrix.com/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html).

## VDA Registration

Depending on the network topology, the subnet containing the Virtual Apps and Desktops Delivery Controllers might not allow communication to or from the physical PCs. To properly register with the Delivery Controller, the VDA on the PC must be able to communicate with the Delivery Controller in both directions using the following protocols:

*  VDA to Controller: Kerberos
*  Controller to VDA: Kerberos

If the VDA is unable to register with the controller, review the [VDA registration](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html) article. If you are using Citrix Cloud, the Cloud Connectors take the place of the Delivery Controller.

## Further Guidance

More design guidance including considerations and troubleshooting steps can be found in the [Remote PC Access product documentation](/en-us/citrix-virtual-apps-desktops/install-configure/remote-pc-access.html).
