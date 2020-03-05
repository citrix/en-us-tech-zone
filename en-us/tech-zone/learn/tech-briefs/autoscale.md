---
layout: doc
---
# Autoscale

## Contributors

**Author:** [Mayank Singh](https://twitter.com/techmayank)

**Special Thanks:** Nitin D. Mehta and Daniel Feller

# Overview

A poorly designed power-management scheme can increase cloud computing costs by ???% over what the organization requires to support the users. 
Cloud-based services often use a pay-as-you-go model where organizations are billed, sometimes every second, for compute time, network throughput, storage consumption and transactions. To minimize costs, organizations need to intelligently utilize, allocate and deallocate resources, which is in stark contrast from a traditional, on-premises model where organizations leave resources allocated indefinitely. 
In a virtual desktop deployment, organizations must consider the cost of permanently allocating hundreds or thousands of virtual machines that are minimally utilized. This approach will make the solution too expensive to be feasible. 
The Citrix Virtual Apps and Desktops service leverages Autoscale with vertical load balancing to help lower cloud costs. These capabilities allows organizations to fully utilize virtual desktops, identify usage trends and convert those trends into schedule and load-based rules that dynamically allocates and deallocates resources to maintain a positive user experience.

# Cost vs Experience

The main impact of power managing machines on user experience manifests in the time taken to connect a user to the requested session. If the power management is too aggressive and any machine that is not hosting sessions is shutdown, the pool capacity (from running machines) depletes. When another user requests a session and the powered on machines have no available capacity, the user must wait for a machine to be powered-on. This affects the user experience negatively, adding up to a few minutes to the session launch time. When a large number of users login simultaneously, for example at the beginning of a shift, the boot time could be even longer as a number of machines need to be powered on at the same time.
Cost savings from powering off unused machines come from the compute cost being nil, and reduction of network ingress/egress and data transactions (reading and writing to storage) cost.
This results in the admin needing to perform a balancing act of ensuring that users can login to sessions quickly, while keeping costs down by shutting down as many unutilized machines as is optimal.

1) To effectively balance cost vs experience, Citrix incorporates the following technologies: Autoscale
2)	Load balancing algorithms
3)	Zone Preference and Tagging




