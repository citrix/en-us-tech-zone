---
layout: doc
h3InToc: true
contributedBy: Daniel Feller
description: A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization's business objectives help select the right approach as the different models impact the local IT team's management scope. Learn how Citrix Virtual Apps and Desktops management scope changes based on using a locally managed deployment, a cloud service deployment and a cloud managed deployment.
---
# Evaluating Delivery Models

## Overview

Designing a desktop virtualization solution is simply a matter of following a proven process and aligning technical decisions with organizational and user requirements. Without the standardized and proven process, architects tend to randomly jump from topic to topic, which leads to confusion and mistakes. The recommended approach focuses on working through five distinct layers:

![Layered Design Model](/en-us/tech-zone/design/media/design-decisions_delivery-model-comparison_layer-model.png)

Users are segmented into logical groups, based on personalization, security, mobility, resiliency, and activity requirements. The top three layers are designed for each user group independently. These layers define the users’ resources and how users access their resources. Upon completion of these three layers, the foundational layers (control and hardware) are designed for the entire solution.

This process guides the design thinking in that decisions made higher up impact lower level design decisions.

## Delivery Models

A Citrix Virtual Apps and Desktops solution can take on many delivery forms. The organization’s business objectives help select the right approach.
Even though the infrastructure remains the same for all delivery models, the local IT team’s management scope changes.

*  Citrix Virtual Apps and Desktops - This delivery model can be deployed entirely on-premises, entirely in the cloud or in a hybrid model. Even though components of the solution are using different hosting providers and hypervisors, the entire environment is a single solution using the same code and managed as a single entity. The local IT team continues to manage all aspects of the solution except for the hardware associated with the cloud-hosted resources. In many situations, organizations using an on-prem model extend the environment to the cloud when required to support contractors or seasonal workers.

![Citrix Virtual Apps and Desktops Architecture](/en-us/tech-zone/design/media/design-decisions_delivery-model-comparison_cvad-architecture.png)

*  Citrix Virtual Apps and Desktops Service - Breaks a typical deployment into multiple management domains. The access and control layer components are hosted and managed in the Citrix Cloud by Citrix while the resource layer components remains managed by the local IT team either as an on-premises, public cloud or hybrid cloud model. Citrix manages the hardware, sizing, and updates to the access and control components while the local IT team manages the resources. In addition, if the public cloud hosts the resources, the local IT team does not have to manage the resource hardware.

![Citrix Virtual Apps and Desktops Service Architecture](/en-us/tech-zone/design/media/design-decisions_delivery-model-comparison_cvads-architecture.png)

*  Citrix Virtual Apps and Desktops Standard for Azure - Not only manages the access and control layer components (similar to the cloud service offering), but it also manages the hardware layer.

![Citrix Virtual Apps and Desktops Standard for Azure Architecture](/en-us/tech-zone/design/media/design-decisions_delivery-model-comparison_cmd-architecture.png)

*  Citrix Service Provider - A hosted service offering where third party organizations design, build, and manage a Citrix Virtual Apps and Desktops environment on behalf of the organization. The Citrix Service Provider program is an outsourced IT management service that can handle solution deployments, updates, and management.

## Design Considerations

Selecting the correct delivery model is based on identifying how much of the solution does the local IT team want and need to manage. The local IT team can have complete ownership, no ownership or somewhere in the middle. The following diagram provides a high-level overview of how the different options compare.

![Delivery Model Comparison](/en-us/tech-zone/design/media/design-decisions_delivery-model-comparison_vdi-compare.png)
