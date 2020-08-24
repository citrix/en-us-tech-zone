---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of nFactor for Citrix Gateway Authentication with Device Certificate
---
# Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Device Certificate

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** [Daniel Feller](https://twitter.com/djfeller)

## Introduction

Device Certificates may be securely issued to a variety of types of mobile devices to provide an additional security factor reliably. Once issued the Citrix Gateway can seamlessly validate the certificate maintaining a good user experience while increasing security posture.

![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_conceptualarchitecture.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP to validate Active Directory credentials as the first factor and use Device Certificate as the second factor. It uses a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix Gateway installed, licensed, and configure with an externally reachable virtual server bound to a wildcard certificate.
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Citrix Cloud account established
*  Endpoint with Citrix Workspace app installed
*  Active Directory (AD) is available in the environment

## Citrix Gateway

### nFactor

1.  Log in to the Citrix ADC UI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.

### LDAP - authentication policy

1.  Next navigate to 'Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > LDAP'
1.  Select Add
1.  Populate the following fields
    *  Name - a unique value
    *  Server Name / IP address - select an FQDN or IP address for AD server/(s). We enter '192.168.64.50_LDAP'
    *  Base DN - enter the path to the AD user container. We enter 'OU=Team Accounts, DC=workspaces, DC=wwco, DC=net'
    *  Administrator Bind DN - enter the admin/service account to query AD to authenticate users. We enter 'workspacesserviceaccount@workspaces.wwco.net'
    *  Confirm / Administrator Password - enter / confirm the admin / service account password
    *  Server Logon Name Attribute - in the second field below this field enter 'userPrincipalName'
1.  Select Create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_ldapaction.png)
For more information see [LDAP authentication policies](/en-us/citrix-adc/13/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-ldap-tsk.html)
/*:
UPDATE HERE
*/

### Device Certificate - authentication policy

1.  Next navigate to 'Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Actions > CERT'
/*:
UPDATE HERE
*/

### nFactor

1.  Next navigate to 'Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows'
1.  Select Add and select the plus sign in the Factor box
1.  Enter nFactor_OTP and select create
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorotp.png)
/*:
UPDATE HERE
*/

#### nFactor - Ldap Flow

1.  Select Add Policy and select Add again next to Select Policy
1.  Enter 'authPol_Ldap'
1.  Under Action Type select 'NO_AUTHN'
1.  Select Expression Editor and build the expression by selecting the following in the drop-down menus offered:
    *  'HTTP'
    *  'REQ'
/*:
UPDATE HERE
*/

#### nFactor - Cert Flow

1.  Select the blue plus sign under the 'authPol_OTPReg' policy
1.  Enter 'authPol_Cert'
1.  Under Action Type select 'NO_AUTHN'
1.  Under Expression enter true
1.  Select Create
1.  Select the green plus sign next to the 'authPol_OTPAuth' policy to create a factor
1.  Enter 'AuthCert'
1.  Select Create
1.  In the box created select Add Schema
1.  Select Add and enter 'lschema_DualAuthOTP'
1.  Under Schema Files navigate to LoginSchema, and select 'DualAuthPushOrOTP.xml'
1.  Select the blue select button, followed by Create, followed by Ok
1.  In the same box select Add Policy
1.  Select the policy we created during the setup of the Registration flow that maps to your first LDAP authentication action. We use 'authPol_LDAP'
1.  Select Add
1.  Select the green plus sign next to the 'authPol_Ldap' policy to create a factor
1.  Enter 'OTPAuthDevice' **This Factor will use the OTP token to perform the 2nd factor authentication**
1.  Select Create
1.  In the same box select Add Policy
1.  Select the policy 'authPol_OTPAuthDevice' that we created during setup of the Registration flow
1.  Select Add
1.  Now we've completed the nFactor flow setup and can click Done
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_nfactorflow.png)
/*:
UPDATE HERE
*/

### Citrix ADC Authentication, Authorization,and Auditing (Citrix ADC AAA) virtual server

1.  Next navigate to 'Security > AAA - Application Traffic > Virtual Servers' and select Add
1.  Enter the following fields and click Ok:
    *  Name - a unique value
    *  IP Address Type - 'Non Addressable'
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_aaavserver.png)
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Select No nFactor Flow
1.  Under Select nFactor Flow click the right arrow, select the 'nFactor_OTP' flow created earlier
1.  Click Select, followed by Bind
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_authenticationvserver.png)
/*:
UPDATE HERE
*/

### Citrix Gateway - virtual server

1.  Next navigate to 'Citrix Gateway > Virtual Servers'
1.  Select your existing virtual server that provides proxy access to your Citrix Virtual Apps and Desktops environment
1.  Select Edit
1.  Under Basic Authentication - Primary Authentication select LDAP Policy
1.  Check the policy, select Unbind, select Yes to confirm, and select Close
1.  Under the Advanced Settings menu on the right select Authentication Profile
1.  Select Add
1.  Enter a name.  We enter 'PUSH_auth_profile'
1.  Under Authentication virtual server click the right arrow, and select the Citrix ADC AAA virtual server we created 'PUSH_Auth_Vserver'
1.  Click Select, and Create
1.  Click Ok and verify the virtual server now has an authentication profile selected while the basic authentication policy has been removed
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_gatewayvserver.png)
1.  Click Done
/*:
UPDATE HERE
*/

## User Endpoint

Now we test PUSH by registering a mobile device and authenticating into our Citrix Virtual Apps and Desktops environment.

### Deploy Device Certificate

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway 'https://citrixadc5.workspaces.wwco.net'
1.  After your browser is redirected to a login screen enter user UPN
/*:
UPDATE HERE
*/

### Citrix Virtual Apps and Desktops Authentication, Publication, and Launch

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use 'https://citrixadc5.workspaces.wwco.net'
1.  After the your browser is redirected to a login screen enter user UPN and password.
/*:
UPDATE HERE
*/

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multi-factor authentication without making the user experience complex. Users can get access to all of their Workspaces resources by entering their standard domain user and password and seamlessly have a certificate validated.

## References

For more information refer to:

[Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) – watch a Tech Insight video regarding integrating with On Premises Citrix Gateway to improve authentication security for your Citrix Workspace
