---
layout: doc
h3InToc: true
contributedBy: Phil Wiffen
description: Learn how to get started with Citrix App Builder and deploy Podio-backed Integrations to Workspace
tz_title: Proof of Concept Guide - App Builder
tz_products: citrix-workspace
---
# Proof of Concept Guide - App Builder

## Overview

Citrix App Builder (powered by Podio) provides a simplified way to start building Microapps with a Citrix-managed System of Record. With App Builder, Citrix Podio provides the back-end System of Record for the microapps, meaning that you do not have to bring or build your own System of Record. Using App Builder helps Administrators and Developers to deploy and build microapps more quickly and easily, and without having to deploy and manage their own backend system of record.

## Scope

This Proof of Concept guide covers how to use App Builder to roll out an example set of Microapps for Broadcasting information to people who use Citrix Workspace. The concepts covered are applicable to other Citrix-provided App packages that use Citrix Podio as the System of Record.

## Goals

By the end of this Proof of Concept guide, you will have:

1.  used App Builder to create or link a Podio instance to Citrix Cloud
2.  added a Citrix-provided Podio app to act as the system of record for the Broadcast microapp
3.  added and configured the Microapp Integration to use the Podio-backed System of Record
4.  added Subcribers to the microapps, so that appropriate people can post Broadcasts, and that everyone using Workspace will see Broadcasts posted

## Pre-requisites

-  Citrix Workspace
-  Citrix Microapps

Note: You *do not* need a Podio instance: one can be created when first using App Builder.

## Concepts and terminology

-  System of Record - a place where data is read from, and written to. Similar to a database. Microapps Integrations use these to store data associated with the microapp.
-  Microapp - a user interface element, embedded into the
-  Podio apps: apps that live inside Citrix Podio. In this use case, these apps provide a ready-built backend System of Record for Citrix Workspace Microapp integrations.

## Deployment Steps

### Use App Builder to create or link a Podio instance to Citrix Cloud

This step is best covered in the [product documentation for App Builder](https://docs.citrix.com/en-us/citrix-cloud/app-builder.html), but is summarised below:

1.  Sign in to Citrix Cloud using your Citrix credentials
1.  From the Citrix Cloud console, under Available Services, select Set up on the App Builder tile
1.  If your organisation already has a Podio account, it can be connected to the Citrix Cloud account. If your organisation does not have a Podio account, one can be created here

Once a new podio account has either been created or linked, the next step is to add the example Citrix-provided Podio app to the Podio instance

### Add a Citrix-provided Podio app to act as the System of Record for the Broadcast microapp

Add the [Citrix Workspace Broadcast app](https://podio.com/market/apps/192189-citrix-workspace-broadcast) from the [!Podio App Market](https://podio.com/market):

1.  Go to [Citrix Workspace Broadcast app](https://podio.com/market/apps/192189-citrix-workspace-broadcast) in the Podio Market
2.  Click "Get App"
3.  Create a new space for the Citrix Workspace Broadcast app. This helps keep the data separate from any other non-microapps Podio workspaces you may have.
4.  Set access settings to Private. This workspace is used like a database, so having it private is advisable
5.  Click "Try the app now"

[Note] If architecting this at a later date for production, it's worth noting that Podio Workspaces can host multiple Podio apps. This means that a generically named Podio Workspace, dedicated to Citrix Microapps, may make more sense, because you'll be able to continue to add more Podio-backed microapps to the same workspace. In this Proof of Concept guide, we're keeping things compartmentalised, as this isn't production.

At this point, the Podio-backed System of Record, and its associated database schema is in place. Next, configure the Microapps service to use this new System of Record

### Add a Podio Integration to the Microapps service

1.  From citrix.cloud.com, log into your Citrix Cloud customer
2.  Click on the Microapps tile
3.  Add a new Integration by clicking on "Add Integration"
4.  Because this is a Citrix-provided solution, choose to "Add a new integration from Citrix-provided templates"
5.  Choose Citrix Podio
6.  Choose Add

At this point, the Citrix Podio integration, that includes a few template Microapps, has been added to the Microapps service. Next, configure the Integration to talk to the Citrix Podio app that was added earlier.

### Configure the Integration to talk to the Citrix Podio app

After adding the Citrix Podio integration, it must be configured. The Configuration page should appear after adding the Integration, but if it does not: Navigate to the Microapps tile, locate the Citrix Podio integration, and click on the "update configuration" link.

The main reference documentation for this step is on Citrix Docs: [Integrate Podio](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html)

When configuring, you must change the following fields in the Configuration form:

1.  Podio app ID: Each Podio workspace has an App ID. Enter this value as an Access token parameter when you set up the integration replacing the podio_app_id variable. You can use the App ID from any Podio app in the Podio workspace. See Collect App ID and App token.
1.  App token: Use this token to authenticate as an app rather than a user. Enter this value as an Access token parameter when you set up the integration replacing the podio_token_id variable. Collect this with the App ID.
1.  Client ID: The client ID is the string representing client registration information unique to the authorization server. See Collect Client ID and Client Secret.
1.  Client Secret: The client secret is a unique string issued when setting up the target application integration.

### Collect configuration values

#### Collect the Podio app ID and token

The app ID and token are retrieved from Podio. Full documentation is [here](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-app-id-and-app-token), but a summary is provided below

1.  Login to podio.com
2.  From the navigation bar, choose the Workspace associated with the Podio app that was added earlier. In this example: Citrix Workspace Broadcast
3.  This gets you into the Workspace. Now navigate to the Podio app itself, by clicking on the loudhailer icon, next to Activity.
4.  From here, click the spanner icon, then choose Developer, under the App category.
5.  The App ID and Token are displayed. Make a note of these.

#### Collect View ID

The main reference documentation for this step is on Citrix Docs: [Collect View ID](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-view-id)

#### Collect Client ID and Client Secret

To talk to the Podio backend, a Client ID and Client Secret must be created and entered into the Podio Integration configuration. This is not the same as a Citrix Cloud Client ID and Secret, and must be generated from Podio.

Full documentation is [here](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#collect-client-id-and-client-secret), but a summary is provided below:

1.Log in to [API keys](https://podio.com/settings/api). Complete the fields under API Key Generator:

1.  Enter a name for Application name - for this proof of concept, name it Citrix Workspace Broadcast
2.  Enter your Microapps instance URL for Full domain (without protocol) of your return URL. This section of the URL {yourmicroappserverurl} is composed of a tenant part, a region part, and an environment part: <https://{tenantID}.{region(us/eu/ap-s)}.iws.cloud.com>. An example might be: mycloudworkspace.us.iws.cloud.com
3.  Select Generate API Key.
4.  Under Your API keys, copy and save the Client ID and Client Secret values for the application you just added. You enter these values when you set up the integration.

#### Confirm all data is collected

Ensure that the following values have been collected. These will be needed in the next step.

-  App ID
-  Token
-  View ID
-  Client ID
-  Client Secret

### Configuring the Broadcast microapp Service Actions

The full documentation for this step is [here](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#replace-data-loading-and-service-action-variables). Only follow guidance that mentions *Broadcast*. The configuration of the FAQ microapp is out of the scope of this document.

### Learn about the Broadcast microapp capabilities

A summmary of the Broadcast microapp capabilities is here: [Use Podio Microapps](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html#use-podio-microapps)

### Granting access to the Microapps in Workspace

Finally, configure the Microapps subscribers, so that appropriate people have access from Citrix Workspace:

Conceptually:

1.  People who need to post Broadcast can be granted access to (Subscribed) to the *Create Broadcast* microapp.
2.  People who should be able to view Broadcasts and receive notifications in Workspace from the Broadcase app should be Subscribed to the *Broadcast* microapp
3.  People who need to be able Manage Broadcasts, including deleting them, should be Subscribed to the *Manage Broadcasts* microapp



Note: In this Proof of Concept guide, the FAQ microapp is not being configured. By default, no one is Subscribed to the FAQ microapp, so there is no need to delete it if you do not want people to see it in Workspace.

## Further reading

-  More details on integrating Citrix Podio app with Citrix Workspace microapps: <https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-podio.html>
