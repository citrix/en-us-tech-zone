---
layout: doc
h3InToc: true
contributedBy: Frank Srp
specialThanksTo: Eric Beiers, Martin Zugec, Daniel Feller
description: Learn how to set up Citrix Secure Internet Access in conjuntion with Citrix Secure Workspace Access to provide Secure access to all applications, anywhere, from any device.
---
# Proof of Concept: Citrix Secure Internet Access with Citrix Secure Workspace Access

## Overview

Citrix Secure Internet Access provides a full cloud-delivered security stack to protect users, apps, and data against all threats without compromising the employee experience. This proof of concept (PoC) guide is designed to help you quickly configure Citrix Secure Internet Access within your Citrix Cloud environment. At the end of this PoC guide you will be able to protect your Citrix Secure Workspace Access deployment with Citrix Secure Internet Access. You will be able to allow your users access applications using Direct Internet Access (DIA) without compromising on performance.

![Overview](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-swa_overview.png)

## Scope

In this Proof-of-Concept guide, you will experience the role of a Citrix administrator and you will create a connection between your organization’s Secure Workspace Access deployment and Citrix Secure Internet Access for Corporately Owned Devices.

This guide showcases how to perform the following actions:

*  Logging into Citrix Secure Internet Access (CSIA)
*  Interfacing CSIA Security Groups with Groups from Domain Integration
*  Configuring the Proxy settings of the CSIA cloud platform
*  Configure web security policies to allow/deny access to certain websites or categories via CSIA Console.
*  Applying Policies to Security Groups
*  Download the CSIA agent (Cloud Connector)
*  Configure the CSIA agent (Cloud Connector) via Agent Policy
*  Configure the CSIA agent (Cloud Connector) Manually (OPTIONAL - Windows Only)
*  Deploy CSIA agent (Cloud Connector) via AD group policy to Corporate Windows Devices
*  Deploy CSIA agent (Cloud Connector) via Endpoint Management Solution to Corporate Devices
*  Deploy CSIA agent (Cloud Connector) manually to Corporate Devices

## Prerequisites

*  Microsoft Windows 7, 8, 10 (x86, x64, and ARM64)
*  Microsoft Server 2008R2, 2012, 2016, 2019
*  Microsoft .NET 4.5, or above
*  PowerShell 7 (only for Windows 7)
*  The following Firewall rules

### Network Requirements

#### Port / Firewall settings

**CSIA --> Outbound**
| Protocol  | Port  | Description  | Destination  |
|--- |--- |--- |--- |
| TCP  | 80  | Proxy connections<br>Custom block pages  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 443  | PAC script retrieval over HTTPS | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 8009  | Alternative port for proxy connections  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 8015  | Proxy authentication over HTTP  | ibosscloud.com  ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 8016  | Alternative port for proxy authentication  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 8080  | Default block page  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
| TCP  | 10080  | PAC script retrieval over HTTP  | ibosscloud.com<br>api.ibosscloud.com<br>accounts.iboss.com<br>**Customer CSIA Node**-reports.ibosscloud.com<br>**Customer CSIA Node**-swg.ibosscloud.com  |
## Citrix Secure Internet Access (CSIA) Cloud Configuration

In this section we will focus on the configuration of CSIA within the administration console.

### Log into Citrix Secure Internet Access

1.  Log into Citrix Cloud and Access the Secure Internet Access tile
    ![Log into Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-swa_2.png)
1.  Select the **Configuration** tab and Click **Open Citrix SIA Configuration** to access the Configuration Console.
    ![Citrix SIA Configuration](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-swa_3.png)

### Configure the Citrix Secure Internet Access PAC Settings

1.  From the **Configuration** tab navigate to **Locations & Geomapping**.
2.  On the **Zones** tab click on **Edit Default Zone**.
3.  Click on **PAC Settings**.
4.  If you need to bypass a domain, use the **Add a Function**.
5.  These are the recommended Citrix Domain and Sub-domains to be added to the PAC File:  
⋅ cloud.com & \*.cloud.com  
⋅ citrixdata.com &*.citrixdata.com  
⋅ citrixworkspaceapi.net & \*.citrixworkspaceapi.net  
⋅ citrixworkspacesapi.net & \*.citrixworkspacesapi.net  
⋅ citrixnetworkapi.net & \*.citrixnetworkapi.net  
⋅ nssvc.net & \*.nssvc.net  
⋅ xendesktop.net & \*.xendesktop.net  
⋅ cloudapp.net & \*.cloudapp.net  
⋅ netscalergateway.net & \*.netscalergateway.net  
6.  Note the node shown "**node-clusterxxxxxx-swg.ibosscloud.com:80**". This should match the customer’s SWG node in Node Collection Management

### Interfacing CSIA Security Groups with Groups from Domain Integration

Users connecting to Citrix Secure Internet Access (CSIA) will also communicate domain OU information if it is available from their current user login and device. The CSIA cloud platform can be used to match the groups provided by domain-controlled user accounts. Correlating the groups of your domain integration with the security groups contained on the CSIA cloud platform allows you to administer policies and restrictions in a manner similar to the existing security policies within your organization.

The strategy for integrating domain group information into the CSIA cloud platform is to edit security groups to match the aliases of domain groups reported to the platform.

#### Integration Example

To demonstrate the execution of this concept, let's map the domain credentials of a Windows user into a security group on the CSIA cloud platform.

1.  Open command prompt on the target computer, and execute the command "net user (username) /domain"  
2.  Gather the aliases of groups reported back by the domain controller.  
3.  Go to the CSIA cloud platform and edit either the **Group Name** or the **Alias Name** to correspond to one of the groups reported by the domain user.  
4.  Now when users from the integrated domain group authenticate to the CSIA cloud platform, they will be assigned automatically to their corresponding security group.

### Configuring the Proxy settings of the CSIA cloud platform

1.  Navigate to the **Proxy & Caching** module.
2.  Set **Enable Proxy Settings** to **YES**.
3.  Set **User Authentication Method** to **Local User Credentials + Cloud Connections**.
4.  Click **Save**.

## Configure Web Security Policies

In this section we will focus on the configuration of CSIA Web Security Policies within the administration console. This is the main place where we set actions on web categories.

### Applying Policies to Security Groups

1.  Navigate to the **Web Security** module.
2.  For web security policies that have a group-based implementation available a drop-down will be present at the top of the page above the configuration form for the policy. The current status of this drop-down menu will indicate the which group's policy configuration you are currently viewing.
3.  Click the **Group** drop-down menu and then **select group** that you want to reconfigure for the current web security policy.
4.  Clicking **Save** will save the current policy's configuration only to the currently selected security group from the **Group** drop-down menu.  
   ⋅ **_Note:_ If you are looking to apply these settings to multiple groups**  
Click **Save to Multiple Groups** will open a window that will allow you to assign the current policy's configuration to multiple security groups simultaneously.  
Click the **corresponding checkbox** for any security group you want to apply the current policy configuration.  
Click **Add** to add the selected security groups to the configure group.  
Groups can either be reconfigured to accept only configuration changes that have currently been applied or accept and overwrite all configuration settings for the current policy.  
The default selection is **Apply Changed Settings** which will only apply changes you have configured. Select to overwrite all configured settings for the designated security groups.  
Individual groups can be removed by clicking **Remove** or **remove all groups** selected for configuration by clicking **Remove All**. Click **Save** to confirm the configuration being applied to all designated security groups.

### Web Categories

Domains are tagged with web categories based on their content. The categories of visited websites are recorded in Reporting & Analytics.

#### Category Actions

You can associate actions with web categories. This enables you to deploy security policies quickly while minimizing the need to allow or block individual URLs. Possible actions include Allow, Block, Stealth, Soft Override, or SSL Decryption Enabled.

1.  Each category can have actions that are applied independently from the other categories.

| Action  | Description  |
|--- |--- |
| Allow  | Allows users to access sites of this category. Allow is the default state for all categories. |
| Block  | Blocks access to sites of a particular category. |
| Stealth  | Flags traffic to that category as violations in the logs but will still allow access to the site.  |
| Soft Override  | Presents a block page to the user but includes an option to bypass the block temporarily. This allows users to access blocked content without requiring immediate administrator intervention.Soft overrides last until the next day at 2:00 AM in the time zone configured for the gateway. Any block page presented with a soft override will appear in Reporting & Analytics as "soft-blocked." After a user has requested a soft override, any traffic to that URL will show as "allowed" until the override has expired.   |
| SSL Decryption  | Disable this option to turn off SSL Decryption for a specific category. This ensures privacy compliance and concerns with specific categories like Finance or Health. The priority value for this category does NOT apply to this setting. If a site belongs to multiple categories, and any of those categories has the "SSL Decryption Disabled" option on, that site will not be decrypted.  |
| Lock  | When a category is locked by the primary administrator into either an Allowed or Blocked state, delegated administrators cannot log in to the web gateway management interface and change the status of that category.  |
| Category Override  | When you activate a category override, a delegated administrator cannot log in to the web gateway management interface and add a URL to the allow list that would contradict the rule for this category. For example, the "Art" category is blocked and set to "Overrides," so delegated administrators cannot add art.com to the Allow List for that group.  |

2.  Click each icon to toggle the action for the respective web category.
3.  Actions that you want to generally apply to all web categories can be implemented with the Actions drop-down menu.

#### Category Priorities

If a domain is associated with multiple categories, the action is determined by comparing the priority values for the categories. Categories with higher priority value take precedence.

Example:

1.  A domain is categorized as both government and hacking. In the web categories configuration, the government is allowed with a weight of 100 while hacking is blocked with a weight of 200.
2.  This domain would be blocked when visited by a user due to the block action of the hacking web category possessing a higher priority value.
3.  Click the priority value field and enter a numerical value to reconfigure a category's priority level. The priority value has a configurable range of 0-65535.

#### Additional Settings

Settings relevant to web categories are configured with toggles under **Additional Settings**. Configuring a toggle to **Yes** will enable the setting while configuring a toggle to **No** will disable the setting. For a full set of descriptions for all available web category settings refer to the following table:
| Feature  | Description  |
|---  |---  |
| Enable Logging  | Enable and disable logging of violation attempts for the current set of blocked website categories. Log reports may be viewed on the CSIA Reports page. The report information includes date, time, user, website address, and category of the violation.  |
| Enable Stealth Mode  | It allows you to stealthily monitor Internet activity without blocking access to forbidden sites. With both Logging and Stealth Mode enabled, you can monitor Internet web surfing activity by viewing the log reports on the CSIA Reports page while remaining unnoticed by Internet users on the network.  Note: Web sites and online applications will not be blocked when the action for the web category is configured to Stealth Mode.  |
| Enable HTTP Scanning on Non-Standard Ports  | If this feature is enabled, CSIA will scan for HTTP web requests on non-standard ports.  |
| Allow Legacy HTTP 1.0 Requests  | If this feature is enabled, CSIA will allow HTTP 1.0 requests that are missing the "HOST" header. Disabling this feature provides a higher level of Security and makes bypassing the Security more difficult. If this feature is enabled, it may offer more compatibility with older non-HTTP 1.1 compliant software.  |
| Enable ID Theft / IP Address URL Blocking  | Protects against potential identity theft attempts by notifying you when someone is trying to steal your personal information through Internet Phishing. Enabling this feature will also block users from navigating to websites using IP address URLs.  |
| Enable Blocked Site Override  | Enabling this feature will activate a soft override action, allowing users to proceed to the site belonging to a blocked category. When this setting is active, the user will be warned that a page is blocked but will provide a button to proceed anyway.  |
| Auto Categorize Uncategorized Sites  | When this toggle is switched to Yes, any sites that are not categorized are automatically submitted for categorization. Note: When this toggle is set to Yes, uncategorized sites may be assigned a Web Category of "Informational" or other such designation. This will only be in effect while the category is being appropriately categorized. When this switch is set to No, uncategorized are automatically designated as "Not Rated," which is controlled as its own Web Category.  |

#### Category Scheduling

Block events can also be configured on an advanced weekly schedule to allow access during particular times.

1.  Set Category Scheduling to **Allow Selected Categories Using an Advanced Schedule** to enable the scheduling feature. Click **Advanced Scheduling** to begin scheduling blocked categories.
2.  The current advanced schedule can either be configured to apply to all blocked categories or only to a particular blocked category.
3.  Each day during the week can be delegated specific periods of time that the category will be allowed. These particular periods of time are indicated by a blue rectangle.
4.  After finalizing the schedule for a particular category, you can click the category drop-down menu once again and select a new category to configure.
5.  Click **Save** to confirm all schedule configurations.

### Allow List

The allow lists selectively provides access to a specific website or network resource. This feature enables you to override security policies and allow certain users to access a website. In particular, you can use this to grant access to specific URLs for a domain that is otherwise blocked. This is sometimes referred to as "punching a hole."

#### Adding a URL to an Allow List

1.  Navigate to **Web Security Policies**.
2.  On the drop down click on **Allow List**.
3.  From above the list section, click **URL/IP Range**.
4.  Type a **domain, subdomain, URL, IP address, or IP Range**. This is the only required field, but many other criteria can be specified.
5.  From the right side of the list, click **+Add**. The entry is now added to the list.

#### Scrape Tool

The Allow List section includes the handy Scrape Tool. Use this to quickly identify all of the domains that a website uses. In the modern web, many sites use resources from other domains that are not immediately apparent. With the Scrape Tool, these are easily revealed and added to an allow list.

Another use for the Scrape Tool is to selectively only allow portions of a website, while not allowing unwanted content, such as ad servers.

1.  From above the list section, click **Scrape**.
2.  Enter the URL to Scrape and click **Scan**.
3.  Select domains to add to Allow List and click Add Selected to Allow List.

### Keyword Block List / Allow List

Keyword filtering is used to inspect URLs for specific words. If a keyword is identified, the content can be allowed or blocked.

#### Adding a URL to a Block List / Allow List

1.  Navigate to **Web Security Policies**.
2.  On the drop down click on **Keywords**.
3.  Enter the **keyword** that you would like to block in the **Keyword** field and specify the designations that will apply to this keyword. Click **Add**.

#### Pre-defined Keyword Lists

You can enable filtering for pre-defined lists of Adult and High-Risk keywords or add more keywords manually.

The Pre-defined Keyword Lists contain common Adult and High-Risk keywords. Wildcard matching is applied to all keywords in these lists. A wildcard match will recognize the keyword's sequence of characters anywhere in the URL, including the hostname. The High-Risk list generates an email to the recipient of alert emails when a High-Risk keyword is detected by the Reporting & Analytics functionality of the CSIA cloud platform.

1.  To enable either one of the keyword lists, set the Adult or High-Risk toggle to **Yes**. Click **Save**.

## Citrix Secure Internet Access Agent Configuration

In this section we will focus on the configuration and the installation of the Citrix Secure Internet Access (CSIA) Agent.

### Configure the CSIA Agent Download (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors**.
2.  Click **Configure Connector Download**.
3.  Click the **Use HTTP PAC** dropdown menu and select **No**.  
(**Note:** Use HTTP PAC to **"No"** if you want to use **_HTTPS_** for PAC download)
4.  Click **Security Group** and select the desired default security group for this particular installation file download.
5.  Keep the remaining connector download settings as **Default** and Click **Save**.

### Configure the CSIA Agent Advanced Connector Settings (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors > Advanced Connector Settings**.
2.  Under Global Settings Enable the below:  
⋅ **Enable Security Cloud Connector Filtering**  
⋅ **Configure Auto Login Cloud Connectors to use Key for Group**  
⋅ **Use Session Encryption**
3.  Under Source IP Logging Enable - **Use private source IP of client (if available)**.
4.  Under Group Specific Settings verify that the **Correct Group** is selected and Click **Save**.

### Download the Citrix Secure Internet Access Agent (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors > Download Connectors**.
2.  Under Windows Cloud Connector, click **Download** and **Download All**.  
The .msi installation packages can be downloaded directly from the CSIA admin console. Before starting the installation, be sure that you are using the correct package for the version of Windows and processor architecture.

### Configure the CSIA Agent Policies in the CSIA Admin Console

1.  From the CSIA admin console, go to **Connect Device to Cloud > Agent Policies**.
2.  Click **Add Agent Policies**.
3.  Provide a **Name** for your Agent Policy and Select **Add Agent Policy**.
4.  To configure the policy, click **Edit Agent Policy**.
5.  Click on **Agent Settings** and set the recommended settings.

#### Recommended Settings for Corporate Owned Devices

| Setting  | Recommended Value  |
|---  |---  |
| Multi-User Mode:  | Disable Multi-User/Terminal Server Mode - Enable to support multiple user sessions when running a terminal server. This should be enabled for terminal servers even if users are not logged in simultaneously.  |
| Use Machine Name for Username:  | Disable Setting - Use the user account name as the username.  |
| Use UPN for Username:   | Disable Setting - Use the Security Account Manager (SAM) account name, such as DOMAIN\username.  |
| Redirect All Ports:  | Enable Setting  |
| Bypass Private Subnets:  | Enable Setting  |
| Captive Portal Detection:  | Disable Setting  |
| Auto-Update Enabled:  | Enable Setting  |
| Auto-Update Release Level:  | Level 1 - Mature  |
| Enable Windows Desktop App: (Agent will not run-on Multi-User Deployments)  | Enable Setting  |
| Allow End Users to Disable Security:  | Disable Setting  |
| Require Password to Disable Security:  | Enable Setting  |
| Require Password to View Diag Info:  | Enable Setting  |

6.  Click on **Dynamic Linking** and select the Groups you wish to assign the policy too.
7.  Click **Save**.

### **(OPTIONAL)** Manual Configuration of the of the Citrix Secure Internet Access Agent

The CSIA Agent for Windows .msi editing via Orca is only recommended for troubleshooting.

Orca.msi is available in the Windows Cloud Connector “Download All” option

#### Opening an .MSI File with Orca

1.  Open Zip file and install Orca.msi
2.  Locate the desired installation file in a file explorer program.
3.  Right-click the .msi installation file.
4.  Click **Edit with Orca**.

#### Configuring Properties of an .MSI in Orca

1.  Double-click to open the **Property table**.
2.  Each property can be edited by double-clicking the property's **Value** field.

#### Recommended Settings for Corporate Owned Devices

| Setting  | Recommended Value  |
|---  |---  |
| Multi-User Mode: (PARAM_MULTI_USER_SUPPORT)  | (0): Disable multi-user mode.  Enable to support multiple user sessions when running a terminal server. This should be enabled for terminal servers even if users are not logged in simultaneously.  |
| Terminal Server Mode: (PARAM_TERMINAL_SERVER_MODE)  | (0): Disabled - this appears to be deprecated in favor of Multi-User Support  |
| Use Machine Name for Username: (PARAM_USE_MACHINE_NAME_FOR_USERNAME)  | (0): Disabled - Use the user account name as the username.  |
| Redirect All Ports: (PARAM_REDIRECT_ALL_PORTS)  | (1): Enabled - Redirect all ports to the proxy.  |
| Bypass Private Subnets: (PARAM_BYPASS_PRIVATE_SUBNETS)  | (1): Enable bypass  |
| Captive Portal Detection: (PARAM_CAPTIVE_PORTAL_DETECTION)  | (0): Disabled  |
| Auto-Update Enabled: (PARAM_AUTO_UPDATE_ENABLE)  | (1): Enabled – The cloud connector to be updated automatically.  |
| Restart After Upgrade: (PARAM_RESTART_AFTER_UPGRADE)  | (0): Disabled - Will not prompt a restart.    |

3.  Within Orca, click the **Save** icon to save changes made to the parameters of the Windows Cloud Connector.  
**Note:** _Ensure files are saved within Orca only using this method **(not using Save As)**. This will cause issues with the functionality of the Windows cloud connector if it is not saved in this manner._

## Citrix Secure Internet Access Agent (Cloud Connector) Deployment

Citrix recommends that for a CSIA PoC you only install the SIA Agent to Corporate Devices.

### Deploy CSIA agent via AD group policy to Corporate Windows Devices

#### Create a Distribution Point

1.  **Create folder** on an AD joined computer that will act as the file server.
2.  **Save** the SIA Agent .msi package inside that folder.
3.  **Right Click** the newly created folder, **select properties**.
4.  Go to **“Sharing”** tab.
5.  Select **“Advanced Sharing”**.
6.  Enable **“Share this Folder”**.
7.  In Settings under Share name add a **$** after the SIA Agent folder name. (Example. SIA_Agent$).
8.  Select **Apply**.
9.  Then **Close**.

#### Create Group Policy Object

1.  **Open** Group Policy Management.
2.  **Right click** Group Policy Object.
3.  Select **New**, and name new GPO (Example: Deploy SIA Agent).
4.  **Right click** the newly created Group Policy Object from above, Select Edit.
5.  **Expand** Software Settings folder.
6.  Select **software installation**.
7.  **Right click** in the right panel.
8.  Select **New**.
9.  Select **Package**.
10.  In the corresponding window, type in the location (file path) of your SIA Agent folder containing the SIA Agent .msi package.
11.  Select the **MSI**.
12.  Select **Open**.
13.  In the Deploy Software window, choose **“Advanced”** underneath Select Deployment Method.
14.  Under Deployment select **Uninstall This Application when…**. (this means next time the endpoint runs a gpupdate and the SIA Agent has been removed it will be removed from the endpoint as well).
15.  Select **OK** and **Close**.

### Deploy CSIA agent via Citrix Endpoint Management Solution to Corporate Devices

#### Installing the Windows Agent

1.  In the Endpoint Management console, navigate to **Configure > Apps**. Click **Add**.
2.  Click **Enterprise**.
3.  On the App information page, configure the following:  
⋅ **Name:** Type a descriptive name for the app. The name appears under App Name on the Apps table.  
⋅ **Description:** Type an optional description of the app.  
⋅ **App category:** Optionally, in the list, click the category to which you want to add the app.
4.  Click **Next**. The **App Platforms** page appears.
5.  Select the platform: **Windows Desktop/Tablet**.
6.  On the Windows Desktop/Tablet Enterprise App page, click **Upload** and navigate to the file.
7.  Configure these settings:
8.  Specify **deployment rules** and **store configuration** as needed.
9.  Click **Next** until you get to the Summary page and then click **Save**.
10.  In the Endpoint Management console, navigate to **Configure > Delivery Groups**. Select the delivery group to configure and click the **Apps page**.
11.  Drag the desired apps to the **Required Apps** box.
12.  On the Summary page, Click **Save**.

#### For Deploying the CSIA Agent via a 3rd Party UEM

See your Unified Endpoint Management Documentation for deploying .MSI file.

#### Installing the macOS Agent

1.  In the Endpoint Management console, navigate to **Configure > Apps**. Click **Add**.
2.  Click **Enterprise**.
3.  On the App information page, configure the following:  
⋅ **Name:** Type a descriptive name for the app. The name appears under App Name on the Apps table.  
⋅ **Description:** Type an optional description of the app.  
⋅ **App category:** Optionally, in the list, click the category to which you want to add the app.
4.  Click **Next**. The **App Platforms** page appears.
5.  Select the platform: **macOS**.
6.  **Upload** the PKG file (macOS) and complete the configuration. Click **Next**.
7.  Click **Next** until you get to the Summary page and then click Save.
8.  In the Endpoint Management console, navigate to **Configure > Delivery Groups**. Select the delivery group to configure and click the **Apps page**.
9.  Drag the desired apps to the **Required Apps** box.
10.  On the Summary page, Click **Save**.

#### For Deploying the CSIA Agent via a 3rd Party UEM

See your Unified Endpoint Management Documentation for deploying .PKG file.

### Deploy CSIA agent via manually to Corporate Devices

#### Installing the Windows Agent

1.  **Power on** the Windows Machine and log on.
2.  **Install** the appropriate SIA Agent .msi package for your platform.

#### Installing the macOS Agent

1.  Navigate to and open the downloaded archive in Finder.
2.  Open the installation package file.
3.  Complete the installation process.

## Validated Use Cases

### CSIA Website Filtering via the CSIA Agent

#### Citrix Workspace App Launching a

1.  SaaS app without enhanced security enabled is protected via the CSIA Policies
2.  SaaS app with enhanced security enabled is protected via the CSIA Policies
3.  Internal Web app without enhanced security enabled is protected via the CSIA Policies
4.  Internal Web app with enhanced security enabled is protected via the CSIA Policies

#### Citrix Workspace HTML Launching a

1.  SaaS app without enhanced security enabled is protected via the CSIA Policies
2.  SaaS app with enhanced security enabled is NOT protected via the CSIA Policies
3.  Internal Web app without enhanced security enabled is protected via the CSIA Policies
4.  Internal Web app with enhanced security enabled is NOT protected via the CSIA Policies

#### Citrix Workspace via Browser Extension

1.  SaaS app without enhanced security enabled is protected via the SIA Policies
2.  SaaS app with enhanced security enabled is NOT protected via the SIA Policies
3.  Internal Web app without enhanced security enabled is protected via the SIA Policies
4.  Internal Web app with enhanced security enabled is NOT protected via the SIA Policies

## Troubleshooting

### Important Tools for Troubleshooting

1.  Windows Event Log
2.  CSIA Real-Time Dashboard **(Reporting & Analytics > Real-Time Log)**
3.  CSIA Event Logs **(Reporting & Analytics > Logs > Event Log)**
4.  CSIA IPS Logs **(Reporting & Analytics > Logs > IPS Log)**
5.  Registration Information for Connected Devices **(Users, Groups & Devices > Cloud Connected Device > Info)**
6.  URL Lookup Tool **(Tools > URL Lookup)**
7.  Enhanced Logging  
 ⋅ To set this, the following registry key needs to be altered, varying from 0 to 4, the higher giving more verbose logging.  
 ⋅  **HKEY_LOCAL_MACHINE\SOFTWARE\IBoss\IBSA\Parameters\LogLevel**  
⋅ Once the registry key has been set, the IBSA service under Windows Services will need to be restarted for the setting to take effect. Checking windows event viewer, you will see many entries being logged depending log level set.
8.  Windows Agent Logs (C:\Windows\SysWOW64\ibsa_0.log)

### A Keyword is not Being Blocked

There is a multitude of policies and variables within and surrounding the operations of the CSIA cloud platform that can interfere with properly blocking configured keywords. Refer to the following;

1.  Ensure that SSL decryption is active for this website. Keywords cannot be observed or controlled for HTTPS websites.
2.  If the source IP of the client workstation or the destination IP of the webserver has been added to the Network > Bypass IP Ranges list, web security controls will not be enforced.
3.  Keywords configured to block will not take effect if the website is added to the Web Security > Allow List without the Keyword / Safesearch option enabled. With the Keyword / SafeSearch checkbox selected, Web Gateway will allow access to the website but still to enforce Keyword controls and Safesearch.
4.  The keyword contains asterisks, instead remove the asterisks and activate Wildcard matching for the keyword if that is the desired effect.
5.  The keyword has multiple words, and Wildcard Matching is not enabled, or the spaces were not indicated with a plus sign ("+").
6.  The user is not associated with the expected web security group that has the keyword control enabled.

### Conflicting Actions from Pre-defined Keyword Lists

In some situations, a word in one of the built-in lists of keywords may inadvertently block content unexpectedly. To correct this action, edit the specific pre-defined keyword list. When you click the pencil icon to edit a built-in list, the CSIA cloud platform interface will present a page of keywords with checkboxes next to them. To remove the keyword from the built-in list, uncheck the box next to the keyword you would like to remove and click **Save**. To apply this action to all groups, check the box that is labeled **Apply to All Groups**.

### Identifying the Customer’s Citrix Secure Internet Access Node

1.  From the **Home** navigate to the **Node Collection Management**.
2.  Click on **Node Groups**.
3.  This will provide you with both the **_Customer SIA Node_**-reports.ibosscloud.com and the **_Customer SIA Node_**-swg.ibosscloud.com Node Clusters.

### Splunk Integration with Citrix Secure Internet Access Node

#### Splunk Server Setup

1.  Navigate to the Splunk Server instance and click on the **Settings** link at the top of the page, followed by the **Data inputs** link under the “Data” subsection.
2.  Click the Add new link to the right of the “UDP” section.
3.  Enter a port into the "port" field (above 1023, if possible, to avoid security restrictions with the operating system). In the “Only accept connection from” field, enter your CSIA Reporter Node's IP address. If nothing is entered in this field, connections from all hosts will be accepted. When done, click **Next**.
4.  On the next page, click **Select Source Type** and type **"syslog,"** then select it.
5.  Change the App Context to **Search & Reporting (search)**.
6.  Change the Host Method to **IP**.
7.  Click **Review** (review current configuration), then click **Submit**.

#### CSIA Reporting & Analytics Module Setup

1.  Navigate to **Reporting & Analytics > Log Forwarding > Forward From Reporter** in the iboss cloud platform interface.
2.  Under **Splunk Integration**, click **Actions**, then click **Add Server**.
3.  Add the Address/Hostname of the Splunk Server to the “Hostname” and the port number chosen on the Splunk server. Next, in the dropdown box “Splunk Integration Protocol,” choose a protocol.
The options available when adding a Splunk server appear as follows:
4.  You can also configure the Splunk server's integration protocol as HEC. Configuring integration with a Splunk server using the HEC protocol requires the acquisition of the HEC token from the configuration of the Splunk server. Place the retrieved token into the Token field below the Splunk Integration Protocol selection drop-down menu.
5.  If implementing an **ELFF** log format for Splunk logging the **Splunk Integration ELFF Batch Size** field will become available for configuration. The default value for configuration is **100**.
6.  A toggle called “Accept All SSL Certs” is available under the "Splunk Integration" section within **Settings > External Logging**; if a non-standard SSL Certificate such as a Self-Signed certificate or a certificate signed by a non-trusted root CA is used, switch this toggle to "YES" to bypass SSL certificate verification, otherwise leave the switch off.
7.  Select the format in which the log data will be delivered from the "Log Format" drop-down. Finally, switch one or more of the toggles at the bottom of the interface to select the desired logging information types. Click the **Save** button to update the changes. The logging will begin immediately. Perform a search on the Splunk instance to check data is being sent and indexed properly. See the sample output below.

### Changing the Proxy Port

The CSIA proxy port may be changed, but you mustn't attempt to change the proxy port to one that the gateway uses for other services.

Ports that should **not** be used include: 53, 139, 199, 443, 445, 953, 1080, 1344, 5432, 6001, 7009, 7080, 7443, 8008 ,8015, 8016, 8025, 8026, 8035, 8036, 8080, 8200, 8201, 9080, 9443, 17500, 22022

_**All other ports are an acceptable alternative to the default port.**_

1.  Navigate to **Proxy & Caching > Proxy Settings**
2.  Under the **Settings tab**, enter the desired port number on which the proxy will listen for traffic into the Proxy Port field.  
**Note:** The port configured for this setting will be used when configuring proxy settings in other platform functionalities. Some ports may not be available for assignment to this setting due to pre-configured gateway services.
3.  Click **Save** to apply this change.

## Appendix

### Main Category List

_**Note:** Be careful with the “Not Rated” category, as it will match against many sites that are not categorized._

| Category  | Description  |
|---  |---  |
| Abortion* | Sites related to abortion |
| Ads | Sites used to distribute advertising graphics or content as well as online coupons, advertising sales, voucher, deals, and offers |
| Adult Content | Sites that contain adult-oriented material. Sites in this category do not contain any nudity but do feature profane and vulgar content. Sites that self-identify as being inappropriate for those under 18 falls under this category. |
| Alcohol/Tobacco | Sites that contain alcohol and tobacco content. Also includes sites related to alcohol such as bars. Sites in this category do not contain illegal drugs, but may discuss, encourage, promote, offer, sell, supply, or otherwise advocate the use or creation of alcohol/tobacco. |
| Art | Sites that contain art or discuss art, including museums. May also include printable coloring books, sculptures, mosaic, tattoos, calligraphy, fonts, painting, graffiti, Christian or religious designs, animation drawing, artistic design|
| Auctions | Sites related to auctions and bidding on goods and services, both online and live |
| Audio & Video | Sites that contain streaming or downloadable audio/video content such as mp3s, movie clips, and TV shows, as well as sites that sell this content. |
| Business | Sites that represent a business's online presence. May be engaged in commerce or the activity of buying and selling products and services between the company and consumer. Includes Manufacturers, Producers, Suppliers, Dealers, Distributors, Wholesalers, Retailers, Family-owned businesses, and any other business-oriented entities. |
| CDN* | Content Delivery Networks (CDNs) and sites related to CDNs |
| Dating & Personals | Sites that offer dating services or aid in the establishment of romantic relationships |
| Dictionary | Sites containing extensive collections of information and knowledge. Includes resources such as wikis, lexical dictionaries, maps, censuses, almanacs, library catalogs, genealogy-related sites, scientific information, and directories as well as utilities like clocks, calculators, timers, and templates. |
| Drugs | Sites containing content relating to illegal drugs such as Amphetamines, Barbiturates, Benzodiazepines, Cocaine, Designer Drugs, Ecstasy, Heroin, etc. Does not refer to Cannabis/Marijuana |
| Drugs - Controlled* | Sites related to controlled drugs and substances |
| Dynamic DNS* | Sites that utilize dynamic DNS services to map their domain names to dynamic IP addresses. |
| Education | Sites that provide educational services such as schools and universities, as well as sites that offer educational materials for sale or reference. Includes websites that offer information on education or trade/vocational/career schools and programs. Also includes sites that are sponsored by schools, educational facilities, faculty, or alumni groups. |
| Entertainment | Sites that contain or promote television, movies, magazines, radio, books, food, fashion, and lifestyle. More specifically, sites that provide information about or promote popular culture including (but not limited to) film, film critiques, and discussions, film trailers, box office, television, home entertainment, music, comics, graphic novels, literary news, and reviews, as well as other entertainment-oriented periodicals, interviews, fan clubs, celebrity gossip, podcasts, and music and film charts, show, events, quotes, memes, lyrics, musicians, bands, theatre arts, drama, opera, orchestra. |
| Extreme* | Sites containing intensely vulgar, graphic, shocking, or disgusting content that would be considered highly offensive to most individuals. |
| File Sharing | Sites for services that provide online file storage, file sharing, synchronization of files between devices, and/or network-based data backup and restoration. These services may provide the means to upload, download, paste, organize, post, and share documents, files, computer code, text, non-copyright-restricted videos, music, and other electronically formatted information in virtual data storage. Additionally, this category covers services that distribute software to facilitate the direct exchange of files between users. |
| Finance | Sites that contain content about banking, financial news and tips, the stock market, investing, credit cards, insurance, and lending. |
| Food | Sites that contain content related to restaurants, food, dining, as well as sites that list, review, discuss, advertise and promote food, catering, dining services, cooking, and recipes. |
| Forums | Sites containing message boards, online chat rooms, and discussion forums |
| Freeware / Shareware* | Sites related to distributing freeware and shareware software |
| Friendship | Sites that contain platonic friendship related materials and social networking sites. |
| Gambling | Sites that promote or contain gambling-related content such as online poker and casinos, sports betting, and lotteries. Sites where a user can place bets or participate in betting pools, lotteries, or receive information, assistance, recommendations, or training in such activities. This category does not include sites that sell gambling-related products or sites for offline casinos/hotels unless they meet one of the above requirements. |
| Games | Sites that contain online games, or provide services and information about electronic games, household video games and consoles, computer games, and role-playing games. Also includes game guides/cheats, and accessories |
| Government | Sites sponsored by or representing government agencies, including military and political organizations. May provide information on taxation, emergency services, and laws of various governmental entities. Also includes sites that provide adoption services, information about adoption, immigration information, and immigration services. |
| Guns & Weapons | Sites that promote, sell, or provide information regarding firearms, knives, and other weapons |
| Hacking* | Sites related to hacking and hacking tools |
| Health | Sites that contain content related to health, illnesses, and ailments, including hospitals, doctors, and prescription drugs, including sites primarily focusing on health research. Also, sites that provide advice and information on general health such as fitness and well-being, personal health, medical services, over-the-counter and prescription medications, health effects of both legal and illegal drug use, alternative and complementary therapies, dentistry, optometry, and psychiatry. Additionally, includes self-help and support organizations dedicated to a disease or health conditions. |
| Humor* | Sites primarily related to jokes, humor, or comedy |
| Illegal Activity* | Sites related to illicit activities or activities illegal in most countries |
| Image/Video Search | Sites for image and video searching, including sharing of media (e.g., photo sharing) and have a low risk of including objectionable content such as adult or pornographic material. |
| Informational | Sites containing informational content such as regional information or advice |
| Infosec* | Sites featuring content related to Information Security |
| Internet Communication | Sites Related to internet communication and VOIP |
| IoT* | Sites related to the Internet of Things and IoT devices |
| Jobs | Sites that contain job search engines and other materials such as advice and strategies for seeking employment. |
| Kids* | Sites primarily related to kids and very young adults |
| Malware Content* | Sites containing malicious software, viruses or malware, software hacks, illegal codes, and computer hacker-related material A common practice is to block the Malware category for all groups. |
| Marijuana* | Sites related to the production, use or sale of marijuana |
| Messaging* | Site related to instant messaging and chat |
| Mobile Phones | Sites that sell or provide information and services about mobile (cellular) phones |
| News | Sites that provide current news and events, including online newspapers. Sites that primarily report information or comments on current events or contemporary issues of the day.  Also includes news radio stations and news magazines. May not include sites that can be better captured by other categories. |
| Nudity* | Sites containing any form of nudity |
| Online Meetings* | Sites related to software for online meetings or hosting online meetings |
| Organizations | Sites that contain content related to organizations that foster volunteerism for charity such as non-profits, foundations, societies, associations, communities, institutions. Also includes recognized pageants (Miss Earth), boys/girls scouts, and bodies that cultivate philanthropic or relief efforts. |
| P2P* | Sites related to Peer-to-Peer (P2P) file sharing |
| Parked Domains | Sites that are parked, meaning the domain is not associated with any service such as email or a website. These domains are often are very often listed "for sale." |
| Phishing* | Sites and sites used in phishing and spearfishing campaigns |
| Piracy* | Sites related to digital piracy |
| Political | Sites that contain political content, including those representing political organizations or organizations promoting political views |
| Porn - Child | Sites that contain adult-oriented content, including sexually explicit graphics and material featuring children, or appearing to feature children. |
| Porn/Nudity | Sites that contain adult-oriented content, including sexually explicit graphics and material. Includes art with nudity, sex shops, and websites with ads showing nudity, games with nudity |
| Private Websites | Sites created by individuals containing personal expressions such as blogs, personal diaries, experiences or interest |
| Professional Services | Sites offering professional, intangible products, or expertise (as opposed to material goods). Includes sites for services performed expertly by an individual or team for the benefit of its customers. The typical services include cleaning, repairs, accounting, banking, consulting, landscaping, education, insurance, treatment, and transportation services. Additionally, includes online tutoring, dance, driving, martial arts, musical instrument lessons, and essay writing. |
| Real Estate | Sites focusing on real estate including agents, renting and leasing residences and offices, and other real estate information |
| Religion | Sites that promote or provide information regarding religious beliefs and practices |
| Remote Access Tools* | Remote access tools such as screen sharing services |
| Scams* | Sites related to scams |
| Search Engines | Sites that are used to search the web |
| Sex Ed | Sites that contain content relating to sexual education. Content may be graphic but is designed to inform about the reproductive process, sexual development, safe sex practices, sexuality, birth control, tips for better sex, and sexual enhancement products. |
| Shopping | Sites that are used to purchase consumer goods, including online auctions and classifieds. Includes event tickets |
| Spam* | Sites related to spam or used in spam email campaigns |
| Sports | Sites relating to sports and active hobbies. This includes organized, professional, and competitive sports as well as active hobbies such as fishing, golf, hunting, jogging, canoeing, archery, chess, etc. |
| Streaming Radio/TV | Sites that contain streaming radio or television content |
| Suicide* | Sites related to suicide, including suicide information |
| Suspicious* | Sites that do not necessarily contain malware or malicious content, but, due to certain attributes, have been flagged as questionable. iboss' Malware Analysis will classify these sites as "Unsafe", even if no malware is detected. The Web Request Heuristic Protection, if active, will also flag sites categorized as suspicious. |
| Swimsuit | Sites that contain sexually revealing content, but no nudity. Includes shopping sites for bikini, swimsuits,  lingerie, and other intimate apparel.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Technology | Sites that contain content related technology, including software, computer hardware, technology companies, and technical computer help. Also, sites that sponsor or provide information, news, reviews, opinions, and coverage of computing, computing devices and technology, consumer electronics, and general technology. |
| Terrorism/Radicalization | Sites that feature radical groups or movements with aggressive anti-government convictions or beliefs. |
| Tech Infrastructure* | Sites Related to technology infrastructure |
| Toolbars | Sites that offer toolbar downloads for web browsers |
| Translation Services* | Sites providing translation services |
| Transportation | Sites that contain content related to transportation. This includes information on trains, bus routes, and public transportation, and also sites selling, promoting, or relating to cars, motorcycles, boats, and aircraft. |
| Travel | Sites that provide travel-related services or information such as online travel discussion, planning, tourism, lodging, and transportation such as airlines, trains, and buses, and the pertinent schedules and fares. Sites that promote or provide travel reservations, travel experiences, vehicle rentals, descriptions of travel destinations, or promotions for hotels/casinos or other travel-related accommodations. May include festivals and Amusement Parks. |
| Violence & Hate | Sites that promote violent behavior or depict gratuitous images of death, gore or bodily harm |
| Web Hosting* | Web hosting providers |
| Webmail | Sites offering web-based email services |
| Web Proxies | Sites that offer information on, services for, or downloads of web proxies, method often used in an attempt to bypass URL and Content Filters |
