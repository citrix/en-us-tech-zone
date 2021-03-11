---
layout: doc
h3InToc: true
contributedBy: Nagapandu Potti, Alejandra Roca
description: Learn how to provide the dynamic delivery of resources by redirecting browsing to a Secure Browser service protecting the company network without sacrificing user experience.
---
# Proof of Concept Guide: URL Redirection to Secure Browser with Citrix ADC in Azure

## Overview

Here are the configuration steps for setting up an ADC, configuring SSL Forward Proxy, and SSL Interception using the latest Citrix ADC marketplace template. The URL Redirection to Secure Browser capability of the ADC enables administrators to define specific website categories to be redirected from the local browser to Secure Browser automatically. The Citrix ADC acts as an intermediate proxy to do the interception between local browsing and the internet, thus achieving web isolation and protecting the corporate network. This capability increases security without compromising user experience.

## Conceptual Architecture

[![URL redirection to Secure Browser Service Architecture](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_1.png)](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_1.png)

## Scope

This proof-of-concept guide describes the following:

1.  Obtain Secure Browser Trial Account
1.  Set up ADC in Azure
1.  Set up Citrix ADC appliance as proxy
1.  Set up SSL Interception
1.  Set up Rewrite Policy and Actions

## Deployment Steps

### Section 1: Obtain Secure Browser Trial Account

[Reference doc for Secure Browser Service](/en-us/citrix-cloud/secure-browser-service.html)

#### Request a Secure Browser trial

1.  Navigate to your Citrix Cloud account and enter user name and password

1.  Click Sign In. If your account manages more than one customer select the appropriate one

    ![Log in to Citrix Cloud](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_2.png)

1.  Double-click the **Secure Browser Tile.**

    ![Secure Browser Tile](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_3.png)

1.  If you know who your account team is, then reach out to them to get the trial approved. If you are unsure who your account team is, then continue to the next step.

1.  Click **Request a Call**

    ![Request a Call](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_4.png)

1.  Enter your details and in the **Comments** section specify **“Secure Browser service trial.”**

1.  Click **Submit**.

    ![Request a Call form](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_5.png)

    >**Note:**
    >
    >Citrix Sales will contact you to give you access to the service. This is not immediate, a Citrix sales rep will reach out

1.  Once you have the Secure Browser trial approved, refer to the **Publish a Secure Browser** [section of the Citrix Doc](/en-us/citrix-cloud/secure-browser-service.html) to publish a Secure Browser app.

#### Enable URL Parameters

1.  In your Citrix Cloud subscription, double-click the **Secure Browser** tile

1.  On your published browser, called “browser” in this example, click the three dots and select **Policies**

    ![published browser app](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_6.png)

1.  Enable **URL Parameters policy** on your published browser

    ![URL Parameters policy enable](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_7.png)

### Section 2: Set up ADC in Azure

The ADC can be set up in any cloud of choice. In this example Azure is our Cloud of choice.

#### Configure an ADC instance

1.  Navigate to **All Resources** and click **+ Add** button, search for Citrix ADC

1.  Select **Citrix ADC template**

1.  Select the software plan according to your requirements (in this example Bring Your Own License)

1.  Click **Create**

    ![Set up ADC in Azure](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_8.png)

#### Configure NIC Card

1.  Navigate to **All Resources** and select the NIC card for the ADC instance

1.  Select **IP Configurations**, make a note of the **ADC management address**

1.  Enable IP Forwarding Settings, save the changes.

    ![Configure NIC for ADC](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_9.png)

#### Configure Virtual IP

1.  Click **Add**, set `virtualip` as the name of the new config

1.  Select **Static** and add new IP address after the management address

1.  Enable Public address option and create a new public IP address

1.  Save the changes

    ![Configure Virtual IP](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_10.png)

#### Set up the FQDN on the client

1.  Navigate to the Public IP address resource created for the `virtualip` configuration

1.  Click **Configuration**, and add a DNS label (in this example,   `urlredirection.eastus.cloudapp.azure.com`)

    ![Set FQDN](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_11.png)

#### Set up Networking rules

1.  Add the following Networking rules

    ![Networking rules](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_12.png)

    >**Note:**
    >
    >You may choose to close the ports 22 and 443 after the configuration is done, as those ports are only needed for logging into management console for configuration purposes.

1.  At this point **the ADC instance in Azure is set up**

### Section 3: Set up Citrix ADC appliance as proxy

Set up the ADC as a proxy to route the traffic from the client browser to the Internet.

#### Log in to ADC management console

1.  Navigate to the Citrix ADC management console by inputting the instance's public IP address in the search bar of your browser  

    >**Note:**
    >
    >Use the IP address of the machine you provisioned in the previous steps, in this example `https://40.88.150.164/`

1.  Log in to the console by inputting the user name and password you set up in the previous steps

    ![Log in to management console](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_13.png)

1.  From the initial configuration screen, click **Continue**

#### Upload the licenses

1.  Navigate to **System > Licenses > Manage Licenses**

1.  Upload the necessary licenses for ADC.

    >**Note:**
    >
    >The licenses you bring must support the features highlighted in the steps 11 and 13 under Configure Basic Features and Configure Advanced Features (e.g CNS_V3000_SERVER_PLT_Retail.lic, and CNS_WEBF_SSERVER_Retail.lic)

    ![Manage licenses](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_14.png)

1.  **Reboot** the server after uploading both licenses.

1.  After reboot, log in to the management again

1.  Navigate to **System > Settings > Configure Modes**

1.  Only two options must be enabled **Mac based forwarding** and **Path MTU Discovery**

    ![Configure Modes](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_15.png)

    ![Configure Modes](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_15-1.png)

1.  Navigate to **System > Settings > Configure Basic Features**

    ![Configure Basic Features](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_16.png)

1.  Select: `SSL Offloading`, `Load Balancing`, `Rewrite`, `Authentication, Authorization, and Auditing`, `Content Switching`, and `Integrated Caching`

    ![Configure Basic Features](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_16-1.png)

1.  Navigate to **System > Settings > Configure Advanced Features**

    ![Configure Advanced Features](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_17.png)

1.  Select: `Cache Redirection`, `IPv6 Protocol Translation`, `AppFlow`, `Reputation`, `Forward Proxy`, `Content Inspection`, `Responder`, `URL Filtering`, and `SSL Interception`

    ![Configure Advanced Features](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_17-1.png)

#### Set up the NTP Server

1.  Navigate to **System > NTP Servers > Add**

    ![Set up NTP Server](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_18.png)

1.  Create a server for example `pool.ntp.org`

    ![Set up NTP Server](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_19.png)

1.  Enable NTP when prompted and set server to enabled

    ![Set up NTP Serve](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_20.png)

1.  Save the Configuration from the management portal save action

    ![Save configuration](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_21.png)

1.  Open SSH Session to ADC management address, log in with credentials you used while provisioning the ADC from Azure

#### Set up TCP Profile and vServer

1.  Get the `virtualip` from the steps in Section 2 and input in the command (in this example 10.1.0.5)

1.  Run the following commands with the `sslproxy` address for example, `virtualip`:

1.  To add TCP profile:

    ```
    add ns tcpProfile proxy-tcpprofile01 -dynamicReceiveBuffering ENABLED -KA ENABLED -mptcp ENABLED -mptcpDropDataOnPreEstSF ENABLED -mptcpSessionTimeout 360 -builtin MODIFIABLE
    ```

1.  To add virtual server

    ```
    add cs vserver sslproxy01 PROXY 10.1.0.5 8080 -cltTimeout 360 -tcpProfileName proxy-tcpprofile01 -persistenceType NONE

    bind cs vserver sslproxy01 -lbvserver azurelbdnsvserver

    add netProfile proxy-netprofile01 -srcIP 10.1.0.5 -srcippersistency ENABLED -MBF ENABLED -proxyProtocol ENABLED -proxyProtocoltxversion V2

    set cs vserver sslproxy01 -netProfile proxy-netprofile01

    set ssl vserver sslproxy01 -sslProfile ns_default_ssl_profile_frontend

    save ns config
    ```

1.  To change the **Cache settings** go back to management session on browser

1.  Navigate to **Optimization > Integrated Caching**

1.  Navigate to **Settings > Change cache settings**

    ![Change Cache settings](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_22.png)

1.  Set **Memory Usage Limit** to `250 MB` and click **OK**

    ![Memory usage limit](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_23.png)

#### Set up the client for URL Redirection

1.  On a client, for example Firefox

1.  Configure your browser proxy to `virtualip`, Public IP, or FQDN: 8080 that you configured in Section 2 (for example, `urlredirection.eastus.cloudapp.azure.com:8080`)

    ![Configure Browser proxy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_24.png)

1.  Now that we have an ADC set up, test for any website connectivity from the browser with the ADC acting as a proxy.

### Section 4: Set up SSL Interception

SSL interception uses a policy that specifies which traffic to intercept, block, or allow. Citrix recommends that you configure one generic policy to intercept traffic and more specific policies to bypass some traffic.

References:

[SSL Interception](/en-us/citrix-adc/current-release/forward-proxy/ssl-interception.html)

[URL categories](/en-us/citrix-adc/current-release/forward-proxy/url-filtering-for-ssl-forward-proxy/url-categorization.html)

[Video example of configuration](https://citrix.sharefile.com/d-s74bb5855411495bb)

#### Create an RSA Key

1.  Navigate to **Traffic management > SSL > SSL Files > Keys**

1.  Select **Create RSA Key**

    ![Create RSA Key](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_25.png)

1.  Select the key file name and required key size

    ![Create RSA Key](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_26.png)

1.  Once the key is created, download the `.key` file for later use

    ![Create RSA Key](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_26-1.png)

#### Create a Certificate Signing Request (CSR)

1.  Navigate to **Traffic management > SSL > SSL Files > CSRs > Create Certificate Signing Request (CSR)**

    ![CSR](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_27.png)

1.  Name the request file, for example `semesec_req1.req`

    ![CSR creation](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_28-1.png)

1.  Click **Key Filename > Appliace** the key file name is the one created in the previous step, in this example `smesec_key1.key`

    ![CSR creation](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_28-2.png)

1.  After selecting the Key continue to fill in the blanks required: Common Name, Organization Name, and State or Province

1.  Click Create

#### Create a Certificate

1.  Navigate to **Traffic management > SSL > SSL Files > Certificates > Create Certificate**

    ![Create certificate](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_29.png)

1.  Give the certificate a name and choose both the Certificate Request File (`.req`) and the Key File name (`.key`) created in the previous steps

    ![Create Certificate](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_30.png)

1.  Click Create

1.  Once the certificate is created, download the `.cert` file for later use

    ![Create Certificate](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_30-1.png)

#### Create SSL INTERCEPT policy

1.  Navigate to **Traffic management > SSL > Policies**

1.  Click Add

    ![Create SSL Policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_31.png)

1.  Give the policy a name and select the INTERCEPT action

1.  Expression to intercept news:

    `client.ssl.detected_domain.url_categorize(0,0).category.eq("News")`

1.  Click Create

    ![Create SSL Intercept](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_32.png)

1.  To bind the Intercept policy to the virtual server navigate to **Security > SSL Forward Proxy > Proxy Virtual Servers**
  
    ![SSL proxy01](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_34.png)

1.  Select the virtual server, in this example `sslproxy01`

1.  Select **add SSL Policies** and click **No SSL Policy Binding**

1.  Bind the intercept policy:

    ![Bind Intercept policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_35.png)

#### Create SSL BYPASS policy

1.  Navigate to **Traffic management > SSL > Policies**

1.  Click Add

    ![Create SSL Policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_31.png)

1.  Give the policy a name and select the NOOP action - there is no BYPASS option, see next step

1.  Expression to bypass policy:
`CLIENT.SSL.DETECTED_DOMAIN.CONTAINS("cloud")`

    ![Create bypass policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_36.png)

1.  Navigate to **Security > SSL Forward Proxy > SSL Interception Policies**

    ![SSL bypass policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_37.png)

1.  Select the policy to edit it

1.  Change Action from NOOP to BYPASS

1.  Click OK

    ![SSL bypass policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_37-1.png)

1.  Double check that the Action is now BYPASS

1.  Go back to **Traffic management > SSL > Policies** to double check the change

    ![Bypass policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_39.png)

1.  To bind the Bypass policy to the virtual server navigate to **Security > SSL Forward Proxy > Proxy Virtual Servers**
  
    ![SSL proxy01](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_34.png)

1.  Double-click the virtual server, in this example `sslproxy01`

1.  Select **add SSL Policies** and click **SSL Policy Binding**

1.  **Bind the bypass policy > Add**

    ![Step 5.7](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_42.png)

1.  Click Bind

    ![Step 5.8](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_43.png)

    >**Note:**
    >
    >This policy is created to bypass the ADC interception for traffic going to secure browser `launch.cloud.com`

#### Create SSL Profile

1.  Navigate to **System > Profiles > SSL Profile > Add**

    ![Step 6.1](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_44.png)

1.  Create the profile by giving it a name, in this example `smesec_swg_sslprofile`

    ![SSL profile name](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_45.png)

1.  Check the box to enable SSL Sessions Interception, then click OK

    ![Step 6.3](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_46.png)

1.  Click OK to create SSL Profile

1.  Must install the cert-key pair

1.  Make sure you have a `.pfx` format of the cert-key pair before. See the following step for guidance on how to generate a `.pfx` file from the `.cert` and `.key` files that you previously downloaded.

#### Prepare cert-key pair

1.  Start by [installing the SSL tool](https://slproweb.com/products/Win32OpenSSL.html)

1.  Add the `openssl` installation path to the system environment variables

    ![Path of SSL install](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_47.png)

1.  From PowerShell, run the command:

    `openssl pkcs12 -export -out smesec_cert1.pfx -inkey smesec.key1.key -in smesec.cert1.cert`

    ![PowerShell screenshot](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_48.png)

#### Bind an SSL Interception CA Certificate to the SSL Profile

1.  Navigate to **System > Profiles > SSL Profile**

1.  Select the profile created previously

1.  Click **+ Certificate Key**

1.  Click Install

1.  Choose the .pfx file prepared previously

1.  Create a password (you need it later)

1.  Click Install

    ![Step 8](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_49.png)

#### Bind the SSL Profile to the virtual server

1.  Navigate to **Security > SSL Forward Proxy > Proxy Virtual Servers**

    ![SSL proxy01](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_34.png)

1.  Select the virtual server, in this example `sslproxy01`

1.  Click to edit SSL Profile

    ![Edit SSL profile](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_50.png)

1.  Choose the SSL profile created in previously, in this example `smesec_swg_sslprofile`

1.  Done

### Section 5: Set up Rewrite Policies and Actions

A rewrite policy consists of a rule and action. The rule determines the traffic on which rewrite is applied and the action determines the action to be taken by the Citrix ADC. The rewrite policy is necessary for URL redirection to happen to Secure Browser based on the category of the URL entered in the browser, in this example "News".

[Reference](/en-us/citrix-adc/current-release/appexpert/rewrite.html)

#### Create rewrite policy and action

1.  Navigate to **AppExpert > Rewrite > Policy**

1.  Click Add

    ![Create rewrite policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_51-1.png)

1.  Create the policy by naming it, cloud_pol in this example and use the expression:
        `HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL).URL_CATEGORIZE(0,0).CATEGORY.EQ("News")`

1.  Click create

    ![Create rewrite policy](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_51.png)

1.  Create the Action in PuTTy

1.  Run the following command:

    `add rewrite action cloud_act REPLACE_HTTP_RES q{"HTTP/1.1 302 Found" + "\r\n" + "Location: https://launch.cloud.com/<customername>/<appname>?url=https://" + HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL.PATH) + "\r\n\r\n\" "}`

    >**Note:**
    >
    >In the command replace `<customername>` with your Citrix Cloud customer account name and replace `<appname>` with the Secure Browser published app name for which the URL parameters policy is enabled. Referring to the published app you created in Section 1.

#### Bind rewrite policy to virtual server

1.  Back to the ADC management console

1.  Navigate to **AppExpert > Rewrite > Policy**

1.  Go to the policy cloud_pol and change the action to cloud_act (the one created previously)

    ![cloud_act Action](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_52.png)

1.  To choose the type of the rewrite policy navigate to **Security > SSL Forward Proxy > Proxy Virtual Servers**

1.  Select “+ Policies”

1.  Policy: Rewrite

1.  Type: Response

    ![Step 11.2](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_53.png)

1.  Select the policy created, in this example `cloud_pol`

1.  Priority: 10

1.  Bind

    ![Step 11.3](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_54.png)

1.  Click done

1.  Save configuration

#### Bind Certificate key to Profile

1.  Navigate to **System > Profiles > SSL Profile**

1.  Select the profile created, for example `smesec_swg_sslprofile`

1.  Double-click **+ Certificate Key**

    ![Step 12.2](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_56.png)

1.  Select the certificate key, for example `smesec_cert_overall`

    ![Step 12.3](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_57.png)

1.  Click Select
1.  Click Bind
1.  Click Done
1.  Save configuration

#### Import the certificate file to the browser

1.  Upload the certificate into firefox (per our example with News category websites)

1.  Go to **Options** in your browser of choice, Firefox in this example

1.  Search **“certs” > click “View Certificates”**

    ![Step 13.1](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_58.png)

1.  In the Certificate Manager window click “Import…”

    ![Step 13.2](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_59.png)

1.  Browse for your cert and click open, `smesec_cert1.cert` in this example

    ![Step 13.3](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_60.png)

1.  Input the password you created when making the certificate

1.  Your certificate authority must be installed properly

    ![Step 13.4](/en-us/tech-zone/learn/media/poc-guides_secure-browser-adc-integration_61.png)

#### Demo

News websites from the local browser are redirected to Secure Browser automatically. See the following [demo](https://citrix.sharefile.com/d-s7a540d5498c42a59)

## Summary

In this PoC guide, you have learned how to set up Citrix ADC in Azure and Configure SSL Forward Proxy and SSL Interception. This integration allows the dynamic delivery of resources by redirecting browsing to Secure Browser service. Thus, protecting the company network without sacrificing user experience.
