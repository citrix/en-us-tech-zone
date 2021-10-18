---
layout: doc
h3InToc: true
contributedBy: Andy Mills, Patrick Coble, Martin Zugec
specialThanksTo: Eric Beiers, Steven Wright, Jessie LaCome
description: Tech paper focused on security recommendations and security practices for administrators.  Use this guide to navigate security planning, implementation, and ongoing operation.
tz_title: Security best practices for Citrix Virtual Apps and Desktops
tz_products: citrix-virtual-apps-and-desktops
---
# Tech Paper: Security best practices for Citrix Virtual Apps and Desktops

*Disclaimer: This information is provided on an "AS IS" basis without warranty of any kind. It is for information purposes only, and is subject to change at any time at Citrix's sole discretion.*

## Introduction

Global organizations—including healthcare, government, and financial services—rely on Citrix Virtual Apps and Desktops (CVAD) to provide secure remote access to environments and applications. When properly configured, CVAD can provide security measures that extend well beyond what is natively available in enterprise operating systems. Citrix provides extra controls that you enable by using virtualization.

This tech paper shares recommendations and resources to help you establish a security baseline for your virtualized environment. We highlight some of the most important security improvements you can make. Always try out changes like these in a test or development environment before you modify your production environment. Testing can help you avoid unexpected problems or results.

This tech paper uses the traditional layered methodology developed by Citrix professional services:

[![Layered Approach](/en-us/tech-zone/build/media/tech-papers_cvad-security-best-practices_001.png)](/en-us/tech-zone/build/media/tech-papers_cvad-security-best-practices_001.png)

In the Citrix layered model, security does not have its own layer. Any security process or security functionality is intertwined between the layers. It's crucial that security is covered throughout an infrastructure, including the processes surrounding it.

Does organization need to meet specific security standards to satisfy regulatory requirements? This document does not cover that subject, because such security standards change over time. For up-to-date information on security standards and Citrix products, visit [Security and Compliance Information](https://www.citrix.com/about/legal/security-compliance/) and [Citrix Trust Center](https://www.citrix.com/about/trust-center/).

## User and Device Layer

How does a business enable their end-users to connect to virtual desktops? Options such as Bring Your Own Device (BYOD) or corporate-issued devices are available. Both come with their own operational overhead. The endpoint delivery model can have many design implications including decisions around what features can be offloaded to the endpoint. A user connecting from a BYOD device might have no access to the clipboard, client drive mapping (CDM), or printing. A user connecting from a 'trusted' corporate-owned device can have access to clipboard and local drives. There is no 'one size fits all' for endpoint devices. Endpoint clients must be selected based on the use case, mobility, performance, cost, and security requirements

### Device Lockdown

The endpoint device has the potential to be used as an attack vector, including such attacks as keyloggers or points of ingress into the network. The benefit when using Citrix is the reduction of ingress point to the mouse, keyboard, and screen refreshes. Users can require more functionality such as accessing local client drives, printing facilities, and clipboard functionality. That functionality is configurable using Citrix policies. Citrix has the controls to reduce the risk of attack vectors such as anti-keylogging and disabling any points of ingress and egress.

These questions are all for the IT support teams to consider when deploying devices to the end users.

-  Do users require local administrative permissions on the device?
-  What other software is running on the endpoint? Can other software be installed?
-  Does the user have a VPN capability?
-  Who updates the device in terms of operating system and application patching?
-  What resources can the endpoint access?
-  Who has access to the endpoint itself?

### Endpoint Logging

Even if a user cannot install software on the endpoint, make sure that logs are enabled and centrally captured. The logs can help you to understand what occurred in a breach. They can also be given to forensic analysts for further review. Logging is a crucial element to any environment, and its security monitoring processes. It is also a good practice to forward all logs to a Security Information and Event Management (SIEM) system. This detail allows you to set alerts for known attack methods or other key events.

As with any kind of data collection, it is important to not only collect data, but be able to analyze it. It is easy to get overwhelmed by the amount of reported data. Make sure you can detect and respond to a potential alert.

### Thin Client terminals

In many scenarios, thin client devices are perfect for high-risk environments. Thin clients run a specialized version of an operating system, only having enough of an operating system and application to connect to a Citrix session. In this scenario, there are benefits around patching. With thin clients there are limited applications to be patched and maintained. Usually the operating systems have reduced footprint. Due to the devices having a simplified and purpose-built operating system, there is limited data at rest on the endpoint.

For thin client terminals that run a full operating system, the use of write filters to stop the persistence of data is useful. Resetting the terminals when a reboot occurs reduces the chance of an attacker persisting data on the endpoint that can be used to formulate an attack. Make sure you are not destroying logs and other critical data, as it is needed for later forensic analysis. Thin Client terminals have the added benefit of usually being cheaper to purchase and maintain than traditional desktops or laptops.

### Patching

Patch management must be one of the foremost considerations to factor into choosing an endpoint. How does the business get security fixes applied to the endpoints? In most traditional Windows environments, machines have patches pushed to them through Windows Services Update Service (WSUS), System Center Configuration Manager (SCCM), or through other automated services. These services are great if corporate IT manages the endpoint.

What about BYOD? Who patches those devices to ensure they are up to date? With work from home growing rapidly, endpoint patching becomes more complicated. Policies must be defined with clear responsibilities for the user. The policy is needed to ensure that users know how to patch and update their devices. Simple notifications on the login page suggesting a new update has been released and they need to be patched as soon as possible is one method. You can also use [Endpoint Analysis Scans (EPA)](/en-us/citrix-gateway/current-release/vpn-user-config/advanced-epa-policies.html) on the endpoint to deny access to the infrastructure unless they are running up to date software.

It is also important to ensure that the Citrix Workspace app is patched and up to date on the endpoint. Citrix provides not only feature enhancements, but also security fixes in new releases.

### App Protection Policies

The end user is widely considered the weakest piece on the attack surface of an organization. It has become common practice for attackers to use sophisticated methods to fool users into installing malware on their endpoints. Once installed, the malware can silently collect and exfiltrate sensitive data.  User’s credentials, sensitive information, company’s intellectual property, or confidential data are targeted. Endpoints become an even more exposed threat surface with the increase in BYO devices. And when accessing corporate resources from unmanaged endpoints. With many users working from home, the risk to the organizations is heightened due to the untrustworthiness of the endpoint device.

With the use of virtual apps and desktops, an attack surface of endpoints has been greatly reduced. Data is stored centrally in a data center and it is much harder for the attacker to steal it. The virtual session is not running on the endpoint and users generally do not have permission to install apps within the virtual session. The data within the session is secure in the data center or cloud resource location. However, a compromised endpoint can capture session keystrokes and information displayed on the endpoint. Citrix provides administrators the ability prevent these attack vectors, using an add-on feature called App protection. The feature enables CVAD administrators to enforce policies specifically on one or more delivery groups. When users connect to sessions from these delivery groups, the user’s endpoint has either anti screen capture or anti-keylogging or both enforced on the endpoints.

You can find more information in [App protection policies tech brief](/en-us/tech-zone/learn/tech-briefs/app-protection-policies.html).

### Account Management

Through the user and device layer, there has been a major focus on device security. Things like User account creation and resource assignment authorization processes need to be centralized and managed efficiently. Many times, accounts are simply 'copied' from users in similar roles. It can speed up onboarding, but it creates creep in terms of permissions and unauthorized group memberships within the central directory. Copying a user's permissions from a previous account can lead to unauthorized access to data as well. Ideally, the data owner must authorize any group request before being permitted access.

Decommissioning accounts can be simple if integrated properly with HR systems. Sometimes businesses need to flex resources in and out from third parties. Vendor accounts and extra support for busy periods and when outsourcing business. How do those accounts get decommissioned and ultimately deleted? In the best-case scenario, the business has clearly defined procedures for both onboarding and offboarding any contracting resource. Complete offboarding by with end dates set to a minimum, disable the account and moved into a 'holding OU' within the Active Directory. Then it must be deleted when the resource is confirmed as no longer being required. Ultimately, we do not want contractors to log in months after they have left the business.

## Access Layer

Within the Citrix design process, the Access layer is where users authenticate. Here the necessary policies are applied, and dynamic contextual based access is evaluated. The access tier is designed with a strong level of security in mind and is critical.

In the following section, we aim to cover the main tasks to help better secure your Citrix ADC deployment. There are two common deployment types for Citrix ADCs. One way is using the ADC as a proxy for CVAD deployments. The other way is for load balancing applications to make them more highly available and secure. Following the details contained in this document help lower your risk and exposure for items that the Citrix ADC is interacting with.

### Strong authentication

Strong authentication is recommended for all external connections to any internal system. Unfortunately, there are a great number of user names and passwords out there that are searchable by attackers. They are from past breaches and leaks, with over 12.3 billion records. This number increases the risks of a breach into your company, as more become available.  The reuse of a password is common with almost everyone in the world, and your business can be at risk due to your user's bad password habits. A user name and password must not be your only defense to your applications.  You must not inherently trust people with just two pieces of information that can be easily stolen. Adding multifactor authentication to all applications isn't alway feasible with the user's workflow, but we recommend deploying wherever possible, especially for external access.

You can find more information in [product documentation](/en-us/citrix-adc/current-release/aaa-tm/authentication-methods/multi-factor-nfactor-authentication.html).

### HDX encryption

It has been a long since standard for enabling RC5 – 128 bit as the security encryption for the HDX/ICA stream. It provides a level of security on the ICA stream. We recommend enabling VDA SSL on the ICA stream between either the endpoint and the VDA. Enable SSL between the ADC and the VDA an extra level of encryption. The configuration binds a certificate to the Desktop service. It encrypts the ICA stream to the given standard of the certificate bound. For more information on how to carry out this process, read [CTX220062](https://support.citrix.com/article/CTX220062).

### SSL/TLS Ciphers

Validate Transport Layer Security (TLS) Ciphers and SSL Score for all External VIPs (Citrix Gateway). There have been many vulnerabilities in the SSL and TLS protocols over the past couple of years. Ensure you are using all the TLS best practices as they are ever-changing. Ensure older protocols like SSLv3 and TLS 1.0 and TLS 1.1 are disabled.

Choosing between a Wildcard or SAN certificate is another aspect of this process. Wildcards can be cost-effective, but they also can be more complicated. Certificate expirations highly impact deployments with a high number of hosts. However, a SAN certificate can effectively be a restricted wildcard for just 2–5 sites. All with the ability to use more than one TLD in the same certificate.

Read [Citrix Networking SSL/TLS Best Practices](/en-us/tech-zone/build/tech-papers/networking-tls-best-practices.html) for latest recommendations.

### Encrypting XML

Ensure that your XML traffic from the Delivery Controller or Cloud Connector to StoreFront servers is always encrypted. The purpose is to prevent someone with simple network access from seeing what is being requested by users. Certificates are required for each server along with changing the configuration of each server to use the new encrypted port (by default port 443). You can find more information in [product documentation](/en-us/citrix-virtual-apps-desktops/secure/tls.html#install-tls-server-certificates-on-controllers).

**High-Security Recommendation** - For a high-security deployment it is recommended to use non-standard ports for obfuscation. Also ensure that there are firewalls configured between the server roles for the XML traffic to control traffic bidirectionally.

### StoreFront SSL

We highly recommend using an encrypted Base URL for any StoreFront deployment. Encryption ensures that neither credentials nor session launch data traverse a network without transport protection. In most deployments, the certificate can be issued from an internal certificate authority. These servers are behind a Citrix ADC deployment as part of the **ICA Profile** settings for a Citrix Gateway for external devices. Or they are behind a Citrix ADC VIP that is typically accessed by internal devices only. Ensure the use of strong ciphers and TLS 1.1 or 1.2 for server communication. Details for this process are outlined in [Microsoft documentation](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn786418(v=ws.11)).

**High-Security Recommendation** - In high-security deployments we recommend enabling ICA file signing to ensure the files received by the Citrix Workspace app are trusted. This process is outlined in [StoreFront product documentation](/en-us/storefront/1912-ltsr/configure-using-configuration-files/strfront.html#enable-ica-file-signing).

### VDA Encryption

VDA Encryption is typically one of the last items configured to increase the session transport security. The configuration is completed on VDA and on the Delivery Group objects.

**High-Security Recommendation** - In high-security deployments we recommend enabling VDA encryption to further protect the transport of the Citrix session information. Using a PowerShell script is the simplest method, and the process is outlined in [product documentation](/en-us/xenapp-and-xendesktop/7-15-ltsr/secure/tls.html#tls-settings-on-vdas).

### Control Physical Access

Citrix ADC appliances must be deployed in a secure location with sufficient physical access control to protect them from unauthorized access. This requirement applies to physical models and their COM ports. It also applies to virtual models on virtualization hosts and their associated ILO\DRAC connections. Find more information in [Citrix ADC product documentation - physical security best practices](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#physical-security-best-practices).

### Change Default Passwords

Having any default password that is known by the IT community is a huge risk. There are network scanners that can search for default credentials being used on a given network out there. The effectiveness of these scanners can easily be mitigated or even eliminated by simply changing the default password. All service account passwords must be stored in a secure location like a password manager. The secure storage of all service account passwords is a foundational information security standard. The password for each role and deployment (HA Pair) must be unique and credentials must never be reused. Find more information in [Citrix ADC product documentation - Change the default passwords](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#other-network-security-considerations).

-  **nsroot** - Change the default when you start the initial configuration of the device. This account allows full administrative access and protecting this password must be the number one priority when deploying this system. If you are deploying a new Citrix ADC system in 2020 it has the Serial Number of the appliance as the default password.
    -  Create a second superuser that can be used in emergency scenarios when external authentication is down. Use this account instead of using the default `nsroot` account. Find more information in [Citrix ADC product documentation - Create an alternative superuser account](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#administration-and-management)
    -  SDX users must also create an administrative profile to configure the default admin password for each VPX instance provisioned. We recommend creating a different Admin Profile for each instance. Each HA pair has the same password but is unique to the other instances. Find more information in [knowledge base article CTX215678](https://support.citrix.com/article/CTX215678)
-  **Lights on Management (LOM)** – This account is common in many physical Citrix ADC platforms. It  allows access to the command line and power management of the device. It also has default passwords that must be changed upon initial configuration to help prevent unauthorized remote access.
-  **Key-Encryption-Key (KEK) Password** – This configuration encrypts sensitive password areas locally on each appliance. Find more information [in KEK chapter](#data-protection-with-kek).
-  **SVM Admin Account** – This account and password change only applies to Citrix ADC SDX platforms. It has the default `nsroot` password and does not use the serial number like physical appliances. The password must be changed upon its initial configuration to help prevent unauthorized remote access.

### Secure NSIP traffic from all network access

Every device in your network must be denied the ability to manage your Citrix ADC appliances. Your management IP addresses must be in a VLAN where you control ingress and egress traffic. This configuration ensures that only Authorized Privileged Workstations or other authorized networks can access them for management. Many of the vulnerabilities that have been found over the past 10+ years have been related to having access to the NSIPs. By securing these IPs in a controlled manner you can drastically lower this attack vector or eliminate it altogether. Anytime you limit access of the management of any IT system, you must be document it thoroughly.  It's best to have business continuity plans in place so that you clearly understand the impact.

-  Ensure that Subnet IPs (SNIPs) and Mapped IPs (MIPs) don't have management enabled on them. Having management enabled will result in your DMZ network having access to the management console via the SNIP or MIP.
-  To check this browse to your SNIP, or MIP via HTTP and HTTPS. Within the console navigate to System – Network – IPs and select the IP and you see if the management check box is selected.
-  You can also use Access Control Lists to limit access to the management of the ADC appliance. Limiting to specific hosts or subnets can be done without the management interface being placed behind a firewall. You can find more information in [CTX228148 - How to Lock Down Citrix ADC Management Interfaces with ACLs](https://support.citrix.com/article/CTX228148) and [Citrix ADC product documentation - Network security](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#network-security).
-  If you place your Citrix ADCs behind a firewall, you must plan according to the services the appliance is providing. Also plan access to the management interface of each device. Also plan for any supporting maintenance or security devices like Citrix Application Delivery Management (ADM) and Citrix Analytics. You can find more information in [Communication Ports Used by Citrix Technologies](/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html)) and [CTX113250 - Sample DMZ Configuration](https://support.citrix.com/article/CTX113250).
-  In higher security deployments, you can also change the management ports from the default HTTP (TCP 80) and HTTPS (TCP 443). Change them to a custom port to create obfuscation and help prevent identification from typical scans. Each appliance must be changed individually. You can find more information in [Citrix ADC product documentation](/en-us/citrix-adc/current-release/system/basic-operations.html#how-to-configure-http-and-https-management-ports-for-internal-services).
-  Limit all Egress (outbound) and Ingress (inbound) traffic from the management networks that the Citrix ADC is deployed on. Not having any restrictions along with monitoring to these management networks increases the risk to that device. With critical devices like a Citrix ADC, management must be restricted to specific privileged workstations to reduce the risk of potential attacks. Control external access to stop or slow any attacker from accessing other devices. Preventa attackers from accessing command and control software hosted outside of your network.
-  Keep all the NISIPs, LOMs, and SVMs and Citrix ADC management IPs separated from other networking equipment. Depending on your policy standards, put the LOMs on physical and out of band management networks with other similar systems. Place the NSIPs and SVMs on another separate network. Using a VLAN just for the Citrix ADC management IPs can simplify your firewall and ACL rules. Limit access internally to a privileged workstation and restrict external access.

### Bind Citrix ADC to LDAPS

We do not recommend using generic login accounts for day-to-day administration. When all configuration changes are done by `nsroot` in an IT team, you will not be able to track who logged in and made a particular change. Once the `nsroot` accounts password has been changed, the system should then immediately be bound to LDAPS to track usage to a specific user account. This step provides delegated control so you can create **View Only** groups for auditing and application teams. You can then allow full admin rights to specific AD groups. We do not recommend binding using unencrypted LDAP, as credentials can be collected with packet captures. You can find more information in [CTX212422](https://support.citrix.com/article/CTX212422)

If you are binding to Microsoft Active Directory, ensure you are only using LDAPS as the protocol. Also, ensure NTLMv2 is the only hashing method for credentials, and network sessions. This step is an AD security best practice. It ensures that credentials are as protected as possible while in transit for authentication requests and network sessions. Properly test this configuration and validate with older Windows clients - to identify compatibility issues with older versions of Windows.

When binding to any external authentication source, you must disable local authentication for accounts like `nsroot`. Optionally, only enable specific local accounts to use local authentication. Depending on your deployment requirements can require one path or another. Most deployments don't require local accounts. If there is a service account needed, you can create and delegate an LDAP user. [This guide](https://www.citrix.com/blogs/2020/09/23/how-to-secure-restrict-citrix-adc-management-access/) covers setting up both methods, one method must be deployed.

### Logging and Alerting

The practice of Syslog Forwarding is critical for providing incident response along with more advanced troubleshooting. These logs allow you to see login attempts to the ADC. The Citrix Gateway can take actions which are based on policies from packet and system operations such as:

-  GeoIP & BadIP blocking
-  AppQoE Protection
-  Responder Policy Actions

This information can be invaluable for troubleshooting issues. This detail helps you understand a potential current attack situation. It will help determine how to best react based on actionable data to resolve issues, stop, or mitigate an attack. Without a Syslog target configured on your Citrix ADC, the required logs are deleted to save space for the system to stay operational. You can find more information in [Citrix ADC product documentation - Logging and monitoring](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#logging-and-monitoring).

There are many free and paid Syslog servers and collectors available. Some are priced according to the number of messages per day, storage space of messages, or they are just a continuous subscription. These solutions must be planned for, as they require a sizable amount of storage space to be allocated. Your Syslog server or collector must be sized based on the number of events from other critical services. These services include Active Directory, database, and file servers, VDAs, and other application servers.

Citrix offers the Citrix ADM service as a Syslog collector. Citrix ADM can allow you to have some off-device retention. Citrix ADM is also a great tool to be able to search and view these logs and use its events dashboards. There are many great default views of log data to help you troubleshoot, view hardware, authentication issues, configuration changes, and much more. You can find more information in Citrix ADM product documentation - [Configuring syslog on instances](/en-us/citrix-application-delivery-management-service/setting-up/configuring-syslog-on-instances.html) and [View and Export syslog messages](/en-us/citrix-application-delivery-management-service/networks/events/how-to-export-syslog-messages.html).

We highly recommend using this list of messages to create your alerts based on what features of the Citrix ADC you have in use. Just having the logs retained and searchable is valuable to troubleshooting and auditing security events. It is important to ensure that you are alerting based on the threshold for Bad Logins along with Any Logins using the default `nsroot` accounts. You can find more information in [Developer Docs - Syslog Message Reference](https://developer-docs.citrix.com/projects/citrix-adc-syslog-message-reference/en/latest/).

Configure SNMP in your monitoring solution to ensure you have both service and physical level monitoring enabled. This setting ensures service, and appliance operation. Configuring email allows system notifications to be sent out to the appropriate mailbox. This step also allows you to be notified of expiring certificates and other notifications.

### Plan Services to Monitor and Protect

Taking the time to plan which services to use on your Citrix ADC helps you understand which features can be applied to those IP addresses. It is a great opportunity to validate if you have IPs assigned to test. Always validate all security settings on a Test VIP before applying the responder and other policies to your Production VIP. This step can require more DNS records, SSL Certificates, IP addresses, and other configurations to make it work.

The three most common use cases for the Citrix ADC are Load Balancing, Global Server Load Balancing, and Citrix Gateway.

We recommend organizing all the planned VIPs by either Internal or External. Internal VIPs might not require all the security features that are necessary for external VIPs. Evaluating which countries need to access external resources is a good planning step if you plan to use the GeoIP features. Meet with your business leadership and application owners to understand if you need to restrict certain countries.

If possible, deploy Citrix ADM before you start making configuration changes. Citrix ADM can easily back up the appliances regularly and provide better log visibility along with metrics. Other more advanced features can be added to a Citrix ADC deployment to provide even more visibility and security. Citrix ADM has paid editions along with Citrix Analytics that can go beyond just a monitoring solution. We can become an automatic security response system.

### Replace Default SSL Certificates

Modern browsers warn you that a certificate is invalid and will be a security risk if is the default certificates. If you acknowledge this error every day, you are susceptible to a true man-in-the-middle attack. Someone can read what you type as you type because they can get into the TLS stream. Replacing the certificate can be easy depending on if you host your own internal Certificate Authority, use a third-party service (possible cost associated), or self-signed certificates. Once you have updated the certificates your browser must trust the new certificate and must not error while accessing the device. If you ever experience a certificate error again, you must check the expiration date. It's possible it was replaced without your knowledge.

-  **NSIP** – Changing this certificate must be done first, as it is where all management for the appliance happens. This process must be completed on each appliance. You can find more information in [CTX122521](https://support.citrix.com/article/CTX122521).
-  **LOM** – Changing these certificates must be the next goal once the NSIP management Certificates have been changed from the default. This process must be completed on each appliance. You can find more information in [product documentation](/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/installing-a-certificate-key-on-lom-gui.html).
-  **SDX SVM** – If you are an SDX customer, ensure that the default certificate has been replaced. The SDX SVM becomes your primary method of appliance management along with managing the instances themselves. This process also must be completed on each appliance. You can find more information in [CTX200284](https://support.citrix.com/article/CTX200284).

### Firmware Update

1.  **Initial Deployment** - Use the latest available firmware version for each Citrix ADC platform. Be sure to review the release notes to see if there are any "known issues" that may impact you. N-1 methodology is often used to remain one version behind from the latest release. Firmware updates are done on each instance regardless of which platform you are using whether it's a CPX, MPX, SDX, or VPX.
1.  **LOM\IPMI Firmware Updates** - We recommend that the firmware is updated at the time of deployment. Updates ensure you have the latest features and fixes. You can find more details here [product documentation](/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/upgrading-the-lom-firmware.html).
1.  **Ongoing Updates** - [Sign up for alerts](https://support.citrix.com/user/alerts) for Citrix Security Bulletins and Update notifications for the Citrix ADC, Citrix ADM and Citrix Web Application Firewall.

Plan out how often you choose to update your Citrix ADC’s each year. Features deployed and IT organization policies must determine how you schedule these updates. We typically see updates being released 2–4 times per year. Also with two feature updates and two updates that address known issues and security fixes. There are guides available to help you plan and schedule these upgrades without service interruption. Also be applied to other supporting systems like Citrix ADM, since version parity is always recommended. Learn more about [upgrading a high availability pair](/en-us/citrix-adc/current-release/upgrade-downgrade-citrix-adc-appliance/upgrade-downgrade-ha-pair.html).

### Data Protection with KEK

By creating a KEK key pair, you can gain further data protection. We highly recommend doing this step on each appliance. The key pair encrypts the configuration in key password related areas. If someone gains access to your `ns.conf` file, they are not able to harvest any credentials or passwords from it as a result. The two key files that are at the root of the `\flash\nsconfig\` folder must be considered highly sensitive and must be protected accordingly with backups with appropriate security.

This caveat here means that migrations from one appliance to another requires more steps. The **KEK** key must be added to the new deployment before your configuration can be decrypted. [Learn more](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html) about creating a master key for data protection (search for string `KEK`).

### Drop Invalid Packets

Many invalid packets are sent every day to your Citrix ADC device. Some are benign, but most are used for fingerprinting purposes along with protocol-based attacks. Enabling this feature saves CPU and memory resources on your device. Prevent most known protocol attacks by not sending a partial or bad packet to the back-end application that the ADC proxies. This step can even block potential future attacks that rely on packet manipulation.

You can find more information in [product documentation](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#applications-and-services), [CTX227979](https://support.citrix.com/article/CTX227979) and [CTX121149](https://support.citrix.com/article/CTX121149).

### HTTP Strict Transport Security

Ensure HSTS is configured on all SSL VIPs. The primary goal of HTTP Strict Transport Security is to protect applications from various attack methods such as Downgrade attacks, Cookie Hijacking, and SSL Stripping. This is similar to **Drop Invalid Packets** but is based both HTTP and HTTPS. This is based on standards found in [RFC 6797](https://tools.ietf.org/html/rfc6797) by using an entry into the HTTP header. This adds yet another layer of defense to any applications behind the Citrix ADC.

## Resource Layer

The resources that host the user session can present a higher level of risk to compromise. A user running a VDI session is similar to a computer that is connected to the corporate network. Utilizing well-designed access and control layers allows the business to move towards zero-trust models. With zero-trust, access is dynamically adjusted to the resources presented to the end-user dependent upon a given set of variables. The following guidance provides higher levels of control in protecting the corporate assets from users.

### Build Hardening

Hardening operating system builds can be complex and difficult to achieve. It comes with the trade-offs between user experience, usability, and security all being a fine balance. Many customers choose to follow the Center for Internet Security (CIS) baselines for hardening virtual machines in varying roles. Microsoft also provides hardening guides for similar workloads. There are even the ADMX files to implement directly into group policy. If you choose this route, proceed with caution. Be sure to test thoroughly first, if initial policies are overly restrictive. These baselines are a great starting point for hardening. However, they are not meant to be exhaustive for all scenarios. The key to locking down is to test thoroughly and encourage third party penetration testing engagements. Testing validates your security controls and their effectiveness against the latest attack methods.

As part of hardening the system, we recommended that administrators spend some time optimizing the underlying operating systems, services, and scheduled tasks. This step removes any unnecessary processes from the underlying systems. It also improves the responsiveness of the session host and provides an improved user experience to the end-user. Citrix provides the optimizer tool [Citrix Optimizer](https://support.citrix.com/article/CTX224676) that optimizes many elements of the operating system automatically for administrators. The Citrix Optimizer results to adjust to ensure that there is no negative impact within your environment.

#### Scheduled Tasks

In some instances we need to utilise scheduled tasks to perform either routine maintenance or to resolve an ongoing issue on the environment. If in the event we need to utilise scehduled tasks, there are a few recommendations that need to be considered beforehand.

**Privileged Accounts** - Where possible ensure that any scheduled tasks are configured with accounts that have the correct permissions to what they need to do. Running scheduled tasks with domain administrator credentials for example is not recommended practice.

**Alternatives** - It can be that a scheduled task is being used as a 'plaster' over a wider issue. Is there a better way than using a scheduled task to fix the problem. It's possbily resolving a temporary issue, but fix the route cause rather than leaving a scheduled task running. It is especially true with image updates, as there is a chance that if it has not been documented or fed into the build procedures. It will be missed from future updates and potentially cause more issues.

### Patching and Updates

Ensuring IT systems are patched and up to date is standard practice for all software, operating systems, and hypervisors. Providers have an obligation to ensure that their software is patched and do not allow any security vulnerabilities. Some provide more stability and feature enhancements. Nearly all vendors have a release schedule or cycle for applying patches and updates to their software. It is recommended that customers read and understand what is being patched, or new features being introduced. These updates are often processed through route-to-live solutions. Proper testing can help to avoid outages caused by changes to the production environment.

### Anti-malware

Anti-malware or antivirus is always recommended to be deployed on all servers throughout the infrastructure. Antivirus does provide a good first line of defense against known malware, and many other types of viruses. One of the more complex aspects of antivirus is ensuring that the virus definitions are updated regularly. Particular attention is needed within non-persistent VDI or hosted shared workloads. There are many articles that detail ways of redirecting antivirus definitions to persistent drives. The goal is to ensure that the machines are identified as individual objects in the antivirus management suite. Follow these guidelines to ensure that an antivirus solution is deployed and installed correctly. Antivirus vendors will have their own recommended practices for deploying their antimalware software. We recommended following their guidelines for correct integration. You can read more in [Endpoint Security and Antivirus Best Practices](/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html).

### Application Control

Technologies such as AppLocker can be difficult to implement, but they are powerful tools. Especially in terms of protecting servers with published applications with predictable usage patterns. Being able to granularly lock down the environment to the Executable level. Clearly defining what can or cannot run, and by whom is highly beneficial. Not to mention the logging capabilities during applications launch. In a large enterprise environment with more than 500 applications, all of these things need to be carefully considered.

### Windows Policies

When applying Windows policies to a session host, whether it be a VDI based workload or server-based, it can play two major roles:

-  Hardening the operating system
-  Optimizing the user experience

To simplify the management of operating systems, these policies must be applied through Microsoft Group Policy. This eases the image creation process. If Group Policy is not used, then an alternative method of delivering the lockdowns must be found. Hardening and optimizations that can't be delivered via policy must be automated as part of the image build process.

Always make sure that the operating system is hardened before users can gain access to it. Users must only be able to carry out the minimal number of tasks that are required to perform their role. Any administrative based applications must be secured, and access disabled for general users. This step reduces the risk of a user's ability to 'break out' out their session. Prevent users from potentially either gaining access to unauthorized data or perform malicious acts within the operating system.

### Citrix Policies

Policies can be applied depending on the user access scenario. Examples of Citrix session evaluations include turning off clipboard access or client drive mapping if this is deemed necessary. Even enabling one-way clipboard to allow data to be copied into a session, but not out of a session. Several policies can assist with controlling unauthorized egress of data from the system session. Each policy needs to be carefully planned and understood.

Many of the hardening configurations that were discussed in the **System Hardening** section of this article can be applied in the form of group policies. Allowing for a consistent application across all servers that are applied at boot up before the user logs on. This step allows for a consistent user experience across the infrastructure. Which means less troubleshooting, along with ensuring security controls are consistent across all assets and users.

 Configure the delivery groups to allow the VDA to settle before allowing users to log in. This step provides enough time for policies and lockdowns to be applied to the session. It ensures that all the policies have applied to the server before the user logs in. Read more about [SettlementPeriodBeforeUse in Developers Docs](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/latest/Broker/Set-BrokerDesktopGroup/).

### Image Management

Proper implementation of an Image Management system has proven to be a significant time-saver for customers. We support both Machine Creation Services (MCS) and Provisioning Services (PVS). Image management provides tremendous benefits to operational and security functionality.

First, a lockdown or control is applied in a build consistently across all the machines that users access their resources from. Manually building individual machines is not only time-consuming but it's all but guaranteed that some configuration or setting is missed or inconsistent. Setting the configuration once and rolling it out to every machine provides peace of mind that a security implementation is consistent across all the machines.

Second, during a user session on a VDA, the user will open other applications, documents, and access sensitive data. The data is ultimately cached on the virtual desktop or within the application. Once the user is then logged off remnants of data is undoubtedly left behind. Rebooting the machine and reverting to the original golden master image provides a level of reassurance that any sensitive data is wiped clean. Everything is ready for the next user to log in and commence work without the risk of accessing the previous user's data. You can find more information here [Image Management reference architecture](/en-us/tech-zone/design/reference-architectures/image-management.html).

Here are some considerations for single image management:

-  Be mindful of antivirus configurations.
-  Don’t create accounts on a template or image before Machine Creation Services or Provisioning Services has an opportunity to copy the entire image.
-  Do not schedule tasks using stored privileged domain accounts.
-  Service accounts must have a dedicated account with the relevant permissions applied.
-  Make sure to remove all log files, configuration files and other information sources that attacker can use to learn about your environment.

Maintaining these security practices help prevent a machine attack from obtaining local persistent account passwords. These password are then used to log on to MCS/PVS shared images belonging to others.

### Workload separation

Placing resources into separate silos has long been a recommended practice. Not only at the resource layer but also in both the hardware and networking layers, which we cover later in this article. Separating workloads into various delivery groups or machine catalogs will not help fine-tune security policies to a set of machines. It will also reduce the impact of a security breach. If you have a set of high-risk users accessing high impact data, then these must be separated off into an appropriately configured segregated environment. The environment must have much stricter policies and logging applied. Capabilities such as session recording provides an extra layer of protection when using are accessing this data.

Workloads can be separated on different levels - hardware separation (dedicated hosts), VM separation, or even inside OS separation (for example app masking or strict NTFS rules). As part of separating users into various tiers of risk profiles, the data they access must also be treated similarly. This step involves applying the correct file permissions and access rules to data.

#### Micro Segmentation

A concept that has now become more prominent as the move to Cloud hosted infrastructure and zero trust architectures are deployed. If an attacker was succesful in gaining access to resources, we want to limit the damage they could potentially cause. Whether this be malicous damage or data leakage.

Therefore, workload and data seperation is a good practice to follow. This step involves some input from Business Analysts (BA) to help highlight key assets and data across the network. Ideally then, each portion of data can be categorized as high, medium or low impact to the business should the data be accessed.

Depending on the impact to the business, each data segment can be treat in a different manner and seperated from one another. For example, a user browsing the internet can be high risk. It is especially true if they have the ability to download software or documents and save them in their session. Therefore, we would want to consider isolating more critical resources from higher-risk user activities. Web browsing and email access is separate from data such as Personally Identifiable Information (PII) or even Personal Health Information (PHI).

 Seperate workloads between firewalls and control what applications and protocols can traverse those network segments. Workloads in 'high risk' areas may be locked down much more than 'standard' user access machines. The key here is implementing the necessary security controls based on user and data segmentation.

### Session Recording

Session Recording provides IT teams with the ability to record and replay video of what transpired during a given users session. The video is used in the case where a user was carrying out something malicious within the environment. This ability may not be needed for all users. It can be enabled for key individuals, user groups, or when accessing sensitive applications, desktops, or resources. Many takeaways can be gleaned from these recordings that might not be possible with just Windows event and application logs. The videos can be helpful in an incident response scenario or root cause analysis. This feature is powerful and must be carefully considered based on your user policies and agreements with your legal and IT team's approval.

### Watermarking

For sessions that have a user accessing sensitive data, a great deterrent to having the data be stolen is a watermark. Especially if the watermark can uniquely identify the user. Citrix enables admins to configure what to display. You can display:

-  User logon name
-  Client IP address
-  VDA IP address
-  VDA host name
-  Login timestamp
-  Customized text.

Being a server-side feature, it is applicable to all sessions (not just on specific endpoints). It is immune to process termination at the end point by the user as a workaround.

Learn more about session watermarking [in product documentation](/en-us/citrix-virtual-apps-desktops/policies/reference/ica-policy-settings/session-watermark-policy-setting.html).

### Concurrent Usage

A more contentious topic has been the use of multi-session hosts vs single user VDI sessions. Having multiple users log on to a single server can cause issues. It is especially true if a disgruntled user can run software or code to harvest other credentials or access other user data. Running a solid virtual desktop infrastructure does come at an extra cost and these trade-offs need to be considered. Threat model users and select the most efficient delivery mechanism. Decide whether a multiuser session host or single user session host is the ideal path. One size does not fit all users. Carry out user profiling to understand their requirements and then select the best delivery method of workloads for the user groups.

### Virtualization-based Security

Virtualization-based security (VBS) uses a secure portion of memory to store secure assets from the session. This feature requires a Trusted Platform Module (TPM) or Virtual Trusted Platform Module (vTPM) running on a supported platform to provide secure integration. It means that even if somehow malware is successfully deployed into the kernel, the user stored secret data remains protected. Any code that can run in the secured environment is required to be signed by Microsoft, providing an extra layer of control. Microsoft VBS can be enabled in both the Windows Desktop and Server operating systems. Microsoft VBS is built up from a suite of technologies that include credential guard and application guard. You can find [more information about virtualization-based security here](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-vbs).

## Control Layer

The control tier is the layer of the solution that allows administrators to manage the Citrix environment, along with permitting user access to resources. Details on enabling resources to communicate with one another can be found in this section. Due to the integration of this layer, ensuring that components can integrate and communicate securely is key. The following reduces the security posture of the components.

### Ensure Availability

When deploying any solution, components must be deployed in an highly available manner. Having services that are suffering constant outages due to single points of failure is poor practice. Therefore, using the N+1 approach to capacity ensures that there is enough resource available during logon and log off storms. But there is an acceptable level of component loss to retain the 'good' user experience. Now, most customers follow N+1 in terms of planning the amount of resources that need to be available. However, depending on the tolerable level of risk, this may be N+2.

Also, to ensure there are enough resources available to handle the user load, components should be seperated off onto dedicated virtual machines. It is bad practice to run shared components on virtual machines, not only from a perfomance perspective, but aso security. Key components from a Citrix perspective are as follows, but not limited to:

-  StoreFront
-  Delivery Controllers
-  SQL Server
-  Federated Authentication Service
-  Director
-  License Server
-  Cloud Connectors

### Encrypt Data Flows

Businesses are moving to a 'Zero Trust' approach to services across their environments. It is more crucial than ever to ensure that all communications between components are secured, and even authenticated where possible. This step reduces the chances of attackers being able to read confidential information while it is 'inflight' across the network. From a Citrix perspective, this step essentially involves binding a certificate to the relevant services from either a private or public Public Key Infrastructure (PKI).

**High-Security Recommendation** - In high-security deployments, the standards set out suggest that Federal Information Processing Standards (FIPS) may need to be adhered to. This requirement involves changes to virtualised components, and when considering components such as Citrix ADC. Also it  requires accredited hardware appliances. [Citrix Trust Center](https://www.citrix.com/en-gb/about/trust-center/fips.html)

### Build Hardening

As discussed in the **Resource Layer** section, it is highly recommended to harden the operating system and the services that are deployed. This step involves disabling any unused services, scheduled tasks, or features. Be sure not to impact the elements that are used to reduce the attack service on the machine. There are baselines such as the CIS and Microsoft security baselines that can be used to lock down virtual machines running in the environment. Include both the session servers that host the user sessions and the control services such as cloud connectors and supporting infrastructure. An infrastructure machine must equally have any services, features, or scheduled tasks disabled that are not required for the service to operate.

### Service Account Hardening

Some elements of a Citrix solution require the use of service accounts. Service accounts allow for automated functions to progress with some level of authentication and authorization. A service account must only be allowed to carry out the task that is required and must not have any elevated access on the network. Service accounts must be created for each automated function. This step inherently narrows down the authorization elements and ensures that no privilege creep occurs within the service. We recommend ensuring service account passwords are reset at least yearly or more frequently based on your compliance requirements. These accounts and or groups must also be in Protected User groups within the Active Directory for extra protection and logging.

To further harden service accounts, it is also recommneded to deny interactive logon rights to such accounts where possible to ensure that the account cannot be re-used to logon to a network device in the event of the password becoming compromised.

Ensure that the account cannot logon interactively with any account on the network be it a:

-  User account
-  Administrator account
-  Service account

Accounts should be periodically auditted and their permissions verified to ensure that the account is still required. Also check there has been no additional 'privilege creep' across the network. Privilege Creeping is when overtime, additional permissions may be assigned to accounts and never retracted. For example, permissions are added for troubleshooting an issue, but they never get reviewed and removed. The concept of least privilege is not adhered to.

### Least Privilege

This framework is applied to any account on the network. This step will ensure that all accounts created only have the necessary permissions to perform the role. When administrative access is required, users must have separate accounts that are used for performing administrative functions. In an ideal scenario, permissions to accounts must only be granted for the time it takes to perform that task. Once the administrative actions are completed, any permissions that are no longer required must be removed.

### Application Control

Application controls have been covered in the Resource Layer. However, a similar approach can be taken for the control tier. Permitting the executables specific to an application, further reduces the attack surface on running machines. Any unauthorized executables will not be permitted to run. This comes with administrative overhead, but adds an extra layer of control. This step must be applied to all applications deployed. A considertio with this approach is that updates that change finles or executables running on the system need to be preapproved. This step allows them to run after an update is made.

### Host Based Firewall

Most commonly and likely the first thing that administrators disable is the host-based firewall. It can however be enabled for certain scenarios such as troubleshooting. Leaving such services disabled permanently leaves the environment at a high risk of compromise. The firewall is designed to stop attackers from gaining access to the server through unknown or spurious tools or applications. Ensure that the firewall is configured to stop any unauthorized elements from running. But it needs to be configured to allow applications to function and communicate with one another. When you install the Citrix VDA, it automatically creates the firewall rules needed for basic VDA communication. It can add Windows Remote Assistance and the required RealTime Audio ports as well. Test Environments are required to allow administrators to clearly understand how applications function. This step allows you to configure firewalls and services in the production environment with confidence. Avoid negative impact on the application or to the user experience. For ports required to allow Citrix environments to function, read [CTX101810](https://support.citrix.com/article/CTX101810).

### Transport Layer Security

TLS encrypts communications between two or more components. The authentication, enumeration, and launching of a session requires the transfer of sensitive information. If these communications are not encrypted, an attacker may potentially retrieve user credentials or other sensitive data. When hardening environments, enabling TLS is one of the first things that must do. When enabling TLS, follow the recommended practices for the creation of the private keys. There are many articles around key management and the creation of certificate recommended practices out there. We recommend enabling TLS for the following communications as a minimum:

-  STA Service
-  XML Brokering
-  StoreFront (Base URL)
-  ADC Management interface
-  Gateway
-  Director if running on-premises

## Host Layer

Every virtual environment compromises of several components such as storage, compute, hypervisor, and networking. Always design and implementing these components with security focus. It has a significant impact on the continuous reduction of your attack surface. With cloud services, not all are applicable, but most apply regardless of where the resources are located.

### Hardware separation

In today's cloud era, the concept of Hardware Separation has become less of a concern for admins. Also, more businesses are looking for a greater return on investment by ensuring that all hardware is fully utilized. With some cloud providers, this is something that is not even possible. However, with on-premises physical infrastructures, you can still separate resources into unique hosting environments. Such attacks include hyper jacking, where an attacker can drill down into a VM through the VM toolstack. Then the attacker gains access into the hypervisor.

Many of the documented attacks are theoretical. But the fact that they are publicly documented suggests that this type of attack can be effective. Since this is a real possibility, it comes back to the protecting the data. Separate workloads into unique clusters and ensure that workloads hosting the same data classification are retained within those unique clusters. If an attacker broke into the hypervisor layer somehow, higher classifications of data are not compromised.

### Network Separation

Breaking down workloads into individual subnets that are logically separated, can dramatically reduce the impact, or spread of an attack. Usually, these subnet layouts are a perfect place to start:

-  **Access Components.** Small subnet compromising of the ADC IP addresses and call-back gateway.
-  **Citrix Infrastructure.** The Citrix infrastructure subnet depending on the infrastructure being deployed would include the following; StoreFront, Cloud Connectors/Controllers, Director servers, Citrix ADM.
-  **Supporting Infrastructure.** Depending on which infrastructure components are required, these services are prime examples for separation; SQL servers, Jump servers, and Licensing servers. This is dependent on your compliance needs.
-  **VDA Subnets.** There is no right or wrong answer when sizing the VDA subnets. In the past, we have used historical data to guide us around PVS subnet sizing. Over time, PVS recommended practices have evolved. The main thing to note is that subnet sizing must be allocated based on the number of users and VDAs and the security context that they are accessing. Placing users with a similar risk profile into a single subnet can also ensure that each of these subnets can be separated by a firewall.

### Firewalls

Firewalls are one of the primary elements of implementing security in an environment. Implementing host-based and network-based firewalls will introduce significant operational overhead. Implementing two levels of firewalls from both a host-based and network-level will allow for separation of duties. This step alllows an application to communicate from one server to another. Any firewall rules must be well documented and clearly marked as to which roles or functions are assigned. This detail will assist you in getting approvals for exceptions from your security and network teams.

### Hypervisor Hardening

For a hypervisor to function properly, a certain level of service and processes need to be enabled. Like within any operating system, many of the Hypervisor-level services can be disabled to reduce the attack surface in the hypervisor tier. Compromising the hypervisor can be a declaration of a complete overrun from an attacker. The attacker has access to read memory, CPU instructions, and control machines. If an attacker gets into this layer, it would be a serious matter.

### Memory Introspection

Vendors now offer the ability to introspect the running memory of a virtual machine. Monitoring this active memory space provides a deeper level of protection from a more highly sophisticated level of attack. Visit Citrix Ready to [learn more about hypervisor memory introspection](https://citrixready.citrix.com/bitdefender/bitdefender-hypervisor-introspection.htm).

## Operations Layer

The operational procedures for running a secure environment are as critical as the technical configuration itself. The following items provide a great start in building a solid foundation of operational security excellence.

### User Training

Provide sufficient user and administration training. It promotes good security practices and awareness is one of the easiest bases to cover. One example is ensuring that users are aware of the normal expected behavior of a login page. Understand which details the support staff requires. Train staff how to handle pop-ups and those dreaded email phishing attempts. Users must be aware of how to respond to and understand where and how to report security-related issues to security operational centers.

### User Monitoring

Enabling enhanced user monitoring such as user session recording can sometimes be controversial.
Notify users within their employment contract for example, that their actions on the IT systems may be recorded for auditing purposes. You can provide an appropriate level of coverage for any Human Resourcing legal implications. More controversially, it's possible to enable keylogging. These kinds of tools must be approached with some level of caution, as the potential for problems exist. It depends on what the user is accessing while on their corporate machines. Administrators can collect user names or passwords to personal email or even bank accounts, unbeknownst to the user. This needs to be approached with caution and again, the users notified that such actions are being undertaken.

### Logging and Auditing

Ensure logging of all actions of both IT administrators and users. Enabled logging throughout the environment. Include all the layers described within this article. Collate and store these logs for auditing purposes. Retain the logs, just in case they are needed for review, in the case of a breach. This step requires you to continually manage and act upon the logs that are generated. Some applications provide some level of automation within that can enable logs and alerts to be processed for action.

Citrix offers a cloud based solution as part of the analytics service. This service enables security risk scoring of a user. Based on the risk score, you can increase your security stance, disconnect, or even lock the account automatically. These events can then enable functions as **Session Recording** automatically, as discussed earlier in this article. We can disable Citrix features and disconnect the session, until an administrator can review the actions of that user. **Security Analytics** can correlate user behavioral analytics and applied security controls together in an automated fashion. You can find more information in [Citrix Analytics tech brief](/en-us/tech-zone/learn/tech-briefs/analytics.html).

### Segregation of Duties

 Ensure good role-based access control mechanisms are implemented. However, it is also a great approach to ensure that no single administrator can act alone to turn on a feature or egress point. Ideally implement separation of duties to force multiple administrators to act independently to enable a feature. A good example is enabling Client Drive Mapping into a Citrix session. This can be controlled using a Citrix administrative policy enabled in Citrix ADC within a SmartControl policy. Require two separate changes to enable the CDM feature.

It is important to have two administrators to enable a feature. Administrators must also have separate accounts from their normal user account.  The admin accounts are for performing administrative tasks only. This step reduces the attack vector of phishing attacks. If an administrator has their normal user credentials hived up, and these credentials had domain admin permissions, there will be a significant impact on the environment. Therefore the administrator’s normal user account is then unable to carry out administrative tasks. Users must not have global admin rights across a network or domain. Ideally, the use of role-based access controls (RBAC) groups must be used to provide permission for administrative based functions at a more granular level.

### Change Management

One of the common things that some customer rarely consider is the importance of having separate test and development environments. These environments are crucial elements of thoroughly testing updates and changes to understand how they impact the user environment. The infrastructures must be closely interlinked with change control processes to mandate administrators to make changes in the test and development environment before moving into production. As part of testing for installation or upgrade changes in development environments, it is equally important to thoroughly test the rollback of changes. This way the administrators are more intimately familiar with the process, must they ever need to invoke a rollback process within production. Thorough testing and solid roll-out plans protect the production environment from unnecessary outages. Ideally, a rough outline toward going "Live" must comprise the following phases:

-  **Test**. An environment loosely controlled and a more sandbox area for admins to become familiar with the updated software and new features.
-  **Pre-Production**. Pre-Production must be treated similarly to production, closely protected by change control, and kept in lockstep with production. This provides a strong reassurance that the behaviors of upgrades in pre-production are in step with that of production, just on a smaller scale.
-  **Production**. Production goes without saying, its production. Unauthorized changes without Change Control must not be allowed.

### Software Hashes

It is good practice when downloading software from vendor websites to validate the hash provided on the downloads page. This would verify that the file downloaded has not been tampered with by an adversary.

### Security Audits

Security Operations is a wide encompassing topic and includes many elements of user documentation, legal documentation, any environment. As much as the technical aspects of any infrastructure are critical. It's important to keep in mind that the supporting documentation, legalities, and IT Health Checks leading ultimately to "sign off" on use or holding of data such as Personal Identifiable Information (PII) or Payment Card Industry (PCI) are crucial. Ensuring you have regular security audits and penetration tests to validate your security operations and controls are highly recommended.

## Summary

Many security controls are integrated into an environment in the early phases of a project. However, security risks are constantly evolving, and revisiting the controls and procedures in place is a continuous process. It needs to be reviewed frequently. All efforts must be reinforced and validated through penetration testing against the virtualized environment overall. This approach provides the greatest level of resiliency against a real-world attack.
