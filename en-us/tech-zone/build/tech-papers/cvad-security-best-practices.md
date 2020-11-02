---
layout: doc
h3InToc: true
contributedBy: Andy Mills, Patrick Coble, Martin Zugec
specialThanksTo: First Last, First2 Last2
description: Copy & paste description from TOC here
---
# Security best practices for Citrix Virtual Apps and Desktops

TODO: Disclaimer

## Introduction

In the following article, we aim to cover off the main components to help secure a Citrix Virtual Apps and Desktops Infrastructure. To provide further context, the information is provided in the traditional Citrix methodology used by professional services as shown below:

![Layered Approach](/en-us/tech-zone/build/media/tech-papers_cvad-security-best-practices_001.png)
_Citrix Consulting Layered Approach_

As shown in the diagram, security does not have its own layer. Any security process or functionality is intertwined with all the layers and is crucial that its covered throughout an infrastructure, including the processes surrounding it.

First, one of the key factors as to what security controls are implemented into a Citrix environment comes down to risk. What are the chances of something happening and what would be the implications if the 'something that happened' did happen? Implications of a breach can, of course, vary from financial loses, data breaches, data leakage, brand loyalty and the worst-case scenario; loss of life. Hopefully, with any of the above risk outcomes, business sponsors and risk managers have carried out some level of risk analysis of the assets involved and therefore this provides some quantifiable value to the asset the business is protecting. Now given this value then depends on how much effort and of course expenditure is invested in protecting such assets. Ideally, the security controls in place do not outprice the asset that is to be protected; from this one can understand what suitable security controls can be implemented into an infrastructure. If the control outweighs the cost of the asset; then this is ultimately a waste of time and cost on security products or controls. We include time here as with some controls the time invested in operational procedures can increase, and usually with the increase of operational procedures comes at employing more support personnel to run the environment.

Many security controls can be designed into an environment in the early phases of a project, however; it is not to be forgotten that security risks are constantly evolving and revisiting the controls and procedures in place is a continual service improvement process and needs to be revisited frequently.

These types of factors are understood when designing a new solution. In this article, we aim to cover off the varying levels of controls that can be implemented to secure a Citrix environment, some controls are recommended as standards to be deployed across all environments regardless of risk, whereas some are very specific and require a little more thought and rationale before integrating into the solution.

## User and Device Layer

When a business is considering how to enable their end users to connect to virtual desktops, varying schemes such as Bring Your Own Device (BYOD) or corporate issued devices are available; both come with their operational overhead. Implementation of such initiatives may change many factors; including decisions around what features can be offloaded onto the endpoint. A user connecting from a BYOD device may have no access to clipboard, client drive mapping (CDM) or printing. Whereas a user accessing from a corporate owned device where additional controls and 'trusted' build procedures have been carried out, clipboard and local drive access may be permitted. There is no 'one size fits all' when it comes to endpoint devices. End point clients need to be selected dependent upon the end user use case.

The following may be additional requirements or lockdowns to be considered when considering the endpoints:

Device Lockdown

The endpoint device that a user consumes has potential to be used as an attack vector; including such attacks as keyloggers or points of ingress into the network. One major benefit instantly when running Citrix technologies like Virtual Apps and Desktops, the ingress point is significantly reduced with only mouse, keyboard and screen refreshes being a necessity for a user to carry out their work. If, however, users require additional functionality such as accessing local client drives, printing facilities and clipboard functionality these can be enabled on a case by case basis. As capabilities increase Citrix does have controls to reduce the risk of these attack vectors such as anti-keylogging and disabling any points of ingress/egress.

Device lockdown is more than just what functionality a user may have when roaming between devices, as many users may have several. Understanding what level of risk each endpoint device poses may adjust the above functionality. But, there are such configurations outside of Citrix that can make a difference.

-  Do users require local administrative permissions on the device?
-  What other software is running on the endpoint?
-  Does the user have a VPN capability?
-  Who updates the device in terms of operating system and application patching?
-  What resources can the endpoint access?
-  Who has access to the endpoint itself?
-  What from the endpoint could a potential infection spread to?

These are all questions for the IT support teams to consider when deploying devices to the end users.

### Endpoint Logs

Even if a user can or cannot install software of any kind on the endpoint, ensuring logs are enabled and centrally captured so that they cannot be intercepted assists in understanding what may have occurred in a breach or providing to the forensic analyst for further review. Logging is a crucial element to any environment security monitoring process. There is a trade-off though, first it's easy to turn on all the logging and have no process or system to process the data, second; you can have too many logs to trawl through and try and decipher. This comes back to enabling just enough logs to fit within any response process or logging system to be able to decipher and respond to a potential alert. It may also be beneficial to forward all logs to your security information and event management (SIEM) and then set alerts to notify you known attack methods or other key events based on your logging strategy.

### Thin Client terminals

In some scenarios thin client devices are perfect to be deployed into any environment and of course high-risk environments. Thin clients run a cut down version of operating systems only having enough of an operating system and application to connect to a Citrix session. These have other benefits such as patching, there are limited applications that need to be patched and maintained, and usually the operating systems are reduced footprint, it only takes a few minutes to update the devices. Due to the devices having a simplified and purpose build operating system, there is limited data at rest on the endpoint allowing the business to deliver business critical applications to endpoints that may have a different security posture on the network with a level of assurance. For thin client terminals that run a full operating system, the use of write filters to stop the persistence of data are useful. Resetting the terminals when reboot occurs reduces the chance of an attacker persisting data on the endpoint that could be used to formulate an attack. Thin Client terminals have the added benefit of usually being cheaper to purchase and maintain than traditional desktops or laptops.

### Citrix Workspace App and HTML5

Both receivers enable users to connect to published desktops and applications. There is a difference between the two receiver types as to the footprint on the endpoint client. HTML5 receiver in recent releases has increased in functionality with additional features such as copy and paste; local printing. The plus side to this is that all

### Patching

As part of this article patching has already been mentioned, patch management should be one of the first considerations to any endpoint. How does the business get security fixes applied to the endpoints? In traditional environments machines have patches pushed to them through Windows Services Update Service (WSUS), System Center Configuration Manager (SCCM) or through other automated services. This is great if the endpoint is managed by the corporate IT teams. What about BYOD, who patches those and ensure they are up to date. With work from home growing quickly endpoint patching becomes more complicated to ensure the endpoints are reporting in on a regular basis along with possible scheduling changes based on the adjusted hours. This is where clear responsibilities of the users are called out when such schemes are undertaken to ensure that users know how to patch and update their systems. Simple notifications on the login page suggesting a new update has been released and they should patch as soon as possible is one method. Or, to place Endpoint Analysis Scans (EPA) on the endpoint to not allow them to access the infrastructure unless they are running up to date software. Both provide the answer in asking users to upgrade, but one certainly forces the user to update especially if they want to access the data.

As much as its important to keep the operating system patched and up to date, it is equally as important to ensure that the Citrix Workspace App is patched and up to date on the endpoint client as Citrix will incorporate not only feature enhancements, but also security fixes into releases.

### Identity Creation/decommissioning

Through the user and device layer there has been much focus on the device security element. How user accounts are created to allow users to Authenticate and Authorise them to resources needs to be centrally managed and efficient. Ideally linking such tools into Human Resource (HR) systems to automate the creation and removal of accounts eases this process. Many times, accounts are 'copied' from similar roles; as much as this does ease things it also creates creep in terms of permissions and unauthorised group additions in the central directory. Copying a user's permissions from a previous account could lead to the unauthorised access to data. Ideally any group request or access should be authorised by the data owner before being permitted to gain access.

Of course, with the creation of any identity comes the decommissioning of it, if user provisioning tools are integrated with HR systems then this could be relatively simple. There are times though when business need to flex resources in and out from 3rd parties including vendors, additional support for busy periods or when outsourcing elements of the business. How do those accounts get decommissioned and ultimately deleted? Ideally the business has a procedure for both onboarding and offboarding any contracting resource with end dates set to at least disable the account and moved into an 'holding OU' within Active Directory, then deleted when the resource is confirmed as no longer being required. Ultimately, we do not want these contractors having the ability to log in months after they have left the business, or alternatively reenabling of an account when the user has been offboarded without the relevant approvals.

## Access Layer

Within the Citrix design process, the Access layer is where users authenticate, the necessary policies are applied and if configured; dynamic contextual based access is evaluated. Therefore, ensuring that the access tier is designed with a strong level of security in mind.

In the following article, we aim to cover the main tasks to help further secure a Citrix ADC deployment. There are usually two major deployment types for Citrix ADCs, it is used as a proxy for Citrix Virtual Apps and Desktop deployments and/or load balancing other applications to make them more highly available and secure. Most of these resources are typically very sensitive too and their overall security should be a high priority. These items in this checklist will help lower your risk and exposure for items that the Citrix ADC is interacting with.

### Control Physical Access

Citrix ADC appliances must be deployed in a secure location with sufficient physical access control to protect them from unauthorized access. This applies to physical models and their COM ports to virtual models on virtualization hosts and their associated ILO\DRAC connections. Find more information in [Citrix ADC product documentation - physical security best practices](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#physical-security-best-practices).

### Change Default Passwords

Having any admin default standard password that is known by the IT community is a very high risk. There are scanners that will search for default credentials being used on a network and their effectiveness can drastically be lowered and/or eliminated by just changing the default password. We recommend that all service account passwords should be stored in a secure location like a password manager. The secure storage of all service account passwords is a foundational information security standard. We also recommend that each password for each role and deployment (HA Pair) be unique and to not reuse credentials if possible. Find more information in [Citrix ADC product documentation - Change the default passwords](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#other-network-security-considerations).

### NSROOT

We recommend changing the default as soon as you start the configuration process of the device. This account allows full administrative access and protecting this password should be the number one priority when deploying this system. If you are deploying a new Citrix ADC system in 2020 it will have the Serial Number of the appliance as the default password.

-  We also recommend creating another superuser that will be used in emergency situations when external authentication is down instead of the default nsroot account. Find more information in [Citrix ADC product documentation - Create an alternative superuser account](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#administration-and-management)
-  SDX users must also create an admin profile to configure the default admin password for each VPX instance provisioned. We recommend creating a different Admin Profile for each Instance so each HA pair will have the same password but will be unique to the other instances. Find more information in [knowledge base article CTX215678](https://support.citrix.com/article/CTX215678)
-  **Lights on Management (LOM)** –  common in many physical Citrix ADC platforms. This account allows access to the command line of the appliances along with power control of the device. These also have default passwords that should be changed upon initial configuration to help prevent unauthorized remote access.
-  **Key-Encryption-Key (KEK) Password** – This is an optional configuration that will encrypt your configuration for sensitive password areas locally on each appliance. This is covered in just a couple sections below Hyperlink Here
-  **SVM Admin Account** – This account and password change will only apply to Citrix ADC SDX platforms. This will have the default nsroot password and will not use the serial number like physical appliances. This should be changed upon its initial configuration to help prevent unauthorized remote access.

### Secure NSIP traffic from all network access

Every device in your network should not be able to manage your Citrix ADC appliances. Your management IP addresses should be in a VLAN that you can control ingress and egress to ensure only authorized privileged workstations or other authorized networks can access them to manage the devices. Many of the vulnerabilities that have been found over the past 14 years have been related to having access to the NSIPs and by securing these IPs in a controlled manner you can eliminate or drastically lower this attack vector. When limiting access to the management of any IT system it must be documented thoroughly and also have business continuity planning completed to understand device management impacts if your planning IPs or Subnets are down and how you can recover if other systems are still operable.

-  You must also ensure that any Subnet IPs (SNIPs) and Mapped IPs (MIPs) should not have management enabled on them. This could lead to your DMZ network having access to the management console via this SNIP or MIP because it was unintentionally checked.
-  To check just browse your SNIP or MIP via HTTP and HTTPS. Or within the console navigate to System – Network – IPs and select the IP and you will see if the management check box is selected also.
-  You can also use Access Control Lists within the Citrix ADC to limit access to the management of the appliance to specific hosts or subnets without the management being placed behind a firewall. You can find more information in [CTX228148 - How to Lock Down Citrix ADC Management Interfaces with ACLs](https://support.citrix.com/article/CTX228148) and [Citrix ADC product documentation - Network security](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#network-security).
-  If your Citrix ADCs will be placed behind a firewall you will need to plan based on the services the appliance is delivering along with access to the management to each device along with any supporting maintenance or security devices like Citrix ADM and Citrix Analytics. You can find more information in [CTX101810 - Ports used by Citrix Services](https://support.citrix.com/article/CTX101810) and [CTX113250 - Sample DMZ Configuration](https://support.citrix.com/article/CTX113250).
-  In higher security deployments to also change the management ports from the default HTTP (TCP80) and HTTPS (TCP443) to a custom port to create obfuscation and help prevent identification from typical scans. This must be done on each appliance. You can find more information in [Citrix ADC product documentation](https://docs.citrix.com/en-us/citrix-adc/current-release/system/basic-operations.html#how-to-configure-http-and-https-management-ports-for-internal-services).
-  Limiting Egress (outbound) and Ingress (inbound) from the management networks that the Citrix ADC will be deployed onto. Having unlimited and monitored network access from any device to anywhere is an inherent risk. Limiting external exposure to external assets is a great step outside of even the Citrix ADC control plane. There should also be limitations of what can access these management resources so that only specified devices or ranges can manage your Citrix ADCs.
-  It is a common practice to keep all the NISIPs, LOMs and SVMs on a separate network from other networking equipment. Depending on your policy standards you may put the LOMs on physical/out-of-band management networks with other similar systems and the NSIPs and SVMs on another network. It can also be helpful to have a subnet for them to make a standard

### Bind Citrix ADC to LDAPs

Using generic login accounts for day-to-day administration post setup is not recommended. When configuration changes are done by nsroot in an IT team of 20+ people who have access to the password manager, you will not be able to easily track who logged in and made the change. We recommend once the nsroot accounts password has been changed that the system is immediately bound to LDAPS to track usage to a specific user account. This will also allow you to have better delegated control so you can create View only groups for auditing and application teams and allow full admin rights to specific AD groups also. We do not recommend binding using unencrypted LDAP as credentials could be collected with packet captures. You can find more information in [CTX212422](https://support.citrix.com/article/CTX212422).

If binding to Windows Active Directory we recommend ensuring you are only using LDAPs as the protocol. It is also important are ensuring NTLMv2 is the only hashing method for credentials and even network sessions too. This is an AD security best practice to ensure that credentials are as protected as possible while in transit for authentication requests and network sessions. These should not be turned out without proper testing and knowing how old your Windows clients are and how compliant their patches are as there can be incompatibilities to older versions of Windows.

When binding to any external authentication source you should either disable local authentication for local accounts like "nsroot" or only enable specific local accounts to use local authentication. Depending on your deployment requirements may require one path or another. Most deployments will not have requirement to have an account that must use local authentication because if there is a service account needed in most cases you can just create and delegate a LDAP user. [This guide](https://www.citrix.com/blogs/2020/09/23/how-to-secure-restrict-citrix-adc-management-access/) covers setting up both methods, one method should be deployed.

### Logging and Alerting

Having the Syslog logs forwarded is critical to be able to provide incident response along with more advanced troubleshooting. These logs will allow you see login attempts to the ADC system and to the Citrix Gateway if used, actions taken by policies such as GeoIP & BadIP blocking, AppQoE Protection, Responder Policy Actions and other packet and system operations. This information can be invaluable during troubleshooting issues along with understanding your current attack situation and to be able to react to either with actionable data to fix problems or stop\mitigate an attack. Without a Syslog target configured on your Citrix ADC you can get in a situation where there logs you need for a troubleshooting or security investigation is not possible because the logs were rolled off to save space for the system to stay operational. You can find more information in [Citrix ADC product documentation - Logging and monitoring](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#logging-and-monitoring).

There are many Syslog servers\collectors available that have licenses that are free or paid subscriptions. Many of these systems will be priced by the number of messages per day, storage space of messages per day or some are just a continuous subscription. These solutions should be planned as they will require a sizable amount of storage space to be allocated based on the number and size of the events each day. Your Syslog server\collector should also be sized based on the number of events from other critical services like Active Directory, Database and File Servers, VDAs, Desktops and other application Servers too.

Citrix offers the Citrix Application Delivery Management service as a Syslog collector which can allow you to have some off-device retention. The Citrix ADM is also a great tool to be able to search and view these logs and also use its events dashboards for many great default views of this log data to help you troubleshoot and view hardware failures, authentication failures, configuration changes and many others. You can find more information in Citrix ADM product documentation - [Configuring syslog on instances](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/setting-up/configuring-syslog-on-instances.html) and [View and Export syslog messages](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/networks/events/how-to-export-syslog-messages.html).

We recommend using this list of messages to create your alerts based on what features of the Citrix ADC are in use. Just having the logs retained and searchable will be very valuable to troubleshooting and auditing security events. It is important to ensure you are alerting based on threshold for bad logins along with any logins using the default nsroot accounts. You can find more information in [Developer Docs - Syslog Message Reference](https://developer-docs.citrix.com/projects/citrix-adc-syslog-message-reference/en/latest/).

SNMP should also be configured to your monitoring solution to ensure you have service and physical that type of monitoring enabled to ensure services and/or the appliance are up.

Configuring Email can be very help to allow for system notifications to be sent out like expiring certificates and other system items to your team.

### Plan Services to Monitor and Protect

Planning what services you will be using on your Citrix ADC will help you understand what features may be applied to those IP addresses. The main three items that the Citrix ADC are commonly used for are. This is a good point to also know if you will have IPs assigned to test. It is highly recommended that you validate all the other security settings on a Test VIP before applying the responder and other policies to your Production Items. This could require more DNS records, SSL Certificates, IP Address and other configurations to make it work.

-  Load Balancing (to include SSL Offload)
-  Global Server Load Balancing
-  Citrix Gateway

We recommend organizing all the planned VIPs by Internal and External because we may not need many of the security features on the internal assets, but they may be a must have for all external assets. Evaluating which countries may need to access external resources is a good planning step if you plan on using the GeoIP features below. Working with the business leadership and applications owners are a great place to find out if you could restrict countries.

If you can we recommend deploying Citrix ADM before you start making configuration changes based on its ability to back up the appliances on a regular basis along with some better log visibility along with some other metrics. There are other more advanced features that can be added to a Citrix ADC deployment to provide even more visibility and security. Citrix ADM has paid editions along with Citrix Analytics that can go beyond just a monitoring solution to an automatic security response system.

### Replace Default SSL Certificates

When default certificates are left on anything managed or access over the web all modern browsers will warn you that the certificate is invalid and could be a security risk. If you are accepting this error every day you could be accepting a true man-in-the-middle attack where someone could be seeing what your typing because they are in the TLS stream. Replacing the Certificate can be very easy depending on if you host your own Internal Certificate Authority, use a third-party service (possible cost associated) or self-signed certificates. Once you have done this your browser should trust this certificate and should not give you any errors while accessing the device, and if it ever does again you should check the expiration and check to see if it has been replaced without your knowledge.

-  **NSIP** – Changing this certificate should be done first, as it is where all management for the appliance will happen. This process will need to be completed on each appliance. You can find more information in [CTX122521](https://support.citrix.com/article/CTX122521).
-  **LOM** – Changing these certificates should be the next goal once the NSIP management Certificates has been changed from default. This process will need to be completed on each appliance. You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/installing-a-certificate-key-on-lom-gui.html).
-  **SDX SVM** – If you are an SDX customer then you will want to ensure you also that this certificate has been replaced as this will be your primary method to manage the appliances along with managing the instances themselves. This process will need to be completed on each appliance. You can find more information in [CTX200284](https://support.citrix.com/article/CTX200284).

### Firmware Update

1.  **Initial Deployment** - Use the latest released version for each Citrix ADC platform and the chosen Firmware Build after reviewing the release notes and to see if there are any "known issues" that may prevent you from being able to deploy. In many deployments they use the N-1 methodology to always be just one version behind from the latest released to help ensure there are no major known issues and that the version has been deployed at other customers in that time period also. Firmware updates will be done on each instance no matter which platform you are using like CPX, MPX, SDX or VPX.
1.  **LOM\IPMI Firmware Updates** - We recommend also ensuring that this is updated at the time of deployment to ensure you will have the latest features and fixes. You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-hardware-platforms/mpx/netscaler-mpx-lights-out-management-port-lom/upgrading-the-lom-firmware.html).
1.  **Ongoing Updates** - [Sign up for alerts](https://support.citrix.com/user/alerts) for Citrix Security Bulletins and Update notifications for the Citrix ADC, Citrix ADM and Citrix Web Application Firewall.

Plan on how often and when you will update your Citrix ADCs each year. Depending on the features deployed and your IT organization policies will determine how you may schedule these updates. We typically see updates happening 2-4 times per year with two updates being feature updates and two updates for known issues they may be experiencing and security issues. There are guides that will help you plan and schedule these upgrades in a way to ensure no service interruption. This policy should also go to other supporting systems if in use like Citrix ADM too as keeping version parity is always recommended. Learn more about [upgrading a high availability pair](https://docs.citrix.com/en-us/citrix-adc/current-release/upgrade-downgrade-citrix-adc-appliance/upgrade-downgrade-ha-pair.html).

### Data Protection with KEK

We recommend enabling further data protection on each appliance pair by creating a KEK key pair. This will encrypt the configuration in key password related areas so that if someone or something gained access to your ns.conf file they would not be able to read those areas of the configuration related to passwords. The two key files that will be at the root of the \flash\nsconfig\ folder should be classified as sensitive and protected accordingly with backups and secure storage of them.

This also means that migrations from one appliance to another appliance will require more steps as the KEK key must be added to the new deployment before those items will work again as those items of the configuration are encrypted. [Learn more](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html) about creating a master key for data protection (search for string `Create the system master key for data protection`).

### Backup

TODO - We recommend deploying Citrix ADM appliance to users.

### SSL/TLS Ciphers

Validate TLS Ciphers and SSL Score for all External VIPs (Citrix Gateway). There have been many vulnerabilities in the SSL and TLS protocols over the past couple years and ensuring you are using all the TLS best practices as they are ever changing. We know we want to ensure older protocols are disabled like SSLv3 and TLS 1.0 and TLS 1.1. Read [Citrix Networking SSL/TLS Best Practices](https://docs.citrix.com/en-us/tech-zone/build/tech-papers/networking-tls-best-practices.html) for latest recommendations.

Choosing between a Wildcard or SAN certificate is another aspect of this process. Wildcards can be very cost effective, but also can be more complicated based on the number of hosts that will be impacted with expiration and deployments. A SAN certificate can effectively be a restricted wildcard for just 2-5 sites, along with the ability to use more than one TLD in the same certificate too depending on the provider.

### Configure Bot Network Protection

Citrix Bot Management is feature that is similar to IP Reputation services, but it's focused on bots which are becoming more and more advanced and malicious over the past 10 years. This feature will look through a allow and block list, then through IP reputation, rate limiting, bot signatures and device fingerprinting checks that are done within this solution. Configuring this solution can help consolidate the configuration of many of these features like BadIP, AppQoE into the same configuration. You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-adc/current-release/bot-management.html).

### Configure GeoIP

This feature can allow you to prevent unnecessary countries from accessing resources behind your Citrix ADCs. Depending on where users needed access to these resources you can lower your possible attack surface by thousands to billions of IPs from being able to connect to those resources. There are many companies that only do business with users in certain specific countries and it can be possible to block certain countries. This should be planned to help prevent GeoIP restrictions affecting traveling employees, in some cases you may need to put users into groups that could bypass this default behavior for this IP. It is important to check with your network team because in some cases they may be doing some blocking at the firewall level and you could get conflicting results if both are in play and using different IP definitions or rules. You can find more information in [CTX130701](https://support.citrix.com/article/CTX130701).

### Configure IP Reputation (BadIP)

IP reputation is a feature that will look at source IP addresses against a database which is hosted by Webroot to see if it comes from known malicious sources and block them based on policies. This is recommended for external facing sites that are behind the Citrix ADCs. In most cases all these categories could be enabled without impact based on most clients should not be coming from these type of source IP addresses.

Types of traffic sources that are blocked

-  Virus Infected personal computers
-  Centrally managed and automated botnet
-  Compromised webserver
-  Windows Exploits
-  Known spammers and hackers
-  Mass e-mail marketing campaigns
-  Phishing Proxies
-  Anonymous proxies

You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-adc/current-release/reputation/ip-reputation.html).

### Configure Application level Quality of Experience (AppQoE)

This feature will limit the amount of throughput to resources behind a Citrix ADC to help prevent Denial-of-Service attacks. This will help also ensure the experience for the application stays as close to the normal even under an attack. With the even increasing amount of DoS attacks and even Distributed DoS attacks have been on the continual rise and the attacks have been worse as the throughput of the internet connections have went up along with the number of devices have grown too that can be used as a platform for the attacks. In many cases your internet uplink is a fractional amount to the network uplink of your Citrix ADC appliance. Most corporate internet speeds are around 2Gbps and with most physical appliances coming with on average 10Gbps there is no way a VIP should need more than 2Gbps and may be more appropriate at 200Mbps based on its actual usage with some overhead.

We recommend looking at the metrics for the VIP in scope to understand what limits you may want to set. We recommend also adding some overhead for growth to these metrics to ensure there is enough throughput.

Another important note overall is that the more features that are enabled the less throughput and processing power you may have. Watch the utilization of their uplinks, CPU and Memory as you enable more and more features and put the Citrix ADC in a more central role to more applications.

You can find more information in [CTX126853](https://support.citrix.com/article/CTX126853).

### Ensure Drop Invalid Packets is Enabled

There are many invalid packets that are sent every day to your Citrix ADC device and some are benign, but most are used for fingerprinting purposes along with protocol-based attacks. Turning this feature on will save on CPU and Memory on your device by not sending the partial packet to the backend application that is being proxied by the Citrix ADC. By dropping these types of packets it will prevent many protocol attacks known today and some that will be discovered in the future that are passed on packet manipulation which is deemed invalid.

You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-adc/citrix-adc-secure-deployment/secure-deployment-guide.html#applications-and-services), [CTX227979](https://support.citrix.com/article/CTX227979) and [CTX121149](https://support.citrix.com/article/CTX121149).

### Ensure HSTS Is configured on all SSL VIPs

HTTP Strict Transport Security main goal is to protect applications by various attack methods like Downgrade attacks, Cookie Hijacking and SSL Strip. This is similar to Drop Invalid Packets but is based on HTTP\HTTPS based standards found in RFC 6797 by using an entry into the HTTP header. This will add yet another layer of defense to applications behind the Citrix ADC.

### Citrix Gateway Configure MFA is possible

This highly recommend for at least all external connections to any internal system. With the number of usernames and passwords that are searchable by attackers from breaches and leaks over 12.3 billion records it continually increases the risks of a breach into you company as those records grow. With password reuse is so common with almost everyone in the world, your business is at risk based on your user's password habits. A user name and password should not be your only defense to your applications, and we should not inherently trust people will just two pieces of information that is globally available to sensitive business information and applications. Adding Multi-factor authentication to all applications may not be feasible with the user's workflow, but we do recommend deploying wherever possible especially for unrestricted source external access. This layer of defense will not stop all attacks as social engineering attacks could convince the user to give a person their one-time code or accept the push notification for testing purposes as an example. What multi-factor authentication gains you is more time for detection, and it will require the attacker to be more advanced and persistent.

You can find more information in [product documentation](https://docs.citrix.com/en-us/citrix-adc/current-release/aaa-tm/authentication-methods/multi-factor-nfactor-authentication.html).

### ICA encryption

It has been a long since standard for enabling RC5 – 128bit as the security encryption for the ICA stream. This does provide some level of security on the ICA stream, however; for those who are looking to gain that extra level of encryption on the ICA stream between either the endpoint and the VDA, or between the ADC and the VDA, enabling VDA SSL is recommended. This binds a certificate to the Desktop service encrypting the ICA stream to the given standard of the certificate bound. For more information on how to carry out this process, please review: [https://support.citrix.com/article/CTX220062](https://support.citrix.com/article/CTX220062)

## Resource Layer

The resources that host the user session themselves can present an additional level of risk to compromise. A user running a VDI session is as good as having a computer on their desk connected to the corporate network. Utilising a well-designed access and control layers allows the business to move towards a zero-trust models; dynamically adjusting access to the resources presented to the end user dependent upon a given set of variables. The following guidance will provide additional levels of control in protecting the corporate assets from users.

### Build Hardening

Hardening operating system builds can be very complex and difficult to achieve, with the trade-off of user experience, usability and security all being a fine balance. Many customers usually follow the Center for Internet Security (CIS) baselines for hardening virtual machines across the varying roles. Microsoft also provide hardening guides for similar workloads and even the ADMX files to implement directly into group policy. Proceed with caution and make sure to test thoroughly first, as these initial policies may be overly restrictive, and disable required features for other software to run. If features are genuinely required and justified to be enabled for functionality, then a relaxation of policy may be considered acceptable from the security teams to allow features to be enabled and any user execution of such software or feature is logged. These baselines are a starting point for hardening and are not meant to be exhaustive for all scenarios. The key to lock down is to test thoroughly and encourage 3rd party penetration testing engagements to validate your security controls and their effectiveness against the latest attack methods.

### Workload separation

Placing resources into separate silos has long been a recommend practice, from not only a resource layer but also a hardware and networking layers which is covered off later in this article. Separating workloads into various delivery groups or machine catalogs will not only help fine tune security policies to a set of machines, but also reduce the impact of a security breach. If you have a set of high-risk users accessing high impact data, then these should be separated off into a separate environment with much stricter policies and logging applied, other capabilities such as session recording may provide additional layer of assurance when using are accessing this data.

As part of ensuring that the users are separated into varying risk profiles, the data they access must also be treat equally. This would involve in correctly applying the correct file permissions and access rules to data.

### Application Control

Ask any support engineer when it comes to implementing application control technology. Technologies such as AppLocker or IVANTI can be awkward and hard to implement. Being able to granularly lockdown to the executable level what can or cannot run and by whom is beneficial; not to mention the logging capabilities when applications are attempted to be launched. Given this granularity, a large enterprise environment multiple of 100's if not 1000's of applications why planning the implementation of them is crucial. Application control technologies at this level need to be carefully considered in terms of operational overhead as without proper planning and testing you could cause impact to your users' workflow.

### Windows Policies

Windows policies when applied to a session host whether it be a VDI based workload or server based, play two major roles:

-  Hardening the operating system
-  Optimising the user experience

To ease the management of operating systems these should be applied through Microsoft Group Policy which eases the image creation process. However, some hardening and optimisations may need to be required during the build process and these should be automated along with the full build process.

Ensuring that the operating system is hardened prior to users gaining access limits the unauthorised execution of processes or applications. Users should only be able to carry out the minimal number of tasks that are required to perform their role, any administrative based applications should be locked down and access disabled for users. This reduces the risk of a user being able to 'break out' out their session and potentially either gaining access to unauthorised data or perform malicious acts on the operating system.

As part of hardening the system, it is recommended that administrators spend some time optimising the underlying operating system, services and scheduled tasks to remove any unnecessary processes from the underlying system. This improves the responsiveness of the session host and provides an improved user experience to the end user. Citrix provides the optimizer tool [Citrix Optimizer](https://support.citrix.com/article/CTX224676) that optimises many elements of the operating system automatically for administrators. It is recommended to review what the Citrix optimiser is going to adjust to ensure there is no negative impact within your environment.

### Citrix Policies

Policies are applied and enforce behaviours dependent upon the user scenario. Typical Citrix session evaluations include, turning off clipboard access or client drive mapping if this is deemed necessary; or even enabling one-way clipboard to allow data to be copied into a session, but not out of a session. There are several policies that can assist with controlling unauthorised egress of data from the system session; and each need to be carefully planned and understood.

Many of the hardening configurations that were discussed under system hardening of this article can be applied in group policies. Allowing for a consistent application across all servers that are applied at boot up before the user logs on. This will allow for a consistent user experience across the infrastructure; which allows IT administrators to troubleshoot issues too along with ensuring security controls are consistent between all users using the system unless they are have been excluded from some controls based on their job role.

To provide enough time to allow policies and other lockdowns are applied to the session, configuration to the delivery groups to allow the VDA to settle and to wait before allowing users to logon, this ensures that all the policies have applied to the server before the user logs in. (SettlementPeriodBeforeUse - [https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/latest/Broker/Set-BrokerDesktopGroup/](https://developer-docs.citrix.com/projects/citrix-virtual-apps-desktops-sdk/en/latest/Broker/Set-BrokerDesktopGroup/))

### Image Management

Proper implementation of Image management for many customers is a significant time saver for customers. Those of you who are familiar with the Citrix products will know that the two offerings are Machine Creation Services (MCS) and Provisioning Services (PVS), if you want to learn more on how these technologies function please review the following article: [https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/image-management.html](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/image-management.html). Image management not only provides great benefit to operational procedures, but also security ones. First, a lockdown or control is applied in a build consistently across all the machines that users will consume their resources from. Manually building individual machines is not only time consuming but it's almost a guarantee that configurations and settings will be missed; set once and roll out to all the machines provides peace of mind that a security implementation will be consistent across all the machines. Secondly, during the lifetime of a user session on a VDA, the user may open other applications, documents and access sensitive data which is ultimately cached on the virtual desktop or application, once the user is then logged remnants of data will undoubtedly be left behind, therefore after a session is logged off rebooting the machine and reverting back to the original golden master image provides a level of reassurance that any sensitive data is wiped clean ready for the next user to login and commence work without the risk of accessing the previous users data.

### Session Recording

Session recording is an interesting topic and may bring some overhead in terms of process and user trust, putting those aside for a second; recording the actions of a user within a session especially when accessing sensitive data or potentially doing something untoward gives no doubt that something in that session was happening by a user logged onto that workstation. This ability may not be needed for all users, but may should be enabled for key individuals, user groups or when accessing sensitive applications, desktops or resources. There are many things that could be gained from these recordings that may not be possible with just Windows event and application logs which can be very helpful in an incident response scenario. This feature is very powerful and should be carefully considered based on your user policies and agreements with your legal and IT team's approval.

### App Protection Policies

Recently Citrix have released additional controls that can be implemented to provide additional level of security control when users are accessing the environment. App Control policies that include anti-key logging and anti-screen capture provide reassurance that should attempts be made to egress data during sessions that some level of due diligence has been carried out the IT teams to deter users from doing this. What about a camera I hear you ask? Well, Citrix offer watermarking functionality which doesn't stop an individual taking the picture, but it does watermark the session with key details so that the screen shot can be traced back to its origin. Although it may not entirely stop someone from taking a picture, at least it can be traced back to the source of egress.

### Operating System

A more contentious topic has been the use of multi-session hosts vs single user VDI. Having multiple user's logon to a single server can cause issues, especially if a disgruntled user has been able to execute software or code to commence hiving up other credentials or browsing into other user directories there is a security risk. If a 1:1 mapping of user to desktop is achieved, then this risk is completely mitigated. Running solid virtual desktop infrastructures does come at an additional cost. Again, threat modelling our users and selecting the most efficient delivery mechanism in terms of a multiuser session host or single user session host can be chosen. One size does not fit all users, finding a trade-off of cost and security is key.

### Microsoft Virtualisation Based Security (VBS)

Microsoft VBS essentially uses a secure portion of memory to store secure assets from the session. This feature requires a Trusted Platform Module (TPM) or Virtual Trusted Platform Module (vTPM) running on a supported platform to provide the secure integration. This essential means that even if malware were successfully deployed into the kernel, the user stored secrets remain protected. Code that can run in the secured environment requires to be signed and approved by Microsoft to execute providing an additional layer of control. Microsoft VBS can be enabled in both Windows Desktop and Server operating systems. Microsoft VBS is built up from a suite of technologies including credential guard and application guard. For more information please review the following URL: [https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-vbs](https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-vbs)

## Control Layer

The control tier is the aspect of the solution that allows administrators to administer the Citrix solution, along with permit user access to resources. Other key elements of enabling resources to communicate with one another also occurs in this section. Due to the integration of this layer, ensuring that components can securely integrate and communicate is key. The following will reduce the security posture of the components:

### Build Hardening

As previously discussed in the Resource Layer, it is recommended to harden the operating system and services that are deployed. This involves disabling any unused services, scheduled tasks or features that aren't used to reduce the attack service on the machine. There are baselines such as the CIS and Microsoft security baselines that can be used to lockdown virtual machines running in the environment; including both the session hosts hosting the user sessions but also the control services such as cloud connectors and supporting infrastructure. An infrastructure machine should equally have any services, features or scheduled tasks disabled that are not required for the service to operation.

### Service Account Hardening

Some elements of a Citrix solution require the use of service accounts, service accounts allow for automated functions to progress with some level of authentication and authorisation. A service account should only be permissioned to carry out the task that is required and most certainly should not have any elevated access on the network. Ideally a service accounts are created for each automated function to narrow down the authorisation elements and ensure that no privilege creep occurs on the service. We recommend ensuring service account passwords are reset at least yearly or earlier based on your compliance requirements. These accounts and or groups should also be in Protected Users within Active Directory for extra protection and logging.

### Least Privilege

As discussed, this framework is applied to any account on the network. This ensures that any account that is created only has the necessary permissions to perform the role. When administrative access is required, users should have separate accounts that are used for performing administrative functions. In an ideal scenario, permissions to the accounts to perform an action should only be permitted for the time in which the task is being performed. Once the action on administrative tasks has been completed, any permissions that are no longer required should be removed.

### Application Control

Application controls has been covered in the Resource Layer, however; a similar approach can be completed for the control tier. Permitting the executables specific to an application further reduces the attack surface on running machines. Any executables that have not been authorised will not be permitted to run. This does come at an administrative overhead, but is an extra layer of control that can be applied to the applications that have been deployed.

The main problem with this approach is that any update that changes files or executables running on the system need to be re-approved to allow them to run post the update.

### Host Based Firewall

One of the most common and probably the first thing that most administrators disable is the host-based firewall. As much as this may be ok for certain scenarios such as troubleshooting. Leaving such services disabled permanently does leave the environment at high risk of compromise. The firewall is designed to stop attackers from gaining access to the server through unknown or spurious tools or applications. Ensuring that the firewall is configured to stop any unauthorised elements from running, it is needed to be configured to allow applications to function and communicate to one another. When the Citrix VDA Installs it will automatically make the rules needed for basic VDA communications, but it also can add Windows Remote Assistance and the required RealTime Audio ports also. Environments such as test environments are required to allow administrators to understand how the application functions to correctly configure firewalls and services correctly within the production environments without negative affect to the application or user experience. For ports required to allow Citrix environments to function, please review: [https://support.citrix.com/article/CTX101810](https://support.citrix.com/article/CTX101810)

### Transport Layer Security

Transport Layer Security (TLS) encrypts the communications between two or more components. During the authentication, enumeration and launching of a session requires the transfer of sensitive information. If these communicates aren't encrypted an attacker could potentially retrieve user credentials or other sensitive data. When hardening environments enabling TLS is one of the first things that should always be carried out. Although part of enabling TLS, following recommended practices in terms of creation of the private keys. This is not to be delved into within this article as there are many articles around key management and creation of certificate recommended practices. TLS should be enabled for the following communications as a minimum:

-  STA Service
-  XML Brokering
-  StoreFront (Base URL)
-  ADC Management interface
-  Gateway
-  Director if running on-premise

## Host Layer

The hardware layer in virtual environments compromises of components such as storage, compute, hypervisor and networking. Continuing with designing and deploying these components with security as the focus will continue the reduction of attack surface. As with cloud services, not all these will be applicable, but others apply regardless of where the resources are located.

### Hardware separation

In the cloud era this has slowly being forgotten about and more and more business and companies are looking for a greater return on investment by ensuring that all hardware is full utilised. In cloud provider examples, this is something which is not even possible. However, in on-premise infrastructures separating up resources onto separate physical hosting infrastructure. Such attacks as hyper jacking where the attacker can drill down through a VM and the virtual machine tools running the OS through into the hypervisor. Although many of the documented attacks are theoretical, the mere fact its documented suggests that there may be a method if such an attack could be effective - or one day it may be possible. Given this, and again, it comes back to the data you are protecting; separating workloads into separate clusters and ensuring that workloads hosting the same data classification are retained within their own clusters. Therefore, if an attacker were to break out into the hypervisor layer, that higher classifications of data is not compromised.

### Network Separation

Breaking down workloads into subnets that are logically separated can further reduce the spread of an attack. Usually the following subnets layouts are a good start:

-  **Access Components.** Small subnet compromising of the ADC IP addresses and call-back gateway.
-  **Citrix Infrastructure.** The Citrix infrastructure subnet depending on the infrastructure being deployed would include the following; StoreFront, Cloud Connectors/Controllers, Director servers and Application Delivery Manager (ADM).
-  **Supporting Infrastructure.** Depending on what additional infrastructure components are required, the following services are prime examples of being separated; including SQL, jump servers and licensing server. This all depends on what level of environment separation is required and mandated.
-  **VDA Subnets.** I haven't found a right or wrong answer on the VDA subnet sizing. History used to have guidance around PVS subnet sizing, but again; as time as gone on and PVS recommended practices have evolved somewhat. The main thing here is that subnet sizing is suitable to the number of users / VDA's and of course the security context that is being accessed. Logically placing similar risk profile of users in a single subnet does seem logical, ensuring that each subnet is then separated by a firewall.

### Firewalls

Firewalls are one of the main elements to implementing security in an environment. Implementing host based and network-based firewalls do introduce an amount of operational overhead. Implementing two levels of firewalls from both a host based, and network level does allow for separation of duties to permit an application to communicate from server A to server B. Ideally any firewall rules should be documented and clearly marked as to what their role and function is to ease approvals from the security and network teams.

### Hypervisor Hardening

For a hypervisor to function a certain level of services and process do need to be enabled. Although in very much the same method as an operating system, many of these services can be disabled to reduce the attack service and exploitable operations that run in the hypervisor tier. Compromising the hypervisor can almost be a declaration of a complete overrun of an attacker, potentially being able to read memory, CPU instructions and control machines. If an attacker were to get into this layer, it would be a serious matter.

### Memory Introspection

Vendors now offer the ability to introspect the running memory of a virtual machine. Monitoring this active memory provides a greater level of protection from more sophisticated level of attacks. Please review the following blog for more information: [https://www.citrix.com/blogs/2018/04/11/citrix-bitdefender-prevent-another-zero-day-vulnerability-with-hypervisor-introspection/](https://www.citrix.com/blogs/2018/04/11/citrix-bitdefender-prevent-another-zero-day-vulnerability-with-hypervisor-introspection/)

## Operations Layer

The operational procedures in building a secure environment are as crucial as the technical. The following items provide a good start in building a strong foundation of operational security.

### User Training

Providing training to users and administrators to promote a good security practice and awareness is one of the easiest to cover. Ensuring that users are aware of what is normal behaviour of a login page, what details support staff will and will not request, how to handle pop-ups and of course email phishing attempts. Users should be aware of how to respond to and understand where to report security related issues to security operational centres.

### User Monitoring

Enabling enhanced user monitoring such as user session recording can sometimes be controversial. If the user has either been notified in their contract of employment or as a separately issued document that their actions on the IT systems may be recorded for auditing purposes, provides a good level of cover in terms or Human Resourcing. More controversially, there are possibilities to enable key logging, again; these kinds of tools should be approached with some level of caution, as potentially depending on what the users are accessing from their corporate machines, the administrators could be collecting usernames and passwords to personal email accounts, and even bank accounts. This does need to be approached with caution and again, the users notified that such actions are being undertaken.

### Logging and Auditing

Ensuring logging of all actions of both IT administrators and users are logged throughout the environment, this includes all the layers described within this article. Then being able to collate and store these logs for auditable purposes to review in the case of a breach; this does then of course require a team to continually manage and act upon the logs that are generated throughout the system. Some applications do now enable a level of automation of these logs and alerts to be processed for a team to act.

### Segregation of Duties

Whilst ensuring good role-based access control mechanisms are implemented, it is an approach to ensure that no single administrator could act alone to turn on a feature or egress point. Ideally implementing separation of duties forces two administrators to act independently to enable a feature. A good example would be enabling Client Drive Mapping into a Citrix session. This could be controlled using Citrix policy to be controlled by the Citrix administrators, and then enabling in Citrix ADC from a Smart Control policy. Therefore, requiring two separate changes to enable CDM.

As much as it is important to have two administrators to potential enable a functionality, administrators should have separated accounts from their normal user account for performing administrative changes. This then reduces the attack vector of phishing attacks, if an administrator were to have their normal user credentials hived up, and these credentials had domain administrator permissions, there could be a significant impact on the environment; vs the administrator losing their normal user account that is unable to carry out an administrative tasks. As an additional note, users should not have global admin rights across a network or domain; ideally the use of role-based access controls (RBAC) groups are used to provide permission administrative based functions at a granule level.

### Route to Live

One of the common things that customers do not consider is the importance of test and development environments. These environments are crucial in being able to thoroughly test updates, changes and how they could potentially impact the user environments. These environments should be closely interlinked with change control processes to encourage administrators to make changes in test/development environment before moving into production. As part of testing the installation or upgrade of changes in development environments, it is equally as important to thoroughly test the roll back of changes then the administrators are familiar with the process should then need to invoke the roll back plan within production. This thorough testing and roll out plans protect the production environment from unnecessary outages. Ideally a through route to live would relate to the following:

-  **Test**. An environment loosely controlled and a more sandpit area for admins to get to grips with the software and any new features.
-  **Pre-Production**. Pre-Production should be treated in a very similar manner to production, closely protected by change control and kept in lock step with production. This provides a strong reassurance that the behaviour of upgrades within this environment will be equal to that of production albeit on a smaller scale.
-  **Production**. Production goes without saying, its production. No unauthorised changes without Change Control.

### Windows Patching

Ensuring IT systems are patched and up to date is a standard practice for all software, operating systems and hypervisors. Providers are obliged to ensure that their software or application is patched if any security vulnerabilities are identified along with other stability and feature enhancements. Nearly all vendors have a release schedule or cycle for applying patches and updates to their software. It is recommended that customers read and understand what is being patched, or features introduced. These updates are then processed through route-to-live solutions; this then protects the production environment from outages that could have been avoided through testing.

### Anti-malware

Anti-malware or antivirus is always recommended to be deployed on all servers throughout the infrastructure. Although can potentially be bypassed; antivirus does provide a good first line of defence against known malware, viruses and many other types of viruses. One of the more complex aspects of Antivirus is ensuring that the virus definitions are updated regularly, especially in non-persistent VDI workloads. There are many articles on redirecting antivirus definitions to persistent drives and ensuring that the machines are identified as individual objects in the antivirus management suite. These all need to be followed to ensure that Antivirus is deployed and installed correctly. Antivirus vendors may also have their own recommended practices for deploying their antimalware software, it is recommended to follow their guidelines for correct integration.

### Physical security

Not much is going to be provided on physical security within this document, as it very much depends on the data that is being protected, building location, access, shared office spaces and then extending to datacentre physical security. It is recommended that the guidance from the security teams are followed and acted upon. Citrix does recommend ensuring a physical security plan and protections is in place for your servers if self-hosted and then even your endpoints. Physical access to any computing device can eventually lead to compromise.

### FIPS compliance

The Federal Information Processing Standard (FIPS) provides a level of assurance that the cryptographic functions or modules. FIPS is mainly deployed in government entities as the classification of the data and the encryption surrounding the transfer of the data needs to be accredited and assured. FIPS requirements are usually set out very early in a project, and then the necessary appliances either virtual or physical can be designed and catered for.

### Security Insights

Citrix does offer a cloud service as part of the analytics service that can in the event of a risk scoring of a user increasing, disconnect or lock the account. These events can then enable elements such as session recording automatically as discussed in this article, disable Citrix features and even, disconnect the session until an administrator has reviewed the actions of that user. This can correlate user behavioural analytics and applied security controls in an automated fashion.

### Cloud Security

Many cloud security providers have service offerings that will increase security. Although the responsibility of what is enabled and how application access is firmly in the responsibility of the customer, these service offerings do provide an increased level of security controls when running infrastructures in the public cloud. It is recommended that customers closely engage the cloud providers ensuring that the right security features are enabled to ensure that the data is protected as much as possible.

### Software Hashes

It is good practice when downloading software from vendor websites to validate the hash provided on the downloads page. This would verify that the file downloaded has not been tampered with by any 3rd party adversary.

### Security Operations

Security Operations is a wide encompassing topic and includes many elements including user documentation, legal documentation, sign off of any environment including many more. As much as the technical elements of any infrastructure is crucial. it is also important to keep in mind that the supporting documentation, legalities and IT Health Checks leading to sign off various uses or holding of data such as Personal Identifiable Information (PII) or Payment Card Industry (PCI) is just as crucial. Ensuring you have regular security audits and penetration tests to validate your security operations and controls.
