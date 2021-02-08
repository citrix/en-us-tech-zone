---
layout: doc
h3InToc: true
contributedBy: Matthew Brooks, Alyssa Ramella
specialThanksTo: Himanshu Shukla
description: Learn how to implement an extensible and flexible approach to configuring multifactor authentication with nFactor for Citrix Gateway authentication with Native OTP.
---
# Proof of Concept Guide: nFactor for Citrix Gateway with Native OTP Authentication

## Introduction

Implementing multifactor authentication is one of the best ways to verify identity, and improve security posture. Native (time-based) One Time Password (OTP) is a convenient way to implement another factor using readily available authenticator applications. It allows users to enter validation codes from their authenticator application, into a gateway form, to authenticate.

Citrix Gateway supports Native OTP, and can provide authentication for various services including web services, VPN, and Citrix Virtual Apps and Desktops. In this POC Guide we demonstrate using it for authentication in a Citrix Virtual Apps and Desktops environment.

### Conceptual Architecture

![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_registrationflow.png)

![Native OTP Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authenticationflow.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP to validate Active Directory credentials as the first factor, and Native OTP as the second factor.

It makes assumptions about the completed installation, and configuration of the following components:

*  Citrix Gateway installed, licensed, and configured with an externally reachable virtual server bound to a wildcard certificate
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Endpoint with Citrix Workspace app installed
*  A supported Authenticator app, that supports Time Based OTP, installed (including Microsoft Authenticator, Google Authenticator, or Citrix SSO)
*  Active Directory (AD) is available in the environment

Refer to Citrix Documentation for the latest product version, and license requirements: [Native OTP Authentication](/en-us/citrix-adc/current-release/aaa-tm/native-otp-authentication.html)

## nFactor

### LDAP Policies

First we create two LDAP policies which we reference later when we are building our nFactor flow.

#### Native OTP Registration

This LDAP registration policy is used to exchange, and store the key used to generate the time based OTP code.

1.  Log in to the Citrix ADC UI
1.  Navigate to **Security > AAA-Application Traffic > Policies > Authentication > Advanced Policies > Policy**
1.  Click `Add`
1.  Enter `polldap_notpmanage` for the policy name, and change the Action Type to `LDAP`.
1.  Click `Add` under Action
1.  Populate the following fields:
    *  Name - enter `actldap_notpmanage`
    *  Server Name / IP address - select an FQDN or IP address for AD server/(s). We enter `192.168.64.50`
    *  Clear `Authentication` **This setting along with the OTP Secret below indicate the policy will set, rather than get, object attributes**
    *  Base DN - enter the path to the AD user container. We enter `DC=workspaces, DC=wwco, DC=net`
    *  Administrator Bind DN - enter the admin/service account to query AD to authenticate users. We enter `wsadmin@workspaces.wwco.net`
    *  Confirm / Administrator Password - enter / confirm the admin / service account password
    *  Click Test Network Connectivity to ensure connection
    *  Server Logon Name Attribute - in the second field below this field enter `userPrincipalName`
    *  OTP Secret - Enter `userParameters` **This is the User's LDAP object that will get updated with the key that`s used with hash to generate the time based OTP code**
1.  Select Create
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_actldap-notpmanage.png)
1.  Enter the expression `true`, and click `OK`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_polldap-notpmanage.png)

#### Native OTP Authentication

This LDAP authentication policy is used to do the first factor authentication.

1.  Navigate to `Security > AAA-Application Traffic > Policies > Authentication > Advanced Policies > Policy`
1.  Click `Add`
1.  Enter `polldap_notpauth` for the policy name, and change the Action Type to `LDAP`.
1.  Click `Add` under Action
1.  Populate the following fields:
    *  Name - enter `actldap_notpauth`
    *  Server Name / IP address - select an FQDN or IP address for AD server/(s). We enter `192.168.64.50`
    *  Base DN - enter the path to the AD user container. We enter `DC=workspaces, DC=wwco, DC=net`
    *  Administrator Bind DN - enter the admin/service account to query AD to authenticate users. We enter `wsadmin@workspaces.wwco.net`
    *  Confirm / Administrator Password - enter / confirm the admin / service account password
    *  Click Test Network Connectivity to ensure connection
    *  Server Logon Name Attribute - in the second field below this field enter `userPrincipalName`
1.  Select Create
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_actldap-notpauth.png)
1.  Enter the expression `true`, and click `OK`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_polldap-notpauth.png)

For more information see [LDAP authentication policies](/en-us/citrix-adc/current-release/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-ldap-tsk.html)

### Login Schemas

Login Schemas are used when data needs to be gathered on behalf of a policy.

#### Native OTP lSchema - Single Authentication

This registration login schema corresponds to the LDAP registration policy.

1.  Navigate to `Security > AAA-Application Traffic > Login Schema`
1.  Select the `Profile` tab
1.  Click `Add` under Profile, and name it `prolschema_notpsingle`
1.  Click the pencil icon next to `noschema`
1.  Click `Login Schema`, and scroll down to select `SingleAuthManageOTP.xml`, and select the blue `Select` in the right corner.
1.  Click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_prolschema-notpsingle.png)

#### Native OTP lSchema - Dual Authentication

This registration login schema corresponds to the dual factor authentication where the user enters both their password, and the OTP passcode.

1.  Under the `Profile` tab click `Add` again
1.  Enter the name `pollschema_notpdual`
1.  Click `Add` under Profile, and also name it `prolschema_notpdual`
1.  Click the pencil icon next to `noschema`
1.  Click `Login Schema`, and scroll down to select `DualAuth.xml`, and select the blue `Select` in the right corner.
1.  Click `More`
1.  In the field `Password Credential Index` enter `1`
1.  Click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_prolschema-notpdual.png)

### Native OTP `AAA` Virtual Server - Visualizer Flow

1.  Next navigate to `Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows`
1.  Click `Add`
1.  Click the `+` sign to create the initial factor. **This factor will not take action, rather handle directing incoming traffic to registration or authentication factor flows.**
1.  Enter `factor0-notp`, and click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_factor0-notp.png)

#### Registration Flow

1.  Select `Add Policy`
1.  Select `Add` next to `Select Policy`
1.  Enter name `polfactor0-notpmanage`
1.  Set the `Action Type` to `NO_AUTHN`
1.  Paste in `HTTP.REQ.COOKIE.VALUE(“NSC_TASS”).EQ(“manageotp”)` for the expression OR build it with Expression builder
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_polfactor0-notpmanageexpr.png)
**You can optionally limit registration to endpoints on the internal network by adding a source IP address criteria such as** `http.req.cookie.value("NSC_TASS").eq("manageotp") && client.IP.SRC.IN_SUBNET(10.0.0.0/8)`
1.  Click `Create`, followed by `Add`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_polfactor0-notpmanage.png)
1.  Select the green `+` to the right of the `polfactor0-notpmanage` policy you just created
1.  Enter `factor1-notpmanage`, and click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_factor1-notpmanage.png)
1.  In the new factor box, select `Add Schema`
1.  Select `prolschema_notpsingle`, and click `Ok`
1.  Select `Add Policy`
1.  From the drop-down list under `Select Policy` select `polldap_notpauth`, and click `Add`
1.  Select the green `+` to the right of the `polldap_notpauth` policy
1.  Enter `factor2-notpmanage`, and click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_factor2-notpmanage.png)
1.  In the new factor box, select `Add Policy`
1.  From the drop-down list under `Select Policy` select `polldap_notpmanage`, and click `Add`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_nfactorflow-manage.png)

#### Authentication Flow

1.  Now in the initial factor box we created `factor0-notp`, select the blue `+`
1.  Select `Add` next to `Select Policy`
1.  Enter name `polfactor0-notpauth`
1.  Set the `Action Type` to NO_AUTHN
1.  Enter `true` for the expression
1.  Click `Create`, followed by `Add` **Notice that the policy priority has increased to 110 meaning it will be executed only if the above policy `polfactor0-notpmanage` at 100 is not a match.**
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_polfactor0-notpauth.png)
1.  Select the green `+` to the right of the `polfactor0-notpauth` policy you just created
1.  Enter `factor1-notpauth`, and click `Create`
1.  In the new factor box, select `Add Schema`
1.  Select `prolschema_notpdual`, and click `Ok`
1.  Select `Add Policy`
1.  From the drop-down list under `Select Policy` select `polldap_notpauth`, and click `Add`
1.  Select `Done`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_nfactorflow-auth.png)

### Native OTP `AAA` Virtual Server

This `AAA` Virtual Server is where the policies and schema are bound with the appropriate priority.

1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain.
See [Citrix ADC SSL certificates](/en-us/citrix-adc/current-release/ssl/ssl-certificates.html) for more information.
1.  Next navigate to `Security > AAA - Application Traffic > Virtual Servers`, and select Add
1.  Enter the following fields:
    *  Name - a unique value. We enter `nativeotp_authvserver`
    *  IP Address Type - `Non Addressable`
1.  Click `Ok`
1.  Select No Server Certificate, select the arrow under `Select Server Certificate`, select the domain certificate, click Select, Bind, and Continue
1.  Under `Advanced Authentication Policies`, select `No Nfactor Flow`
1.  Select the right arrow under `Select nFactor Flow`, select `factor0_notp`, click `Select`, click `Bind`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_nfactorflowbinding.png)
1.  Click `Continue`, followed by `Done`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_authvserver.png)

### Traffic Policy

Now we create a traffic policy to relay the LDAP password to StoreFront, instead of the OTP passcode.

1.  Navigate to **Citrix Gateway > Virtual Servers > Policies > Traffic**
1.  Select the `Traffic Profiles` Tab, and click Add
1.  Enter the name `notp_trafficprofile`
1.  Select `HTTP`
1.  In the SSO Password Expression enter `http.REQ.USER.ATTRIBUTE(1)`
1.  Click Create
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_notp-trafficprofile.png)
1.  Now click the Traffic Policies Tab
1.  In the Request Profile field, select the `notp_trafficprofile` Traffic Profile you just created.
1.  Enter the name `nOTP_TrafficPolicy`
1.  In the Express box enter `true`
1.  Click `Create`
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_notp-trafficpolicy.png)

### Gateway Virtual Server

The Gateway Virtual Server is bound to the Native OTP `AAA` Virtual Server to provide authentication for Citrix Virtual Apps and Desktops.

1.  Navigate to `Citrix Gateway > Virtual Servers`
1.  Select your current Gateway, and click `Edit`
1.  Select Authentication Profile from the Advanced Settings panel on the right hand side
1.  Select `Add`
1.  Enter a profile name. We enter `nativeotp_authprofile`
1.  Under Policy select the arrow, and select the Native OTP `AAA` Virtual Server `nativeotp_authvserver`
1.  Click `Create`
1.  Select Policies from the Advanced Settings panel on the right hand side
1.  Select the `+` sign to Add
1.  Under `Choose Policy` select `Traffic`, and under `Choose Type` select `Request`. The select `Continue`
1.  Click the right arrow, select `notp_trafficpolicy`, and select `OK`
1.  Click `Done`, and save the running configuration
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_gatewayvserver.png)

## User Endpoint

Now we test Native OTP by authenticating into our Citrix Virtual Apps and Desktops environment.

### Registration with Citrix SSO app

First the user registers their device for Native OTP using the Citrix SSO app.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway with `/manageotp` appended to the end of the FQDN. We use `https://citrixadc5.workspaces.wwco.net/manageotp`
1.  After your browser is redirected to a login screen enter user UPN, and password
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_userregistration.png)
1.  On the next screen select Add Device, enter a name. We use `iPhone7_nOTP`
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regadddevice.png)
1.  Select Go, and a QR code appears
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regqrcodedisplay.png)
1.  On your mobile device open your Citrix SSO app or other authenticator app such as Microsoft or Google's (available for download from app stores)
1.  Select Add New Token
1.  Select Scan QR Code
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regssoqrcode.png)
1.  Select Aim your camera at the QR Code, and once it`s captured select Add
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regssoscanqrcode.png)
1.  Select Save to store the token
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regssosave.png)
1.  The Token is now active, and begins displaying OTP codes at 30 second intervals
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_regssotoken.png)
1.  Select Done and you see confirmation that the device was added successfully
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_addeddevicesuccessfully.png)

### Citrix Virtual Apps and Desktops Authentication, Publication, and Launch

Then the user enters their UserPrincipalName, Password, and the OTP Passcode from the Citrix SSO app to access their virtual apps, and desktops.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use `https://citrixadc5.workspaces.wwco.net`
1.  After your browser is redirected to a login screen enter user UserPrincipalName, and password
1.  Open the Citrix SSO app enter the OTP code in the passcode field for the `iPhone7_nOTP` device entry
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_notplogins.png)
1.  Verify the users virtual apps, and desktops are enumerated, and launch once logged in
![Native OTP Registration](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_cvadlogin.png)

## Troubleshooting

Here we look at a couple common troubleshooting areas for Native OTP.

### NTP Errors

Upon login with your OTP code the page may post a message advising you to verify NTP synchronization. The Citrix ADC's time must be sync in order to generate the correct time based OTP. If you have not implemented NTP follow these steps:

*  [Set the time manually on your Citrix ADC](https://support.citrix.com/article/CTX121356) to the current time. **This will speed up the synchronization that would otherwise take a longer period time**
*  [Add NTP Server/s](/en-us/citrix-application-delivery-management-software/current-release/configure/configure-ntp-server.html)
*  If you still get an NTP error upon submitting the OTP code see [Time Display on NetScaler Does Not Sync Using NTP](https://support.citrix.com/article/CTX201156)

### Authentication Errors

*  `Cannot complete your request.` - if this error message occurs after successful authentication it likely indicates an error passing user credentials to StoreFront. Verify the Dual Authentication schema and Traffic Policy settings.
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_cannotcompleteyourrequest.png)
*  `Try again or contact your help desk` - this error message often indicates a LDAP login failure.
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_tryagainorcontactyourhelpdesk.png)
If you have verified the password is correct verify the Administrator bind password has been set. You may have had an existing LDAP authentication policy, and created the manage policy by selecting it, followed by selecting add. This step saves time by populating existing settings like the `Base DN`, and you may see the Administrator password field appears to be populated, but you MUST reenter the password.
![Native OTP](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-native-otp_blankadministratorpassword.png)

## Summary

With Citrix Workspace, and Citrix Gateway, Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Users can gain access to their Citrix Virtual Apps and Desktops, by entering their domain username, and password, and then simply confirming their identity by entering a One Time Password from their registered authenticator app.

## References

For more information refer to:

[Native OTP Authentication](/en-us/citrix-adc/current-release/aaa-tm/native-otp-authentication.html) – find more details regarding Native OTP implementation, and use cases.
