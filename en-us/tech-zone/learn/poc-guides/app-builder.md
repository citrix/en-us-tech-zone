---
layout: doc
h3InToc: true
contributedBy: Phil Wiffen
description: Learn how to get started with Citrix App Builder and deploy Podio-backed Integrations to Workspace
tz_title: Proof of Concept Guide - Use App Builder to roll out Citrix Podio-backed Broadcast and FAQ microapp templates to Citrix Workspace
tz_products: citrix-workspace
---
# PoC Guide: Proof of Concept Guide - Use App Builder to roll out Citrix Podio-backed Broadcast and FAQ microapp templates to Citrix Workspace

## Overview

Citrix App Builder (powered by Podio) provides a simplified way to start building Microapps with a Citrix-managed System of Record. With App Builder, Citrix Podio provides the back-end System of Record for the microapps, meaning that you do not have to bring or build your own System of Record. Using App Builder helps Administrators and Developers to deploy and build microapps more quickly and easily, and without having to deploy and manage their own backend system of record.

## Scope

This Proof of Concept guide covers how to use App Builder to roll out the Broadcast and FAQ (Frequently Asked Questions) microapps to people using Citrix Workspace. However, the concepts covered are applicable to other Citrix-provided Podio App packages that use Citrix Podio as the System of Record

## Goals

By the end of this Proof of Concept guide, you will have:

1.  used App Builder to create or link a Podio instance to Citrix Cloud
1.  added a Citrix-provided Podio app to act as the system of record for the Broadcast microapp and the FAQ microapp
1.  added and configured the Microapp Integration to use the Podio-backed System of Record
1.  added Subcribers to the microapps, so that appropriate people can post Broadcasts, and that everyone using Workspace will see Broadcasts and FAQs
1.  Tested the basic functionality of the Broadcast and FAQ microapps in Citrix Workspace

## Pre-requisites

-  Citrix Workspace
-  Citrix Microapps

Note: You *do not* need a Podio instance: one can be created when first using App Builder.

## Concepts and terminology

Some of the terms used in this guide may be unfamiliar.

-  System of Record - a place where data is read from, and written to. Similar to a database. Microapps Integrations use these to store data associated with the microapp. They can be thought of as a "source of truth"
-  Microapp - a user interface element, embedded into the Citrix Workspace experience
-  Podio Workspace - a podio concept that helps Podio admins logically separate teams or projects. Not the same as Citrix Workspace
-  Podio apps - apps that live inside a Citrix Podio workspace. In this use case, these apps provide a ready-built backend System of Record for Citrix Workspace Microapp integrations.
-  Integration - an integration connects to a System of Record. An integration also provides Microapps that can be made visible to people using Citrix Workspace, and reads data from the System of Record so that it can be displayed in the Microapps in Citrix Workspace.

## Deployment Steps

### Use App Builder to create or link a Podio instance to Citrix Cloud

This step is best covered in the [product documentation for App Builder](/en-us/citrix-cloud/app-builder.html), but is summarised below:

1.  Sign in to Citrix Cloud using your Citrix credentials
1.  From the Citrix Cloud console, under Available Services, select Set up on the App Builder tile
1.  If your organisation already has a Podio account, it can be connected to the Citrix Cloud account. If your organisation does not have a Podio account, one can be created here, for free.

Once a new podio account has either been created or linked, the next step is to add the example Citrix-provided Podio apps to the Podio instance

### Add the Citrix-provided Podio apps to act as the System of Record for the Broadcast and FAQ microapps

In this section, a new Podio Workspace will be created, specifically for Citrix Microapp Integrations. Inside the new Workspace, Podio apps provided by Citrix will be added.

#### Add the Broadcast Podio app

Add the [Citrix Workspace Broadcast app](https://podio.com/market/apps/192189-citrix-workspace-broadcast) from the [Podio App Market](https://podio.com/market):

1.  Go to [Citrix Workspace Broadcast app](https://podio.com/market/apps/192189-citrix-workspace-broadcast) in the Podio Market
1.  Click "Get App"
1.  Create a new space for the Citrix Workspace Broadcast app, called "**Citrix Microapp Integrations**". This helps keep the Podio apps and data separate from any other non-microapps Podio workspaces you may have, or want in the future. As other Citrix-provided Podio apps are added, they can be placed in this Workspace, too.
1.  Set access settings to Private. This workspace is used like a database, so having it private is advisable
1.  Click "Try the app now"

#### Add the FAQ Podio app

Next, add the [Citrix Workspace FAQ app](https://podio.com/market/apps/194119-citrix-workspace-faq) from the [Podio App Market](https://podio.com/market):

1.  Go to [Citrix Workspace FAQ app](https://podio.com/market/apps/194119-citrix-workspace-faq) in the Podio Market
1.  Click "Get App"
1.  Use the Existing Podio workspace created in the previous step: Citrix Microapp Integrations. This will mean that both the Broadcast and FAQ Podio apps are inside the same Podio Workspace.
1.  Set access settings to Private. This workspace is used like a database, so having it private is advisable.
1.  Click "Try the app now"

[Note] If architecting this at a later date for production, it's worth re-iterating that Podio Workspaces can host multiple Podio apps. This means that a generically named Podio Workspace, dedicated to Citrix Microapps, may make more sense than creating a new Podio Workspace for each Microapp Integration for ease of management and maintenance. In this Proof of Concept guide, we're keeping things compartmentalised, as this isn't production.

At this point, the Podio-backed System of Record, and its associated database schema for each Podio app is in place.

Next, collect data from Podio that's needed to configure the Microapp Integrations at a later step.

### Collect microapp integration configuration values from Podio

In this section, the following data will be collected:

-  App ID for Citrix Workspace Broadcast
-  Token for Citrix Workspace Broadcast
-  View ID for Citrix Workspace Broadcast
-  App ID for Citrix Workspace FAQ
-  Token for Citrix Workspace FAQ
-  View ID for Citrix Workspace FAQ
-  Client ID
-  Client Secret

This data is needed to configure the Podio Integration for microapps once it's added in Citrix Cloud.

#### Collect the Podio app ID and token for each app (Broadcast and FAQ)

The app ID and token are retrieved from Podio. Full documentation is [here](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-app-id-and-app-token), but a summary is provided below

1.  Login to podio.com
1.  From the navigation bar, choose the Workspace associated with the Podio app that was added earlier. In this example: Citrix Microapp Integrations
1.  This gets you into the Workspace. Now navigate to the Podio app itself, by clicking on the loudhailer icon, next to Activity.
1.  From here, click the spanner icon, then choose Developer, under the App category.
1.  The App ID and Token are displayed. Make a note of these.
1.  Go to the next Podio app

#### Collect View ID

Collect the View ID For each app (both Broadcast and FAQ).

The main reference documentation for this step is on Citrix Docs: [Collect View ID](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-view-id)

1.  For Broadcast app, the field is **Modified Today**
1.  For FAQs app, the field is **Published FAQs**

#### Collect Client ID and Client Secret

For the microapps podio integration to talk to the Podio backend, a Client ID and Client Secret must be created and entered into the Podio Integration configuration. This is not the same as a Citrix Cloud Client ID and Secret, and must be generated from Podio.

Full documentation is [here](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-client-id-and-client-secret), but a summary is provided below:

1.Log in to [API keys](https://podio.com/settings/api). Complete the fields under API Key Generator:

1.  Enter a name for Application name - for this proof of concept, name it **Citrix Microapp Integrations**
1.  Enter your Microapps instance URL for Full domain (without protocol) of your return URL. This section of the URL {yourmicroappserverurl} is composed of a tenant part, a region part, and an environment part: `https://{tenantID}.{region(us/eu/ap-s)}.iws.cloud.com`. An example might be: mycloudworkspace.us.iws.cloud.com
1.  Select Generate API Key.
1.  Under Your API keys, copy and save the Client ID and Client Secret values for the application you just added. You enter these values when you set up the integration.

#### Confirm all data is collected

Ensure that the following values have been collected. These will be needed in the next step.

-  App ID for Citrix Workspace Broadcast
-  Token for Citrix Workspace Broadcast
-  View ID for Citrix Workspace Broadcast
-  App ID for Citrix Workspace FAQ
-  Token for Citrix Workspace FAQ
-  View ID for Citrix Workspace FAQ
-  Client ID
-  Client Secret

Next, configure the Microapps service to use this new System of Record

### Add a Podio Integration to the Microapps service

The Podio Integration template in Citrix Microapps is a pre-built Integration that offers two use cases out of the box: Broadcast and FAQs. The back-end system of record has been created in Podio, and now the Microapps Integration and its Microapps templates will be added.

1.  From citrix.cloud.com, log into your Citrix Cloud customer
1.  Click on the Microapps tile
1.  Add a new Integration by clicking on "Add Integration"
1.  Because this is a Citrix-provided solution, choose to "Add a new integration from Citrix-provided templates"
1.  Choose Citrix Podio
1.  Choose Add

At this point, the Citrix Podio integration, that includes a few template Microapps, has been added to the Microapps service. Next, configure the Integration to talk to the Citrix Podio apps that were added earlier.

### Configure the Integration to talk to the Citrix Podio apps

After adding the Citrix Podio integration, it must be configured. The Configuration page should appear after adding the Integration, but if it does not: Navigate to the Microapps tile, locate the Citrix Podio integration, and click on the "update configuration" link.

The main reference documentation for this step is on Citrix Docs: [Integrate Podio](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html)

Note: When adding the Integration, it does not matter which App ID and Token you provide. You can provide either the Broadcast App ID and token, or the FAQ ID and token. This is due to how Podio workspaces work. Later, however, when configuring the Data loading and service actions, those App ID and and View IDs must match the corresponding Podio apps.

When configuring, you must change the following fields in the Configuration form:

1.  Podio app ID: Each Podio workspace has an App ID. Enter this value as an Access token parameter when you set up the integration replacing the podio_app_id variable. You can use the App ID from any Podio app in the Podio workspace.
1.  App token: Use this token to authenticate as an app rather than a user. Enter this value as an Access token parameter when you set up the integration replacing the podio_token_id variable.
1.  Client ID: The client ID is the string representing client registration information unique to the authorization server.
1.  Client Secret: The client secret is a unique string issued when setting up the target application integration.

### Configuring the Data loading and Service Actions

In this section, take the App IDs and View IDs for each of the Broadcast and FAQ podio apps and replace the variables and fields mentioned in the full documentation for this step [here](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#replace-data-loading-and-service-action-variables).

### Learn about the Broadcast and FAQ microapp capabilities

A summmary of the Broadcast and FAQ microapp capabilities is here: [Use Podio Microapps](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#use-podio-microapps)

### Granting access to the Microapps in Workspace

Finally, [configure the Microapps subscribers](/en-us/citrix-microapps/assign-subscribers.html#manage-subscribers), so that appropriate people have access from Citrix Workspace:

Conceptually:

1.  People who need to post Broadcast can be granted access to (Subscribed) to the *Create Broadcast* microapp.
1.  People who should be able to view Broadcasts and receive notifications in Workspace from the Broadcase app should be Subscribed to the *Broadcast* microapp
1.  People who need to be able Manage Broadcasts, including deleting them, should be Subscribed to the *Manage Broadcasts* microapp
1.  People who need to see FAQS in Workspace should be Subscribed to the FAQ microapp

The main reference for Subscribing people to Microapps is [here](/en-us/citrix-microapps/assign-subscribers.html#manage-subscribers)

Note: As this is a proof of concept, it may be desired to limit who can see the Microapps (by modifying Subscribers).

### Testing the Microapps

In this section, basic functionality of the microapps will be tested for the proof of concept.

#### Broadcast microapps

In this example someone with permission to Create Broadcast will create a new broadcast. Then, that Broadcast will be viewed in Workspace, both in the Activity feed and in the Broadcast microapp.

1.  Login to your Citrix Workspace
1.  Go to Actions, then click Create Broadcast
1.  Compose a test Broadcast
1.  Click Create
1.  When Podio pops up asking to grant permission, choose Grant Access

At this point, a new Broadcast has been posted to Citrix Workspace

Check the Broadcast has been posted to the System of Record (which in this case is Podio):

1.  In Citrix Workspace, go to Actions, then click Broadcast
1.  The Broadcast post is visible, and can be clicked to view the full details

Check the Broadcast is posted to the Activity Feed

By default, Notifications to Citrix Workspace Activity Feed are only posted when Data is incrementally synced from the Podio-backed system of record to the Microapps integration service. By default, this sync happens every hour.

To change this, or to force a sync see [Synchronize Data](/en-us/citrix-microapps/synchronize-data.html)

Once a data sync occurs, the Notification is displayed in Workspace Activity Feed:

![Activity Feed](/en-us/tech-zone/learn/media/poc-guides_app-builder_activity-feed-broadcast.png)

Note that, because the person logged into Workspace has access to both the Create Broadcast, and the Broadcast microapps, they see both Notifications: for Creation confirmation, and for the Broadcast itself.

If the Notifications are not in the Activity Feed, try to change the Feed sort from Recommended to Most Recent.

#### FAQ microapp

Testing the FAQ microapp is a little different. There is not currently a microapp to create or edit FAQs, so this is done through Podio. Fortunately, this helps to demonstrate how the Podio backend interacts with Microapps.

1.  Log into Podio and Navigate to the FAQs app
1.  Choose: Add Question
1.  Fill in the Title and Answer (these are Required), and any other fields that are appropriate
1.  To Publish the question, set the Status to Published
1.  Click Save Question

This saves the Question into the Podio app backend. Next, check Workspace for the new FAQ entry:

1.  Login to your Citrix Workspace
1.  Go to Actions, then click FAQ
1.  If a data sync has occured, the FAQ will appear. To force an incremental sync see [Synchronize Data](/en-us/citrix-microapps/synchronize-data.html)
1.  Click the new FAQ entry to read its full content

Note: By default, Notifications for new FAQ entries are not posted automatically at regular intervals to the Activity Feed. This helps prevent overcrowding of the Activity Feed. To change this, learn more about [Microapp Notifications](/en-us/citrix-microapps/create-microapps/build-a-notification.html)

This completes the testing of the basic microapp functionality

## Final summary

This is the end of the proof of concept guide. While following this guide you:

1.  used App Builder to create or link a Podio instance to Citrix Cloud
1.  added a Citrix-provided Podio app to act as the system of record for the Broadcast microapp and the FAQ microapp
1.  added and configured the Microapp Integration to use the Podio-backed System of Record
1.  added Subcribers to the microapps, so that appropriate people can post Broadcasts, and that everyone using Workspace will see Broadcasts and FAQs
1.  Tested the basic functionality of the Broadcast and FAQ microapps in Citrix Workspace

## Further reading

-  More details on integrating Citrix Podio app with Citrix Workspace microapps: [Integrate Podio](/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html)
