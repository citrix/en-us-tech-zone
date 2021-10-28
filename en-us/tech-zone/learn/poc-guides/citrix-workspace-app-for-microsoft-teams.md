---
layout: doc
h3InToc: true
contributedBy: Steven Beals
specialThanksTo: Ben Robbins, Ryan Hafer, Geeta Vemuri
description: Learn how to enable the Citrix Workspace App for Microsoft Teams to enable your users to easily access their Citrix ShareFile documents and share them within Teams.
tz_title: Citrix Workspace App for Microsoft Teams
tz_products: citrix-content-collaboration;citrix-virtual-apps-and-desktops;citrix-workspace;
---
# PoC Guide: Citrix Workspace App for Microsoft Teams

## Overview

The Citrix Workspace App for Microsoft Teams provides users with the ability to quickly access their ShareFile documents and share them with their peers in Microsoft Teams within just a few clicks. The Citrix Workspace App for Teams will allow end users to easily share content in 1:1 chats, group chats, and team channel chats.

## Scope

In this Proof of Concept guide you will enable the Citrix Workspace App for Teams.  This guide will showcase how to perform the following actions:

• Create a Citrix Cloud account

• Obtain a Citrix Content Collaboration service trial

• Add files to Citrix ShareFile

• Add the Citrix Workspace App to Microsoft Teams

• Link Citrix Workspace App for Microsoft Teams to Citrix ShareFile

• Share a document via Workspace App for Teams within a 1:1 Chat message

• Share a document via Workspace App for Teams within a Teams channel post

• Open a ShareFile hosted file directly within Teams Chat

• Download a file

• Open a file on a mobile device

## Prerequisites

• Citrix Workspace App for Microsoft Teams must be enabled within the customers Microsoft Teams Account

• An active Citrix Cloud Account

• Active Citrix Content Collaboration Service

## Deployment Steps

### Create Citrix Cloud Account

If you are an existing Citrix Cloud customer, skip to the next section: Obtain a Citrix Content Collaboration Service Trial.  Ensure that you have an active Citrix Cloud account. If your account has expired, contact your account manager to enable it.

If you are new to Citrix Cloud and need to create a Citrix Cloud account, visit the Sign Up for Citrix Cloud page for step-by-step instructions of creating your Citrix Cloud account.

### Obtain a Citrix Content Collaboration Service Trial

1.  Login to citrix.cloud.com with your Citrix Cloud username and password
2.  Once authenticated to Citrix Cloud, find the Content Collaboration service tile, select the dropdown, and click Request Trial.
![Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_cc-services.png)
3.  In the GEO Location section, select the service region you want to use and acknowledge that the location can’t be changed after requesting the trial.
4.  In the Select a subdomain section, enter the unique subdomain you want to use.
5.  Click Request Trial. Citrix Cloud sends you an email after your Content Collaboration account is created.
6.  Under My Services, click Manage on the Content Collaboration tile to continue to the Content Collaboration Admin Overview.

### Add Users to Content Collaboration Service

For the Citrix Workspace App for Microsoft Teams, employee accounts will be created to allow users to share cloud hosted files directly within Teams.  To create an employee:

1.  Go to Users > Manage Users Home in the Citrix Content Collaboration console in Citrix Cloud. Use the Create Employee button to begin creating a user.
![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_create-employee.png)
2.  Type your user’s name, email address, and company info. You can also customize their password.
![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_type-user.png)
3.  You can customize your new employee’s User Access and File settings. Depending on your account or plan and your own permissions, certain permissions might not be visible or applicable. User Access settings are typical access and feature-based permissions you can use to manage your employee’s access and abilities on the account.
![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_shareFile-user-access.png)
4.  Assign folders to your user and add the user to Distribution Groups. You can also customize the user’s permissions to various folders on your account. To grant a user access to a folder, choose the check box beside the folder name.
![Citrix Sharefile](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_shareFile-assign-users-folder.png)
5.  Send a Welcome Email to your new user or opt to do so later. This email includes a link to activate their new account.

### Add Files to Citrix ShareFile

1.  Open the URL of your ShareFile account in the form of subdomain.sharefile.com
2.  Navigate to your Personal Folder, access the blue circular Action button, and choose Upload.

![Citrix ShareFile](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_shareFile-file-upload.png)

### Add the Citrix Workspace app to Teams

1.  Open your Microsoft Teams desktop client or sign-in via [Microsoft Teams](https://teams.microsoft.com)
2.  Once Teams has opened, click on the Apps icon.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_add-app.png)
3.  Search for “Citrix Workspace”, and click Add.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-search-workspace.png)
4.  Click Sign in and Authenticate with your Citrix ShareFile.credentials.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-authenticate.png)
5.  Enter your ShareFile Company (subdomain) name.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-sfcompanyname.png)
6.  Enter your ShareFile credentials.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-sfusername.png)
7.  Click on the three dots to open the Find an app window.

    ![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-threedots.png)
8.  Select Citrix Workspace or type Citrix Workspace to find the Workspace for Teams app.

![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-workspaceapp-select.png)
9.  Right-click the Citrix Workspace for Teams app and select Pin.

![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-pin.png)

The Citrix Workspace App for Teams should now be pinned to your Teams client:

![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-pinned.png)

### Share ShareFile Link within a Chat

1.  Start a new Chat within Teams and click on Citrix Workspace icon to share a file.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-chat.png)
2.  Select the location of the file you wish to share within ShareFile.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-filelocation.png)
3.  Select the file you wish to share, then click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-filetoshare.png)
4.  Your message with the ShareFile linked file is now ready to be sent.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-sent.png)

### Share ShareFile Link Within a Chat

1.  Start a New Conversation within a Team and click on Citrix Workspace icon to share a file.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-chatsend.png)
2.  Select the location of the file you wish to share within ShareFile.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-filelocation.png)
3.  Select the file you wish to share, then click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-filetoshare.png)
4.  Your message with the ShareFile linked file is now ready to be sent.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-chatsend.png)

### Open a Shared File From Within Teams

1.  Within Teams, select the post or message where ShareFile hosted file is available.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-openfile.png)
2.  Click Open, which will open Citrix ShareFile hosted file in a browser window.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-openendfile.png)

### Download a File From Within Teams

1.  Browse to a recent chat where a file has been shared via ShareFile and click the Download button.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-download.png)

Your file will begin downloading to your local PC after quick URL validation and will be available to be opened.

### Share a Document From Your Mobile Microosft Teams Client

1.  On your mobile device, login into your Microsoft Teams client.
2.  Start a new chat, select the plus action button, and then select Citrix Workspace.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-mobile.png)
3.  Choose your file location.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-mobilefile.png)
4.  Select the file to be shared and click Share.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-mobileshare.png)
5.  Send your message.
![Microsoft Teams](/en-us/tech-zone/learn/media/poc-guides_citrix-workspace-app-for-microsoft-teams_msteams-mobilesend.png)
