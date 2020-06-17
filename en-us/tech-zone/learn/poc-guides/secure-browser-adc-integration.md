---
layout: doc
description: Learn how to set up Citrix ADC in Azure and Configure SSL Forward Proxy and SSL Interception. This integration allows the dynamic delivery of resources by redirecting browsing to Secure Browser service providing a secury to the company network without sacrificing user experience.
---
# Proof of Concept Guide: URL Redirection to Secure Browser with Citrix ADC in Azure

## Contributors

**Author:** [Alejandra Roca](URL)

## Overview

Here are the configuration steps for setting up an ADC, configuring SSL Forward Proxy and SSL Interception using Citrix ADC 13.0 marketplace template. This capanility of the ADC on Azure enables new websites from local browser to be redirected to Secure Browser automatically, increasing security without compromising user experience.The Citrix ADC acts as an intermediate proxy to do the interception between local browsing and the internet. The SSL certificate is a contract between the browser and the ADC, that points to that specific ADC to intercept its traffic.

## Conceptual Architecture

## Step-by-Step Guide

## Citrix ADC on Azure Setup

For setting up ADC as proxy to route the traffic through it: (If you already have ADC setup, you can read steps 10, 12, 13, 18, 19 and skip to Section 2)

1.  Go to All Resources and click on + Add button, search for citrix ADC

1.  Select ‘Citrix ADC 13.0’ template, select Bring your own Licenses for software plan, click Create

1.  Once ADC is created go to All Resources and select the NIC card for the ADC instance

1.  Go to IP Configurations, make a note of the ADC management address.  

1.  Enable IP Forwarding Settings, save the changes.

1.  Click on Add, set virtualip as name of the new config, select Static and add new IP address after the management address; enable Public address option and create a new public IP address; save the changes to create new IP configuration.

1.  Go to the Public IP address resource created for the virtualip configuration, go to Configuration, add a DNS label (e.g., redirection) unique for the group, this will be used as the FQDN for the proxy settings on the client

1.  Add the following Networking rules in the Azure portal for the ADC.[insert image]

1.  Login to the management public ip address of the ADC instance; from the initial configuration screen, click Continue (we will configure the rest of the settings manually because we will not be using Subnet IP in the setup)

1.  Go to System/Licenses, click on Manage Licenses and upload the [two licenses](https://citrix.sharefile.com/d-sc0719582db546e28) and reboot the server after uploading both licenses. The licenses you bring should support the features highlighted in bold below.  

1.  Login again to management, go to System/Settings/Configure Modes; disable Use Subnet IP, only two options that should be enabled are Mac based forwarding and Path MTU Discovery

1.  Go to Configure Basic Features and select SSL Offloading, Load Balancing, Integrated Caching, Authentication, Authorization and Auditing.  

1.  Go to Configure Advanced Features and select Cache Redirection, AppFlow, URL Filtering, SSL Interception, Responder, Reputation, Forward Proxy, IPv6 Protocol Translation and Content Inspection.  

1.  Go to System/NTP Servers, create a server and use pool.ntp.org, enable NTP when prompted and set server to enabled

1.  Save the Configuration from the management portal save action

1.  Open SSH Session to ADC management address, login with credentials you used while provisioning the ADC from Azure, run the following commands with the sslproxy address i.e., virtualip

        add ns tcpProfile proxy-tcpprofile01 -dynamicReceiveBuffering ENABLED -KA ENABLED -mptcp ENABLED -mptcpDropDataOnPreEstSF ENABLED -mptcpSessionTimeout 360 -builtin MODIFIABLE

        add cs vserver sslproxy01 PROXY <private ip of vserver – the virtual ip created in step 6> 8080 -cltTimeout 360 -tcpProfileName proxy-tcpprofile01 -persistenceType NONE 

        bind cs vserver sslproxy01 -lbvserver azurelbdnsvserver 

        add netProfile proxy-netprofile01 -srcIP <private ip of vserver> -srcippersistency ENABLED -MBF ENABLED -proxyProtocol ENABLED -proxyProtocoltxversion V2 

        set cs vserver sslproxy01 -netProfile proxy-netprofile01 

        set ssl vserver sslproxy01 -sslProfile ns_default_ssl_profile_frontend 

        save ns config 

1.  Go back to management session on browser, go to Optimization/Integrated Caching, click on Settings -> Change cache settings, set Memory Usage Limit to 250 MB, click OK

1.  On a client, configure your browser proxy to virtualip Public IP or FQDN:8080 that you configured in step 7 (i.e. for e.g., redirection.westus.cloudapp.azure.com:8080) for proxy settings and test any site connectivity.

1.  If you got step 18 working that means, you have the connections from browser a vserver (adc is serving as proxy) DNS Internet are good.

## SSL Interception and Rewrite policies/Actions for url redirection to Secure Browser Setup

For the url redirection to happen to secure browser based on the type of the url entered in the browser, SSL Interception needs to be set up in the ADC for it to be able to read the url and apply the corresponding policies/actions.  

Steps involved are:

a - Create a Cert (You will have to download this and import into your browser as well)

b - Create SSL policies and bind to vserver

c - Create SSL Profile and bind an SSL Interception CA Certificate to SSL Profile

d - Bind SSL Profile to vserver

e - Create rewrite policy and action

f - Bind rewrite policy to vserver

From the ssh session of ADC management console, run the command
enable feature IC urlfiltering rewrite CS

[Reference](https://docs.citrix.com/en-us/citrix-adc/13/forward-proxy/ssl-interception.html) or [Follow the process shown in this video for setting up steps (a) through (f)](https://citrix.sharefile.com/d-s74bb5855411495bb)

Supporting information for the video

*  *04:58 minute* Action BYPASS rule is needed for detected domain “cloud” to not intercept traffic for launch.cloud.com so as to make the secure browser session work after the redirection happens.  

*  *09:20 minute* pfx format of the cert-key pair needs to be uploaded. It can be prepared by [openssl tool](https://www.cloudinsidr.com/content/how-to-install-the-most-recent-version-of-openssl-on-windows-10-in-64-bit/)
        * [More information here](https://stackoverflow.com/questions/6307886/how-to-create-pfx-file-from-certificate-and-private-key)
  
Policies used

*  client.ssl.detected_domain.url_categorize(0,0).category.eq("News")

*  CLIENT.SSL.DETECTED_DOMAIN.CONTAINS("cloud")

*  HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL).URL_CATEGORIZE(0,0).CATEGORY.EQ("News")

        {
            set rewrite action cloud_act -target q{"HTTP/1.1 302 Found" + "\r\n" + "Location: http://launch.cloud.com/nagpottest5/browser?url=https://" + HTTP.REQ.HOSTNAME.APPEND(HTTP.REQ.URL.PATH) + "\r\n\r\n\" "} 
        }

## Demo of the solution

Citrix ADC URL redirection to Secure Browser automatically. See demo video [here](https://citrix.sharefile.com/d-s7a540d5498c42a59)

Note: launch.cloud.com/`<customername>`/`<appname>`

For the `<appname>` e.g., ‘browser’ in the above example, enable ‘URL Parameters’ policy for the app which is currently in Tech Preview
