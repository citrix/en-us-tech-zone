---
layout: doc
description: Personalized interface that provides access to all assigned SaaS apps, web apps, virtual Windows apps, virtual Linux apps, desktops, and data.
---
# Workspace app

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

As users access their resources -- SaaS apps, web apps, Windows apps, Linux apps, desktops, and data -- from numerous corporate and personal devices, they need a simple and unified experience that enables them to seamlessly gain access to everything. Organizations also must make it easy for users to onboard new devices while ensuring that centralizing security controls do not negatively impact the users’ experiences.

## Overview

Workspace app gives users a personalized interface enabling instant access to all their SaaS and web apps, files, virtual Windows apps, virtual Linux apps, desktops, mobile apps, Citrix Virtual Apps and Desktops, and data. It is an all-in-one interface powered by Citrix Workspace services and it provides the visual interface for Citrix Workspace.

Users either can opt for a locally installed Workspace app on their desktops and mobile devices or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the overall experience remains familiar and comfortable while automatically adjusting to the device’s form factor and touch-based capabilities. Workspace app incorporates multiple engines allowing users access to numerous types of app and data resources. Each engine optimizes the user experience for a particular resource. It provides the organization with insights into user activities and potential security threats.

![Citrix Workspace App Overview](/en-us/tech-zone/learn/media/tech-briefs_workspace-app_workspaceapp-overview.png)

To speed up the initial deployment, Workspace app only incorporates the engines associated with subscribed resources. If the organization does not subscribe to SaaS and web app resources, Workspace app does not deploy related engines.

## Embedded Browser Engine

The embedded browser engine keeps SaaS and web apps contained within the Workspace app instead of launching them on a locally installed and unmanaged browser. With the embedded browser, Workspace app is able to intercept user-selected hyperlinks in SaaS and web apps and request a risk analysis before approving, denying, or isolating access. In the embedded browser, enhanced security policies can

-  Restrict clipboard access: Disables cut, copy, and paste operations between the app and the endpoint’s clipboard.
-  Restrict printing: Disables ability to print from within the app.
-  Restrict navigation: Disables the next/back browser buttons.
-  Restrict downloads: Disables the user's ability to download from within the SaaS and web app or copy files from the virtual app and desktop to the endpoint.
-  Display watermark: Overlays a screen-based watermark showing the username and IP address of the endpoint.

## HDX Engine

The HDX engine establishes connections to virtual browsers, virtual apps and desktop sessions running on either Windows or Linux operating systems. With the HDX engine, Windows and Linux resources run remotely, while the display remains local, on the endpoint. To provide the best possible user experience, the HDX engine utilizes different virtual channels to adapt to changing network conditions and application requirements.. To overcome high-latency or high-packet loss networks, the HDX engine automatically implements optimized transport protocols and greater compression algorithms. Each algorithm is optimized for a certain type of display, such as video, images, or text. The HDX engine identifies these types of resources in an application and applies the most appropriate algorithm to that section of the screen.

## Networking Engine

The networking engine identifies whether or not an endpoint or an app on the endpoint requires network connectivity to a secured back end resource. The networking engine can automatically establish a full VPN tunnel for the entire endpoint device, or it can create an app-specific µ-VPN connection. A µ-VPN limits what back end resources an application and an endpoint device can access, thus protecting the back end infrastructure. In many instances, certain user activities benefit from unique network-based optimizations. If the user requests a file copy, Workspace app can automatically utilize multiple network connections simultaneously to complete the activity faster. If the user initiates a VoIP call, Workspace app improves its quality by duplicating the call across multiple network connections. It uses only the packets that arrive first.

![Citrix Workspace App Awareness](/en-us/tech-zone/learn/media/tech-briefs_workspace-app_application-awareness.png)

## Content Collaboration Engine

For many users, a workspace centers around data. The content collaboration engine allows users to integrate all data into the workspace, whether that data lives on-premises or in the cloud. The content collaboration engine allows administrators and users to create a set of connectors to corporate and user-specific data storage locations. Those can include OneDrive, Dropbox, on-premises network file shares and more. Users can maintain files in multiple repositories and allow Workspace app to consolidate them into a single, personalized library.

## Management Engine

The management engine keeps Workspace app current. This not only provides users with the latest capabilities, but also, includes extra security enhancements. Workspace app includes an auto-update service that routinely checks and automatically deploys updates based on customizable policies.

## Analytics Engine

The analytics engine reports on the user’s device, location and behavior, where cloud-based services identify any potential anomalies that might be the result of a stolen device, a hacked identity or a user who is preparing to leave the company. The information gathered by the analytics engine protects company assets by automatically implementing counter-measures.

## Conceptual Architecture

[![Citrix Workspace App Architecture](/en-us/tech-zone/learn/media/tech-briefs_workspace-app_conceptual-architecture.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-app_conceptual-architecture.png)

## Other Tech Zone content

[Learn -> Tech Insight -> Authentication - Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) - Utilize an on-premises Citrix Gateway as an identity provider for Citrix Workspace.

[Learn -> Tech Insight -> Authentication - TOTP](/en-us/tech-zone/learn/tech-insights/authentication-totp.html) - Time-based One-Time Password (TOTP) provides multifactor authentication to the user's Workspace experience.

[Learn -> Tech Insight -> Site Aggregation](/en-us/tech-zone/learn/tech-insights/site-aggregation.html) - Hybrid deployment that allows your on-premises Citrix Virtual Apps & Desktops environments to be part of Citrix Workspace.

[Learn -> Tech Insight -> Authentication - Okta](/en-us/tech-zone/learn/tech-insights/okta.html) - Utilize Okta as the user's primary identity for Citrix Workspace

[Learn -> Tech Insight -> Microapps](/en-us/tech-zone/learn/tech-insights/microapps.html) - Increase productivity by adding microapps to Citrix Workspace. Microapps allow users to view information and perform actions without launching the full application.

[Learn -> Tech Insight -> Microapps Custom Integrations](/en-us/tech-zone/learn/tech-insights/microapps-custom-integrations.html) - Create custom integrations with the microapp builder through the HTTP connector.

[Learn -> Tech Brief -> Virtual Assistant](/en-us/tech-zone/learn/tech-briefs/virtual-assistant.html) - The Citrix Assistant guides users to information and allows them to interact with back-end applications to complete simple requests.

[Learn -> Diagrams and Posters -> Citrix Workspace](/en-us/tech-zone/learn/diagrams-posters/workspace.html) - Conceptual architecture drawing for Citrix Workspace.

[Learn -> Tech Insight -> Authentication - Push](/en-us/tech-zone/learn/tech-insights/authentication-push.html) - Extend an on-premises TOTP deployment with Push authentication, eliminating the need for users to manually enter the temporary token.

[Learn -> Tech Brief -> Workspace Single Sign-On](/en-us/tech-zone/learn/tech-briefs/workspace-sso.html) - Learn how Citrix Workspace provides single sign-on capabilities to SaaS apps, web apps, mobile apps, Windows virtual apps and Windows virtual desktops. In addition, learn how Workspace single sign-on can support IdP chaining configurations.

[Learn -> Tech Brief -> Workspace Microapps](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html) - Streamline functionality from complex enterprise applications creating simple actions users can complete right within their feed.

[Learn -> Tech Brief -> Workspace Identity](/en-us/tech-zone/learn/tech-briefs/workspace-identity.html) - Learn how Citrix Workspace utilizes a secure primary identity to broker authorization to SaaS, web, mobile and virtual apps.

[Design -> Reference Architectures -> Microapps Service with Citrix Workspace](/en-us/tech-zone/design/reference-architectures/workspace-intelligence.html) - Learn about the Citrix Microapps Service, which brings intelligent features to Citrix Workspace. Component architecture, use cases, and integration strategies for implementing a comprehensive solution are covered.
