---
layout: doc
description: Single sign-on, secure remote access, URL, and content inspection and filtering for SaaS and web applications.
---
# Access Control

## Contributors

**Author:** [Daniel Feller](https://twitter.com/djfeller)

As users consume more SaaS-based applications, organizations must be able to unify all sanctioned apps, simplify login operations while still enforcing authentication standards, and capture user behavior. Is also is crucial to monitor vendor SLAs and application utilization analytics.

## Unified Experience

### Citrix Workspace

Citrix Workspace aggregates all resources into a single, personalized user interface. Users can either opt for a locally installed Workspace App (desktop and mobile) or use their local browsers to access a web-based workspace. Regardless of the selected approach and the chosen device, the experience remains familiar and consistent.

![Citrix Workspace App Overview](/en-us/tech-zone/learn/media/tech-briefs_access-control_workspaceapp-overview.png)

### Bring Your Own Identity

Each organization selects its own unique identity provider for users' initial logons to the workspace.

-  Azure Active Directory: The Workspace sign-in process is redirected to Azure Active Directory, which can incorporate custom branding, multi-factor authentication, password policies, auditing, etc.
-  Windows Active Directory: Utilizes an organization’s on-premises Active Directory environment for initial sign-on to the user’s workspace experience. In order to federate the on-premises Active Directory with Citrix Cloud, the administrator deploys redundant cloud connectors in the same resource location as Active Directory. Each cloud connector establishes an outbound link to the organization’s Citrix Cloud subscription.

[![Citrix Bring Your Own Identity](/en-us/tech-zone/learn/media/tech-briefs_access-control_bring-your-own-identity.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control_bring-your-own-identity.png)

### Single Sign-On

Once the user is authenticated to Citrix Workspace with a primary identity, subsequent authentication challenges to SaaS and web apps are automatically fulfilled by the Single Sign-On µ-service in Citrix Cloud using SAML assertions.

By default, the SAML assertion utilizes the email address associated with the user’s Active Directory account (identity provider) with the email address associated with the user's SaaS or web app account (service provider).

[![Citrix Access Control SSO](/en-us/tech-zone/learn/media/tech-briefs_access-control_sso.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control_sso.png)

## Content Control

To protect content, organizations incorporate enhanced security policies within the SaaS applications. Each policy enforces a restriction on the embedded browser when using Workspace app for desktop or on Secure Browser when using Workspace app web or mobile.

-  Preferred browser: Disables local browser use and relies on the embedded browser engine (Workspace app - desktop) or Secure Browser service (Workspace app – mobile and web).
-  Restrict clipboard access: Disables cut/copy/paste operations between the app and endpoint clipboard.
-  Restrict printing: Disables ability to print from within the app browser.
-  Restrict navigation: Disables the next/back browser buttons.
-  Restrict downloads: Disables the user's ability to download from within the SaaS app.
-  isplay watermark: Overlays a screen-based watermark showing the username and IP address of the endpoint. If a user tries to print or take a screenshot, the watermark will appear as displayed on the screen.

[![Citrix Access Control SSO Enhanced Security](/en-us/tech-zone/learn/media/tech-briefs_access-control_sso-enhanced-security.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control_sso-enhanced-security.png)

## Contextual Access

Although an authorized SaaS app is considered safe, content in the SaaS app actually can be dangerous - constituting a security risk. When a user clicks a hyperlink within a SaaS app, the traffic is routed through the web filtering µ-service, which provides a risk assessment for the hyperlink. Based on the hyperlink's risk assessment, and the customized list of URL categories, the web filtering µ-service allows, denies or redirects the hyperlink request from the user as follows:

-  Approved: the hyperlink is considered safe and it is accessed by the embedded browser within Workspace App
-  Denied: the hyperlink is considered dangerous and access is denied
-  Redirected: the hyperlink request is redirected to the Secure Browser service, where the user’s internet browsing activities are isolated from the endpoint device, the corporate network and the SaaS app.

[![Citrix Access Control URL Filtering](/en-us/tech-zone/learn/media/tech-briefs_access-control_url-filtering.png)](/en-us/tech-zone/learn/media/tech-briefs_access-control_url-filtering.png)

Most SaaS applications consume content from multiple websites. When utilizing web filtering, the SaaS application must be thoroughly tested to validate secondary sources of content are not being blocked or redirected. The time it takes for the user to select a URL and the browser to load the page might take slightly longer with enhanced security because each URL accessed within the SaaS application gets analyzed by the web filtering µ-service, which runs runs in Citrix Cloud.

## Security and Performance Analytics

Security analytics identifies risky behavior and provides an overall risk score for each user.

![Citrix Analytics](/en-us/tech-zone/learn/media/tech-briefs_access-control_analytics-brief.png)

Using machine learning and artificial intelligence, the Citrix Analytics service monitors user activities and identifies deviations from historical behavior that can increase or decrease the user’s risk score.

Users invariably access SaaS apps that have enhanced security inherent in them. Workspace app, the Gateway service and the Secure Browser service provide the Security analytics service with information about the following user and application behaviors because they impact the user’s overall risk score:

-  App launch time
-  App end time
-  Print action
-  Clipboard access
-  URL Access
-  Data upload
-  Data download

The web filtering µ-service evaluates the risk of each hyperlink selected within the SaaS application. Accessing these sites and monitoring changes in user behavior increases the user’s overall risk score because it signals the endpoint device is compromised and started to infect or encrypt data or the user and device are stealing intellectual property.
