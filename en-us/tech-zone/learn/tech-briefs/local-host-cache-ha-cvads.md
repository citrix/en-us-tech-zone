---
layout: doc
h3InToc: true
contributedBy: Jose Augustin
specialThanksTo: Bala Swaminathan
description: Copy & paste description from TOC here
---
# Local Host Cache / High Availability mode for Citrix Virtual Apps and Desktops Service

## Overview

Local Host Cache, in the context of the Citrix Virtual Apps and Desktops (CVAD) service, can be thought of as an insurance policy. This insurance policy comes into play when, for whatever reason (outages, connection issues, internet blackouts, etc.), the Citrix Cloud Connectors are not able to communicate with the Citrix brokering service (part of the CVAD service and henceforth referred to as the cloud broker). A communication breakdown between a resource location and the cloud broker could lead to end user impact – Local Host Cache is designed to mitigate such end user impact.

Local Host Cache is a combination of several services and components which come together to take over the brokering responsibilities until the connection to the cloud broker can be reestablished.

[![Citrix Virtual Apps and Desktops - Normal Operations](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-conceptual.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-conceptual.png)

## Cloud Connector Components

There are several components within the Citrix Cloud Connector which are required for the Local Host Cache operations.

-  **Configuration Synchronizer Service:** The Configuration Synchronizer Service (CSS) periodically checks with the cloud broker (every 60 seconds) to see if any configuration changes were made. The changes can be administrator-initiated (such as changing a delivery group property) or system actions (such as machine assignments). When changes are detected, CSS synchronizes the changes from the cloud broker to the connector machines.
-  **LocalDB:** The connector includes an installation of SQL Server Express LocalDB where a local copy of the configuration is stored (through CSS). A new instance of the database is created for every sync operation. Once the sync is completed successfully, the latest DB instance replaces the prior DB instance.
-  **High Availability Service** : The High Availability Service (HA Service)is a specialized Broker service that provides the runtime brokering functionality during an outage. The HA Service is also referred to as the secondary broker.
-  **Remote Broker Provider:** The Remote Broker Provider performs several important functions:
-  It acts as a proxy relaying communication between the Citrix Virtual Desktop Agent (VDA) and the Cloud Broker
-  It acts as a proxy relaying communication between an on-premises StoreFront or an on-premises ADC and the various Citrix Cloud services
-  It determines when to switch a resource location between HA mode and normal operation

[![Citrix Virtual Apps and Desktops - Normal Operations](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-regular.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-regular.png)

Proper sizing of the cloud connector machines is an important step to ensure that appropriate resources are available for the services when in High Availability mode. Review [scale and size considerations](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache-size-and-scale.html) article to learn more.

## High Availability Mode

Citrix Cloud Connectors are capable of entering or exiting HA mode automatically without administrator intervention. HA mode can be triggered by any of the following:

-  Failure of StoreFront enumerations or launch requests
-  Failure to relay communications between the VDA and the cloud broker
-  Failure to present STA requests to the CVAD service on behalf of an on-premises ADC during a launch

When in HA mode, the HA Service takes over several important brokering functions, it enumerates resources, brokers session launches, and accepts VDA registrations. In addition, the HA Service acts as a STA provider. When multiple Cloud Connectors are present in a resource location, the HA Services on the Cloud connectors communicate with one another as part of an [election process](bookmark://_Multiple_connectors_in) to determine which instance of the HA Service will take over if HA mode is entered.

[![Citrix Virtual Apps and Desktops - High Availability Mode](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-ha.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_citrix-cloud-ha.png)

## Entering/Exiting High Availability Mode

The decision to transition to HA mode is dependent upon enumeration and launch traffic flowing through a given Cloud Connector instance. Any connector machine not configured as a Delivery Controller in StoreFront will not support the HA mode detection and transition. This optimization is necessary to prevent unnecessary VDA registrations.

There are several stages during the entire cycle of entering and exiting HA mode. During the **Working Normally** state, all components are healthy and all brokering transactions are handled by the cloud broker. The CSS is actively replicating the configurations from the cloud broker to the connector machines.

In the event that some of the components fail to report healthy, the connector transitions to the **Pending HA** state. When in this stage, a comprehensive health check is initiated to determine the next course of action. The connectors interact with other connectors in the resource location to determine their health status. The decision to move from Pending HA to Initial HA is based on the health status of all the connectors in a given resource location. At the end of this stage, the connector will either transition back to the Working Normally state if the health checks are successful OR transition to the Initial HA stage if the health checks continue to be erratic.

[![LHC State Diagram](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_ha-state-diagram.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_ha-state-diagram.png)

During the **Initial HA** stage, the High Availability Service on the connector takes over brokering responsibilities. All VDAs in the current resource location that were registered with the cloud broker will now register with the HA Service / secondary broker on the connector. At the end of Initial HA, a health check takes place. If all health checks succeed, the state transitions to Pending Recovery, otherwise the state transitions to Extended HA.

Health checks continue during the **Extended HA** period and as soon as all the health checks succeed, the state transitions to Pending Recovery. There is no minimum time duration for a connector to remain in the Extended HA state.

**Pending Recovery** serves as a waiting period, where all components are healthy, before handing off brokering back to the cloud broker. If any of the health checks fail during Pending Recovery, the state transitions back to Extended HA. If all the health checks succeed during the entirety of the Pending Recovery period, then the state transitions to Working Normally. With this transition, the HA mode has exited, and all the VDAs in the resource location that were registered with the secondary Broker now re-register with the cloud broker.

## CVAD service instance with multiple Resource Locations

The cloud broker is designed to have a view of the whole deployment – across multiple resource locations. However, when in HA mode, each resource location becomes its own independent pod, and the elected secondary broker in each resource location will effectively manage the brokering transactions only for the VDAs within that resource location. This is also a critical reason to ensure the StoreFront is configured to include all the Cloud Connectors from all the resource locations that contain VDA workloads such that StoreFront can distribute launch requests and effectively load balance users across multiple resource locations.

## VDA Registrations

When the outage begins, the elected secondary broker (read the section on [Multiple connectors in a Resource Location](#_Multiple_connectors_in) to know more about the election process) does not have current VDA registration data, but as soon as a VDA communicates with it, a registration process is triggered. During that process, the elected secondary broker also gets current session information for that VDA. The VDA communicates with the broker at least every 5 minutes. Depending on when the last heartbeat was completed, it may take a VDA up to 5 minutes to realize the change from the cloud broker to the elected secondary broker and trigger the registration with the elected secondary broker.

While the elected secondary broker is handling connections, the remote broker provider monitors the connection to Citrix Cloud. When the connection is restored, the remote broker provider instructs the elected secondary broker to stop listening for connection information, and the remote broker provider resumes conveying brokering operations to the cloud broker. The next time a VDA communicates with the remote broker provider, another registration process is triggered. The elected secondary broker removes any remaining VDA registrations from the previous outage. The CSS resumes synchronizing information when it learns that configuration changes have occurred in Citrix Cloud.

## Multiple connectors in a Resource Location

Citrix recommends a minimum of 2 connectors in every resource location / zone. In each zone, there is an election process constantly running to make sure the HA Services know which connector machine would take over brokering responsibilities in case of an interruption. This election happens at all times – both during normal operations and when running in HA mode.

The CSS routinely provides the secondary broker with information about all Cloud Connectors in the resource location. Having that information, each connector knows about all peer connectors running in the resource location. The secondary brokers communicate with each other on a separate channel. Those services use an alphabetical list of FQDN names of the machines they&#39;re running on to elect which secondary broker will broker operations in the zone if an outage occurs. When in HA mode, the elected secondary broker takes over brokering responsibilities while the other secondary brokers in the zone actively reject incoming connection and VDA registration requests.

If an elected secondary broker fails during an outage, another secondary broker is elected to take over, and VDAs register with the newly elected secondary broker. During HA mode, if a connector is restarted:

-  If that connector is not the elected secondary broker, the restart has no impact.
-  If that connector is the elected secondary broker, a different Cloud Connector is elected, causing VDAs to register with the new elected secondary broker. After the restarted Cloud Connector powers on, it automatically takes over brokering, which causes VDAs to register again. In this scenario, performance can be affected during the registrations.

The event log provides information about elections. For more information on the associated events, please review the [event logs](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html#event-logs) article from the product documentation.

## Local Host Cache with Multiple Resource Locations

### Load balancing across connectors in a resource location

The on-premises StoreFront sends a heartbeat message to all the Cloud Connectors configured in its store every 60 seconds by default. Only healthy Cloud Connectors (that respond successfully to the heartbeat) are considered for load balancing app enumeration and launch requests. The same heartbeat request to the Cloud Connectors also activates the connector to participate in the HA mode algorithm described above. To ensure that all resource locations are enabled to perform in HA mode, it is critical to ensure that the on-premises StoreFront has all the Cloud Connectors identified as Delivery Controllers in the StoreFront configuration. Failure to have appropriate StoreFront configurations could result in loss of capacity when the site enters HA mode.


[![Citrix Virtual Apps and Desktops - Normal Operations](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_lhc-multiple-resource-locations.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_lhc-multiple-resource-locations.png)

### HA mode for resource locations publishing same apps/desktops

One of the CVAD service deployment models include multiple resource locations – all publishing identical applications and desktops across all resource locations. For example, a deployment containing applications from a single multi-session image or pooled VDI desktops may be deployed uniformly in all resource locations.

When such a deployment is operating in HA mode, users may be directed to any of the VDAs in the various configured resource locations. In this scenario, the StoreFront load balances requests to all configured Cloud Connectors across various resource locations.

### HA Mode for resource locations publishing different apps/desktops

A CVAD service deployment may also have certain applications available only in a specific subset of resource locations. For example, a Japanese OS desktop may be available only on the VDAs running in Japan. Another example is with static/assigned desktops that are user specific and tied to a specific resource location after assignment.

When such a deployment operates in HA mode, the application or desktop launch requests need to be routed to the appropriate Cloud Connector in the specific resource locations where the app and/or desktops reside since cross-zone brokering is not available in HA mode. The _AdvancedHealthCheck_ feature offered by StoreFront 1912 LTSR Cumulative Update 1 or later facilities such deployments as described below.

The StoreFront enumerates applications and desktops from Cloud Connectors in any region. The enumeration information now contains a mapping between the resource (an application or a desktop) and the resource locations where the application/desktop resides. This mapping is used to direct the user launch requests to specific resource locations. In order to enable the StoreFront to utilize this functionality, the administrator should enable it as prescribed in the [product documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html#storefront-requirement).

## Architectures involving Citrix ADC

### Monitoring

The Citrix ADC appliance provides a built-in monitor, CITRIX-XD-DDC monitor, which monitors the Citrix Virtual Apps and Desktop Delivery Controller servers. In the context of Citrix Virtual Apps and Desktops service, the Cloud Connectors are equivalent to the Delivery Controller servers. The monitor sends a probe to the configured controller/connector servers in the form of an XML message. If the server responds to the probe with the identity of the farm, the probe is considered to be successful and the server&#39;s status is marked as UP. If the response does not have a success code or the identity of the server farm is not present in the response, the probe is considered to be a failure and the server&#39;s status is marked as DOWN.

More information about the CITRIX-XD-DDC monitor is available in [Citrix ADC documentation](https://docs.citrix.com/en-us/citrix-adc/current-release/load-balancing/load-balancing-builtin-monitors/monitor-xd-delivery-controller-services.html).

### Citrix ADC for resource locations publishing different apps/desktops

For architectures involving Citrix ADC in conjunction with resource locations publishing different apps and desktops, the following configurations need to be performed.

-  Aggregate the Cloud Connectors in each resource location to a unique VIP in the ADC load balancer.
-  Enable the StoreFront _AdvancedHealthCheck_ feature as described in the [section above](#_HA_Mode_for).
-  Map each zone / resource location to an ADC Virtual IP (VIP)
-  Add all ADC VIPs as Delivery Controllers to the StoreFront.
-  Setup the ADC load balancer to monitor the Cloud Connectors in each resource location via the CITRIX-XD-DDC monitor.

[![Citrix Virtual Apps and Desktops - Normal Operations](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_lhc-adc.png)](/en-us/tech-zone/learn/media/tech-briefs_local-host-cache-ha-cvads_lhc-adc.png)

## Pooled Desktop VDA Workload Considerations

When a user logs off a pooled desktop VDA, the VDA&#39;s image is reset to remove any user specific data from the last user who was using the VDA. When a site is running in HA mode, the reset operation is not available. And hence when a user logs off from a pooled desktop VDA, the machine is placed into maintenance mode. This prevents a tainted image being made available to another user.

Depending on the security needs for an implementation, this behavior can be modified by applying a site-wide as well as a per-delivery-group update. More information about how to override the default behavior is available in the [product documentation](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html#application-and-desktop-support).

For pooled desktop VDA workloads running on VMware vSphere and Citrix Hypervisor, a new feature which introduces support for power operations during HA mode is available in tech preview. This feature will add the capability to reset images even when a site is functioning in HA mode. Read more about the tech preview offering [here](https://www.citrix.com/blogs/2020/08/31/tech-preview-power-operations-during-local-host-cache-lhc-mode/).

## Testing Local Host Cache

Local Host Cache is designed to work without any user intervention - - it is fully autonomous. You can however verify that all of the Cloud Connectors are correctly synced and ready to take over. The following steps are recommended:

-  As described in the earlier sections, every connector performs synchronization of site configuration independently. The results of the sync are available in the Event Viewer. Refer to [this section](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html#event-logs) of the product documentation for details of the events.
-  An outage can be simulated to test the Local Host Cache solution in an environment. Guidance on how to force an outage is available [here](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/manage-deployment/local-host-cache.html#force-an-outage). When forcing an outage, take special care to set all the connectors in a resource location to the forced outage mode.
