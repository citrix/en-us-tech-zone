---
layout: doc
h3InToc: true
contributedBy: Hubert Krautter
specialThanksTo: Martin Zugec, Chetan Takker, Johnathan Campos, Jeroen J.V Lebon
description: Learn how to migrate your Citrix Endpoint Management policies and apps step by step. Take your Endpoint Management from a legacy Android Device Administrator deployment to Android Enterprise by using a managed Google Play account.
---
# Migration from Android Device Administrator to Android Enterprise with Citrix Endpoint Management

## Introduction

Android Enterprise is a set of tools and services provided by Google as an enterprise management solution for Android devices. With Android Enterprise, you use Citrix Endpoint Management (CEM) to manage corporate-owned and bring your own (BYOD) Android devices.
You can manage the entire device or a separate profile on the device. The separate profile isolates business accounts, apps, and data from personal accounts, apps, and data. You can also manage devices dedicated to a single use, such as inventory management.

For an overview of Android Enterprise capabilities from Google, see [Android Enterprise Management](https://www.android.com/enterprise/management/)

Prior to Android Enterprise, companies were using Device Administrator (DA) to manage and secure Android devices. With the needs of organizations changing, devices now are reaching more corporate and confidential data than ever. At the same time end-users require protection of personal data and are concerned of their privacy. With this Google announced the deprecation of Device Administrator (DA) mode in favor for Android Enterprise, a modern management platform that focuses on security and user privacy. Google is recommending that customers using device administrator mode migrate to Android Enterprise due to Device Administrator (DA) deprecation.

This guide is intended to provide step by step information on how to move from a legacy device administrator Android deployment to Android Enterprise by using a managed Google Play Account.

For more information refer to [Android Enterprise Migration Bluebook](https://static.googleusercontent.com/media/www.android.com/en//static/2016/pdfs/enterprise/Android-Enterprise-Migration-Bluebook_2019.pdf).

## Prerequisites for using Android Enterprise

When you integrate CEM with managed Google Play to use Android Enterprise, an enterprise is created. Google defines an enterprise as a binding between the Android Enterprise organization and your enterprise mobile management (EMM) solution. Registering Citrix as your EMM Provider, is required as part of this process. All the users and devices that the organization manages through your EMM solution belong to the created enterprise.

If you are not using a G-Suite account, a personal/corporate shared Google account is required to complete the enterprise registration. This account is then responsible for this enterprise, and becomes the main managed Google Play account. More information can be found by visiting the following [link](https://support.google.com/googleplay/work/answer/7042221?hl=en&ref_topic=7042018).

The G-Suite and personal/corporate shared accounts are free. The main difference between a managed Google Play account and a managed Google account is that the Managed Google Account is based on a G-Suite Subscription and needs to prove the domain ownership. See the following [link](/en-us/xenmobile/server/provision-devices/android-enterprise/legacy-android-enterprise-for-g-suite-customers.html).

If you do not have a Google account readily available to create this enterprise, use the following [instruction](https://support.google.com/googleplay/work/answer/7042126?hl=en&ref_topic=7042018) and prerequisites to create your account.

For Citrix Endpoint Management (CEM) administrators, managed Google Play combines the user experience and app store features of Google Play designed for enterprises.

Managed Google play provides a store for end-users managed and controlled by the admin.

## Activation of Android Enterprise

To set up Android Enterprise for your organization, register Citrix as your EMM provider through managed Google Play. Completing this setup results in the creation of your enterprise connecting managed Google Play to Citrix Endpoint Management.

Google Play infrastructure is used to offer services that include a managed, private enterprise app delivery store. Google Play is also where device profiles live. The good news is that setup is rather simple.

For activation of Android Enterprise, navigate to your **CEM** settings, and select **Android Enterprise** and follow the instructions.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_01.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_01.png)

**Note:**
Depending on your Endpoint Management version, you see a different type of menu used to create the Android Enterprise account. In all versions an enterprise ID is added for Android Enterprise in your Console.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_02.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_02.png)

Select **Enable** your Android Enterprise.

**Note:** If you already use Enrollment profiles with Android Legacy Mode, you can see the following message:

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_03.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_03.png)

## Configuration Enrollment Profiles for Android Enterprise

In Citrix Endpoint Management and in XenMobile version 10.12+ we have several Enrollment profiles. Enrollment profiles control how Android devices are enrolled if Android Enterprise in enabled for your Endpoint Management deployment.
Enrollment profiles determine whether Android devices are enrolled as Android Enterprise devices or legacy (device administrator) devices.

These profiles allow admins to begin the easy migration to Android Enterprise. Full support for all Android Enterprise management profiles is available.

-  Fully Managed Devices
-  Dedicated devices (COSU Devices)
-  Fully Managed Devices with a work profile (Cope Devices)

**Note:** With cloud deployments, more options are available to differentiate between work profile and other modes. RBAC is also not required for setup of dedicated devices

On-prem deployments do not have this option and must use RBAC for setup of dedicated devices. On-prem deployments also do not provide an option within Enrollment Profiles for dedicated devices. See the following for [more information](https://support.citrix.com/article/CTX237983).

For additional information on enrollment profiles, visit the [link](/en-us/citrix-endpoint-management/device-management/android/android-enterprise.html#creating-enrollment-profiles).

**Note:** With Android 11, all of this is about to change. Read the blog [Changes ahead for Android Enterprise’s Fully Managed with Work Profile](https://www.citrix.com/blogs/2020/04/09/changes-ahead-for-android-enterprises-fully-managed-with-work-profile/) for more details.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_04.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_04.png)

When creating enrollment profiles, you must assign delivery groups to them. If a user belongs to multiple delivery groups that have different enrollment profiles, the name of the delivery group determines the enrollment profile used.

In a delivery group conflict select the delivery group that appears last in an alphabetized list of delivery groups. For more information, see [Enrollment Profiles](/en-us/citrix-endpoint-management/users/enrollment-profiles.html).

**IMPORTANT:** Do not assign the new enrollment profile to any delivery group at the moment if you have a user who belongs to multiple delivery groups.

## Creating the Delivery Group for Use in Android Enterprise

The best way to start is to copy an existing delivery group to associate to an Android Enterprise Enrollment Profile:

-  Step 1: Name the new delivery group
    -  Ensure to make the name last alphabetically. Example `zTestUserGroup`
-  Step 2: Assign the new delivery group to the new Enrollment Profile
    -  DO NOT Assign an AD Group to the delivery group until your settings are completed

## Migration Apps

### Mobile productivity apps to Android Enterprise users

Although CEM has support for many different application types, for this migration scenario, we show how to migrate Secure Mail.

-  Step 1: Create an app App Category for existing iOS and Android Legacy Apps
  
   [![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_05.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_05.png)

-  Step 2: Move Citrix Secure Mail configured for Android (legacy DA) into this new category to have a clear separation of Android (legacy DA) applications. **NOTE**: This action must be done for all Android (legacy DA) applications in your environment

-  Step 3: Edit Secure Mail Android (legacy DA) with the following Deployment Rule to prevent showing the app twice within Secure Hub (One: Android (legacy DA) Two: Android Enterprise)

  [![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_06.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_06.png)

Limit by known device property name Android Enterprise Enabled Device ID isn't equal to true

**Note:** this value is only available if you already enrolled an Android Enterprise Device

-  Step 4: Create a second category for Secure Mail for Android Enterprise
**NOTE:** This action must be done for all Android Enterprise applications in your environment.

-  Step 5: Configure Secure for Android Enterprise
-  _Example: SecureMail_AE_
    -  Assign the app to the Android Enterprise category.
    -  Configure the app for use in Android Enterprise only.
    -  Approve the app in Google Play for Work Store within the CEM console.
    -  Assign the app to the delivery group for use within Android Enterprise enrollment profile.

-  Step 6: Edit the app’s policies and actions to match your previous version of Secure Mail Android (legacy DA) within the Android Enterprise section.

### Publishing Enterprise Apps (MDX and Non-MDX)

**If you are not using privately developed wrapped or enterprise applications, you can skip this section. Click [here](#add-private-enterprise-app-as-android-enterprise-app-non-mdx) to go to the next section.**

ALL Enterprise apps need to be uploaded to Google Play for using with Android Enterprise.
To simplify IT admins’ app management workflow, we’ve integrated the [Managed Google Play iframe](https://developers.google.com/android/work/play/emm-api/managed-play-iframe) into Citrix Endpoint Management (CEM). This enables IT admins to approve and publish public or private apps from within the CEM console. Admins no longer need to go outside the console to the Managed Play or Developer portals to approve or publish apps.
This iFrame allows you to upload to Google play without creating a google developer account (saving $25). Once enterprise apps are uploaded by IT admins, they are only available within your enterprise deployment.

## Add private Enterprise app as Android Enterprise app (non-mdx)

In the **Endpoint Management Console**, click configure > Apps and select Enterprise Apps and upload the apk. The upload button opens the managed Google Play store.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_12.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_12.png)

## Add private Android Enterprise apps as MDX-wrapped Enterprise app

Use command line toolkit to wrap a private AE app with the MDX Toolkit.
Your output is:

  a. Wrapped .apk (size greater than original apk)

  b. .mdx file

-  Upload the .apk to Google Play (similar to the non-mdx apps above)
-  Goto **publish MDX app** and upload the mdx file

For more details and wrapping example [visit](/en-us/citrix-endpoint-management/apps.html#add-an-enterprise-app)

## Editing Existing Policies for the Correct Device Type Android (Legacy DA) and Android Enterprise (AE

To differentiate devices enabled for Android Enterprise and Android (legacy DA), a Deployment Rule can be used to ensure the correct policies are delivered with the app to the correct device. For Android (legacy DA): Limit by known device property name Android Enterprise Enabled Device? Isn’t equal to true

This Deployment Rule checks if the Android device is **NOT** enabled for Android Enterprise and deliver the policies along with the application.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_07.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_07.png)

This Deployment Rule checks if the Android device is enabled for Android Enterprise and deliver the policies along with the application.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_08.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_08.png)

## Testing and Review

To test, assign an Active Directory Group to the recently created delivery group used for Android Enterprise. Use the same AD-Group as used before on the existing delivery group.

Re-enroll the user’s device to begin the Android Enterprise enrollment process. Note the new look and feel of Android Enterprise

## Unenroll and reenroll the Device

Users can unenroll from within Secure Hub. Follow the detailed [instructions](/en-us/xenmobile/server/provision-devices/devices-enroll.html)

In this guide we are using the work profile owner mode, so that the device does not need to be new or factory reset.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_10.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_10.png)

Your device is now Android Enterprise enabled.

From your **CEM console**, navigate to Manage followed by Devices for an overview of which devices are Android Enterprise enabled.

[![da-to-ae-migration-Image-01](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_11.png)](/en-us/tech-zone/build/media/deployment-guides_android-device-administrator-to-android-enterprise_11.png)

After successful re-enrollment you can see two devices, one Android (legacy DA) and the other Android Enterprise.
The previous listed Android (legacy DA) device can be deleted to ensure the most updated device list.
