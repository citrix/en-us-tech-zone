---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Dan Feller
description: Learn how to implement a Proof of Concept environment consisting of nFactor for Citrix Gateway Authentication with Device Certificates.
---
# Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Device Certificate

## Introduction

Large Enterprise environments require flexible authentication options to meet the needs of various user personas. With Device Certificate, coupled with LDAP credentials, Enterprises get "something you have" and "something you know" multifactor authentication. This allows users to seamlessly verify their identity and securely access their applications and data.

[![Device Certificate Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_conceptualarchitecture.png)](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-certificate_conceptualarchitecture.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It validates a Device Certificate as the first factor using Endpoint Analysis (EPA). Then it uses the user's domain credentials as the second factor. It uses a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix ADC installed, and licensed
*  Citrix Gateway configured with an externally reachable virtual server bound to a wildcard Certificate
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Active Directory (AD) is available in the environment
*  Windows 10 endpoint with Citrix Workspace app installed
*  The endpoint user must have local admin rights or have the Citrix Gateway Plug-in installed

Refer to Citrix Documentation for the latest product version, licensing, and requirement details: [Device certificate in nFactor as an EPA component](/en-us/citrix-gateway/current-release/vpn-user-config/endpoint-policies/device-certificate-in-nfactor-as-an-epa-component.html)

## nFactor

First, we log in to the CLI on our Citrix ADC and enter the authentication actions and associated policies for EPA and LDAP respectively along with the login schema. Then we log in to our GUI to build our nFactor flow in the visualizer tool and complete the multifactor authentication configuration.

### EPA Authentication policies

Next we create the EPA action to check the device certificate, and the policy that references it.

#### EPA action 1 - authAct_EPA_dcnf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication epaAction authAct_EPA_dcnf -csecexpr "sys.client_expr(\"device-cert_0_0\")"`

#### EPA policy 1 - authPol_EPA_dcnf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_EPA_dcnf -rule true -action authAct_EPA_dcnf`

### LDAP Authentication policies

We create the LDAP actions, and the policies that reference them.

For LDAP Actions populate the required fields to create the LDAP action in a string and paste it into the CLI:

*  `ldapAction` - enter the action name.
*  `serverIP` - enter the domain server/s FQDN or IP address.
*  `serverPort` - enter the LDAP port.
*  `ldapBase` - enter the string of domain objects and containers where pertinent users are stored in your directory.
*  `ldapBindDn` - enter the service account used to query domain users.
*  `ldapBindDnPassword` - enter your service account password.
*  `ldapLoginName` - enter the user object type.
*  `groupAttrName` - enter the group attribute name.
*  `subAttributeName` - enter the sub attribute name.
*  `secType` - enter the security type.
*  `ssoNameAttribute` - enter the single sign-on name attribute.

For LDAP Policies populate the required fields to reference the LDAP Action in a string and paste it into the CLI:

*  `Policy` - enter the policy name.
*  `action` - enter the name of the Email action we created above.

For more information see [LDAP authentication policies](/en-us/citrix-adc/13/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-LDAP-tsk.html)

1.  First connect to the CLI by opening an SSH session to the NSIP address of the Citrix ADC and log in as the `nsroot` administrator or equivalent admin user.

#### LDAP action 1 - authAct_LDAP_dcnf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication ldapAction authAct_Ldap_dcnf -serverIP 192.0.2.50 -serverPort 636 -ldapBase "DC=workspaces,DC=wwco,DC=net" -ldapBindDn wsadmin@workspaces.wwco.net -ldapBindDnPassword xyz123 -encrypted -encryptmethod ENCMTHD_3 -kek -suffix 2021_03_23_19_58 -ldapLoginName userPrincipalName -groupAttrName memberOf -subAttributeName cn -secType SSL -passwdChange ENABLED`

#### LDAP policy 1 - authPol_LDAP_dcnf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_LDAP_dcnf -rule true -action authAct_Ldap_dcnf`

### Login Schema

Next we create Login Schemas used with each factor.

#### lSchema 1 - lSchema_EPA_dcnf

The EPA factor does not require a Login Schema.

#### lSchema 2 - lSchema_LDAP_dcnf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication loginSchema ls_ldap_dcnf -authenticationSchema "/nsconfig/loginschema/LoginSchema/SingleAuth.xml"`

### nFactor

#### Domain Certificate

In this POC we used a wildcard certificate corresponding to our Active Directory domain and it also corresponds to the fully qualified domain name we use to access to the Gateway virtual server (gateway.workspaces.wwco.net)

1.  Log in to the Citrix ADC GUI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate and CAs installed and linked.  See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.

#### Device Certificate

There are many systems and options for user and device certificate management. In this POC we use the Microsoft Certificate Authority installed on our Active Directory server.

1.  From the start menu on our domain joined Windows 10 endpoint we enter `mmc`, right-click and run as administrator
1.  Select File > Add/Remove, select Certificates, select the arrow to move it to the Selected snap-in pane, select Computer account, Next, Local computer, Finish and, click OK
1.  Open the Personal folder, right-click the Certificates folder > All Tasks > Request New Certificate
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_requestnewcert.png)
1.  Click next until you are offered certificate types, select Computer, and click Enroll, followed by Finish
1.  Double-click the certificate it installed, select the Certification Path tab, select the root CA on the top, and click View Certificate. (Note: We can export the CA from the Active Directory server, yet for the POC we can eliminate steps by doing it here)
1.  In the popup select the Details tab, select Copy to File, click Next, click Next (to accept DER encoding)
1.  Select browse, and enter a file name, select save, select next, and select finish to store the CA certificate file.
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_exportdevicecacert.png)
1.  Now we will import it into the ADC by navigating to **Traffic Management > SSL> Certificates > CA Certificates
1.  Click Install, we enter the name `DeviceCertificateCA`, select Chose File, Local, and select the file, Open and click Install
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_installca.png)

### Visualizer

1.  Next navigate to `Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows`
1.  Select Add and select the plus sign in the Factor box

#### Factor1_Epa_dcnf

1.  Enter `Factor1_Epa_dcnf` and select create
1.  In the same box select Add Policy
1.  Select the EPA policy `authPol_EPA_dcnf`
1.  Select Add
1.  Select the green plus sign next to the `authPol_EPA_dcnf` policy to create another factor

#### Factor2_Ldap_dcnf

1.  Enter `Factor2_Ldap_dcnf`
1.  Select Create
1.  In the same box select Add Schema
1.  Select `ls_ldap_dcnf`
1.  In the same box select Add Policy
1.  Select `authPol_LDAP_dcnf`
1.  Under Goto Expression select `END`

![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_nfactorflow.png)

### Citrix ADC authentication, authorization, and auditing (Citrix ADC AAA) virtual server

1.  Next navigate to **Security > AAA - Application Traffic > Virtual Servers** and select Add
1.  Enter the following fields and click OK:
    *  Name - a unique value. We enter `DC_AuthVserver`
    *  IP Address Type - `Non Addressable`
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Select No nFactor Flow
1.  Under Select nFactor Flow click the right arrow, select the `Factor1_Epa_dcnf` flow created earlier
1.  Click Select, followed by Bind, followed by Continue
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_authvserver.png)

### Citrix Gateway - virtual server

1.  Next navigate to **Citrix Gateway > Virtual Servers**
1.  Select your existing virtual server that provides proxy access to your Citrix Virtual Apps and Desktops environment
1.  Select Edit
1.  Under Basic Settings select the pencil icon to edit, then select more at the bottom
1.  At the bottom right, under Device Cert CA select Add, and click the plus (+) sign next to the DeviceCertificateCA followed by OK
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_devicecertca1.png)
1.  Now under Certificate, select CA Certificate, Add Binding, select the right arrow under Select CA Cert and select DeviceCertificateCA followed by Bind and Close
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_devicecertca2.png)
1.  If you currently have an LDAP policy bound navigate under Basic Authentication - Primary Authentication select LDAP Policy. Then check the policy, select Unbind, select Yes to confirm, and select Close
1.  Under the Advanced Settings menu on the right select Authentication Profile
1.  Select Add
1.  Enter a name.  We enter `DC_AuthProfile`
1.  Under Authentication virtual server click the right arrow, and select the Citrix ADC AAA virtual server we created `DC_AuthVserver`
1.  Click Select, and Create
1.  Click OK and verify the virtual server now has an authentication profile selected while the basic authentication policy has been removed
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_gatewayvserver.png)
1.  Click Done

## User Endpoint

We test authentication by authenticating into our Citrix Virtual Apps and Desktops environment.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use `https://gateway.workspaces.wwco.net`
1.  Select download if the EPA plug-in has not been installed.
1.  Otherwise select Yes when the EPA plug-in prompts you to scan (you can also select Always to automatically scan). Thereafter it scans for device certificates.
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_epalogin.png)
1.  If you have multiple device certificates it prompts you to select the appropriate one to authenticate with otherwise it presents a logon prompt.
1.  Enter the domain user name and password.
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_ldaplogin.png)
1.  Select the virtual desktop from the available resources in their workspace and verify a successful launch.
![Device Certificate](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-device-certificate_cvad.png)

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Device Certificates allow Enterprises to seamlessly add a 2nd authentication factor to user credentials maintaining a good user experience while improving security posture.

## References

For more information refer to:

[How to Configure Device Certificate on Citrix Gateway for Authentication](https://support.citrix.com/article/CTX200290) - learn how to implement an OSCP responder to verify certificate revocation status.

[Understanding and Configuring EPA Verbose Logging on NetScaler Gateway](https://support.citrix.com/article/CTX209148) - verify the nsepa.txt on the endpoint logs the correct CA in the list that is downloaded. "Netscaler has sent list of allowed CA for device certificate." If not verify you imported and bound the correct one, that issued the device certificate, to the Gateway vServer.

[Citrix ADC Commands to Find the Policy `Hits` for Citrix Gateway Session Policies](https://support.citrix.com/article/CTX1388400) - learn more about CLI commands like `nsconmsg -d current -g _hits` to track policy `hits` to help troubleshoot.
