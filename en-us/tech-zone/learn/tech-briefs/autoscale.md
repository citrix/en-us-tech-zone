---
layout: doc
---
# Autoscale

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** Nitin D. Mehta and Daniel Feller

# Overview

A poorly designed power-management scheme can increase cloud computing costs by ???% over what the organization requires to support the users. Cloud-based services often use a pay-as-you-go model where organizations are billed, sometimes every second, for compute time, network throughput, storage consumption and transactions. To minimize costs, organizations need to intelligently utilize, allocate and deallocate resources, which is in stark contrast from a traditional, on-premises model where organizations leave resources allocated indefinitely. In a virtual desktop deployment, organizations must consider the cost of permanently allocating hundreds or thousands of virtual machines that are minimally utilized. This approach will make the solution too expensive to be feasible. 

The Citrix Virtual Apps and Desktops service leverages Autoscale with vertical load balancing as one of the ways to help lower cloud costs. These capabilities allows organizations to fully utilize virtual desktops, identify usage trends and convert those trends into schedule and load-based rules that dynamically allocates and deallocates resources to maintain a positive user experience.


# Cost vs Experience

The main impact of power managing machines on user experience manifests in the time taken to connect a user to the requested session. If the power management is too aggressive and any machine that is not hosting sessions is shutdown, the pool capacity (from running machines) depletes. When another user requests a session and the powered on machines have no available capacity, the user must wait for a machine to be powered-on. This affects the user experience negatively, adding up to a few minutes to the session launch time. When a large number of users login simultaneously, for example at the beginning of a shift, the boot time could be even longer as a number of machines need to be powered on at the same time.

Cost savings from powering off unused machines come from the compute cost being nil, and reduction of network ingress/egress and data transactions (reading and writing to storage) cost. This results in the admin needing to perform a balancing act of ensuring that users can login to sessions quickly, while keeping costs down by shutting down as many unutilized machines as is optimal.

To effectively balance cost vs experience, Citrix incorporates the following technologies: 
1.  Autoscale
1.	Load balancing algorithms
1.	Zone Preference and Tagging

## Autoscale

A delivery group groups together users that have similar requirements for apps and desktops. The group of users most likely have similar working hours; for example, the HR department of a company in the London office. Therefore admins configure Autoscale at a delivery group level. 
The users in the delivery group are allocated resources running on pool(s) of machines (machine catalog(s)) that can service their requests. (Learn about types of delivery groups in the Appendix).
To make accurate decisions about powering on and off machines in a pool, Autoscale must know the capacity of the delivery group. To ensure that Autoscale has an accurate view of hosts that can accept session requests, Autoscale includes only machines that are registered with the service when determining the capacity for a given delivery group.

Autoscale enables the admin to perform power management based on the following two settings:
1.	Schedule-based scaling
1.	Load-based scaling
Note: Both are used in conjunction to define the power management settings.

[![Autoscale - Schedule and Load based scaling config dialog](/en-us/tech-zone/learn/media/tech-briefs_autoscale_1-scaling-config.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_1-scaling-config.png)

### Schedule-based scaling

In a typical office with defined working hours, peak and off-peak hours are differentiated. Admins define the number of machines to be powered on during working hours and non-working hours.
Working hours generally would need to have a minimum number of machines turned on to meet demand. If the work day begins at 9 AM, the virtual desktops must already be available. Sometime before the working hours begin (8 AM for example), a number of hosts are booted to handle the anticipated load. This results in the users getting the positive logon experience. So the schedule would be set from 8 AM to 6 PM.

At the end of the working hours (6 PM for example) or on the weekend, unused hosts are shut down and only the number of hosts required are left powered on. This results in drastically reducing the cloud consumption cost, as opposed to running the entire inventory of hosts 24/7. Citrix offers the admins the flexibility to define these hours with a granularity of 30 minutes. For multi-session delivery groups, admins can set the minimum number of running hosts separately for each half-hour of the day. For pooled single session delivery groups, admins can set the minimum number of running hosts separately for each hour of the day. Admins can set the working hours individually for different days.

### Load-based Scaling

Load-based scaling lets admins create a capacity buffer of machines in case they are needed to host sessions. The capacity buffer is a safety net to support unexpected increases in usage without negatively impacting the user experience. Ascertaining the right value for the capacity buffer (as a percentage of the pool capacity), is based on usage and the understanding the load variance in the customer environment.

As users login, the available capacity of the delivery group depletes. When it falls below the capacity buffer value, another machine in the pool is started to bring the capacity buffer back above the defined value. On the other side, when users start logging off, the machines with the least load are put in drain mode. Once the machines are clear of sessions, the machines are shut down until the pool capacity reduces to the set capacity buffer value.

[![Autoscale - Savings illustration](/en-us/tech-zone/learn/media/tech-briefs_autoscale_2-savings-illustration.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_2-savings-illustration.png)

## Load-balancing algorithms

Citrix employs a horizontal load-balancing algorithm by default to power manage the machines in the cloud. Incoming sessions are load balanced to optimize the user experience. Each new session is assigned to the machine that is least loaded. Once a new machine in a catalog is started, it will have the least load and would be used for all subsequent logons until the load is no longer the lowest.

In the below example, there are 6 machines in a delivery group. If the capacity buffer is set to 15%, then soon as free capacity goes below a new machine is powered-on. Now when a new user logs on then the user is assigned to the newly powered-on machine. The next user will be assigned the same machine (least loaded) and so on. This reduces the chances of any of the machines being able to be shut down as almost all have equal number of sessions running on them.

[![Autoscale - Horizontal Load-balancing](/en-us/tech-zone/learn/media/tech-briefs_autoscale_3-horizontal-load-balancing.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_3-horizontal-load-balancing.png)

With vertical load-balancing, admins can configure it so that incoming sessions are load balanced to optimize for least powered-on machine count. Sessions are assigned to the most loaded machine, so long as it does not reach the high watermark for max load. This ensures that only the minimum number of machines needed to service the current load are powered on, thus being much more cost effective. 
Consider the scenario where the admin is unsure how many machines are required to service user requests. Chances are, the admin over allocates machines for the delivery group. With vertical load balancing, then only the required number of machines will power on for the user load. The capacity buffer helps to keep additional machines available to handle load in this scenario.

In the below example, machine 3 would not be powered on till machine 1 is at its max load of 6 sessions and machine 2 has more than 4 sessions on it. Machine 3 would be powered off earlier as well, if the load falls. In addition, sessions on the least loaded machine are marked for logout first.

[![Autoscale - Vertical Load-balancing](/en-us/tech-zone/learn/media/tech-briefs_autoscale_4-vertical-load-balancing.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_4-vertical-load-balancing.png)

## Zone Preference and Tag Restriction

In hybrid deployments, there are resources in cloud and on-premises. A hybrid deployment can segment a single delivery group into different zones:

*  Primary: The preferred location for resource utilization.
*  Secondary: The resource location tapped when the primary zone is fully utilized.

For example, in a burst to the cloud scenario, an organization would fully utilize primary, on-premises resources.  When extra capacity is required, secondary instances are consumed from the cloud. The delivery group would consist of two catalogs, one for the on-premises resource location and another for the cloud location.

### Hybrid deployment with on-premises and cloud based instances

[![Autoscale - Zone Preference Hybrid Deployment](/en-us/tech-zone/learn/media/tech-briefs_autoscale_5-hybrid-zone-preference
.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_5-hybrid-zone-preference.png)

### Cloud deployment with reserved and pay-as-you-go-instances

[![Autoscale - Zone Preference Cloud only Deployment](/en-us/tech-zone/learn/media/tech-briefs_autoscale_6-cloud-zone-preference.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_6-cloud-zone-preference.png)

Zone preference and tagging is critical for reducing costs in hybrid deployments. Primary instance machines are paid for upfront. There is limited cost savings by powering these resources down. The organization uses these resources first before the secondary instances are used. 

The Citrix Virtual Apps and Desktops Service supports the use of multiple resource locations with the zone preference option. The admin uses zone preference to define which resource location to use first to fulfil demand and which location power down first when session demand drops. Once the capacity of the primary zone is fully utilized, the hosts marked as the secondary boot to serve session demand. When demand falls, the hosts in the secondary zone (cloud resources) are shutdown first, resulting in optimal cloud utilization.
The settings for these are done at the machine catalog level.

[![Autoscale - Zone Preference Config](/en-us/tech-zone/learn/media/tech-briefs_autoscale_7-zone-preference-config.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_7-zone-preference-config.png)

Admins must use tag restriction along with Zone preference to let Autoscale know the specific set of hosts (the cloud location) from the delivery group are to be power managed or not. 
Tag restriction specifies the hosts to be power managed by Autoscale; these are hosts earmarked to serve the burst/disaster recovery load.

[![Autoscale - Tagging](/en-us/tech-zone/learn/media/tech-briefs_autoscale_8-tags.png)](/en-us/tech-zone/learn/media/tech-briefs_autoscale_8-tags.png)

For more information, see [Zone Preference](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/manage-deployment/zones.html#zone-preference) and [Tags](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/manage-deployment/tags.html#manage-tags-and-tag-restrictions)

## Autoscale cost savings with Schedule-based scaling

To measure the cloud cost savings with Autoscale, we need to determine the cost of running machines in the cloud. The sizes and cost (as of writing this paper) for pay as you go in West US of the VMs, as calculated using [Azure pricing calculator](https://azure.microsoft.com/en-us/pricing/calculator) are as follows:

<Insert table>

Cost per month calculation assumes the machine is running for the entire month (730 hours). The storage cost is a fixed monthly cost regardless if the machine is powered on or off.

**With Autoscale, we reduce the time the machine remains powered-on to better align with user behaviors.**

To better determine the potential cost savings with Autoscale, Citrix conducted LoginVSI single server scalability tests with the following setup:

*  Citrix Virtual Apps and Desktops 1902
*  Citrix Workspace app 1903
*  Windows Server 2016 Multi-session OS Azure VMs
*    D3_V2
*    D4_V2
*    F16
*  LoginVSI Workload: Knowledge Worker

This resulted in the following user load per virtual machine instance:
