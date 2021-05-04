---
layout: doc
h3InToc: true
contributedBy: Jacob Rutski
specialThanksTo: Ronan O'Brien, Jen Sheerin, Paul Stansel, Patrick Coble, Carl Stalhood
description: Learn how to deploy Citrix Web Application Firewall (WAF) standalone or as a part of a Citrix ADC deployment. Protect web servers or applications from various attacks including Cross Site Scripting, SQL Injection, Buffer Overflow, Forceful Browsing and more. Deploy in any public cloud or your on-premises environment.
tz_title: Web Application Firewall Deployment
tz_products: citrix-networking;
---
# PoC Guide: Web Application Firewall Deployment

## Overview

This proof of concept (PoC) guide is designed to help you quickly deploy Citrix Web App Firewall (WAF) either standalone or as a part of an existing ADC deployment to protect web applications and services. This guide covers some of the basics of Citrix WAF, deployment best practices, and next steps for your WAF projects. This guide will NOT cover every protection available nor will it cover all web technologies available today.

These instructions cover cloning an application or server that is already being proxied by an ADC appliance allowing administrators to build policies and rules without disrupting existing traffic. This is not the only way to deploy a PoC for Web Application Firewall, but it is easier than other methods.

## Conceptual Architecture

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_0.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_0.png)

A Web Application Firewall operates at Layer 7, the application layer, unlike traditional network firewalls that operate at Layer 3 or Layer 4. It understands web and application protocols like HTTP, XML, SQL, and HTML.

The Citrix WAF provides both positive and negative security models for the widest protection possible. The **negative security model** employs vulnerability signatures to prevent known attacks. It also allows for rapid initial deployment and provides a mechanism for "Virtual Patching" of applications by importing the results from third party assessment tools.

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_2.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_2.png)

The **positive security model** defines the traffic patterns and user behaviors that are allowed and blocks everything else. Dynamic profiling is used by the WAF engine to build rule sets for the behavior that is acceptable.

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_3.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_3.png)

## Prerequisites

This guide assumes the following is already configured on your Citrix ADC or standalone Citrix WAF:

1.  A basic understanding of networking concepts and managing the Citrix ADC
2.  The ADC appliance is already deployed and accessible on the network
3.  A suitable license has been applied - either standalone WAF or for ADC, a **Premium** license is required
4.  An NSIP has been assigned, and is accessible for management
5.  A SNIP has been assigned and can communicate with one or more services that are protected
6.  An _internal only_ IP address and DNS entry for another VIP (the service may already be load balanced by the ADC, ensure that a separate IP and DNS are available)

    >**Note:**
    >
    >Using a different DNS name and URL to access an application MAY require adding configuration to the application to make it aware of the new name

7.  A Basic understanding of the architecture and structure of the application or service that you are protecting. _For example, if the site processes credit card information determines if credit card protections are enabled, or if the application uses an SQL database for storage determines if SQLi protections are used_
8.  A basic understanding of web protocols and technologies such as HTTP, HTML, regular expressions, JavaScript and more, depending on the application that is being protected
9.  A Basic understanding of layer 4 load balancing and configuration on the Citrix ADC
10.  A dedicated syslog server - while local syslogs are available, having a dedicated syslog server configured is recommended. Alternatively, Citrix ADM with Security Insight provides detailed analytics into the WAF engine and application security

## WAF Proof of Concept Considerations

More than likely, the web service or application that we want to protect is already being proxied by the ADC for load balancing or other traffic management. During the WAF PoC deployment we do NOT want to disrupt the application functionality or availability for existing users. To accomplish this, a duplicate virtual server is made with the only difference being that the duplicate virtual server has WAF policies bound to it.

This method also gives users the ability to test the application to ensure that it is functioning correctly and to build WAF rules for acceptable traffic.

Sizing the ADC deployment needs to be considered before enabling Web Application Firewall as it can introduce significant resource consumption. The best way to estimate sizing is to use the web server to analyze and estimate the request sizes, response sizes, and number of forms. Citrix ADC can be used to gather basic web sizing metrics, but only if the application is already being proxied:

```bash

show httpband -type RESPONSE
show httpband -type REQUEST
```

Certain protections require **significantly** more resources than others, thus referred to as **Advanced** protections. Advanced protections consume more resources because they are remembering Form Fields, URLs, Cookies, and other data in the web transaction. The WAF engine validates that the data does not change between transactions to ensure that requests and responses are not tampered with and as such consume more appliance memory. Some advanced protections support _Sessionless_ checking as noted in the configuration - sessionless protection requires more CPU resources rather than memory.

As an example, an appliance with _Advanced_ policies is able to process approximately one tenth as much traffic as an identical appliance with only _Basic_ protections. Refer to the Citrix ADC data sheets for the supported WAF throughput capabilities for each different appliance.

## Recommended Protections

These security checks can usually be enabled in the WAF profile. Most of these checks **require learned rules** to be applied before enabling blocking; do NOT enable **BLOCK** by default, without deploying learned rules first.

>**Note:**
>
>This list is NOT exhaustive. Some protections are not compatible with some applications and some protections do not add any benefit to other applications.

-  Cookie Consistency
    -  Allows session cookies from the application to be encrypted or proxied by the WAF engine
    -  Cookie Consistency prevents a malicious actor from tampering with cookies
    -  Cookie Consistency is an **Advanced** protection

       >**Note:**
       >
       >Do NOT enable this setting when users are actively using the application, as any non-encrypted session cookies sent will not be allowed

    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/cookie-protection.html)
-  Buffer Overflow
    -  Prevents attacks against insecure OS or web server software that can behave unpredictably when receiving data that is larger than it can handle
    -  Buffer Overflow detects if the URL, cookies, or headers are longer than the specified maximum size
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/top-level-protections/buffer-over-flow-check.html)
-  Form Field Consistency Check
    -  Validates that web forms returned by users have not been modified before being sent to the server
    -  Form Field Consistency is an **Advanced** protection with sessionless support
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/form-protections/form-field-consistency-check.html)
-  SQL Injection
    -  Blocks or transforms SQL special keywords and characters from the POST body, header, and cookies
    -  Only applicable if the web application uses an SQL database
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/top-level-protections/html-sql-injection-check.html)
-  Cross-Site Scripting Check (XSS)
    -  Checks the headers and POST bodies of requests for cross-site script attacks
    -  WAF can either block the request completely or transform the offending scripts to prevent them from running
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/top-level-protections/html-cross-site-scripting-check.html)
-  Deny URL
    -  Blocks connections to URLs that are commonly accessed by hackers and malicious code or scripts
    -  Custom Deny URL Rules can be added in the **Relaxation Rules > Deny URL** UI, by clicking **Edit** then **Add**
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/url-protections/denyurl-check.html)
-  Start URL (URL Closure)
    -  Prevents direct access to random URLs on a site (forceful browsing) through bookmarks, external links, page jumping, or manually entering URLs
    -  Start URL is an **Advanced** protection with sessionless support; learned rules **MUST** be deployed before blocking based on Start URL
    -  [More information in Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/url-protections/starturl-check.html)

## Initial WAF Configuration

There are two options when configuring WAF protection:

-  **Option 1:** Use the Web App Firewall configuration Wizard to create a WAF profile and WAF policy
    -  The wizard guides you through building an initial signature set and other security checks and settings as follows:
        -  In the web UI, navigate to: **Security > Citrix Web App Firewall**

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_29.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_29.png)

        -  Choose a name for the WAF profile and the profile type (Web, XML, or JSON depending on the type of application you are protecting)

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_9.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_9.png)

        -  Specify the expression that determines which traffic is processed by the WAF engine

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_13.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_13.png)

        -  Select the signatures that apply protections for known vulnerabilities - there are options to use an existing signature set or create a new set. To choose which signatures to apply to this protection profile, there are two editor options offering basic or advanced capabilities.

        -  There are navigation options on both editors to select the signatures then enable, block, log, or stats. Watch out for multiple pages of signatures and ensure that all are selected. The Basic editor is easier to get started with, while the advanced editor offers more capabilities

        >**Note:**
        >
        >Ensure that the signatures are marked as ENABLED prior to moving on to the next step

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_10.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_10.png)

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_21.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_21.png)

        -  Choose the Deep Protections that are applicable to the application. It is recommended to start by enabling logging, stats, and learning; do NOT enable blocking initially as many protections require some number of learned rules

        [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_11.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_11.png)

        >**IMPORTANT:**
        >
        >If the WAF wizard is used to create a profile and policy, it does become bound GLOBALLY! If the filtering expression is left as default (true) then ALL traffic is processed by the WAF engine, likely leading to applications misbehaving. The recommended action is to unbind the policy from the _Default Global_ bind point and instead bind to the LB virtual server that is created later. To unbind a WAF policy from the Global bind point, do the following:
        >
        >-  Navigate to **Security > Citrix Web App Firewall > Policies > Firewall**
        >-  Choose **Policy Manager**, then choose **Default Global**
        >-  Highlight the WAF policy listed, and choose **Unbind**

    [![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_12.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_12.png)

-  **Option 2:** Alternatively, WAF protection can be applied to an application or site by manually creating a WAF profile, WAF policy, and applying a signature set
    1.  **Create a signature set for the application**
        1.  Navigate to **Security > Citrix Web App Firewall > Signatures**
        1.  Highlight the **Default Signatures** then press **Add** to create a copy of the default signature set
        1.  Select the appropriate signatures depending on the application or site being protected and ensure that the signatures are set to **ENABLED**
    2.  **Create a Web App Firewall Profile**
        1.  Navigate to **Security > Citrix Web App Firewall > Profiles**
        1.  Give the profile a descriptive name then select Web, XML, or JSON depending on the type of application you are protecting
        1.  Select **Basic** to apply the basic default settings
        1.  Once created, highlight the new Profile, and press **Edit**
        1.  Open **Security Checks** and deselect **BLOCK** on all items
        1.  Open **Profile Settings**
            -  Select the previously created Signature Set in the **Bound Signatures** setting
            -  If the back-end application server supports _chunked requests_, enable **Streaming** to improve performance
            -  Modify any additional settings based on the application or website being protected
    3.  **Create a Web App Firewall Policy**
        1.  Navigate to **Security > Citrix Web App Firewall > Policies > Firewall**
        1.  Press **Add** to create a new policy
        1.  Give the policy a descriptive name and select the WAF Profile created earlier
        1.  Create a traffic expression that determines which traffic is processed by the WAF engine

Regardless of the method used, a basic Web App Firewall configuration is now available to be bound. The final step is to update the signature file.

-  Navigate to Security > Citrix Web App Firewall > Signatures
-  Highlight the newly created signature set and press **Select Action > Auto Update Settings**
-  Enable _Signatures Auto Update_ and ensure that the update URL is populated

   >**Note:**
   >
   >Signature update requires that the ADC can resolve DNS names and access the public internet from the NSIP

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_14.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_14.png)

-  Once auto-update is configured, ADC checks for updates once per hour. The signatures can be updated manually as well and a manual update needs to be run once after the WAF profile and policy are created

To verify if the ADC appliance is able to reach the signature updates, the following command is run from the **CLI** (exit to shell):

```bash

shell
curl -I https://s3.amazonaws.com/NSAppFwSignatures/SignaturesMapping.xml
```

The system should return an **HTTP/1.1 200 OK**

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_30.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_30.png)

The most recent signature update documentation is available on [Citrix Prod Docs](/en-us/citrix-adc/current-release/application-firewall/signature-alerts.html) - updates via RSS are also available.

By default, Common Event Format (CEF) logging is not enabled, with the WAF using native logging format. Additionally, the WAF enginge can include GeoLocation information for requests. For ease of troubleshooting, it is recommended to enable CEF logging and GeoLocation by navigating to **Security > Citrix Web App Firewall > Change Engine Settings**. Check the boxes for **CEF Logging** and **Geo-Location Logging**. Alternatively, these settings can be enabled in via the CLI as follows:

```bash

set appfw settings GeoLocationLogging ON CEFLogging ON
```

## Clone Virtual Server Configuration

Once the Web App Firewall Profile and Policy have been created, it needs to be bound to the load balancing virtual server. If the application or service is not already configured on the ADC, it needs to be configured from scratch. If the application is already configured, it needs to be cloned.

To quickly clone an existing virtual server, do the following:

-  Navigate to **Traffic Management > Load Balancing > Virtual Servers**

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_31.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_31.png)

-  Highlight the existing virtual server and press **Add** to create a new virtual server with similar settings
    -  Give the new virtual server a new name and different IP address
    -  Ensure that the port and protocol are correct then press **OK**
    -  Bind services or service groups then press **Continue**
    -  Modify the load balancing method, persistence, and any other Traffic settings needed to match the existing virtual server
    -  If this virtual server is an HTTPS virtual server, ensure that an appropriate certificate is bound and a secure Cipher Suite is bound

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_15.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_15.png)

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_16.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_16.png)

Next, the WAF policy needs to be bound to this new virtual server:

-  Navigate to **Traffic Management > Load Balancing > Virtual Servers**
-  Highlight the newly created virtual server and press **Edit**
-  In the right **Advanced Settings** column, choose **Policies**
-  Press the **+ (plus)** icon to add a policy
    -  Choose an **App Firewall** policy with a type of **Request**
    -  The default binding details can be left alone

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_17.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_17.png)

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_18.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_18.png)

## User Acceptance Testing and Initial Troubleshooting

At this point, users can begin testing the application using the new URL, accessing the web application with WAF policies bound. Depending on the complexity of the application, multiple users across different business units need to use the application to generate an acceptable amount of traffic. This traffic is used by the learning engine to see all parts of the application and to ensure there are no false positive blocks that prevent application functionality.

During initial testing, information pertaining to the offending request can be difficult to find, but there a few settings that assist with this. The first is to set a custom HTML Error Object that presents diagnostic information whenever a request is blocked. If an error object is not set, a user is often be presented with a blank page that does not provide any information.

>**Note:**
>
>This is recommended to be removed or changed once the WAF protected application is exposed to a non-testing audience.

-  Set the **HTML Error Object** to present diagnostic information
    -  Go to the **WAF Profile**, then expand **Profile Settings**
    -  Select the HTML Error Object and click the add button (or plus) to create a new object
    -  In the 'Import HTML Error Page' choose **Text**
    -  Give the error object a name and paste the following code:

```html
<html>
<head>
<title>Application Firewall Block Page</title>
</head>
<body>
<h1><B>Your request has been blocked by a security policy<B><BR></H1>
<H3>Access has been blocked - if you feel this is in error, please contact the site
administrators quoting the following: </H3> <UL>
<li>NS Transaction ID: ${NS_TRANSACTION_ID}
<li>AppFW Session ID: ${NS_APPFW_SESSION_ID}
<li>Violation Category: ${NS_APPFW_VIOLATION_CATEGORY}
<li>Violation Details: ${NS_APPFW_VIOLATION_LOG} </UL>
</body>
</html>
```

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_4.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_4.png)

The resulting error page presents valuable diagnostic information, including the violation category and log details. This information allows the administrator to validate if the block needs to be relaxed due to a false positive.

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_5.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_5.png)

>**Important:**
>
>Some WAF blocks only prevent certain elements of the page from loading, for example an image, CSS formatting, script or button\form functionality may not work. These blocks are still considered issues, though the diagnostic error page does not show. Use the syslog viewer (see the example below) to find the offending signature or security check that is causing the issue.

The **Syslog Messages** that are generated by the WAF engine help understand blocked sessions when the diagnostic error page is not available

-  Go to **System > Auditing > Syslog messages**

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_32.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_32.png)

-  Under the **Filter By** column, select Module and choose **APPFW**
-  Press apply

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_6.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_6.png)

Some violations can be immediately relaxed from the **Syslog** viewer if there are false positive blocks

-  From the Syslog Messages view (preceding), find a Syslog Message that is a rule violation
-  Check the box in the top right corner of the message
-  Select the Action menu then choose deploy
-  Alternatively, you can modify the relaxation rule before deploying it

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_7.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_7.png)

>**Note:**
>
>A false-positive block from a signature violation often means that the web application is not following standards or is written poorly. The immediate action to allow the application to function is to apply a relaxation rule, but the long term goal is to fix the application itself. Relaxation rules on poorly written applications can allow malicious content through by means of allowing the application to function.

By turning on **stats** for all items, the appliance gathers counters anytime a rule or signature is violated. This is useful in the initially as it allows counters to be cleared to see if any new blocked requests occurred as a result of the WAF policy.

From the **Dashboard** tab of the ADC UI, set the stats view to **Application Firewall** to view all of the associated stat counts. Stats can be cleared from this view as well.

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_19.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_19.png)

Alternatively, stats can be viewed from the _shell_ command line of the appliance by using the following command:

```bash

nsonmsg -d stats | grep appfw
```

Be aware that the Web Application Firewall follows web standards and by default, malformed requests are blocked. To change this behavior, do the following:

-  Citrix WAF uses a strict HTTP check
-  By default invalid, non-RFC compliant HTTP requests are **blocked**
    -  To modify non-RFC compliant request handling, go to **Security > Citrix Web App Firewall** then **Change Engine Settings**
    -  See the following code for the CLI method to set only logging and stat generation for malformed requests
    >**Note:**
    >
    >This setting is global. _**If you disable the block option, the WAF engine bypasses application firewall processing for any non-RFC compliant requests. This is not recommended.**_

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_8.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_8.png)

```bash

set appfw settings -malformedReqAction log stats
```

## Security Check Learning

Learned data is used to create rules for the **positive security model** of the WAF engine - a few quick details about learned rules:

-  Stored in RegEx format and can be exported to CSV for analysis or migration
-  Are propagated in an HA pair of appliances
-  Security checks can be left in learning mode permanently, but is not necessary
-  Gathers more useful data the more traffic is processed during UAT
-  It is **strongly recommended to configure Trusted Learning Clients** for learning when the appliance is receiving traffic from both known and unknown clients. This setting ensures that malicious traffic is not allowed into a learned rule.

Learned engine runs as the `aslearn` process and rules are stored in SQLite format in the following location:

```bash

/var/nslog/asl/appfwprofile.db
```

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_22.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_22.png)

Learning mode can be enabled permanently, but the learning database has a size limit of 20 MB. The learning engine does not add new data once the database reaches this size.

From a process perspective, learning occurs as follows:

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_23.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_23.png)

Anytime that a user is accessing the new application with a bound WAF policy containing enabled learning checks, learned rules are generated. By using a **duplicate virtual server** approach, production traffic is not blocked while users test the protected application. Additionally, all traffic that is passed through the WAF engine at this point will generate learned rules.

The typical flow for building relaxation rules and ensuring the application is working as expected without false positive blocks occurs continuously:

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_20.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_20.png)

This process must be repeated anytime there is a change to an application or workflow where rules were not previously created.

To view the learned data using the UI, do the following:

-  Navigate to **Security > Citrix Web App Firewall > Profiles**, select the App Firewall profile, then choose **Edit**
-  On the right column, choose **Learned Rules**
-  Each of the security checks that support learning are displayed with a few options:
    -  Select **Edit** to view and deploy the rules that have been learned for a specific security check

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_25.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_25.png)

-  Once rules are deployed, they are moved from _learned rules_ to _relaxation rules_
-  From the learned rules UI, the rules can be exported to CSV using the **Export Learned Data** button before moving rules to relaxation rules
-  The **Visualizer** is also a valuable tool especially for Start URL checks to visualize rules and a URL map of the protected application

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_24.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_24.png)

Once the learned rules have been deployed to relaxation rules, the security check can be set to **Block**.

>**Note:**
>
>If enough traffic has not been processed or all of the application paths have not been seen by the WAF engine, there will likely be false positive blocks. The learned rule process can be repeated until the application behaves as expected.

## Import Export

The entire configuration of a Web Application Firewall profile can be replicated across multiple appliances. This process will capture all bound objects such as the HTML error object, XML error object, WSDL\XML schema, signatures, relaxation rules and so on. This can be particularly useful when migrating from a test\dev environment to a production environment or to back up the WAF configuration before making changes.

>**Note:**
>
>Exported configurations require that both appliances be on the same build.

To export the WAF profile using the GUI, on the source appliance, navigate to **Security > Citrix Web App Firewall > Profiles > Select the WAF profile, then choose Action > Export** The export process may take some time to generate the export file. Once it has been archived, a **WAFProfile.tgz** file is downloaded to the client computer.

To import the WAF profile using the GUI, on the destination appliance, navigate to **Security > Citrix Web App Firewall > Profiles > Select Action > Import**. Select 'Import From' File, then select 'Choose File' to locate the previously downloaded archive. Any issues with the import process are logged to:

```bash

/var/log/ns.log
```

>**Note:**
>
>The import process does NOT create a WAF policy or binding.

## Dynamic Profiling

The Dynamic Profiling feature was added to the WAF Engine in version 13.0 and allows for learned data to be automatically deployed to relaxation rules automatically after a configured threshold. An alert is sent to administrators to allow them to skip the data if it is not valid, otherwise it is added automatically. By default, dynamic profiling is disabled for all supported security checks - to enable it and configure the settings, do the following:

-  Navigate to **Security > Citrix Web App Firewall > Profiles**, select the App Firewall profile, then choose **Edit**
-  On the right column, choose **Dynamic Profiling**
    -  Each security check can be enabled or disabled from this UI
    -  The dynamic profiling settings for this WAf profile can also be modified by selecting **Settings**

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_26.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_26.png)

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_28.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_28.png)

When using dynamic profiling, it is strongly recommended to enable **Trusted Learning Clients** to ensure that only valid application traffic is generating rules. These clients can be added from:

-  Learned Rules UI
-  Dynamic Profiling UI

[![Citrix Web Application Firewall and Apps Architecture](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_27.png)](/en-us/tech-zone/learn/media/poc-guides_citrix-waf-deployment_27.png)

## Considerations When Moving to Production

The most important consideration with using the **duplicate virtual server** method is that any of the learned rules that include URLs do not apply if the application URL changes. For example, the Start URL and CSRF Form Tagging rules include URLs.

The best way to migrate to the production virtual server would be to turn off blocking for security checks and relearn and deploy rules that include URLs. Alternatively, the learned rules could be exported then readded after modifying the URL. It would also be very important to enable **Trusted Learning Clients** if they have not already been added.

Lastly, if the PoC is being run on a test platform that has different resources than production, it is important to consider the resource requirements that change by enabling Web Application Firewall.

## References

[Citrix Product Documentation - Web App Firewall](/en-us/citrix-adc/current-release/application-firewall)

[Citrix Product Documentation WAF - FAQ](/en-us/citrix-adc/current-release/application-firewall/DeploymentGuide.html)

[How Citrix Application Firewall Modifies Application Data Traffic](https://support.citrix.com/article/CTX131488)

[WAF Basic VS Advanced Protections](https://support.citrix.com/article/CTX130546)

[Common Event Format (CEF) Logging Support in the Application Firewall](https://support.citrix.com/article/CTX136146)

[Exporting and importing an Web App Firewall profile](/en-us/citrix-adc/current-release/application-firewall/profiles/import-export-profiles.html)
