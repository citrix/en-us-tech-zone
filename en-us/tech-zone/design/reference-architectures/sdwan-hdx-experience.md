---
layout: doc
description: Measure HDX user interface performance improvements provided by Citrix SD-WAN flow enhancements

---
# Measuring HDX User Interface (UX) Performance Improvements with Citrix SD-WAN

## Contributors

**Authors:** 

[Matthew Brooks](https://twitter.com/tweetmattbrooks)

[Shoaib Yusuf](https://twitter.com/Shoaibys)

**Special Thanks:**  Daljit Singh, Dan Feller, Derek Thorslund, Jaskirat Singh Chauhan, Karthick Srivatsan, Mallikarjuna Komma, Mallikarjuna Reddy M, Muhammad Dawood, Ramanjaneya Kamalapuram, Ravindra L Patil, Siva Prasad, and Thavamani Rajan

## Executive Summary

In a Citrix Virtual Apps and Desktops environment, the length of time it takes a user’s action on an endpoint to get sent to the virtual desktop, get rendered and transmitted back to the user’s display determines the responsiveness of the session.  This “graphical” round-trip time directly impacts the overall user experience.

Based on tests comparing a traditional routed network versus one using Citrix SD-WAN:

*  Citrix SD-WAN reduced the “graphical” round-trip time by greater than 500%, when congestion was encountered, in comparison to a traditional routed network.

*  When redundant links were added into the environment, Citrix SD-WAN reduced the round-trip time by greater than 4 seconds on average, when congestion was encountered, in comparison to a traditional routed network.

Citrix SD-WAN provides a better user experience than traditional routing when network conditions are not ideal.

SD-WAN seamlessly routes traffic, on a per-packet basis, over the best available path according to quality requirements whether on-premises or in the cloud.  Citrix SD-WAN can uniquely analyze the HDX protocol flows, used to transport Citrix Virtual Apps and Desktops sessions, and prioritizes delivery of each packet by class of service to maximize the user experience.

## Success Criteria

Citrix Virtual Apps and Desktops is heavily reliant on the quality of the network connection between the user and the virtual app and desktop. While Citrix’s HDX technologies and ICA protocol will squeeze the best performance possible out of any network, a better connection results in a better user experience.  Optimizing the session traffic through quality of service handling, queuing, and routing is an effective strategy for improving the user experience, especially when the network conditions are not ideal.  These are the success criteria that were used to evaluate HDX network performance improvements with Citrix SD-WAN:

1.  Use the default settings that come with a new implementation, using software-based Citrix SD-WAN instances (VPX) and the latest software version (11.0.3 at the time of testing).  For the comparison network, also use the default settings on open source router instances, which would also be software based.  This includes no special provisions that may improve forwarding performance such as custom queuing, load balancing, etc.
2.  Both the Citrix SD-WAN instances and open source router instances should be allocated the same amount of memory on the hypervisor host.
3.  Design an environment that may be reproduced by Citrix customers, field employees and partners to observe similar results.
4.  Develop an isolated environment that will provide consistent, reproducible results free of variable networking conditions that may occur in the cloud.
5.  The environment should use Citrix Storefront to enumerate virtual apps and desktops in Workspace App, yet results should also apply to Citrix Virtual Apps and Desktops service delivered via Citrix Workspace.
6.  Follow or highlight Citrix best practices and recommendations where possible.

## Scenarios

### Measurement

Citrix [HDX](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/technical-overview/hdx.html) technology stack is a set of capabilities that work together to deliver a high-definition user experience on virtual desktops and applications, to any device, over any network, from a central location. Citrix Independent Computing Architecture ([ICA](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/technical-overview/virtual-channels.html)) is a proprietary Citrix protocol that provides a detailed framework for passing data between virtual server resources and client endpoints. Therefore, it includes a server component or virtual delivery agents (VDAs), a network protocol component, and a client component (Citrix Workspace App).

ICA Round Trip Time (RTT) is used to measure the delivery of virtual session graphics output on a user’s virtual app or desktop. It is the elapsed time from when the user requests content until the content response is displayed in the virtual app or desktop session.

Therefore, ICA RTT constitutes the actual application layer delay, which includes the time required for the action to traverse the following components:

[![ICA RTT](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_ICARTT.png)](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_ICARTT.png)

The ICA RTT counter was used to measure the quantitative HDX performance for each test. It was captured directly on the VDA during each test.  The same client and server infrastructure were used for each test iteration, therefore only changes made to the network had a relative impact on the ICA RTT measured between tests.

### Network Topologies

Every set of tests ran for three iterations using the following network topologies to compare Citrix SD-WAN HDX UX performance improvements:

[![Network Topologies](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_nettopos.png)](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_nettopos.png)


1. **Routed + MPLS** – using this environment, HDX traffic from the London (LON) Client to the New York City (NYC) VDA traverses the routed “underlay” network’s MPLS path.

The link throughput is rate limited to 2Mbps similar to a branch with an E1 or T1 (1.544 Mpbs) leased line.  This type of topology is commonly used to backhaul all traffic to a central data center to access apps and data on the intranet or for security inspection prior to being routed to the Internet.

2. **SD-WAN + MPLS** - using this environment, HDX traffic from the LON Client to the NYC VDA traverses the SD-WAN “overlay” network, consisting of only a single MPLS path.

Here we will observe the Citrix SD-WAN appliance’s ability to automatically identify and prioritize ICA traffic classes, providing Quality of Service to the delivery across the wide area network.  While it is ultimately delivered over the same routed network infrastructure (used in the Routed + MPLS scenario) its unique performance enhancement capabilities provide the best user experience for Citrix Virtual Apps and Desktops session traffic.

3. **SD-WAN + MPLS + Internet** - using this environment, HDX traffic from the LON Client to the NYC VDA traverses the SD-WAN “overlay” network’s MPLS and Internet (INET) paths.

Here we will observe Citrix SD-WAN ability to seamless redirect HDX flows, on a per-packet basis, securely over the INET path, when it detects poor quality on the MPLS path.  The link throughput is rate limited to 20 Mbps similar to a branch with a DSL or Cable link.

Both SD-WAN instances analyze the conditions of each available path, using real-time measurements of best one-way latency, loss, jitter, and congestion, that are introduced in the test cases.  Additionally, the instances provide dynamic Quality of Service to efficiently deliver the HDX traffic across the virtualized wide area network, achieving optimal user experience.

### Test Cases

*  While running each test case, ICA RTT was captured using the Windows Management Instrumentation Interface (WMI).

a) From the NYC-VDA MsDos prompt the following was executed:
**wmic**
**/namespace:\\root\citrix\euem“path citrix_euem_RoundTrip get /value”**

b) The result of the ICA “Round Trip Time” counter was then recorded.

*  On the LON Client, the playback of a video was looped.  The video pertained to Citrix Workspace with Intelligence where a “virtual tornado” represented the many elements IT teams must manage was spinning rapidly.  During each test case we observed the graphics quality, and how rapidly the “virtual tornado” was spinning.

| Test       | Overview           | Description  | Observation |
| :-------------:| :-------------:| :--------:| :--------:|
| Baseline      | Steady state environment| This test captured the ICA RTT “baseline” after the virtual desktop was launched on the Windows 10 client and in a “steady state”.| Citrix SD-WAN instances are delivering the virtual desktop HDX flows, over virtual paths, using UDP transport. We’ll notice a few ms delay added in the Citrix SD-WAN topologies, but we will see the invaluable performance benefits in the results of the additional tests.|
| Latency      | (+) added “latency” | This test captured the ICA RTT with a 100ms delay added to the MPLS path, via a WAN emulator, between the Client and VDA. | There is no visible effect to the video playback on the client.  The ICA RTT increased by 100ms in both the Routed and SD-WAN single-path iteration, but the SD-WAN multi-path  iteration remained the same since the HDX traffic was redirected from the MPLS path with the added latency to the Internet (INET) path. |
| Interactive BW | (+) added “interactive” BW | This test by ran a video loop, using a .mp4 file hosted on the VDA, with no caching on the player | The video plays unimpeded by network constraints in each topology. The graphics quality is good and the “virtual tornado” is spinning rapidly. |
| Congestion BW | (+) added “congestion” BW | This test added “congestion” by sending CIFS traffic by copying a large file from the VDA file system to the Windows 10 Client file system. | ICA RTT increases significantly in the Routed iteration since it directly fights for transmit queue buffers on the LON_CE_Rtr. Here we see the “virtual tornado” begin to slow and pause intermittently. For the SD-WAN multi-stream iteration again there is no effect since the SD-WAN instance is dynamically rerouting HDX flows over the INET path. For the SD-WAN single-stream iteration there is some effect since there is congestion queuing and policing measures take effect.  However, since the HDX traffic ICA classes have appropriate queue allocations there is no virtual no noticeable effect on the video playback. |
| Bulk BW      | (+) added “bulk” BW | This test copied a large file from the VDA to the Client file system, from within the virtual desktop, which occurs within the HDX session. | We’ll observe that ICA RTT increases significantly in the Routed iteration since now there is greater demand for bandwidth, and transmit queues, while the client and VDA continue to need to retransmit lost packets, while fighting for transmit queue buffers on the LON_CE_Rtr. For the SD-WAN multi-stream iteration again there is no effect since the SD-WAN instance is dynamically rerouting HDX flows over the INET path. For the SD-WAN single-stream iteration there is some effect since now there are additional, and bandwidth demands while the client and VDA need to retransmit lost packets and congestion queuing and policing measures are in effect.   Yet effect on the video playback of the “virtual tornado” is mitigated.  Here we observe the unique [Citrix SD-WAN ICA classification](https://docs.citrix.com/en-us/citrix-sd-wan/11/quality-of-service/app-classification-sd-wan.html) benefits as the HDX flows are automatically streamed into 4 separate classes and class_0 voice and class_1 video flows are prioritized over the class_2 bulk file transfer traffic. |
| Loss      | (+) added 25% loss | This test added 25% loss directly to the MPLS path, between the Client and VDA using Wan Emulation. | We’ll observe that ICA RTT increases significantly in the Routed iteration since now the client and VDA need to retransmit lost packets, while fighting for transmit queue buffers on the LON_CE_Rtr. For the SD-WAN multi-stream iteration again there is no effect since the SD-WAN instance is dynamically rerouting HDX flows over the INET path. For the SD-WAN single-stream iteration there is some effect since now the client and VDA need to retransmit lost packets and congestion queuing and policing measures are in effect.  However, since the HDX traffic ICA classes have appropriate queue allocations the effect is nearly not noticeable to the user experience. |

### Results

When reviewing the quantitative results between the topologies Routed + MPLS and the Citrix SD-WAN + MPLS, Citrix SD-WAN provided **greater than 500% improvement** delivering HDX traffic running video, and a large file transfer, when faced with latency, congestion, and loss, using a single MPLS link, in comparison to HDX delivered over a traditional routed network.  When a second path, over an Internet link, in the Citrix SD-WAN + MPLS + INET topology was added there was greater than a 4 second reduction in in the ICA Round Trip time using the same test scenarios.

[![Results](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_results.png)](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_results.png)

Below are the results and detailed test steps used to measure quantitative benefits of delivery with Citrix SD-WAN versus delivery over a traditional routed network without Citrix SD-WAN.

ICA RTT was configured for measurement every second by the VDA. For the Interactive BW, Congestion BW, Bulk BW, and Loss tests the RTT varied increasingly due to the increased bandwidth and subsequent queue management and retransmissions.  Therefore, three measurements were captured, and the median value was recorded. Each complete set of tests, for all three scenarios, were further repeated three times and the median value was again recorded as the result.


| Test Case | Steps | ROUTED + MPLS | SD-WAN + MPLS | SD-WAN + MPLS + INET |
| :-------------: |:-------------:| :--------:| :--------:| :--------:|
| Baseline      | a. Open a Google Chrome browser b. Navigate to ddc.training.lab/Citrix/StoreWeb/ c. Login as user1@training.lab / Citrix123 d. Launch desktop NYC. e. RECORD ICA RTT on NYC-VDA | 3 | 6 | 6 |
| Latency      | a. Open Google Chrome from the NYC Util server and navigate to 192.168.10.26/WANem b. Select “Advanced Mode” > eth1 c. Set the “Delay time” field to 100 and select “Apply Setting” toward the bottom of the screen d. OBSERVE the Video on the LON_Client e. RECORD ICA RTT on NYC-VDA | 104 | 13 | 109 |
| Interactive Bandwidth (BW)      | a. Within the virtual desktop open VLC media player b. Select Media > Open File > C://Citrix_Workspace_with_Intelligence.mp4 c. Select View > Advanced Controls d. Set the video at the start of the ‘virtual hurricane’ and click the third button ‘Loop from point A to point B continuously’. You should see the first part of the icon turn red. Set the video to the end of the ‘virtual hurricane’ and press the button again. The second part of the loop button should not be red too. Press Play to view the loop. e. OBSERVE the Video on the LON_Client f. RECORD ICA RTT on NYC-VDA | 126 | 16 | 142 |
| Congestion BW      | a. On the LON_Client open File Explorer b. Navigate to the “FILES” share on NYC_VDA and copy “LARGE_FILE.mp4” to the C:\outside-HDX-download on LON_Client c. OBSERVE the Video on the LON_Client d. RECORD ICA RTT on NYC-VDA | 1606 | 63 | 485 |
| Bulk BW      | a. Within the virtual desktop running on LON_Client open File Explorer b. Navigate to the C:\FILES (on NYC_VDA) and copy “LARGE_FILE.mp4” to the C:\inside-HDX-download on LON_Client c. OBSERVE the Video on the LON_Client d. RECORD ICA RTT on NYC-VDA | 3486 | 62 | 465 |
| Loss      | a. Open Google Chrome from the NYC Util server and navigate to http:192.168.10.26/WANem b. Select “Advanced Mode” > eth1 c. Set the “Loss” field to 25 and select “Apply Setting” toward the bottom of the screen d. OBSERVE the Video on the LON_Client e. RECORD ICA RTT on NYC-VDA | 4243 | 64 | 536 |

**NOTE**: Find a worksheet with a detailed compilation of results [here]( https://citrix.sharefile.com/d-s80a041db88d46ceb).

## Conceptual Architecture

The architecture is based on a mock setup with a Windows 10 client, hosted in a London (LON) branch, connecting to a Citrix Virtual Apps and Desktops virtual delivery agent (VDA), running on Windows Server 2016, hosted in a New York City (NYC) data center.

A Citrix SD-WAN Standard Edition (SE) VPX instance hosted in NYC, is configured as the Master Control Node (MCN), while another Citrix SD-WAN Standard Edition (SE) VPX instance is hosted in LON.  The SD-WAN instances create virtual paths over an MPLS and or Internet (INET) link to optimize delivery of the Citrix Virtual Apps and Desktops HDX session flows.

Citrix SD-WAN supports a variety of [deployment topologies](https://docs.citrix.com/en-us/citrix-sd-wan/11/use-cases-sd-wan-virtual-routing.html) to provide flexible options to integrate with Enterprise networks. The NYC_SDWAN_SE instance is deployed in “virtual inline mode”, which is a common deployment for Data Center networks.  It relies on dynamic routing with the NYC_Core_Rtr (LAN) to take over routing for NYC LAN traffic and manage delivery of HDX flows to and from the NYC_VDA.  While the LON_SDWAN_SE instance is deployed in “inline mode”, which is a common deployment for branch office networks. It sits as a bridge between the LON_Client LAN and the LON_CE_Rtr (LAN/WAN) LAN and intervenes to manage delivery of HDX flows to and from the LON_Client.

A series of Vyatta routers are implemented to simulate an MPLS provider network and an Internet provider network.  A WanEm virtual appliance is used to interject latency and loss.

[![Architecture](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_architecture.png)](/en-us/tech-zone/design/media/reference-architecture_sdwan-hdx-experience_architecture.png)

### Environment

The environment is composed of various software and virtual machines hosted on a Citrix Hypervisor.  It is based on an environment that is commonly used for Citrix readiness labs used for field and events training.

The open source routers used have limited memory, in order to be hosted within the confines of a 32G hypervisor (including supporting management router and jump server) and would typically use more if used in production.  However, the SD-WAN virtual paths traverse the same paths and open source routers as the directly routed traffic.  Therefore, the absolute measurements should not be referenced, rather the comparative differences that demonstrate quantitative benefits of using Citrix SD-WAN to deliver HDX sessions.  The NYC_Lan (Core_Rtr) and LON_LAN/WAN (CE_Rtr) routers were allocated the same 4G of memory that is allocated to the NYC_SDWAN_SE and LON_SDWAN_SE Citrix SD-WAN instances.

### Hardware

| Component | Notes (Location/Resources/Version)|
| :-------------:| :-------------:|
| Server (Citrix Hypervisor)   | 32G RAM |

### Software

| Component | Notes (Location/Resources/Version)|
| :-------------:| :-------------:|
| SD-WAN   | Citrix SD-WAN VPX  |
| Routers   | Vyatta OS  |
| Wan Emulation | WanEm open source tool  |
| Citrix Virtual Apps and Desktops | V7 1912 LTSR |
| Deliver Controller and VDA | Windows Server 2016  |
| Domain Controller and Utility Server | Windows Server 2012 R2 |
| Client   | Windows 10  |

#### Citrix SD-WAN

[Citrix SD-WAN](https://docs.citrix.com/en-us/citrix-sd-wan.html) simplifies branch networking with a reliable and high-performance workspace experience that helps accessing SaaS applications, virtual desktops, or traditional data centers.

The [Citrix SD-WAN Standard Edition](https://docs.citrix.com/en-us/citrix-sd-wan/11.html) used in testing includes Virtual WAN features. It supports software-defined WAN capability to create a highly reliable network from multiple network links and to ensure that each application takes the best path to achieve the highest application performance.

#### Citrix Virtual Apps and Desktops

[Citrix Virtual Apps and Desktops](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/technical-overview.html) are virtualization solutions that give IT control of virtual machines, applications, licensing, and security while providing anywhere access for any device.
Citrix Virtual Apps and Desktops allow:
* End users to run applications and desktops independently of the device’s operating system and interface.
* Administrators to manage the network and control access from selected devices or from all devices.
* Administrators to manage an entire network from a single data center.

### Virtual Machines

| Component | OS | Memory |
| :-------------:| :-------------:| :-------------:|
| AD.training.lab | Windows Server 2012 R2 | .5G |
| LON_LAN/WAN (CE_Rtr) | Vyatta | 4G |
| LON_Client | Window 10 | 1G |
| LON_SDWAN_SE | Citrix SD-WAN | 4G |
| NYC_DDC | Windows Server 2016 | 4G |
| NYC_VDA | Windows Server 2016 | 3G |
| NYC_Lan (Core_Rtr) | Vyatta | 4G |
| NYC_INET_Rtr | Vyatta | .5G |
| NYC_MPLS_Rtr | Vyatta | .5G |
| NYC_SDWAN_SE | Citrix SD-WAN | 4G |
| NYC_Server | Windows Server 2012 R2 | .5G |
| PE_INET_Rtr | Vyatta | .5G |
| PE_MPLS_Rtr | Vyatta | .5G |
| PE_WANem | WanEm | .5G |
	
### Network

| Component | Vlan | IP Address |
| :-------------:| :-------------:| :-------------:|
| AD.training.lab | Internal / NYC_LAN | 192.168.10.11 / 172.16.10.11 |
| LON_CE_Rtr | LON_SD / PE_WeMPLS_LON | 172.70.1.1 / 169.15.70.3 |
| LON_Client | Internal / LON_SD | 192.168.10.28 / 172.70.1.28 |
| LON_SDWAN_SE | Internal / LON_SD / LON_LAN / PE_WeINET_LON | 192.168.10.201 / 172.70.1.27 / 172.70.1.28 / 169.15.71.3 |
| NYC_DDC | Internal / NYC_LAN | 192.168.10.200 / 172.16.10.51 |
| NYC_VDA | Internal / NYC_LAN | 192.168.10.202 / 172.16.10.53 |
| NYC_Core_Rtr | NYC_LAN / NYC_RtrMPLS / NYC_RtrINET / NYC_SD | 172.16.10.199 / 172.16.20.2 / 172.16.30.4 / 172.16.40.1 |
| NYC_INET_Rtr | NYC_Rtr_INET / PE_INET_NY | 172.16.30.3 / 169.15.60.2 |
| NYC_MPLS_Rtr | NYC_RtrMPLS / PE_MPLS_NYC | 172.16.20.1 / 169.15.50.2 |
| NYC_SDWAN_SE | Internal / NYC_SD| 192.168.10.200 / 172.16.40.2 |
| NYC_Server | Internal / NYC_LAN | 192.168.10.12 / 172.16.10.12 |
| PE_INET_Rtr | Internal / PE_INET_NYC / PE_INET_WeLON | 192.168.10.92 /  169.15.60.1 / 169.15.71.1 |
| PE_MPLS_Rtr | PE_MPLS_NYC / PE_MPLS_WeLON | 169.15.50.1 / 169.15.70.1 |
| PE_WANem | Internal / PE_MPLS_WeLON / PE_WeMPLS_LON / PE_INET_WeLON /  PE_WeINET_LON | 192.168.10.26 / 169.15.70.2 / 169.15.70.2 / 169.15.71.2 / 169.15.71.2 |

## Summary

Citrix SD-WAN significantly improves the network performance of Citrix Virtual Apps and Desktops HDX sessions, over environments without Citrix SD-WAN, resulting in a better user experience.  This was demonstrated by measuring the quantitative results of test scenarios under a variety of network constraints.

A couple key reasons behind the network performance improvements that were observed include:

*  The inherent nature of SD-WAN technology’s ability to route traffic dynamically based on real time network conditions, as opposed to traditional routers that rely on routing protocol timeouts.
*  Only Citrix SD-WAN “Multi-stream HDX Auto-QoS” can dynamically identify and prioritize HDX flows according to ICA class of service resulting in the least delay for the most time sensitive data.
