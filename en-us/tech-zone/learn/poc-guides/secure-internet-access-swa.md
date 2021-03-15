---
layout: doc
h3InToc: true
contributedBy: Frank Srp
specialThanksTo: Eric Beiers, Martin Zugec, Daniel Feller
description: Learn how to set up Citrix Secure Internet Access in conjuntion with Citrix Secure Workspace Access to provide Secure access to all applications, anywhere, from any device.
---
# Proof of Concept: Citrix Secure Internet Access with Citrix Secure Workspace Access

## Overview

Citrix Secure Internet Access provides a full cloud-delivered security stack to protect users, apps, and data against all threats without compromising the employee experience. This proof of concept (PoC) guide is designed to help you quickly configure Citrix Secure Internet Access within your Citrix Virtual Apps and Desktops environment. At the end of this PoC guide you will be able to protect your Citrix Virtual Apps and Desktops deployment with Citrix Secure Internet Access. You will be able to allow your users access applications using Direct Internet Access (DIA) without compromising on performance.

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

CSIA --> Outbound

## Citrix Secure Internet Access (CSIA) Cloud Configuration

In this section we will focus on the configuration of CSIA within the administration console.

### Log into Citrix Secure Internet Access

1.  Log into Citrix Cloud and Access the Secure Internet Access tile
2.  Select the **Configuration** tab and Click **Open Citrix SIA Configuration** to access the Configuration Console.

### Configure the Citrix Secure Internet Access PAC Settings

1.  From the **Configuration** tab navigate to **Locations & Geomapping**.
2.  On the **Zones** tab click on **Edit Default Zone**.
3.  Click on **PAC Settings**.
4.  If you need to bypass a domain, use the **Add a Function**.
5.  These are the recommended Citrix Domain and Sub-domains to be added to the PAC File:  
⋅ cloud.com & *.cloud.com  
⋅ citrixdata.com &*.citrixdata.com  
⋅ citrixworkspaceapi.net & *.citrixworkspaceapi.net  
⋅ citrixworkspacesapi.net &*.citrixworkspacesapi.net  
⋅ citrixnetworkapi.net & *.citrixnetworkapi.net  
⋅ nssvc.net &*.nssvc.net  
⋅ xendesktop.net & *.xendesktop.net  
⋅ cloudapp.net &*.cloudapp.net  
⋅ netscalergateway.net & *.netscalergateway.net  
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
