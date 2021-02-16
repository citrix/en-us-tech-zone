---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Dan Feller
description: Learn how to implement a Proof of Concept environment consisting of nFactor for Citrix Gateway Authentication with Group Extraction
---
# Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Group Extraction

## Introduction

Large Enterprise environments require flexible authentication options to meet the needs of a variety of user personas. With Group Extraction user AD group membership determines the number, and type of nFactor authentication methods users are required to complete to verify their identity and access their applications and data.

Examples of user groups include:

*  normal-security-group for individuals that may have lower security requirements by the nature of their job or limited data access and are located within the bounds of the corporate security perimeter. This group may only require 1 factor.
*  elevated-security-group for third party workers or contractors who may not have had background checks done and have higher security requirements. This group may require 2 or more factors.
*  high-security-group for employees that perform critical jobs, and require special government clearance, or industry approval. This group may require 2 or more factors and contextual verifications such as source IP address.

![Group Extraction Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_conceptualarchitecture.png)

## Overview

This guide demonstrates how to implement a Proof of Concept environment using two factor authentication with Citrix Gateway. It uses LDAP only to validate Active Directory credentials if the user's endpoint is on a private subnet, indicating they are on the corporate intranet, or if they are a member of a "VIP" AD group such as a CXO. Otherwise, it is assumed they are located external to the perimeter of the Enterprise network and not a member of a group with lower security requirements, and are required to complete a second factor in the form of entering an email One Time Password (OTP). It uses a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix ADC installed, and licensed
*  Citrix Gateway configured with an externally reachable virtual server bound to a wildcard certificate
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops environment which uses LDAP for authentication
*  Endpoint with Citrix Workspace app installed
*  Active Directory (AD) is available in the environment
*  Access to an SMTP server to originate email

Refer to Citrix Documentation for the latest product version, and license requirements: [nFactor Group Extraction](/en-us/citrix-adc/current-release/aaa-tm/configure-two-passwords-group-extraction.html)

## nFactor

First, we log in to the CLI on our Citrix ADC and enter the authentication actions and associated policies for LDAP and Email respectively. Then we log in to our GUI to build our nFactor flow in the visualizer tool and complete the multifactor authentication configuration.

### LDAP Authentication policies

We create the LDAP actions, and the policies that reference them. We also create the Email action, and the policy that references it, which is the multifactor authentication method for users that are not members of the VIP group or on a local subnet.

1.  First connect to the CLI by opening an SSH session to the NSIP address of the Citrix ADC and log in as the `nsroot` administrator or equivalent admin user.

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
*  `defaultAuthenticationGroup` - enter the default authentication group.
*  `alternateEmailAttr` - enter the user domain object attribute where their email address can be retrieved.

For LDAP Policies populate the required fields to reference the LDAP Action in a string and paste it into the CLI:

*  `Policy` - enter the policy name.
*  `action` - enter the name of the Email action we created above.

For more information see [LDAP authentication policies](/en-us/citrix-adc/13/aaa-tm/configure-aaa-policies/ns-aaa-setup-policies-authntcn-tsk/ns-aaa-setup-policies-auth-LDAP-tsk.html)

#### LDAP action 1 - authAct_GroupExtract_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication ldapAction authAct_GroupExtract_genf -serverIP 192.168.64.50 -ldapBase "OU=Team Matt,OU=Team Accounts,OU=Demo Accounts,OU=Workspaces Users,DC=workspaces,DC=wwco,DC=net" -ldapBindDn workspacessrv@workspaces.wwco.net -ldapBindDnPassword 123xyz -encrypted -encryptmethod ENCMTHD_3 -ldapLoginName userPrincipalName -groupAttrName memberOf -subAttributeName cn -secType SSL -authentication DISABLED`

#### LDAP policy 1 - authPol_GroupExtract_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_GroupExtract_genf -rule true -action authAct_GroupExtract_genf`

![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_cli.png)

#### LDAP policy 2A - authPol_LdapOnly_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_LdapOnly_genf -rule "AAA.USER.IS_MEMBER_OF(\"VIP\") || client.IP.SRC.IN_SUBNET(10.0.0.0/8)" -action NO_AUTHN`

#### LDAP policy 2B - authPol_TwoFactor_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_TwoFactor_genf -rule "client.IP.SRC.IN_SUBNET(10.0.0.0/8).NOT" -action NO_AUTHN`

#### LDAP action 3A - authAct_Ldap_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication ldapAction authAct_Ldap_genf -serverIP 192.168.64.50 -ldapBase "OU=Team Matt,OU=Team Accounts,OU=Demo Accounts,OU=Workspaces Users,DC=workspaces,DC=wwco,DC=net" -ldapBindDn workspacessrv@workspaces.wwco.net -ldapBindDnPassword 123xyz -encrypted -encryptmethod ENCMTHD_3 -ldapLoginName userPrincipalName -groupAttrName memberOf -subAttributeName cn -secType SSL -passwdChange ENABLED`

#### LDAP policy 3A - authPol_GroupExtract_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_Ldap_genf -rule true -action authAct_Ldap_genf`

#### LDAP action 3B - authAct_LDAP_eotp_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication ldapAction authAct_LDAP_eotp_genf -serverIP 192.168.64.50 -serverPort 636 -ldapBase "DC=workspaces,DC=wwco,DC=net" -ldapBindDn wsadmin@workspaces.wwco.net -ldapBindDnPassword 123xyz -encrypted -encryptmethod ENCMTHD_3 -ldapLoginName userPrincipalName -groupAttrName memberOf -subAttributeName cn -secType SSL -ssoNameAttribute userPrincipalName -defaultAuthenticationGroup Email-OTP -alternateEmailAttr otherMailbox`

#### LDAP policy 3B - authPol_LDAP_eotp_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication Policy authPol_LdapEtop_genf -rule true -action authAct_LDAP_eotp_genf`

### Email Authentication policy

Populate the following fields to create the Email action and paste the completed string into the CLI:

*  `emailAction` - enter the action name.
*  `userName` - enter the user, or service account, that log in to the mail server.
*  `password` - enter your service account password to log in to the mail server. (The password is encrypted by the Citrix ADC by default)
*  `serverURL` - enter the FQDN or IP address of the mail server.
*  `content` - enter the user message next to the field to enter the email code.
*  `time out` - enter the number of seconds the email code is valid.
*  `emailAddress` - enter the LDAP object to query for the user email address.

For the Email policy populate the required fields to reference the Email Action in a string and paste it into the CLI:

*  `Policy` - enter the policy name.
*  `action` - enter the name of the Email action

For more information see [Email OTP authentication policy](/en-us/citrix-adc/current-release/aaa-tm/authentication-methods/email-otp.html)

#### Email action 4B - authAct_Email_eotp_genf

Update the following fields for your environment and copy and paste the string into the CLI:

`add authentication emailAction authAct_Email_eotp_genf -userName admin_matt@workspaces.wwco.net -password 123xyz -encrypted -encryptmethod ENCMTHD_3 -serverURL "smtps://192.168.64.40:587" -content "Your OTP is $code" -timeout 60 -emailAddress "aaa.user.attribute(\"alternate_mail\")"`

#### Email policy 4B - authPol_Email_eotp_genf

Update the following fields for your environment and copy and paste the string into the CLI:

`add authentication Policy authPol_Email_eotp_genf -rule true -action authAct_Email_eotp`

### Login Schema

#### lSchema 1 - lSchema_GroupExtract_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication loginSchema lSchema_GroupExtract_genf -authenticationSchema "/nsconfig/loginschema/LoginSchema/OnlyUsername.xml"`

#### lSchema 2 - CheckAuthType_genf

The second factor does not require a Login Schema. It just has policies with expressions to check which factor to do next.

#### lSchema 3A - lSchema_LDAPPasswordOnly_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication loginSchema lSchema_LDAPPasswordOnly_genf -authenticationSchema "/nsconfig/loginschema/PrefilUserFromExpr.xml"`
   **Here you may receive a warning that http.req.user has been replaced with aaa.user. You must edit the xml file from the cli.**

![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_httprequserdeprecated.png)

1.  Log in to the Citrix ADC CLI
1.  Enter `shell`
1.  Enter `cd /nsconfig/loginschema/LoginSchema`
1.  Enter 'vi PrefilUserFromExpr.xml'
1.  Enter `/http.req`
1.  Press x 8 times to delete the http.req string
1.  Press the escape key
1.  Press i and enter `aaa`, press the escape key again
1.  Press the colon key ':', enter `wq` and press enter.
1.  **NOTE that you can use this method to modify other aspects of the login schema such as the field prompts**

#### lSchema 3B - lSchema_EOTPPasswordOnly_genf

Update the following fields for your environment and copy and paste the string into the CLI:
`add authentication loginSchema lSchema_EOTPPasswordOnly_genf -authenticationSchema "/nsconfig/loginschema/PrefilUserFromExpr.xml"`
NOTE: The 3B factor also uses the PrefilUserFromExpr.xml schema, but we label the policy differently for the EOTP path.

#### lSchema 4 - EOTP_genf

The fourth factor does not require a Login Schema. It generates the email with the One Time Passcode.

### nFactor

1.  Log in to the Citrix ADC GUI
1.  Navigate to **Traffic Management > SSL> Certificates > All Certificates** to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.
1.  Next navigate to `Security > AAA - Application Traffic > nFactor Visualizer > nFactor Flows`
1.  Select Add and select the plus sign in the Factor box

### Visualizer

#### Factor1_GroupExtract_genf

1.  Enter `Factor1_GroupExtract_genf` and select create
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_factor1groupextractgenf.png)
1.  Select Add Schema
1.  Select the Login Schema lSchema_GroupExtract_genf
1.  Select OK
1.  In the same box select Add Policy
1.  Select the LDAP policy `authPol_GroupExtract_genf`
1.  Select Add
1.  Select the green plus sign next to the `authPol_GroupExtract_genf` policy to create a factor

#### Factor2_CheckAuthType_genf

1.  Enter `Factor2_CheckAuthType_genf` **This Factor is used to verify the authentication required**
1.  Select Create
1.  In the same box select Add Policy
1.  Enter `authPol_LdapOnly_genf`
1.  Under Goto Expression select `END`
1.  Select Add
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_authpolldaponlygenf.png)
1.  Select the blue plus sign under the `authPol_LdapOnly_genf` policy to add a second policy
1.  Select the policy `authPol_TwoFactor_genf`
**Next we make the Two Factor policy occur prior to the LDAP only policy by lowering the priority to 90 which is less than the default of 100. This ensures that remote users in the VIP group are identified for LDAP only authentication.**
1.  Enter `90` for the Priority
1.  Select Add

#### Factor3A_LDAPPasswordAuth_genf

1.  Back next to the `authPol_GroupExtract_genf` policy select the green plus sign to create another factor
1.  Enter `Factor3A_LDAPPasswordAuth_genf`
1.  Select Create
1.  In the same box select Add Policy
1.  Enter `authPol_Ldap_genf`
1.  Under Goto Expression select `END`
1.  Select Add
1.  Select Add Schema
1.  Select the Login Schema `lSchema_LDAPPasswordOnly_genf`
1.  Select OK

#### Factor3B_EOTPPasswordAuth_genf

1.  Back next to the `authPol_TwoFactor_genf` policy select the green plus sign to create another factor
1.  Enter `Factor3B_EOTPPasswordAuth_genf`
1.  Select Create
1.  In the same box select Add Policy
1.  Enter `authPol_LdapEtop_genf`
1.  Select Add
1.  Select Add Schema
1.  Select the Login Schema `lSchema_EOTPPasswordOnly_genf`
1.  Select OK

#### Factor4B_EOTP_genf

1.  Next to the `authPol_LdapEtop_genf` policy select the green plus sign to create another factor
1.  Enter `Factor4B_EOTP_genf`
1.  Select Create
1.  In the same box select Add Policy
1.  Enter `authPol_Email_eotp_genf`
1.  Select Add
1.  Select Done and the nFactor flow is complete
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_nfactorflow.png)

### Citrix ADC authentication, authorization, and auditing (Citrix ADC AAA) virtual server

1.  Next navigate to **Security > AAA - Application Traffic > Virtual Servers** and select Add
1.  Enter the following fields and click OK:
    *  Name - a unique value. We enter 'GroupExtraction_AuthVserver'
    *  IP Address Type - `Non Addressable`
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Select No nFactor Flow
1.  Under Select nFactor Flow click the right arrow, select the `Factor1_GroupExtract_genf` flow created earlier
1.  Click Select, followed by Bind, followed by Continue
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_authvserver.png)

### Citrix Gateway - virtual server

1.  Next navigate to **Citrix Gateway > Virtual Servers**
1.  Select your existing virtual server that provides proxy access to your Citrix Virtual Apps and Desktops environment
1.  Select Edit
1.  If you currently have an LDAP policy bound navigate under Basic Authentication - Primary Authentication select LDAP Policy. Then check the policy, select Unbind, select Yes to confirm, and select Close
1.  Under the Advanced Settings menu on the right select Authentication Profile
1.  Select Add
1.  Enter a name.  We enter `GroupExtract_AuthProfile`
1.  Under Authentication virtual server click the right arrow, and select the Citrix ADC AAA virtual server we created `GroupExtraction_AuthVserver`
1.  Click Select, and Create
1.  Click OK and verify the virtual server now has an authentication profile selected while the basic authentication policy has been removed
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_gatewayvserver.png)
1.  Click Done

## User Endpoint

First we test whether One Factor authentication is applied to VIP users by authenticating into our Citrix Virtual Apps and Desktops environment.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use `https://citrixadc5.workspaces.wwco.net`
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsvipuser-username.png)
1.  After your browser is redirected to a login screen. First enter a user name. We use `wsvipuser@workspaces.wwco.net`
**This user must be a member of the AD group `VIP`**
1.  nFactor determines that the user is a member of the VIP group and you are promoted to submit the user password.
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsvipuser-password.png)
1.  Now the user is logged into their Workspace page.
1.  Select a virtual desktop and verify launch.
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsvipuser-cvad.png)

Now we test Two Factor authentication with Email OTP by authenticating into our Citrix Virtual Apps and Desktops environment again.

1.  Open a browser, and navigate to the domain FQDN managed by the Citrix Gateway. We use `https://citrixadc5.workspaces.wwco.net`
1.  After your browser is redirected to a login screen. First enter a user name. We use `wsuser@workspaces.wwco.net`
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsuser-username.png)
1.  nFactor determines that the user is not local, nor a member of the VIP group, you are be prompted to submit the user password.
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsuser-password.png)
1.  The nFactor then presents a form requesting the OTP passcode. We copy and paste the passcode from the `wsuser` email account.
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsuser-otp.png)
1.  Now the user is logged into their Workspace page.
1.  Select a virtual desktop and verify launch.
![Group Extraction](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-group-extraction_wsuser-cvad.png)

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multifactor authentication without making the user experience complex. Group Extraction allows Enterprise to cater the depth of their multifactor use, along with contextual authentication, according to user group persona requirements.

## References

For more information refer to:

[Citrix ADC Commands to Find the Policy `Hits` for Citrix Gateway Session Policies](https://support.citrix.com/article/CTX1388400) - learn more about CLI commands like `nsconmsg -d current -g _hits` to track policy hits to help troubleshoot.

[nFactor for Citrix Gateway Authentication with Email OTP](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-email-otp.html) - learn how to implement an extensible and flexible approach to configuring multifactor authentication with nFactor for Citrix Gateway authentication with email one-time password.
