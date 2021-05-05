---
layout: doc
h3InToc: true
contributedBy: Rico Projer, Jason Delgado
specialThanksTo: Sasa Petrovic, Muhammad Dawood, Nick Czabaranek, Martin Zugec, Martin Latteier
description: To meet different user requirements, the Citrix HDX protocol allows for different graphics modes to be configured. Learn about the different HDX modes and how they are configured.
tz_title: HDX Graphics Overview
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: HDX Graphics Overview

## Introduction

To meet different user requirements, Citrix HDX protocol allows for different graphics modes to be configured. The purpose of this article is to outline the different HDX modes and how they are configured. It gives you a starting point from where you can configure your environment to best fit the needs of your users, your workload, and the current network conditions.

Important to note: This article is based on Citrix Virtual Apps and Desktops 1912 unless stated otherwise. For an overview based on 7.15, consult the blog article [HDX Graphics Encoder Configuration Overview – What Really Matters](https://www.citrix.com/blogs/2017/09/29/hdx-graphics-encoder-configuration-overview-what-really-matters/).

**DISCLAIMER**: Results may vary and it is highly recommended to run your own tests to see what works best for your use case(s).

## HDX Graphics Overview

Before diving into the specific graphics policies, let’s review how we categorize what you see on your HDX session screen and the underlying technologies that are used for presentation.

As we deliver graphic content for applications or desktops the HDX graphics encoding engine, Thinwire, dynamically categorizes display data into three types:

*  Text, Simple Images and Solid Colors
*  Static Image Content
*  Moving (or Fluid) Images

![HDX Graphics 1](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_000.png)

In the example above, text or simple images are highlighted in blue, static images in orange and moving (or fluid) images in green.

Within Citrix Virtual Apps and Desktops, Thinwire can take different approaches for display analysis, compression, and delivery: Citrix adapts the use of industry leading standards, H.264, and H.265 for efficient delivery of high-quality video content in its “Full-Screen” and “Selective” codec implementations.

*  Choosing to **Configure Thinwire to not use the Video Codec** or **Configure Thinwire to use the Video Codec for Actively Changing Regions** allows Thinwire to sense regions of transient content (fluid images or video) and encodes it based on set policy and capabilities detected on the endpoint. Thinwire encodes these “selected” (or transient) regions either as Adaptive JPEG or H.264 / H.265. Adaptive JPEG and “Selective” H.264 / H.265 are considered subfeatures as Thinwire is the core technology. The remaining, non-transient regions (encoded as JPEG and Run-Length Encoding (RLE)) are then combined to complete the in-session display.
*  Choosing to **Configure Thinwire to use the Codec For the Entire Screen** tells Thinwire to treat the entire screen as transient content, except for text (by default), and encodes display data using either H.264 or H.265 video codecs. Text is then overlaid onto the screen to provide a complete image. H.265 achieves higher compression over H.264 without compromising quality. However, H.265 is expensive in terms of processing and is only supported when used with [select GPUs](/en-us/citrix-workspace-app-for-windows/configure.html#h265-video-encoding) on the Virtual Delivery Agent (VDA). H.265 cannot be used when CPU encoding is used. Additionally, H.265 compatible hardware, in the form of GPU or purpose-built thin client, is required for decoding H.265 display data on the client endpoint. Review vendor documentation to determine H.265 supportability for your endpoint hardware.

As H.264 compatibility has a broader base, we focus on Full-Screen H.264 and Selective H.264 within this article unless otherwise noted.

Depending on the HDX mode configured, these categories are encoded by different means:

*  **Text and simple images** are almost always encoded in a lossless fashion using Run-Length Encoding (RLE). Starting with Version 7.17, a Citrix proprietary RLE flavor called MDRLE is used which allows for a better compression rate [CTX232041](https://support.citrix.com/article/CTX232041). Enabling the **Optimize for 3D graphics workload** will disable the lossless text detection and transfer the content with H.264 / H.265, rather than RLE. You can visualize this policy through our Visio diagram later in this article.
*  For **static images** with both Selective H.264 / H.265 and Adaptive JPEG, JPEG is used for encoding while the video codec H.264 / H.265 is used if Full-Screen H.264 / H.265 has been chosen as the graphics mode. If JPEG is used, the quality of it can be configured with the Visual Quality setting. Check the attached Visio chart later in this article for more details.
*  For **moving images** the video codec H.264 / H.265 is used when configuring Full-Screen H.264 / H.265 or Thinwire with Selective H.264. If Thinwire with Adaptive JPEG has been configured, JPEG is used with a quality that automatically adapts (hence the name) to conditions such as frame rate and available bandwidth.

To recap, Thinwire utilizes different technologies when configured as follows:

### Configure Thinwire to not use the Video Codec

*  Text: RLE
*  Simple Images and Solid Colors: RLE
*  Static images: JPEG
*  Moving Images: Adaptive JPEG

### Configure Thinwire to use the Video Codec for Actively Changing Regions

*  Text: RLE
*  Simple Images and Solid Colors: RLE
*  Static images: JPEG
*  Moving Images: H.264 / H.265

### Configure Thinwire to use the Codec For the Entire Screen

*  Text: RLE (or H.264 / H.265 if Optimize for 3D graphics workload has been enabled)
*  Simple Images and Solid Colors: H.264 / H.265
*  Static images: H.264 / H.265
*  Moving Images: H.264 / H.265

In the next section, we will cover the policies to achieve the behavior mentioned above.

## HDX Graphics Modes

The **Use Video Codec for Compression** policy is the central function to give your end-users an optimal experience by configuring display methods appropriate for different use cases. Below we map the technologies we outlined above with policy settings configurable within Citrix Studio.

*  **For Actively Changing Regions** = Thinwire with Selective H.264 / H.265
*  **Do Not Use Video Codec (default fallback method)** = Thinwire with Adaptive JPEG
*  **For the Entire Screen** = Thinwire Full-Screen H.264 / H.265
*  **Use When Preferred** (policy default) = **Thinwire with Selective H.264 / H.265** is used unless **Optimize for 3D Graphics Workloads** is also set, then **Thinwire Full-Screen H.264 / H.265** is used.

Capabilities of both the client endpoint and the Virtual Delivery Agent (VDA) are evaluated at session launch or session reconnect. If the client does not support H.264 / H.265, the display method is Thinwire with Adaptive JPEG regardless of the policy set on the VDA.

### Configure Thinwire to use the Video Codec for Actively Changing Regions

The **For Actively Changing Regions** graphics mode is our most balanced setting. As such, we recommend starting with this mode as you begin to baseline policies within your environment since it covers a wide user base (for example Office worker with occasional video playback).

At its core this mode is leveraging JPEG for still images, RLE for text, simple images, and solid color blocks, in addition to bitmap caching for areas of the screen that are determined by the VDA to be static. The VDA continuously analyzes the screen for regions of fluid movement, such as multimedia, and selectively uses **H.264 / H.265** to encode the fluid region.

As illustrated below, H.264 / H.265 is “Inactive” until regions of fluid movement are detected. The VDA then transitions to H.264 / H.265 to encode the selected region during fluid movement and returns to an “Inactive” state once the selected region no longer contains fluid content.

![HDX Graphics 2](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_001.png)

H.264 / H.265 provides a much more rich experience than adaptive JPEG at the expense of CPU to compress regions of fluid movement. Network bandwidth will generally be less with H.264 / H.265 compared to Adaptive JPEG for multimedia workload. It is highly recommended to run your own tests with your specific use case (check the Tools section below).

### Configure Thinwire to not use the Video Codec

The **Do Not Use Video Codec** offers maximum compatibility for client endpoints, including endpoints that do not support the decoding of H.264 / H.265 graphics.

In this graphics mode, Thinwire behaves similarly as when it is configured For Actively Changing Regions. The VDA analyzes the screen for regions of fluid movement. Rather than encoding with H.264 / H.265, however, Thinwire encodes moving images as Adaptive JPEG to deliver high compatibility or where H.264 / H.265 is not needed. The remaining regions are presented as JPEG for still images, and RLE for text and simple graphics to deliver quality imagery.

CPU processing for encoding moving images using Adaptive JPEG is typically lower than with Thinwire with H.264 for Full Screen or Actively Changing Regions. This mode is desired if server scalability is your priority. The trade-off is seen in terms of increased bandwidth and decreased moving image fidelity in WAN scenarios. This graphics mode is recommended for use case in which moving images are minimal, such as in a call center or point of sale system. In which case, the bandwidth utilization in this mode would be similar in comparison to Thinwire configured with Video Codec for Actively Changing Regions.

The **Do Not Use Video Codec** policy setting is the default fallback method for the other two graphics modes (Use the Video Codec for Actively Changing Regions or For the Entire Screen).

### Configure Thinwire to use the Codec For the Entire Screen

The **For the Entire Screen** graphics mode setting configures the VDA to encode all display data using H.264 / H.265, except for text. Text is encoded using RLE and is overlaid with the remainder of the screen. If **Optimize for 3D Graphics Workloads** is enabled the entire screen, including text, is encoded as H.264 / H.265.

Configuring Thinwire to use the Video Codec for the Entire Screen is designed for the heavy multimedia use case, where larger regions of the screen are in motion. Higher compression and quality is achieved at the expense of CPU and server scalability.

On its own, this mode provides a good user experience when heavy multimedia, 3-D modeling, or CAD drawing applications are in use. The CPU can quickly become a bottleneck, if undersized, resulting in poor performance and user experience under heavy multimedia conditions. Consider GPU offload capabilities to supplement this graphics mode while using these application types.

By default [YUV420](https://en.wikipedia.org/wiki/YUV) is used as color space. With Full-Screen H.264, you can choose between YUV420 or YUV444:

![HDX Graphics 3](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_003.png)

As you can see, YUV444 results in a better quality but it has a significant impact on the bandwidth requirements. The use of YUV444 will also disable hardware decoding on the client side (and therefore also H.265 where available).

You can enable YUV444 for Full-Screen H.264 with the following settings:

*  Visual Quality: Always lossless / Build to lossless
*  Allow visually lossless compression: Enabled

Check the Visio chart in this article for more details.

## HDX Graphics Configurations

As the **Use Video Codec for Compression** policy is a good starting point to baseline your configuration, additional policies can be set to further customize your visual policies to fit your different workloads. By customizing these supporting policy settings, you can opt to reduce quality in certain areas to reclaim resources and achieve higher scalability and save on bandwidth. You can also opt to increase quality to support use cases requiring precise visualizations, as in the healthcare industry. The chart below outlines these settings (click image to view full size PDF):

[![HDX Graphics 2](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_002.png)](/en-us/tech-zone/design/downloads/design-decisions_hdx-graphics_overview.pdf)

Furthermore, review our **Use Cases** Section below to discover how these additional policies (listed below) can reduce resource consumption, although at a slight reduction in quality (in some cases).

## CPU or GPU

By default, all processing for encoding graphics occurs within the CPU on the VDA. AMD, Intel, and NVIDIA graphics cards are currently supported to offload encoding to the GPU before being sent to your endpoint for decoding.

Offloading graphics encoding to a GPU will free up resources on the CPU for other tasks, resulting in a better overall experience for the end-user.

Due to varying GPU feature support, visit [Citrix Docs](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/graphics-policy-settings.html#use-hardware-encoding-for-video) to review feature supportability for each vendor GPU when enabling the **Use hardware encoding for video codec** policy setting.

## Use Cases

Once the settings details are known, the obvious next questions are: “What HDX mode is best for my use case?” or “Are there any configuration recommendations?” As usual, the answer is: It depends. In most cases, a "one size fits all" approach may not be the best approach but rather different settings for different use cases. So, the first questions you have to ask yourself are: What challenges and use cases do I have? Are there are any graphics intense workload, any multimedia requirements that I need to fulfill? How is the network connection of the users?

In most cases, configuring Thinwire to use video codec for actively changing regions is the best choice. Additionally, it is a good idea to explicitly configure the different settings to ensure the same settings apply even after an update of your environment. As you can see in the following link, the default HDX mode used has changed over time [HDX Graphics Encoder Configuration Overview – What Really Matters](https://www.citrix.com/blogs/2017/09/29/hdx-graphics-encoder-configuration-overview-what-really-matters/). Therefore, explicitly configure the HDX mode you would like to run. Generally, refrain from using "Use Video Codec for Compression: use when preferred" as this setting may have a different effect depending on the type of OS, Hardware, and VDA version you are running. Also avoid configuring any Citrix policies that are linked to legacy graphics mode. These settings are only supported on Windows Server 2008 R2 and Windows 7 and are left for compatibility reasons.

To give you an idea on how to start, we have created a few base line configurations for a few generic use cases below. Still, we recommend you run your own tests to ensure you have the best mode configured for your specific needs:

### Low Bandwidth

This use case describes a user connecting through a connection with serious bandwidth constraints. The following base line can be a good start:

*  Use Video Codec for Compression: Do not use video codec
*  Visual Quality: Low
*  Preferred color depth for simple graphics: 8 bit / 16 bit
*  Extra Color Compression: Enabled
*  Target Frame Rate: 15
*  Target minimum frame rate: 10
*  Moving Image Compression: Enabled
*  HDX Adaptive Transport: Preferred

As you can see, even with a low bandwidth connection we oftentimes do not set the color depth to 8 bit but keep it at 16 bit. While 8 bit can lower the bandwidth requirement substantially, it also comes with a significantly lowered user experience. Therefore, 8 bit is only recommended for the most extreme cases where access won't be possible otherwise.

### Call-Center / Point-of-Sales

This use case describes a call-center or point-of-sales workplace with no special multimedia requirements. The goal is to find a good mix between user-experience and user density:

*  Use Video Codec for Compression: Do not use video codec
*  Visual Quality: Medium
*  Preferred color depth for simple graphics: 24 bit
*  Extra Color Compression: Disabled
*  Target Frame Rate: 20
*  Target minimum frame rate: 10
*  Moving Image Compression: Enabled
*  HDX Adaptive Transport: Preferred

### Task Worker

In the task worker use case, a user has some multimedia requirements such as watching videos online in addition to using a basic set of office applications:

*  Use Video Codec for Compression: For actively changing regions
*  Use hardware encoding for video codec: Enabled (where available)
*  Visual Quality: Medium
*  Preferred color depth for simple graphics: 24 bit
*  Extra Color Compression: Disabled
*  Target Frame Rate: 30
*  Target minimum frame rate: 10
*  Moving Image Compression: Enabled
*  HDX Adaptive Transport: Preferred
*  Hardware Acceleration for graphics: Enabled (configured for Citrix Workspace app if available)

### 3D workload

For 3D workload such as CAD / CAE, the user experience is key, therefore the following settings are used:

*  Use Video Codec for Compression: For actively changing regions
*  Use hardware encoding for video codec: Enabled (where available)
*  Visual Quality: Build to lossless
*  Target Frame Rate: 30 (can be 60 if necessary)
*  Target minimum frame rate: 10
*  HDX Adaptive Transport: Preferred
*  Hardware Acceleration for graphics: Enabled (configured for Citrix Workspace app if available)
*  H265 Decoding for graphics: Enabled (configured for Citrix Workspace app if available)

## Endpoint Device Consideration

Our goal is to support delivery of your Citrix Virtual Apps and Desktops to any device, anywhere.

On the surface this sounds appealing. However, this does not necessarily mean all capabilities are present on all endpoints. For example, H.264 / H.265 decoding support may be missing, or supported only within specific boundaries such as the max monitor resolution or max number of monitors.

We recommend reviewing the vendor documentation for your chosen endpoint to determine overall supportability for H.264 / H.265.

### Thin Clients

Most thin clients are purpose built for targeted use cases, with specific software configurations that are optimized for their hardware platform. We encourage working with your vendor to evaluate a thin client test unit within your environment prior to purchase to ensure the endpoint meets your organizational needs.

It is highly recommended to visit our [Citrix Ready website](https://citrixready.citrix.com/) during the research phase of your project to determine if the hardware under consideration has passed evaluation and is compatible with the features you desire.

The Citrix Ready program classifies [thin clients](https://citrixready.citrix.com/info/endpoints.html) based on qualified use case capabilities:

*  **HDX Ready** – Supports Task Workers accessing basic office applications and light multimedia.
*  **HDX Premium** – Supports similar workloads as HDX Ready endpoints Additionally, HDX Premium endpoints provide support for Unified Communications such as Skype for Business.
*  **HDX 3D Pro** – Supports Power Users requiring high-end endpoint performance when accessing graphically intensive applications, such as CAD, Geographical Information System (GIS), and medical imaging related software H.264 / H.265 Codec support is required to pass qualification.

You can find the certification criteria for the features under each HDX level [here](https://citrixready.citrix.com/content/dam/ready/assets/thin-clients/thin-clients-features.pdf).

### Thick Clients

If you manage thick client endpoints in your environment, consider the following components when determining Codec Support (H.264 / H.265):

*  Operating System: Some Linux distributions require additional libraries to be installed.
*  Browser: Citrix Workspace app for HTML5
*  Citrix Receiver/ Workspace app version: Feature Support Matrix can be found [here](https://www.citrix.com/content/dam/citrix/en_us/documents/data-sheet/citrix-workspace-app-feature-matrix.pdf).
*  GPU Offload Capability
    *  Requires additional configuration for [Citrix Workspace app for Windows](/en-us/citrix-workspace-app-for-windows/configure.html#hardware-decoding) and [Citrix Workspace app for Linux](/en-us/citrix-workspace-app-for-linux/configure-xenapp.html#h264) to enable Hardware Acceleration for Graphics with compatible GPU.

## Tools

Are there any tools that help you configure your environment? Yes, there are quite a few to help you on your journey on finding the perfect configuration set for you:

### HDX Monitor

![HDX Graphics 4](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_004.png)

HDX Monitor helps you to check what settings are actually in effect in a particular session. The latest version can be found [here](https://cis.citrix.com/hdx/download/).

### Graphics status indicator

The build-in graphics status indicator can be enabled through Citrix policy by enabling the **Graphic status indicator** setting and will show you the current settings in the Citrix session:

![HDX Graphics 5](/en-us/tech-zone/design/media/design-decisions_hdx-graphics_005.png)

## Key Takeaways

The **Use Video Codec for Compression** policy lets you choose between the different HDX graphics modes illustrated above. Each mode has its benefits and trade-offs in terms of resource consumption, whether it be CPU or network utilization. Resource consumption, particularly CPU, affects server scalability.

Additional policies, such as Visual Quality, Target Framerate, and others can be customized to offset the resource consumption at the expense of minor visual quality, or increase quality where it is needed most. Customize these policies to fit the uses-cases within your own environment. Refer to the Visio Diagram to guide you through the process.

Endpoint selection is essential for compatibility with your selected graphics mode. The VDA configures Thinwire to not use the video codec, as a fallback method, for endpoints without H.264 support.

Leverage our built-in tools (HDX Monitor and the Graphics Status Indicator) to evaluate if your policy settings have met your desired outcome.

Thinwire **For Actively Changing Regions** is often times an appropriate starting point. However, knowing your use case(s) and configuring your environment accordingly is the best approach to deliver a rich experience to end-users.

## Sources

The goal of this article is to assist you with planning your own implementation. To make this task easier, we would like to provide you with source diagrams that you can adapt for your own needs: [source diagrams](https://citrix.sharefile.com/d-s7af08eb73f74aeb8).
