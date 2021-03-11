---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Wayne Liu
description: Learn how Workspace Environment Management uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops sessions, as well as enhances the security of the deployment.
---
# Workspace Environment Management

## Introduction

Workspace Environment Management (WEM) uses intelligent resource management and Profile Management technologies to deliver the best possible performance, desktop logon, and application response times for Citrix Virtual Apps and Desktops(CVAD) deployments. WEM has several security features that bolster the security posture of the deployment. It is a software-only, driver-free solution.

## Architecture

Workspace Environment Management can be installed on-premises or accessed as a service from Citrix Cloud.

WEM pushes profile settings to the users and machines running the session, it obtains the data from the organization’s **Active Directory**.

**WEM Agent** - The WEM agent is common to both the on-premises and service deployment options. It is installed on Windows session hosts or physical machines that WEM manages. WEM agent monitors the host in real time and reports the state of the machine. It receives instructions from the WEM infrastructure services to apply policy settings on the machine and configure it. The agents maintain a local cache of the settings to be resilient to situations where the WEM server / service is unreachable.

### On-premises deployment

The on-premises WEM deployment architecture:

[![Workspace Environment Management on-premises architecture](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_wem-on-premises-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_wem-on-premises-architecture-large.png)

The other components of the on-premises architecture are:

**WEM Administration console** – Deployed on a single or multi-session Windows OS machine the console it is used to manage WEM. It interacts with the infrastructure server and allows admins to control the various features.

**WEM Infrastructure server** – Installed on a multi-session Windows OS machine, the services facilitate the communication and synchronization between the various components of the WEM deployment.

**SQL Server database** – WEM requires an SQL Server database to store its settings. The database can be hosted in an SQL Server Always On availability group if necessary.

### WEM service

The WEM service deployment architecture:

[![Workspace Environment Management service architecture](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_wem-service-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_wem-service-architecture-large.png)

When the Citrix Cloud hosted **WEM service** is used, the control and database components are hosted in the cloud and managed by Citrix. The admin console, the infrastructure services, and the database (hosted in Azure SQL) are part of the service. The admin console is accessed by the **Manage** tab when accessing the WEM service from the Citrix Cloud web console.

**Cloud Connectors** – A Citrix Cloud Connector is the conduit for communication from entities in a resource location with the Citrix Cloud services. For resiliency we suggest that at least a pair of cloud connectors is installed in each resource location. The WEM service uses the Cloud Connectors to access the customer Active Directory and authenticate the WEM agents in the resource location.

**WEM agent** - It interacts with the WEM services over HTTPS using the Citrix Cloud Messaging Service. The agents maintain a local cache of the settings to be resilient to network interruptions and service outages. As of writing of this article, a single service instance can support 100,000 WEM agents. Check [the limits page](/en-us/workspace-environment-management/service/limits.html) to get the current number of supported WEM agents per WEM service instance.

### Migrating WEM on-premises to the WEM service

The migration involves exporting the on-premises SQL server contents and uploading them to the service. A toolkit is run to migrate the existing on-premises WEM database to the WEM service. The toolkit includes a wizard to generate an SQL file containing the content of the on-premises WEM database. An admin can then upload the SQL file to the WEM service Azure database. For more information, read [Migrate to WEM Service](/en-us/workspace-environment-management/service/migrate.html).

## WEM Features

Having understood the architecture of both the on-premises and service offerings of WEM, lets take a look at the features that WEM makes available to organizations. There are three main feature sets:

1.  [Resource Management](#resource-management)

1.  [Profile Management](#profile-management)

1.  [Security features](#security-features)

## Resource Management

To provide the best experience for users the WEM agent monitors and analyzes user and application behavior within the session, in real time. It then intelligently adjusts RAM, CPU, and I/O in the user workspace environment.

### RAM optimization

When a new process is launched, it takes up more RAM than it needs for its normal running. But generally, the process does not relinquish these resources once they are allocated to it. WEM in real time detects which processes are in focus of the user. A portion of the RAM working set of apps that are not in focus can then be reclaimed. It is observed that even if these apps come back into focus, they generally need a smaller subset of the amount of RAM that was reclaimed from them. These actions optimize RAM consumption in the cloud and increase single server scalability.

The following graph shows the amount of memory consumed by a set of sessions, with and without WEM.

![WEM RAM optimization](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_ram-optimization.png)

### CPU optimization

If a process is detected to be hogging CPU resources, this can negatively affect not only the session that it is running in, but also slow down other sessions running on the same machine and even impact log on times for other users.

CPU optimization with WEM, involves real-time monitoring of the process running on each VM. When a process is detected to be hogging CPU resources (for a defined amount of time), WEM automatically reduces the priority of the process. This action allows other process to use the CPU and alleviates server load. When the process is seen to have returned to low CPU consumption overtime, then its priority is reset back to normal.

![WEM CPU Optimization](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_cpu-optimization.gif)

To validate the effect of CPU optimization by WEM, in a noisy neighbor scenario, these scale tests were extended. To simulate a noisy neighbor, a user not part of the LoginVSI knowledge worker test run, is added to the test setup. This user’s session is configured, to launch a process that consumes 3 CPU cores for an average of 50%-70% of total CPU, based on the number of cores in the Azure VM.

Windows 10 2004 multi-session VMs, were tested with CVAD 2006 installed and the Citrix Optimizer applied and WEM agent over HDX. To baseline the test results, the same test was run with Microsoft RDP as the connection protocol and the VMs had Out of the Box optimizations from Microsoft.

As can be seen from the following table, the inclusion of WEM, suppresses the effect of the CPU consuming noisy neighbor. WEM inclusion increases the VSImax (no of users that can be supported on the machine) from 20% to 43%. Resulting in a higher number of users that can run on a single VM, even in the stress scenario.

**Scale breakdown with Noisy Neighbor scenario**:

| Azure Size  |  MS RDP | Citrix HDX  | % WEM Scalability Improvement  |
|---|---|---|---|
| D4V3  | 7.3  | 11.3  | **43.0%**  |
| D3V2  | 12  | 16  | **28.6%**  |
| D4V2  | 28  | 34.5  | **20.8%**  |

As WEM reduces CPU spikes, another important inference from the results is, that the response time for the user is much better. Citrix Virtual Apps and Desktops sessions have an almost 1000 ms lower response time when compared to MS RDP (at the instant VSImax is reached) for the same number of users.

Similarly, the latency observed in the session is between 25%-50% lesser on both the machines with 4 vCPUs. Both these results point to a much smoother and snappier user experience when WEM is in the picture.

![WEM Latency Improvement Graph](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_latency-improvement-graph.png)

### Logon Optimization

To deliver the best possible logon performance, WEM service replaces commonly used Windows Group Policy Object objects, logon scripts, and preferences with an agent, which is deployed on each virtual machine or server. The agent is multi-threaded and applies changes to user environments only when required, ensuring that users always have access to their desktop as quickly as possible.

## Profile Management

Provides a management interface to the Citrix Profile Management. Under the Citrix Profile Management Settings (in Policies and Profiles), the console supports configuring all settings for the current version of Citrix Profile Management.

## Security features

WEM includes several features that bolster the security posture and reduce the threat surface of the deployment.

### Application Security

The Application Security component of WEM is a front-end for Microsoft AppLocker.

![WEM application security console](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_application-security.png)

Setting application security policies (AppLocker) with Workspace Environment Management follows the same process as doing it from with group policy objects. Admins create executable, Windows installer, scripts, package, and DLL rules. For each rule, the same options are available to base the rule on a publisher, path, or a file hash. However, unlike AppLocker, WEM allows the admin to select multiple rules and change assigned users, making it easy to support hundreds of application security policies.

The WEM policy engine ensures that the AppLocker policies are applied more consistently to all sessions that are managed when compared to the GPO based approach with AppLocker.

### Privilege elevation

On many occasions, users need administrator privileges to be able to install or run applications. With most organizations adopting the principle of least privilege, end users generally are not given admin rights.

In the scenarios when they are required the admin must either log in to the system (physically or via remote access) or provide the user with a temporary admin password. This practice is tedious for the admin or ends up in creation of workarounds to the security policy.

![WEM privilege elevation console](/en-us/tech-zone/learn/media/tech-briefs_workspace-environment-mgmt_privilege-elevation.png)

The feature allows the admin to temporarily elevate user’s privilege to local admin privileges and thus can install or run the required application. The apps that are on the approved list can be installed with this feature while the apps on the blocked list cannot be installed and the operations are logged. Each rule is based on either the path, the publisher, or the hash of the executable.

When a new rule is added three options are available:
**Apply to Child Processes**, if you want to have child processes inherit the privilege elevation or not.

The **Start and End Time** options allow the elevation to be restricted to a certain period.

## Deployment options

There are three types of configurations to deploy WEM. Use the type that suits the actual environment the best.

### Single domain in a single forest

This configuration can be used in an environment that has a single domain in a single forest. Normally, this single domain contains all the resources and user objects. So, in this configuration, the admin only needs to deploy one set of Cloud Connectors to enable all your devices to connect to the WEM service.

### Multiple domains in a single forest

This configuration can be used in environments where multiple domains in a single forest exist. As the domains in the forest can communicate with each other, in this configuration, the admin only needs to deploy one set of Cloud Connectors to enable all your devices to connect to the WEM service.

### Users and resources in separate forests (with trust)

In this configuration, users and resources reside in different domain forests for management purposes. A trust exists between the two forests that allows the users to log on to resources in another forest. In this deployment, the admin needs to deploy Cloud Connectors into each domain forest to complete the WEM deployment.

## Configuration considerations

The WEM service is designed for large-scale enterprise deployments. On the server side, WEM service monitors the communication flow between front- and back-end components and scales up or down dynamically based on data in transit.
When evaluating WEM service for sizing and scalability, the machine size and number of Cloud connectors is important. To ensure high availability, we recommend at least two Cloud Connectors in each resource location and based on the deployment options discussed before.

For information about sizing Cloud Connectors, see [Citrix Cloud Connector](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector.html).

To learn more, visit the [WEM product documentation](/en-us/workspace-environment-management/service.html).

For the latest updates about WEM, check out [What’s new](/en-us/workspace-environment-management/service/whats-new.html).
