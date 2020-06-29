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

## Scope

This proof-of-concept guide will describe the following:

1.  Obtain Secure Browser Trial Account
1.  Set up ADC as proxy to route the traffic from client browser to the Internet
1.  Set up SSL Interception and Rewrite Policies/Actions for URL redirection to Secure Browser

## Deployment Steps

### Obtain Secure Browser Trial Account

[Reference](https://docs.citrix.com/en-us/citrix-cloud/secure-browser-service.html)

1.  Enter Username and Password Click Sign In. If your account manages more than one customer select the appropriate one

2.  Click Citrix Cloud on top of the page. Search for the Secure Browser Tile.

*  If you know who your account team is, please reach out to them to get the trial approved. If you are unsure who your account team is, please continue to next step.

(image)

3.  Click Request a Call
4.  

----

4.  Open `SSH Session` to ADC management address, login with credentials you used while provisioning the ADC from Azure, run the following commands with the `sslproxy address i.e., virtualip`

        add ns tcpProfile proxy-tcpprofile01 -dynamicReceiveBuffering ENABLED -KA ENABLED -mptcp ENABLED -mptcpDropDataOnPreEstSF ENABLED -mptcpSessionTimeout 360 -builtin MODIFIABLE

        add cs vserver sslproxy01 PROXY <private ip of vserver – the virtual ip created in step 6> 8080 -cltTimeout 360 -tcpProfileName proxy-tcpprofile01 -persistenceType NONE 

        bind cs vserver sslproxy01 -lbvserver azurelbdnsvserver 

        add netProfile proxy-netprofile01 -srcIP <private ip of vserver> -srcippersistency ENABLED -MBF ENABLED -proxyProtocol ENABLED -proxyProtocoltxversion V2 

        set cs vserver sslproxy01 -netProfile proxy-netprofile01 

        set ssl vserver sslproxy01 -sslProfile ns_default_ssl_profile_frontend 

        save ns config 

5.  Go back to management session on browser, go to `Optimization/Integrated Caching`, click on `Settings` -> Change cache settings, set `Memory Usage Limit` to 250 MB, click `OK`

6.  On a client, configure your browser proxy to `virtualip Public IP` or `FQDN:8080` that you configured in step 7 (i.e. for e.g., `redirection.westus.cloudapp.azure.com:8080`) for proxy settings and test any site connectivity.

7.  If you got step 18 working that means, you have the connections from browser a vserver (adc is serving as proxy) DNS Internet are good.

## SSL Interception and Rewrite policies/Actions for url redirection to Secure Browser Setup

For the url redirection to happen to secure browser based on the type of the url entered in the browser, SSL Interception needs to be set up in the ADC for it to be able to read the url and apply the corresponding policies/actions.  

Steps involved are:

a - Create a Cert (You will have to download this and import into your browser as well)

b - Create `SSL policies` and bind to `vserver`

c - Create `SSL Profile` and bind an `SSL Interception CA Certificate` to `SSL Profile`

d - Bind `SSL Profile` to `vserver`

e - Create rewrite policy and action

f - Bind rewrite policy to vserver

From the `ssh` session of ADC management console, run the command
enable feature `IC urlfiltering rewrite CS`

[Reference](https://docs.citrix.com/en-us/citrix-adc/13/forward-proxy/ssl-interception.html) or [Follow the process shown in this video for setting up steps (a) through (f)](https://citrix.sharefile.com/d-s74bb5855411495bb)

Supporting information for the video

*  *04:58 minute* `Action BYPASS rule` is needed for detected domain `"cloud"` to not intercept traffic for `launch.cloud.com` so as to make the secure browser session work after the redirection happens.  

*  *09:20 minute* `pfx format` of the `cert-key` pair needs to be uploaded. It can be prepared by [openssl tool](https://www.cloudinsidr.com/content/how-to-install-the-most-recent-version-of-openssl-on-windows-10-in-64-bit/) More information [here](https://stackoverflow.com/questions/6307886/how-to-create-pfx-file-from-certificate-and-private-key)
  
Policies used

*  `client.ssl.detected_domain.url_categorize(0,0).category.eq("News")`

*  `CLIENT.SSL.DETECTED_DOMAIN.CONTAINS("cloud")`

*  `HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL).URL_CATEGORIZE(0,0).CATEGORY.EQ("News")`

        set rewrite action cloud_act -target q{"HTTP/1.1 302 Found" + "\r\n" + "Location: http://launch.cloud.com/nagpottest5/browser?url=https://" + HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL.PATH) + "\r\n\r\n\" "

### Demo of the solution

Citrix ADC URL redirection to Secure Browser automatically. See demo video [here](https://citrix.sharefile.com/d-s7a540d5498c42a59)

Note: `launch.cloud.com/<customername>/<appname>`

For the `<appname>` e.g., ‘browser’ in the above example, enable ‘URL Parameters’ policy for the app which is currently in Tech Preview
