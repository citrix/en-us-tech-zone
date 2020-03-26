---
layout: doc
description: Measure HDX user interface performance improvements provided by Citrix SD-WAN flow enhancements

---
# Measuring Citrix SD-WAN performance improvements to Citrix Virtual Apps and Desktops HDX sessions

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:**  Daljit Singh, Dan Feller, Derek Thorslund, Jaskirat Singh Chauhan, Karthick Srivatsan, Mallikarjuna Komma, Mallikarjuna Reddy M, Muhammad Dawood, Ramanjaneya Kamalapuram, Ravindra L Patil, Shoaib Yusuf, Siva Prasad, and Thavamani Rajan

## Executive Summary
In a Citrix Virtual Apps and Desktops environment, the length of time it takes a user’s action on an endpoint to get sent to the virtual desktop, get rendered and transmitted back to the user’s display determines the responsiveness of the session.  This “graphical” round-trip time directly impacts the overall user experience.

Based on tests comparing a traditional routed network versus one using Citrix SD-WAN:

* Citrix SD-WAN reduced the “graphical” round-trip time by greater than 500%, when congestion was encountered, in comparison to a traditional routed network.

* When redundant links were added into the environment, Citrix SD-WAN reduced the round-trip time by greater than 4 seconds on average, when congestion was encountered, in comparison to a traditional routed network.

Citrix SD-WAN provides a better user experience than traditional routing when network conditions are not ideal.

SD-WAN seamlessly routes traffic, on a per-packet basis, over the best available path according to quality requirements whether on-premises or in the cloud.  Citrix SD-WAN can uniquely analyze the HDX protocol flows, used to transport Citrix Virtual Apps and Desktops sessions, and prioritizes delivery of each packet by class of service to maximize the user experience.

## Success Criteria

Citrix Virtual Apps and Desktops is heavily reliant on the quality of the network connection between the user and the virtual app and desktop. While Citrix’s HDX technologies and ICA protocol will squeeze the best performance possible out of any network, a better connection results in a better user experience.  Optimizing the session traffic through quality of service handling, queuing, and routing is an effective strategy for improving the user experience, especially when the network conditions are not ideal.  These are the success criteria that were used to evaluate HDX network performance improvements with Citrix SD-WAN:

1. Use the default settings that come with a new implementation, using software-based Citrix SD-WAN instances (VPX) and the latest software version (11.0.3 at the time of testing).  For the comparison network, also use the default settings on open source router instances, which would also be software based.  This includes no special provisions that may improve forwarding performance such as custom queuing, load balancing, etc.
2. Both the Citrix SD-WAN instances and open source router instances should be allocated the same amount of memory on the hypervisor host.
3. Design an environment that may be reproduced by Citrix customers, field employees and partners to observe similar results.
4. Develop an isolated environment that will provide consistent, reproducible results free of variable networking conditions that may occur in the cloud.
5. The environment should use Citrix Storefront to enumerate virtual apps and desktops in Workspace App, yet results should also apply to Citrix Virtual Apps and Desktops service delivered via Citrix Workspace.
6. Follow or highlight Citrix best practices and recommendations where possible.

## Scenarios

### Measurement

Citrix [HDX](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/technical-overview/hdx.html) technology stack is a set of capabilities that work together to deliver a high-definition user experience on virtual desktops and applications, to any device, over any network, from a central location. Citrix Independent Computing Architecture ([ICA](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/technical-overview/virtual-channels.html)) is a proprietary Citrix protocol that provides a detailed framework for passing data between virtual server resources and client endpoints. Therefore, it includes a server component or virtual delivery agents (VDAs), a network protocol component, and a client component (Citrix Workspace App).

ICA Round Trip Time (RTT) is used to measure the delivery of virtual session graphics output on a user’s virtual app or desktop. It is the elapsed time from when the user requests content until the content response is displayed in the virtual app or desktop session.

Therefore, ICA RTT constitutes the actual application layer delay, which includes the time required for the action to traverse the following components:

[![ICA RTT](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_ICARTT.png)](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_ICARTT.png)
