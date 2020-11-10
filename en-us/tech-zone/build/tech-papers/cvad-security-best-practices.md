---
layout: doc
h3InToc: true
contributedBy: Andy Mills, Patrick Coble, Martin Zugec
specialThanksTo: Eric Beiers, Steven Wright
description: Copy & paste description from TOC here
---
# Security best practices for Citrix Virtual Apps and Desktops

*Disclaimer: This information is provided on an “AS IS” basis without warranty of any kind, for information purposes only,
and is subject to change at any time at Citrix’s sole discretion.*

## Introduction

Global organizations including healthcare, government, and financial services rely on Citrix Virtual Apps and Desktops to provide secure remote access to environments and applications. When properly configured, Citrix Virtual Apps and Desktops provide security measures that extend beyond what is natively available in an enterprise operating system by providing additional controls enabled through virtualization.

This tech paper outlines recommendations and resources for establishing a security baseline for your virtualized environment and highlights some of the most important security improvements you can do. All changes must be implemented in a test or development environment before modifying the production environment to avoid any unexpected side effects.

The information provided in this tech paper is using the traditional Citrix layered methodology used by professional services as shown below:

![Layered Approach](/en-us/tech-zone/build/media/tech-papers_cvad-security-best-practices_001.png)
_Citrix Consulting Layered Approach_

As shown in the diagram, security does not have its own layer. Any security process or functionality is intertwined with all the layers and is crucial that it is covered throughout an infrastructure, including the processes surrounding it.

Many security controls are integrated into an environment in the early phases of a project. However, security risks are constantly evolving and revisiting the controls and procedures in place is a continuous process and needs to be revisited frequently. All efforts should be reinforced and validated through penetration testing against the virtualized environment as a whole. This approach provides the greatest level of resiliency against a real-world attack.

Your organization may need to meet specific security standards to satisfy regulatory requirements. This document does not cover this subject, because such security standards change over time. For up-to-date information on security standards and Citrix products, visit [Security and Compliance Information](https://www.citrix.com/about/legal/security-compliance/) and [Citrix Trust Center](https://www.citrix.com/about/trust-center/).

## User and Device Layer

When a business is considering how to enable their end users to connect to virtual desktops, different options such as Bring Your Own Device (BYOD) or corporate issued devices are available. Both come with their own operational overhead. Endpoint delivery model can affect many design decisions -  including decisions around what features can be offloaded to the endpoint. A user connecting from a BYOD device may have no access to clipboard, client drive mapping (CDM), or printing. User connecting from a 'trusted' corporate owned device can have access to clipboard and local drives. There is no 'one size fits all' for endpoint devices. Endpoint clients are selected based on the use case, mobility, performance, cost, and security requirements.

### Device Lockdown

The endpoint device has potential to be used as an attack vector; including such attacks as keyloggers or points of ingress into the network. One major benefit when using Citrix technologies like Virtual Apps and Desktops, the ingress point is reduced to mouse, keyboard and screen refreshes. If users require additional functionality such as accessing local client drives, printing facilities and clipboard functionality, these can be enabled on a case by case basis. As capabilities increase Citrix does have controls to reduce the risk of these attack vectors such as anti-keylogging and disabling any points of ingress/egress.

-  Do users require local administrative permissions on the device?
-  What other software is running on the endpoint? Can additional software be installed?
-  Does the user have a VPN capability?
-  Who updates the device in terms of operating system and application patching?
-  What resources can the endpoint access?
-  Who has access to the endpoint itself?

These are all questions for the IT support teams to consider when deploying devices to the end users.

### Endpoint Logging

Even if a user cannot install software on the endpoint, ensuring logs are enabled and centrally captured helps with understanding what have occurred in a breach or providing to the forensic analyst for further review. Logging is a crucial element to any environment security monitoring process. There is a trade-off though, it is easy to get overwhelmed by the amount of reported data. Make sure you are able to detect and respond to a potential alert. It is also a good practice to forward all logs to your security information and event management (SIEM) and set alerts for known attack methods or other key events.

### Thin Client terminals

In many scenarios, thin client devices are perfect for high-risk environments. Thin clients run a cut down version of operating systems only having enough of an operating system and application to connect to a Citrix session. There are other benefits, like patching - there are limited applications to be patched and maintained, and usually the operating systems have reduced footprint. Due to the devices having a simplified and purpose built operating system, there is limited data at rest on the endpoint. For thin client terminals that run a full operating system, the use of write filters to stop the persistence of data is useful. Resetting the terminals when reboot occurs reduces the chance of an attacker persisting data on the endpoint that can be used to formulate an attack, just make sure you are not destroying logs and other data important for forensic analysis. Thin Client terminals have the added benefit of usually being cheaper to purchase and maintain than traditional desktops or laptops.

### Patching

Patch management must be one of the first considerations for any endpoint. How does the business get security fixes applied to the endpoints? In traditional environments machines have patches pushed to them through Windows Services Update Service (WSUS), System Center Configuration Manager (SCCM) or through other automated services. This is great if the endpoint is managed by the corporate IT teams. What about BYOD, who patches those and ensure they are up to date. With work from home growing quickly endpoint patching becomes more complicated. This is where clear responsibilities of the users are called out to ensure that users know how to patch and update their systems. Simple notifications on the login page suggesting a new update has been released and they should patch as soon as possible is one method. You can also use [Endpoint Analysis Scans (EPA)](/en-us/citrix-gateway/current-release/vpn-user-config/advanced-epa-policies.html) on the endpoint to deny access to the infrastructure unless they are running up to date software.

It is also important to ensure that the Citrix Workspace app is patched and up to date on the endpoint client as Citrix provides not only feature enhancements, but also security fixes in new releases.

### Account Management

Through the user and device layer there has been much focus on the device security element. How user accounts are created and authorized to resources needs to be centrally managed and efficient. Many times, accounts are 'copied' from similar roles. As much as this speeds onboarding, it also creates creep in terms of permissions and unauthorized group additions in the central directory. Copying a user's permissions from a previous account could lead to the unauthorized access to data. Ideally any group request or access has to be authorized by the data owner before being permitted to gain access.

Decomissioning accounts can be simple if integrated properly with HR systems. There are times though when business need to flex resources in and out from 3rd parties including vendors, additional support for busy periods or when outsourcing elements of the business. How do those accounts get decommissioned and ultimately deleted? Ideally the business has a procedure for both onboarding and offboarding any contracting resource with end dates set to at least disable the account and moved into an 'holding OU' within Active Directory, then deleted when the resource is confirmed as no longer being required. Ultimately, we do not want contractors having the ability to log in months after they have left the business.

## Access Layer

Within the Citrix design process, the Access layer is where users authenticate, the necessary policies are applied and dynamic contextual based access is evaluated. Therefore, ensuring that the access tier is designed with a strong level of security in mind is crucial.

In the following article, we aim to cover the main tasks to help further secure a Citrix ADC deployment. There are usually two major deployment types for Citrix ADCs, it is used as a proxy for Citrix Virtual Apps and Desktop deployments or load balancing other applications to make them more highly available and secure. These items in this checklist help lower your risk and exposure for items that the Citrix ADC is interacting with.

### Strong authentication

Recommended for at least all external connections to any internal system. With the number of user names and passwords that are searchable by attackers from breaches and leaks over 12.3 billion records it continually increases the risks of a breach into your company as those records grow. Password reuse is common with almost everyone in the world and your business can be at risk due to your users password habits. A user name and password should not be your only defense to your applications, and we should not inherently trust people with just two pieces of information that can be easily stolen. Adding multifactor authentication to all applications may not be feasible with the user's workflow, but we do recommend deploying wherever possible especially for external access.

You can find more information in [product documentation](/en-us/citrix-adc/current-release/aaa-tm/authentication-methods/multi-factor-nfactor-authentication.html).

### HDX encryption

It has been a long since standard for enabling RC5 – 128 bit as the security encryption for the HDX/ICA stream. This does provide some level of security on the ICA stream. For those who are looking to gain that extra level of encryption on the ICA stream between either the endpoint and the VDA, or between the ADC and the VDA, enabling VDA SSL is recommended. This binds a certificate to the Desktop service encrypting the ICA stream to the given standard of the certificate bound. For more information on how to carry out this process, read [CTX220062](https://support.citrix.com/article/CTX220062).

### SSL/TLS Ciphers

Validate TLS Ciphers and SSL Score for all External VIPs (Citrix Gateway). There have been many vulnerabilities in the SSL and TLS protocols over the past couple years and ensuring you are using all the TLS best practices as they are ever changing. Ensure older protocols like SSLv3 and TLS 1.0 and TLS 1.1 are disabled.

Choosing between a Wildcard or SAN certificate is another aspect of this process. Wildcards can be very cost effective, but also can be more complicated based on the number of hosts that will be impacted with expiration and deployments. A SAN certificate can effectively be a restricted wildcard for just 2–5 sites, along with the ability to use more than one TLD in the same certificate too depending on the provider.

Read [Citrix Networking SSL/TLS Best Practices](/en-us/tech-zone/build/tech-papers/networking-tls-best-practices.html) for latest recommendations.

### Encrypting XML

Ensure your XML traffic from your Delivery Controllers or Cloud Connectors to your StoreFront servers is encrypted. This prevents someone with just network access from seeing what is being requested by users. This requires issuing certificates for each server along with changing the configuration of each server to use the new encrypted port (by default port 443). You can find more information in [product documentation](/en-us/citrix-virtual-apps-desktops/secure/tls.html#install-tls-server-certificates-on-controllers).

**High Security Recommendation** - For a high security deployment it is recommended to use non-standard ports for obfuscation along with ensuring there are firewalls configured between the server roles for this XML traffic to control traffic bidirectionally.

### StoreFront SSL

We recommend using an encrypted Base URL for any StoreFront deployment to ensure that credentials or session launch data are not traversing a network without transport protection. In most deployments this can be issued from an internal certificate authority since these servers will either be behind a Citrix ADC deployment as part of the ICA Profile settings for a Citrix Gateway for external devices or behind a Citrix ADC VIP that may typically be accessed by internal devices only. Ensure the use of strong ciphers and TLS 1.1 and/or 1.2 for these servers communication. This process is outlined in [Microsoft documentation](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn786418(v=ws.11)).

**High Security Recommendation** - In high security deployments we recommend enabling ICA file signing to ensure the files received by the Citrix Workspace app are trusted. This process is outlined in [StoreFront product documentation](/en-us/storefront/1912-ltsr/configure-using-configuration-files/strfront.html#enable-ica-file-signing).

### VDA Encryption

This is typically reserved as one of the last items to increase the session transport security when many of the other tasks hardening tasks are completed.  This requires configuration to be completed on VDA and then on the Delivery Group objects.

**High Security Recommendation** - In high security deployments we recommend enabling VDA encryption to further protect the transport of the Citrix session information. Using the PowerShell script is the easiest method and that process is outlined in [product documentation](/en-us/xenapp-and-xendesktop/7-15-ltsr/secure/tls.html#tls-settings-on-vdas).

### Control Physical Access

Citrix ADC appliances must be deployed in a secure location with sufficient physical access control to protect them from unauthorized access. This applies to physical models and their COM ports to virtual models on virtualization hosts and their associated ILO\DRAC connections. Find more information in [Citrix ADC product documentation - physical security best practices](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#physical-security-best-practices).

### Change Default Passwords

Having any default password that is known by the IT community is a high risk. There are scanners that search for default credentials being used on a network and their effectiveness can drastically be lowered or eliminated by just changing the default password. All service account passwords should be stored in a secure location like a password manager. The secure storage of all service account passwords is a foundational information security standard. Password for each role and deployment (HA Pair) should be unique and credentials should not be reused. Find more information in [Citrix ADC product documentation - Change the default passwords](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#other-network-security-considerations).

-  **nsroot** - Change the default when you start the initial configuration of the device. This account allows full administrative access and protecting this password should be the number one priority when deploying this system. If you are deploying a new Citrix ADC system in 2020 it has the Serial Number of the appliance as the default password.
    -  Create another superuser that is used in emergency situations when external authentication is down instead of the default `nsroot` account. Find more information in [Citrix ADC product documentation - Create an alternative superuser account](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#administration-and-management)
    -  SDX users must also create an admin profile to configure the default admin password for each VPX instance provisioned. We recommend creating a different Admin Profile for each Instance so each HA pair has the same password but is unique to the other instances. Find more information in [knowledge base article CTX215678](https://support.citrix.com/article/CTX215678)
-  **Lights on Management (LOM)** –  Common in many physical Citrix ADC platforms. This account allows access to the command line of the appliances along with power control of the device. These also have default passwords that should be changed upon initial configuration to help prevent unauthorized remote access.
-  **Key-Encryption-Key (KEK) Password** – This is an optional configuration that encrypts your configuration for sensitive password areas locally on each appliance. Find more information [in KEK chapter](#data-protection-with-kek).
-  **SVM Admin Account** – This account and password change will only apply to Citrix ADC SDX platforms. This has the default `nsroot` password and does not use the serial number like physical appliances. This should be changed upon its initial configuration to help prevent unauthorized remote access.

### Secure NSIP traffic from all network access

Every device in your network should not be able to manage your Citrix ADC appliances. Your management IP addresses should be in a VLAN that you can control ingress and egress to ensure only authorized privileged workstations or other authorized networks can access them to manage the devices. Many of the vulnerabilities that have been found over the past 10+ years have been related to having access to the NSIPs and by securing these IPs in a controlled manner you can eliminate or drastically lower this attack vector. When limiting access to the management of any IT system it must be documented thoroughly and also have business continuity planning completed to understand device management impacts if your planning IPs or Subnets are down and how you can recover if other systems are still operable.

-  Ensure that Subnet IPs (SNIPs) and Mapped IPs (MIPs) don't have management enabled on them. This could lead to your DMZ network having access to the management console via this SNIP or MIP because it was unintentionally checked.
-  To check, browse to your SNIP or MIP via HTTP and HTTPS. Or within the console navigate to System – Network – IPs and select the IP and you see if the management check box is selected.
-  You can use Access Control Lists within the Citrix ADC to limit access to the management of the appliance to specific hosts or subnets without the management being placed behind a firewall. You can find more information in [CTX228148 - How to Lock Down Citrix ADC Management Interfaces with ACLs](https://support.citrix.com/article/CTX228148) and [Citrix ADC product documentation - Network security](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#network-security).
-  If your Citrix ADCs are placed behind a firewall you need to plan based on the services the appliance is delivering along with access to the management to each device along with any supporting maintenance or security devices like Citrix ADM and Citrix Analytics. You can find more information in [Communication Ports Used by Citrix Technologies](/en-us/tech-zone/build/tech-papers/citrix-communication-ports.html)) and [CTX113250 - Sample DMZ Configuration](https://support.citrix.com/article/CTX113250).
-  In higher security deployments, you can also change the management ports from the default HTTP (TCP 80) and HTTPS (TCP 443) to a custom port to create obfuscation and help prevent identification from typical scans. This must be done on each appliance. You can find more information in [Citrix ADC product documentation](/en-us/citrix-adc/current-release/system/basic-operations.html#how-to-configure-http-and-https-management-ports-for-internal-services).
-  Limit Egress (outbound) and Ingress (inbound) from the management networks that the Citrix ADC is deployed to. Having unlimited and monitored network access from any device to anywhere is an inherent risk. Limiting external exposure to external assets is a great step outside of even the Citrix ADC control plane. There should also be limitations of what can access these management resources so that only specified devices or ranges can manage your Citrix ADCs.
-  It is a common practice to keep all the NISIPs, LOMs, and SVMs on a separate network from other networking equipment. Depending on your policy standards you may put the LOMs on physical/out-of-band management networks with other similar systems and the NSIPs and SVMs on another network. It can also be helpful to have a subnet for them to make a standard

### Bind Citrix ADC to LDAPS

Using generic login accounts for day-to-day administration is not recommended. When all configuration changes are done by `nsroot` in an IT team of 20+ people who have access to the password manager, you will not be able to easily track who logged in and made the change. We recommend once the `nsroot` accounts password has been changed that the system is immediately bound to LDAPS to track usage to a specific user account. This will also allow you to have better delegated control so you can create View only groups for auditing and application teams and allow full admin rights to specific AD groups. We do not recommend binding using unencrypted LDAP as credentials could be collected with packet captures. You can find more information in [CTX212422](https://support.citrix.com/article/CTX212422).

If binding to Microsoft Active Directory, ensure you are only using LDAPS as the protocol. Also ensure NTLMv2 is the only hashing method for credentials and even network sessions too. This is an AD security best practice to ensure that credentials are as protected as possible while in transit for authentication requests and network sessions. Properly test this configuration and validate with older Windows clients - there can be compatibility issues with older versions of Windows.

When binding to any external authentication source you should either disable local authentication for accounts like `nsroot` or only enable specific local accounts to use local authentication. Depending on your deployment requirements may require one path or another. Most deployments don't require local accounts - if there is a service account needed, you can create and delegate an LDAP user. [This guide](https://www.citrix.com/blogs/2020/09/23/how-to-secure-restrict-citrix-adc-management-access/) covers setting up both methods, one method should be deployed.

### Logging and Alerting

Having the Syslog logs forwarded is critical to provide incident response along with more advanced troubleshooting. These logs allow you see login attempts to the ADC system and to the Citrix Gateway if used, actions taken by policies such as GeoIP & BadIP blocking, AppQoE Protection, Responder Policy Actions and other packet and system operations. This information can be invaluable during troubleshooting issues along with understanding your current attack situation and to be able to react to either with actionable data to fix problems or stop\mitigate an attack. Without a Syslog target configured on your Citrix ADC you can get in a situation where required logs have been deleted to save space for the system to stay operational. You can find more information in [Citrix ADC product documentation - Logging and monitoring](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#logging-and-monitoring).

There are many free and paid Syslog servers\collectors available. Some are priced by the number of messages per day, storage space of messages or they are just a continuous subscription. These solutions should be planned as they require a sizable amount of storage space to be allocated based on the number and size of the events each day. Your Syslog server\collector needs to be sized based on the number of events from other critical services like Active Directory, database and file servers, VDAs, and other application servers.

Citrix offers the Citrix Application Delivery Management service as a Syslog collector which can allow you to have some off-device retention. The Citrix ADM is also a great tool to be able to search and view these logs and also use its events dashboards for many great default views of this log data to help you troubleshoot and view hardware failures, authentication failures, configuration changes and many others. You can find more information in Citrix ADM product documentation - [Configuring syslog on instances](/en-us/citrix-application-delivery-management-service/setting-up/configuring-syslog-on-instances.html) and [View and Export syslog messages](/en-us/citrix-application-delivery-management-service/networks/events/how-to-export-syslog-messages.html).

We recommend using this list of messages to create your alerts based on what features of the Citrix ADC are in use. Just having the logs retained and searchable will be valuable to troubleshooting and auditing security events. It is important to ensure you are alerting based on threshold for bad logins along with any logins using the default `nsroot` accounts. You can find more information in [Developer Docs - Syslog Message Reference](https://developer-docs.citrix.com/projects/citrix-adc-syslog-message-reference/en/latest/).

SNMP should also be configured to your monitoring solution to ensure you have service and physical that type of monitoring enabled to ensure services or the appliance are up.

Configuring Email allow system notifications to be sent out (expiring certificates and others).

### Plan Services to Monitor and Protect

Planning what services you are using on your Citrix ADC helps you understand what features may be applied to those IP addresses. This is a good point to also know if you have IPs assigned to test. You should always validate all security settings on a Test VIP before applying the responder and other policies to your Production VIP. This can require more DNS records, SSL Certificates, IP addresses and other configurations to make it work. The main three items that the Citrix ADC are commonly used for are following.

-  Load Balancing
-  Global Server Load Balancing
-  Citrix Gateway

We recommend organizing all the planned VIPs by Internal and External. Internal VIPs might not require all the security features that are important for external VIPs. Evaluating which countries may need to access external resources is a good planning step if you plan on using the GeoIP features. Work with the business leadership and applications owners to find if you can restrict countries.

If available, deploy Citrix ADM before you start making configuration changes. Citrix ADM can back up the appliances regularly and provide better log visibility along with some other metrics. There are other more advanced features that can be added to a Citrix ADC deployment for better visibility and security. Citrix ADM has paid editions along with Citrix Analytics that can go beyond just a monitoring solution to an automatic security response system.

### Replace Default SSL Certificates

When default certificates are left on anything managed or accessed over the web, all modern browsers warn you that the certificate is invalid and could be a security risk. If you are accepting this error every day you could be accepting a true man-in-the-middle attack where someone could be seeing what your typing because they are in the TLS stream. Replacing the certificate can be easy depending on if you host your own internal Certificate Authority, use a third-party service (possible cost associated) or self-signed certificates. Once you have done this your browser should trust this certificate and should not give you any errors while accessing the device, and if it ever does again you should check the expiration and check to see if it has been replaced without your knowledge.

-  **NSIP** – Changing this certificate should be done first, as it is where all management for the appliance happens. This process needs to be completed on each appliance. You can find more information in [CTX122521](https://support.citrix.com/article/CTX122521).
-  **LOM** – Changing these certificates should be the next goal once the NSIP management Certificates has been changed from default. This process needs to be completed on each appliance. You can find more information in [product documentation](/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/installing-a-certificate-key-on-lom-gui.html).
-  **SDX SVM** – If you are an SDX customer, ensure that this certificate has been replaced as this will be your primary method to manage the appliances along with managing the instances themselves. This process must be completed on each appliance. You can find more information in [CTX200284](https://support.citrix.com/article/CTX200284).

### Firmware Update

1.  **Initial Deployment** - Use the latest firmware version for each Citrix ADC platform after reviewing the release notes to see if there are any "known issues" that may impact you. N-1 methodology is often used to always be just one version behind from the latest release. Firmware updates are done on each instance no matter which platform you are using like CPX, MPX, SDX, or VPX.
1.  **LOM\IPMI Firmware Updates** - We recommend also ensuring that this is updated at the time of deployment to ensure you have the latest features and fixes. You can find more information in [product documentation](/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/upgrading-the-lom-firmware.html).
1.  **Ongoing Updates** - [Sign up for alerts](https://support.citrix.com/user/alerts) for Citrix Security Bulletins and Update notifications for the Citrix ADC, Citrix ADM and Citrix Web Application Firewall.

Plan on how often and when you update your Citrix ADCs each year. Features deployed and your IT organization policies determine how you may schedule these updates. We typically see updates happening 2–4 times per year with two updates being feature updates and two updates for known issues and security fixes. There are guides to help you plan and schedule these upgrades without service interruption. This policy should also go to other supporting systems like Citrix ADM, as version parity is always recommended. Learn more about [upgrading a high availability pair](/en-us/citrix-adc/current-release/upgrade-downgrade-citrix-adc-appliance/upgrade-downgrade-ha-pair.html).

### Data Protection with KEK

We recommend enabling further data protection on each appliance pair by creating a KEK key pair. This encrypts the configuration in key password related areas - if someone gains access to your `ns.conf` file, they are not able to harvest any credentials or passwords from it. The two key files that are at the root of the `\flash\nsconfig\` folder should be classified as sensitive and protected accordingly with backups and secure storage of them.

This also means that migrations from one appliance to another appliance requires more steps as the KEK key must be added to the new deployment before configuration can be decrypted. [Learn more](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html) about creating a master key for data protection (search for string `KEK`).

### Drop Invalid Packets

There are many invalid packets that are sent every day to your Citrix ADC device. Some are benign, but most are used for fingerprinting purposes along with protocol-based attacks. Turning this feature on saves CPU and memory on your device by not sending the partial packet to the back end application that is being proxied by the Citrix ADC. By dropping these types of packets it will prevent many protocol attacks known today and block potential future attacks that rely on packet manipulation.

You can find more information in [product documentation](/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#applications-and-services), [CTX227979](https://support.citrix.com/article/CTX227979) and [CTX121149](https://support.citrix.com/article/CTX121149).

### HTTP Strict Transport Security

Ensure HSTS is configured on all SSL VIPs. HTTP Strict Transport Security main goal is to protect applications by various attack methods like Downgrade attacks, Cookie Hijacking and SSL Strip. This is similar to Drop Invalid Packets but is based on HTTP\HTTPS based standards found in [RFC 6797](https://tools.ietf.org/html/rfc6797) by using an entry into the HTTP header. This adds yet another layer of defense to applications behind the Citrix ADC.

## Resource Layer

The resources that host the user session can present an additional level of risk to compromise. A user running a VDI session is as good as having a computer on their desk connected to the corporate network. Utilizing a well-designed access and control layers allows the business to move towards a zero-trust models; dynamically adjusting access to the resources presented to the end user dependent upon a given set of variables. The following guidance provides additional levels of control in protecting the corporate assets from users.

### Build Hardening

Hardening operating system builds can be complex and difficult to achieve with the trade-off of user experience, usability, and security all being a fine balance. Many customers usually follow the Center for Internet Security (CIS) baselines for hardening virtual machines across the varying roles. Microsoft also provides hardening guides for similar workloads and even the ADMX files to implement directly into group policy. Proceed with caution and make sure to test thoroughly first, as these initial policies may be overly restrictive. These baselines are a starting point for hardening and are not meant to be exhaustive for all scenarios. The key to lock down is to test thoroughly and encourage 3rd party penetration testing engagements to validate your security controls and their effectiveness against the latest attack methods.

As part of hardening the system, it is recommended that administrators spend some time optimizing the underlying operating system, services, and scheduled tasks to remove any unnecessary processes from the underlying system. This improves the responsiveness of the session host and provides an improved user experience to the end user. Citrix provides the optimizer tool [Citrix Optimizer](https://support.citrix.com/article/CTX224676) that optimizes many elements of the operating system automatically for administrators. It is recommended to review what the Citrix Optimizer is going to adjust to ensure there is no negative impact within your environment.

### Windows Patching

Ensuring IT systems are patched and up to date is a standard practice for all software, operating systems and hypervisors. Providers are obliged to ensure that their software or application is patched if any security vulnerabilities are identified along with other stability and feature enhancements. Nearly all vendors have a release schedule or cycle for applying patches and updates to their software. It is recommended that customers read and understand what is being patched, or features introduced. These updates are then processed through route-to-live solutions. Proper testing can help to avoid outages caused by changes to the production environment.

### Anti-malware

Anti-malware or antivirus is always recommended to be deployed on all servers throughout the infrastructure. Antivirus does provide a good first line of defense against known malware, and many other types of viruses. One of the more complex aspects of antivirus is ensuring that the virus definitions are updated regularly, especially in non-persistent VDI or hosted shared workloads. There are many articles on redirecting antivirus definitions to persistent drives and ensuring that the machines are identified as individual objects in the antivirus management suite. These all need to be followed to ensure that antivirus is deployed and installed correctly. Antivirus vendors may also have their own recommended practices for deploying their antimalware software, it is recommended to follow their guidelines for correct integration. You can read more in [Endpoint Security and Antivirus Best Practices](https://docs.citrix.com/en-us/tech-zone/build/tech-papers/antivirus-best-practices.html).

### Workload separation

Placing resources into separate silos has long been a recommend practice, from not only a resource layer but also in hardware and networking layers which is covered off later in this article. Separating workloads into various delivery groups or machine catalogs will not only help fine tune security policies to a set of machines, but also reduce the impact of a security breach. If you have a set of high-risk users accessing high impact data, then these should be separated off into a separate environment with much stricter policies and logging applied, other capabilities such as session recording may provide additional layer of assurance when using are accessing this data.

Workloads can be separated on different levels - hardware separation (dedicated hosts), VM separation or even inside OS separation (for example app masking or strict NTFS rules). As part of separating users into various tiers of risk profiles, the data they access must also be treated similarly. This involves applying the correct file permissions and access rules to data.

### Application Control

Technologies such as AppLocker can be hard to implement, but they are very powerful, especially in protecting servers with published applications. Being able to granularly lockdown to the executable level what can or cannot run and by whom is beneficial; not to mention the logging capabilities when applications are attempted to be launched. In large enterprise environment (100s or 1000s of applications), implementation needs to be carefuly considered.

### Windows Policies

Windows policies when applied to a session host whether it be a VDI based workload or server based, play two major roles:

-  Hardening the operating system
-  Optimizing the user experience

To ease the management of operating systems these should be applied through Microsoft Group Policy which eases the image creation process. If Group Policy is not used then an alternative method of delivering the lockdowns must be found. Hardening and optimizations that cannot be delivered by policy should be automated as part of the image build process.

Ensuring that the operating system is hardened prior to users gaining access limits the unauthorized execution of processes or applications. Users should only be able to carry out the minimal number of tasks that are required to perform their role, any administrative based applications must be locked down and access disabled for users. This reduces the risk of a user being able to 'break out' out their session and potentially either gaining access to unauthorized data or perform malicious acts on the operating system.

### Citrix Policies

Policies are applied and enforce behaviors dependent upon the user scenario. Typical Citrix session evaluations include, turning off clipboard access or client drive mapping if this is deemed necessary; or even enabling one-way clipboard to allow data to be copied into a session, but not out of a session for example. There are several policies that can assist with controlling unauthorized egress of data from the system session. Each policy need to be carefully planned and understood.

Many of the hardening configurations that were discussed under system hardening of this article can be applied in group policies. Allowing for a consistent application across all servers that are applied at boot up before the user logs on. This allows for a consistent user experience across the infrastructure; which eases the troubleshooting of issues, along with ensuring security controls are consistent across all assets and all users, unless they are have been excluded from some controls based on their job role or function.

To provide enough time to allow policies and other lockdowns are applied to the session, configuration to the delivery groups to allow the VDA to settle and to wait before allowing users to logon, this ensures that all the policies have applied to the server before the user logs in. Read more about [SettlementPeriodBeforeUse in Developers Docs](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/latest/Broker/Set-BrokerDesktopGroup/).

### Image Management

Proper implementation of Image management for many customers is a significant time saver for customers. Those of you who are familiar with the Citrix products know that the two offerings are Machine Creation Services (MCS) and Provisioning Services (PVS). Image management not only provides great benefit to operational procedures, but also security ones. First, a lockdown, or control is applied in a build consistently across all the machines that users access their resources from. Manually building individual machines is not only time consuming but it's almost a guarantee that configurations and settings are missed and inconsistent. Set once and roll out to all the machines provides peace of mind that a security implementation is consistent across all the machines. Secondly, during the lifetime of a user session on a VDA, the user may open other applications, documents, and access sensitive data which is ultimately cached on the virtual desktop or within the application, once the user is then logged off remnants of data will undoubtedly be left behind, therefore after a session is logged off rebooting the machine and reverting to the original golden master image provides a level of reassurance that any sensitive data is wiped clean ready for the next user to login and commence work without the risk of accessing the previous users data. You can find more information in [Image Management reference architecture](/en-us/tech-zone/design/reference-architectures/image-management.html).

There are few considerations for single image management, those already that have been discussed such as antivirus configurations. Also, do not create an account on a template or image before it is duplicated by Machine Creation Services or Provisioning Services, do not schedule tasks using stored privileged domain accounts, any service account must have a dedicated account with the relevant permissions applied. These practices help prevent a machine attack from obtaining local persistent account passwords and then using them to log on to MCS/PVS shared images belonging to others.

### Session Recording

Session Recording provides IT teams with the ability to record and replay what occurred within a given session. In the case that a user was carrying out something that may be malicious within the environment. This ability may not be needed for all users but may be enabled for key individuals, user groups or when accessing sensitive applications, desktops or resources. There are many elements that could be gained from these recordings that may not be possible with just Windows event and application logs that can be helpful in an incident response scenario. This feature is powerful and should be carefully considered based on your user policies and agreements with your legal and IT team's approval.

### App Protection Policies

Recently Citrix has released additional controls that can be implemented to provide additional level of security control when users are accessing the environment. App Protection policies that include anti-keylogging and anti-screen capture provides reassurance that should attempts be made to egress data during sessions that some level of due diligence has been carried out the IT teams to deter users from doing this. What about a camera I hear you ask? Well, Citrix offer watermarking functionality which doesn't stop an individual taking the picture, but it does watermark the session with key details so that the screenshot can be traced back to its origin. Although it may not entirely stop someone from taking a picture, at least it can be traced back to the source of egress.

### Concurrent User

A more contentious topic has been the use of multi-session hosts vs single user VDI. Having multiple user's logon to a single server can cause issues, especially if a disgruntled user has been able to run software or code to commence hiving up other credentials or browsing into other user directories. If a 1:1 mapping of user to desktop is achieved, then this risk is mitigated. Running solid virtual desktop infrastructures does come at an additional cost and the trade off needs to be considered. Again, threat modeling our users and selecting the most efficient delivery mechanism in terms of a multiuser session host or single user session host can be chosen. One size does not fit all users, and carrying out user profiling in understanding these requirements and then selecting the best delivery method of workloads for the user groups.

### Virtualization-based Security

Virtualization-based security (VBS) essentially uses a secure portion of memory to store secure assets from the session. This feature requires a Trusted Platform Module (TPM) or Virtual Trusted Platform Module (vTPM) running on a supported platform to provide the secure integration. This essential means that even if malware were successfully deployed into the kernel, the user stored secrets remain protected. Code that can run in the secured environment requires to be signed and approved by Microsoft to execute providing an additional layer of control. Microsoft VBS can be enabled in both Windows Desktop and Server operating systems. Microsoft VBS is built up from a suite of technologies including credential guard and application guard. You can find [more information about virtualization-based security here](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-vbs).

## Control Layer

The control tier is the aspect of the solution that allows administrators to administer the Citrix solution, along with permit user access to resources. Other key elements of enabling resources to communicate with one another also occurs in this section. Due to the integration of this layer, ensuring that components can securely integrate and communicate is key. The following will reduce the security posture of the components.

### Build Hardening

As previously discussed in the Resource Layer, it is recommended to harden the operating system and services that are deployed. This involves disabling any unused services, scheduled tasks, or features that aren't used to reduce the attack service on the machine. There are baselines such as the CIS and Microsoft security baselines that can be used to lockdown virtual machines running in the environment; including both the session hosts hosting the user sessions but also the control services such as cloud connectors and supporting infrastructure. An infrastructure machine should equally have any services, features, or scheduled tasks disabled that are not required for the service to operation.

### Service Account Hardening

Some elements of a Citrix solution require the use of service accounts, service accounts allow for automated functions to progress with some level of authentication and authorization. A service account should only be permitted to carry out the task that is required and most certainly should not have any elevated access on the network. Ideally service accounts are created for each automated function to narrow down the authorization elements and ensure that no privilege creep occurs on the service. We recommend ensuring service account passwords are reset at least yearly or earlier based on your compliance requirements. These accounts and or groups should also be in Protected Users within Active Directory for extra protection and logging.

### Least Privilege

As discussed, this framework is applied to any account on the network. This ensures that any account that is created only has the necessary permissions to perform the role. When administrative access is required, users should have separate accounts that are used for performing administrative functions. In an ideal scenario, permissions to the accounts to perform an action should only be permitted for the time in which the task is being performed. Once the action on administrative tasks has been completed, any permissions that are no longer required should be removed.

### Application Control

Application controls has been covered in the Resource Layer, however; a similar approach can be completed for the control tier. Permitting the executables specific to an application further reduces the attack surface on running machines. Any executables that have not been authorized will not be permitted to run. This does come at an administrative overhead, but is an extra layer of control that can be applied to the applications that have been deployed.

The main problem with this approach is that any update that changes files or executables running on the system need to be reapproved to allow them to run post the update.

### Host Based Firewall

One of the most common and probably the first thing that most administrators disable is the host-based firewall. As much as this may be OK for certain scenarios such as troubleshooting. Leaving such services disabled permanently does leave the environment at high risk of compromise. The firewall is designed to stop attackers from gaining access to the server through unknown or spurious tools or applications. Ensuring that the firewall is configured to stop any unauthorized elements from running, it is needed to be configured to allow applications to function and communicate to one another. When the Citrix VDA Installs it will automatically make the rules needed for basic VDA communications, but it also can add Windows Remote Assistance and the required RealTime Audio ports also. Environments such as test environments are required to allow administrators to understand how the application functions to correctly configure firewalls and services correctly within the production environments without negative affect to the application or user experience. For ports required to allow Citrix environments to function, read [CTX101810](https://support.citrix.com/article/CTX101810).

### Transport Layer Security

Transport Layer Security (TLS) encrypts the communications between two or more components. During the authentication, enumeration and launching of a session requires the transfer of sensitive information. If these communicates aren't encrypted an attacker could potentially retrieve user credentials or other sensitive data. When hardening environments enabling TLS is one of the first things that should always be carried out. Although part of enabling TLS, following recommended practices in terms of creation of the private keys. This is not to be delved into within this article as there are many articles around key management and creation of certificate recommended practices. TLS should be enabled for the following communications as a minimum:

-  STA Service
-  XML Brokering
-  StoreFront (Base URL)
-  ADC Management interface
-  Gateway
-  Director if running on-premises

## Host Layer

The hardware layer in virtual environments compromises of components such as storage, compute, hypervisor, and networking. Continuing with designing and deploying these components with security as the focus will continue the reduction of attack surface. As with cloud services, not all these are applicable, but others apply regardless of where the resources are located.

### Hardware separation

In the cloud era this has slowly being forgotten about and more business and companies are looking for a greater return on investment by ensuring that all hardware is full utilized. In cloud provider examples, this is something which is not even possible. However, in on-premises infrastructures separating up resources onto separate physical hosting infrastructure. Such attacks as hyper jacking where the attacker can drill down through a VM and the virtual machine tools running the OS through into the hypervisor. Although many of the documented attacks are theoretical, the mere fact its documented suggests that there may be a method if such an attack could be effective - or one day it may be possible. Given this, and again, it comes back to the data you are protecting; separating workloads into separate clusters and ensuring that workloads hosting the same data classification are retained within their own clusters. Therefore, if an attacker were to break out into the hypervisor layer, that higher classifications of data is not compromised.

### Network Separation

Breaking down workloads into subnets that are logically separated can further reduce the spread of an attack. Usually the following subnets layouts are a good start:

-  **Access Components.** Small subnet compromising of the ADC IP addresses and call-back gateway.
-  **Citrix Infrastructure.** The Citrix infrastructure subnet depending on the infrastructure being deployed would include the following; StoreFront, Cloud Connectors/Controllers, Director servers and Application Delivery Manager (ADM).
-  **Supporting Infrastructure.** Depending on what additional infrastructure components are required, the following services are prime examples of being separated; including SQL, jump servers and licensing server. This all depends on what level of environment separation is required and mandated.
-  **VDA Subnets.** I haven't found a right or wrong answer on the VDA subnet sizing. History used to have guidance around PVS subnet sizing, but again; as time as gone on and PVS recommended practices have evolved somewhat. The main thing here is that subnet sizing is suitable to the number of users / VDAs and the security context that is being accessed. Logically placing similar risk profile of users in a single subnet does seem logical, ensuring that each subnet is then separated by a firewall.

### Firewalls

Firewalls are one of the main elements to implementing security in an environment. Implementing host based and network-based firewalls do introduce an amount of operational overhead. Implementing two levels of firewalls from both a host based, and network level does allow for separation of duties to permit an application to communicate from server A to server B. Ideally any firewall rules should be documented and clearly marked as to what their role and function is to ease approvals from the security and network teams.

### Hypervisor Hardening

For a hypervisor to function a certain level of services and process do need to be enabled. Although in very much the same method as an operating system, many of these services can be disabled to reduce the attack service and exploitable operations that run in the hypervisor tier. Compromising the hypervisor can almost be a declaration of a complete overrun of an attacker, potentially being able to read memory, CPU instructions and control machines. If an attacker were to get into this layer, it would be a serious matter.

### Memory Introspection

Vendors now offer the ability to introspect the running memory of a virtual machine. Monitoring this active memory provides a greater level of protection from more sophisticated level of attacks. Visit Citrix Ready to [learn more about hypervisor memory introspection](https://citrixready.citrix.com/bitdefender/bitdefender-hypervisor-introspection.htm).

## Operations Layer

The operational procedures in building a secure environment are as crucial as the technical. The following items provide a good start in building a strong foundation of operational security.

### User Training

Providing training to users and administrators to promote a good security practice and awareness is one of the easiest to cover. Ensuring that users are aware of what is normal behavior of a login page, what details support staff will and will not request, how to handle pop-ups and of course email phishing attempts. Users should be aware of how to respond to and understand where to report security related issues to security operational centers.

### User Monitoring

Enabling enhanced user monitoring such as user session recording can sometimes be controversial. If the user has either been notified in their contract of employment or as a separately issued document that their actions on the IT systems may be recorded for auditing purposes, provides a good level of cover in terms or Human Resourcing. More controversially, there are possibilities to enable key logging, again; these kinds of tools should be approached with some level of caution, as potentially depending on what the users are accessing from their corporate machines, the administrators could be collecting user names and passwords to personal email accounts, and even bank accounts. This does need to be approached with caution and again, the users notified that such actions are being undertaken.

### Logging and Auditing

Ensuring logging of all actions of both IT administrators and users are logged throughout the environment, this includes all the layers described within this article. Then being able to collate and store these logs for auditable purposes to review in the case of a breach; this does then of course require a team to continually manage and act upon the logs that are generated throughout the system. Some applications do now enable a level of automation of these logs and alerts to be processed for a team to act.

Citrix does offer a cloud service as part of the analytics service that can in the event of a risk scoring of a user increasing, disconnect, or lock the account. These events can then enable elements such as session recording automatically as discussed in this article, disable Citrix features and even, disconnect the session until an administrator has reviewed the actions of that user. This can correlate user behavioral analytics and applied security controls in an automated fashion. You can find more information in [Citrix Analytics tech brief](/en-us/tech-zone/learn/tech-briefs/analytics.html).

### Segregation of Duties

While ensuring good role-based access control mechanisms are implemented, it is an approach to ensure that no single administrator could act alone to turn on a feature or egress point. Ideally implementing separation of duties forces two administrators to act independently to enable a feature. A good example would be enabling Client Drive Mapping into a Citrix session. This could be controlled using Citrix policy to be controlled by the Citrix administrators, and then enabling in Citrix ADC from a SmartControl policy. Therefore, requiring two separate changes to enable CDM.

As much as it is important to have two administrators to potential enable a functionality, administrators should have separated accounts from their normal user account for performing administrative changes. This then reduces the attack vector of phishing attacks, if an administrator were to have their normal user credentials hived up, and these credentials had domain administrator permissions, there could be a significant impact on the environment; vs the administrator losing their normal user account that is unable to carry out an administrative tasks. As an additional note, users should not have global admin rights across a network or domain. Ideally the use of role-based access controls (RBAC) groups are used to provide permission administrative based functions at a granule level.

### Change Management

One of the common things that customers do not consider is the importance of test and development environments. These environments are crucial in being able to thoroughly test updates, changes and how they could potentially impact the user environments. These environments should be closely interlinked with change control processes to encourage administrators to make changes in test/development environment before moving into production. As part of testing the installation or upgrade of changes in development environments, it is equally important to thoroughly test the roll back of changes then the administrators are familiar with the process should then need to invoke the roll back plan within production. This thorough testing and roll out plans protect the production environment from unnecessary outages. Ideally a through route to live would relate to the following:

-  **Test**. An environment loosely controlled and a more sandpit area for admins to get to grips with the software and any new features.
-  **Pre-Production**. Pre-Production should be treated in a similar manner to production, closely protected by change control and kept in lock step with production. This provides a strong reassurance that the behavior of upgrades within this environment are equal to that of production, just on a smaller scale.
-  **Production**. Production goes without saying, it's production. No unauthorized changes without Change Control.

### Software Hashes

It is good practice when downloading software from vendor websites to validate the hash provided on the downloads page. This would verify that the file downloaded has not been tampered with by any 3rd party adversary.

### Security Audits

Security operations is a wide encompassing topic and includes many elements including user documentation, legal documentation, sign off of any environment including many more. As much as the technical elements of any infrastructure is crucial. It is also important to keep in mind that the supporting documentation, legalities, and IT Health Checks leading to sign off various uses or holding of data such as Personal Identifiable Information (PII) or Payment Card Industry (PCI) is as crucial. Ensuring you have regular security audits and penetration tests to validate your security operations and controls.
