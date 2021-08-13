---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: A set of technologies ensuring an unparalleled user experience when accessing virtual Windows/Linux applications and desktops.
tz_title: HDX
tz_products: citrix-virtual-apps-and-desktops;
---
# Tech Insight: HDX

HDX is a set of remoting technologies providing the user with the best possible virtual application and desktop experience. The technologies within HDX include things like the ICA protocol, adaptive display, adaptive throughput, browser content redirection and more. Each technology within HDX focuses on a unique part of the overall virtual app and desktop session delivery approach. To see how these capabilities improve the overall experience, watch the following videos.

## Adaptive Display

Citrix Virtual Apps and Desktops incorporates multiple encoders, each optimized for different types of content. Instead of selecting a single encoder, adaptive display uses multiple encoders simultaneously.

Adaptive Display analyzes and segments the display into unique parts based on content. HDX uses the best encoder for each unique section of the display. Adaptive Display allows Citrix Virtual Apps and Desktops to deliver the best experience while optimizing CPU and bandwidth usage.

**Watch this video to [learn more](https://www.youtube.com/watch?v=xvzWWKhrGwE):**

&nbsp;

{% include video.html id="xvzWWKhrGwE" type="youtube" %}

## Adaptive Throughput

As network conditions change (latency, jitter, bandwidth), the amount of data being put onto the wire must adapt to avoid retransmissions or underutilization.

Citrix Virtual Apps and Desktops incorporates adaptive throughput to automatically change buffering dynamics. The dynamic buffer improves to improve overall throughput, which results in a better virtual application and desktop experience.

**Watch this video to [learn more](https://www.youtube.com/watch?v=tsq8petwsMw):**

&nbsp;

{% include video.html id="tsq8petwsMw" type="youtube" %}

## Adaptive Transport

Being able to fully utilize the available bandwidth allows the user to achieve the best experience possible based the underlying network conditions. However, as a user's physical distance from their virtual application or desktop increases, network latency continues to increase. Higher latency directly impacts the amount of data one can put on the wire when using a TCP-based transport.

Citrix Virtual Apps and Desktops utilizes an adaptive transport that automatically switches between TCP and an enlightened, UDP-based transport protocol.

**Watch this video to [learn more](https://www.youtube.com/watch?v=FyM47FDGw_4):**

&nbsp;

{% include video.html id="FyM47FDGw_4" type="youtube" %}

## Browser Content Redirection

The browser is one of the most used and most dynamic applications in any virtual desktop implementation. HDX applies different encoding algorithms to efficiently transport the content to the endpoint. However, certain public websites delivering high definition video content can be more efficiently delivered and rendered directly on the endpoint instead of on the virtual desktop.

Browser content redirection provides administrators with the ability to decide how different websites should be fetched and rendered.

**Watch this video to [learn more](https://www.youtube.com/watch?v=zYNE73utPQs):**

&nbsp;

{% include video.html id="zYNE73utPQs" type="youtube" %}

## Microsoft Teams Optimization

Meetings don't happen entirely in a conference room, which is why virtual conferencing software, like Microsoft Teams, is in high demand.

By optimizing the way Microsoft Teams voice and video communication packets cross the wire, Citrix Virtual Apps and Desktops delivers a virtual meeting experience identical to that of a traditional PC.

**Watch this video to [learn more](https://www.youtube.com/watch?v=BYzeltxcYJw):**

&nbsp;

{% include video.html id="BYzeltxcYJw" type="youtube" %}
