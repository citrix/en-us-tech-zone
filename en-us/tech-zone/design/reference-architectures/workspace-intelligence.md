---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: Dan Feller, Allen Furmanski, Radovan Hrabcak
description: Learn about the microapps platform service, which brings intelligent features to Citrix Workspace. Component architecture, use cases, and integration strategies for implementing a comprehensive solution are covered.
---
# Reference Architecture on Microapps service with Citrix Workspace

## Executive Summary

Citrix introduced the Microapps Service to add intelligence to the Citrix Workspace Platform. The Microapps Service allows administrators to build microapps and intelligent features to the workspace that eliminates the need for users to parse through complex enterprise apps to complete simple tasks. The intelligent features of Citrix Workspace significantly reduce the time spent on day-to-day business tasks by simplifying the access workflow. Microapps organize, automate, and guide employees through their workday, maximizing productivity and boosting the overall user experience and engagement. Let’s review the component architectures and sample use cases to see how Citrix Workspace Intelligence adds value to organizations.

## An Introduction to the Microapps service

A digital workspace becomes intelligent when it moves beyond organizing apps but also guiding and eventually automating the work. The intelligent features of a digital workspace enable IT to deliver business applications through a simplified and unified interface with single sign-on. Users can access all the necessary resources within the digital workspace to complete their prioritized as well as repetitive tasks with simple mouse clicks and actions.

The Citrix Microapps service is a new cloud-based service introduced to add intelligence to the Citrix Workspace platform. The service allows IT to extract the relevant information and actions from the business applications in a frequent interval. The Microapps service uses Analytics insights to prioritize and present the events and notifications to the users in a simple way through Citrix Workspace. Save users time by reducing context switching and eliminating the need to learn how to use various applications for one-off interactions. This approach improves the user experience because they can focus on their primary responsibilities. Refer to the Citrix documentation on Microapps [Tech Insight](/en-us/tech-zone/learn/tech-insights/microapps.html) and [Tech Brief](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html) for more detailed information.

### New features of Citrix Workspace Intelligence

#### Microapp

A microapp is a small, task-specific application that delivers highly targeted functionality. These apps allow users to accomplish single-purpose activities in a simple and quick manner. Microapps deliver actionable forms and notifications. Microapps can be transactional and write back to source systems. For example, a microapp needed for the “Approve PTO” use case and another microapp for “Submit PTO”. Similarly, for each use case, administrators can create or use built-in out-of-the-box microapps as per the organization's requirement.

The microapps utilize APIs available within SaaS, web, or home-grown applications allowing users to see the content and interact with specific actions without needing the full launch of the application. Prior to microapps, users would have to launch an application, navigate to the page, and then perform the action.

*  **Event-driven microapp experience** - Automated, event-driven microapps notify users when something requires their attention like a new expense report that must be approved, or a new course available for registration. Also, it shows up as cards in the Workspace activity feed as **Notifications**.

[![Citrix-Microapps-Image-1](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_001.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_001.PNG)

*  **User-Initiated action microapp experience** - Present the workflows in an easy way to do things like request PTO or submit a ticket to your help desk or creating an event is available as actions in Citrix Workspace.

[![Citrix-Microapps-Image-2](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_002.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_002.PNG)

#### Intelligent Feed

Citrix Workspace includes an **Intelligent Smart Feed** that allows end-users to interact with microapps to perform actions or gain insights from legacy, on-premises, and SaaS applications. Admins can configure or build microapps using the **Microapps service** available in the Citrix Cloud console.

The **intelligent feed** can identify changes in the data like new, updated, deleted, or matching records and trigger notification based on the business rules set as part of the notification event setup. So, it adds value on a top of just displaying by identifying those changes and notifying users of them. End users engage with content directly within the feed without the need to launch the full app. The feed data is getting populated by utilizing machine learning and AI algorithms.

[![Citrix-Microapps-Image-3](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_003.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_003.PNG)

#### System of Record (SoR)

All the applications like SaaS, legacy, and homegrown applications from on-premises or cloud that can be connected via the Microapps service to create microapps is called **Systems of Record (SoR)**. There are Integrations available as out-of-the-box templates to integrate several applications with the Microapps service. However, custom applications also can be integrated through the microapp page builder. The Microapp page builder helps to create streamlined user workflows via microapp actions.

Workflows are presented to the end-user via the **feed** by pulling content, actions, and insights from connected applications (Systems of Record), allowing end-users to interact with systems of records directly within Citrix Workspace. End-users can customize the actions pane to reduce complex workflows.

#### Actions

Actions are user-initiated tasks to an application, and the tasks can be customized and provisioned based on end-user groups and functions. The microapp helps to reduce complex workflows into streamlined actions.

#### Insights

Insights are event-driven microapps that present the data and reports from an application. The additional feed data is populated and viewed as a **card** on the right side of the workspace, and this can be customized based on end-user groups and their functions. The microapp builder helps to create a sophisticated page that fits within the workspace.

#### Notifications

Notifications are event-driven microapps that automatically notify users when something requires their attention, for example, as a card in the Workspace activity feed. Such microapps include ‘New Expense Report for Approval’ and ‘New Course Available for Registration.’

#### Card

The card is an individual item that lets you review the data, report, and documents from the different System of Records (SoR).

#### Search

Universal search is a single unified experience to quickly find content and data within the workspace no matter where it’s stored.

#### Virtual Assistance

Users interact with virtual-assistance within Citrix Workspace and ask questions such as “What is the Support Team phone number?” or “What absences are pending my approval?”. The system then parses these requests with the integrated back end systems and response. Users can directly interact with virtual assistance through the workspace app or directly from Microsoft Teams.

#### Microapp Builder

The microapp builder allows the creation of microapps quickly and easily. Administrators can add new integrations and connect to the data sources and create events based on data changes.

The microapp page builder delivered within the Citrix Cloud console connects to legacy, on-premises, and SaaS applications to easily create streamlined user workflows with low-code tooling. It allows creating your microapp actions to leverage custom applications, including those residing on-premises, with the generic web services connector. Administrators can configure the content within microapps via the microapp page builder.

[![Citrix-Microapps-Image-4](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_004.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_004.PNG)

## Component Architecture of Citrix Workspace with Intelligence Features

[![Citrix-Microapps-Image-5](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_005.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_005.PNG)

1.  **Endpoints** - An endpoint is an application interface with plug-ins that an end-user can use to interact with Citrix Workspace and its new features, including the intelligent feed and microapp actions. The Citrix Workspace app itself is the primary endpoint, whether the native desktop app, the web client, or the native mobile app. The microapps allows integrating with other endpoints like Microsoft Teams.

2.  **Systems of Record (SoR)** - The Systems of Record (SoR) are the applications that Citrix Workspace interacts with to create microapps. These could be SaaS applications, legacy applications, homegrown applications and could be hosted on-prem or cloud.

3.  **On-Premises Cloud Connector** - The on-premises Cloud Connector is a component that serves as a channel for communication between Citrix Cloud services and customer on-premises locations. It enables us to interact with home-grown applications without requiring any complex networking or infrastructure configuration.

4.  **Microapps service** - The Microapps service is a single-tenant service responsible for configuring and exposing microapps. It periodically polls systems of record to update its local data cache. Its event engine then generates raw events based on its data cache, and it sends to Citrix Analytics to sort them for relevancy.

5.  **Data Integration Provider Service** - The Data Integration Provider interacts with the Systems of Record (SoR) to decrypt end-user credentials and write back actions to the SoR under the identity of the end-user. The write-back operations utilize a user’s actual account wherever possible to ensure all activities performed are compliant with the data policies of the system.

6.  **Credential Wallet Service** - The credential wallet Key Management Service (KMS), stores encrypted service credentials for the system of records and user OAuth2 tokens.

7.  **Citrix Analytics Service** - The Citrix Analytics Service processes the raw events and creates targeted scored notifications and sends them to the notification service.

8.  **Notification Service** - The notification service processes the notifications created and either store them in a database to be later served as notification cards or sends them out immediately as a push notification to the end-user.

Refer to the [Tech Zone document](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html#microapps-and-workflows) for more detailed microapp architecture and process flow.

## Which applications are supported to integrate with Citrix Workspace using Microapps service?

Integrations extend Citrix Workspace, and microapps provide users with a cutting-edge experience and user interface. Deliver relevant, actionable notifications, combined with intuitive microapp workflows, to make essential use-cases of business systems and applications directly accessible from a user’s Workspace. Review and understand the requirements from the [Getting Started Guide](/en-us/citrix-microapps/getting-started.html) to onboard your applications.

There are countless applications that can be integrated using:

*  Built-in Template Integration
*  Custom Application HTTP Integration

### Built-in Template Integration

Citrix Workspace microapps has a large selection of out-of-the-box integrations to use. Administrators can set up a template integration and use the out-of-the-box microapps or build their own. There are over 100 out-of-the-box microapps available to integrate all kinds of Systems of Record. Refer to the [Citrix documentation](/en-us/citrix-microapps/set-up-template-integrations.html) for the complete list of template integration and out-of-box microapps.

### Custom Application HTTP Integration

Citrix Workspace Microapps service allows integrating any custom application that is not available as built-in-template integrations. This method enables the organization to integrate any compatible home-grown applications and legacy applications using an API-wrapper to communicate with the Microapps service. The generic applications that support the following are eligible for custom application integration:

*  REST API support
*  JSON, OData-JSON, XML
*  Write back in user context using OAuth2 or using a service account

Refer to the [Citrix documentation](/en-us/citrix-microapps/build-a-custom-application-integration.html) to plan, create, and configure the custom application integration.

## How to integrate a System of Record (business application) with Citrix Workspace using the Microapps service?

The following are the simple steps for a System of Record integration with Citrix Workspace:

*  Selecting a business app, identifying use-cases, and determining the APIs that are required.
*  Access the Citrix Cloud Portal and Microapps tile to start integrating your application by adding the base URL, setting up authentication, and configuring the integration.
*  Create or use a built-in microapp to add notifications & pages to it.
*  Add users and group subscriptions to each microapp to enable the notifications and actions as an intelligent feed when users log in to Citrix Workspace.

Review the [Technical Security guide](/en-us/citrix-microapps/technical-security-overview.html) for the security overview, credential handling, data flow, and deployment consideration for the integration.

## Sample Use-Cases

### System of Record #1: ServiceNow

ServiceNow integration enables the organization to allow the users to monitor an incident and add change and problem requests within Citrix Workspace. Refer to the [Citrix documentation](/en-us/citrix-microapps/set-up-template-integrations/integrate-servicenow.html#review-prerequisites) to review the prerequisites, requirements and to learn how to integrate ServiceNow using Microapps.

After the creation of the integration, out-of-the-box microapps are available on the microapps page that is associated with the ServiceNow Integration. Each microapp can be either an actionable event, an informational event, or a user-initiated action. The microapps that are of type event contain events that can be triggered to generate Feed Cards. If the microapp is an actionable type, the user has to input information that performs Writeback Actions to the back-end System of Record.

#### Conceptual Architecture – ServiceNow Integration

[![Citrix-Microapps-Image-6](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_006.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_006.PNG)

#### Data Synchronization

The Microapps service needs to synchronize all the required tables and data from ServiceNow to its local cache. For the data synchronization from ServiceNow, the Microapps service retrieves encrypted service account credentials for the system of record from the Credential Wallet and requests sync from the Data Integration Provider. The service account must have read permission to all the required data from the ServiceNow System of Record.

The Data Integration Provider decrypts the service account credentials from the Credential Wallet (Key Management Service). With the service account details, the Data Integration Provider connects the ServiceNow application and retrieves the updated data. There are two types of synchronization configured with the Microapps Service - Full and Incremental Synchronization. The incremental sync is possible as soon as service supports that with time-related information in API payload. The interval for synchronization is set based on the organization's requirements in order not to break SaaS service API limits as well as to avoid sync jobs that are getting queued in the microapps platform.

The Data Integration Provider streams ServiceNow table schema and data to the Microapps service for storage and processing. During the full or incremental synchronization, the microapps service compares the local cache with previous and current data and processes them to send them to users as notifications and actions.

#### Microapps - Workflows

Built-in template integration for ServiceNow provides several out-of-the-box microapps to accomplish the required workflow for the users. Below are the sample workflows that are associated with the ServiceNow System of Record.

#### Workflow: Event Notifications

| **Microapps**               | **Event Type**                |
| --------------------------- | ----------------------------- |
| Incident Updates            | Notification - Informational |
| Change Request Updates     | Notification – Informational |
| Incident Assignments       | Notification – Informational |
| Change Request Assignments | Notification – Informational |

The following diagram depicts a sample workflow for an event notification:

[![Citrix-Microapps-Image-7](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_007.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_007.PNG)

1.  The Microapps service checks for updates with ServiceNow on the synchronization schedule.
2.  ServiceNow has updated records for Incident, Change, and Problem requests.
3.  Microapps service retrieves the updated data and stores in its local cache.
4.  Microapps Service compares the previous sync data with the current data to identify changes in the data and based on business rules set as part of the event configuration. It triggers notification events as soon as the rules match and send the updated events to Citrix Analytics Service for relevancy scoring.
5.  Citrix Analytics Service (CAS) performs Active Directory OID lookup for the notifications and creates targeted scored notifications.
6.  CAS sends the targeted notification to the User Notification Service for processing and storage.
7.  User Notification Service pushes the updated notifications to user endpoints.
8.  Endpoints receive notifications for the respective users on their Citrix Workspace user interface.

#### Workflow: Approvals

| **Microapps**           | **Event Type** |
| ----------------------- | -------------- |
| Approve Change Request | Action – Event |
| Reject Change Request  | Action – Event |

The following diagram depicts a sample workflow for an Action - Approval Event:

[![Citrix-Microapps-Image-8](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_008.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_008.PNG)

1.  The user invokes an approval action from the change request approval microapp
2.  The Microapps service receives the user action and retrieves an encrypted OAuth token from KMS.
3.  The Key Management Service sends an action to the Data Integration Provider with the OAuth token.
4.  The Data Integration Provider decrypts the end user’s OAuth token via the Credential Wallet for the action, which is invoked by the user for the ServiceNow Change request.
5.  The Data Integration Provider does the pre-writeback validation. Upon validation, it writes user actions to the system of record under the identity of the end-user.
6.  The Data Integration Provider reads back changed data and updates the Microapps service.
7.  The Microapps service sends the updated events to the Citrix Analytics Service.
8.  The Citrix Analytics Service creates target scored notifications.
9.  CAS sends the targeted notification to the User Notification Service.
10.  The User Notification Service pushes the updated notifications to user endpoints.
11.  Endpoints receive feedback on Successful actions

#### Workflow: User-Initiated action with Text input submission

| **Microapps**          | **Event Type**              |
| ---------------------- | --------------------------- |
| Submit Incident        | Action + Text Input Submit |
| Submit Change Request | Action + Text Input Submit |

The following diagram depicts a sample workflow for a User-Initiated Action - Submit Event:

[![Citrix-Microapps-Image-9](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_009.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_009.PNG)

1.  A user submits an incident request for software access. Users provide title and description as text inputs for the incident ticket and submit it through Workspace UI.
2.  The Microapps service receives the action with the incident details and retrieves encrypted OAuth token from KMS.
3.  The Key Management Service sends the submissions and user’s action to the Data Integration Provider with OAuth token.
4.  The Data Integration Provider decrypts the end user’s OAuth token via the Credential Wallet for the action, which is invoked by the user for the ServiceNow Incident request.
5.  The Data Integration Provider writes the incident request to the system of record under the identity of the end-user.
6.  The Data Integration Provider reads back changed data and incident details to the Microapps service.
7.  The Microapps service sends the updated raw events to Citrix Analytics Service.
8.  The Citrix Analytics Service performs the user and group extraction and creates targeted scored notifications.
9.  CAS sends the targeted notification to the User Notification Service.
10.  The User Notification Service pushes the updated notifications to user endpoints.
11.  Endpoints receive feedback on Successful submission of Incident requests.

### System of Record #2: SAP Concur

SAP Concur is an enterprise business travel and expense management application used by the employees and their manager. SAP Concur Integration enables the organization users to submit expense and travel reports and receive notifications about the status of requests within Citrix Workspace. Refer to the [Citrix documentation](/en-us/citrix-microapps/set-up-template-integrations/integrate-concur.html) to review the prerequisites, requirements and to learn how to integrate SAP Concur using Microapps.

The SAP Concur integrates with the Microapps service using a built-in template; hence, the admin can configure the out-of-the-box microapps which are available on the microapps page. Each microapp can be either an actionable event, an informational event, or a user-initiated action. For SAP Concur, the microapp can be an actionable type; the user has to input information that performs writeback actions to the backend System of Record.

#### Conceptual Architecture – SAP Concur Integration

[![Citrix-Microapps-Image-10](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_010.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_010.PNG)

#### Data Synchronization

The Microapps service needs to synchronize all the required tables and data from SAP Concur to its local Cache.  For the data synchronization, the Microapps service retrieves the encrypted service account credentials for the system of record (SAP Concur) from the Credential Wallet and requests sync from the Data Integration Provider. The service account configured must have read permission to all the necessary data on the SAP Concur application system.

The Data Integration Provider decrypts the service account credentials from the Credential Wallet (Key Management Service). With the service account details, the Data Integration Provider connects the Concur application and synchronizes the updated data. Based on the synchronization configuration, the microapps service continuously synchronizes the data either by full or incremental.

The Data Integration Provider streams SAP Concur schema and data to the microapps service for storage and processing. During the full or incremental synchronization, the microapps service compares the local cache with previous and current data and processes them to send them to users as notifications and actions.

#### Microapps - Workflows

After creating the integration with SAP Concur, the microapps built-in template enables several out-of-the-box microapps to accomplish the required workflow for the users and managers. Below are the sample workflows that are associated with SAP Concur.

#### Workflow: Approvals

| **Microapps**                  | **Event Type** |
| ------------------------------ | -------------- |
| Expense Submissions Reminders | Action – Event |
| Expense Report Approvals      | Action – Event |
| Report Updates                 | Action – Event |

The following diagram describes a sample workflow for an Action event - Approvals:

[![Citrix-Microapps-Image-11](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_011.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_011.PNG)

1.  The Microapps service checks for the updates with the SAP Concur on the synchronization schedule for full or incremental data sync.
2.  SAP Concur has few updated records for expense reports, and those are pending for the manager's approval.
3.  The Microapps service retrieves the updated data and stores it in its local cache.
4.  The Microapps service compares the previous sync data with the current data and sends the updated events to the Citrix Analytics Service for relevancy scoring.
5.  The Citrix Analytics Service(CAS) performs Active Directory OID lookup for the notifications and creates targeted scored notifications.
6.  CAS sends the targeted notification to the User Notification Service for processing and storage.
7.  The User Notification Service pushes the updated notifications to user endpoints.
8.  Endpoints receive notifications for respective users on their Citrix Workspace user interface. For Managers, it is approval notification and for the employee’s the status of the expense report.
9.  The manager invokes the action for expense report approval or rejection. If it is approved, there are no comments needed, and if it is a rejection of the expense report, the card page appears to enter the feedback.
10.  The Microapps service receives action with the incident details and retrieves encrypted OAuth token from KMS.
11.  The Key Management Service sends the submissions and user’s action to the Data Integration Provider with OAuth token.
12.  The Data Integration Provider decrypts end user’s (Manager) OAuth token via the KMS for the SAP Concur expense report approval action.
13.  The Data Integration Provider writes the approval/rejection information to the system of record under the identity of the end-user.
14.  The Data Integration Provider reads back the changed data of the expense report and sends those details to the Microapps service. The process follows, and finally, the managers receive the notification on successful action, and the employee receives the notification with the status of the report.

#### Workflow: Submit Expense Reports

| **Microapps**  | **Event Type** |
| -------------- | -------------- |
| Create Expense | Action: Input  |

The following diagram describes a sample workflow for an Action Input – Expense Report Submit:

[![Citrix-Microapps-Image-12](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_012.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_012.PNG)

1.  A user submits an expense report with text inputs for all the required fields.
2.  The Microapps service receives an action with the text inputs.
3.  The Microapps service retrieves encrypted OAuth token from Key Management Service.
4.  The Key Management Service sends the report details and user’s action to the Data Integration Provider with OAuth token.
5.  The Data Integration Provider decrypts the end user’s OAuth token via the Credential Wallet(KMS) for the expense report action.
6.  Data Integration Provider writes the expense report to the system of record under the identity of the end-user.
7.  Data Integration Provider reads back changed data and expense report details to the Microapps service.
8.  Microapps service sends the updated raw events to Citrix Analytics Service for processing.
9.  Citrix Analytics Service performs the user and group extraction and creates targeted scored notifications.
10.  CAS sends the targeted notification to the User Notification Service.
11.  User Notification Service pushes the updated notifications to user endpoints.
12.  Endpoints receive feedback on the successful submission of the expense report.

## How does the Microapps service and Citrix Workspace help organizations in increasing user engagement and productivity?

Let’s compare the traditional access approach vs. Microapps Integration to understand how Microapps service delivers value to the organization.

### Traditional Access

The below diagram depicts the traditional access method for an employee with illustration tasks that the employee needs to perform every day.

[![Citrix-Microapps-Image-13](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_013.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_013.PNG)

*  Employees access Windows, Linux, mobile, SaaS, Web, and Virtual applications with various points of authentication every workday. In a day, switching between these applications distracts, and for each app requires time-consuming authentication.
  
*  Users find it hard to remember their respective credentials to access each one of them.

*  Searching for the application details to access them is time-consuming, and it impacts productivity.
  
*  Accessing the extensive application and navigating multiple modules to complete simple mouse clicks like approving time off is frustrating.
  
*  It is prompting for authentication several times after the session expires, which is frustrating and time-consuming when the users need to perform simple tasks like approving the expense reports and PTO.

### Microapps Access

The below diagram depicts the new access method with Microapps integration for an employee with illustration tasks that he needs to perform every day.

[![Citrix-Microapps-Image-14](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_014.PNG)](/en-us/tech-zone/design/media/reference-architectures_workspace-intelligence_014.PNG)

*  With the help of Microapps Service and System of Record integration, employees can access all business applications through Citrix Workspace. It allows accessing all the business applications in a single user interface, and it eliminates the login time of each application.
*  Microapps Service allows the organization to integrate the applications using OAuth; hence, it enables single-sign-on to all the applications. Users don’t have to enter the credentials for each application; therefore, it saves time and allows continuous user engagement.
*  Citrix Workspace intelligent features enable employees to accomplish their tasks with simple mouse clicks, which saves time on searching and navigating the extensive applications.
*  Citrix Workspace and Microapps service enable the employees to access all the resources from the Workspace UI, and it saves context switching time, thus increasing user productivity.

## Summary

The Microapps service with Citrix Workspace is the solution for a better way to work. It enables modern digital workspace that unifies all resources for users to do their best work and help their organizations gain an edge. Citrix Workspace and Microapps Service allow customers to build microapps to increase end-user productivity. By utilizing public APIs, admins completely control which microapps are made available to users. Microapps organize, automate, and guide your employees through their workday, maximizing productivity and boosting the overall user experience.

## Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s0f439f6a6014e2ca).

## References

[Getting Started Guide](/en-us/citrix-microapps/getting-started.html)

[Tech Insight on Microapps](/en-us/tech-zone/learn/tech-insights/microapps.html)

[Tech Brief on Microapps](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html)

[Microapps Workflow](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html#microapps-and-workflows)

[Template Integration](/en-us/citrix-microapps/set-up-template-integrations.html)

[Custom Application HTTP Integration](/en-us/citrix-microapps/build-a-custom-application-integration.html)

[Technical Security Guide](/en-us/citrix-microapps/technical-security-overview.html)
