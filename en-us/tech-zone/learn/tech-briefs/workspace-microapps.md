---
layout: doc
---
# Workspace Intelligence Features - Microapps

## Contributors

**Author:** [Ana Ruiz](https://twitter.com/mobileruiz)

**Special Thanks:** Dan Brinkmann

Organizations understand that when employees are forced to use complex, hard-to-use applications, they become frustrated and less productive. Employees spend 20% of their time searching for information while utilizing 4 or 5 key functions in about 80% of all business applications. [Citrix Workspace](/en-us/tech-zone/learn/tech-briefs/workspace-app.html/) allows users to access all their resources—SaaS apps, web apps, Windows Apps, Linux apps, desktops, and data through a single pane of glass. Citrix introduces intelligent features to workspace which extend these capabilities by adding machine learning-fueled microapps and workflows, universal search, and virtual assistance.

## Overview

Citrix is a leader in delivering a secure digital workspace. This next generation Citrix Workspace will deliver all the capabilities of the secure digital workspace, while also enhancing the employee experience by guiding work and providing time-saving workflows. The intelligent workspace gives users a single unified experience to all apps and data no matter where they reside while, incorporating machine-learning microapps and workflows to guide and enhance productivity, universal search to quickly surface data no matter where it’s stored, and virtual assistance to guide you to information and interact with applications. Analytics and automated intelligence makes it possible to customize the experience for individual users. Citrix Workspace is designed with security in mind—all data and keys are encrypted; therefore, Citrix admins do not have access to customer information.

To create this intelligent workspace, we are unbundling the applications to provide the end user with simple actions right within their feed. Actions are user-initiated activities taken within the microapps that provide inputs to the business applications. The actions a user performs within the microapp are designed to address specific common problems and use cases quickly and easily, adding to increased user productivity (for example: request PTO, submit a help desk ticket). We can also push event-driven microapps and notify users of something that requires their attention (for example: approval of an expense report, new course available for registration).

Universal Search provides users the ability to search for relevant information across all files and apps. A simple keyword search can be used to find app resources, SaaS apps, desktops, and files. This functionality enhances user productivity and efficiency as app and data sprawl is prevalent across all organizations.

Virtual assistance allows users to remain productive and take quick actions. Users can interact with the “Virtual Assistance” and ask questions such as “What is Bob Smith’s phone number” or “What absences are pending my approval”. The system then parses these requests and responds because it is integrated with multiple systems on the back-end. Users are able to interact with the virtual assistance through either the workspace app or directly from Microsoft Teams. This feature allows employees to work efficiently, stay organized, and deliver only the specific information they’re looking for.

## Microapps and workflows

A microapp is a single use case made available to users to streamline functionality from complex enterprise applications. Microapps utilize APIs available within SaaS, web, or home-grown applications allowing users to see content without needing the full launch of the application or the need to switch context. Before utilizing microapps, users would have to launch an application, navigate to the action they need to perform, and then perform the action. Microapps streamline routine tasks for frequently performed actions and provide users the ability to perform actions within their Workspace app without having to launch the native application. The workspace app feed, aggregates relevant notifications, tasks, and insights; giving the end user a dynamic productivity tool. The feed is intelligently populated by utilizing machine learning and AI algorithms. Microapps are configured within Citrix Cloud; giving administrators a powerful tool to create more productive workflows, without the need for additional infrastructure. Whether pushed to a user or initiated by a user, microapps are short cuts that simplify and streamline key tasks that would otherwise require opening full enterprise applications.

Citrix’s “Extract, Transform, and Notify” (ETN) methodology enables focused integrations by targeting active data that is actionable. Those actionable events are put into a database and then events are created based on what is changing within that data. Citrix is securely integrating with back-end systems and building on top of their existing APIs to build a solution that is scalable. With out-of-the-box templates, administrators with API account permissions are able to build microapp solutions targeted for their needs. Administrators also have the tools necessary to build custom microapps.

![Workspace Microapps](/en-us/tech-zone/learn/media/tech-briefs_workspace-microapps_ent.png)

Below are some sample applications (Systems of Record) which have out-of-the-box microapp integrations:

-  Atlassian Jira
-  Google G-Suite Directory
-  Google G-Suite Calendar
-  Microsoft Dynamics CRM
-  Microsoft Power BI
-  Salesforce
-  SAP Ariba
-  SAP Concur
-  ServiceNow
-  Tableau
-  Workday
-  Zendesk

Over 100 out-of-the-box microapps are available.

Citrix’s Identity Mapping allows the system to understand who the user is by looking at EMMs, IdPs, and directories while mapping the user identity across all solutions to see what is relevant for that end user. It provides unified user data lookup and single-sign-on for the end user. Also, permissions to access the microapp are based on membership in groups from the systems of record. Therefore, not only can we assign microapps based on AD groups, but also based on target groups that are part of the application.

### Microapp architecture and process flow

Citrix Intelligent Workspace is comprised of different μ-services and interacts with existing Citrix Workspace services:

-  The Systems of Record (SoR) are the applications the intelligent workspace interacts with to create microapps. These applications can be SaaS applications, legacy applications, homegrown applications and can be hosted on-prem or cloud. There are connectors with out-of-the-box templates for several applications (see list above), however integration with other applications can be configured through the microapp page builder. The microapp page builder connects to legacy, on-premises, and SaaS systems by creating streamlined user workflows via microapp actions. The intelligent workspace supports REST API, JSON, OData-JSON, and XML. The system can write back to the SoR using OAuth2 or a service account.
-  The microapp service is a single-tenant service responsible for creating the microapps and sends raw events to Citrix Analytics for processing that it pulls from the SoR. The microapp server is periodically pulling active data from the Systems of Record.
-  The active data cache µ-service is single-tenant and stores all configuration information and microapp data. It utilizes a per-tenant database encryption key and per-tenant database credentials.
-  The credential wallet µ-service, stores encrypted service credentials for the system of records and user OAuth2 tokens.
-  The data integration provider µ-service interacts with the SoR to decrypt end-user credentials and write back actions to the SoR under the identity of the end-user. The write-back actions utilize a user’s actual account to ensure all actions performed are compliant with data policies of the system we’re interacting with.
-  The notification service µ-service processes the notifications created. It either stores them in a database to be later served in an IWS notification feed, or send the notifications out immediately as a push notification to the end user.
-  Finally, the Analytics Service, processes the raw events and creates targeted scored notifications and sends them to the notification service.

The data flow of the Intelligent workspace can be found below:

[![Citrix Workspace Microapps Architecture](/en-us/tech-zone/learn/media/tech-briefs_workspace-microapps_processflow.png)](/en-us/tech-zone/learn/media/tech-briefs_workspace-microapps_processflow.png)

## Summary

Citrix Workspace customers are able to build microapps to increase end-user productivity. It eliminates the need for users to parse through complex enterprise apps to complete a single task. Citrix Workspace is designed with security and end-user experience in mind. It encrypts all the data and keys; therefore, admins do not have access to customer information. The system only stores the data it needs to create events. By utilizing public APIs, admins completely control which microapps are made available. No audit trail is lost because everything is written back in the context of the user. Microapps organize, automate, and guide your employees through their workday; maximizing productivity and boosting the overall user experience.

**Note: Microapps is currently not generally available feature, information above is subject to change.**
