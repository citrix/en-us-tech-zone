---
layout: doc
h3InToc: true
contributedBy: Nitin Mehta
specialThanksTo: Mayank Singh, Kevin Nardone, Harsh Gupta
description: The goal of the document is to help answer frequently asked questions on how to optimize costs while addressing cloud use cases. The document provides best practices for configuring Autoscale for different admin use cases and their infrastructure and technical requirements. This is an evolving document as more use cases and capabilities are introduced within Autoscale.
---
# Autoscale design questions / frequently asked questions

## Overview

The goal of the document is to help answer frequently asked questions on how to optimize costs while addressing cloud use cases. The document provides best practices for configuring Autoscale for different admin use cases and their infrastructure and technical requirements. This is an evolving document as more use cases and capabilities are introduced within Autoscale.

## Use cases related questions

### Can an admin configure Autoscale to first exhaust on-premises resources and only then on-demand **burst to the cloud**?

-  The Autoscale [selective power management](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/restrict-autoscale.html) feature is useful here. The admin must tag the cloud workloads within the delivery group that will be utilized for burst. Then configure Autoscale to power manage only these tagged workloads. The admin can then leverage zone preference to prefer on-premises workloads (which are always powered on) for incoming requests. After this configuration, incoming requests will first be brokered to on-premises resources until those are fully utilized. Cloud resources will be powered on only after the on-premises capacity is completely utilized

### Can an admin use their on-premises resources for normal operations and Cloud resources only in a **business continuity or disaster recovery** scenario?

-  Use the above configuration

### Can an admin configure Autoscale for users **ramping up slowly / seasonal users / not worry about capacity planning upfront?**

-  When an organization has users onboarding slowly and does not want to provision entire capacity ahead of time, then the [dynamic provisioning](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/dynamic-machine-provisioning.html) feature with Autoscale can help. This feature constantly evaluates the load on a delivery group and creates new machines and deletes them based on the load on the delivery group. An admin must define the high and low watermark for load on the delivery group to create machines. Admins can optionally define the maximum number of machines to be provisioned, tag to be applied to these machines etc.

### How should an **admin of an university** with varying schedules and lectures during the day configure Autoscale?

-  The admin needs to understand the capacity utilization and lecture schedule and feed it into Autoscale [schedule-based scaling](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/schedule-based-and-load-based-settings.html) to step up and step-down capacity during the day. Autoscale allows admins to vary the powered-on capacity with 30 min granularity and allows for different schedules on different days. If there is a lot of volatile workload, it would be good to have capacity buffer defined so that the users do not have to wait for machines to power on.

### How should the admin of an **outsourcing firm with agents logging in at fixed timings** configure Autoscale?

-  Utilizing [schedule-based scaling](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/schedule-based-and-load-based-settings.html) depending on the shift schedules. Autoscale has 30 mins granularity. Admins can configure Autoscale to power on different workloads at separate times of the day. If the number of users is unexpected, then admins also define the buffer capacity. Do factor in the time it takes to power on the capacity, as some platforms can take several minutes to switch on machines, which could result in poor UX if machines are not powered on beforehand.

### How can the admin configure Autoscale if they are **using heavy machine instances**?

-  If the organization provides static desktops to its users, there might be a use case where the admin wants the machines to be started when the user clicks on the desktop icon (say for knowledge workers who occasionally work on those large machines) but shutdown at specific time of the day say at 6 PM. In this case the admin can define the property AutomaticPowerOnForAssigned to false
How can an admin configure Autoscale to save storage costs apart from compute?

## Infrastructure questions

### How many **reserved instances (RI)** should the admin purchase?

-  Observe the [capacity utilization](/en-us/citrix-virtual-apps-desktops-service/monitor/site-analytics/autoscale-managed-machines.html) data under Monitor > Trends > Machine Usage over a time period (that covers all variations such as seasonality, work force changes etc.) in the environment. The minimum capacity that is powered on for this time period could be a good indication of the minimum reserved instances you should buy. Some other parameters that define this are.

-  Is there a fixed schedule say 9 AM - 5 PM? That would mean machines can be shutdown 66% of the time and achieve close to RIs cost savings of ~70%.
-  More RIs than the minimum capacity usage could be bought depending on workload usage and savings. E.g., say if there are 10% more machines running more than 50% of the time.
-  The organizations expansion should also be factored in.
-  It can save up to 70% costs compared to pay as you go machines over a three years duration.

### How many **burstable instances** should the admin purchase?

-  For virtual desktop users with high memory consumption (E.g., For applications such as chrome browser, MSFT applications) but low CPU consumption, admins can consider using burstable vms which could be half the cost of thier peer machines (as is the case in Azure for B series and D series). In Citrix Director, check Resource utilization and average CPU consumption and match it with the base CPU% guarantee of the burstable vms. We recommend using these machines for single user sessions only since the impact is limited compared to a multi-session where a lot of users could be impacted if the CPU gets throttled.
-  This mechanism could be used to lower costs by as much as 50% from the regular pay as you go model.

### How can the admin choose the **right instance size**?

-  Admins can optimize costs if they right size the machine instances in public clouds. Smaller instances host fewer user sessions than larger instances. Therefore, in the case of smaller instances, Autoscale puts machines into drain state much faster because it takes less time for the last user session to be logged off. As a result, Autoscale powers off smaller instances sooner, thereby reducing costs. Citrix recommends provisioning smaller instances if they match workload performance and capacity requirements. An effective way to understand this would be to have no more than 15-20 sessions per machine.

### What **scale limits** can the admin operate under?

-  It depends on the limits of the chosen cloud platform and the provisioning technique. Say for example if the cloud platform is Azure and the provisioning technique is MCS (Machine Creation Services) then the [guidance](/en-us/citrix-virtual-apps-desktops-service/limits.html#machine-creation-services-mcs-limits) is ~1200 machines per subscription.
-  Note that is this is an evolving page. It is also recommended to evaluate other limits like # of machines in a resource location, hosting connections etc. /en-us/citrix-virtual-apps-desktops-service/limits.html

## Technical questions

### How can the admin **start new machines based on session count**?

-  Look at the [load index](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html#load-index). By default, it is based on session count (250) and is the factor considered for powering on more machines.

### How does Autoscale **turn off machines**?

-  Autoscale always attempts to scale down the number of powered-on machines in the delivery group to the configured pool size and capacity buffer. It does so by putting the excess machines with the fewest sessions into “drain state” and powering them off when all sessions are logged off. This occurs when session demand lessens, and the schedule requires fewer machines than are powered on. [More info](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html#drain-state).

### How does Autoscale **turn on machines**?

-  It powers on machines based on the schedule and load-based settings. If the current machines cannot accommodate the incoming load and respect the capacity buffer configured, then it will power on more machines. See this [working example](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/schedule-based-and-load-based-settings.html#example-scenario) for better understanding of how it works.

### Where do I see **machines which are candidates for shutting down**?

-  Just like maintenance mode, a list of machines in drain state is maintained. If a machine is in drain state, then it is a candidate to be shut down. Once all the sessions are drained off, the machine is shutdown. To find out which machines are in drain state, use the PowerShell command: Get-BrokerMachine -DrainingUntilShutdown $true. [More info](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html#drain-state)

### How do I see my **cost savings**?

-  The Monitor > Trends > Machine Usage page also displays the estimated [cost savings](en-us/citrix-virtual-apps-desktops-service/monitor/site-analytics/autoscale-managed-machines.html#estimated-savings). achieved by enabling Autoscale in the selected delivery group. Both $ saved and % savings using Autoscale are displayed. These can be sent as monthly savings reports to higher management.
