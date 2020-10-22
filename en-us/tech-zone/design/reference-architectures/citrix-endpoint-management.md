---
layout: doc
h3InToc: true
contributedBy: Vivekananthan Devaraj
specialThanksTo: David Egan, Chetan Thakker, Vikas Nambiar
description: Learn about the architecture and integration with Microsoft EMS/Intune and Android Enterprise to deliver applications securely to any device and how it enables security and productivity benefits for both Microsoft EMS/Intune and Citrix customers.
---
# Reference Architecture on Citrix Endpoint Management integration with Microsoft Intune/EMS and Android Enterprise

## Citrix Endpoint Management Overview

Citrix Endpoint Management simplifies device and app management with a comprehensive, unified endpoint management solution. It also enables anywhere, any-device access to everything people need to be productive - including intelligence features that guide and automate work. There are also cloud management options for Citrix Virtual Apps and Desktops. Citrix Endpoint Management is available as a service in Citrix Cloud that removes the need for the customer to manage infrastructure, allowing them to focus on the device policies and application management.

Citrix Endpoint Management has Mobile Device Management (MDM) and Mobile App Management (MAM) features. MDM features of Endpoint Management allow admins to deploy device policies and apps, retrieve asset inventories, and carry out actions on devices, such as a device wipe. MAM features of Endpoint Management allow securing the apps and data on Bring Your Own mobile devices, delivering mobile enterprise apps, locking apps, and wiping app data.

Refer to the [Citrix documentation](/en-us/xenmobile/server/advanced-concepts/xenmobile-deployment/reference-architecture-on-prem.html), which illustrates the reference architectures for the Endpoint Management deployment, formerly known as XenMobile. The deployment scenarios include MDM-only, MAM-only, and MDM+MAM as the core architectures.

Refer to the [Product documentation](/en-us/citrix-endpoint-management/about.html#architecture), which illustrates the Endpoint Management components and comprehensive reference architecture diagrams with communication flow. It also covers the core reference architecture, integration with Citrix Virtual Apps and Desktops, Endpoint Management connector for Exchange ActiveSync, and Citrix Gateway Connector for Exchange ActiveSync.

## Microsoft EMS Overview

Microsoft EMS stands for Microsoft Enterprise Mobility + Security. It includes the following:

*  **Azure Active Directory Premium** – the Microsoft identity hub that enables single-sign-on for all resources from anywhere, and it includes security aspects such as conditional access and multifactor authentication.

*  **Microsoft Intune** – is the Microsoft mobile device management and mobile app management solution which provides security controls for users, data, and apps on mobile devices. Microsoft is planning to bring together the System Center Configuration Manager (SCCM) and the Microsoft Intune mobile management service into a new brand called "Microsoft Endpoint Manager."

*  **Azure Rights Management** – provides document-level security that manages and enforces rights to access protected data.

*  **Microsoft Advanced Threat Analytics** – provides real-time security monitoring utilizing big data to identify threats and notify of risks

Microsoft EMS is licensed through several Microsoft offerings with different sets of features that vary by enterprise needs. EMS includes Intune licenses; therefore, every EMS customer can utilize Intune MDM and/or Intune App Protection capabilities.

## Citrix Endpoint Management integration with Microsoft Intune/EMS

The purpose of the integration between both of the products is to deliver applications securely to any device. Integration with Microsoft Intune/EMS is a feature of Citrix Endpoint Management Service that adds value to Microsoft EMS + Intune by providing secure access to on-premises resources for Intune and EMS-enabled apps, such as Office365 and other line-of-business apps. It also provides security and productivity benefits to Intune and Citrix Endpoint Management customers.

Mobile app deployment for enterprise organizations contains the following:

*  Office 365 apps
*  Other native mobile apps
*  Custom-built apps hosted on-premises
*  Web and SaaS apps
*  Virtualized apps hosted on-premises and cloud

These days organizations deploy Enterprise Mobility Management and Office365 apps for Bring Your Own Devices (BYOD) or Company-Owned Devices (COD), and it includes Microsoft EMS/Intune. The most popular Office365 apps are Word, Excel, Outlook, and PowerPoint. There are also other Office 365 apps, such as Microsoft SharePoint and Microsoft Dynamics 365. End-users need access to these productivity apps on their device. Enterprise organizations need to monitor those apps and how the app is handling the data on the user device with the best user experience.

As part of the Citrix Secure Digital Workspace, Endpoint Management allows IT to configure and apply micro-VPN connectivity to mobile applications. Each managed mobile app on the device has its own private micro-VPN from which application data can flow securely from the endpoint to corporate resource locations behind the firewall. Citrix Endpoint Management also delivers functionality like exchanging data and documents between Office 365 apps and organizational apps. With the help of Citrix Gateway, Citrix Endpoint Management can provide a per-app-micro-VPN to secure the data in motion.

With this integration, Citrix Endpoint Management (CEM) can push device-compliance status to Azure Active Directory Premium through Microsoft Intune device compliance service. CEM ensures the appropriate conditions are met for access to company resources, through Microsoft Intune device compliance service and Azure Active Directory Conditional Access.

### Conceptual Architecture

[![Citrix-Endpoint-Management-Image-1](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_001.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_001.png)

### Benefits of CEM Integration with EMS/Intune

Citrix Endpoint Management allows IT to protect and isolate corporate data and apps from personal apps and data on endpoint devices. Citrix Endpoint Management comes with an app store which is a secure and private app store designed for the enterprise. In this app store, IT can deliver corporate apps and public apps.

CEM brings the value of Citrix Endpoint micro-VPN to Microsoft Intune aware apps, such as Microsoft Managed Browser. It also allows enterprises to wrap their line-of-business apps with Intune and Citrix to provide micro-VPN capabilities inside an Intune mobile app management (MAM) container. Citrix Endpoint micro-VPN enables mobile apps to access on-premises resources.

IT can manage and deliver Office 365 apps, line-of-business apps, and Citrix Secure Mail in one container for ultimate security and user productivity. Micro-VPN brings the remote access capabilities of the market-leading Citrix Gateway to mobile devices via apps integrated with the CEM/XenMobile SDK. It is an on-demand application VPN connection initiated by Secure Hub on mobile devices to access corporate network sites or resources.

[![Citrix-Endpoint-Management-Image-2](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_002.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_002.png)

### Sample use-cases for CEM Integration with Intune

With CEM micro-VPN capability, a user can access internal resources using the Intune Browser, which now has CEM/XenMobile micro-VPN SDK embedded, without requiring a device-level VPN or full MDM enrollment.

With CEM micro-VPN capability, an IT admin can enable access to the line of business app servers hosted on-premises without having to set up a device-level VPN or having to go through full MDM enrollment.

### Deployment Modes

Citrix Endpoint Management and Microsoft EMS can work together in various ways. We are focusing here on integration with Intune deployment models or scenarios. The section following lists possible mobility management scenarios with Endpoint Management and Intune. Choosing the right deployment model depends on several factors, such as mobility requirements or current licensing investment. Each deployment model has its own merits. The goal is to highlight the solution that provides the "best of" what Citrix and Microsoft have to offer together.

Let's review the three key integration scenarios:

### Citrix MDM + Intune MAM + mVPN SDK

The recommended deployment model, which brings together all the key benefits into a single solution. It provides an excellent admin experience due to access to Microsoft Graph APIs within the Citrix console. All Citrix apps are Intune Enlightened hence it allows share data securely within a single container with DLP. It also includes all the Micro-VPN value-adds, and it provides an excellent user experience. This model applies comprehensive Citrix Unified Endpoint Management with Intune MAM for Office 365 apps and Secure Mail.

[![Citrix-Endpoint-Management-Image-3](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_003.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_003.png)

### Intune MDM + Intune MAM + mVPN SDK

This deployment model may be a popular scenario for existing Microsoft EMS customers that are utilizing Intune as MDM (devices enrolled). Citrix Endpoint Management with Micro VPN provides value-add in several areas, including enlightened ShareFile, Secure Mail, Microsoft Managed Intune Browser with XenMobile SDK, Citrix Gateway NAC, and the CEM integration with Intune EMS wizard to facilitate Intune configuration. Moreover, no re-enrollment is required. Hence it adds value instantly.

[![Citrix-Endpoint-Management-Image-4](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_004.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_004.png)

### Intune MAM Only + mVPN SDK

This deployment model provides excellent value for Intune customers who find MDM enrollment intrusive and may use third-party VPN solutions to access corporate resources behind the firewall. However, the downside of third-party VPN solutions is that they increase maintenance, can be inconsistent, require expensive infrastructure, licensing, and operational costs. Moreover, the device VPN solutions, in addition to per-app VPN solutions, generally aren't as efficient for mobile devices and cause the battery drain. On the other hand, the Citrix proprietary micro-VPN is clientless, and the Citrix Gateway configuration well drives it. Now Intune customers using Microsoft Managed Intune browsers (with or without Intune MDM) can use the Citrix micro-VPN to access Intranet resources.

[![Citrix-Endpoint-Management-Image-5](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_005.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_005.png)

No device enrollment or device level VPN is required. Citrix is the only vendor to provide micro-VPN for Intune apps or Intune wrapped apps without MDM enrollment or use of legacy device VPN clients.

[![Citrix-Endpoint-Management-Image-6](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_006.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_006.png)

This deployment model is for customers who are looking to utilize dual MAM containers for both Intune and Citrix wrapped applications. MDM and MAM with Micro VPN provide value-add in several areas, including Content Collaboration, Secure Mail, Microsoft Managed Intune Browser with XenMobile SDK, Citrix Gateway NAC. The CEM integration with Intune EMS wizards to facilitate Intune configuration, and it is important that the Secure Mail defaults to Citrix MAM container.

### Getting Started with integration

Refer to the [Citrix documentation](/en-us/citrix-endpoint-management/integration-intune-ems.html) for system requirements and pre-requisites to prepare the integration. Also, refer to this [guide](/en-us/citrix-endpoint-management/downloads/integration-intune-ems-getting-started-guide.pdf), which walks you through how to set up Citrix Endpoint Management integration with Microsoft Intune/EMS so you can deliver apps securely from Azure to any device.

### Summary of Intune Integration

Citrix and Microsoft have several joint innovations that provide flexible scenarios to deliver apps and data securely. Choosing the right scenario is the key to success here, and Citrix's common goal is to deliver a solution that meets the organization's requirements by providing the "best of" what Citrix and Microsoft can offer together.

CEM Integration with EMS provides a higher level of security with the per-app VPN option. It also provides security for data in motion and data at rest. It enables a fine-grained setup of policies for Mobile Device Management and Mobile Application Management. Seamless integration of all Office 365 apps with Citrix Secure Mail. It's a single pane of glass to manage different devices and platforms with a wide range of supported devices and has an enterprise app store for all your corporate apps.

## Android Enterprise with Citrix Endpoint Management

### Overview of Android Enterprise

Google announced device admin deprecation with its 2019 Android release. Device management using device admin permissions is considered a legacy management approach for Android devices. Android Enterprise is a modern management platform.

Android Enterprise is a set of tools and services provided by Google as an enterprise management solution for Android devices. The program offers APIs and other tools for developers to integrate support for Android into their Enterprise Mobility Management (EMM) solutions like Citrix Endpoint Management.

With Android Enterprise:

*  Customers can use Endpoint Management to manage company-owned Android devices and Bring Your Own (BYO) Android devices.
*  Customers can manage the entire device or a separate work profile on the device. The separate work profile isolates business accounts, apps, and data from personal accounts, apps, and data.
*  Customers can also manage devices dedicated to single-use, such as inventory management.

When Endpoint Management integrated with managed Google Play to use Android Enterprise in the organization is called enterprise. Google defines that an enterprise is a binding between the organization and the mobile management (EMM) solution. All the users and devices that the organization manages through the EMM solution belong to its enterprise. When  Endpoint Management integrates with Android Enterprise, the complete solution has these components:

*  Citrix Endpoint Management: The Citrix Endpoint Management is the unified endpoint management for a secure digital workspace. Endpoint Management provides the means for IT administrators to manage devices and apps for their organizations.
*  Citrix Secure Hub: The Citrix DPC app. Secure Hub is the launchpad for Endpoint Management. Secure Hub enforces policies on the device.
*  Managed Google Play: A Google enterprise app platform that integrates with Citrix Endpoint Management and its API sets app policies and distributes apps.

### Benefits of Android Enterprise with Citrix Endpoint Management

Whether corporate or employee-owned, Citrix Endpoint Management, and Android Enterprise deliver the controls that organizations need to protect their information while enabling user productivity. Citrix Endpoint Management supports each of the Android Enterprise management modes, including BYOD (Android work profile) and corporate profiles, including COPE(Company Owned/Personally Enabled), COBO(Company Owned/Business Only), COSU(Corporate Owned, Single Use) use cases. For BYOD users, Android Enterprise managed by Citrix Endpoint Management users get peace of mind and personal privacy while IT benefits from data security and compliance.

When administered by Citrix Endpoint Management, Android Enterprise provides flexibility in protecting company information.  Apply the multiple layers of Android security, including hardened security and Google Play Protection, and extend advanced device and app management controls from Citrix Endpoint Management.

For faster onboarding and enrollment, Citrix Endpoint Management supports the different provisioning options provided by Android Enterprise, including EMM token, zero-touch enrollment, NFC, and QR code. In addition to Android Enterprise managed by Citrix Endpoint Management, users get seamless access to their Android business apps through managed Google Play. When combined with Citrix Workspace, users also get access to all other apps, including virtual, SaaS, and web. Users also get more work done with Citrix mobile productivity apps, including Citrix Secure Mail and Citrix Content Collaboration with integrated workflows.

### Impact of device administration deprecation

Google announced to deprecate the following Device Administration APIs. These APIs won’t work on devices running Android Q after you upgrade Secure Hub to target the Android Q API level:

*  Disable camera: Controls access to device cameras.
*  Keyguard features: Control features that are related to the device lock, such as biometrics and patterns.
*  Expire password: Forces users to change their password after a configurable time.
*  Limit password: Sets restrictive password requirements.
*  The deprecated APIs have no impact on devices enrolled in Citrix MAM-only mode.

With the increased need for Android devices in the enterprise world and its growing use cases, Google introduced Android Enterprise with modern management modes – work profile, fully managed, and dedicated device. Refer to [Google developer documentation](https://developers.google.com/android/work/overview) for more details about the use-cases and profiles.

### Reference Architecture for Android Enterprise with Citrix Endpoint Management

[![Citrix-Endpoint-Management-Image-7](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_007.png)](/en-us/tech-zone/design/media/reference-architectures_citrix-endpoint-management_007.png)

To enroll a new customer through the EMM console, you need to create an enterprise. In an Android Enterprise deployment, an enterprise maintains control over various aspects of user devices, such as isolating work-related information from users' personal data, pre-configuring approved apps for the environment, or disabling device capabilities (for example, the camera). Refer to the [Google documentation](https://developer.android.com/work/dpc/build-dpc).

On the CEM server, you bind Citrix as your EMM partner for Android Enterprise (a 3-step process). CEM creates an Enterprise Service Account, which is used to manage data via Google Play APIs. The Google Play infrastructure offer services that include a managed, private enterprise app delivery store.

Once the integration is set up, Citrix Endpoint Management and managed Google Play work seamlessly together to secure, configure, and manage organizations’ Android devices and the required public or corporate apps.

An admin uses the EMM console to perform a range of tasks, including configuring device settings and apps. The DPC Secure Hub creates and manages the work profile on the device on which it is installed. The work profile encrypts work-related information and keeps it separate from users' personal apps and data. Before creating the work profile, the DPC can also provision a managed Google Play Account for use on the device.

In Android Enterprise for work profile or fully managed devices, users receive their apps via the Managed Google Play Store. EMM Admins approve public apps for use and can also add private apps on the Managed Google Play Store. The OrgID binding with Citrix Endpoint Management controls visibility of the private apps, which are approved through the Managed Google Play store, for devices enrolled with that organization.

The Secure Hub applies the device policies as set by an admin to meet an organization's requirements and constraints. For example, security policy might require that device lock after a certain number of failed password attempts. The DPC queries the EMM console for current policies then applies the policies

**Provisioning methods:**

### Fully Managed Device Provisioning Method

**QR code** — Android 9 devices and higher have a QR code reader built-in. For this method, the user simply turns on the device, taps the welcome screen six times, and scans the QR code, which automatically starts the enrollment-provisioning process by connecting to Google Play to access the management profile.

**Android zero-touch** — Using Android zero-touch enrollment, IT admins can create, edit, and delete UEM configurations. In doing so, devices or groups of devices can be shipped with the enrollment already complete. All the user needs to do is turn on the device, connect to Wi-Fi, and enter their password.

**EMM token** — With this method, a user’s IT department provides them with a token. For Citrix Endpoint Management, the token is afw#xenmobile. This token has to be entered after the new device turns on when the user is prompted for “Email or phone.” Entering the correct EMM token downloads the Citrix Endpoint Management device policy controller app so that the user can simply enter credentials to get set up.

**NFC Bump** — The NFC Bump method uses “Near Field Communication” to provision the device. Using NFC Bump, the new device must be nearby (4 centimeters) to another. Bulk enrollment of corporate-issued devices has always been a major headache for IT. With NFC Bump enrollment, IT enrolls a master device, carrying the MDM server details, and simply taps the device to other unenrolled devices to start the automated-enrollment process. Bulk enrollment made easy!

### BYOD Provisioning Method

In addition to the Work Managed options above, the BYOD method is popular for workers using a personally owned device. With this method, IT manages the business data (the Android work profile), leaving all the personal data and applications private. In other words, IT only has visibility and control of the work applications and nothing else. With this method, there is no device management, only mobile application management (MAM).

### Migrate from device administration to Android Enterprise

| Site  Details                                     | Default  Enrollment Profile                      | Comments/Recommendation                                      |
| ------------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| New  Site                                         | Android  Enterprise – Fully Managed/Work Profile | Any new sites default to Android Enterprise (AE). Recommendation: Set up AE if not already set up and enroll devices in AE, Device Admin is a legacy mode |
| Existing Site with Android Enterprise (AE) setup | Android Enterprise – Fully Managed/Work Profile | Any sites with AE configured defaults to Android Enterprise.   Recommendation:   a) If the site is AE with no Device Admin enrollment – no change required b) If  the site has Device Admin mode enrollment – make sure to update the Enrollment Profile for those devices to point to Legacy (device administrator) |
| Existing Site NOT setup with Android Enterprise  | Legacy (device administrator)                   | Sites without an Android Enterprise setup will default to Legacy (device  administrator). Recommendation: Set up Android Enterprise and plan migration |

Android Enterprise includes support for fully managed and work profile device modes. The Google publication, [Android Enterprise Migration Bluebook](https://static.googleusercontent.com/media/android.com/en/enterprise/static/2016/pdfs/enterprise/Android-Enterprise-Migration-Bluebook_2019.pdf), explains in detail about how legacy device administration and Android Enterprise differ. We recommend that you read the migration approach from Google. Also, refer to the [Android Enterprise Solution Directory](https://androidenterprisepartners.withgoogle.com/devices/) for a list of Android-recommended devices that meet the elevated enterprise requirements. And for more information, visit [Citrix’s Android Enterprise product page](https://www.citrix.com/products/citrix-endpoint-management/android-mdm.html).

### Sources

The goal of this reference architecture is to assist you with planning your own implementation. To make this job easier, we would like to provide you with source diagrams that you can adapt in your own detailed designs and implementation guides: [source diagrams](https://citrix.sharefile.com/d-s00fae3bac694bca9).

### References

[Reference Architecture for On-Premises Deployments](/en-us/xenmobile/server/advanced-concepts/xenmobile-deployment/reference-architecture-on-prem.html)

[Core Reference Architectures of CEM](/en-us/citrix-endpoint-management/about.html#architecture)

[CEM Product Document for EMS/Intune Integration](/en-us/citrix-endpoint-management/integration-intune-ems.html)

[Get Started with Intune Integration](/en-us/citrix-endpoint-management/downloads/integration-intune-ems-getting-started-guide.pdf)

[Android Enterprise Google Guide](https://developers.google.com/android/work/overview)

[Google document on build DPC](https://developer.android.com/work/dpc/build-dpc)

[Android Enterprise Migration Bluebook](https://static.googleusercontent.com/media/android.com/en/enterprise/static/2016/pdfs/enterprise/Android-Enterprise-Migration-Bluebook_2019.pdf)

[Android Enterprise Solution Directory](https://androidenterprisepartners.withgoogle.com/devices/)

[Citrix’s Android Enterprise product document](/en-us/citrix-endpoint-management/device-management/android/android-enterprise.html)
