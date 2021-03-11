---
layout: doc
h3InToc: true
contributedBy: Ana Ruiz
description: The Citrix Assistant guides users to information and allows them to interact with back-end applications to complete simple requests.
---
# Workspace Intelligence Features - Citrix Assistant

Consumers have rapidly adopted voice assistants to do simple searches, requests, or questions. In 2018, there were over 2.5 billion assistants in use—this number is estimated to grow over 3 times in the next 4 years. It is estimated that by 2023, there will be over 8 billion digital voice assistants. Whether it’s on their smart phones, smart tvs, or speakers, consumers expect the ability to interact and search with virtual assistants.

## Overview

Citrix delivers secure digital workspaces that provide intelligent capabilities to enhance the employee experience. Citrix Workspace provides users a unified experience to apps and data no matter where they reside. Workspace intelligent features give users a single unified experience to all apps and data no matter where they reside while, incorporating machine-learning [microapps](/en-us/tech-zone/learn/tech-briefs/workspace-microapps.html/) and workflows to guide and enhance productivity, universal search to quickly surface data no matter where it’s stored, and virtual assistance to guide you to information and interact with applications.

The Citrix Assistant guides users to information and allows them to interact with back-end applications to complete simple requests. It allows users to have natural language interaction and do queries for the data on the systems of records within Citrix Workspace. Users are able to ask questions such as “What are my pending expense report approvals?” or “Show all open tickets.” It then parses through the system and is able to respond because it is integrated with multiple systems on the back-end. The user does not need to be aware which application on the back-end is used to get the information. The users are also able to see what type of requests they can ask through the help menu.  

The list of skills and applications that are supported today are the following:

-  Accounts (Salesforce, MS Dynamics)
-  Appointments (Salesforce, MS Dynamics)
-  Contracts (Salesforce)
-  Company Directory (Workday)
-  Expense Reports (Concur)
-  Leads (Salesforce, MS Dynamics)
-  Learning Courses (SAP SuccessFactors)
-  Opportunities (Salesforce, MS Dynamics)
-  Time off (Workday)
-  Purchase Orders (SAP Ariba, Workday)
-  Tasks (Salesforce, MS Dynamics)
-  Tickets (ServiceNove, Jira, Zendesk)

## Citrix Assistant architecture and process flow

Citrix Assistant is comprised of different μ-services. Citrix maintains all components and hosts them within the Citrix Cloud control plane. The following multitenant µ-services are used by the Citrix assistant.

-  Endpoints: allow users to interact with the Citrix assistant. Currently users can interact with the Citrix assistant either through Citrix Workspace or Microsoft Teams.
-  Bot μ-service: responsible for managing the session and routing all the messages to the specific μ-service to fulfill the user’s request (these can be either a question or an action)
-  Skills μ-service: responsible for creating dialogs with the user and fetch any specific data from the systems of records to create a response
-  Microapp service: responsible for creating microapps and providing information to the Citrix assistant. This is a single-tenant service
-  Utterance μ-service: To understand the user’s request, the utterance service uses natural language processing (NLP) to understand the user’s intent and entity of the query (for example if user asks “Who is Bob Smith?”, the intent is a directory lookup for the entity Bob Smith)
-  Spellcheck μ-service: to avoid user frustration when they have a typo. Corrects anything that is misspelled within the user’s request

Below is a detailed diagram of the data flow for the Citrix Assistant:

[![Citrix Workspace Virtual Assistant Architecture](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_processflow.png)](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_processflow.png)

## Administrative Controls

Administrators are able to create Citrix assistant resolvers. This means the administrator is able to choose a Citrix assistant intent and map them to specific source tables (see image below)

[![Citrix Workspace Virtual Assistant Admin Console1](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console1.png)](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console1.png)

They are also able to create filtering conditions. When the user asks for a lead from an application, the administrator would map user entities to data base entities. For example, when the user asks for the name of a lead, it would map to a specific entity within the database from that specific system of record.

[![Citrix Workspace Virtual Assistant Admin Console2](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console2.png)](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console2.png)

The administrator is also in control of what is the response value. When a user queries a lead, the administrator is mapping the fields that we need with the respective database fields.

[![Citrix Workspace Virtual Assistant Admin Console3](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console3.png)](/en-us/tech-zone/learn/media/tech-briefs_virtual-assistant_admin-console3.png)

## Authentication and authorization

The end-user would authenticate to Citrix Workspace like they usually would. Within Citrix Workspace they are able to interact and engage with the Citrix assistant. All service to service communications are encrypted using TLS encryption 1.2 or above. Communication between bot μ-service, skills μ-service, and the microapp service use RSA key pairs which are onetime use tokens to provide trust between the services. The authorization path must be implemented on the microapp service platform. The service μ-service to the SoR uses the user email to read from the microapp cache. Use the service accounts to read from the SoR while using user context for the writeback to the SoRs.

## Summary

Citrix Workspace’s intelligent features allow users to complete daily tasks in a more efficient manner. Gone are the days of wasting hours searching for information thanks to the Citrix Assistant. The Citrix assistant allows users to find information and complete actions even if they are not sure which application to find that information or complete the task in. To learn more, visit [product documentation for Citrix Assistant](/en-us/citrix-workspace/experience/citrix-assistant.html).
