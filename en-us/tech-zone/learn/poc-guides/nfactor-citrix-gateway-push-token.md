---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks
specialThanksTo: Daniel Feller
description: Learn how to implement an extensible and flexible approach to configuring multifactor authentication with nFactor for Citrix Gateway authentication with Push Tokens.
---
# Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Push Token

## Introduction

Time Based One Time Passwords (TOTP) are an increasingly common method to provide an authentication that can increase security posture with other factors. TOTP with PUSH takes advantage of mobile devices by allowing users to receive and accept authentication validation requests at their fingertips. The exchange is secured by applying a hash to a shared key, distributed during setup.

Citrix Gateway supports push notifications for OTP and, can provide authentication for various services including web services, VPN, and Citrix Virtual Apps and Desktops. In this POC Guide we demonstrate using it for authentication in a Citrix Virtual Apps and Desktops environment.

![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_conceptualarchitecture.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP to validate Active Directory credentials as the first factor and use Citrix Cloud Push Authentication as the second factor. It uses a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix Gateway installed, licensed, and configure with an externally reachable virtual server bound to a wildcard certificate.
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Citrix Cloud account established
*  Endpoint with Citrix Workspace app installed
*  Mobile device with Citrix SSO app installed
*  Active Directory (AD) is available in the environment

Refer to Citrix Documentation for the latest product version and license requirements. [PUSH Authentication](/en-us/citrix-gateway/current-release/push-notification-otp.html)

## Citrix Gateway

### nFactor

1.  Log in to the Citrix ADC UI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/current-release/ssl/ssl-certificates.html) for more information.

### Push service action

1.  Next navigate to `Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > Push service`
1.  Select Add
1.  Populate the following fields and click OK:
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_pushserviceaction.png)
    *  Name - a unique value.
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

### LDAP - authentication action

1.  Next navigate to `Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > LDAP`
1.  Select Add
1.  Populate the following fields
    *  Name - a unique value
    *  Server Name / IP address - select an FQDN or IP address for AD server/(s). We enter `192.168.64.50_LDAP`
    *  Base DN - enter the path to the AD user container. We enter `OU=Team Accounts, DC=workspaces, DC=wwco, DC=net`
    *  Administrator Bind DN - enter the admin/service account to query AD to authenticate users. We enter `workspacesserviceaccount@workspaces.wwco.net`
    *  Confirm / Administrator Password - enter / confirm the admin / service account password
    *  Server Logon Name Attribute - in the second field below this field enter `userPrincipalName`
1.  Select Create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapaction.png)
For more information see [LDAP authentication policies](/en-us/citrix-adc/current-release/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-ldap-tsk.html)

### LDAP - token storage action

1.  Next navigate to `Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > LDAP`
1.  Select the LDAP action created above and select create
1.  Append OTP or any identifier to the name and unselect authentication
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapotppolicyunauth.png)
1.  Under Connection Settings verify the Base DN, Administrator Bind DN, and Password. **Be sure that the administrator user or service account is a member of domain administrators. This policy will be used to write the token registered by the user`s authenticator app in the userParameters attribute of their user object.**
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapotppolicyconnectionsettings.png)
1.  Scroll down to Other Settings
    *  OTP Secret - enter `userParameters`
    *  Push Service - select the PUSH service policy created above
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapotppolicyothersettings.png)
1.  Select Create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapactions.png)

### nFactor

1.  Next navigate to `Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows`
1.  Select Add and select the plus sign in the Factor box
1.  Enter nFactor_OTP and select create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorotp.png)

#### nFactor - Registration Flow

1.  Select Add Policy and select Add again next to Select Policy
1.  Enter `authPol_OTPReg`
1.  Under Action Type select `NO_AUTHN`
1.  Select Expression Editor and build the expression by selecting the following in the drop-down menus offered:
    *  `HTTP`
    *  `REQ`
    *  `COOKIE.VALUE(String) = NSC_TASS`
    *  `EQ(String) = manageotp`
1.  Select Done, followed by Create, followed by Add
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorauthpolotpreg.png)
1.  Select the green plus sign next to the authPol_OTPReg policy to create a factor
1.  Enter `OTPRegAD` and select Create
1.  In the box created select Add Schema
1.  Select Add and enter `lschema_SingleRegOTP`
1.  Under Schema Files navigate to LoginSchema, and select `SingleAuthManageOTP.xml`
1.  Select the blue select button, followed by Create, followed by OK
1.  In the same box select Add Policy and select Add again next to Select Policy
1.  Enter authPol_LDAP for the name
1.  Under Action Type select LDAP
1.  Under Action select your first LDAP authentication action. We use `192.168.64.50_LDAP`
1.  Under Expression enter true
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorauthpolldap.png)
1.  Select Create followed by Add
1.  Select the green plus sign next to the `authPol_LDAP policy` to create a factor
1.  Enter `OTPRegDevice` and select Create
1.  In the same box select Add Policy and select Add again next to Select Policy
1.  Enter `authPol_OTPAuthDevice` for the name
1.  Under Action Type select LDAP
1.  Under Action select your newly created (second) LDAP authentication action. We use `192.168.64.50_LDAP_OTP`
1.  Under Expression enter true
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorotppolldap.png)
1.  Select Create followed by Add

#### nFactor - Authentication Flow

1.  Select the blue plus sign under the `authPol_OTPReg` policy
1.  Enter `authPol_OTPAuth`
1.  Under Action Type select `NO_AUTHN`
1.  Under Expression enter true
1.  Select Create
1.  Select the green plus sign next to the `authPol_OTPAuth` policy to create a factor
1.  Enter `OTPAuthAD`
1.  Select Create
1.  In the box created select Add Schema
1.  Select Add and enter `lschema_DualAuthOTP`
1.  Under Schema Files navigate to LoginSchema, and select `DualAuthPushOrOTP.xml`
1.  Select the blue select button, followed by Create, followed by OK
1.  In the same box select Add Policy
1.  Select the policy we created during the setup of the Registration flow that maps to your first LDAP authentication action. We use `authPol_LDAP`
1.  Select Add
1.  Select the green plus sign next to the `authPol_Ldap` policy to create a factor
1.  Enter `OTPAuthDevice` **This Factor will use the OTP token to perform the 2nd factor authentication**
1.  Select Create
1.  In the same box select Add Policy
1.  Select the policy `authPol_OTPAuthDevice` that we created during setup of the Registration flow
1.  Select Add
1.  Now we`ve completed the nFactor flow setup and can click Done
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorflow.png)

### Citrix ADC Authentication, Authorization,and Auditing (Citrix ADC AAA) virtual server

1.  Next navigate to `Security > AAA - Application Traffic > Virtual Servers` and select Add
1.  Enter the following fields and click OK:
    *  Name - a unique value
    *  IP Address Type - `Non Addressable`
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_aaavserver.png)
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Select No nFactor Flow
1.  Under Select nFactor Flow click the right arrow, select the `nFactor_OTP` flow created earlier
1.  Click Select, followed by Bind
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_authenticationvserver.png)

### Citrix Gateway - virtual server

1.  Next navigate to `Citrix Gateway > Virtual Servers`
1.  Select your existing virtual server that provides proxy access to your Citrix Virtual Apps and Desktops environment
1.  Select Edit
1.  Under Basic Authentication - Primary Authentication select LDAP Policy
1.  Check the policy, select Unbind, select Yes to confirm, and select Close
1.  Under the Advanced Settings menu on the right select Authentication Profile
1.  Select Add
1.  Enter a name.  We enter `PUSH_auth_profile`
1.  Under Authentication virtual server click the right arrow, and select the Citrix ADC AAA virtual server we created `PUSH_Auth_Vserver`
1.  Click Select, and Create
1.  Click OK and verify the virtual server now has an authentication profile selected while the basic authentication policy has been removed
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_gatewayvserver.png)
1.  Click Done

## User Endpoint

Now we test PUSH by registering a mobile device and authenticating into our Citrix Virtual Apps and Desktops environment.

### Registration with Citrix SSO app

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
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssosave.png)
1.  The Token is now active and begins displaying OTP codes at 30 second intervals
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssotoken.png)
1.  Select Done and you will see confirmation that the device was added successfully
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_addeddevicesuccessfully.png)

### Citrix Virtual Apps and Desktops Authentication, Publication, and Launch

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use `https://citrixadc5.workspaces.wwco.net`
1.  After the your browser is redirected to a login screen enter user UPN and password. On this screen you see the option to Click to input OTP manually if for some reason your camera is not working
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regldaplogin1f.png)
1.  On your mobile device in your Citrix SSO app select OK to confirm PUSH authentication
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_regssoaccept.png)
1.  Verify the users virtual apps, and desktops are enumerated, and launch once logged in
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_cvadlogin.png)

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Users can get access to all of their Workspaces resources by entering their standard domain user and password and simply confirming their identity with the push off a button in the Citrix SSO app on their mobile device.

## References

For more information refer to:

[Authentication Push](/en-us/tech-zone/learn/tech-insights/authentication-push.html) – watch a Tech Insight video regarding the use of TOTP to improve authentication security for your Citrix Workspace

[Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) – watch a Tech Insight video regarding integrating with on-premises Citrix Gateway to improve authentication security for your Citrix Workspace
