---
layout: doc
description: Short description of this article
---
# Title

## Contributors

**Author:** [Albert Lee](URL)

## Audience

This document is intended for Citrix technical professionals, IT decision-makers, partners, and security consultants who want to explore the migration strategies from a customer-hosted on-premises environment to the Virtual Apps and Desktops Service on Citrix Cloud. The reader should have a basic understanding of Citrix products, security, and Citrix Cloud frameworks. For more information on Citrix Cloud and its services, see the official product documentation for [Citrix Cloud](/en-us/citrix-cloud.html).

## Objective of this document

The purpose of this document is to describe a set of procedures to migrate an on-premises Citrix Virtual Apps and Desktops environment to the Citrix Cloud Virtual Apps and Desktops Service (CVADS). It is intended for an audience planning to migrate the Control Layer components from a local data center, private, or public cloud to Citrix Cloud. The actual resources that users connect to (Virtual Delivery Agents) can remain on-premises and/or in a public cloud.

## Prerequisites and Assumptions

It is important that the reader has background knowledge on Citrix FlexCast Management Architecture (FMA), Citrix Cloud Architecture, design concepts and terminologies to execute the migration strategies. As a prerequisite, Citrix strongly recommends reviewing the [product documentation](/en-us/citrix-virtual-apps-desktops-service.html) and [reference architecture](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html) materials to gain adequate knowledge in preparation for the migration approaches.

## Planning and Design

At a high-level, the solution is based on a unified and standardized layer model.

*  User Layer – Defines the unique user groups, endpoints, and locations.
*  Access Layer – Defines how a user group gains access to the resources.
*  Resource Layer – Defines the applications, desktops, and data provided to each user group.
*  Control Layer – Defines the underlying infrastructure components required to securely access the resources.
*  Platform Layer – Defines the hardware components, a private, public, and hybrid cloud that will be used for the Citrix environment.
*  Operations Layer – This layer explains the procedures and tools that support the core product and solution.

As part of the planning strategy, assess the existing Citrix environment and prepare a new design with Citrix Cloud.
