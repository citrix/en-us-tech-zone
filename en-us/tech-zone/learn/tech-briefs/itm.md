---
layout: doc
description: Citrix Intelligent Traffic Management (ITM) service brings “intelligence” to DNS by incorporating a broad set of capabilities to determine the best query response based on analysis of real time conditions.
---
# Introduction to Citrix Intelligent Traffic Management

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Antoine Thoumelin

Citrix Intelligent Traffic Management (ITM) service brings “intelligence” to the Internet standard Domain Name Service (DNS) by incorporating a broad set of capabilities to determine the best query response based on analysis of real time conditions

![Introduction to ITM](/en-us/tech-zone/learn/media/tech-briefs_itm_introA1.png)

## ITM Overview

Today Citrix customers have a great amount of control and visibility over their users’ endpoints and internet facing apps, as well as their backend systems and services irrespective of the type of cloud environment they’re hosted in. However, they lack visibility into the internet across which those applications and services are delivered. The ITM solution eliminates this internet “blind-spot” primarily in two ways.

First it provides **Visualization** of the performance from the 10s of thousands of autonomous networks across the internet to sites where ITM customer’s internet applications and services are hosted including Internet Service Providers (ISP), Content Delivery Networks (CDN), Public or Private Clouds. Second it provides robust **Optimization** by analyzing a performance database composed of a vast set of Realtime User Measurements (RUM) to determine the optimal response to DNS queries initiated by a users’ browser or application.

![ITM Visualization and Optimization](/en-us/tech-zone/learn/media/tech-briefs_itm_overviewA2.png)

Citrix ITM, hosted in Citrix Cloud, is comprised of several components that together provide the optimal response to DNS queries. Decisions are fueled by data collected from a variety of sources including data gathered from RUM or through APIs to 3rd party platforms with other sources of performance data.
Customers may benefit from its intelligent DNS response decisions by pointing their web site domain records to it through a CNAME record or ITM may act as the authoritative DNS for the domains delivered by any of nearly 100 Name Servers (NS) around the world. With ITM, internet service users are directed to target sites according to the desired mix of several metric options such as cost, proximity, or throughput.
With its rich sources of data including billions of measurements from 10s of thousands of global networks daily and complex proprietary decision algorithms, ITM provides intelligent decision responses that can’t be achieved with other DNS systems. Therefore, ITM customers can provide their users with maximum experience for minimal cost.

![How DNS Works](/en-us/tech-zone/learn/media/tech-briefs_itm_insertA3.png)

## ITM Optimization

ITM Optimization is based on the orchestration of several components that collectively provide users of ITM customer web sites and services with the optimal response to DNS queries based on desired selection criteria.

![ITM Components](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeA4.png)

*  **Radar** - Java script downloaded from ITM customer sites captures internet performance data through a unique tag.
*  **Openmix** - Analyzes performance data to determine the optimal response to DNS queries.
*  **Predictive DNS** - Configure DNS records (A, MX, etc.) and map records to Openmix decisions.
*  **Sonar** - Global website synthetic monitoring with http/https probes used for Openmix decisions.
*  **Fusion** – Provides 3rd party performance data such as Citrix ADC load balancing metrics.
*  **Platforms** – Identifies ISPs, CDNs, Public or Private Clouds used as measurement destinations for Openmix analysis.

![Radar](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeB5.png)
[Radar](/en-us/citrix-intelligent-traffic-management/radar.html) is a unique ITM differentiator whereby a Java script tag is embedded in the index page/(s) of ITM customer web sites and downloaded by site users to measure performance data for the customer sites along with other public ITM sites. The Radar database receives its input through Billions of users making Billions of measurements daily from global networks.

Below we see an example tag add at the end of the body of an IIS web site start page.

![Radar Tag](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeC6.png)

After being downloaded the Radar Java Script captures performance data from site visitor endpoints. By default, it waits 2 seconds after the initial page load to execute and take measurements to avoid performance impact. The script communicates with the ITM service to determine what types of queries it should make to determine response time and throughput.

Queries are made to both ITM customer dedicated private web sites or services as well as a select set of public community sites where Citrix ITM agents are hosted to respond to queries. By default, a 43-byte image is downloaded, but larger images may be used to extrapolate more accurate bandwidth estimates.

Below we see browsing to a workspacedemo.net site with activity logged in Chrome developer tools. 2 seconds after the main page is loaded, and radar.js is downloaded, we see the series of queries it makes such as one to “level3.cedexis-test.com”. We also see responses it receives including a 100KB iceberg image.

![Radar Performance Measurement](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeD7.png)

All of this data is stored in ITM’s global Radar database and mapped to the Autonomous System where that browser’s endpoint’s public IP address is hosted. We’ll see that this data is utilized by other components to provide the Visualization and Optimization to eliminate the Internet blind-spot.

![Openmix](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeE8.png)
 [Openmix](/en-us/citrix-intelligent-traffic-management/openmix.html) gathers and analyzes internet performance data. It is the heart of ITM and brings together all of the data it captures to formulate the most “intelligent” DNS response based on desired selection requirements. It obtains input from several ITM components:

*  **Radar** – provides the private and public community performance database utilized by Openmix.
*  **Fusion** – feeds Openmix with performance data from API integrations with a variety of content and service providers to improve response decisions. (more below)
*  **Sonar** – feeds Openmix with the result of Synthetic Monitoring of configured customer sites. (more below)
*  **Platforms** – integrates Radar, Fusion and Sonar data into a target vehicle for Openmix analysis. (more below)

Openmix includes forms to get started quickly or the ability to craft java scripts. ITM provides a variety of [developer templates](https://developers.cedexis.com/) available in a public GitHub site. Decision parameters are configured in “Openmix Apps”.

Below we see an example Openmix “Throughput” App that returns 1 of 2 possible [Fully Qualified Domain Names](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) if selected as the optimal response after evaluating the criteria configured for the two respective Platforms (more below).

*  If analysis determines that “fastsite” is the optimal Platform, then Openmix responds with the FQDN fast.workspacedemo.net
*  If analysis determines that “slowsite” is the optimal Platform, then Openmix responds with the FQDN slow.workspacedemo.net

You can also see in the “Description” quadrant a [Canonical Name]( https://en.wikipedia.org/wiki/CNAME_record) record which [Address records](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (A) records in a customer’s Authoritative NS may map to. Alternatively, if ITM Predictive DNS is used as the Authoritative NS it may have an OPENMIX record that maps directly to the app Name “ITMThroughput”.

![Openmix throughput app](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeF9.png)

![Predictive DNS](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeG10.png)
 [Predictive DNS](/en-us/citrix-intelligent-traffic-management/predictive-dns.html) provides Authoritative DNS. It includes all standard record types (A, MX, CNAME, etc.) as well a unique OPENMIX record, which maps a simple tag to an Openmix app to obtain intelligent DNS response decisions. Hosted in Citrix Cloud it includes nearly 100 Global NSs available to provide a response nearest to the source of the DNS query.

Below we see easy to use menus within the ITM console to add Zones and Records.

![Predictive DNS Records](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeH11.png)

![Fusion](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeI12.png)
[Fusion](/en-us/citrix-intelligent-traffic-management/fusion.html) provides API integrations to third party data feeds with performance data for other internet services, providers, and clouds to enrich ITMs own data sources.

Below we see on an overlay to create a new Fusion data feed to Citrix NetScaler (now Citrix ADC - currently in Beta). In the overlay to the left of it we see performance data being pulled from the Citrix ADC via it’s [Nitro API]( /en-us/netscaler/12/nitro-api.html) and the result of the query for the stat for Virtual Server health “vslbhealth” highlighted. The Citrix ADC Virtual Server is the entity used to load balance services that map to actual services and a result of 50% indicated only half of them are up which may be used by Openmix Apps to make response decisions.

![Fusion and Citrix ADC](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeJ13.png)

Below we see an Openmix record with overlays used to create it that import a java script that utilizes the “vslbhealth” data.

![Fusion and Openmix script](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeK14.png)

![Platforms](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeL15.png)
 [Platforms](/en-us/citrix-intelligent-traffic-management/platforms.html) are destination provider sites that may be monitored. They are generally at or near the location where target ITM customer sites are hosted and identifies provider, or cloud sites where target content is hosted. Platforms are used as the vehicle to aggregate data to based used by Openmix apps for analysis.

Get started in the ITM console by creating a Platform manually with quick start forms using a public IP addresses or by selecting from the list of nearly 100 public community platforms. Below we see custom images configured for Radar to downloaded to gather site performance data, as well as Sonar polling togged to enable along with a custom URL to test.

![Platforms settings](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeM16.png)

Also, platform entries may be created by importing the GSLB sites from a Citrix ADC configuration or via the Citrix ADM service if it’s configured in Citrix Cloud along with Citrix ITM. Below we an image of the Platform menu for importing an ADC config superimposed over the resulting map showing average global response times (covered more in the upcoming ITM Visualizer section) to the 2 imported GSLB sites selected (GSLB_east and GSLB_west):

![Platforms and Citrix ADC](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeN17.png)

![Sonar](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeO18.png)
 [Sonar](/en-us/citrix-intelligent-traffic-management/sonar.html) provides “[Synthetic Monitoring](https://en.wikipedia.org/wiki/Synthetic_monitoring)” of web sites and other internet services. It works by making HTTP or HTTPS requests from multiple points-of-presence (POPs) around the world to a URL that you specify. It is the 1st criteria in the site selection decision workflow by Openmix Apps.

![Openmix decision workflow](/en-us/tech-zone/learn/media/tech-briefs_itm_optimizeP19.png)

## ITM and Navigating the Internet

The backbone of the internet is based on a web of Autonomous Systems (AS) that typically map to an ISP, CDN, or Cloud network. Each provider manages their network of routers and switches to deliver traffic efficiently to the endpoints they host represented by a range of IP addresses within their AS.

The Border Gateway Protocol (BGP) is used by providers to announce the routes they host within their AS, and subsequently to figure out the shortest path of ASes to reach each respective internet destination requested by their customer endpoints. While there are other factors providers can implement in BGP to affect path preference, the shortest AS path is, by default, the primary factor used to determine the way an AS routes traffic. This method has served the internet well to manage navigating the hundreds of thousands of networks available globally; however, it lacks the granularity to route based on factors that affect performance like Quality of Service (QOS) or congestion.

The ITM Radar service utilizes the user community of its customer’s web sites to gather billions of performance measurements daily from each ISP, CDN or Cloud provider AS to each global set of mirror sites. ITM analyzes this data and responds to its user’s DNS queries with the public IP address of the site which will give them the best performance from the AS their queries originate from.

In the diagram below, we see that after a user in their corporate Office makes their first DNS query for a Citrix ITM customer website it receives a response of “1.1.1.1” as the public IP address and subsequently its HTTP traffic to GET the initial page is routed to Site A (**shown by the RED dotted line**). As part of the initial page load a “Radar” tag is executed and a Java Script is downloaded. It communicates with the ITM service to obtain a set of instructions regarding what types of queries it should make to that particularly Citrix ITM customer web sites as well as a limited set of other sites where public ITM agents are hosted.

Thereafter if for example an Openmix Throughput app was configured the Java Script would be instructed to download an identical size image from both Site A and Site B (**shown by the dotted ORANGE lines**) from which it would extrapolate the respective throughput. Then, although Site B may have a longer “path” in the eyes of BGP and may even have a longer Round-Trip Delay (RTD) from the Office, ITM will would return “2.2.2.2” on subsequent queries since it calculates better bandwidth available as a result of the congestion in the path from the Office to Site A. Thereafter the Office endpoint’s HTTP traffic to GET the initial page would be routed to Site B (**shown by the GREEN dotted line**)

![Navigating the Internet](/en-us/tech-zone/learn/media/tech-briefs_itm_insertB20.png)

This capability is a significant unique differentiator for Citrix ITM, that other DNS or Global Server Load Balancing (GSLB) systems do not provide, thanks to the unique knowledge of the user’s perspective from the Radar RUM database.

## ITM Visualization

Visualizer provides a global heat map of average request durations for global traffic based on private or public community radar measurements. The private measurements are available to individual customers only through their ITM service account, which is accessible via Citrix Cloud. Community measurements are also available in the ITM service console or through the public site [Citrix ITM Performance Visualizer]( https://itm.cloud.com/performance) which utilizes a massive set of daily measurements:

*  ~10 billion daily ITM performance measurements
*  ~42,000 daily global networks measured
*  ~242 daily countries measured

It provides a global view of Internet Performance based on average user measurements of the complete time from the initial attempt to navigate to a web page until the first byte is received. This measurement is referred as “Average Request Direction for Global Traffic”, which is comprised of 3 sets of times:

*  **DNS** – the client queries the primary NS to determine the IP address of the site. This duration consists of the time for the endpoint client to receive a response to that initial query, which includes the collective time for any recursive queries.
*  **TCP** – once the client has the public IP address, it will try to establish a TCP session to transport the Hypertext Transfer Protocol (HTTP) traffic. The time required to establish a TCP session includes, at a minimum, a SYN to the server, an ACK back to the client, followed by a SYN+ACK back to the server to complete session setup.
*  **Response** – the client browser performs a HTTP GET to the web site page, resulting in the the client downloading the contents while recording the Time to first byte (TTFB). TTFB is a standard measurement in most current browsers, indicating the duration between the client’s initial GET to the first byte of the page being received by the browser.

![Average Request Direction for Global Traffic](/en-us/tech-zone/learn/media/tech-briefs_itm_visualizeA21.png)

To use the community map, customers may select their public ISP, CDN, or Cloud provider from a drop-down list of sites where ITM agents are hosted. Then global performance for selected sites, or points selected directly on the map, may be compared and contrasted against alternative DNS response methods:

*  **Round Robin** – is a common default method for DNS systems which responds by rotating through it’s list of public IP addresses mapped to a record in circular fashion. It provides some redundancy, but results in random and generally poor performance.
*  **Geo Proximity** – is a common method for GSLB systems that responds with the public IP of the closet site by identifying the approximate location of the local DNS querier through a static global database of IP addresses or through a dynamic ping.
*  **ITM Optimized** – analyzes a broad set of data including it’s Radar community measurements, Fusion API extracted input, or Sonar Synthetic monitoring to calculate the most “Intelligent” response based on selection criteria such as RTD, bandwidth, or cost.

Below we see an example comparison of 4 AWS sites that use Geographic Proximity for their DNS response method alongside a set of 3 sites that use, radar based, ITM for DNS responses. We can see that there’s a 13% increased performance benefit by using ITM albeit with only 3 sites instead of 4.

The AWS sites chosen are from among nearly 100 public sites available where Citrix ITM agents are hosted. Alternatively, customers may enter public IP addresses of their On-Premises or Private Cloud hosted sites, or just click at point on the map they would like to measure to.

![Visualizer Benefit](/en-us/tech-zone/learn/media/tech-briefs_itm_visualizeB22.png)

![DNS, BGP, and IP Anycast](/en-us/tech-zone/learn/media/tech-briefs_itm_insertC23.png)

## ITM Use Cases

ITM can solve many Citrix Workspace customer internet service delivery challenges with these key Use Cases:

*  **Intelligent DNS** – ITM Predictive DNS, hosted by Citrix, provides optimal reliability and response times globally.
*  **Intelligent Traffic** – ITM Openmix responds with the ideal customer hosted web site.
*  **Intelligent Cloud** – ITM Visualizer helps customers strategically identify their optimal cloud service providers.
*  **Intelligent Workspace** – ITM directs Citrix Workspace users to the nearest global pop for the best user experience.

### Intelligent DNS

Citrix ITM Predictive DNS could be a benefit to any Citrix customer with internet applications and services, yet there are two scenarios where it clearly provides value:

1.  Customers that currently host their own authoritative DNS On-Premises and are preparing or have begun moving services to the cloud. They would immediately enjoy globally high availability, performance improvement resulting in better user experiences, and all of the cost and operational benefits of Citrix managing it.
2.  Customers who use hosted DNS in the cloud with load balancing services limited to Round Robin or GSLB. They would benefit from the performance improvements and reduced cost from ITM’s sophisticated traffic steering capabilities. They would also enjoy the benefits of Citrix Global managed service.

ITM Predictive DNS is supported by nearly 100 authoritative NSs around the world. This provides Enterprises with a global reach of authoritative DNS servers in close proximity to user queries from anywhere in the world and subsequently the ability to respond to them. It also includes all of the benefits of having Citrix manage their DNS service such as operational management, scalability, reliability, etc.
To get started with Predictive DNS import your zone file or manually create your pertinent records. Then migrate the Start of Authority for your domain/(s), or subdomain/(s) to ITM NSs.

*  ns1.cedexis.net
*  ns2.cedexis.net
*  ns3.cedexis.net
*  ns4.cedexis.net

Below see an example of Start of Authority (SOA) changed from GoDaddy.com to Citrix ITM for the domain workspacedemo.net

![Changing GoDaddy.com domain SOA](/en-us/tech-zone/learn/media/tech-briefs_itm_usecasesA24.png)

### Intelligent Traffic

Citrix ITM advanced steering capabilities powered by Openmix could also be a benefit to any Citrix customer with internet applications and services, yet there are two scenarios where it clearly provides value:

1.  Citrix ADC customers using GSLB
2.  Customers who need to improve performance for their internet applications or services to other markets or countries, but unable to add another Cloud POP or CDN due to cost, red tape, or implementation time and complexity.

Openmix incorporates Radar community data, Sonar Synthetic Monitoring, and Fusion API gathered metrics and uses a robust  [decision workflow](/en-us/citrix-intelligent-traffic-management/media/openmix-sample-workflow.png) to analyze and decide the most “intelligent” response. Each Openmix app has a CNAME that may be referenced directly by an authoritative NS or they may be easily referenced by their app name with Predictive DNS.

Radar provides a performance report that can be filtered by type and statistic as well as the source include specific platforms, locations or resources. Below is a graph of the throughput calculations to a private community site, which would support the decision of an Openmix throughput app to respond with the faster site indicated by the top blue line. The blue line on the top of the graph represents the throughput ITM calculates based on Radar measurements for the website fastsite.workspacedemo.net. The orange line toward the bottom of the graph represents the throughput ITM calculates for the mirror website slowsite.workspacedemo.net. Fast site and slow site are identical web pages, yet slowsite has a WAN emulator in front of it that reduces bandwidth significantly simulating internet congestion.

![Intelligent Traffic and Internet congestion detection](/en-us/tech-zone/learn/media/tech-briefs_itm_usecasesB25.png)

Therefore, if for example a FQDN of www.workspacedemo.com was mapped to an Openmix Throughput app, using these sites, based on this data it would direct traffic to the fastsite as long as the bandwidth calculation to it remained higher than slowsite. This particular type of performance measurement is a potent capability to avoid internet congestion between the client and server which differentiates ITM capabilities form traditional DNS or GSLB systems.

### Intelligent Cloud

Citrix ITM Visualizer could also be a benefit to any Citrix customer with internet applications and services, yet there are two scenarios where it clearly provides value:

1.  **Cloud Strategy** - ITM Visualizer can help Citrix customers plan their move to the cloud or optimize their current footprint. Customers can enter their current sites and or alternative sites they are considering as part of planning exercises. ITM can provide great detail on the performance they may expect to their target markets such as ASNs, countries, or continents.
2.  **Network Operations** - the Visualizer functionality included with the ITM service, shown below, also includes a variety of operation alerts and reports that can serve as a valuable asset in a Network Operations Center. Get started with Visualizer by requesting an ITM trial in [Citrix Cloud](https://onboarding.cloud.com/).

![Visualizer and Network Operations](/en-us/tech-zone/learn/media/tech-briefs_itm_usecasesC26.png)

### Intelligent Workspace

This use case is benefit built-in to Citrix Workspace. Customers that implement Citrix Workspace services that utilize Citrix Gateway such SaaS, Web, or Virtual Apps and Desktops, benefit from the power of Citrix ITM and its Intelligent Traffic Management capabilities to direct endpoints to the optimal POP from among dozens hosted globally. You may recognize this by performing a Nslookup or Dig to obtain the SOA for Gateway hosts which is Citrix Predictive DNS.

![Nslookup and Dig](/en-us/tech-zone/learn/media/tech-briefs_itm_usecasesD27.png)

1.  If we look into the text contents of an .ica file behind the launch of Citrix Virtual App or Desktop from Citrix Workspace we see an entry for “SSLProxyHost=global.g.nssvc.net:443”.

*  SSLProxyHost indicates the Gateway that will proxy the virtual session to the Virtual Delivery Agent.
*  global.g.nssvc.net indicates the FQDN that maps to Citrix Gateway POPs.

Below in the image below on the left we see the results of a Nslookup for the Start of Authority (SOA) for global.g.nssvc.net indicating that Citrix ITM (formerly Cedexis) is in fact the Authoritative NS.
2.  If we launch a SaaS app through Citrix Workspace via a browser and look at the URI, we’ll also notice a FQDN. For example, in the image below on the right we see the results of a Nslookup for the Start of Authority (SOA) for az-us-e.app.netscalergateway.net indicating that Citrix ITM (formerly Cedexis) is also the Authoritative NS.

![Nslookup Workspace FQDNs SOA](/en-us/tech-zone/learn/media/tech-briefs_itm_usecasesE28.png)

Visit [The Citrix website](https://www.citrix.com/Workspace) for more information on how to get started with Citrix Workspace.

## ITM and Citrix ADC

Citrix ADC GSLB has been a long-standing pillar of reliability in the business continuity operation of thousands of Citrix customers for many years. Citrix ADCs utilize GSLB site entities to communicate service state information through the Metric Exchange Protocol (MEP) to ensure they have the same metrics to make the same decision for each client as to which site to steer Clients to. It also hosts Authoritative DNS (ADNS) to respond to queries for the subdomains with services hosted behind a Citrix ADC Virtual Server, that proxies’ access to the target services, with the public IP address for its “BEST available” option based on the methods implemented. [Citrix ADC GSLB Methods]( /en-us/citrix-adc/12-1/global-server-load-balancing/methods.html) include:

*  **Algorithm based** – uses traditional load balancing methods, like least connected or least response time, to select the “best” Virtual Server IP address to response to the query.
*  **Static Proximity** – determines the closest site depending on the proximity of the clients local DNS (LDNS) server by utilizing a global database maintained by [IANA]( https://www.iana.org/) which maps public IP network allocations globally.
*  **Dynamic Proximity (RTT)** – measures the time delay in the network between the clients LDNS and a data resource to select the “best” Virtual Server IP address to response to the query.

In the diagram below, we see an example scenario:

1.  Site A has the least connections, communicated between both sites via MEP, therefore it would be selected by GSLB as the **BEST** site option if Algorithm was implemented as the primary selection method
2.  Site B has the closet proximity to the clients local DNS server that made the query, therefore it would be selected by GSLB as the **BEST** site option if Proximity was implemented as the primary selection method

![ITM and Citrix ADC](/en-us/tech-zone/learn/media/tech-briefs_itm_insertD29.png)

Citrix ITM brings a new aspect of visibility to GSLB environments based on its unique visibility of site performance directly from user’s perspective. ITM can import GSLB site addresses from a ns.conf configuration or directly from Citrix ADM. The site addresses, represented as a platform object in ITM, may be selected to view average global performance in Visualizer or as the source for Openmix decisions apps.
ITM Fusion can also integrate with Citrix ADCs via their nitro APIs. Then Openmix script-based apps may draw upon Citrix ADC visibility into availability metrics such as least connected or least response time which allows ITM to make an even better DNS response decision.

## ITM Getting Started

Citrix ITM is sold as a cloud service in several editions with additive functionality.

**ITM Express**
A free service available to try ITM with the ability to import Citrix ADC GSLB sites and view them or public community sites in Visualizer. It includes 24 hours of Network Monitoring and Traffic Management Data Retention.
Get started by selecting “Request Trial” from your Citrix Cloud account.

![ITM and Citrix ADC](/en-us/tech-zone/learn/media/tech-briefs_itm_getstartedA30.png)

**ITM Standard**
Adds Predictive DNS and the ability to provide global reach and the high availability that comes with as-a-service cloud hosted solutions, along with Sonar and Synthetic Monitoring. It includes 100 days of Data Retention.

**ITM Advanced**
Adds the power Openmix apps and ability to make complex DNS load balancing decision, along with Fusion and the ability to ingest Citrix ADC GSLB metrics in real time through the Nitro API. It includes 1 year of Data Retention.

**ITM Premium**
Adds the ability to delivery traffic management decisions through HTTP APIs in addition to DNS and captures 3rd Party Video Data and Metrics. It also includes 1 year of Data Retention.
