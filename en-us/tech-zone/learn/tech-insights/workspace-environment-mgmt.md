---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: Workspace Environment Management monitors and analyzes user and application behavior in real time, then intelligently adjusts system resources to improve the user experience.
tz_title: Workspace Environment Management
tz_products: citrix-virtual-apps-and-desktops;
---
# Tech Insight: Workspace Environment Management

To provide the best experience for users, Workspace Environment Management monitors and analyzes user and application behavior in real time. Workspace Environment Management then intelligently adjusts system resources in the user workspace environment. These capabilities improve the user experience and better utilize system resources for active processes.

The [Workspace Environment Management Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-environment-mgmt.html) provides additional details on the communication and architecture.

To see how these capabilities improve the overall experience, watch the following videos.

## CPU Optimization

Workspace Environment Management continuously monitors process CPU utilization in an attempt to identify processes that are overtaxing the CPU for an extended duration. When working in a multi-user environment, like VDI and Azure Virtual Desktop, a single user can inadvertently impact the experience of every other user. If one user runs a process that consumes a large percentage of CPU resources, it negatively impacts other users.

The CPU optimization capability within Workspace Environment Management can identify and implement corrective action to maintain the user experience.

**Watch this video to [learn more](https://www.youtube.com/watch?v=aRyn7JEVkOs):**

&nbsp;

{% include video.html id="aRyn7JEVkOs" type="youtube" %}

## Logon Optimization

Logging into a Windows-based machine can easily take 60+ seconds, depending on profiles, preferences, policies, login scripts, and mappings. With logon optimization capabilities, Workspace Environment Management can change the overall logon process to drastically improve logon time to 5-15 seconds.

**Watch this video to [learn more](https://www.youtube.com/watch?v=j44JzA7d0bM):**

&nbsp;

{% include video.html id="j44JzA7d0bM" type="youtube" %}

## RAM Optimization

In a multi-user environment like VDI and Azure Virtual Desktop, it is important that resources are released back to the host when idle. Releasing resources allows the operating system to reallocate to memory starved processes. Many applications, especially web browsers, continue to consume more RAM resources the longer they are running and do not release resources until they are closed.

With the RAM optimization capability within Workspace Environment Management, RAM is released back to the system if a process remains idle for a configured amount of time.

**Watch this video to [learn more](https://www.youtube.com/watch?v=Ey017aboRQc):**

&nbsp;

{% include video.html id="Ey017aboRQc" type="youtube" %}
