---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of nFactor for Citrix Gateway Authentication with Email OTP
---

# Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Email OTP

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** [Daniel Feller](https://twitter.com/djfeller)

## Introduction

Multifactor authentication is one of the best ways to verify identity and increase security posture. Email OTP is a convenient way to implement another factor using readily available email. It allows users to receive, copy, and paste authentication validation codes from their email client on any device.

Citrix Gateway supports Email OTP authentication, and can provide authentication for various services including web services, VPN, and Citrix Virtual Apps and Desktops. In this POC Guide we demonstrate using it for authentication in a Citrix Virtual Apps and Desktops environment.

![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-email-token_conceptualarchitecture.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP to validate Active Directory credentials as the first factor and use Email OTP as the second factor. It uses a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix Gateway installed, licensed, and configure with an externally reachable virtual server bound to a wildcard certificate
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  SMTP server access with the ability to login with username and password to originate emails
*  Endpoint with Citrix Workspace app installed
*  Active Directory (AD) is available in the environment

## Citrix Gateway

### Authentication policies

1.  Log in to the Citrix ADC UI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.

#### Email OTP policy

1.  Next navigate to `Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > Push service`
1.  Select Add
1.  Populate the following fields and click OK:
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_pushserviceaction.png)
    *  Name - a unique value
**We will enter values in the following fields to integrate with Citrix Cloud - PUSH Service**
    *  Log in to Citrix Cloud and navigate to **Identity and Access Management > API Access**
    *  Create a unique name for the push service and select create client
**Now we will copy and paste these values to our Citrix ADC policy to integrate with Citrix Cloud - PUSH Service**
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ccidandsecretpopup.png)
    *  Client ID - copy & paste the Client ID from the Citrix Cloud ID and secret popup
    *  Client Secret - copy & paste the Client ID from the Citrix Cloud ID and secret popup
    *  Select Close
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_cciamapiaccess.png)
    *  Customer ID - copy & paste the Client ID from the Citrix Cloud Identity and Access Management API Access page
1.  Click Create

#### LDAP - authentication policy

1.  Next navigate to `Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > LDAP`
1.  Select Add
1.  Populate the following fields
    *  Name - a unique value
    *  Server Name / IP address - select an FQDN or IP address for AD server/(s). We enter `192.168.64.50_LDAP`
    *  Base DN - enter the path to the AD user container. We enter `OU=Team Accounts, DC=workspaces, DC=wwco, DC=net`
    *  Administrator Bind DN - enter the admin/service account to query AD to authenticate users. We enter `wsrv@workspaces.wwco.net`
    *  Confirm / Administrator Password - enter / confirm the admin / service account password
    *  Server Logon Name Attribute - in the second field below this field enter `userPrincipalName`
1.  Select Create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapaction.png)
For more information see [LDAP authentication policies](/en-us/citrix-adc/13/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-ldap-tsk.html)

### nFactor

1.  Next navigate to `Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows`
1.  Select Add and select the plus sign in the Factor box
1.  Enter nFactor_OTP and select create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorotp.png)

#### nFactor - LDAP

1.  Select Add Policy and select Add again next to Select Policy
1.  Enter `authPol_OTPReg`
1.  Under Action Type select `NO_AUTHN`

#### nFactor - Email OTP

1.  Select Add Policy and select Add again next to Select Policy
1.  Enter `authPol_OTPReg`
1.  Under Action Type select `NO_AUTHN`

## User Endpoint

Now we test PUSH by registering a mobile device and authenticating into our Citrix Virtual Apps and Desktops environment.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway with /manageotp appended to the end of the FQDN. We use `https://citrixadc5.workspaces.wwco.net/manageotp`
1.  After your browser is redirected to a login screen enter user UPN and password
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regldaplogin.png)
1.  On the next screen select Add Device, enter a name. We use `iPhone7`
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regadddevice.png)
1.  Select Go and a QR code will appear
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regqrcodedisplay.png)
1.  On your mobile device open your Citrix SSO app which is available for download from apps stores
1.  Select Add New Token
1.  Select Scan QR Code
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssoqrcode.png)
1.  Select Aim your camera at the QR Code and once it`s captured select Add
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssoscanqrcode.png)
1.  Select Save to store the token
1.  The Token is now active and begins displaying OTP codes at 30 second intervals
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssotoken.png)
1.  Select Done and you will see confirmation that the device was added successfully
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_addeddevicesuccessfully.png)

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Users can get access to all of their Workspaces resources by entering their standard domain user and password and simply confirming their identity with Email OTP send to their email client.

## References

For more information refer to other nFactor authentication options:

[Authentication Push](/en-us/tech-zone/learn/tech-insights/authentication-push.html) – watch a Tech Insight video regarding the use of TOTP to improve authentication security for your Citrix Workspace

[Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) – watch a Tech Insight video regarding integrating with on-premises Citrix Gateway to improve authentication security for your Citrix Workspace
