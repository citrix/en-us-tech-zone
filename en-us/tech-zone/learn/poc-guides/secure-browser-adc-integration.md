---
layout: doc
description: Learn how to set up Citrix ADC in Azure and Configure SSL Forward Proxy and SSL Interception. This integration allows the dynamic delivery of resources by redirecting browsing to Secure Browser service providing a secury to the company network without sacrificing user experience. 
---
# Proof of Concept Guide: URL Redirection to Secure Browser with Citrix ADC in Azure

## Contributors

**Author:** [Nagapandu Potti](URL), [Alejandra Roca](URL)

## Overview

Here are the configuration steps for setting up an ADC, configuring SSL Forward Proxy and SSL Interception using the latest Citrix ADC marketplace template. This capability of the ADC enables administrators to define specific website categories to be redirected from the local browser to Secure Browser automatically, thus achieving web isolation and protecting the corporate network. All of this increases security without compromising user experience. The Citrix ADC acts as an intermediate proxy to do the interception between local browsing and the internet.

## Conceptual Architecture

[![URL redirection to Secure Browser Service Architecture](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_1.png)](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_1.png)

## Scope

This proof-of-concept guide will describe the following:

1.  Obtain Secure Browser Trial Account
1.  Set up ADC as proxy to route the traffic from client browser to the Internet
1.  Set up SSL Interception and Rewrite Policies/Actions for URL redirection to Secure Browser

## Deployment Steps

### Section 1: Obtain Secure Browser Trial Account

[Reference](https://docs.citrix.com/en-us/citrix-cloud/secure-browser-service.html)

**Step 1:** Enter Username and Password Click Sign In. If your account manages more than one customer select the appropriate one

![Step 1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_2.png)

**Step 2:** Click Citrix Cloud on top of the page. Search for the Secure Browser Tile.

*  If you know who your account team is, please reach out to them to get the trial approved. If you are unsure who your account team is, please continue to next step.

![Step 2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_3.png)

**Step 3:** Click Request a Call

![Step 3.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_4.png)

*  Enter your details and in Comments section specify “Secure Browser service trial”. Click Submit. Citrix Sales will contact you to give you access to the service. Note: This is not immediate, a Citrix sales rep will reach out

![Step 3.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_5.png)

Once you have Secure Browser trial approved, refer to Publish a secure browser [section of the Citrix Doc](https://docs.citrix.com/en-us/citrix-cloud/secure-browser-service.html) to publish a Secure Browser app.

To enable URL Parameters for the published app:

*  In your Citrix Cloud subscription, click on the Secure Browser tile
*  On your published browser, e.g. “Browser”
*  Click on the three dots > select Policies

![Step 3.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_6.png)

*  Enable the URL Parameters policy on your published browser

![Step 3.4](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_7.png)

### Section 2: Set up ADC as proxy to route the traffic from client browser to the Internet

**Step 1:** To Set-up the ADC

*  Go to All Resources and click on + Add button, search for Citrix ADC
*  Select Citrix ADC template
*  Select any software plan based on your requirements (e.g.  Bring Your Own License)
*  Click Create

![Step 1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_8.png)

**Step 2:** Configure NIC card for the ADC – ipconfig1:

*  Go to All Resources and select the NIC card for the ADC instance
*  Go to IP Configurations, make a note of the ADC management address
*  Enable IP Forwarding Settings, save the changes.

![Step 2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_9.png)

**Step 3:** Configure Virtual IP – virtualip

*  Click on Add, set virtualip as name of the new config
*  Select Static and add new IP address after the management address
*  Enable Public address option and create a new public IP address
*  Save the changes to create new IP configuration.

![Step 3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_10.png)

**Step 4:** Set the FQDN for the proxy settings on the client

*  Go to the Public IP address resource created for the virtualip configuration
*  Go to Configuration, add a DNS label (e.g., urlredirection.eastus.cloudapp.azure.com)

![Step 4](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_11.png)

**Step 5:** Set up Networking rules in the Azure portal for the created ADC:

*  Add the following Networking rules

![Step 5](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_12.png)

Note: You may choose to close the ports 22 and 443 after the configuration is done, as those ports are only needed for logging into management console for configuration purposes.

**Step 6:** At this point the ADC instance in Azure is set up

*  To continue with the configuration. Log in the management public IP address of the ADC instance by using the IP address of the machine you provisioned, e.g. `https://40.88.150.164/`
*  From the initial configuration screen, click Continue (we will configure the rest of the settings manually because we will not be using Subnet IP in the setup)

![Step 6](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_13.png)

**Step 7:** Upload the licenses:

*  Go to System > Licenses, click on Manage Licenses
*  Upload the necessary licenses for ADC. The licenses you bring should support the features highlighted in steps 9 and 10 under Configure Basic Features and Configure Advanced Features (e.g CNS_V3000_SERVER_PLT_Retail.lic, and CNS_WEBF_SSERVER_Retail.lic)
*  Reboot the server after uploading both licenses.

![Step 7](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_14.png)

**Step 8:** After reboot, login again to management

*  Go to System > Settings > Configure Modes
*  Disable Use Subnet IP
*  Only two options that should be enabled are Mac based forwarding and Path MTU Discovery:

![Step 8](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_15.png)

**Step 9:** System > Settings > Configure Basic Features

*  Select: (1) SSL Offloading, (2) Load Balancing, (3) Rewrite, (4) Authentication, Authorization and Auditing, (5) Content Switching, (6) Integrated Caching

![Step 9](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_16.png)

**Step 10:** System > Settings > Configure Advanced Features

*  Select: (1) Cache Redirection, (2) IPv6 Protocol Translation, (3) AppFlow, (4) Reputation, (5) Forward Proxy, (6) Content Inspection, (7) Responder, (8) URL Filtering, (9) SSL Interception:

![Step 10.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_17.png)

![Step 10.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_18.png)

**Step 11:** Set up the NTP Server:

*  Go to System > NTP Servers
*  Create a server and use pool.ntp.org
*  Enable NTP when prompted and set server to enabled:

![Step 11.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_19.png)

![Step 11.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_20.png)

**Step 12:** Save the Configuration from the management portal save action

![Step 12](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_21.png)

**Step 13:** Open SSH Session to ADC management address, login with credentials you used while provisioning the ADC from Azure

**Step 14:** Run the following commands with the sslproxy address i.e., virtualip:

*  Get the virtualip from step 3 and input in the command (e.g. 10.1.0.5)
*  To add TCP profile:

        add ns tcpProfile proxy-tcpprofile01 -dynamicReceiveBuffering ENABLED -KA ENABLED -mptcp ENABLED -mptcpDropDataOnPreEstSF ENABLED -mptcpSessionTimeout 360 -builtin MODIFIABLE

*  To add virtual server

        add cs vserver sslproxy01 PROXY 10.1.0.5 8080 -cltTimeout 360 -tcpProfileName proxy-tcpprofile01 -persistenceType NONE

        bind cs vserver sslproxy01 -lbvserver azurelbdnsvserver

        add netProfile proxy-netprofile01 -srcIP 10.1.0.5 -srcippersistency ENABLED -MBF ENABLED -proxyProtocol ENABLED -proxyProtocoltxversion V2

        set cs vserver sslproxy01 -netProfile proxy-netprofile01

        set ssl vserver sslproxy01 -sslProfile ns_default_ssl_profile_frontend

        save ns config

**Step 15:** Change the Cache settings:

*  Go back to management session on browser
*  Go to Optimization > Integrated Caching
*  Click on Settings > Change cache settings
*  Set Memory Usage Limit to 250 MB
*  Click OK:

![Step 15.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_22.png)

![Step 15.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_23.png)

**Step 16:** Setup the client:

*  On a client, e.g. Firefox in this example
*  Configure your browser proxy to virtualip Public IP or FQDN:8080 that you configured in step 4 (e.g., urlredirection.eastus.cloudapp.azure.com:8080)
*  Test any site connectivity:

![Step 16](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_24.png)

**Step 17:** Now that the above steps are configured, and we have an ADC set up let us test for any website connectivity from the browser with the ADC acting as a proxy.

### Section 3: Set up SSL Interception and Rewrite Policies/Actions for URL redirection to Secure Browser

References:

*  [SSL Interception](https://docs.citrix.com/en-us/citrix-adc/13/forward-proxy/ssl-interception.html)
*  [URL categories](https://docs.citrix.com/en-us/citrix-adc/13/forward-proxy/url-filtering-for-ssl-forward-proxy/url-categorization.html)
*  [Video example of configuration](https://citrix.sharefile.com/d-s74bb5855411495bb)

**Step 1:** Create an RSA Key

*  Traffic management > SSL > SSL Files > Keys > Create RSA Key

![Step 1.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_25.png)

![Step 1.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_26.png)

*  Once the key is created, download the .key file for later use

**Step 2:** Create a CSR:

*  Traffic management > SSL > SSL Files > CSRs > Create Certificate Signing Request (CSR)

![Step 2.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_27.png)

![Step 2.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_28.png)

*  Key Filename is the key created in the previous step, e.g. smesec_key1.key.
*  Select Appliance
*  Select the key filename

![Step 2.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_29.png)

After selecting the Key continue to fill in the blanks required *

*  Common Name, Organization Name, and State or Province
*  Click Create

**Step 3:** Create a Certificate:

*  Traffic management > SSL > SSL Files > Certificates > Create Certificate

![Step 3.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_30.png)

![Step 3.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_31.png)

*  Create a Certificate by giving it a name and choose both the Certificate Request File (e.g .req) and The Key Filename (e.g. .key) created in the previous steps
*  Click Create

![Step 3.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_32.png)

*  Once the certificate is created, download the .cert file for later use

**Step 4:** Create SSL INTERCEPT policy:

*  Traffic management > SSL > Policies
*  Click Add
*  Create the INTERCEPT policy:

Example:

*  Name: smesec_intercept
*  Action: INTERCEPT
*  Expression to intercept news:
`client.ssl.detected_domain.url_categorize(0,0).category.eq("News")`
*  Click Create

![Step 4.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_33.png)

Bind policy to vserver

*  Security > Proxy Virtual Servers
  
![Step 4.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_34.png)

*  Click on the virtual server > add SSL Policies > click on No SSL Policy Binding
*  Bind the intercept policy:

![Step 4.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_35.png)

**Step 5:** Create SSL BYPASS policy (this policy is created to bypass the ADC interception for traffic going to secure browser launch.cloud.com):

*  Traffic management > SSL > Policies
*  Click Add
*  Create the BYPASS policy:

Example:

*  Name: smesec_bypass_ssli
*  Action: NOOP (There is no BYPASS option, see next step)
Expression:
`CLIENT.SSL.DETECTED_DOMAIN.CONTAINS("cloud")`

![Step 5.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_36.png)

*  Security > SSL Forward Proxy > SSL Interception Policies

![Step 5.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_37.png)

*  Click on the policy to edit it
*  Change Action from NOOP to BYPASS
*  Click OK
*  Double check that the Action is now BYPASS

![Step 5.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_38.png)

*  Go back to Traffic management > SSL > Policies to double check the change

![Step 5.4](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_39.png)

Bind policy to vserver

*  Security > Proxy Virtual Servers

![Step 5.5](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_40.png)

Click on the virtual server > add SSL Policies > click on SSL Policy Binding

![Step 5.6](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_41.png)

Click add binding

![Step 5.7](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_42.png)

Bind the bypass policy:

![Step 5.8](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_43.png)

**Step 6:** Create SSL Profile and bind an SSL Interception CA Certificate to SSL Profile

*  System > Profiles > SSL Profile > Add

![Step 6.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_44.png)

*  Name the profile, e.g. smesec_swg_sslprofile

![Step 6.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_45.png)

*  Check the box to enable SSL Sessions Interception, then click ok

![Step 6.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_46.png)

*  Click ok to create SSL Profile
*  Need to install the cert-key pair
*  Before installing make sure you have a .pfx format of the cert-key pair (see next step for guidance on how to generate a .pfx file from the .cert and .key files that you previously downloaded in steps 1 and 3)

**Step 7:** Prepare cert-key pair with SSL Tool

*  [Install the SSL tool](https://slproweb.com/products/Win32OpenSSL.html)
*  Add the openssl installation path to the system environment variables

![Step 7.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_47.png)

*  From Powershell, run the command:
  
        openssl pkcs12 -export -out smesec_cert1.pfx -inkey smesec.key1.key -in smesec.cert1.cert

![Step 7.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_48.png)

**Step 8:** Bind an SSL Interception CA Certificate to SSL Profile

*  System > Profiles > SSL Profile
*  Go back to the profile you created in step 6
*  Click “+ Certificate Key”
*  Click Install
*  Choose the .pfx file prepared in step 7
*  Create a password (you will need it later)
*  Install

![Step 8](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_49.png)

**Step 9:** Bind SSL Profile to vserver

*  Security > SSL Forward Proxy > Proxy Virtual Servers
*  Select the vserver
*  Click to edit SSL Profile

![Step 9](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_50.png)

*  Choose the SSL profile created in step 6, e.g. smesec_swg_sslprofile
*  Done

**Step 10:** Create rewrite policy and action

*  AppExpert > Rewrite > Policy
*  Click Add

Expression:
`HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL).URL_CATEGORIZE(0,0).CATEGORY.EQ("News")`

*  Create

![Step 10](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_51.png)

Create the Action in PuTTy

*  Run the following command:

        add rewrite action cloud_act REPLACE_HTTP_RES q{"HTTP/1.1 302 Found" + "\r\n" + "Location: http://launch.cloud.com/<customername>/<appname>?url=https://" + HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL.PATH) + "\r\n\r\n\" "}

Note:
In the command replace `<customername>` with your Citrix Cloud customer account name and replace `<appname>` with the Secure Browser published app name for which the URL parameters policy is enabled. Referring to the published app you created in Section 1.

**Step 11:** Bind rewrite policy to vserver

*  Back to management
*  AppExpert > Rewrite > Policy
*  Go to the policy cloud_pol and change the action to cloud_act (the one just created)

![Step 11.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_52.png)

To choose the type:

*  Go to Security > SSL Forward Proxy > Proxy Virtual Servers
*  Select “+ Policies”
*  Choose type
*  Policy: Rewrite
*  Type: Response

![Step 11.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_53.png)

*  Select the policy created, e.g. cloud_pol
*  Priority: 10
*  Bind

![Step 11.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_54.png)

*  Done
*  Save configuration

**Step 12:** Bind Certificate key to Profile

*  Go to System > Profiles > SSL Profile
*  Select the profile created, e.g. smesec_swg_sslprofile

![Step 12.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_55.png)

*  Click on “+ Certificate Key”

![Step 12.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_56.png)

*  Select the certificate key, e.g. smesec_cert_overall

![Step 12.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_57.png)

*  Click Select
*  Click Bind
*  Click Done
*  Save configuration

**Step 13:** Import cert file to the browser + screenshot

*  Upload the cert into firefox (per our example with news)
*  Go to options in your browser of choice, e.g. Firefox
*  Search “certs” > click “View Certificates”

![Step 13.1](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_58.png)

*  In the Certificate Manager window click “Import…”

![Step 13.2](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_59.png)

*  Browse for your cert and click open, e.g. smesec_cert1.cert

![Step 13.3](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_60.png)

*  Input the password you created when making the cert
*  Your certificate authority should be installed properly

![Step 13.4](en-us-tech-zone\en-us\tech-zone\learn\media\poc-guides_secure-browser-adc-integration_61.png)

**Demo:**
News websites from local browser will be redirected to Secure Browser automatically. See the following [demo](https://citrix.sharefile.com/d-s7a540d5498c42a59)

## Summary

To learn more about Citrix Secure Browser ADC integration [Citrix web site](https://www.citrix.com/) and to learn more about its technical capabilities visit [Citrix Tech Zone](/en-us/tech-zone/).
