---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of nFactor for Native OTP 
---
# Proof of Concept Guide: nFactor For Citrix Gateway Authentication - Native OTP

## Contributors

**Author:** [Alyssa Ramella]

**Special Thanks:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

## Introduction

Implementing multifactor authentication is one of the best ways to verify identity and improve security posture. Native OTP is a convenient way to implement another factor using readily available authenticator applications. It allows users to enter validation codes from their authenticator application, into their gateway authentication form, to authenticate.

Citrix Gateway supports Native OTP authentication, and can provide authentication for various services including web services, VPN, and Citrix Virtual Apps and Desktops. In this POC Guide we demonstrate using it for authentication in a Citrix Virtual Apps and Desktops environment.

### Conceptual Architecture

![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_registrationflow.png)

![Native OTP Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authenticationflow.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP to validate Active Directory credentials as the first factor.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix Gateway installed, licensed, and configure with an externally reachable virtual server bound to a wildcard certificate.
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Endpoint with Citrix Workspace app installed
*  Mobile device with Citrix SSO app installed (or other authenticator app that supports Time Based Passwords)
*  Active Directory (AD) is available in the environment

Refer to Citrix Documentation for the latest product version and license requirements: [Native OTP Authentication](/en-us/citrix-adc/current-release/aaa-tm/native-otp-authentication.html)

## Citrix Gateway

### nFactor

1.  Log in to the Citrix ADC UI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.

### Native OTP Virtual Server

1.  Navigate to '**Security > AAA** - **Application Traffic > Virtual Servers**'
1.  Select 'Add'
1.  Create a new virtual server populating the fields as seen in the following image and click OK:
![Native OTP Virtual Server](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_vserver.png)
1.  Select 'No Server Certificate' then select and bind the domain certificate that was checked for in the steps above. Click continue.
1.  Click 'Continue' again.
1.  Under Advanced Settings on the right hand side, select '+ Portal Themes'
1.  Ensure 'RfWebUI' is the selected Portal Theme and click 'OK'
1.  Click 'Done'

## Native OTP Authentication Profile

1.  Navigate to '**Security > AAA-Application Traffic > Authentication Profile**'
1.  Select 'Add'
1.  Name your Authentication Profile and enter your Gateway URL as the authentication host. Click to Select the virtual server you made above as the Authentication Virtual Server. See the image below as an example.
![Native OTP Authentication Profile](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authpro.png)
1.  Click 'OK' to create the authentication Profile

### Edit Your Gateway

1.  Navigate to '**Citrix Gateway > Virtual Servers**'
1.  Select your current Gateway and click 'Edit'
1.  Change the Authentication Profile to the new profile you created. You may must add this from the Advanced Settings panel on the right hand side
1.  Click 'OK', then 'Done', and save the running configuration

## Create Policies

1.  Navigate to '**Security > AAA-Application Traffic > Policies > Authentication > Advanced Policies > Policy**'
1.  Click 'Add'
1.  Enter a name for your policy and change the Action Type to 'LDAP' as seen below
1.  Click 'Add' under Action
1.  Give your server a name, change server type to 'AD', and select the correct security type
![LDAP Server Name and Type](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authpolicy.png)
1.  Under Connection Settings enter your base dn, an administrator login, and the password
![LDAP Connection Settings](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_connectionsettings.png)
1.  Click Test Network Connectivity to ensure connection
1.  Under Other Settings set Server Logon Name Attribute to *sAMAccountName* and click 'Create'
1.  Enter the expression 'true' and click 'OK'
![Finished Authentication Policy](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authpol2.png)

We will now create the second authentication policy for validation.

1.  While you are in Authentication Policies, click 'Add' again
1.  Name this new Authentication Policy and select the Action Type 'LDAP'
1.  Select 'Add' under Action
1.  Enter your new LDAP server, this time deselecting the 'Authentication' check box as seen below
![Second LDAP Policy](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_secondpolicy.png)
1.  Fill out the Connection Settings section the same way you did previously
1.  Under Other Settings, change Server Logon Name Attribute to 'sAMAccountName' and then in the OTP Secret box enter 'userParameters'
![Second LDAP Policy Settings](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_secondpolicy2.png)
1.  Click 'OK', enter the expression 'true' and click 'OK' again

## Login Schemas

We will now create 2 Login Schemas.

1.  Navigate to '**Security > AAA-Application Traffic > Login Schema**'
1.  Click 'Add'
1.  Enter a name for your Login Schema
1.  Click 'Add' under Profile and give your Profile a name
1.  Click the pencil icon next to 'noschema'
![Login Schema Profile](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_loginschema.png)
1.  Click 'Login Schema' and scroll down to select 'DualauthOrOTPRegisterDynamic.xml' and select the blue 'Select' in the right corner.
![DualAuthOrOTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_dualauth.png)
1.  Click 'Create'
1.  Enter 'true' under Rule and click 'Create'
![Final Login Schema](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_finalschema.png)

Similarly, we will create a second login schema by clicking 'Add' again.

1.  Give this second login schema a name
1.  Click 'Add' under Profile. Give this Profile a name and then click the pencil icon next to 'noschema' as done previously
1.  Click 'LoginSchema' and scroll down to select 'SingleAuthManageOTP.xml'
![Second Login Schema](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_singleauth.png)
1.  Click the blue 'Select' button and then click 'Create'
1.  For this rule, we enter 'http.REQ.COOKIE.VALUE("NSC_TASS").eq("manageOTP"). Feel free to use the drop-down lists.
![Second Login Schema Final](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_finallogin.png)
1.  Click 'Create'

## Create and Bind Policy Labels

1.  Navigate back to '**Security > AAA-Application Traffic > Virtual Servers**'
1.  Select the server you created previously and click 'Edit'
1.  Under 'Advanced Authentication Policies', select 'No Authentication Policy'
1.  Click to select and choose the first authentication policy created and select 'Add'
1.  Under Binding Details, choose the '+' icon under 'Select Next Factor'
1.  Give your Authentication PolicyLabel a name and click 'Continue'
1.  Under Policy Binding, click 'Click to Select' and choose the second advanced authentication policy. In this example, it is called 'nOTP_validation'
1.  Click 'Bind', 'Done', then 'Bind" again for the original authentication policy

Next we bind our Login Schemas.

1.  Under 'Advanced Settings' on the right hand side, click Login Schemas to add it to your Authentication Virtual Server
1.  Under 'Login Schema' click 'No Login Schema'
1.  Click to Select the policy and choose your otp_management policy
![Bind Login Schema](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_binding.png)
1.  Click 'Bind'
1.  In a similar fashion, bind your other login schema

You must bind your login schema's in this order.

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Users can get access to all of their Workspaces resources by entering their standard domain user and password and simply confirming their identity with the push off a button in the Citrix SSO app on their mobile device.

## References

For more information refer to:

[Authentication Push](/en-us/tech-zone/learn/tech-insights/authentication-push.html) – watch a Tech Insight video regarding the use of TOTP to improve authentication security for your Citrix Workspace

[Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) – watch a Tech Insight video regarding integrating with on-premises Citrix Gateway to improve authentication security for your Citrix Workspace
