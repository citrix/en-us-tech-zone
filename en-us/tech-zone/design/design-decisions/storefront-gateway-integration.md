---
layout: doc
h3InToc: true
contributedBy: Sarah Steinhoff
description: Learn about the different integration decisions involved when integrating StoreFront with Citrix Gateway for secure remote access.
tz_title: Designing StoreFront and Gateway Integration
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: Designing StoreFront and Gateway Integration

The purpose of this article is to dive a little deeper into Citrix Gateway integration with StoreFront: what the settings mean and design considerations for how to configure them.

## Gateway URLs, Call back URLs, and GSLB URLs

StoreFront allows administrators to define multiple Gateways that can be used for Gateway passthrough authentication in a single Store. This feature is greatly beneficial as it minimizes the number of Stores that have to be configured in large, global deployments. We see that there are four key parameters for this integration to work successfully: Citrix Gateway URL, virtual server IP address, Callback URL, and GSLB URL, which are discussed in the following sections.

### Gateway URLs

The first configuration screen in setting up a Gateway prompts the administrator to enter a friendly display name and the Gateway URL, which is the URL entered by users, as shown in the following:

![General Settings](/en-us/tech-zone/design/media/design-decisions_storefront-gateway-integration_1.png)
Figure 1: General Settings

StoreFront only allows Gateway passthrough from Gateways that are defined in its configuration. Citrix Gateway takes the URL entered into users’ web browsers or Workspace app (Receiver) clients and inserts it into an HTTP header (XCITRIXVIA). This information is passed back to StoreFront. StoreFront then attempts to match this value against one of the defined Gateway URLs. The **Gateway URL** field is mandatory and must match what is entered into a users’ web browser or Workspace app (Receiver) for Gateway passthrough to StoreFront to work.

### Callback URLs and Virtual Server IP Addresses

Next, we cover some optional fields: Virtual server IP address and Callback URL, whose usage is related and is covered together. These fields show up on the **Authentication Settings** screen of the Gateway configuration wizard, as shown below:

![General Settings](/en-us/tech-zone/design/media/design-decisions_storefront-gateway-integration_2.png)
Figure 2: Authentication Settings

The **Callback URL** is intended to be used by StoreFront to gather additional information about a user’s Gateway session. It is not strictly used for authentication. Instead, it queries for things like the name of the Gateway Session policy applied to the user’s session. These additional details can then be used as part of CVAD Access Control-based (SmartAccess) policy filters. It is also required in “password-less” authentication scenarios such as when Smart Card and SAML are in use to perform extra validation on the user’s Gateway session. If one of these scenarios is not in play, both the Callback URL and virtual server IP address fields are not required and can be left blank.

If a Callback URL is required and multiple Gateways are linked to a single Store, StoreFront needs a way of correctly identifying not just whether traffic is coming through a Gateway, but which Gateway virtual server the traffic is coming from to route the callback to the correct Gateway that holds the user’s session. StoreFront first does this action by matching on the Gateway URL, which it receives via the XCITRIXVIA HTTP header as previously covered. If there are multiple Gateways that have the same Gateway URL specified (which would occur in GSLB architectures where the same URL resolves to multiple individual Gateway virtual servers), StoreFront falls back to using an IP address to identify a Gateway, which is a unique value. StoreFront receives the virtual server VIP address from a Gateway via another HTTP header (XCITRIXVIAVIP). Once received, it then attempts to match against the value of the **vServer IP Address** field in one of its assigned Gateways. Assuming StoreFront can identify one Gateway based on a virtual server IP address match, then the callback completes successfully. Therefore, the virtual server IP address is only needed when a Callback URL is specified and there are multiple Gateways bound to a Store with the same Gateway URL defined.

### GSLB URLs

Lastly, a parameter that is not shown in the GUI: the **GSLB URL**. StoreFront version 3.9 introduced the ability *via PowerShell only* to specify a GSLB URL parameter to a Gateway definition. This GSLB URL functions as an alternate source that StoreFront accepts authentication requests from for that same Gateway definition. This parameter is visible in the **Get-STFRoamingGateway** command output. The purpose of this parameter is to decrease the total number of Gateways that need to be defined for a single Store to simplify administration. Without it, a Gateway object must be defined for every Gateway URL + unique Callback URL combination that can be used by users, which can add up quickly in an enterprise environment.

For example, three global Gateways behind a unified GSLB address (`https://www.nsg.com`): one in America, one in Europe, one in Asia, each with a unique Callback URL would require three defined Gateways. On top of that fact, these administrators want to be able to test authenticating at each Gateway individually. This is important in understanding if there are GSLB issues, resulting in alternate, unique names for each Gateway being defined. This instance means a total of six Gateways have to be configured for the Store, looking like the following:

| **Display Name** | **Gateway URL**         | **vServer IP Address** | **Callback URL**                |
|--------------|---------------------|--------------------|-----------------------------|
| **GSLB US**      | `https://www.nsg.com` | 1.1.1.1            | `https://callback-us.nsg.com` |
| **GSLB EU**      | `https://www.nsg.com` | 2.2.2.2            | `https://callback-eu.nsg.com` |
| **GSLB AP**      | `https://www.nsg.com` | 3.3.3.3            | `https://callback-ap.nsg.com` |
| **US Gateway**   | `https://us.nsg.com`  | 1.1.1.1            | `https://callback-us.nsg.com` |
| **EU Gateway**   | `https://eu.nsg.com`  | 2.2.2.2            | `https://callback-eu.nsg.com` |
| **AP Gateway**   | `https://ap.nsg.com`  | 3.3.3.3            | `https://callback-ap.nsg.com` |

Table 1: Complete Gateway List

By instead applying the GSLB URL parameter, the number of Gateways defined can be cut in half, while still allowing users to connect via all GSLB and unique Gateway addresses as follows:

| **Display Name** | **Gateway URL**        | **vServer IP Address** | **Callback URL**                | **GSLB URL**            |
|--------------|--------------------|--------------------|-----------------------------|---------------------|
| **US Gateway**   | `https://us.nsg.com` | 1.1.1.1            | `https://callback-us.nsg.com` | `https://www.nsg.com` |
| **EU Gateway**   | `https://eu.nsg.com` | 2.2.2.2            | `https://callback-eu.nsg.com` | `https://www.nsg.com` |
| **AP Gateway**   | `https://ap.nsg.com` | 3.3.3.3            | `https://callback-ap.nsg.com` | `https://www.nsg.com` |

Table 2: Consolidated Gateway List with GSLB URL

Even though the parameter is called “GslbUrl,” it functions just as an alternate host name for that Gateway definition. It does not *have* to be a GSLB address, just another name by which that same Gateway virtual server can be accessed. Another use case can be split DNS with external/internal Gateway addresses that are routed to the same virtual server.

Note, this parameter is not recognized by Workspace app (Receiver) and therefore usually, the URLs in the above example would be flipped so that Workspace app (Receiver) clients can continue to use the GSLB address and the per-Gateway URLs can be used when connecting through web browsers:

| Display Name | Gateway URL        | Virtual server IP Address | Callback URL                | GSLB URL            |
|--------------|--------------------|--------------------|-----------------------------|---------------------|
| US Gateway   | `https://www.nsg.com` | 1.1.1.1            | `https://callback-us.nsg.com` | `https://us.nsg.com` |
| EU Gateway   | `https://www.nsg.com` | 2.2.2.2            | `https://callback-eu.nsg.com` | `https://eu.nsg.com` |
| AP Gateway   | `https://www.nsg.com` | 3.3.3.3            | `https://callback-ap.nsg.com` | `https://ap.nsg.com` |

Table 3: GSLB URL Adjusted for Receiver & Workspace app

### Design Takeaways

To summarize the previous sections:

*  Gateway URL is always needed and must match what is entered into web browsers or Workspace app (Receiver) clients
*  Callback URL is only needed if SmartAccess CVAD policies or password-less authentication methods (Smart Cards, SAML, and so on) are used
*  Virtual server IP address is only needed if a Callback URL specified and there are multiple Gateways bound to the Store that have the same Gateway URL specified
*  GSLB URL is a new parameter that was added in StoreFront 3.9. The URL simplifies Gateway integration with StoreFront when a single Gateway virtual server can be accessed via multiple URLs
*  Workspace app (Receiver) does not read the GSLB URL parameter, so the Gateway URL is always been the URL that is used by those clients and the GSLB URL is an alternate URL that can be used by web browser-based connections

## Selecting a "Default Appliance"

When an administrator enables Remote Access for a Store, a “default appliance” must be defined as shown in the screenshot.

![Default Appliance](/en-us/tech-zone/design/media/design-decisions_storefront-gateway-integration_3.png)
Figure 3: Default Appliance

For web browser-based access, the “default appliance” setting has no impact. For Workspace app (Receiver)-based access, this setting is downloaded to Receiver on connection to the Store as part of the Store configuration. Gateway is used thereafter by default. If all defined Gateways share the Gateway URL, then again, occurence has no impact (Receiver just uses that Gateway definition to pull the URL to query for authentication, so in GSLB configurations, that query is GSLB routed). As noted earlier, the Workspace app (Receiver) does not read the “GSLB URL” parameter, so the Gateway defined as the default appliance must have an appropriate Gateway URL defined, as shown in “Table 3: GSLB URL Adjusted for Receiver & Workspace app” in the previous section.

If the Gateways bound to the Store have different Gateway URLs, then whichever one is defined as the default will be used by all Receiver clients. This occurrence is problematic if you have defined different Gateways to be used by different sets of users. For instance, one Gateway configured with LDAP+RADIUS authentication for users with tokens and one Gateway configured with Smart Card authentication. If the user enters the Smart Card Gateway URL into the Workspace app (Receiver), but StoreFront has the LDAP+RADIUS Gateway defined as the default one, after the Workspace app (Receiver) connects to StoreFront and caches the configuration, all future authentication requests will be sent to the LDAP+RADIUS Gateway, despite what the user originally entered. The only way around this issue is a separate Store or Server Group.

### Design Takeaways

To summarize:

*  Store with Remote Access enabled has a default Gateway that defines which Gateway URL is used by the Workspace app (Receiver) clients
*  To use multiple Gateway URLs and Workspace app (Receiver)-initiated access, separate Stores, or StoreFront server groups must be defined

## Optimal HDX Routing

Outside of performing authentication, another reason Gateways can be defined in StoreFront is for Optimal HDX Routing. This setting assigns a Gateway per Citrix Virtual Apps and Desktops Site or Zone. The purpose of the setting is to reroute the ICA connection through a Gateway that can be different from the user’s original authentication point (such as through a Gateway that is closer to the VDA hosting the user’s session). If this “optimal” Gateway is not otherwise performing authentication, it only needs STA servers bound for the session proxy and can be set up in StoreFront as an “HDX Routing only” Gateway type, which eliminates all of the **Authentication** settings.

When assigning that Gateway to a Site (via Manage Delivery Controllers) or Zone (via Manage Zones) in the Store settings shown below, there is an optional **External only** check box.

![Optimal HDX Routing](/en-us/tech-zone/design/media/design-decisions_storefront-gateway-integration_4.png)
Figure 4: Optimal HDX Routing

That setting means that the “optimal” Gateway will only be used for ICA sessions originating “externally,” meaning sessions that use Gateway passthrough as the authentication type (which StoreFront assumes means the user is originating outside the corporate network). The setting will not apply to users who authenticate directly at StoreFront (which StoreFront assumes means the user is inside the corporate network). If the box is cleared, then all ICA sessions destined for that Site or Zone will be routed through the defined Gateway, whether the user is external or internal. There is no “internal only” check box. That means that the defined Gateway URL needs to be reachable from all possible user locations, which can be challenging without split DNS. This is another case where separate Stores (since this is a Store-level setting) may be required for complex architecture scenarios with Optimal HDX Routing.

### Design Takeaways

To summarize:

*  Gateway used for Optimal HDX Routing only requires STA servers bound
*  Gateways used for Optimal HDX Routing can either be “external only” or “external and internal,” but not “internal only”
*  Separate Stores or Servers Groups are required to define separate internal and external Gateway URLs for Optimal HDX Routing

## Beacons

Beacons are defined separately from the Gateways in the StoreFront configuration and are enabled automatically when you Enable Remote Access for a Store and configure the first Gateway. Beacons are website addresses that to help the Workspace app (Receiver) identify whether the endpoint client is inside or outside the corporate network and seamlessly route the access request to either the StoreFront base URL, if the client is determined to be “internal,” or the default Gateway address, if the client is determined to be “external” without prompting the user for more input. To do this, the Workspace app (Receiver) will first query the internal beacon address (assuming the external, internet-facing address can always be reachable) and then fall back to the external beacon addresses only if the internal fails. If it can reach the internal URL (gets an HTTP 200 response), then the client is assumed to be on the corporate network and will be directed to the StoreFront base URL.

![Manage Beacons](/en-us/tech-zone/design/media/design-decisions_storefront-gateway-integration_5.png)
Figure 5: Manage Beacons

Beacons are not used at all if a user is connecting through a web browser. This is because a user provides input by typing a URL into the browser bar and therefore is directing the browser to a specific address. With the Workspace app (Receiver), after initial configuration, the user never provides further input on what URL to use. The Workspace app (Receiver) has the base URL and any configured Gateway URLs cached as part of the configuration and needs to be able to intelligently choose on behalf of the user which URL to use when the user attempts to use the application.

There should no need to modify or tweak the beacon addresses unless the same Gateway URL and StoreFront base URL are used. In this case, the external and internal beacons would then be the same and the internal URL would be accessible externally, which defeats the purpose. Therefore, an administrator would need to choose to “Specify beacon address” for the internal beacon and enter a website that is only accessible from the corporate network. Otherwise, it is simply a good troubleshooting practice to understand how the beacon addresses are applied so that they are not examined when investigating issues with web browser-based access.

### Design Takeaways

To summarize:

*  Beacons are only used Workspace app (Receiver) clients
*  Beacons are only be modified if the StoreFront base URL matches a Gateway URL (and is accessible from outside the corporate network)

## References

[Integrating Gateway and StoreFront](/en-us/storefront/current-release/integrate-with-netscaler-and-netscaler-gateway.html)

[Configuring Optimal HDX Routing](/en-us/storefront/current-release/set-up-highly-available-multi-site-stores.html)

[StoreFront New Features](/en-us/storefront/current-release/whats-new.html)
