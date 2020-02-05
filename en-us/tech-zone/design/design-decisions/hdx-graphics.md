---
layout: doc
description: Copy & paste description from TOC here
---
# HDX Graphics Overview

## Contributors

**Author:** [Rico Projer](https://www.linkedin.com/in/rico-projer-0a8b2354/), [Jason Delgado](https://www.linkedin.com/in/jason-delgado-819a0575/)

**Special thanks:** [Saša Petrović](https://twitter.com/petrovicsasa), [Muhammad Dawood](https://www.linkedin.com/in/muhammad-dawood/), [Nick Czabaranek](https://www.linkedin.com/in/nick-czabaranek-9b03504/)

## Introduction

To meet different user requirements, Citrix' HDX protocol allows for different graphics modes to be configured. The purpose of this article is to outline the different HDX modes and how they are configured. It will give you a starting point from where you can configure your environment to best fit the needs of your users, your workload, and the current network conditions.

Important to note: This article is based on Citrix Virtual Apps and Desktops 1912 unless stated otherwise. For an overview based on 7.15, please consult the blog article [HDX Graphics Encoder Configuration Overview – What Really Matters!](https://www.citrix.com/blogs/2017/09/29/hdx-graphics-encoder-configuration-overview-what-really-matters/).

DISCLAIMER: Results may vary and it is highly recommended to run your own tests to see what works best for your use-case(s).

## HDX Graphics Overview

Before diving into the specific graphics policies, let’s review how we categorize what you see on your HDX session screen and the underlying technologies that are used for presentation.

Within Citrix Virtual Apps and Desktops, there are two main display technologies at work:  **Thinwire+** and **Fullscreen H.264/H.265**.  Citrix adapts the use of industry leading standards, H.264 and H.265 for efficient delivery of high-quality video content in its “Full Screen” and “Selective” codec implementations.

*  **Thinwire+** is an adaptive remote display technology that senses regions of transient content (fluid images or video) and encodes it based on set policy and capabilities detected on the endpoint.  Thinwire+ encodes these “selected” (or transient) regions either as Adaptive JPEG or H.264/H.265.  Adaptive JPEG and “Selective” H.264/H.265 are considered sub-features as Thinwire+ is the core technology.  The remaining, non-transient regions (encoded as JPEG and RLE) are then combined to complete the in-session display.
*  **Fullscreen H.264/H.265** treats the entire screen as transient content, with the exception of text (by default), and encodes display data using one of these two codecs.  Text is then overlayed onto the screen to provide a complete image.  H.265 achieves higher compression over H.264 without compromising quality.  However, H.265 is expensive in terms of processing and is currently only supported when used with [select GPUs](/en-us/citrix-workspace-app-for-windows/configure.html#h265-video-encoding) on the VDA.  Additionally, H.265 compatible hardware, in the form of GPU or purpose-built thin client,  is required for decoding H.265 display data on the client endpoint.  Please review vendor documentation to determine H.265 supportability for your endpoint hardware.

As H.264 compatibility has a broader base, we will focus on Fullscreen H.264 and Selective H.264 within this article unless otherwise noted.

As we deliver graphic content for applications or desktops the HDX graphics encoding engine dynamically categorizes display data into three types:

*  Text or Simple Images
*  Static Image Content
*  Moving (or Fluid) Images

![HDX Graphics 1](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_000.png)

In the example above, text or simple images are highlighted in blue, static images in orange and moving (or fluid) images in green.

Depending on the HDX mode configured, these categories are encoded by different means:

*  **Text and simple images** are almost always encoded in a lossless fashion using Run-Length Encoding (RLE). Starting with Version 7.17, a Citrix proprietary RLE flavor called MDRLE is used which allows for a better compression rate [CTX232041](https://support.citrix.com/article/CTX232041). As we call out in the Visio chart below, enabling the **Optimize for 3D graphics workload** will disable the lossless text detection and transfer the content with H.264/H.265, rather than RLE as our default.
*  For **static images**, with Thinwire+ JPEG is used for encoding while the video codec H.264/H.265 is used if Full Screen H.264 has been chosen as the graphics mode. If JPEG is used, the quality of it can be configured with the Visual Quality setting. Check the attached Visio chart below for more details.
*  For **moving images**, the video codec H.264/H.265 is used when configuring Full Screen H.264 or Thinwire+ with Selective H.264. If Thinwire+ with Adaptive JPEG has been configured, JPEG is used with a quality that automatically adapts (hence the name) to conditions such as frame rate and available bandwidth.

So, to recap the following three different flavors can be configured:

**Thinwire+ with Adaptive JPEG**
Text: RLE
Static images: JPEG
Moving Images: Adaptive JPEG

**Thinwire+ with Selective H.264**
Text: RLE
Static images: JPEG
Moving Images: H.264

**Full Screen H.264**
Text: RLE (or H.264 if Optimize for 3D graphics workload has been enabled)
Static images: H.264
Moving Images: H.264
But how can these modes be configured?

In the next section, we will cover how these modes can be configured.

## HDX Graphics Modes

The **Use Video Codec for Compression** policy is the central function to give your end-users an optimal experience by configuring display methods appropriate for different use-cases.  Below, we map the technologies we outlined above with policy settings configurable within Citrix Studio.

*  **For Actively Changing Regions** = Thinwire+ with Selective H.264
*  **Do Not Use Video Codec (default fallback method)** = Thinwire+ with Adaptive JPEG
*  **For the Entire Screen** = Full Screen H.264
*  **Use When Preferred** (policy default) = **Thinwire+ with Selective H.264** will be used unless **Optimize for 3D Graphics Workloads** is also set, then **Full Screen H.264** will be used.

Capabilities of both the client endpoint and the Virtual Delivery Agent (VDA) are evaluated at session launch or session reconnect.  If the client does not support H.264, the display method will be Thinwire+ with Adaptive JPEG regardless of the policy set on the VDA.

### For Actively Changing Regions (Thinwire+ with Selective H.264)

The **For Actively Changing Regions** graphics mode is our most balanced setting.  As such, we recommend starting with this mode as you begin to baseline policies within your environment since it covers a wide user base (i.e. Office worker with occasional video playback).

At its core is Thinwire+, leveraging still image (JPEG) compression, RLE for text, and bitmap caching for areas of the screen that are determined by the VDA to be static.  The VDA continuously analyzes the screen for regions of fluid movement, such as multimedia, and selectively uses **H.264** to encode the fluid region.

As illustrated below, H.264 is “Inactive” until regions of fluid movement are detected.  The VDA then transitions to H.264 to encode the selected region during fluid movement and returns to an “Inactive” state once the selected region no longer contains fluid content.

![HDX Graphics 2](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_001.png)

H.264 provides a much more rich experience than adaptive JPEG at the expense of CPU to compress regions of fluid movement.  Network bandwidth will generally be less with H.264 compared to adaptive JPEG for multimedia workload. It is highly recommended to run your own tests with your specific use-case (check the Tools section below).

### Do Not Use Video Codec (Thinwire+ with Adaptive JPEG)

The **Do Not Use Video Codec** offers maximum compatibility for client endpoints, including endpoints that do not support the decoding of H.264 graphics.

Similar to the **For Actively Changing Regions** setting, Thinwire+ is also at the core of this graphics mode. As stated in the HDX Graphics Overview section, you should consider Thinwire+ as the main feature, with Adaptive JPEG and Selective H.264 as sub-features as in below:

Thinwire+

*  Selective H.264
*  Adaptive JPEG

In this graphics mode, Thinwire+ on the VDA analyzes the screen for regions of fluid movement.  Rather than encoding with H.264, however, Thinwire+ encodes moving images as Adaptive JPEG to deliver high compatibility or where H.264 is not needed.  The remaining regions are presented as JPEG for still images, and RLE for text and simple graphics to deliver quality imagery.

CPU processing for encoding moving images using Adaptive JPEG is typically lower than with Thinwire+ with Selective H.264 or Full Screen H.264.  This mode is desired if server scalability is your priority.  The tradeoff is seen in terms of increased bandwidth and decreased moving image fidelity in WAN scenarios.  This graphics mode should be limited to the use-case where accessing moving images is minimal, such as in a call center or point of sale system.  In which case, the bandwidth utilization in this mode would be similar in comparison to Thinwire+ with Selective H.264.

Thinwire+ with Adaptive JPEG is the default fall back method for the other two graphics modes (Thinwire+ with Selective H.264 and Full Screen H.264).

### For the Entire Screen (Full Screen H.264)

The **For the Entire Screen** graphics mode setting configures the VDA to encode all display data using H.264, with the exception of text.  Text is encoded using RLE and is overlayed with the remainder of the screen.  If **Optimize for 3D Graphics Workloads** is enabled the entire screen, including text, is encoded as H.264.

Fullscreen H.264 is designed for the heavy multimedia use-case, where larger regions of the screen will be in motion.  Higher compression and quality is achieved at the expense of CPU and server scalability.

On its own, this mode provides the best user experience when heavy multimedia, 3-D modelling, or CAD drawing applications are in use.  The CPU can quickly become a bottleneck, if undersized, resulting in poor performance and user experience under heavy multimedia conditions.  Consider GPU offload capabilities to supplement this graphics mode while using these application types.

## HDX Graphics Configurations

As the **Use Video Codec for Compression** policy is a good starting point to baseline your configuration, additional policies can be set to further customize your visual policies to fit your different workloads.  By customizing these supporting policy settings, you can opt to reduce quality in certain areas to reclaim resources and achieve higher scalability and save on bandwidth.  You can also opt to increase quality to support use-cases requiring precise visualizations, as in the Health Care industry.

*  Visual Quality
*  Use Hardware Encoding for Video Codec
*  Preferred Color Depth for Simple Graphics
*  Extra Color Compression
*  Target Frame Rate
*  Target Minimum Frame Rate
*  Allow Visually Lossless Compression


