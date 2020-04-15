---
layout: doc
description: Copy & paste description from TOC here
---
# Architectural Considerations for the General Data Protection Regulation (GDPR)

## Contributors

**Authors:** [Matthew Brooks](URL), [Florin Lazurca](URL),  [Rob Sanders](URL), [Martin Zugec](URL), [Allen Furmanski](URL)

## General Data Protection Regulation (GDPR) Overview

GDPR is a set of data privacy rules that apply broadly to both companies in the European Union (EU) and the usage of data pertaining to EU residents. The GDPR went into full effect on the 25th of May, 2018 across the EU. The official regulation includes several
chapters which are further broken down into “articles”, or subsections that describe specific provisions, which are referenced frequently by number or title in documents describing how to comply with their requirements.

### GDPR seeks to:

* Unify European data protection regulations
* Protect the data privacy of EU residents
* Manage how organizations approach data privacy

**Note:** Personal data is any information relating to an identifiable person, therefore it is often referred to as personally identifiable
information (PII). It could include information such as names, photographs, IP or email addresses, and medical information.

### The GDPR applies to:

* Organizations operating within the EU
* Organizations operating outside of the EU who offer goods, services or are involved in ‘monitoring the behavior’ of EU residents

**Note:** Privacy considerations for GDPR span the data lifecycle from opt-in collection, to specific or anonymized usage and secured disposal and retirement of data. It covers all personal data relating to your customers, employees, supply chain, partners and anyone else about whom you collect personal information.

### Two key GDPR article will be the focus of this document

* Access control (Article 25) - calls out particular measures which shall ensure that by default
personal data is not made accessible without the individual’s intervention to an indefinite number of natural persons.
* Encryption and data protection (Article 32) - calls out:
  * Pseudonymisation and encryption of personal data
  * Ensure the ongoing confidentiality, integrity, availability and resilience of processing systems and services
  * The ability to restore the availability and access to personal data in a timelymanner in the event of a physical or technical incident
  * Protection from accidental and unlawful destruction, loss, alteration, unauthorized disclosure of, or access to personal data transmitted, stored or otherwise processed

### How Citrix can help you with your GDPR compliance initiatives

Citrix Workspace simplifies the management of your systems and data by centralizing
services in the data center or cloud as a digital workspace. The goal of this white paper is to
describe how it unifies applications, data and desktops into a digital workspace for your teams and allows you to better align with GDPR requirements around data management, data monitoring and information auditing.

#### Citrix supports clients on their journey to GDPR compliance in 4 key ways:

* By centraling and enclaving applications and data
* By ensuring data is protected when shared or distributed
* By controlling who has access to data and resources
* By bringing IT together for application and data-specific security

### Citrix Workspace - supporting your GDPR journey

(overall architecture diagram. needs update for naming and to include Workspace Service)

## Data oriented approach to GDPR requirements

Following the GDPR guidelines might be much easier for modern cloud companies than traditional enterprises. While most cloud companies have only a few centralized sources where personally identifiable information (PII) is stored, traditional companies potentially have hundreds, if not thousands, of different databases and data sources that need to be
assessed, reviewed and updated to meet the latest data privacy standards.

These data sources can range from traditional SQL databases to emails, digital documents or even physical documents. With today's often aggressive timelines, many enterprises face a challenge to properly prepare and update systems as required. It is important to understand that the GDPR doesn’t affect only active data sources, but also all backups, disaster recovery sites and physical printouts.

The GDPR is all about the maturity of the company when dealing with data and privacy information. Citrix has always been a data and
application oriented company, with a proven record of handling complex, often international projects that are dealing with thousands of applications.

The traditional consulting approach is focused on the business processes, identifying people
and business requirements, access methods and slowly cascading down to infrastructure
and data sources. However, the GDPR requires a more data centric approach. We recommend
starting with identifying and assessing various locations where PII data is currently stored
and moving higher up the stack to make sure that data sources are properly secured. You can think about this as an inside out approach to security.

(Define, Assess, Reduce, Remediate, Review flow diagram)

**Define** - Start by defining the criteria of PII data that is in scope of assessment. This phase should help you define what to look for and how to prioritize data sources from a privacy perspective. This should include employees, customers, vendors, and any other relevant entities.

**Assess** - Analyze all the existing locations where data is stored. Identify the business requirements, data retention and potential challenges to securing the data. Identify not only where the data is stored, but also how it’s being collected. Data segmentation is one of the most time consuming and critical phases of data consolidation projects. This phase requires a comprehensive approach, critical thinking and well-defined methodology. Organizations mustn't ignore data held in legacy systems, even if there is a program in place to modernize soon or that the data is used only as a backup. It is important to understand that all these legacy systems are covered by the GDPR requirements as well and companies need to take a holistic approach.

**Reduce** - The goal of this phase is to identify if it is possible to reduce the number of data sources that need to be secured. For example, it is possible to consolidate data sources that are being used by business units – instead of storing client data in multiple locations, a centralized location could be used to maximize effectiveness of security measures. Maybe the data is not even needed at all – the biggest privacy offender might not even be considered critical for the business units. It is also possible that applications are simply collecting too much data (“just in case”). The applications can be modified to stop collecting excessive information and the existing data can be erased. Instead of trying to secure all the possible locations of PII, companies should ask when/where do they actually need to store the data about customers and other parties. As GDPR compliancy is an ongoing process with periodic reviews, minimizing the amount of included data sources can prove to be a very effective long-term strategy.

**Remediate** - Identify if existing data sources and applications used to access them are following the GDPR guidance, or if changes are needed. If the data source includes PII data and is not secure, identify the possible approaches to solve the situation. A cross departmental GDPR team should also identify, assess, and review not only the data itself, but also access methods, applications used and
other factors, such as limiting user and 3rd party access, revisiting requirements and more specifically defining data security measures.

**Review** - GDPR compliancy is an ongoing process and data assessments need to be performed on a regular basis. It is therefore important to implement a robust, stable, and repeatable process that can be defended if it ever needs to be presented to auditors. The data sources security assessment should be performed and reviewed on a regular basis.

With the large number of data sources that are in scope, the goal for most companies should be to choose
a few, robust, and proven architectures that can help secure data sources that don’t initially meet GDPR requirements. Trying
to create a tailor-made solution for each of the problematic data sources is unrealistic, unless a company has limited data sources. The result is often an environment where only a few applications are properly secured, while the majority are left unsecured, with the implementation project being stalled by months or even years and going well over budget. GDPR also presents an opportunity to update privacy architectures across applications and data usage to support evolving global and regional privacy initiatives.

Complexity is considered one of the biggest enemies of security. You want to identify the minimum number of different architectures
that could be used to secure the majority of the data sources that have been identified as critical and are storing data included in the GDPR scope.

The Pareto principle (also known as 80/20 rule) is important during this data assessment–and companies should try to minimize the effort required to secure the majority of data sources. Most enterprises have hundreds or thousands of different applications and data sources that are being used and they need to promptly identify those applications that contain critical data and don’t meet the GDPR requirements. Automated application assessment solutions can reduce the time required to analyze applications.

Many companies plan to use this mitigation period to transition to a more flexible IT model. While this goal is plausible, it is important to understand the timelines and choose realistic goals. Customers should choose solutions that can be gradually improved without the need for a complete redesign.

In the following few sections, we will present you with a few selected architectures that can provide a universal, secure, and proven solution to secure any type of data, ranging from web-based applications, through legacy client/server applications hosted on Windows or Linux to data stored in various documents or exchanged through emails.

(GDPR reference architecture flow chart)

## Securing Windows and Linux Applications

Trying to secure traditional client/server applications, whether they are running on Windows or Linux operating systems, can prove challenging for various reasons.

The traditional approach was to secure each endpoint where these applications are installed. This involves management challenges, such as keeping all the endpoints up to date, encryption of the network traffic, data and workload encryption, implementing multi-factor authentication (MFA) and encryption of the locally stored or cached data. In traditional IT architecture, defenses need to be set up around all endpoints, applications and networks and the whole environment is as secure as the weakest point. This traditional approach to security has often failed due to the introduction of new concepts including mobile workforce, expansion of security perimeter
with cloud computing and BYOD initiatives.

Another common challenge for applications installed on traditional computers is to provide the same security functionality across the whole portfolio. It’s common to have a multigenerational IT portfolio residing on a single workstation, from Office-based applications (using Microsoft Access databases or custom plugins) through legacy Visual Basic to the latest professionally built applications. Making sure that applications with access to sensitive data support encryption, multifactor authentication and provide enough information for auditors has always been complicated.

(traditional client/server app delivery diagram)

Citrix has a long tradition of providing a platform for the secure delivery of these client/server applications. This secure delivery is based on offloading the client application piece onto a dedicated set of servers (Citrix Virtual Apps and Desktops), specially designed, optimized, and secured for application delivery. By decoupling the application from the endpoint, it's possible to enable additional security features. The advantage of this approach is that security features are applied consistently, without requiring any changes or access to the source code and even to applications that are no longer actively supported.

### Securing Applications to GDPR Standards

#### Article 25 - Access to Personal Data
There are multiple ways to limit or prevent users from accessing published resources. The most basic method is to simply hide the applications or desktops from users by enforcing Active Directory group membership. When publishing resources through the Citrix management console, the access is enforced on the Citrix Virtual Delivery Agent (VDA) machines hosting the workloads. Access and availabile functionality is further tweaked through Citrix's comprehensive policy engine.

The use of traditional username/password authentication is decreasing with more secure multi-factor authentication (MFA) increasing. Even for internal networks, more and more companies are enforcing MFA requirements to enhance security. With Citrix Virtual Apps and Desktops and Citrix ADC, MFA can be applied to any client/server application including even legacy applications that are hard to maintain. The ADC appliance provides an extensible and flexible approach to configuring MFA, from time-based one-time tokens, through smart cards, user or machine certificates to biometric authentication (through third party integration).

This access can also be configured based on various other factors – for example the endpoint a user is connecting from, the security state of endpoints such as antivirus or firewall requirements or the network where the user is connecting from. Context-aware policies can be applied, even enforcing a specific geo-location or using more advanced security measures, such as requirements for user and/or machine certificates to access certain resources. You can learn more about context-aware security through the zero-trust model in the following [blog post](https://www.citrix.com/blogs/2019/10/02/approaching-a-zero-trust-security-model-with-citrix-workspace-part-1/).

Even more flexibility is available through Citrix ADC’s enhanced MFA feature call nFactor authentication. To learn more about different
capabilities of nFactor authentication, refer to the following knowledge base article and one of the many deployment guides: [https://support.citrix.com/article/CTX201949](https://support.citrix.com/article/CTX201949)

The ability to deliver centralized access and authentication is critical in providing information about users connecting to
applications. With Citrix Virtual Apps and Desktops, all access to resources is brokered through a controller with historical data saved in a centralized database. This data can be accessed from an ODATA API to provide integration with SIEM systems or provide copies for auditors if needed. To learn more about monitoring and reporting, refer to the doc on how to [Monitor historical trends across a Site](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops/director/site-analytics/trends.html).





(Link to include in links section https://www.citrix.com/blogs/2019/04/29/citrix-tips-top-10-findings-from-citrix-environment-security-assessments/)
