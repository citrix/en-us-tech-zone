---
layout: doc
h3InToc: true
contributedBy: Frank Srp
specialThanksTo: Eric Beiers, Martin Zugec, Daniel Feller
description: Learn how to set up Citrix Secure Internet Access within a Citrix Virtual Apps and Desktop enviornment that provides Secure access to all applications, anywhere, from any device.
---
# Proof of Concept: Citrix Secure Internet Access with Citrix Virtual Apps and Desktops

## Overview

Citrix Secure Internet Access provides a full cloud-delivered security stack to protect users, apps, and data against all threats without compromising the employee experience. This proof of concept (PoC) guide is designed to help you quickly configure Citrix Secure Internet Access within your Citrix Virtual Apps and Desktops environment. At the end of this PoC guide you are able to protect your Citrix Virtual Apps and Desktops deployment with Citrix Secure Internet Access. You are able to allow your users access applications using Direct Internet Access (DIA) without compromising on performance.

![Citrix SIA OVERVIEW](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_1.png)

## Scope

In this Proof-of-Concept guide, you experience the role of a Citrix administrator and you create a connection between your organization’s Citrix Virtual Apps & Desktop deployment and Citrix Secure Internet Access.

This guide showcases how to perform the following actions:

*  Logging into Citrix Secure Internet Access (CSIA)
*  Interfacing CSIA Security Groups with Groups from Domain Integration
*  Configuring the Proxy settings of the CSIA cloud platform
*  Configure web security policies to allow/deny access to certain websites or categories via the CSIA Console.
*  Applying Policies to Security Groups
*  Download the CSIA agent (Cloud Connector)
*  Configure the CSIA agent (Cloud Connector) via Agent Policy
*  Configure the CSIA agent (Cloud Connector) Manually (OPTIONAL - Windows Only)
*  Deploy CSIA agent (Cloud Connector) manually to VDA main image
*  Deploy CSIA agent (Cloud Connector) via AD group policy to VDA image

## Prerequisites

*  Microsoft Windows 7, 8, 10 (x86, x64, and ARM64)
*  Microsoft Server 2008R2, 2012, 2016, 2019
*  Microsoft .NET 4.5, or above
*  PowerShell 7 (only for Windows 7)
*  The following Firewall rules

### Network Requirements

#### Port / Firewall settings

#### CSIA --> Outbound

| Source  | Destination  | Protocol  | Port  | Description  |
|--- |--- |--- |--- |--- |
| CSIA  | ibosscloud.com  | TCP  | 80  | Proxy connections & Custom block pages  |
|   |   | TCP  | 443  | PAC script retrieval over HTTPS & Proxy authentication over HTTPS  |
|   |   | TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  |
|   |   | TCP  | 8009  | Alternative port for proxy connections  |
|   |   | TCP  | 8015  | Proxy authentication over HTTP  |
|   |   | TCP  | 8016  | Alternative port for proxy authentication  |
|   |   | TCP  | 8080  | Default block page  |
|   |   | TCP  | 10080  | PAC script retrieval over HTTP  |
| CSIA  | api.ibosscloud.com  | TCP  | 443  | PAC script retrieval over HTTPS & Proxy authentication over HTTPS  |
|   |   | TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  |
|   |   | TCP  | 8009  | Alternative port for proxy connections  |
|   |   | TCP  | 8015  | Proxy authentication over HTTP  |
|   |   | TCP  | 8016  | Alternative port for proxy authentication  |
|   |   | TCP  | 8080  | Default block page  |
|   |   | TCP  | 10080  | PAC script retrieval over HTTP  |
| CSIA  | accounts.iboss.com  | TCP  | 443  | PAC script retrieval over HTTPS & Proxy authentication over HTTPS  |
|   |   | TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  |
|   |   | TCP  | 8009  | Alternative port for proxy connections  |
|   |   | TCP  | 8015  | Proxy authentication over HTTP  |
|   |   | TCP  | 8016  | Alternative port for proxy authentication  |
|   |   | TCP  | 8080  | Default block page  |
|   |   | TCP  | 10080  | PAC script retrieval over HTTP  |
| CSIA  | **Customer CSIA Node**-swg.ibosscloud.com  | TCP  | 443  | PAC script retrieval over HTTPS & Proxy authentication over HTTPS  |
|   |   | TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  |
|   |   | TCP  | 8009  | Alternative port for proxy connections  |
|   |   | TCP  | 8015  | Proxy authentication over HTTP  |
|   |   | TCP  | 8016  | Alternative port for proxy authentication  |
|   |   | TCP  | 8080  | Default block page  |
|   |   | TCP  | 10080  | PAC script retrieval over HTTP  |
| CSIA  | **Customer CSIA Node**-reports.ibosscloud.com  | TCP  | 443  | PAC script retrieval over HTTPS & Proxy authentication over HTTPS  |
|   |   | TCP  | 7443  | Alternative port for PAC script retrieval over HTTPS  |
|   |   | TCP  | 8009  | Alternative port for proxy connections  |
|   |   | TCP  | 8015  | Proxy authentication over HTTP  |
|   |   | TCP  | 8016  | Alternative port for proxy authentication  |
|   |   | TCP  | 8080  | Default block page  |
|   |   | TCP  | 10080  | PAC script retrieval over HTTP  |

## Citrix Secure Internet Access (CSIA) Cloud Configuration

In this section we focus on the configuration of CSIA within the administration console.

### Log into Citrix Secure Internet Access

1.  Log into Citrix Cloud and Access the Secure Internet Access tile.  
   ![Log into Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_2.png)

2.  Select the **Configuration** tab and Click **Open Citrix SIA Configuration** to access the Configuration Console.  
   ![Citrix SIA Configuration](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_3.png)

### Configure the Citrix Secure Internet Access PAC Settings

1.  From the **Configuration** tab navigate to **Locations & Geomapping**.  
   ![Citrix SIA PAC Configuration](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_4.png)
2.  On the **Zones** tab click **Edit Default Zone**.  
   ![Citrix SIA PAC EDIT](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_5.png)
3.  Click **PAC Settings**.  
   ![Citrix SIA PAC SETTINGS](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_6.png)
4.  If you must bypass a domain, use the **Add a Function**.  
   ![Citrix SIA PAC ADD A FUNTION](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_7.png)
5.  These are the recommended Citrix Domain and Subdomains to be added to the PAC File:  
⋅ cloud.com & \*.cloud.com  
⋅ citrixdata.com & \*.citrixdata.com  
⋅ citrixworkspaceapi.net & \*.citrixworkspaceapi.net  
⋅ citrixworkspacesapi.net & \*.citrixworkspacesapi.net  
⋅ citrixnetworkapi.net & \*.citrixnetworkapi.net  
⋅ nssvc.net & \*.nssvc.net  
⋅ xendesktop.net & \*.xendesktop.net  
⋅ cloudapp.net & \*.cloudapp.net  
⋅ netscalergateway.net & \*.netscalergateway.net  
6.  Note the node shown "**node-clusterxxxxxx-swg.ibosscloud.com:80**". This must match the customer’s SWG node in Node Collection Management  
   ![Citrix SIA PAC NODE SHOWN](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_8.png)

### Interfacing CSIA Security Groups with Groups from Domain Integration

Users connecting to Citrix Secure Internet Access (CSIA) communicate domain OU information if it is available from their current user login and device. The CSIA cloud platform can be used to match the groups provided by domain-controlled user accounts. Correlating the groups of your domain integration with the security groups contained on the CSIA cloud platform allows you to administer policies and restrictions in a manner similar to the existing security policies within your organization.

The strategy for integrating domain group information into the CSIA cloud platform is to edit security groups to match the aliases of domain groups reported to the platform.

#### Integration Example

To demonstrate the execution of this concept, let's map the domain credentials of a Windows user into a security group on the CSIA cloud platform.

1.  Open a command prompt on the target computer, and run the command "net user (user name) /domain"  
2.  Gather the aliases of groups reported back by the domain controller.  
   ![Citrix SIA AD ALIASES OR GROUPS](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_9.png)
3.  Go to the CSIA cloud platform and edit either the **Group Name** or the **Alias Name** to correspond to one of the groups reported by the domain user.  
   ![Citrix SIA AD ADD ALIAS OR GROUP](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_10.png)
4.  Now when users from the integrated domain group authenticate to the CSIA cloud platform, they are assigned automatically to their corresponding security group.

### Configuring the Proxy settings of the CSIA cloud platform

1.  Navigate to the **Proxy & Caching** module.  
   ![Citrix SIA PROXY & CACHING](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_11.png)
2.  Set **Enable Proxy Settings** to **YES**.  
   ![Citrix SIA PROXY & CACHING ENABLE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_12.png)
3.  Set **User Authentication Method** to **Local User Credentials + Cloud Connections**.  
   ![Citrix SIA PROXY & CACHING AUTH](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_13.png)
4.  Click **Save**.  
   ![Citrix SIA PROXY & CACHING SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_14.png)

## Configure Web Security Policies

In this section we focus on the configuration of CSIA Web Security Policies within the administration console. This is the main place where we set actions on web categories.

### Applying Policies to Security Groups

1.  Navigate to the **Web Security** module.  
   ![Citrix SIA WEB SECURITY](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_15.png)
2.  For web security policies that have a group-based implementation available a drop-down menu is present at the top of the page above the configuration form for the policy. The status of this drop-down menu indicates which group's policy configuration you are currently viewing.  
   ![Citrix SIA WEB SECURITY GROUP](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_16.png)
3.  Click the **Group** drop-down menu and then **select group** that you want to reconfigure for the current web security policy.  
   ![Citrix SIA WEB SECURITY GROUP 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_17.png)
4.  Clicking **Save** saves the current policy's configuration only to the currently selected security group from the **Group** drop-down menu.  
   ![Citrix SIA WEB SECURITY GROUP SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_18.png)
   ⋅ **_Note:_ If you are looking to apply these settings to multiple groups**  
5.  **(Optional)** Click **Save to Multiple Groups** opens a window that allows you to assign the current policy's configuration to multiple security groups simultaneously.  
   ![Citrix SIA WEB SECURITY MULTI GROUP](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_19.png)
6.  Click the **corresponding check box** for any security group you want to apply the current policy configuration.  
   ![Citrix SIA WEB SECURITY MULTI GROUP 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_20.png)
7.  Click **Add** to add the selected security groups to the configure group.  
   ![Citrix SIA WEB SECURITY MULTI GROUP ADD](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_21.png)
Groups can either be reconfigured to accept only configuration changes that have currently been applied or accept and overwrite all configuration settings for the current policy.  
8.  The default selection is **Apply Changed Settings** which only apply the changes you have configured. Select to overwrite all configured settings for the designated security groups.  
   ![Citrix SIA WEB SECURITY MULTI GROUP APPLY](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_22.png)
9.  Individual groups can be removed by clicking **Remove** or **remove all groups** selected for configuration by clicking **Remove All**. Click **Save** to confirm the configuration being applied to all designated security groups.  
   ![Citrix SIA WEB SECURITY MULTI GROUP SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_23.png)

### Web Categories

Domains are tagged with web categories based on their content. The categories of visited websites are recorded in Reporting & Analytics.

#### Category Actions

You can associate actions with web categories. This enables you to deploy security policies quickly while minimizing the need to allow or block individual URLs. Possible actions include Allow, Block, Stealth, Soft Override, or SSL Decryption Enabled.

1.  Each category can have actions that are applied independently from the other categories.

    | Action  | Description  |
    |--- |--- |
    | Allow  | Allows users to access sites of this category. Allow is the default state for all categories. |
    | Block  | Blocks access to sites of a particular category. |
    | Stealth  | Flags traffic to that category as violations in the logs but still allow access to the site.  |
    | Soft Override  | Presents a block page to the user but includes an option to bypass the block temporarily. This allows users to access blocked content without requiring immediate administrator intervention. Soft overrides last until the next day at 2:00 AM in the time zone configured for the gateway. Any block page presented with a soft override appears in Reporting & Analytics as "soft-blocked." After a user has requested a soft override, any traffic to that URL shows as "allowed" until the override has expired.   |
    | SSL Decryption  | Disable this option to clear SSL Decryption for a specific category. This ensures privacy compliance and concerns with specific categories like Finance or Health. The priority value for this category does NOT apply to this setting. If a site belongs to multiple categories, and any of those categories has the "SSL Decryption Disabled" option on, that site is not be decrypted.  |
    | Lock  | When a category is locked by the primary administrator into either an Allowed or Blocked state, delegated administrators cannot log in to the web gateway management interface and change the status of that category.  |
    | Category Override  | When you activate a category override, a delegated administrator cannot log in to the web gateway management interface and add a URL to the allow list that would contradict the rule for this category. For example, the "Art" category is blocked and set to "Overrides," so delegated administrators cannot add art.com to the Allow List for that group.  |

2.  Click each icon to toggle the action for the respective web category.  
   ![Citrix SIA WEB CAT 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_24.png)
   ![Citrix SIA WEB CAT 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_25.png)
   ![Citrix SIA WEB CAT 3](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_26.png)
3.  Actions that you want to generally apply to all web categories can be implemented with the Actions drop-down menu.  
   ![Citrix SIA WEB CAT 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_27.png)  
**NOTE:** _Be careful with the “Not Rated” category, as it matches against many sites that are not categorized_

#### Category Priorities

If a domain is associated with multiple categories, the action is determined by comparing the priority values for the categories. Categories with higher priority value take precedence.

Example:

1.  A domain is categorized as both government and hacking. In the web categories configuration, the government is allowed with a weight of 100 while hacking is blocked with a weight of 200.
   ![Citrix SIA EXAMPLE 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_28.png)
2.  This domain would be blocked when visited by a user due to the block action of the hacking web category possessing a higher priority value.
3.  Click the priority value field and enter a numerical value to reconfigure a category's priority level. The priority value has a configurable range of 0-65535.

#### Additional Settings

Settings relevant to web categories are configured with toggles under **Additional Settings**. Configuring a toggle to **Yes** enables the setting while configuring a toggle to **No** disables the setting. For a full set of descriptions for all available web category settings refer to the following table:
| Feature  | Description  |
|---  |---  |
| Enable Logging  | Enable and disable logging of violation attempts for the current set of blocked website categories. Log reports may be viewed on the CSIA Reports page. The report information includes the date, time, user, website address, and category of the violation.  |
| Enable Stealth Mode  | It allows you to stealthily monitor Internet activity without blocking access to forbidden sites. With both Logging and Stealth Mode enabled, you can monitor Internet web surfing activity by viewing the log reports on the CSIA Reports page while remaining unnoticed by Internet users on the network.  Note: Websites and online applications are not blocked when the action for the web category is configured to Stealth Mode.  |
| Enable HTTP Scanning on Non-Standard Ports  | If this feature is enabled, CSIA scans for HTTP web requests on non-standard ports.  |
| Allow Legacy HTTP 1.0 Requests  | If this feature is enabled, CSIA allows HTTP 1.0 requests that are missing the "HOST" header. Disabling this feature provides a higher level of Security and makes bypassing the Security more difficult. If this feature is enabled, it may offer more compatibility with older non-HTTP 1.1 compliant software.  |
| Enable ID Theft / IP Address URL Blocking  | Protects against potential identity theft attempts by notifying you when someone is trying to steal your personal information through Internet Phishing. Enabling this feature also blocks users from navigating to websites using IP address URLs.  |
| Enable Blocked Site Override  | Enabling this feature activates a soft override action, allowing users to proceed to the site belonging to a blocked category. When this setting is active, the user is warned that a page is blocked but provides a button to proceed anyway.  |
| Auto Categorize Uncategorized Sites  | When this toggle is switched to Yes, any sites that are not categorized are automatically submitted for categorization. Note: When this toggle is set to Yes, uncategorized sites are assigned a Web Category of "Informational" or other such designation. This is only in effect while the category is being appropriately categorized. When this switch is set to No, uncategorized are automatically designated as "Not Rated," which is controlled as its own Web Category.  |

#### Category Scheduling

Block events can also be configured on an advanced weekly schedule to allow access during particular times.

1.  Set Category Scheduling to **Allow Selected Categories Using an Advanced Schedule** to enable the scheduling feature. Click **Advanced Scheduling** to begin scheduling blocked categories.  
   ![Citrix SIA CATEGORY SCHEDULING 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_29.png)
2.  The current advanced schedule can either be configured to apply to all blocked categories or only to a particular blocked category.  
   ![Citrix SIA CATEGORY SCHEDULING 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_30.png)
3.  Each day during the week can be delegated specific periods of time that the category is allowed. These particular periods of time are indicated by a blue rectangle.  
   ![Citrix SIA CATEGORY SCHEDULING 3](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_31.png)
4.  After finalizing the schedule for a particular category, you can click the category drop-down menu once again and select a new category to configure.
5.  Click **Save** to confirm all schedule configurations.  
   ![Citrix SIA CATEGORY SCHEDULING SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_32.png)

### Allow List

The allow lists selectively provides access to a specific website or network resource. This feature enables you to override security policies and allow certain users to access a website. In particular, you can use this to grant access to specific URLs for a domain that is otherwise blocked. This is sometimes referred to as "punching a hole."

#### Adding a URL to an Allow List

1.  Navigate to **Web Security Policies**.
2.  On the drop-down menu click **Allow List**.  
   ![Citrix SIA ALLOW LIST](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_33.png)
3.  From above the list section, click **URL/IP Range**.  
   ![Citrix SIA ALLOW LIST URL/IP](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_34.png)
4.  Type a **domain, subdomain, URL, IP address, or IP Range**. This is the only required field, but many other criteria can be specified.
5.  From the right side of the list, click **+Add**. The entry is now added to the list.

#### Scrape Tool

The **Allow List** section includes the handy Scrape Tool. Use this to quickly identify all of the domains that a website uses. In the modern web, many sites use resources from other domains that are not immediately apparent. With the Scrape Tool, these are easily revealed and added to an allow list.

Another use for the Scrape Tool is to selectively only allow portions of a website, while not allowing unwanted content, such as ad servers.

1.  From above the list section, click **Scrape**.  
   ![Citrix SIA SCRAPE TOOL](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_35.png)
2.  Enter the URL to Scrape and click **Scan**.  
   ![Citrix SIA SCRAPE SCAN](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_36.png)
3.  Select domains to add to Allow List and click Add Selected to Allow List.  
   ![Citrix SIA SCRAPE ADD](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_37.png)

### Keyword Block List / Allow List

Keyword filtering is used to inspect URLs for specific words. If a keyword is identified, the content can be allowed or blocked.

#### Adding a URL to a Block List / Allow List

1.  Navigate to **Web Security Policies**.
2.  On the drop-down menu click **Keywords**.  
   ![Citrix SIA KEYWORD](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_38.png)
3.  Enter the **keyword** that you would like to block in the **Keyword** field and specify the designations that apply to this keyword. Click **Add**.  
   ![Citrix SIA KEYWORD ALLOW](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_39.png)

| Option  | Description  |
|---  |----  |
| Allow Keyword  | Checking this option allows the word if it is in the URL within a keyword parameter.  |
| High Risk  | When any words designated as "High Risk" are searched, an email notification is sent to the administrator of the group.  |
| Wildcard Match  | When enabled, keywords are filtered even when they are just substrings of a larger word. For example, a wildcard match for the keyword "base" blocks searches that include "base" or "baseball." Without wildcard matching, it only blocks "base."  |
| Global  | This option spans across all Security groups when selected. When removing a “Global" entry, it removes the entry from all filtering groups. |

#### Pre-defined Keyword Lists

You can enable filtering for pre-defined lists of Adult and High-Risk keywords or add more keywords manually.

The Pre-defined Keyword Lists contain common Adult and High-Risk keywords. Wildcard matching is applied to all keywords in these lists. A wildcard match recognizes the keyword's sequence of characters anywhere in the URL, including the host name. The High-Risk list generates an email to the recipient of alert emails when a High-Risk keyword is detected by the Reporting & Analytics functionality of the CSIA cloud platform.

1.  To enable either one of the keyword lists, set the Adult or High-Risk toggle to **Yes**. Click **Save**.  
   ![Citrix SIA KEYWORD ALLOW](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_40.png)

## Citrix Secure Internet Access Agent Configuration

In this section we focus on the configuration and the installation of the Citrix Secure Internet Access (CSIA) Agent.

### Configure the CSIA Agent Download (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors**.  
   ![Citrix SIA AGENT](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_41.png)
2.  Click **Configure Connector Download**.  
   ![Citrix SIA AGENT DOWNLOAD CONFIGURE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_42.png)
3.  Click the **Use HTTP PAC** drop-down menu and select **No**.  
(**Note:** Use HTTP PAC to **"No"** if you want to use **_HTTPS_** for PAC download)  
   ![Citrix SIA AGENT HTTP PAC](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_43.png)
4.  Click **Security Group** and select the desired default security group for this particular installation file download.  
   ![Citrix SIA AGENT SECURITY GROUP](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_44.png)
5.  Keep the remaining connector download settings as **Default** and Click **Save**.  
   ![Citrix SIA AGENT SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_45.png)

### Configure the CSIA Agent Advanced Connector Settings (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors > Advanced Connector Settings**.  
   ![Citrix SIA AGENT ADV SETTINGS](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_46.png)
2.  Under Global Settings Enable the below:  
⋅ **Enable Security Cloud Connector Filtering**  
⋅ **Configure Auto Login Cloud Connectors to use Key for Group**  
⋅ **Use Session Encryption**
3.  Under Source IP Logging Enable - **Use private source IP of client (if available)**.
4.  Under Group Specific Settings verify that the **Correct Group** is selected and Click **Save**.  
   ![Citrix SIA AGENT ADV SETTINGS SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_47.png)

### Download the Citrix Secure Internet Access Agent (Cloud Connector)

1.  From the CSIA admin console, go to **Connect Device to Cloud > Cloud Connectors > Download Connectors**.  
   ![Citrix SIA AGENT DOWNLOAD](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_48.png)
2.  Under Windows **Cloud Connector**, click **Download** and **Download All**.  
   ![Citrix SIA AGENT DOWNLOAD ALL](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_49.png)  
The .msi installation packages can be downloaded directly from the CSIA admin console. Before starting the installation, be sure that you are using the correct package for the version of Windows and processor architecture.

| Platform  | Package  |
|---  |--- |
| Windows 10 ARM64 | ibsa64-win10-arm64.msi |
| Windows 10 x64 or Windows 8 x64 or Windows Server 2019 or Windows Server 2016  | ibsa64-win8.msi |
| Windows 10 x86 or Windows 8 x86 | ibsa32-win8.msi |
| Windows 7 x64 or Windows Server 2012 or Windows Server 2008R2 | ibsa64.msi |
| Windows 7 x86 | ibsa32.msi |

### Configure the CSIA Agent Policies in the CSIA Admin Console

1.  From the CSIA admin console, go to **Connect Device to Cloud > Agent Policies**.  
   ![Citrix SIA AGENT POLICIES](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_50.png)
2.  Click **Add Agent Policies**.  
   ![Citrix SIA AGENT POLICIES ADD](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_51.png)
3.  Provide a **Name** for your Agent Policy and Select **Add Agent Policy**.  
   ![Citrix SIA AGENT POLICIES NAME](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_52.png)
4.  To configure the policy, click **Edit Agent Policy**.  
   ![Citrix SIA AGENT POLICIES EDIT](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_53.png)
5.  Click **Agent Settings** and set the recommended settings.  
   ![Citrix SIA AGENT POLICIES SETTINGS](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_54.png)

    **Recommended Settings for Multi-Session OS VDA Deployments**

    | Setting  | Recommended Value  |
    |---  |---  |
    | Multi-User Mode:  | **Enable** Multi-User/Terminal Server Mode - Enable to support multiple user sessions when running a terminal server. This must be enabled for terminal servers even if users are not logged in simultaneously.  |
    | Use Machine Name for User name:  | **Disable** Setting - Use the user account name as the user name.  |
    | Use UPN for User name:  | **Disable** Setting - Use the Security Account Manager (SAM) account name, such as DOMAIN\user name.  |
    | Redirect All Ports:  | **Enable** Setting  |
    | Bypass Private Subnets:  | **Enable** Setting  |
    | Captive Portal Detection:  | **Disable** Setting  |
    | Auto-Update Enabled:  | **Enable** Setting  |
    | Auto-Update Release Level:  | **Level 1 - Mature**  |
    | Enable Windows Desktop App: (Agent is not supported Multi-User Deployments)  | **Disable** Setting  |

    **Recommended Settings for Single-Session OS VDA Deployments**

    | Setting  | Recommended Value  |
    |---  |---  |
    | Multi-User Mode:  | **Disable** Multi-User/Terminal Server Mode - Enable to support multiple user sessions when running a terminal server. This must be enabled for terminal servers even if users are not logged in simultaneously.  |
    | Use Machine Name for User name:  | **Disable** Setting - Use the user account name as the user name.  |
    | Use UPN for User name:  | **Disable** Setting - Use the Security Account Manager (SAM) account name, such as DOMAIN\user name.  |
    | Redirect All Ports:  | **Enable** Setting  |
    | Bypass Private Subnets:  | **Enable** Setting  |
    | Captive Portal Detection:  | **Disable** Setting  |
    | Auto-Update Enabled:  | **Enable** Setting  |
    | Auto-Update Release Level:  | **Level 1 - Mature**  |
    | Enable Windows Desktop App: (Agent is not supported on Multi-User Deployments)  | **Enable** Setting  |
    | Allow End Users to Disable Security:  | **Disable** Setting  |
    | Require Password to Disable Security:  | **Enable** Setting  |
    | Require Password to View Diagnostics Info:  | **Enable** Setting  |

6.  Click **Dynamic Linking** and select the Groups you want to assign the policy too.  
   ![Citrix SIA AGENT POLICIES DYNAMIC LINKING](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_55.png)
7.  Click **Save**.  
   ![Citrix SIA AGENT POLICIES SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_56.png)

### **(OPTIONAL)** Manual Configuration of the of the Citrix Secure Internet Access Agent

The CSIA Agent for Windows .msi editing via Orca is only recommended for troubleshooting.

Orca.msi is available in the Windows **Cloud Connector** “Download All” option

#### Opening an .MSI File with Orca

1.  Open Zip file and install Orca.msi  
   ![Citrix SIA AGENT MSI](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_57.png)
2.  Locate the desired installation file in a file explorer program.
3.  Right-click the .msi installation file.
4.  Click **Edit with Orca**.  
   ![Citrix SIA AGENT MSI EDIT](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_58.png)

#### Configuring Properties of an .MSI in Orca

1.  Double-click to open the **Property table**.  
   ![Citrix SIA AGENT MSI PROPERTY](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_59.png)
2.  Each property can be edited by double-clicking the property's **Value** field.  
   ![Citrix SIA AGENT MSI PROPERTY VALUE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_60.png)

    **Recommended Settings for Multi-Session OS VDA**

      | Setting  | Recommended Value  |
      |---  |---  |
      | Multi-User Mode: (PARAM_MULTI_USER_SUPPORT)  | **(1): Enable multi-user mode.** Enable to support multiple user sessions when running a terminal server. This must be enabled for terminal servers even if users are not logged in simultaneously.  |
      | Terminal Server Mode: (PARAM_TERMINAL_SERVER_MODE)  | **(0): Disabled** - this appears to be deprecated in favor of Multi-User Support  |
      | Use Machine Name for User name: (PARAM_USE_MACHINE_NAME_FOR_USER NAME)  | **(0): Disabled** - Use the user account name as the user name.  |
      | Redirect All Ports: (PARAM_REDIRECT_ALL_PORTS)  | **(1): Enabled** - Redirect all ports to the proxy.  |
      | Bypass Private Subnets: PARAM_BYPASS_PRIVATE_SUBNETS)  | **(1): Enable bypass**  |
      | Captive Portal Detection: (PARAM_CAPTIVE_PORTAL_DETECTION)  | **(0): Disabled**  |
      | Auto-Update Enabled: (PARAM_AUTO_UPDATE_ENABLE)  | **(1): Enabled** – The cloud connector to be updated automatically.  |
      | Restart After Upgrade: (PARAM_RESTART_AFTER_UPGRADE)  | **(0): Disabled** - Does not prompt a restart.  |

    **Recommended Settings for Single-session OS VDA**

      | Setting  | Recommended Value  |
      |---  |---  |
      | Multi-User Mode: (PARAM_MULTI_USER_SUPPORT)  | **(0): Disable multi-user mode.**  Enable to support multiple user sessions when running a terminal server. This must be enabled for terminal servers even if users are not logged in simultaneously.  |
      | Terminal Server Mode: (PARAM_TERMINAL_SERVER_MODE)  | **(0): Disabled** - this appears to be deprecated in favor of Multi-User Support  |
      | Use Machine Name for User name: (PARAM_USE_MACHINE_NAME_FOR_USER NAME)  | **(0): Disabled** - Use the user account name as the user name.  |
      | Redirect All Ports: (PARAM_REDIRECT_ALL_PORTS)  | **(1): Enabled** - Redirect all ports to the proxy.  |
      | Bypass Private Subnets: (PARAM_BYPASS_PRIVATE_SUBNETS)  | **(1): Enable bypass**  |
      | Captive Portal Detection: (PARAM_CAPTIVE_PORTAL_DETECTION)  | **(0): Disabled**  |
      | Auto-Update Enabled: (PARAM_AUTO_UPDATE_ENABLE)  | **(1): Enabled** – The cloud connector to be updated automatically.  |
      | Restart After Upgrade: (PARAM_RESTART_AFTER_UPGRADE)  | **(0): Disabled** - Does not prompt a restart.  |

3.  Within Orca, click the **Save** icon to save changes made to the parameters of the Windows Cloud Connector.  
   ![Citrix SIA AGENT MSI PROPERTY SAVE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_61.png)  
**Note:** _Ensure files are saved within Orca only using this method **(not using Save As)**. This causes issues with the functionality of the Windows cloud connector if it is not saved in this manner._

## Deployment of the Citrix Secure Internet Access Agent to Main Image

Citrix recommends you save copies or snapshots of main images before you update the machines in the catalog. The database keeps a historical record of the main images used with each machine catalog. You can roll back (revert) machines in a catalog to use the previous version of the main image if users encounter problems with updates you deployed to their desktops, thereby minimizing user downtime. Do not delete, move, or rename main images; otherwise, you cannot be able to revert a catalog to use them.

For catalogs that use Provisioning Services, you must publish a new virtual disk to apply changes to the catalog. For details, see the Provisioning Services documentation.

After a machine is updated, it restarts automatically.

### Deploy CSIA agent (Cloud Connector) manually to VDA main image

#### Update Main Image

Before you update the catalog, either update an existing main image or create a new one on your host hypervisor.

1.  On your hypervisor or cloud service provider, take a snapshot of the current VM and give the snapshot a meaningful name. This snapshot can be used to revert (roll back) machines in the catalog, if needed.
2.  **Power on** the main image and log on.
3.  **Install** the appropriate CSIA Agent .msi package for your platform to the main image.  
   ![Citrix SIA AGENT DEPLOY MANUAL](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_67.png)
4.  If the main image uses a Personal vDisk, update the inventory.
5.  Power off the VM.
6.  Take a snapshot of the VM and give the snapshot a meaningful name that can be recognized when the catalog is updated in Studio. Although Studio can create a snapshot, Citrix recommends that you create a snapshot using the hypervisor management console, and then select that snapshot in Studio. This enables you to provide a meaningful name and description rather than an automatically generated name.

#### Update the catalog

To prepare and roll out the update to all machines in a catalog:

1.  Select **Machine Catalogs** in the Studio navigation pane.
2.  Select a catalog and then select **Update Machines** in the Actions pane.
3.  On the **Main Image** page, select the host and the image you want to roll out.
4.  On the **Rollout Strategy** page, choose when the machines in the Machine Catalog can be updated with the new main image: on the next shutdown or immediately. See below for details.
5.  Verify the information on the **Summary** page and then click Finish. Each machine restarts automatically after it is updated.

### Deploy CSIA agent (Cloud Connector) via AD group policy to Static VDA images

#### Create a Distribution Point

1.  **Create folder** on an AD joined computer that acts as the file server.
2.  **Save** the CSIA Agent .msi package inside that folder.
3.  **Right-Click** the newly created folder, **select properties**.
4.  Go to **“Sharing”** tab.
5.  Select **“Advanced Sharing”**.
6.  Enable **“Share this Folder”**.
7.  In Settings under Share name add a **$** after the CSIA Agent folder name. (Example. CSIA_Agent$).
8.  Select **Apply**.
9.  Then **Close**.

#### Create Group Policy Object

1.  **Open** Group Policy Management.
2.  **Right-click** Group Policy Object.
3.  Select **New**, and name new GPO (Example: Deploy CSIA Agent).
4.  **Right-click** the newly created Group Policy Object from above, Select Edit.
5.  **Expand** Software Settings folder.
6.  Select **software installation**.
7.  **Right-click** in the right panel.
8.  Select **New**.
9.  Select **Package**.
10.  In the corresponding window, type in the location (file path) of your CSIA Agent folder containing the CSIA Agent .msi package.
11.  Select the **MSI**.
12.  Select **Open**.
13.  In the Deploy Software window, choose **“Advanced”** underneath Select Deployment Method.
14.  Under Deployment select **Uninstall This Application when…**. (this means next time the endpoint runs a gpupdate and the CSIA Agent has been removed it is removed from the endpoint as well).
15.  Select **OK** and **Close**.

#### Scenario 1: Static VDA with user data stored in local disk

1.  On your hypervisor or cloud service provider, take a snapshot of the VDAs and give the snapshot a meaningful name.
2.  In your Group Policy Management, expand your domain.
3.  Right-click the location of your Static VDAs, then select “Link an existing GPO”.
4.  Select the Deploy CSIA Agent GPO that was created.
5.  Select OK.
6.  Once the Group Policy is applied you must reboot the VDAs.

#### Scenario 2: Static VDA with user data discarded on logout

1.  On your hypervisor or cloud service provider, take a snapshot of the current VM and give the snapshot a meaningful name. This snapshot can be used to revert (roll back) machines in the catalog, if needed.
2.  In your Group Policy Management, expand your domain.
3.  Right-click the location of your Main Image, then select “Link an existing GPO”.
4.  Select the Deploy CSIA Agent GPO that was created.
5.  Select OK.
6.  Once the Group Policy is applied you must reboot the machines.
7.  Take a snapshot of the VM and give the snapshot a meaningful name that is recognized when the catalog is updated in Studio. Although Studio can create a snapshot, Citrix recommends that you create a snapshot using the hypervisor management console, and then select that snapshot in Studio. This enables you to provide a meaningful name and description rather than an automatically generated name.
8.  Update your Machine Catalogs in the Studio navigation pane. (See the Update the catalog section above)

#### Scenario 3: non-MCS Static VDA with user data stored in local disk

1.  On your hypervisor or cloud service provider, take a snapshot of the current VM and give the snapshot a meaningful name. This snapshot can be used to revert (roll back) machines in the catalog, if needed.
2.  In your Group Policy Management, expand your domain.
3.  Right-click the location of your Main Image, then select “Link an existing GPO”.
4.  Select the Deploy CSIA Agent GPO that was created.
5.  Select OK.
6.  Once the Group Policy is applied you must reboot the machines.

## Validated Use Cases

### Deployed CSIA agent (Cloud Connector) manually to VDA main image

#### MCS: Pooled-random desktop

*  Windows 10 Enterprise (Single-Session)
*  Windows Server 2019 (Multi-Session)

#### MCS: Pooled-static desktop

*  Windows 10 Enterprise (Single-Session)

#### MCS: Dedicated desktop

*  Windows 10 Enterprise (Single-Session)

#### PVS: Pooled-random desktop

*  Windows 10 Enterprise (Single-Session)
*  Windows Server 2019 (Multi-Session)

#### PVS: Pooled-static desktop

*  Windows 10 Enterprise (Single-Session)

#### PVS: Dedicated desktop

*  Windows 10 Enterprise (Single-Session)

#### Non-MCS: Random desktop

*  Windows 10 Enterprise (Single-Session)
*  Windows Server 2019 (Multi-Session)

#### Non-MCS: Static desktop

*  Windows 10 Enterprise (Single-Session)

### Deployed CSIA agent (Cloud Connector) via AD group policy to Static VDA images

#### MCS

*  Static VDA with user data stored in local disk
*  Static VDA with user data discarded on logout

#### Non-MCS

*  Static VDA with user data saved into local disk

### VDAs in Different Regions

## Troubleshooting

### Important Tools for Troubleshooting

1.  Windows Event Log
2.  CSIA Real-Time Dashboard **(Reporting & Analytics > Real-Time Log)**
3.  CSIA Event Logs **(Reporting & Analytics > Logs > Event Log)**
4.  CSIA IPS Logs **(Reporting & Analytics > Logs > IPS Log)**
5.  Registration Information for Connected Devices **(Users, Groups & Devices > Cloud Connected Device > Info)**
6.  URL Lookup Tool **(Tools > URL Lookup)**
7.  Enhanced Logging  
 ⋅ To set this, the following registry key must be altered, varying from 0 to 4, the higher giving more verbose logging.  
 ⋅  **HKEY_LOCAL_MACHINE\SOFTWARE\IBoss\IBSA\Parameters\LogLevel**  
 ⋅ Once the registry key has been set, the IBSA service under Windows Services must be restarted for the setting to take effect. Checking windows event viewer, you see many entries being logged depending on the log level set.
8.  Windows Agent Logs (C:\Windows\SysWOW64\ibsa_0.log)

### A Keyword is not Being Blocked

There is a multitude of policies and variables within and surrounding the operations of the CSIA cloud platform that can interfere with properly blocking configured keywords. Refer to the following;

1.  Ensure that SSL decryption is active for this website. Keywords cannot be observed or controlled for HTTPS websites.
2.  If the source IP of the client workstation or the destination IP of the webserver has been added to the **Network > Bypass IP Ranges** list, web security controls are not enforced.
3.  Keywords configured to block do not take effect if the website is added to the **Web Security > Allow List without the Keyword/Safesearch** option enabled. With the Keyword / SafeSearch check box selected, Web Gateway allows access to the website but still to enforce Keyword controls and Safesearch.
4.  The keyword contains asterisks, instead remove the asterisks and activate Wildcard matching for the keyword if that is the desired effect.
5.  The keyword has multiple words, and Wildcard Matching is not enabled, or the spaces were not indicated with a plus sign ("+").
6.  The user is not associated with the expected web security group that has the keyword control enabled.

### Conflicting Actions from Pre-defined Keyword Lists

In some situations, a word in one of the built-in lists of keywords may inadvertently block content unexpectedly. To correct this action, edit the specific pre-defined keyword list. When you click the pencil icon to edit a built-in list, the CSIA cloud platform interface presents a page of keywords with check boxes next to them. To remove the keyword from the built-in list, clear the box next to the keyword you would like to remove and click **Save**. To apply this action to all groups, check the box that is labeled **Apply to All Groups**.

### Identifying the Customer’s Citrix Secure Internet Access Node

1.  From the **Home** navigate to the **Node Collection Management**.  
   ![Citrix SIA NODE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_68.png)
2.  Click **Node Groups**.  
   ![Citrix SIA NODE 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_69.png)
3.  This provides you with both the **_Customer CSIA Node_**-reports.ibosscloud.com and the **_Customer CSIA Node_**-swg.ibosscloud.com Node Clusters.

### Splunk Integration with Citrix Secure Internet Access Node

#### Splunk Server Setup

1.  Navigate to the Splunk Server instance and click the **Settings** link at the top of the page, followed by the **Data inputs** link under the “Data” subsection.  
   ![Citrix SIA SPLUNK](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_70.png)
2.  Click the Add new link to the right of the “UDP” section.  
   ![Citrix SIA SPLUNK 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_71.png)
3.  Enter a port into the "port" field (above 1023, if possible, to avoid security restrictions with the operating system). In the “Only accept connection from” field, enter your CSIA Reporter Node's IP address. If nothing is entered in this field, connections from all hosts are accepted. When done, click **Next**.  
   ![Citrix SIA SPLUNK 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_72.png)
4.  On the next page, click **Select Source Type** and type **"syslog,"** then select it.  
   ![Citrix SIA SPLUNK 3](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_73.png)
5.  Change the App Context to **Search & Reporting (search)**.  
   ![Citrix SIA SPLUNK 4](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_74.png)
6.  Change the Host Method to **IP**.  
   ![Citrix SIA SPLUNK 5](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_75.png)
7.  Click **Review** (review current configuration), then click **Submit**.  
   ![Citrix SIA SPLUNK 6](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_76.png)

#### CSIA Reporting & Analytics Module Setup

1.  Navigate to **Reporting & Analytics > Log Forwarding > Forward From Reporter** in the iboss cloud platform interface.  
   ![Citrix SIA REPORTING](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_77.png)
2.  Under **Splunk Integration**, click **Actions**, then click **Add Server**.  
   ![Citrix SIA REPORTING 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_78.png)
3.  Add the Address/Host name of the Splunk Server to the “Host name” and the port number chosen on the Splunk server. Next, in the drop-down menu “Splunk Integration Protocol,” choose a protocol.
The options available when adding a Splunk server appear as follows:  
   ![Citrix SIA REPORTING 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_79.png)
4.  You can also configure the Splunk server's integration protocol as HEC. Configuring integration with a Splunk server using the HEC protocol requires the acquisition of the HEC token from the configuration of the Splunk server. Place the retrieved token into the Token field below the Splunk Integration Protocol selection drop-down menu.  
   ![Citrix SIA REPORTING 3](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_80.png)
5.  If implementing an **ELFF** log format for Splunk logging the **Splunk Integration ELFF Batch Size** field becomes available for configuration. The default value for configuration is **100**.  
   ![Citrix SIA REPORTING 4](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_81.png)
6.  A toggle called “Accept All SSL Certs” is available under the "Splunk Integration" section within **Settings > External Logging**. If a non-standard SSL Certificate such as a Self-Signed certificate or a certificate signed by a non-trusted root CA is used, switch this toggle to "YES" to bypass SSL certificate verification, otherwise leave the switch off.
7.  Select the format in which the log data is delivered from the "Log Format" drop-down menu. Finally, switch one or more of the toggles at the bottom of the interface to select the desired logging information types. Click the **Save** button to update the changes. The logging begins immediately. Perform a search on the Splunk instance to check data is being sent and indexed properly. See the sample output below.  
   ![Citrix SIA REPORTING 5](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_82.png)

### Changing the Proxy Port

The CSIA proxy port may be changed, but you mustn't attempt to change the proxy port to one that the gateway uses for other services.

Ports that **cannot** be used include: 53, 139, 199, 443, 445, 953, 1080, 1344, 5432, 6001, 7009, 7080, 7443, 8008 ,8015, 8016, 8025, 8026, 8035, 8036, 8080, 8200, 8201, 9080, 9443, 17500, 22022

_**All other ports are an acceptable alternative to the default port.**_

1.  Navigate to **Proxy & Caching > Proxy Settings**  
   ![Citrix SIA PROXY CHANGE](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_83.png)
2.  Under the **Settings tab**, enter the desired port number on which the proxy listens for traffic into the Proxy Port field.  
**Note:** The port configured for this setting is used when configuring proxy settings in other platform functionalities. Some ports may not be available for assignment to this setting due to pre-configured gateway services.  
   ![Citrix SIA PROXY CHANGE 1](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_84.png)
3.  Click **Save** to apply this change.  
   ![Citrix SIA PROXY CHANGE 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-cvad_85.png)

## Appendix

### Main Category List

_**Note:** Be careful with the “Not Rated” category, as it matches against many sites that are not categorized._

| Category  | Description  |
|---  |---  |
| Abortion* | Sites related to abortion |
| Ads | Sites used to distribute advertising graphics or content in addition to online coupons, advertising sales, voucher, deals, and offers |
| Adult Content | Sites that contain adult-oriented material. Sites in this category do not contain any nudity but do feature profane and vulgar content. Sites that self-identify as being inappropriate for those under 18 falls under this category. |
| Alcohol/Tobacco | Sites that contain alcohol and tobacco content. Also includes sites related to alcohol such as bars. Sites in this category do not contain illegal drugs, but may discuss, encourage, promote, offer, sell, supply, or otherwise advocate the use or creation of alcohol/tobacco. |
| Art | Sites that contain art or discuss art, including museums. May also include printable coloring books, sculptures, mosaic, tattoos, calligraphy, fonts, painting, graffiti, Christian or religious designs, animation drawing, artistic design|
| Auctions | Sites related to auctions and bidding on goods and services, both online and live |
| Audio & Video | Sites that contain streaming or downloadable audio/video content such as mp3s, movie clips, and TV shows, in addition to sites that sell this content. |
| Business | Sites that represent a business's online presence. May be engaged in commerce or the activity of buying and selling products and services between the company and consumer. Includes Manufacturers, Producers, Suppliers, Dealers, Distributors, Wholesalers, Retailers, Family-owned businesses, and any other business-oriented entities. |
| CDN* | Content Delivery Networks (CDNs) and sites related to CDNs |
| Dating & Personals | Sites that offer dating services or aid in the establishment of romantic relationships |
| Dictionary | Sites containing extensive collections of information and knowledge. Includes resources such as wikis, lexical dictionaries, maps, censuses, almanacs, library catalogs, genealogy-related sites, scientific information, and directories in addition to utilities like clocks, calculators, timers, and templates. |
| Drugs | Sites containing content relating to illegal drugs such as Amphetamines, Barbiturates, Benzodiazepines, Cocaine, Designer Drugs, Ecstasy, and Heroin. Does not refer to Cannabis/Marijuana |
| Drugs - Controlled* | Sites related to controlled drugs and substances |
| Dynamic DNS* | Sites that utilize dynamic DNS services to map their domain names to dynamic IP addresses. |
| Education | Sites that provide educational services such as schools and universities, in addition to sites that offer educational materials for sale or reference. Includes websites that offer information on education or trade/vocational/career schools and programs. Also includes sites that are sponsored by schools, educational facilities, faculty, or alumni groups. |
| Entertainment | Sites that contain or promote television, movies, magazines, radio, books, food, fashion, and lifestyle. More specifically, sites that provide information about or promote popular culture including (but not limited to) film, film critiques, and discussions, film trailers, box office, television, home entertainment, music, comics, graphic novels, literary news, and reviews, in addition to other entertainment-oriented periodicals, interviews, fan clubs, celebrity gossip, podcasts, and music and film charts, show, events, quotes, memes, lyrics, musicians, bands, theater arts, drama, opera, orchestra. |
| Extreme* | Sites containing intensely vulgar, graphic, shocking, or disgusting content that would be considered highly offensive to most individuals. |
| File Sharing | Sites for services that provide online file storage, file sharing, synchronization of files between devices, and or network-based data backup and restoration. These services may provide the means to upload, download, paste, organize, post, and share documents, files, computer code, text, non-copyright-restricted videos, music, and other electronically formatted information in virtual data storage. Also, this category covers services that distribute software to facilitate the direct exchange of files between users. |
| Finance | Sites that contain content about banking, financial news and tips, the stock market, investing, credit cards, insurance, and lending. |
| Food | Sites that contain content related to restaurants, food, dining, in addition to sites that list, review, discuss, advertise and promote food, catering, dining services, cooking, and recipes. |
| Forums | Sites containing message boards, online chat rooms, and discussion forums |
| Freeware / Shareware* | Sites related to distributing freeware and shareware software |
| Friendship | Sites that contain platonic friendship related materials and social networking sites. |
| Gambling | Sites that promote or contain gambling-related content such as online poker and casinos, sports betting, and lotteries. Sites where a user can place bets or participate in betting pools, lotteries, or receive information, assistance, recommendations, or training in such activities. This category does not include sites that sell gambling-related products or sites for offline casinos/hotels unless they meet one of the above requirements. |
| Games | Sites that contain online games, or provide services and information about electronic games, household video games and consoles, computer games, and role-playing games. Also includes game guides/cheats, and accessories |
| Government | Sites sponsored by or representing government agencies, including military and political organizations. May provide information on taxation, emergency services, and laws of various governmental entities. Also includes sites that provide adoption services, information about adoption, immigration information, and immigration services. |
| Guns & Weapons | Sites that promote, sell, or provide information regarding firearms, knives, and other weapons |
| Hacking* | Sites related to hacking and hacking tools |
| Health | Sites that contain content related to health, illnesses, and ailments, including hospitals, doctors, and prescription drugs, including sites primarily focusing on health research. Also, sites that provide advice and information on general health such as fitness and well-being, personal health, medical services, over-the-counter and prescription medications, health effects of both legal and illegal drug use, alternative and complementary therapies, dentistry, optometry, and psychiatry. Also, includes self-help and support organizations dedicated to a disease or health conditions. |
| Humor* | Sites primarily related to jokes, humor, or comedy |
| Illegal Activity* | Sites related to illicit activities or activities illegal in most countries |
| Image/Video Search | Sites for image and video searching, including sharing of media (for instance, photo sharing) and have a low risk of including objectionable content such as adult or porno graphic material. |
| Informational | Sites containing informational content such as regional information or advice |
| Infosec* | Sites featuring content related to Information Security |
| Internet Communication | Sites Related to internet communication and VOIP |
| IoT* | Sites related to the Internet of Things and IoT devices |
| Jobs | Sites that contain job search engines and other materials such as advice and strategies for seeking employment. |
| Kids* | Sites primarily related to kids and young adults |
| Malware Content* | Sites containing malicious software, viruses or malware, software hacks, illegal codes, and computer hacker-related material A common practice is to block the Malware category for all groups. |
| Marijuana* | Sites related to the production, use, or sale of marijuana |
| Messaging* | Site related to instant messaging and chat |
| Mobile Phones | Sites that sell or provide information and services about mobile (cellular) phones |
| News | Sites that provide current news and events, including online newspapers. Sites that primarily report information or comments on current events or contemporary issues of the day.  Also includes news radio stations and news magazines. May not include sites that can be better captured by other categories. |
| Nudity* | Sites containing any form of nudity |
| Online Meetings* | Sites related to software for online meetings or hosting online meetings |
| Organizations | Sites that contain content related to organizations that foster volunteerism for charity such as non-profits, foundations, societies, associations, communities, institutions. Also includes recognized pageants (Miss Earth), boys/girls scouts, and bodies that cultivate philanthropic or relief efforts. |
| P2P* | Sites related to Peer-to-Peer (P2P) file sharing |
| Parked Domains | Sites that are parked, meaning the domain is not associated with any service such as email or a website. These domains are often listed "for sale." |
| Phishing* | Sites and sites used in phishing and spearfishing campaigns |
| Piracy* | Sites related to digital piracy |
| Political | Sites that contain political content, including those representing political organizations or organizations promoting political views |
| Porn - Child | Sites that contain adult-oriented content, including sexually explicit graphics and material featuring children, or appearing to feature children. |
| Porn/Nudity | Sites that contain adult-oriented content, including sexually explicit graphics and material. Includes art with nudity, sex shops, and websites with ads showing nudity, games with nudity |
| Private Websites | Sites created by individuals containing personal expressions such as blogs, personal diaries, experiences or interest |
| Professional Services | Sites offering professional, intangible products, or expertise (as opposed to material goods). Includes sites for services performed expertly by an individual or team for the benefit of its customers. The typical services include cleaning, repairs, accounting, banking, consulting, landscaping, education, insurance, treatment, and transportation services. Also, includes online tutoring, dance, driving, martial arts, musical instrument lessons, and essay writing. |
| Real Estate | Sites focusing on real estate including agents, renting and leasing residences and offices, and other real estate information |
| Religion | Sites that promote or provide information regarding religious beliefs and practices |
| Remote Access Tools* | Remote access tools such as screen sharing services |
| Scams* | Sites related to scams |
| Search Engines | Sites that are used to search the web |
| Sex Ed | Sites that contain content relating to sexual education. Content may be graphic but is designed to inform about the reproductive process, sexual development, safe sex practices, sexuality, birth control, tips for better sex, and sexual enhancement products. |
| Shopping | Sites that are used to purchase consumer goods, including online auctions and classifieds. Includes event tickets |
| Spam* | Sites related to spam or used in spam email campaigns |
| Sports | Sites relating to sports and active hobbies. This includes organized, professional, and competitive sports in addition to active hobbies such as fishing, golf, hunting, jogging, canoeing, archery, chess, and so on |
| Streaming Radio/TV | Sites that contain streaming radio or television content |
| Suicide* | Sites related to suicide, including suicide information |
| Suspicious* | Sites that do not necessarily contain malware or malicious content, but, due to certain attributes, have been flagged as questionable. iboss' Malware Analysis classifies these sites as "Unsafe", even if no malware is detected. The Web Request Heuristic Protection, if active, also flags sites categorized as suspicious. |
| Swimsuit | Sites that contain sexually revealing content, but no nudity. Includes shopping sites for bikini, swimsuits, lingerie, and other intimate apparel.  |
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
