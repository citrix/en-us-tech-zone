---
layout: doc
h3InToc: true
contributedBy: Steven Beals
specialThanksTo: Ben Robbins, Ryan Hafer, Geeta Vemuri
description: Learn how to enable the Citrix Workspace App for Microsoft Teams to enable your users to easily access their Citrix ShareFile documents and share them within Teams.
tz_title: Citrix Workspace App for Microsoft Teams
tz_products: citrix-content-collaboration;citrix-virtual-apps-and-desktops;citrix-workspace;
---
# POC Guide: Citrix Workspace App for Microsoft Teams

## Overview

The Citrix Workspace App for Microsoft Teams provides users the ability to access their ShareFile documents and share them with their peers in Microsoft Teams with just a few clicks. The Citrix Workspace App for Microsoft Teams allows end users to easily share content in 1:1 chats, group chats, and team channel chats.

## Scope

In this Proof of Concept guide you will enable the Citrix Workspace App for Microsoft Teams. This guide showcases the following actions:

• Create a Citrix Cloud account and create a Citrix Content Collaboration service trial

• Add employee users and files to Citrix ShareFile

• Add the Citrix Workspace App to Microsoft Teams

• Link Citrix Workspace App for Microsoft Teams to Citrix ShareFile

• Share a document via Workspace App for Microsoft Teams within a 1:1 Chat message and Microsoft Teams channel post

• Open a ShareFile hosted file directly within Microsoft Teams Chat

• Download ShareFile content within Microsoft Teams

• Open ShareFile content on a mobile device Microsoft Teams client

## Prerequisites

• Citrix Workspace App for Microsoft Teams must be enabled within the customers Microsoft Teams Account

• An active Citrix Cloud Account

• Active Citrix Content Collaboration Service

## Deployment Steps

### Create Citrix Cloud Account

If you're an existing Citrix Cloud customer, skip to the next section: Obtain a Citrix Content Collaboration Service Trial. Ensure that you have an active Citrix Cloud account. If your account has expired, contact your account manager to enable it.

If you're new to Citrix Cloud and need to create a Citrix Cloud account:
• Visit the [Sign Up for Citrix Cloud]((https://docs.citrix.com/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/signing-up-for-citrix-cloud.html#step-1-visit-the-sign-up-page)) page for step-by-step instructions of creating your Citrix Cloud account.

### Create Citrix Content Collaboration Service Trial

1.  Log into [Citrix Cloud](https://citrix.cloud.com) with your Citrix Cloud username and password
2.  Once authenticated to Citrix Cloud, find the Content Collaboration service tile, select the drop-down list, and click Request Trial.
![Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_cc_services.png)
3.  In the GEO Location section, select the service region you want to use. Acknowledge that the location cannot be changed after requesting the trial.
4.  In the Select a subdomain section, enter the unique subdomain you want to use.
5.  Click Request Trial. Citrix Cloud sends you an email after your Content Collaboration account is created.
6.  Under My Services, click Manage on the Content Collaboration tile to continue to the Content Collaboration Admin Overview.

### Add Users to Content Collaboration Service

Employee accounts need to be created to allow users to share cloud hosted files directly within Microsoft Teams. To create an employee:

1.  Go to Users > Manage Users Home in the Citrix Content Collaboration console in Citrix Cloud. Use the Create Employee button to begin creating a user.
2.  Type the user name, email address, and company info. You can also customize their password.
3.  You can customize the new employee User Access and File settings. Depending on your account or plan and your own permissions, certain permissions might not be visible or applicable.

![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_sharefile_user_access.png)
4. Assign folders to your user and add the user to Distribution Groups. You can also customize the user’s permissions to various folders on your account. To grant a user access to a folder, choose the check box beside the folder name.
![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_sharefile_assign_users_folder.png)
5. Send a Welcome Email to your new user or opt to do so later. This email includes a link to activate their new account.

### Add Files to Citrix ShareFile

1.  Open the URL of your ShareFile account in the form of subdomain.sharefile.com
2.  Navigate to your Personal Folder, access the blue circular Action button, and choose Upload.

![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_sharefile_file_upload.png)

### Add the Citrix Workspace App to Microsoft Teams

1.  Open your Microsoft Teams desktop client or sign-in via [Microsoft Teams](https://teams.microsoft.com)
2.  Once Microsoft Teams has opened, click the Apps icon.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_add_app.png)
3.  Search for “Citrix Workspace” and click Add.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_search_workspace.png)
4.  Click Sign in and Authenticate with your Citrix ShareFile.credentials.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_authenticate.png)
5.  Enter your ShareFile Company (subdomain) name.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_sfcompanyname.png)
6.  Enter your ShareFile credentials.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_sfusername.png)
7.  Click the three dots to open the Find an app window.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_threedots.png)
8.  Select Citrix Workspace or type Citrix Workspace to find the Workspace for Microsoft Teams app.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_workspaceapp_select.png)
9.  Right-click the Citrix Workspace for Microsoft Teams app and select Pin.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_pin.png)

The Citrix Workspace App for Teams is now be pinned to your Teams client:

![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_pinned.png)

### Share ShareFile Link within a Chat

1.  Start a new Chat within Microsoft Teams and click Citrix Workspace icon to share a file.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_chat.png)
2.  Select the location of the file you want to share within ShareFile.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_filelocation.png)
3.  Select the file you want to share, then click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_filetoshare.png)
4.  Your message with the ShareFile linked file is now ready to be sent.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_sent.png)

### Share ShareFile Link Within a Microsoft Teams Post

1.  Start a New Conversation within a Team and click Citrix Workspace icon to share a file.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_chatsend.png)
2.  Select the location of the file you want to share within ShareFile.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_filelocation.png)
3.  Select the file you want to share, then click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_filetoshare.png)
4.  Your message with the ShareFile linked file is now ready to be sent.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_chatsend.png)

### Open a Shared File From Within Microsoft Teams

1.  Within Microsoft Teams, select the post or message where ShareFile hosted file is available.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_openfile.png)
2.  Click Open, which opens the Citrix ShareFile hosted file in a new browser window.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_openendfile.png)

### Download a ShareFile Content From Within Microsoft Teams

1.  Browse to a recent chat where a file has been shared via ShareFile and click the Download button.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_download.png)

Your file will download to your local PC after quick URL validation and be available to be opened.

### Share a Document From Your Mobile Microsoft Teams Client

1.  On your mobile device, log into your Microsoft Teams client.
2.  Start a new chat, select the plus action button, and then select Citrix Workspace.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_mobile.png)
3.  Choose your file location.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_mobilefile.png)
4.  Select the file to be shared and click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_mobileshare.png)
5.  Send your message.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_workspace-app-for-teams_msteams_mobilesend.png)
