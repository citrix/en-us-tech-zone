---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks, Florin Lazurca, Rob Sanders, Martin Zugec, Allen Furmanski
description: Learn how Citrix solutions enable organizations to meet the European GDPR data privacy laws while also meeting business objectives.
tz_title: Architectural Considerations for the General Data Protection Regulation - GDPR
tz_products: security;
---
# Reference Architecture: Architectural Considerations for the General Data Protection Regulation - GDPR

## General Data Protection Regulation (GDPR) Overview

GDPR is a set of data privacy rules that apply broadly to both companies in the European Union (EU) in addition to any company globally that collects and uses data pertaining to EU residents. The GDPR went into effect on May 25, 2018 and includes several chapters which are further broken down into numbered “articles” or subsections that we will refer to in this document. These articles describe the specific requirements applicable to the handling of personal data.

### GDPR seeks to

*  Unify the different data protection regulations adopted by EU member states
*  Protect the data privacy of EU residents
*  Ensure that organizations handle personal data in a responsible and accountable manner from collection through to return or destruction

### The GDPR applies to

*  Organizations operating within the EU
*  Organizations operating outside of the EU who offer goods and services to EU residents

**Note:** Privacy considerations for GDPR span the data lifecycle from collection, usage, storage, and secure disposal and retirement of data. It covers all personal data relating to your customers, employees, supply chain, partners, and anyone else about whom you collect personal information who resides in the EU. Personal data is any information relating to an identifiable person, therefore it is often referred to as personally identifiable information (PII). This can include information such as names, photographs, IP or email addresses, and medical information. For further detail on Citrix’s data lifecycle processes and practices, visit the [Citrix Trust Center](https://www.citrix.com/about/trust-center/).

### Two key GDPR articles are the focus of this document

*  Access control (included in Article 25) - calls out measures which shall ensure that by default personal data is not made accessible to an indefinite number of persons.
*  Encryption and data protection (included in Article 32) - calls out:
    *  Pseudonymization and encryption of personal data
    *  Ensure the ongoing confidentiality, integrity, availability, and resilience of processing systems and services
    *  The ability to restore the availability and access to personal data in a timely manner in the event of a physical or technical incident
    *  Protection from accidental and unlawful destruction, loss, alteration, unauthorized disclosure of, or access to personal data transmitted, stored, or otherwise processed

### How Citrix can help you with your GDPR compliance initiatives

Citrix Workspace simplifies the management of your systems and data by centralizing
services in the data center or cloud as a digital workspace. The goal of this document is to
describe how it unifies applications, data, and desktops into a digital workspace for your teams. One that allows you to better align with GDPR requirements around data management, data monitoring, and information auditing.

#### Citrix supports clients on their journey to GDPR compliance in 4 key ways

*  By centralizing and enclaving applications and data
*  By helping to ensure data is protected when shared or distributed
*  By controlling who has access to data and resources
*  By bringing IT together for application and data-specific security

### Citrix Workspace - helping to enable GDPR compliance

[![Citrix Workspace helps enable GDPR compliance](/en-us/tech-zone/design/media/reference-architectures_gdpr_citrix-solution.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_citrix-solution.png)

## Data oriented approach to GDPR requirements

Following the GDPR guidelines might be much easier for modern cloud companies than traditional enterprises. While most cloud companies have only a few centralized data sources where personal data is stored, traditional companies potentially have hundreds or thousands. These different data sources need to be assessed, reviewed, and updated to meet the latest data privacy standards.

These data sources can range from traditional SQL databases to emails, digital documents, or even physical documents. With today's often aggressive timelines, many enterprises face a challenge to properly prepare and update systems as required. It is important to understand that the GDPR doesn’t affect only active data sources, but also all backups, disaster recovery sites and physical printouts.

GDPR involves two key roles for data: the data controller and the data processor. The data controller is the entity who determines the purpose of the data and how the data is to be handled. The data processor handles or processes the actual data per the controller's guidelines.

The GDPR is all about increasing the maturity of the company when dealing with data and personal information. Citrix has always been a data and application oriented company, with a proven record of handling complex, often international projects that are dealing with thousands of applications.

The traditional consulting approach is focused on the business processes, identifying people and business requirements, access methods and slowly cascading down to infrastructure and data sources. However, the GDPR requires a more data centric approach. We recommend starting with identifying and assessing various locations where personal data is stored. Then move higher up the stack to make sure that data sources are properly secured. You can think about this as an inside out approach to security.

![GDPR Flow Diagram](/en-us/tech-zone/design/media/reference-architectures_gdpr_flow.png)

**Define** - Start by defining the criteria of personal data that is in scope of the assessment. This phase helps you define what to look for and how to prioritize data sources from a privacy perspective. This can include employees, customers, vendors, and any other relevant entities.

**Assess** - Analyze all the existing locations where data is stored. Identify the business requirements, data retention, and potential challenges to securing the data. Identify not only where the data is stored, but also how it’s being collected. Data segmentation is one of the most time consuming and critical phases of data consolidation projects. This phase requires a comprehensive approach, critical thinking, and well-defined methodology. Organizations need to consider data held in legacy systems. This is true even if there is a program in place to modernize or if the data is used only as a backup. It is important to understand that all these legacy systems are covered by the GDPR requirements as well and companies need to take a holistic approach.

**Reduce** - The goal of this phase is to identify if it is possible to reduce the number of data sources that need to be secured. For example, it is possible to consolidate data sources that are being used by business units. Instead of storing client data in multiple locations, a centralized location can be used to maximize effectiveness of security measures. Maybe the data is not even needed at all – the biggest privacy offender might not even be considered critical for the business units. It is also possible that applications are simply collecting too much data (“just in case”). The applications can be modified to stop collecting excessive information and the existing data can be erased. Instead of trying to secure all the possible locations of personal data, companies can take a different approach. Instead, ask when and where do they actually need to store data about customers and other parties. As GDPR compliancy is an ongoing process with periodic reviews, minimizing the number of included data sources can prove to be an effective long-term strategy.

**Remediate** - Identify if existing data sources and applications used to access them are following the GDPR guidance, or if changes are needed. If the data source includes personal data and is not secure, identify the possible approaches to solve the situation. A cross-departmental GDPR team can also identify, assess, and review not only the data itself, but also access methods, applications used and other factors. This includes items such as limiting user and third party access, revisiting requirements, and more specifically defining data security measures.

**Review** - GDPR compliance is an ongoing process and data assessments need to be performed regularly. It is therefore important to implement a robust, stable, and repeatable process that can be defended if it ever needs to be presented to auditors. The data sources security assessment needs to be performed and reviewed regularly.

With the large number of data sources that are in scope, the goal for most companies is to choose
a few, robust, and proven architectures. This approach can help secure data sources that don’t initially meet GDPR requirements. Trying
to create a tailor-made solution for each of the problematic data sources is unrealistic, unless a company has limited data sources. The result is often an environment where only a few applications are properly secured, while the majority is left unsecured, with the implementation project stalling by months or even years and going well over budget. GDPR also presents an opportunity to update privacy architectures across applications and data usage to support evolving global and regional privacy initiatives.

Complexity is considered one of the biggest enemies of security. You want to identify the minimum number of different architectures to secure most of the data sources identified as critical and storing data included in the GDPR scope.

The Pareto principle (also known as the 80/20 rule) is important during this data assessment. Companies need to try to minimize the effort required to secure most data sources. Most enterprises have hundreds or thousands of different applications and data sources that are used. They need to promptly identify the applications that contain critical data and don’t meet the GDPR requirements. Automated application assessment solutions can reduce the time required to analyze applications.

In the following sections, we present a few selected architectures that can provide a universal, secure, and proven solution to help secure any type of data. This ranges from web-based applications, through legacy client/server applications hosted on Windows or Linux to data stored in various documents or exchanged through emails.

**Decision Flow for Data Types**
[![Citrix Workspace helps enable GDPR compliance](/en-us/tech-zone/design/media/reference-architectures_gdpr_data-flow-diagram.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_data-flow-diagram.png)

## Securing Windows and Linux Applications

Trying to secure traditional client/server applications, whether they are running on Windows or Linux operating systems, can prove challenging for various reasons.

The traditional approach was to secure each endpoint where these applications are installed. This involves management challenges, such as keeping all the endpoints up to date, encryption of the network traffic, data and workload encryption, implementing multifactor authentication (MFA) and encryption of the locally stored or cached data. In traditional IT architecture, defenses need to be set up around all endpoints, applications, and networks and the whole environment is only as secure as the weakest point. This traditional approach to security has often failed due to the introduction of new concepts including mobile workforce, expansion of security perimeter
with cloud computing and BYOD initiatives.

Another common challenge for applications installed on traditional computers is to provide the same security functionality across the whole portfolio. It’s common to have a multigenerational IT portfolio residing on a single workstation. From Office-based applications (using Microsoft Access databases or custom plug-ins) through legacy Visual Basic to the latest professionally built applications. Making sure that applications with access to sensitive data support encryption, multifactor authentication and provide enough information for auditors has always been complicated.

**Traditional Client/Server App Delivery**
[![Traditional client/server app delivery diagram](/en-us/tech-zone/design/media/reference-architectures_gdpr_client-server-apps.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_client-server-apps.png)

Citrix has a long tradition of providing a platform for the secure delivery of these client/server applications. This secure delivery is based on offloading the client application piece onto a dedicated set of servers (Citrix Virtual Apps and Desktops), specially designed, optimized, and secured for application delivery. By decoupling the application from the endpoint, it's possible to enable extra security features. The advantage of this approach is that security features are applied consistently. There is also no requirement for source code access and even applications that are no longer supported on newer platforms can be included.

### Securing Applications to GDPR Standards

#### Article 25 - Access to Personal Data

There are multiple ways to limit or prevent users from accessing published resources. The most basic method is to simply hide the applications or desktops from users by enforcing Active Directory group membership. When publishing resources through the Citrix management console, the access is enforced on the Citrix Virtual Delivery Agent (VDA) machines hosting the workloads. Access and available functionality is further tweaked through Citrix's comprehensive policy engine.

The use of traditional user name/password authentication is decreasing with more secure multifactor authentication (MFA) increasing. Even for internal networks, more companies are enforcing MFA requirements to enhance security. With Citrix Virtual Apps and Desktops and Citrix Application Delivery Controller (ADC), MFA can be applied to any client/server application including even legacy applications that are hard to maintain. The ADC appliance provides an extensible and flexible approach to configuring MFA, from time-based one-time tokens, through smart cards, user or machine certificates to biometric authentication (through third party integration).

This access can also be configured based on various other factors. For example the endpoint a user is connecting from, the security state of endpoints such as antivirus or firewall requirements or the network where the user is connecting from. Context-aware policies can be applied, even enforcing a specific geo-location or using more advanced security measures, such as requirements for user or machine certificates to access certain resources. You can learn more about context-aware security through the zero-trust model in the following [blog post](https://www.citrix.com/blogs/2019/10/02/approaching-a-zero-trust-security-model-with-citrix-workspace-part-1/).

Even more flexibility is available through Citrix ADC’s enhanced MFA feature call nFactor authentication. To learn more about different
capabilities of nFactor authentication, refer to the following knowledge base article and one of the many deployment guides: [https://support.citrix.com/article/CTX201949](https://support.citrix.com/article/CTX201949)

The ability to deliver centralized access and authentication is critical in providing information about users connecting to
applications. With Citrix Virtual Apps and Desktops, all access to resources is brokered through a controller with historical data saved in a centralized database. This data can be accessed from an ODATA API to provide integration with SIEM systems or provide copies for auditors if needed. To learn more about monitoring and reporting, refer to the doc on how to [Monitor historical trends across a Site](/en-us/citrix-virtual-apps-desktops/director/site-analytics/trends.html).

Aside from the monitoring and reporting of user access, all administrative changes and activities can be logged to a separate database. It is recommended to enable mandatory logging, where administrative activities are not allowed unless they are logged in the Configuration Logging database first. To learn more, refer to the [Configuration Logging Documentation](/en-us/citrix-virtual-apps-desktops/1912/monitor/configuration-logging.html)

Finally, for the most security-conscious environments, it is possible to create a separate set of user identities and automatically switch to them. This is done using the Federated Authentication Service. The approach can be used to further minimize the impact of lateral movement and contain any security breach.

#### Article 32 - Data Encryption in Transit

With Citrix Virtual Apps and Desktops, only screen pixels are transferred between the hosting server and the endpoint. Connection parameters are established during session initiation or reconnection. CVAD can ensure that traffic coming to and from the endpoint is always encrypted, even if the application itself doesn’t support encryption. This encryption can be enabled for any published application or desktop. For details on end-to-end encryption, refer to [this document](https://www.citrix.com/content/dam/citrix/en_us/documents/white-paper/end-to-end-encryption-with-xenapp-and-xendesktop.pdf)

#### Article 32 - Data Encryption at Rest

While Citrix Virtual Apps and Desktops can help with the encryption between user and application, the back-end itself remains out of scope for this solution. However, CVAD can be used to isolate unencrypted traffic and data during the transition period utilizing secure zones. This makes sure that all data is encrypted with a long-term process. Encapsulating this data in an isolated enclave can provide the required security while the migration project is underway. You can read more about secure zones in this blog post: [“Unsinkable”: The Myth of Foolproof IT Security](https://www.citrix.com/blogs/2017/10/31/unsinkable-the-myth-of-foolproof-it-security/).

As for encryption on the endpoint, it is important to minimize data exposure at the endpoint and control data remanence. Data residing on the endpoint needs to be restricted, delivering only the minimum amount of data necessary. Virtualizing all access to personal data and then managing and protecting residual data, keystrokes and screen data is the Citrix approach. To learn more, read this blog post about the [Citrix ICA client footprint](https://www.citrix.com/blogs/2017/08/31/citrix-ica-client-what-leaks/).

#### Article 32 - Data Isolation and Protection

Contrary to traditional desktops on physical endpoints, server, and desktop OS images within Citrix Virtual Apps and Desktops usually have a much more restricted scope of operation. They are used to host a group of well-defined, centrally managed applications and desktops with predictable behavior and centralized configuration options.

There are different ways how data and applications can be protected and isolated from each other. With aggregation of resources from multiple servers, it is possible to create groups of separated servers to host different applications with different trust levels.

Even if applications are hosted on the same server, it is common practice to isolate and secure them. CVAD servers are used to
host well-defined, centrally managed sets of applications. This means that these servers can support more restrictive security hardening than traditional workstations. Application whitelisting solutions such as Citrix's Workspace Environment Management (WEM) Application Security feature are much more useful with servers built for specific applications rather than general workloads. Allowing only specific white-listed executables is much simpler on these special-purpose built servers than general workstations.

Security is enhanced even more by application of granular Citrix policies. These policies provide control over many aspects of
the workspace: available printers, ability to access network and local drives, or clipboard mapping among many more. A special template for “Security & Control” is included with all the best practices and recommended settings.

To learn more about hardening, refer to the [System Hardening white paper](https://www.citrix.com/content/dam/citrix/en_us/documents/products-solutions/system-hardening-for-xenapp-and-xendesktop.pdf).

Given that users often connect from untrusted devices and locations, extra security layers are often welcomed. Citrix [App Protection Policies](/en-us/citrix-virtual-apps-desktops/secure/app-protection.html) provides anti-keylogging and anti-screen-capturing while users are connected in virtual app and desktop sessions. Sensitive data, such as credit card numbers, is hidden from malicious programs attempting to intercept. [Session Watermarking](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-watermark-policy-setting.html) allows administrators to place customizable text overlays in sessions. [Session Recording](/en-us/session-recording) saves the entire video stream of a session to a file for later playback. These features help deter and remediate possible data exfiltration events.

**Aggregation of Applications with Different Trust Levels**
[![Citrix Workspace helps enable GDPR compliance](/en-us/tech-zone/design/media/reference-architectures_gdpr_apps-trust-levels.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_apps-trust-levels.png)

## Securing Web and SaaS Applications

Web apps are architecturally different from client/server apps yet also similar in many ways – including suffering from security challenges. With web apps, a specific client is replaced with a single corporate standard or multiple general-purpose web browsers,
with varying capabilities and dependencies. Although simplified, the same management challenges apply – keeping the browser up-to-date against vulnerabilities, encryption of traffic, and implementing multifactor authentication.

The demands for legacy application support and modern capabilities for SaaS have driven conflicting requirements. There are two types
of web apps – the born-on-the-internet-apps and webified apps-custom and legacy web apps that support the business. The born-on-the-internet apps drive the requirements on security and architecture – load balancing, scalability, failover, and performance. Webified apps drive the requirements on supportability - browser plug-ins, extensions, and validating browser updates can break functionality.

The goal is for the end user to interact with web apps and manipulate data. This includes personal and sensitive data regardless if running in legacy environments or on SaaS apps pushing the limits of HTML5. Gartner recommends a two-pronged strategy. This is when an organization uses a legacy browser for running legacy applications, but also employs modern browsers for all other applications. That’s where Citrix helps – tying together the user experience and security requirements for hybrid or bimodal web environments.

For the enterprise, maintaining multiple versions of various browsers, plug-ins, and applets while also simplifying
authentication and access is a pain point. It is addressable with Citrix Virtual Apps and Desktops or [Citrix Secure Browser](https://www.citrix.com/products/citrix-secure-browser/) offerings. These solutions allow organizations to build a remote browsing infrastructure that separates internet and intranet web traffic from each other and the endpoint.

**Traditional Web Applications Architecture**
[![Traditional Web Applications Architecture](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps.png)

The second challenge is providing unified security features across the wide range of SaaS and cloud applications that typical enterprises are using. This effectively helps to give back some IT control – especially in BYOD and mobile environments. An example is providing a unified multifactor authentication solution across all apps instead of a fragmented user experience.

Citrix has a long tradition of providing a platform for secure delivery of web apps. This secure delivery is based on the Citrix Application Delivery Controller (ADC). Citrix ADC secures the session between the browser and the web app – by encrypting
data in transit, maintaining strict access control, and data protection. This is largely based on its design as a reverse proxy that brokers connections coming from the browser to web app servers. And, with its position between the client and the server, extra security features can be enabled.

We’re going to cover how this architecture can help you secure the applications to follow GDPR standards. These technical measures fall largely under the requirements or Article 25 and 32 where controllers are required to “implement appropriate technical and organizational measures.”

**Web Applications with Citrix ADC**
[![Web Applications with Citrix ADC Diagram](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc.png)

### Article 25 - Access to Personal Data

Authentication, Authorization, and Auditing are all core to controlling access to personal data. With Citrix ADC AAA proxy, it consolidates, extends, and enhances the traditional authentication schemes even in scenarios where the web apps do not natively support MFA. Citrix ADC supports authentication using user name/password, multifactor (MFA), time-based and one-time tokens (Citrix ADC has native One-time PIN support), smartcards, user or machine certificates and biometrics. While this is especially important for internet facing web apps, some organizations with a zero trust networking approach are moving to require MFA for “internal” access.

Citrix ADC's enhanced MFA, called nFactor authentication, takes into account capabilities such as SAML, client certificates, group extraction, and multiple passwords. Federation and SSO also provide an extra level of security and ease of use.

To learn more about different capabilities of nFactor authentication, refer to the following knowledge base article and one of the many
deployment guides: [https://support.citrix.com/article/CTX201949](https://support.citrix.com/article/CTX201949). On a related note, SMS-based MFA is not recommended as it has been deemed insecure by NIST.

To understand how Citrix protects personal data, refer to the [Citrix Trust Center/Privacy Policy](https://www.citrix.com/about/trust-center/privacy-compliance.html).

Logging, visibility, automation, and other capabilities are provided by Citrix Application Delivery Management (ADM). Refer to the [product landing page](https://www.citrix.com/products/citrix-application-delivery-management/) for more details on Citrix ADM.

#### Article 32 - Data Isolation and Protection

Citrix ADC is a reverse proxy and as such it benefits from its location in the network architecture. Typically it is in a DMZ or security zone. From here it accepts the front-end user connection, creates a secure connection to the back-end server, and has full visibility into requests and responses. Also, Citrix ADC can change the logic of the web traffic on the fly without requiring updates to the back-end application. This includes encryption of not only the packet header but also the body as it does deep packet inspection and rewrite.

Citrix ADC can ensure that traffic coming to and from the browser is always encrypted, even if the web server itself doesn’t support encryption. This encryption can be enabled for any site proxied through the ADC. SSL offloading uses the ADC to perform the resource intensive SSL/TLS handshakes thereby offloading them from the back-end servers. For scenarios requiring end-to-end encryption, Citrix ADC can re-encrypt the connection to the back-end. This allows the ADC to inspect and apply security policies to the traffic. SSL bridging is available for when requirements demand that the ADC plays no part in terminating the connection. Using Citrix ADC with Citrix ADM allows administrators to keep central configuration and visibility of the cipher suites in use, helping prevent negotiation of outdated ciphers.

**Citrix ADC Encryption Options**
[![Citrix ADC Encryption Options](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-adc-encryption-options.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-adc-encryption-options.png)

As a proxy between the browser and the web app, Citrix ADC protects the data flowing through it. That includes protecting from
attacks against databases, attacks against the web app, and other users using its built-in application firewall. Citrix ADC protects against common web attacks including SQL Injection and cross-site scripting. You can read more about the Web App Firewall in our [product documentation](/en-us/citrix-adc/current-release/application-firewall.html).

Protecting data also includes maximizing availability through Denial of Service (DoS/DDoS) attack protections. Combination attacks hit at all layers—so Citrix ADC provides Application layer defense (Layer 7), Transport layer defense (Layer 4) and Network layer
defense (Layer 3). Citrix ADC not only provides a multi-layer approach to DDoS protection but it is coupled with a built-in IP Reputation service. It is an effective tool in identifying the IP address that is sending unwanted requests. Since most malware comes from compromised sites, you can use the IP reputation list to preemptively reject requests that are coming from the IP with the bad reputation. Citrix ADC's forward proxy, Secure Web Gateway, can filter out connections going out to the internet based on reputational risk. This enforces security policies on outgoing web traffic, while blocking access to inappropriate sites on a per user/group basis.

**Citrix ADC Tokenization**
[![Citrix ADC Tokenization](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc-tokenization.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc-tokenization.png)

**Citrix ADC Pseudonymization**
[![Citrix ADC Pseudonymization](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc-pseudo.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-citrix-adc-pseudo.png)

Pseudonymization is another control mentioned in Article 32. Conceptually, it’s a procedure by which identifying fields
within a data record are replaced by one or more artificial identifiers, or pseudonyms. This makes storing personal data more secure
in the event of a breach – by using data segmentation. An example is tokenizing or hashing sensitive data that Citrix ADC parses for web-application traffic. This means hashing personally identifying data while transmitted between a controller and a processor. This is done in PCI-DSS regulated environments. For example, for cardholder data, tokenization guidelines are specific for the Primary Account Number (PAN). Tokenization replaces the PAN with a surrogate value called a token. De-tokenization is the reverse process of redeeming a token for its associated PAN value. The security of an individual token relies predominantly on the infeasibility of determining the original PAN by knowing only the surrogate value. Applications may not need as much security protection as associated with the use of PAN. For GDPR, storing tokens instead of personal data is one alternative that can help to reduce the amount of personal data in the environment, potentially reducing the effort required to adhere to GDPR requirements.

**Citrix ADC Data Protection**
[![Citrix ADC Data Protection](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-adc-data-protection.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_web-apps-adc-data-protection.png)

## Securing Mobile Applications

Mobile devices, particularly with BYOD ownership, present many challenges to enterprises trying to secure data. Their use within the enterprise has driven the inception of technologies to securely manage mobile endpoints. Used beyond the borders of enterprise DMZs on any network, with apps from various sources, mobile devices present special risks to companies and their data.

*  The GDPR data controller must secure personal data used by corporate mobile apps despite the fact they’re hosted on a user-owned mobile device.

*  The GDPR data controller must ensure the confidentiality, integrity, and availability of the personal data during its use. It must also ensure that when a user exercises their right to erase their personal data that no artifacts are left behind and exposed to other apps, users, and so forth.

*  Data controllers must support file sharing and collaboration securely between enterprise mobile apps and be able to erase files from the device in a moment’s notice.

*  They must help protect the platform OS, mitigate the risk of malware, and enforce device security and pertinent policies to control device functions that make data vulnerable to loss.

*  Data controllers must provide Unified Endpoint Management across multiple platforms including control of critical software patches that include updates to address vulnerabilities.

**Traditional Mobile Application Architecture**
[![Traditional Mobile Application Architecture](/en-us/tech-zone/design/media/reference-architectures_gdpr_mobile-apps.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_mobile-apps.png)

Citrix Endpoint Management (CEM) is a market leading Unified Endpoint Management (UEM) component of the Citrix Workspace. It can help you secure and manage various mobile endpoint platforms ranging from iOS, Android, Windows, and Mac to rugged mobile devices and IoT devices. CEM also manages various mobile apps on endpoints and supports various delivery mechanisms including virtualized, web & SaaS, public app store, native enterprise mobile apps, and containerized mobile apps.

In the following sections, we discuss how this architecture can help you secure personal data on mobile endpoints.

The [Citrix MDX Toolkit](/en-us/mdx-toolkit/overview.html), the Citrix Endpoint Management MAM container technology, is a key part of the Citrix Endpoint Management solution to protect data. It provides end-to-end security maximizing protection of personal data, mitigating the risk of loss, by encrypting apps and data and managing the transfer of data through 70+ [MDX Policies](/en-us/mdx-toolkit/policies-platform.html). These include functional areas such as Authentication, Device Security, Networking, Encryption, Access Thresholds, App Interaction, App Restrictions, and other app-specific policies.  All of these are applied on a per-app basis designed to mitigate the risk of personal data loss.

MDX Technologies help provide end-to-end protection by managing encrypted data transfers between device and intranet
data stores, in addition to between managed apps. Once these apps are installed, Secure Hub, a mobile app that provides access to desktops, apps and data, helps continuously enforce the desired policies. IT is always in control of the enterprise content on users’ devices. MDX also includes micro VPN, a per-app VPN that technology that integrates with Citrix Gateway. It is utilized seamlessly by managed apps to encrypt data traffic to and from the enterprise intranet.

### Article 25 - Access to Personal Data

Citrix Endpoint Management provides various enrollment methods to validate user identity before initiating Mobile Device Management or Mobile App Management and then access to secure data. For example, a two-factor authentication solution can include One-time PIN (OTP) enrollment invitations along with Active Directory domain credentials. For environments with the highest security requirements, enrollment invitations may be linked to a device by SN, UDID, EMEI to uniquely identify the hardware.

Citrix Endpoint Management also provides a variety of multifactor authentication options to validate the identity of enrolled user devices. These include combinations of domain user name and password, RADIUS, Azure Active Directory, certificate, or derived credentials (a high security federal standard based on government issued personal identity verification cards). Certificate and domain authentication used with a CEM pin is a popular secure combination that provides a great user experience.

### Article 32 - Data Encryption in Transit

Citrix Endpoint Management supports data encryption in transit through several methods such as:

*  Containerized with embedded VPN when apps utilize the CEM SDK
*  Platform-based utilizing a Citrix ADC VPN client
*  Through policies to utilize native platform OS VPN functionality

The Citrix Endpoint Management SDK, or MDX technology, with micro VPN provides secure per-app VPN functionality to encrypt data in-transit between the mobile endpoint and intranet back-end. It works with Secure Hub and Citrix ADC to ensure MDX app traffic is directed over a dedicated encrypted VPN. It is unique Citrix technology that provides seamless encryption of data in transit.

For more information see this [micro VPN FAQ](https://support.citrix.com/article/CTX136914); configuration of Android platform per-app VPNs using Citrix VPN for Android or iOS; or the configuration of platform per-app VPNs using native functionality.

### Article 32 - Data Encryption at Rest

Citrix Endpoint Management (CEM) supports data encryption at rest through the CEM MDX with Citrix-provided encryption libraries, or through platform level encryption directly or indirectly with partner containerization solutions.

CEM can provide encryption at rest on any supported mobile device independent of platform encryption. The CEM secure app container technology, MDX, uses its own software applied data encryption using FIPS compliant algorithms minimizing the risk of data loss.

Device level encryption varies by platform. Apple's iOS features a file system with the OS information and user data written to flash memory. It also uses a factory-assigned device ID and group ID with the device user's passcode so only that passcode can unencrypt data on the phone or tablet. Android also provides encryption, although not every device manufacturer creates hardware that supports it and users can turn encryption off accidentally or deliberately with a factory reset on Android devices. Find more information about the [MDX Toolkit](/en-us/mdx-toolkit/10.html), [MDX policies](/en-us/mdx-toolkit/policies-platform.html), and [integrating with MDX](/en-us/mdx-toolkit/developer-guide-overview.html) in Citrix documentation.

### Article 32 - Data Isolation and Protection

Containerization enables mobile BYOD programs in corporate environments empowering users to use mobile endpoints as an enterprise device and personal device simultaneously by separating apps and data. It helps enterprises prevent malware, intruders, system resources or other applications from interacting with the application and any of its sensitive information. Citrix Endpoint Management enables containerized native mobile apps through MDX technology, and it also integrates with several partner container solutions providing further value by integrating many broad app and device management capabilities.

## Securing Files with Workflows

Many of our daily workflows consist of the creation of files and collaborating on those files with others. Often those files contain personal data, such as name and address, Social Security numbers or credit card details. Unfortunately, there are many examples of data leakage occurring, from lost USB drives filled with files containing personal information to phishing attempts to file access on secured systems with employee permissions. Under GDPR it is necessary to secure and control this information in every step of the process. This includes storing these files inside a repository to internal and external collaboration and providing context-based access to the files. It also means monitoring for irregular activities and reporting on who has which permissions to which files.

Citrix Content Collaboration (ShareFile) provides a range of controls to help organizations become and remain compliant under GDPR. This starts by having a choice on the location where files are being stored. Options include inside one of the Citrix-managed StorageZones in different global regions or in a StorageZone managed by the customer in their own data centers or private cloud. Multiple locations can be used, allowing for the optimal location to store each individual file. By using the StorageZone Connectors technology to access existing repositories, such as network file shares or SharePoint document libraries, all file related activities are done through a single platform. This makes auditing the activities easier, in addition to making sure that the correct permissions are in place.

**StorageZones Options**
[![StorageZones Options](/en-us/tech-zone/design/media/reference-architectures_gdpr_storagezones.png)](/en-us/tech-zone/design/media/reference-architectures_gdpr_storagezones.png)

For more information on StorageZones including architecture details and deployment options, see the [Content Collaboration Reference Architectures](/en-us/tech-zone/design/reference-architectures.html#citrix-content-collaboration)

Collaboration on files has not changed much over the years. Most of these workflows use email to send files to a group of recipients. Feedback is gathered from each of those recipients as separate emails to the thread and update the files before starting the cycle again. As such, multiple messages and copies of the same document are stored inside the email platform, which makes it more difficult to comply with GDPR policies. By using the ShareFile Feedback and Approvals workflow to collaborate on documents, all feedback and document revisions are stored in a single place, making it easier to comply with such regulations.

Many paper-based workflows in an organization contain personal data in some form. For instance, the workflow to hire people involves multiple steps where personal information needs to be recorded and shared. All this information needs to comply with GDPR regulations, centralizing and digitizing these workflows have a positive impact. ShareFile Custom Workflows is therefore designed to allow this personal data to be securely captured, securely stored inside ShareFile and, where needed, completed with an electronic signature. All information is stored together in a single location and is audited for who accesses and modifies this information, providing a practice that complies with GDPR.

### Article 32 - Data Encryption in Transit

All connections between ShareFile clients and the Content Collaboration SaaS Control Plane, between ShareFile clients and ShareFile StorageZones, in addition to the Content Collaboration SaaS Control Plane and ShareFile StorageZones are fully encrypted. See [CTX208317](https://support.citrix.com/article/CTX208317) and the [Citrix ShareFile Security and Compliance FAQ](https://www.sharefile.com/resources/citrix-sharefile-security-and-compliance-frequently-asked-questions) for further details.

Citrix Files (ShareFile) clients for iOS and Android, which can be managed by Citrix Endpoint Management, also use the embedded VPN capabilities provided by the CEM SDK. See “Data Encryption in Transit” in the Securing Mobile Applications section of this document for more details.

### Article 32 - Data Encryption at Rest

Citrix Content Collaboration offers a flexible architecture which provides customers the choice of where files are stored at rest. These repositories are called StorageZones and are managed by either Citrix or the customer.

For StorageZones managed by Citrix, hosted in either Amazon Web Services or Microsoft Azure, files are stored at rest with 256-bit AES encryption. The encryption key is a shared key for all files stored across every ShareFile tenant. Alternatively, this can be a customer-managed encryption key, configured in the Amazon Key Management Service. When the StorageZone is managed by the customer, per-file encryption can be enabled inside the StorageZone configuration. When enabled, files are encrypted with 256-bit AES encryption.

Files are not only stored at rest inside the repository in the data center or cloud, but also on the devices used by employees. As a best practice, it’s recommended to always use full drive encryption on Windows and macOS devices. On top of that, Content Collaboration allows controls for both corporate-owned and BYOD devices. It allows for a remote wipe of the user files, removing only the corporate files and not touching the personal files of the employee. When a remote wipe is initiated, the Citrix Files client sends back all file activity that has occurred offline between the wipe command and the actual wipe of the user data repository. This occurs when ShareFile logs on to the ShareFile SaaS application tier.

Similar safeguards are in place with the Citrix Files app for iOS and Android. All files at rest are encrypted by using the device keychain and encryption capabilities. When using a Citrix Endpoint Management-managed version of Citrix Files, the encryption key is stored inside Secure Hub. And because ShareFile provides a robust rendering and editing engine for Office files and PDF documents, there are many advantages. With the mobile Citrix Files clients, files don’t need to leave the applications for reviewing or editing. ShareFile offers multiple mobile device management options to secure the files, for instance by blocking access from jailbroken devices and blocking opening files in other applications. When using Citrix Endpoint Management, other advanced policies for more granular control are available.

### Article 25 - Access to Personal Data

Authentication to ShareFile is either controlled by a user name and password (ShareFile credentials) or by using corporate credentials through a SAML Identity Provider. When using ShareFile credentials, the password for the user is subject to the password policy that has been configured. This password policy controls the requirements for the password in terms of complexity, history and how often it must be changed. The password is stored hashed and salted inside the ShareFile SaaS application tier for enhanced security.

SAML based authentication is commonly used for authentication to cloud services. Instead of authenticating directly to the
enterprise directory, such as Active Directory, the authentication is done against an Identity Provider. This removes the need to expose the enterprise directory directly to ShareFile, but still allows users to authenticate with their enterprise credentials. The Identity Provider controls how the user must identify and authenticate itself, based on the context of that authentication attempt. This allows for extra security measures like multifactor authentication for authentication attempts from outside the corporate network and SSO
based on the Windows authentication token for domain-joined devices.

### Article 32 - Data Isolation and Protection

ShareFile integrates with market-leading Data Loss Prevention products for customer-managed StorageZones and Cloud Access Security Broker Services for any type of ShareFile StorageZone, enabling content-aware restrictions. Documents stored inside a ShareFile StorageZone are examined by the same policies that are already set up for other repositories. Based on those scanning results, files can be blocked for download or shared with others.

Sharing files is a key component of modern workflows. This makes controlling the access and permissions to documents containing
privacy related information a priority, especially when the files are outside the direct control of your own security policies. With ShareFile Information Rights Management (IRM) watermarking, documents are protected via watermark with an online view only option that helps address unauthorized access including various image capture techniques. It also facilitates forensic investigations as needed to comply with GDPR.

ShareFile uses versioning to store different versions of the same file. This is not only convenient to review changes made to
documents, but this can help when recovering from a malware or ransomware attack. By restoring the files to the state before the attack, data loss is minimized. Recovery time is reduced by automating the restore to previous versions by using the ShareFile PowerShell cmdlets.

For customers requiring all files to be archived for compliance purposes, ShareFile offers this capability. When a user deletes a file, or when the file is automatically deleted by a retention policy, the file is stored inside an archive instead of being fully deleted from ShareFile. Dedicated auditors can review the contents of the archived files, including access permissions, during an investigation.

## Summary

[Citrix Workspace](https://www.citrix.com/products/citrix-workspace/) simplifies the management of your systems and data by centralizing services in the data center or cloud as a digital workspace. It helps Citrix customers adhere to many GDPR requirements by helping to ensure that applications are centralized and enclaved, data is protected when shared or distributed, access to data and resources is controlled, and IT is brought together for application and data-specific security.

To learn more about security and compliance with Citrix secure digital workspace solutions, visit [https://www.citrix.com/it-security/](https://www.citrix.com/it-security/).

To learn more about Citrix’s approach to data management, including security documentation, privacy and security compliance, and vulnerability management, visit [https://www.citrix.com/about/trust-center/](https://www.citrix.com/about/trust-center/).

## Additional Links

[Citrix Trust Center and Privacy Policy](https://www.citrix.com/about/trust-center/privacy-compliance.html)

[Top 10 findings from Citrix environment security assessments](https://www.citrix.com/blogs/2019/04/29/citrix-tips-top-10-findings-from-citrix-environment-security-assessments/)

[Citrix Ready Security Solutions](https://citrixready.citrix.com/category-results.html?search=security)

[Citrix Global Partners](https://www.citrix.com/global-partners/)
