---
layout: doc
description: Integrates the Access Control service with an on-premises Citrix Virtual Apps and Desktops deployment utilizing StoreFront.
---
# Access Control for StoreFront

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

Citrix StoreFront provides app aggregation services for an on-premises Citrix Virtual Apps and Desktop deployment where users can launch Windows virtual apps and desktops, Linux virtual apps and desktops and published content to SaaS and web applications. Integrating Citrix Access Control with StoreFront enhances access to SaaS and web applications by incorporating single sign-on, application protection policies and website filtering.

## Access Control

Citrix StoreFront allows admins to publish content/links to SaaS and web applications. However, the published links simply equate to browser favorites eliminating the need for users to remember the address.
Citrix Access Control service, within Citrix Cloud, incorporates numerous technologies to improve the user experience and security for SaaS and web applications. At a high-level, Access Control provides:

-  Single Sign-On: Utilizing SAML-based authentication for SaaS applications and Basic, Kerberos and Forms-based authentication for internal web applications, Access Control allows users to connect to the application without being forced to enter a user name and password.
-  Enhanced Security: With app protection policies, Access Control allows administrators to allow/deny actions within the browser session, including restricting downloads, printing, screen captures, keylogging, and more.
-  Website Filtering: Most SaaS and web applications are safe to use, but the content within the applications can be dangerous. Access Control analyzes website links and either allows, denies or redirects the request to keep the user and device safe.
-  Remote Access: Communication between external users and web applications, hosted on-premises, must traverse the firewall. Access Control uses the Citrix Gateway Service, utilizing an outbound-only connection, helping to simplify implementation and overcome networking challenges of allocating public IP addresses, procuring certificates and implementing fault tolerance/business continuity.
-  Embedded Browser: On endpoint devices utilizing a fully installed Workspace app, SaaS and web applications utilize the embedded browser engine. The embedded browser provides a local, native user experience protected with enhanced security and website filtering.
-  Secure Browser: On endpoint devices utilizing the HTML5 version of Workspace app, SaaS and web applications utilize a cloud-hosted, virtual and disposable browser managed by Citrix. Secure Browser breaks the direct connection between the endpoint and the SaaS/web application, helping to disrupt malware transmission, while still adhering to the enhanced security and website filtering policies.

## StoreFront Integration

Utilizing published content functionality within StoreFront, organizations can integrate Citrix Access Control policies with SaaS and web applications.
The Access Control Sync Utility for StoreFront, available in [https://www.citrix.com/downloads/](https://www.citrix.com/downloads/), captures SaaS and web application resources defined in the organization’s Citrix Cloud account and synchronizes them into the on-premises Citrix Virtual Apps and Desktops environment as published content.
The newly created published content includes configurations pointing to the organization’s Access Control subscription for policy enforcement.  Once published, the user flow for SaaS apps is as follows:

[![Single Sign-On to SaaS Apps](/en-us/tech-zone/learn/media/tech-briefs_access-control-storefront_saas-apps.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control-storefront_saas-apps.png)

For on-premises web applications, the flow is as follows:

[![Single Sign-On to Web Apps](/en-us/tech-zone/learn/media/tech-briefs_access-control-storefront_web-apps.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control-storefront_web-apps.png)

## Considerations

Integrating Access Control with an on-premises deployment of StoreFront includes the following design considerations:

-  Sync Utility: The Access Control sync utility is not required for day-to-day operations.  Administrators utilize the sync utility only when new SaaS and web applications are created within Citrix Workspace. Once synchronized, the application information is stored within the Citrix Virtual Apps and Desktops site database, which is available regardless or the current state of the cloud-hosted services.
-  Resiliency - Gateway service: Spread across 12+ global Points-of-Presence (PoPs), Citrix Intelligent Traffic Management directs users to the best Gateway Services site, helping to bypass sites with service outages.
-  Resiliency - Access Control service: Spread across multiple locations, requests for enhanced security policies are directed to the best responding PoP, helping to bypass sites with service outages.
-  Resiliency - Website filtering micro-service: Spread across multiple locations, requests for URL analysis and filtering are directed to the best responding PoP, helping to bypass sites with service outages.
-  Resiliency - Secure Browser service: Spread across 12+ global Points-of-Presence (PoPs), Citrix Intelligent Traffic Management directs users to the best Secure Browser services site, helping to bypass sites with service outages.

## Additional Details

[Access Control for SaaS Apps](/en-us/tech-zone/learn/tech-briefs/access-control.html)
