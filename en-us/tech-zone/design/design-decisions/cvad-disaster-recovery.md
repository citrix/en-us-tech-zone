---
layout: doc
---
# Citrix Virtual Apps and Desktops - Disaster Recovery Planning

## Contributors

**Author:** [Michael Shuster](mailto:michael.shuster@citrix.com)

**Special thanks:** [Kevin Nardone](https://www.linkedin.com/in/kevinrnardone/), [Martin Zugec](https://twitter.com/MartinZugec)

## Overview

This guide assists with business continuity (BC) and disaster recovery (DR) architecture planning and considerations for both on-prem and cloud deployments of Citrix Virtual Apps and Desktops. DR is a significant topic in breadth of scope in and of itself. Citrix acknowledges this document is not a comprehensive guide to overall DR strategy; it may not consider all aspects of DR and at times may take a more layman’s terms perspective on various DR concepts.

This guide is intended to address the following considerations which have significant impact on an organization’s Citrix architecture and provide architectural guidance related to them:

-  Business Requirements
-  Disaster Recovery vs. High Availability
-  Disaster Recovery Tier Classifications
-  Disaster Recovery Options
-  Disaster Recovery in the Cloud
-  Operation Considerations

## Business Requirements

Aligning to the “Business Layer” of Citrix design methodology, gathering clear business requirements and known constraints for recovery of service should be the starting point in the development of any recovery plan for Citrix. This helps gain clarity on scope and provide direction on the DR strategy most suitable to meet the business and functional requirements, and constraints.

Useful questions to have answered should include but are not limited to the following. These are discussed in more detail in the [Disaster Recovery Options](#disaster-recovery-options) section in terms of how they affect a Citrix DR design:

-  **Backup Strategy and Recovery Time Objective (RTO).** What backup strategy is used today for servers? Backup frequency? Retention period? Offsite storage? Tested? Does Citrix need to be immediately available in event of a disaster or brought online within a specific time period? (See Disaster Recovery Tier Classifications). It is worth including application back-ends which Citrix-hosted apps connect to into the discussion to align expectations.
-  **Recovery Point Objective (RPO).** For RPO what degree of data loss is considered tolerable for DR, which can vary depending on the infrastructure component or data classification? How old can the recovered data for the service be? 0 minutes? An hour? A month? Within context of Citrix infrastructure, this may apply only to database changes and user data (profiles, folder redirection, etc.) As with RTO, it is worth considering the application back-ends for Citrix-hosted apps into the discussion.
-  **Scope of Recovery.** This does not include just the Citrix infrastructure, but can include user data or key application servers which Citrix-hosted application clients interface with. It is important to identify if there is going to be a disparity between time to recover Citrix, and time to recover applications hosted on Citrix. This can prolong an outage as only part of the solution is online.
-  **Use Cases.** Citrix platforms often service numerous use cases, each with different levels of business criticality. Does recovery cover all Citrix use cases? Or just key use cases upon which the business’s operational success is fully dependent on. This has a large impact on infrastructure scoping and capacity projections. Segmentation and prioritization of use cases is recommended to compartmentalize the enablement of DR if it is a net new capability for the Citrix environment.  
-  **Capabilities.** Are there any key capabilities which may not need to be included in DR which can reduce cost? An example of this might be use of persistent desktops vs. non-persistent; some VDI use cases which could be served by Hosted Shared Desktops, or even specific application solos. Customers have at times indicated component redundancy (ADCs, Controllers, StoreFront, SQL, etc.) are not considered necessary in DR. This should be viewed as a risk if there is a prolonged outage of production.
-  **Existing DR.** Does an existing DR strategy or plan exist for other Citrix environments and other infrastructure services? Does it recover into new routable subnets or into a “bubble” mirroring production networks? This may help give visibility to current DR approaches, tools, or precedence.
-  **Technology Capabilities.** This may not dictate the final recovery strategy for Citrix, however it is important to understand:
    -  **Recovery:** What storage replication technologies, VM recovery technologies (VMware SRM, Veeam, Zerto, Azure Site Recovery (ASR), etc.) are available within the organization? Some Citrix components alongside Citrix dependencies may need to leverage them.
    -  **Citrix:** What Citrix technologies are in use for image management and access? Some components may not lend themselves well to certain recovery strategies. Non-persistent environments managed by MCS or PVS make for poor candidates for VM replication technologies due to their tight integration with storage and networking.
-  **Data Criticality.** Are user profiles or user data deemed critical to recovery? Persistent VDIs? If this data is unavailable when DR is invoked, would this cause significant impacts to productivity? Or could a non-persistent profile or VDI be used as a temporary solution? This decision may drive up costs and solutions complexity.
-  **Types of Disasters.** This is fairly significant but also may be pre-established through a corporate BC or DR plan. This requirement will often dictate recovery location. Is the strategy intended to accommodate for recovery of service at the application level only? Between hardware racks? Within a metro location? Between two geographies? Two countries? Within a cloud provider region or an entire cloud provider’s infrastructure (multi-region)?
-  **Client Users.** Where are users located who access the services in production? This can have implications as to where service is restored to, including corporate network connectivity which may require manual modifications during a DR event. In addition, this dictates access tier considerations.
-  **Network Bandwidth.** How much traffic (ICA, application, file services) does each user session consume on average? This applies both to cloud and on-prem recovery.
-  **Fallback (or Failback).** Does the organization have pre-existing plans for how to return to service in production once the DR situation has been resolved? How does the organization recover to a normal state? How is new data that may have been created in DR reconciled and consolidated with production?

Constraints limit BC/DR design options or their configurations. They come in many forms but may include:

-  **Regulatory or Corporate Policy.** This may dictate what technologies can or cannot be used for replication or recovery, how they are used, RTO, or minimum distance between facilities.
-  **Infrastructure.** Is there is directive on type of hardware to be used, network bandwidth available, etc.? This may impact RPO considerations or may even present risks. For example, undersized firewalls or network pipes in DR can ultimately lead to outages as network dependencies may not handle the DR workload. Or, in case of compute, undersized hypervisor hosts or use of different hypervisors entirely may impact options. With regards to networking, if the recovery site happens to have more constrained network throughput capability versus production when servicing the WAN. In cloud environments, especially when scaling into thousands of seats, this can quickly become a significant bottleneck due to component service limitations such as virtual firewalls, VPN gateways, and cloud-to-WAN uplinks (AWS Direct Connect, Azure ExpressRoute, Google Cloud Interconnect, etc.).
-  **Budget.** DR solutions come with costs that can conflict with project budgets. More often than not, the shorter the RTO, the higher the cost.
-  **Geography.** If a designated DR facility has been identified, considerations such as latency from production to DR facilities, as well as users connecting to DR should be understood.
-  **Data residency or Data Classification.** This may limit options in terms of locales and technologies or recovery methods.
-  **Cloud Recovery.** Is there a mandate for infrastructure to be recovered in cloud vs. in an on-prem facility?
-  **Application Readiness.** Do the applications hosted on Citrix that have back-end dependencies have a BC/DR plan and how do the RTOs line up to Citrix’s target RTO? Citrix can be designed for rapid recovery but if core applications do not have a recovery plan or one with an extended RTO, this will likely impact usefulness of the Citrix platform in DR.
-  **Network Security.** Does the organization have security policies that might dictate what traffic segments require encryption in transit? Does this vary depending on the network link being traversed? This could require in DR scenarios the use of SSL VDA, ICA Proxy, GRE, or IPsec VPN encapsulation of ICA traffic if network flows for DR are different from production.

## Misconceptions on Disaster Recovery Design

One of the most common misconceptions for Citrix DR designs, which will be referenced to throughout this article, is the notion that a single control plane spanning two data centers constitutes DR. It does not. A single Citrix Site or PVS Farm spanning data centers, even ones in close proximity, constitutes a high availability design, but not a DR design. Some customers choose this path as a business decision valuing simplified management over DR capability. StoreFront Server Groups are an exception to this example as spanning data centers is [unsupported](/en-us/storefront/current-release/plan.html#scalability).

The following diagram is an example of a multi-data center HA Citrix architecture that is not however, a DR platform due to the previously mentioned rationale. This reference architecture is used by customers who treat two physically separate data center facilities as a single logical data center due to their close proximity to each other and low latency, high bandwidth interconnectivity. A DR plan and/or environment for Citrix would still be recommended as this architecture would be unlikely to satisfy the needs of either DR or HA.

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_001.png)

In contrast, the following diagram illustrates unsupported or ill-advised component architectures spanning geographically distant data centers or cloud regions. These provide neither effective HA nor DR due to the distance and latency between components. Platform stability issues may also result due to latency concerns in such a deployment. Furthermore, the stretching of administrative boundaries between sites does not align to DR principals. We have seen similar conceptual designs by customers in the past.

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_002.png)

Another common misconception is the depth of consideration that a Citrix  DR solution should include. Citrix Virtual Apps and Desktops at its core is primarily a  presentation and delivery platform. The Citrix control plane depends on other components (networking, SQL, Active Directory, DNS, RDS CALs, DHCP, etc.) whose own availability and recovery design meet the DR requirements of the Citrix platform. Any shared  administrative  domain of technologies on which Citrix is dependent can impact the efficacy of the Citrix DR solution.

Of similar importance and occasionally not fully accounted for by customers are recovery considerations for file services (user data and business data on shares, etc.) and application back-ends with which Citrix-hosted applications and desktops interface. Referencing the earlier point on application readiness, a Citrix DR platform can be designed to recover in short timespans. However, if these core use case dependencies do not possess a recovery plan with RTO similar to Citrix or no recovery plan at all, this can have the effect of Citrix successfully failing over in DR as expected but users being unable to perform their job functions as these dependencies remain unavailable. Take for instance a hospital who hosts their core EMR application on Citrix. A DR event occurs and Citrix fails over to an “always on” Citrix platform in DR and users are connecting, but the EMR application is recovered via more manual means which may take hours or days to complete. Clinical staff would likely be forced to use offline processes (pen and paper) during this time. This may not be congruent to the overall expectation of the business for recovery time or user experience.

The next section will discuss DR vs. HA in more detail.

## Disaster Recovery (DR) vs. High Availability (HA)

Understanding HA vs. DR is critical in aligning to organizational needs and meeting recovery objectives. HA is not synonymous with DR, but DR can leverage HA. This guide interprets HA and DR as follows:

-  HA is generally regarded as providing fault tolerance for a service. A service can fail over to another system with minimal disruption to a user. This could be as simple as a clustered or load balanced application to a more complex active or standby “hot” or “always on” infrastructure that mirrors the production configurations and is always available. Failover tends to be automated in these configurations and may also be termed a “geo-redundant” deployment.
-  DR on the other hand, is concerned primarily with the recovery of service (usually due to significant app-level failure or catastrophic physical failure of the data center) to an alternate data center (or cloud platform). This tends to involve automated or manual recovery processes. Steps and procedures are documented to orchestrate recovery. It is not concerned with redundancy or fault tolerance of the service and is generally a broader scope strategy that withstands multiple types of failures.

While HA tends to be embedded into design specifications and solutions deployment, DR is largely concerned with the orchestration planning of personnel and infrastructure resources to invoke recovery of the service.

Can HA include DR? Yes. For enterprise mission-critical IT services, this is common. Take for instance the second example in the HA description above, coupled with appropriate recovery of other non-Citrix components related to the solution, would be regarded as a highly available DR solution wherein a service (Citrix) fails over to an opposing data center. That particular architecture could be implemented as Active-Passive or Active-Active in various multi-site iterations such as Active-Passive for all users, Active-Active with users preferring one data center over another, or Active-Active with users being load balanced between two data centers without preference. It is important to note that when designing such solutions, standby capacity must be considered, accounted for, and load monitored on an ongoing basis to ensure capacity remains available to accommodate DR, should it be required.

It is also essential that DR components be kept up-to-date with production to maintain integrity of the solution. This is often overlooked by customers who design and deploy such a solution with the best of intentions, then start consuming additional platform resources in production, and forget to increase available capacity to retain the DR integrity of the solution.

It should be noted that in the context of Citrix, spanning Citrix administrative domains (Citrix Site, PVS Farm, etc.) between two data centers such as two geo-localized facilities as per [published guidance](/en-us/storefront/current-release/plan.html#scalability). would not constitute DR and for some components such as StoreFront Server Groups is unsupported. As the components share dependencies such as databases, this would not protect against several key failure scenarios. If databases became corrupt the failure domain would impact application services at both facilities. To regard an HA Citrix solution as being sufficient for DR, we recommend the second facility does not share key dependencies or administrative boundaries. E.g., create separate Sites, Farms, and Server Groups for each data center in the solution.

## Disaster Recovery Tier Classifications

Tier classifications for DR are an important aspect of an organizations DR strategy as it provides clarity into application or service criticality which in turn dictates the RTO (and thus costs for accomplishing that level of recovery). Generally, the shorter the RTO, higher the DR solution cost. Being able to break down various inter-dependencies into different classifications (based on business criticality and RTO) can help optimize cost-sensitive DR cases.

Below is a set of sample DR tier classifications to serve as a reference when assessing the Citrix infrastructure services, its dependencies, and critical applications or use cases (and associated to VDAs) hosted on Citrix. DR tiers are outlined in order or recovery priority with 0 being the most critical. Organizations are encouraged to leverage or develop a DR tiering classification aligning with their own recovery objectives and classification needs.

### Disaster Recovery Tier 0

#### Tier 0 - Recovery Objectives (Examples)

Recovery Time Objective: 0

Recovery Point Objective: 0

#### Tier 0 - Classification Examples

Core IT Infrastructure

-  Network and security infrastructure
-  Network links
-  Hypervisors and dependencies (compute, storage)

Core IT Services

-  Active Directory
-  DNS
-  DHCP
-  File Services
-  RDS Licensing
-  Critical End User Services (Citrix)

#### Tier 0 - Notes

This classification is largely for core infrastructure components. These are always available in the DR location as they are dependencies for other tiers, and not in an isolated network segment. They need to be provisioned and maintained alongside their production equivalents. If Citrix is hosting mission critical applications, it’s likely regarded as a Tier 0 platform. In such scenarios, Citrix infrastructure should be deployed “hot” in DR.

### Disaster Recovery Tier 1

#### Tier 1 - Recovery Objectives (Examples)

Recovery Time Objective: 4 Hours

Recovery Point Objective: 15 Minutes

#### Tier 1 - Classification Examples

Critical Applications

-  Websites and web apps
-  ERPs and CRM applications
-  Line-of-business applications

Citrix Use Cases

-  Management
-  Customer Service or Sales
-  IT Engineering & Support

#### Tier 1 - Notes

Applications or virtual desktops upon which the business depends to carry on core business activities would typically be contained in this tier. They would likely also employ a similar “hot standby” DR architecture as Tier 0 or benefit from an automated replication and failover solution. If provisioning into cloud, considerations [discussed later](#disaster-recovery-in-cloud), need to be accounted for as they can impact RTO targets.

### Disaster Recovery Tier 2

#### Tier 2 - Recovery Objectives (Examples)

Recovery Time Objective: 48 Hours

Recovery Point Objective: 1 Hour

#### Tier 2 - Classification Examples

Non-critical Applications
Non-critical Citrix use cases
User Data that does not impact the functionality of Tier 1 or Tier 0 apps.

#### Tier 2 - Notes

Applications or use cases which are key to business operations, but whose short-term unavailability is unlikely to cause serious financial, reputation, or operational impacts. These applications are either recovered from backups or recovered as lowest priority by automated recovery tools.

### Disaster Recovery Tier 3

#### Tier 3 - Recovery Objectives (Examples)

Recovery Time Objective: 5 Days

Recovery Point Objective: 8 Hours

#### Tier 3 - Classification Examples

Infrequently Used Applications

#### Tier 3 - Notes

Applications with negligible outage impact should they be unavailable up to one week.
These applications are likely recovered from backups.

### Disaster Recovery Tier 4

#### Tier 4 - Recovery Objectives (Examples)

Recovery Time Objective: 30 Days

Recovery Point Objective: 24 Hours or None

#### Tier 4 - Classification Examples

Non-production environments

#### Tier 4 - Notes

Applications, infrastructure, and VDIs whose outage also have a negligible impact on business operations and can be restored over an extended period of time.
They may have an extended RPO as well, or not at all depending on their nature. These may be recovered from backups or built brand new in DR and is regarded as the last tier to be recovered.

### Why is Tier classification important for Citrix DR planning?

Tier classification is important for Citrix for the following reasons:

-  How critical is the Citrix infrastructure to business operations? It is important to consider that if Citrix is deemed important but an application it hosts is regarded as critical, Citrix infrastructure would become classified as critical.
-  The use cases or core business applications which Citrix hosts; do they have different DR tier classifications?

For the first question, many enterprise Citrix deployments tend to be classified as Tier 0 due to delivery of mission critical apps or desktops; the “always on” tier not unlike network, Active Directory, DNS, and hypervisor infrastructure. This may not always be the case however for every Citrix use case. Treating every Citrix use case as Tier 0 when some may fall into Tier 1 or higher can impact the overall cost and complexity of the DR process.

The second question stresses classification by Citrix use case and lends its importance most significantly in cloud environments which will be [discussed later in more detail](#disaster-recovery-in-cloud). In cloud, there are significant cost considerations between accommodating for “always on” application or desktop platforms, vs. applications or desktops regarded as being less business critical. Such considerations may also influence application or use case isolation (silos) in production to take advantage of deployment flexibility in a DR platform.

When establishing a DR design for Citrix, bringing the discussion beyond the scope of Citrix itself is useful to set expectations to business units. As an example, a Citrix environment is deemed to be an “always on” service and will be made highly available in an alternate data center, yet critical application back-ends which Citrix’s hosted applications depend upon will remain unavailable for several hours as recovery is invoked. This creates a recovery time disparity between the two platforms and may provide a misleading user experience during recovery. Citrix is available immediately but key applications are not functional. Setting expectations at the outset provides appropriate visibility to all stakeholders on what recovery experience may look like.  In some situations, a customer may want to keep Citrix on hot standby (always on) in the opposing facility but manually control failover of the access tier, to avoid misunderstandings on platform availability.

## Disaster Recovery Options

In this section, common Citrix recovery strategies are outlined including their pros and cons, and key considerations. Other recovery capabilities or variations of the following themes for Citrix are possible, this section is focusing on some of the most common. In addition, this section illustrates how responses to  the core questions indicated early on influence the DR design.

Correlating to several of the earlier DR questions, the following question topics have Citrix DR design implications as follows:

-  **Backup Strategy and Recovery Time Objective (RTO).** If Citrix infrastructure or applications served from Citrix are deemed mission critical, it is likely Citrix needs to employ an “always on” model where hot standby or Active-Active Citrix infrastructure is present at two or more data centers. This would result in multiple independent Citrix control planes being created at each data center (separate Citrix Sites, PVS farms if applicable, StoreFront Server Groups, etc.). Spanning the control planes for Citrix infrastructure does not constitute DR (ex. spanning a Site across data centers or using satellite zones); doing so if the business is anticipating a DR platform for Citrix, will be counter to that mandate.
-  **Scope of Recovery.** If Citrix is deployed in a hot standby (always on) capacity in DR but the core business application back-ends take, e.g., 8 hours to recovery, it may not make sense to employ automated access tier failover. Manual failover of the access tier may be more appropriate.
-  **Use Cases.** If only mission critical workloads or core applications need to be rapidly recovered in Citrix to maintain business operations in a DR scenario, this can likely reduce DR costs from a capacity perspective. If multiple use cases of differing priorities need recovery, the classification of importance per use case contrasted to business impact per the recovery tiering strategy [discussed earlier](#disaster-recovery-tier-classifications) may not reduce capacity costs but will provide IT staff a more focused set of recovery priority orders.
-  **Capabilities.** If certain components such as HDX Insight, Session Recording, etc. are not deemed critical to DR platform operation, their omission in the DR environment removes some complexity and maintenance overhead. Similarly, if many use cases in a DR scenario can subsist on a simpler and more generic Citrix delivery option in DR, this may also reduce complexity and costs. For example, using a Hosted Shared Desktop instead of Pooled VDI if technically feasible, or aggregating more use cases into one, provided they are not detrimental to business operations.
-  **Existing DR.** If the existing DR strategy of the organization, for example, recovers Citrix and other application infrastructure using data replication and orchestration tools, most Citrix components may be suitable for this model. If, however, the size of the platform and reliance on single-image management technology is a production platform requirement, then often such technologies may not be suitable; a hybrid approach of a hot standby Citrix platform and perhaps replication of master images may be more appropriate.
-  **Data Criticality.** If profiles are deemed critical for recovery in DR, appropriate replication technology may be necessary. Many organizations are less concerned with profiles in DR scenarios and accept them being created new. This consideration will also apply to user data accessed in Citrix (folder redirection, mapped drives); RPO and RTO for this data may be a business decision. Furthermore, if many persistent VDIs are deemed important enough to retain intact in DR (versus requiring users to reinstall their software, etc.) then these VMs need to be considered for replication, which can incur extra costs to support recovery.
-  **Types of Disasters.** The extent to which a customer wants to protect against failure may dictate various architectural changes. If the customer only wants HA of the Citrix infrastructure within the data center or cloud region, this may be merely require ensuring management components are both redundant and operating on opposing infrastructures. As an example, a StoreFront server pair leverages VMware anti-affinity rules to operate on different hosts in a cluster, or on different clusters entirely within the data center, or perhaps as part of different availability sets. This seldom requires creating redundant control planes entirely (multiple Citrix Sites and StoreFront Server Groups for example). However, if DR is intended to encompass multiple data centers regardless of region, then redundant control planes leveraging local dependencies at each data center (AD, DNS, SQL, hypervisor, etc.) is more appropriate. If the customer is global and employs multiple data centers to service Citrix (or plans to) with respective application back-ends local to these data centers, then it is more likely that a geo-localized Active-Active HA-DR architecture is more appropriate. This provides users with the most optimal user experience when possible, by using geo-localized Citrix infrastructures which can if need be, fail over to a backup data center in a secondary preference order.
-  **Client Users.** Beyond relation to the above point about user, app, and data localization considerations, some client user networks may be relatively locked down with security devices (proxy, firewall, etc.) which may restrict outbound communication to the Internet or even the WAN. It is important to consider if this applies to the client networks and ensure that new IPs for Citrix services (such as StoreFront VIP and VDA IPs, or Citrix Gateway IP) are accounted for in their local security configurations to ensure further recovery delays do not occur due to local LAN security restrictions when invoking DR. From a preparedness perspective, in event of DR, will client access need to change in some fashion? In some DR scenarios for a customer may assume the WAN will be unavailable and all users must access Citrix resources via the Internet. Such steps would necessitate documentation in the BC and readiness plans, with pre-requisites established for end-users (regarding supported Citrix client details, assumptions on corporate or BYOD device access) to remove barriers for users returning to service , further reducing the burden on the support desk.
-  **Network Bandwidth.** The amount of network bandwidth used with regards to VDA traffic (ICA, applications, file services) needs to be factored into DR facility network sizing and firewalls. This is especially important in cloud environments where limits on VPN gateways and virtual firewall capacity exists. Monitoring production traffic from VDAs to determine an average value for sizing is important to effectively size networking. Where network constraints exist, organizations may need to use different network configurations to accommodate anticipated DR traffic load if and when invoked. Citrix SD-WAN’s WAN Optimization technology may help with reducing traffic demands.
-  **Fallback (or Failback).** If user data which has changed in DR, or if VDA images while in DR, the organization may need to plan for failback to propagate these changes back to production, should the production environment prove recoverable. For user data this may be as simple as reversing the replication order and recovering; similarly, for Citrix infrastructure if not using single image management technologies. If using MCS or PVS, master images or vDisks could be manually replicated back to production and VDAs updated accordingly.

The following list outlines various common recovery options for Citrix. Adaptations of each exist in the field, however for sake of comparison we are outlining basic versions of each. The options are organized starting with the simplest (often higher RTO and lower cost) through to more advanced (often lower RTO but higher cost). The ideal option for a given organization should align to recovery time for hosted applications or use cases, as well as the IT skills, budget, and infrastructure available. In addition, although possible to accomplish, many options indicate that Citrix technologies integrated with network and storage such as ADC and single-image management technologies are not suited for methods other than “always on” recovery models. It is not that it is technically impossible to accomplish, but the level of complexity involved in accomplishing them can make recovery riskier and more prone to human error.

### Recovery Option - Recover from Offline Backup

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_003.png)

#### Pros and Cons

Pros:

-  Lower maintenance cost compared to replication or hot-standby solutions

Cons:

-  High downtime impact
-  Larger, more detailed recovery plan (DR orchestration) documentation
-  Extended recovery time
-  Relies on integrity and age of backups
-  Higher degree of human error if manual reconfiguration is necessary (networking, etc.)
-  Generally unsuitable for MCS or PVS
-  Generally unsuitable for Citrix VPX ADC due to networking (and may need to be rebuilt leveraging backups of `nsconfig` directory and `ns.conf` files)

#### Use Case and Assumptions

Useful for less mature IT organizations and organizations with limited IT operations budgets and can allow for extended outages to recover core business services. Assumes backups are tested for restore integrity regularly and follows clearly documented recovery processes.

### Recovery Option - Recover via Replication

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_004.png)

#### Pros and Cons

Pros:

-  Replication is likely automated and aligns to RTO and RPOs
-  Likely uses less complex technologies as compared to automated recovery solutions

Cons:

-  Relies on administrative intervention
-  Larger, more detailed recovery plan (DR orchestration) documentation
-  Higher degree of human error if manual reconfiguration is necessary (networking, etc.)
-  Generally unsuitable for MCS or PVS. Recreation of Machine Catalogs will need to be factored into projected RTO
-  Generally unsuitable for Citrix VPX ADC due to networking and thus better suited to employ hot standby ADC

#### Use Case and Assumptions

Useful for less mature IT organizations and organizations with limited IT operations budgets. This solution relies on storage replication technologies from SAN vendor or hypervisor vendor (vSphere Replication, etc.) to replicate VMs to another facility over the WAN.

### Recovery Option - Replication with Automated Recovery

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_005.png)

#### Pros and Cons

Pros:

-  Lower maintenance cost compared to hot-standby solutions
-  Replication is likely automated and aligns to RTO and RPOs
-  Recovery plans tend to be automated
-  Less administrative intervention and human error

Cons:

-  Relies on more advanced technologies such as VMware SRM, Veeam, Zerto, ASR, etc. to orchestrate recovery and modify network parameters, etc.
-  Generally unsuitable for MCS or PVS. Recreation of Machine Catalogs will need to be factored into projected RTO
-  Generally unsuitable for Citrix VPX ADC due to networking and thus better suited to employ hot standby ADC

#### Use Case and Assumptions

Useful for enterprise organizations with appropriate resources and budget for DR facility.
This solution relies on the same storage replication of the previous option but includes DR orchestration technologies to recover VMs in particular order, adjust NIC configurations (if needed) etc.

### Recovery Option – Hot Standby (Active-Passive) with Manual Failover

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_006.png)

#### Pros and Cons

Pros:

-  Short recovery time as platform is "always on"
-  Supports storage and network dependent components such as VPX, MCS, PVS
-  Lower recovery plan (DR orchestration) documentation

Cons:

-  Relies on administrative intervention to fail over URL or direct users to backup URL
-  Higher cost due to having "hot" hardware in DR sitting on standby
-  Higher administrative overhead to keep standby platform's configurations and updates in sync with production

#### Use Case and Assumptions

Useful for enterprise organizations with appropriate resources and budget for DR facility. Can leverage a hot-standby “fully scaled” platform or a “scale on-demand” platform. The latter may be appealing for cloud recovery to reduce operating costs, with [caveats](#disaster-recovery-in-cloud).

At time of failover administrators update the DNS entry for the access URL(s) to point to the DR IP(s) for Citrix Gateway and StoreFront, or users are advised by formal communication to commence using a “backup” or “DR” URL.

This manual option may be useful for scenarios where application back-ends may require longer recovery time but would add confusion for users if Citrix was fully available and applications were not.

This model assumes a mature IT organization and enough WAN and compute infrastructure are available to support failover.

### Recovery Option – Hot Standby (Active-Passive) with Automated Failover

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_007.png)

#### Pros and Cons

Pros:

-  Short recovery time as platform is "always on"
-  Supports storage and network dependent components such as VPX, MCS, PVS
-  Minimal recovery plan (DR orchestration) documentation
-  More seamless to end-users as URLs fail over
-  Supports EPA Scans at Citrix Gateway

Cons:

-  Higher cost due to having "hot" hardware in DR sitting on standby and Citrix ADC licensing
-  Higher access tier complexity
-  Higher administrative overhead to keep standby platform's configurations and updates in sync with production

#### Use Case and Assumptions

A common configuration with enterprise customers and allows for automated failover to the DR site via Citrix ADC GSLB (ADC Advanced or higher needed).
This model assumes a mature IT organization and enough WAN and compute infrastructure to support failover. This also assumes that application and user data dependencies are in alignment with the latest Active site versions/updates and recoverable at the DR facility in a similarly automated manner to reduce prolonged impact of service to end-user and confusion on account of partial solution functionality.

### Recovery Option – Active-Active with Automated Failover

![Disaster Recovery](/en-us/tech-zone/design/media/design-decisions_cvad-disaster-recovery_008.png)

#### Pros and Cons

Pros:

-  Short recovery time as platform is "always on"
-  Supports storage and network dependent components such as VPX, MCS, PVS
-  Minimal recovery plan (DR orchestration) documentation
-  Seamless to end-users

Cons:

-  Higher cost due to having "hot" hardware in DR sitting on standby and Citrix ADC licensing
-  Highest access tier complexity
-  Higher administrative overhead to keep standby platform's configurations and updates in sync with production
-  Active/Active GSLB does not currently support EPA scans at Citrix Gateway, Active/Passive GSLB configuration for Citrix Gateway URL suggested
-  Relies on administrators to monitor and adjust resource and hardware capacity at all data centers to ensure as platform grows, DR capacity integrity is not impacted

#### Use Case and Assumptions

A more advanced but common configuration with enterprise customers and allows access tier URLs to operate in an Active-Active manner via Citrix ADC GSLB (ADC Advanced or higher needed). This is useful in environments with local data center proximity to each other, or in situations where data centers may be remote but with the means to pin users to preferred data centers (often driven by advanced StoreFront configurations and GSLB to a lesser extent) for multi-site scenarios.

This model assumes a mature IT organization and enough WAN and compute infrastructure to support failover. This also assumes that application and user data dependencies are in alignment with the latest site versions/updates and recoverable at the DR facility in a similarly automated manner to reduce prolonged impact of service to end-user and confusion on account of partial solution functionality.

## Disaster Recovery in Cloud

Disaster Recovery involving on-prem to cloud platforms or cloud-to-cloud comes with its own set of challenges or considerations which often do not present themselves in on-prem recovery scenarios.

The following key considerations should be addressed during DR design planning to avoid missteps which may render the DR plan leveraging cloud infrastructure either invalid, cost prohibitive, or unable to meet target capacity in event of DR.

### Consideration – Network Throughput

#### Impact Areas

Availability
Performance
Cost

#### Details

Customers may underestimate and thus undersize the throughput juncture points in their cloud solution including virtual firewall, VPN gateway, and WAN uplink (AWS Direct Connect, Azure Express Route, GCP Cloud Interconnect, etc.) if the Citrix platform is to recover in the Cloud and be accessible over the WAN.
Azure VPN Gateways and AWS Transit Gateways presently have maximum limits of 1.25 Gbps. This may require scaling out gateways or perhaps leveraging multiple VPCs (in case of AWS) if these are critical to the cloud architecture.  
Many virtual firewalls have licensed limits on throughput they can process or maximums even at their highest limit. This may require scaling out the number of firewalls and load balancing them in some manner.

#### Recommendations

When establishing throughput sizing calculations, assume the full DR capacity load. Capture the following data per concurrent user:

-  Estimated ICA session bandwidth
-  Estimated application communication bandwidth per session if they traverse security boundaries
-  Estimated bandwidth for file services per session if they traverse security boundaries

For the above metrics, it may be useful to gather data on current traffic patterns to and from VDAs in production. It is also important to consider what other data flows unrelated to Citrix are also anticipated to be using these network paths.
Be sure to engage the network and security teams in planning Citrix DR to ensure any bandwidth estimates traversing security zones and network segments is understood and can be accommodated for. When bandwidth is at a premium, Citrix SD-WAN WAN Optimization may help with lowering session footprint on the wire or aggregating bandwidth across multiple network connections.

### Consideration – Windows Desktop OS Licensing

#### Impact Areas

Cost

#### Details

There are potentially complex licensing considerations for Microsoft Desktop OS instances running on different cloud platforms. Microsoft [revised their cloud licensing rights](https://www.microsoft.com/en-us/licensing/news/updated-licensing-rights-for-dedicated-cloud) in August 2019 which may affect VDI cost implications in some deployment scenarios.

#### Recommendations

Please refer to the most current Microsoft guidance when determining the use case architecture. If there is a potential cost challenge for VDI in the DR solution, consider supplementing with Hosted Shared Desktops (augmentation of RDS CALs may be required), if feasible, as they may provide greater flexibility at a lower cost of operation.

### Consideration – Timing of VDA Scale Out (Before or During DR)

#### Impact Areas

Cost
Availability

#### Details

Customers are drawn to cloud by the appeal of paying for capacity only when it is needed. This can reduce DR costs dramatically by not paying for reserved infrastructure whether it is used or not.

However at large scales, a cloud provider may not commit to an SLA of powering on hundreds or thousands of VMs at once. This becomes particularly challenging if the VDA footprint for DR is anticipated to run into the hundreds or thousands of instances. Cloud providers tend to maintain bulk capacity for various instance sizes on-hand; however, this can vary from moment to moment. If a disaster affecting a geographical area occurs, there may be contention from other tenants also requesting on-demand capacity.

Key questions that should be answered that may influence decisions:

-  Is always 100% DR capacity required?
-  Do we need to host critical workloads only (i.e. subset of production)?
-  Is it viable to scale out at time of DR? If so, have the use cases been prioritized by DR Tiers to better understand the varying RTOs of each use case to support a phased scale out of capacity?
-  Can we scale up the OS instances supporting app or hosted shared desktop use cases to save on provisioning time and power off these instances to conserve operating costs?

#### Recommendations

Citrix recommends first engaging with your cloud provider to determine the viability of powering on the anticipated capacity within the RTO timeframe and if it can be met with on-demand instances or not.
To hedge against capacity availability limitations for VDAs in a DR scenario, Citrix recommends provisioning VDAs across as many availability zones as possible. At large scales, it may be worthwhile provisioning across various cloud regions, and adjusting the architecture accordingly. Some cloud providers have suggested employing varying sizes of VM instance types to further mitigate VM exhaustion.
Where possible, it would be prudent to pre-provision VDA instances and keep them offline and update them regularly. Provisioning is a resource and time intensive process and scaling out VDA DR capacity on-demand can be expedited to a degree by pre-provisioning.
If the organization has little appetite for capacity availability risk, it may require leveraging reserved or dedicated compute capacity and budgeting accordingly to guarantee resource availability.
It is possible to combine on-demand and reserved\dedicated models by referencing the DR recovery tier wherein certain use cases may require VDAs to be immediately available, while others may have flexibility to be recovered over a longer RTO of several days or weeks if they are less critical to sustaining the business.

### Consideration – Application and User Data

#### Impact Areas

Cost
Availability
Performance

#### Details

The location of user data and application back-ends can have a notable impact on performance and in some cases, availability of the Citrix DR environment. Some customer scenarios use a multi-data center DR approach where not all application back-ends or user data such as home and departmental drives may not be recovered to cloud alongside Citrix.  This can result in unanticipated latency which may impact performance or even functionality of the applications. From a throughput perspective this can add further strain to available network and security appliance bandwidth.

#### Recommendations

Where possible, keep application and user data local to the Citrix platform in DR to maintain performance as optimal as possible by reducing latency and bandwidth demands across the WAN.

## Operation Considerations

Maintaining the DR platform is essential to maintaining its integrity to avoid unforeseen issues when the platform is required. The following guidelines are recommended with respect to operating and maintaining the DR Citrix environment:

-  **The Citrix DR Platform is Not Non-Prod.** Customers with a “hot-standby” environment may be tempted to cut corners and treat DR as a test platform. This negatively impacts the integrity of the solution. In fact, DR would likely be the last platform to promote changes to, in order to ensure its utility is not affected in event maintenance went catastrophically wrong in a manner that was not exhibited in non-prod environments.
-  **Patching and Maintenance.** Routine maintenance in lock-step with production when using “hot standby” Citrix platforms is essential to maintaining a functional DR platform. Keeping DR in sync with production with regards to OS, Citrix product, and application patches is important. To mitigate risk, allowing several days to a week between patching production and patching DR is suggested.
-  **Periodic Testing.** Regardless of whether DR involves replication of production to a recovery facility or the use of “hot standby” environments, it is important to periodically test (twice a year or more is ideal) the DR plan to ensure the teams are familiar with recovery processes and any flaws in the platform or workflows are identified and remediated.
-  **Capacity Management.** True for both Active-Passive and Active-Active “always on” Citrix environments, capacity or use case changes in production must be considered for DR as well. Especially when Active-Active models are used, it is easy for resource utilization to increase beyond say, a 50% steady-state utilization threshold of resources at each environment, only for a DR event to occur and the surviving platform becoming overwhelmed and either performing poorly or failing due to the overload. Capacity should be reviewed monthly or quarterly.

## Summary

We have covered a fair bit of ground on the topic of disaster recovery in Citrix. The following points summarize the core messages and takeaways of this guide for architects and customers alike to consider when planning their Citrix recovery strategy:

-  Understanding business requirements and technological capabilities of a customer environment will influence the disaster recovery strategy required for Citrix. The recovery strategy chosen should align to business objectives.
-  High availability is not synonymous with disaster recovery. However, disaster recovery can include high availability.
-  Sharing administrative boundaries between physical locations does not constitute a disaster recovery solution.
-  Developing a disaster recovery tiering classification for an organization’s technology portfolio will provide visibility, flexibility, and potential cost savings when developing a recovery strategy.
-  RTO requirements for Citrix infrastructure or the applications that are hosted on Citrix, will be the most significant influencing factor to which disaster recovery design is needed. A shorter RTO often requires a higher solutions cost.
-  Disaster recovery architectures for Citrix which do not employ a “hot” disaster recovery platform will present limitations into technologies a customer can use, such as Citrix MCS, ADC, and Provisioning.
-  Disaster recovery strategy for Citrix should also take into consideration the recovery and recovery time of user data and application back-ends. Citrix can be engineered to recover rapidly, however users may nonetheless be unable to work if application and data dependencies are not available in a similar timeframe.
-  Disaster recovery in cloud environments possess their own considerations not present in on-premises environments which organizations must review to avoid unforeseen bottlenecks or operational impacts when invoking disaster recovery in cloud environments.
-  It is imperative that disaster recovery components are kept up-to-date to match production updates and configurations to maintain integrity of the platform.
-  Platforms that use an active-active configuration for Citrix between locations should be mindful of capacity monitoring to ensure in event of a disaster, sufficient resources are available to support projected load at one or more surviving locations.
-  Customers should test their disaster recovery plan for Citrix periodically to validate its operation and ability to serve its core purpose.

## Sources

The goal of this article is to assist you with planning your own implementation. To make this task easier, we would like to provide you with source diagrams that you can adapt for your own needs: [source diagrams](https://citrix.sharefile.com/d-sc33a95cea884119b).
