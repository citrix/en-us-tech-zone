---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Nitin D. Mehta, Daniel Feller
description: Explore the various ways Citrix enables admins to save on cost when hosting workloads in the cloud. Learn about different load balancing algorithms and scaling methodologies and how much they can save in an environment based on our tests.
---
# Autoscale

## Overview

A poorly designed power-management scheme can increase cloud computing costs by 70%+ over what the organization requires to support the users. Cloud-based services often use a pay-as-you-go model where organizations are billed, sometimes every second, for compute time, network throughput, storage consumption, and transactions. To minimize costs, organizations need to intelligently utilize, allocate, and deallocate resources, which are in stark contrast from a traditional, on-premises model where organizations leave resources allocated indefinitely. In a virtual desktop deployment, organizations must consider the cost of permanently allocating hundreds or thousands of virtual machines that are utilized. This approach makes the solution too expensive to be feasible.

The Citrix Virtual Apps and Desktops service uses Autoscale with vertical load balancing as one of the ways to help lower cloud costs. These capabilities allow organizations to fully utilize virtual desktops, identify usage trends, and convert those trends into schedule and load-based rules that dynamically allocate and deallocate resources to maintain a positive user experience.

The following are some key benefits that customers gain from Autoscale in a Citrix environment:

*  **Hybrid multi-cloud load balancing** - Support for selective power management of resources that are only hosted in a resource location or cost more to run, while resources that are on-premises or reserved instances can be kept running 24x7. Enables on demand burst to cloud-based resources and disaster recovery use cases.
*  **Service Integration** - Autoscale is integrated into the Citrix Cloud console and requires no extra components, apps, scripts to be deployed. Enabled easily from the UI with a few clicks and configured per delivery group.
*  **Subscription wide support** - Power manages machines throughout a subscription, enabling DR to another region.
*  **Schedule-based scaling** – Enables a defined number of machines to be kept powered on during peak hours and this schedule is configurable for different days in the week.
*  **Buffer capacity** - Predefined number of VMs can be powered on before expected concurrent logon time slots, like weekday mornings, to ensure a quick and smooth logon for all users. Buffer capacity is configurable and can be set separately for peak and off peak hours.
*  **VDI Support** - Support for both single session and multi-session delivery groups.
*  **Drain mode** – Can isolate VMs that have low user count and not assign new sessions to the them so that they can be shutdown even during peak hours.
*  **Dynamic Provisioning** - Machines can be created and deleted using Citrix Machine Creation Services, providing additional storage cost savings by deallocating the disk when the machine is not needed.
*  **More decision parameters** - Power management decision rules can be customized as any combination of CPU, memory, disk utilization and number of sessions.
*  **Director reporting** - Capacity planning and power management cost savings reported in the monitoring console.
*  **Power off delay** – Ensures machines don’t keep flapping between on and off states. The period after a powered on machine can be switched off is configurable.

## Cost vs Experience

The main impact of power managing machines on user experience is in the time taken to connect a user to the requested session. If the power management is too aggressive and any machine that is not hosting sessions is shut down, the pool capacity (from running machines) depletes. When another user requests a session and the powered on machines have no available capacity, the user must wait for a machine to be powered-on. The wait affects the user experience negatively, adding up to a few minutes to the session launch time. When many users log in simultaneously, for example at the beginning of a shift, the boot time can be even longer as several machines need to be powered on at the same time.

Cost savings from powering off unused machines come from the compute cost being nil, and reduction of network ingress/egress and data transactions (reading and writing to storage) cost. This scenario results in the admin needing to perform a balancing act of ensuring that users can log into sessions quickly, while keeping costs down by shutting down as many unutilized machines as is optimal.

To effectively balance cost vs experience, Citrix incorporates the following technologies:

1.  Autoscale
1.  Load balancing algorithms
1.  Zone Preference and Tagging

## Autoscale

Autoscale is a set of capabilities that are designed to automate the process of powering managing Virtual Machines that are part of a deployment. Autoscale is designed to reduce the cost of running machines in a Citrix environment.

A delivery group contains users that have similar requirements for apps and desktops. The group of users most likely have similar working hours; for example, the HR department of a company in the London office. Therefore admins configure Autoscale at a delivery group level.
The users in the delivery group are allocated resources running on pools of machines (machine catalogs) that can service their requests.
To make accurate decisions about powering on and off machines in a pool, Autoscale must know the capacity of the delivery group. To ensure that Autoscale has an accurate view of hosts that can accept session requests, Autoscale includes only machines that are registered with the service when determining the capacity for a given delivery group.

Autoscale enables the admin to perform power management based on the following two settings:

1.  Schedule-based scaling
1.  Load-based scaling

Note: Both are used in conjunction to define the power management settings.

[![Autoscale - Schedule and Load based scaling config dialog](/en-us/tech-zone/learn/media/tech-briefs_autoscale_1-scaling-config.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_1-scaling-config.png)

### Schedule-based scaling

In a typical office with defined working hours, peak and off-peak hours are differentiated. Admins define the number of machines to be powered on during working hours and non-working hours.
Working hours (for example 9 AM to 5 PM on weekdays) generally would need to have a minimum number of machines turned on to meet demand. If the work day begins at 9 AM, the virtual desktops must already be available. It's a good idea to have booted up the machines and given them sometime to finish their post boot tasks before users start logging on to them. Therefore sometime before the working hours begin (8 AM in our example), several hosts are booted up to handle the anticipated load, which results in the users getting the positive logon experience. So the schedule would be set from 8 AM to 6 PM.

After working hours are over (at 5 PM in our example) users start to log off. Admins want to shut down unused hosts. Around 6 PM Autoscale start powering off hosts and only the number of hosts required for off-peak use are left powered on. This results in drastically reducing the cloud consumption cost, as opposed to running the entire inventory of hosts 24/7. Citrix offers the admins the flexibility to define these hours with a granularity of 30 minutes. For multi-session delivery groups, admins can set the minimum number of running hosts separately for each half-hour of the day. For pooled single session delivery groups, admins can set the minimum number of running hosts separately for each hour of the day. Admins can set the working hours individually for different days.

**Note:** Schedule based-scaling does not apply to static single session delivery groups as the user connects to a particular machine. Having other machines booted up at specific times does not help, except if a user is trying to log in the first time.

### Load-based Scaling

Load-based scaling lets admins create a capacity buffer of machines in case they are needed to host sessions. The capacity buffer is a SafetyNet to support unexpected increases in usage without negatively impacting the user experience. Ascertaining the right value for the capacity buffer (as a percentage of the pool capacity), is based on usage and the understanding the load variance in the customer environment. For a delivery group, if the total session capacity is 100, and the admin defines capacity buffer as 10% then during peak times, the number of machines needed to keep spare capacity above 10 sessions are powered on.

As users log in, the available capacity of the delivery group depletes. When it falls below the capacity buffer value, another machine in the pool is started to bring the capacity buffer back above the defined value. On the other side, when users start logging off, the machines with the least load are put in drain mode. Once the machines are clear of sessions, the machines are shut down until the pool capacity reduces to the set capacity buffer value.

[![Autoscale - Savings illustration](/en-us/tech-zone/learn/media/tech-briefs_autoscale_2-savings-illustration.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_2-savings-illustration.png)

**Note:** Load based scaling is not available with single session desktop delivery groups as the load on each machine is either zero or full, based on whether the machine has a session running on it or not. Therefore when the capacity buffer is set for a single session delivery group it is based on the number of machines in the delivery group.

### Drain mode

When there is more session capacity than the capacity buffer, the machine with the least number of sessions is put in drain mode. A machine in drain mode will not accept new sessions, even though it may have spare capacity. Autoscale attempts to have all sessions on the machine be logged off, so the machine can be shut down.

Note: If there is no spare capacity in a delivery group left, then Autoscale will launch sessions on the machine in drain mode rather than boot up another machine.

## Load-balancing algorithms

Citrix employs a horizontal load-balancing algorithm by default to power manage the machines in the cloud. Incoming sessions are load balanced to optimize the user experience. Each new session is assigned to the machine that is least loaded. Once a new machine in a catalog is started, it has the least load and would be used for all subsequent logons until the load is no longer the lowest.

In the following example, there are 6 machines in a delivery group. If the capacity buffer is set to 20%, then soon as free capacity goes below 5 sessions, a new machine is powered-on. Now when a new user logs in, that user is assigned the newly powered-on machine. The next user will be assigned the least loaded machine. You can see how the distribution occurs in the following illustration. The equal distribution of sessions reduces the chances of any of the machines being able to be shut down. For example if one of the first 5 sessions logs off then free capacity goes up to 5 sessions but none of the machines can be shut down as each has at least one session running on it.

[![Autoscale - Horizontal Load-balancing](/en-us/tech-zone/learn/media/tech-briefs_autoscale_3-horizontal-load-balancing.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_3-horizontal-load-balancing.png)

With vertical load-balancing, admins can configure it so that incoming sessions are load balanced to optimize for least powered-on machine count. Sessions are assigned to the most loaded machine, so long as it does not reach the high watermark for max load. This ensures that only the minimum number of machines needed to service the current load are powered on, thus being much more cost effective. Consider our preceding example, if one of the first 5 sessions logs off, reducing the session count on machine 1 to 4, then free capacity goes back up to 5 sessions. Machine 3 can now be shut down, saving extra cost. Also a new session would be assigned to machine 1 rather than a new machine booting up. In addition, sessions on the least loaded machine are marked for logout first.

[![Autoscale - Vertical Load-balancing](/en-us/tech-zone/learn/media/tech-briefs_autoscale_4-vertical-load-balancing.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_4-vertical-load-balancing.png)

Consider the scenario where the admin is unsure how many machines are required to service user requests. Chances are, the admin over-allocates machines for the delivery group. With vertical load balancing, only the required number of machines will power on for the user load. The capacity buffer helps to keep extra machines available to handle load in this scenario.

## Zone Preference and Tag Restriction

In hybrid deployments, there are resources in cloud and on-premises. A hybrid deployment can segment a single delivery group into different zones:

*  Primary: The preferred location for resource utilization.
*  Secondary: The resource location tapped when the primary zone is fully utilized.

For example, in a burst to the cloud scenario, an organization would fully utilize primary, on-premises resources. When extra capacity is required, secondary instances are consumed from the cloud. The delivery group would consist of two catalogs, one for the on-premises resource location and another for the cloud location.

### Hybrid deployment with on-premises and cloud based instances

In a Hybrid deployment model, the on-premises resources are your primary zone while the cloud based pay-as-you-go instances make up the secondary zone.

[![Autoscale - Zone Preference Hybrid Deployment](/en-us/tech-zone/learn/media/tech-briefs_autoscale_5-hybrid-zone-preference.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_5-hybrid-zone-preference.png)

### Cloud deployment with reserved and pay-as-you-go-instances

In a cloud only deployment, the reserved instances would be the primary zone and the secondary zone contains the pay-as-you go instances that are consumed on a need basis.

[![Autoscale - Zone Preference Cloud only Deployment](/en-us/tech-zone/learn/media/tech-briefs_autoscale_6-cloud-zone-preference.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_6-cloud-zone-preference.png)

Zone preference and tagging are critical for reducing costs in hybrid deployments. Primary instance machines are paid for upfront. There are limited cost savings by powering these resources down. The organization uses these resources first before the secondary instances are used.

The Citrix Virtual Apps and Desktops Service supports the use of multiple resource locations with the zone preference option. The admin uses zone preference to define which resource location to use first to fulfill demand and which location power down first when session demand drops. Once the capacity of the primary zone is fully utilized, the hosts marked as the secondary boot to serve session demand. When demand falls, the hosts in the secondary zone (cloud resources) are shut down first, resulting in optimal cloud utilization.
The settings for these are done at the machine catalog level.

[![Autoscale - Zone Preference Config](/en-us/tech-zone/learn/media/tech-briefs_autoscale_7-zone-preference-config.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_7-zone-preference-config.png)

Admins must use tag restriction along with Zone preference to let Autoscale know the specific set of hosts (the cloud location) from the delivery group are to be power managed or not. Tag restriction specifies the hosts to be power managed by Autoscale. These are hosts earmarked to serve the burst/disaster recovery load.

[![Autoscale - Tagging](/en-us/tech-zone/learn/media/tech-briefs_autoscale_8-tags.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_8-tags.png)

For more information, see [Zone Preference](/en-us/citrix-virtual-apps-desktops/manage-deployment/zones.html#zone-preference) and [Tags](/en-us/citrix-virtual-apps-desktops/manage-deployment/tags.html#manage-tags-and-tag-restrictions). Follow instructions [here to configure autoscale restrictions](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale/restrict-autoscale.html).

## Autoscale cost savings with Schedule-based scaling

**Note:** Results vary based on usage and load characteristics of each organization. Observe these patterns in your own system to fine-tune the **Autoscale** settings for your environment. The following are results based on our internal testing with LoginVSI loads.

To measure the cloud cost savings with Autoscale, we need to determine the cost of running machines in the cloud. The sizes and cost (as of writing this paper) for pay as you go in West US of the VMs, as calculated using [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator) are as follows:

| VM Instance | VM Specs         | Cost per month | Cost per hour | Storage costs per month |
|-------------|------------------|----------------|---------------|-------------------------|
| D3_V2       | 4 vCPU \ 14 GB   | $367.92        | $0.504        | $5.94                   |
| D4_V2       | 8 vCPU \ 28 GB   | $735.84        | $1.008        | $5.94                   |
| F16         | 16 cores \ 32 GB | $1264.36       | $1.732        | $5.94                   |

Cost per month calculation assumes the machine is running for the entire month (730 hours). The storage cost is a fixed monthly cost regardless if the machine is powered on or off.

**With Autoscale, we reduce the time the machine remains powered-on to better align with user behaviors.**

To better determine the potential cost savings with Autoscale, Citrix conducted LoginVSI single server scalability tests with the following setup:

*  Citrix Virtual Apps and Desktops 1902
*  Citrix Workspace app 1903
*  Windows Server 2016 Multi-session OS Azure VMs
    *  D3_V2
    *  D4_V2
    *  F16
*  LoginVSI Workload: Knowledge Worker

This resulted in the following user load per virtual machine instance:

[![Autoscale - Session per machine for different Azure VM sizes](/en-us/tech-zone/learn/media/tech-briefs_autoscale_9-sessions-per-machine-vs-azure-machine-size.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_9-sessions-per-machine-vs-azure-machine-size.png)

The scalability numbers are used to provide sizing guidance for the following three scenarios.

### Scenario 1 - Horizontal scaling with defined peak & off peak hours. Off-peak hours - Zero active users

*  User Count during Peak Time: 1,000 users
*  User Count per Off Peak Time: 0 users
*  User Count during Off Peak Weekend Time: 0 users
*  Peak Logon Time: 9AM
*  Peak Log off Time: 5PM
*  Peak Start Time: 8:30AM
*  Peak End Time: 5:30PM
*  Active per Day: 9 hours
*  Active per Month: 198 hours
*  Load Balancing Algorithm: Horizontal

Based on this scenario, the active sessions during the day are as follows:

[![Autoscale - Scenario 1 Active sessions graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_10-scenario-1-active-sessions-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_10-scenario-1-active-sessions-graph.png)

If all the machines are shut down after the off peak times begin, the number of hours the machines would be on during a single month is 198 hrs. The cost savings calculations are:

| Knowledge Worker -Machine Size                   | D3_V2       | D4_V2       | F16         |
|--------------------------------------------------|-------------|-------------|-------------|
| VSImax - Sessions / machine                      | 25          | 50          | 74          |
| No of machines needed                            | 40          | 20          | 14          |
| Compute Cost per hour (in USD)                   | 0.504       | 1.008       | 1.732       |
| Cost per hour (incl. 128 GB disk) for 1000 users | 20.48548    | 20.32274    | 24.36192    |
| Cost per month (100% On)                         | 14954.4     | 14835.6     | 17784.2     |
| Cost per month with Autoscale (198 hrs)          | **4229.28** | **4110.48** | **4884.26** |
| Percentage Cost Savings                          | **71.72**   | **72.29**   | **72.54**   |

The following graph shows the difference in cost of running the machines, when being powered on all the time vs being power managed by Autoscale in this scenario.

[![Autoscale - Scenario 1 Cost savings graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_11-scenario-1-cost-savings-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_11-scenario-1-cost-savings-graph.png)

From the table, the cost of machines powered on all the time is over 350% the cost when the machines are power managed by Autoscale. An admin in this scenario can save around 70% of the compute cost by using Autoscale.

### Scenario 2 - Horizontal scaling with defined peak & off peak hours. Off-peak hours – 10% active users

*  User Count during Peak Time: 1,000 users
*  **User Count per Off Peak Time: 100 users (Different than scenario 1)**
*  User Count during Off Peak Weekend Time: 0 users
*  Peak Logon Time: 9AM
*  Peak Log off Time: 5PM
*  Peak Start Time: 8:30AM
*  Peak End Time: 5:30PM
*  Active per Day: 9 hours
*  Active per Month: 198 hours
*  Load Balancing Algorithm: Horizontal

Based on this scenario, the active sessions for the week are as follows:

[![Autoscale - Scenario 2 Active sessions graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_12-scenario-2-active-sessions-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_12-scenario-2-active-sessions-graph.png)

The number of hours all the machines would be on during a single month is 198. Also, 10% of the machines (rounded up) would be running for an extra 15 hrs multiplied by 18 days in the month. The cost savings calculations are:

| Knowledge Worker -Machine Size                   | D3_V2       | D4_V2       | F16         |
|--------------------------------------------------|-------------|-------------|-------------|
| VSImax - Sessions / machine                      | 25          | 50          | 74          |
| No of machines needed                            | 40          | 20          | 14          |
| Compute Cost per hour (in USD)                   | 0.504       | 1.008       | 1.732       |
| Cost per hour (incl. 128 GB disk) for 1000 users | 20.48548    | 20.32274    | 24.36192    |
| Cost per month (100% On)                         | 14954.4     | 14835.6     | 17784.2     |
| Cost per month with Autoscale (198 hrs)          | **4773.6**  | **4564.8**  | **5819.54** |
| Percentage Cost Savings                          | **68.08**   | **68.62**   | **67.28**   |

The following graph shows the difference in cost of running the machines, when being powered on all the time vs being power managed by Autoscale in this scenario:

[![Autoscale - Scenario 2 Cost savings graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_13-scenario-2-cost-savings-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_13-scenario-2-cost-savings-graph.png)

From the table, the cost of machines powered on all the time is over 300% the cost when the machines are power managed by Autoscale. An admin in this scenario can save around 68% of the compute cost by using Autoscale. Comparing the Autoscale cost per month with the previous scenario, the off peak cost per month for both the D_V2 VM sizes comes to about $545 while the larger F16 VMs account for $935 per month during off peak hours. The graph clearly shows that, for the same number of sessions, the smaller size machines are not only cost effective overall but also cost less when there is lower demand.

### Scenario 3 - Vertical scaling with staggered peak hours. Off-peak hours – 10% active users

*  User Count during Peak Time: 1,000 users
*  User Count per Off Peak Time: 100 users
*  User Count during Off Peak Weekend Time: 0 users
*  Peak Logon Time: 9AM
*  Peak Log off Time: 5PM
*  Peak Start Time: 8:30AM
*  Peak End Time: 5:30PM
*  Active per Day: 9 hours
*  Active per Month: 198 hours
*  Load Balancing Algorithm: Vertical (different than scenario 2)
*  Logon\Logoff Rate: 25% each hour

Based on this scenario, the active sessions for the week are as follows:

[![Autoscale - Scenario 3 Active sessions graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_14-scenario-3-active-sessions-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_14-scenario-3-active-sessions-graph.png)

There are fewer machines needed at the beginning of the day. Since the calculation multiplies the number of machines and hours, the number of compute hours reduces by 1 hour a day, 22 hours in the month. As discussed, users logging off would be from random machines, so we assume that the required number of machines drops to 10% after the end of the workday. The cost savings calculations are:

| Knowledge Worker -Machine Size                   | D3_V2       | D4_V2       | F16         |
|--------------------------------------------------|-------------|-------------|-------------|
| VSImax - Sessions / machine                      | 25          | 50          | 74          |
| No of machines needed                            | 40          | 20          | 14          |
| Compute Cost per hour (in USD)                   | 0.504       | 1.008       | 1.732       |
| Cost per hour (incl. 128 GB disk) for 1000 users | 20.48548    | 20.32274    | 24.36192    |
| Cost per month (100% On)                         | 14954.4     | 14835.6     | 17784.2     |
| Cost per month with Autoscale (198 hrs)          | **3893.6**  | **4214.4**  | **5511.54** |
| Percentage Cost Savings                          | **73.96**   | **71.59**   | **69.01**   |

The following graph shows the difference in cost of running the machines, when being powered on all the time vs being power managed by Autoscale in this scenario.

[![Autoscale - Scenario 3 Cost savings graph](/en-us/tech-zone/learn/media/tech-briefs_autoscale_15-scenario-3-cost-savings-graph.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_15-scenario-3-cost-savings-graph.png)

From the table, the cost of machines powered on all the time is over 300% the cost when the machines are power managed by Autoscale. For smallest machine, D3_V2 it’s at 384%. The smaller the machine size the higher number of them can be shut down to serve the same load, when compared to larger machines. Similarly, they are quicker to shut down and they are quicker in reaching zero sessions running on them, when users start to log off.

Considering all three scenarios, and looking at the cost savings graphs, it is evident that the smaller the machine size the more cost effective it is when Autoscale is enabled.

**Important Note:** To allow Autoscale be able to shut down machines, users must disconnect or log off from them. Therefore, consider configuring disconnect and log off timers in the **Autoscale** settings for delivery groups, so the cost savings are higher.

## Monitoring

Admins can monitor the following metrics of Autoscale-managed machines from the **Monitor** tab. The tab shows the graphs of the consumption of the resources per delivery group. These graphs are used to figure out the actual usage of the delivery groups to aid the admin in correctly sizing the catalogs. The following graphs are shown:

*  Machine usage
*  Estimated savings
*  Alert notifications for machines and sessions
*  Machine status
*  Load evaluation trends

Savings are calculated based on the cost per hour of the machines entered by the admin when configuring the **Autoscale** settings. When admin selects All delivery groups, the average value of Estimated Savings across all the delivery groups is displayed.

[![Autoscale - Monitoring](/en-us/tech-zone/learn/media/tech-briefs_autoscale_16-monitoring.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_16-monitoring.png)

## Summary

1.  Autoscale allows for better cost savings (~300%) without sacrificing user exp. It does that through schedule / load based settings
1.  Using smaller size VMs results in better cost savings under horizontal load balancing scenario
1.  Use zone preference and tagging to achieve more cost savings

**Further reading**: Learn more about Autoscale in our [Technical documentation](/en-us/citrix-virtual-apps-desktops-service/manage-deployment/autoscale.html).
