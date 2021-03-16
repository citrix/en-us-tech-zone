---
layout: doc
h3InToc: true
contributedBy: Jacob Rutski
specialThanksTo: Dileep Reddem
description: Learn how to use the security tools built in to the Citrix ADC to protect VPN and Gateway virtual servers, including Web Application Firewall (WAF), Bot Security, and Advanced Authentication Policies. 
---
# Protecting Gateway Virtual Servers with WAF, Bot, and Advanced Authentication Policies

## Overview 

Many VPN and Citrix Gateway deployments are hosted by Citrix ADC appliances that are also providing security protections to other web applications. This PoC guide is designed to help protect VPN and Gateway virtual servers using tools already available on the Citrx ADC appliance. This guide covers protecting the portal login page with Bot security and WAF capabilities, as well as using advanced authentication policies to add context to user logons. 

## Prerequisites

This guide assumes a working knowledge of Citrix WAF deployment, Bot Security deployment, and Advanced Authentication Policies (nFactor). It makes assumptions that a gateway or AAA virtual server is already installed and configured. Additionally, the following requirements must be met:

*  Advanced Authentication Policies require release 12.1 build 57.18 or later
*  Web Application Firewall protections require release 12.1 build 57.18 or later
*  Bot Security protections require release 13.0 build 71.40 or later
*  The majority of the features in this guide require a **premium license**
*  An existing server or service listening on port 80
*  An existing Gateway or AAA virtual server, with an existing advanced authentication configuration (advanced authentication or nfactor flow)
*  The following features need to be enabled: **Citrix Web App Firewall, Citrix Bot Management, and Reputation**

## Bot Protection

**Bot Signatures**

From Security > Citrix Bot Management > Signatures, select the Default Bot Signatures and click the **Clone** button. Apply a descriptive name, then click create.

![Bot Profile Signatures](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botsigclone.png)

**Create a Bot Management Profile and Policy**

From Security > Citrix Bot Management > Profiles, select Add to create a new Bot Management Profile. Give the profile a descriptive name and select the previously created signature set.

![Bot Profile](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botprofile.png)

Once the profile is created, select it to edit the advanced profile settings.

Add **IP Reputation** from the right column and check the box to enable it.

![Bot Profile IP Reputation](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botProfileiprep.png)

Next, choose 'Add' under categories, select IP for the Type, check the box for Enabled, set the action to Drop, check the box for 'Log' and set the log message to something descriptive.

![Bot Profile IP Reputation 2](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botprofileiprep2.png)

![Bot Profile IP Reputation 3](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botprofileiprep3.png)

Select **Device Fingerprint** from the right column, ensure that the 'Enabled' check box is **NOT** checked and click Update.

![Bot Profile Device Fingerprint](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botdevicefingerprint.png)

The last setting for the Bot Profile is to enable rate limiting, select **Rate Limit** from the right column and check the box for enabled. Click 'Add' under Configure Resources, and add three URL type rate limit bindings for the following URLs:

*  /logon/LogonPoint/index.html
*  /logon/LogonPoint/tmindex.html
*  /vpn/index.html

These rate limits should be enabled, with a rate of 5, period of 1000, action of Drop, log set to be enabled, and Log Message with a descriptive message title.

![Bot Profile Rate Limit](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botratelimit.png)

The Bot Profile should now be configured as follows:

![Bot Profile Final](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_botprofilefin.png)

Create a Bot **Management Policy** by going to Security > Citrix Bot Management > Bot Policies and choosing Add. Select the previously created Bot Profile, with an expression as follows:

```bash

HTTP.REQ.URL.CONTAINS("/vpn")||HTTP.REQ.URL.CONTAINS("/logon")
```

## WAF Protection

It is not possible to bind a WAF policy directly to a Gateway or AAA virtual server. Additionally, binding a WAF policy globally with an expression that targets Gateway or AAA virtual servers will likely not function as expected. This is due to the order in which policies are processed - with WAF policies being processed after Gateway and AAA policies.

The WAF protection policy uses an HTTP Callout to protect the logon page and invalidate the authentication flow if a WAF exception is caught. This configuration requires a pattern set (Patset) containing the login URLs, a dummy service and load balancing virtual server, an HTTP callout, and the WAF policy and configuration.

**Patset**

Navigate to AppExpert > Pattern Sets and select 'Add'. Give the new Pattern Set a name, then select 'Insert' and add the following patterns:

*  /cgi/login (index 1)
*  /nf/auth/doAuthentication.do (index 2)

![Pattern Set](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_patset.png)

Alternatively, the Patset can be created from the CLI:

```bash

add policy patset GW_VPN_Patset
bind policy patset GW_VPN_Patset "/cgi/login" -index 1
bind policy patset GW_VPN_Patset "/nf/auth/doAuthentication.do" -index 2
```

**Dummy Virtual Server and Service**

A dummy virtual server is used for the HTTP Callout. This virtual server does not need to be publicly available, so it can be non-addressable. The virtual server DOES need to be up, thus the backend server needs to be up and responding on port 80. A new service and virtual server will be created in this guide, but a pre-existing virtual server can be used.

Go to Traffic Management > Load Balancing > Services and select 'Add'. Give the service a descriptive name, set the protocol to HTTP and port to 80. Enter the IP address of the server and choose OK. Alternatively, an existing server may be used to create the service. All of the default settings may be used, including monitors that are bound to the service.

![Load Balancing Service](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_lbservice.png)

Next create the load balancing virtual server by going to Traffic Management > Load Balancing > Virtual Servers and select 'Add'. Give the server a descriptive name, set the protocol to HTTP, and set the IP address type to 'Non Addressable'. Bind the previously created service to this virtual server by selecting 'No Load Balancing Virtual Server Service Binding' then 'Click to select' and selecting the service. There is now 1 service bound to the virtual server and the state is 'UP'.

![Load Balancing Virtual Server](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_lbvirtualserver.png)

**HTTP Callout**

Navigate to AppExpert > HTTP Callouts and select 'Add'. Give the HTTP Callout a descriptive name, select 'Virtual Server' to receive the callout request, and select the dummy virtual server. In the Request to send to the server, select the type as Expression-Based, set the scheme to 'http' and set the Full Expression to the following:

```bash

HTTP.REQ.FULL_HEADER.BEFORE_STR("\r\n\r\n")+"\r\nGW_VPN-WAF_Callout:abc\r\n\r\n"+HTTP.REQ.BODY(2048)
```
**Note: the name of the header here is 'GW_VPN-WAF_Callout' - this will be used later in the application firewall filtering expression. If it is changed, it must also be changed in the WAF header expression as well.**

In the Server Response section, set the return type to **BOOL** and set the expression to 'true'.

![HTTP Callout](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_httpcallout.png)

Alternatively, the HTTP Callout can be created from the CLI:

```bash

add policy httpCallout GW_VPN_WAF_Callout -vServer dummy-vserver-here -returnType BOOL -fullReqExpr HTTP.REQ.FULL_HEADER.BEFORE_STR("\r\n\r\n")+"\r\nGW_VPN-WAF_Callout:abc\r\n\r\n"+HTTP.REQ.BODY(2048) -scheme http -resultExpr true
```

**Authentication Policy**

An existing LDAP authentication policy will be modified to leverage the HTTP Callout. Open the existing authentication policy by going to Security > AAA Aplication Traffic > Policies > Authentication > Advanced Policies > Policy, select the existing policy and choose 'Edit'. Modify the existing expression to the following:

```bash

HTTP.REQ.URL.CONTAINS_ANY("GW_VPN_Patset") && SYS.HTTP_CALLOUT(GW_VPN_WAF_Callout)
```

![Authentication Policy WAF Binding](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_authenticationpolicy.png)

**Note: this expression can be used with any authentication policy where you want to protect the form fields on the logon page.**

**WAF Profile and Policy**

To build the WAF profile go to Security > Citrix Web Application Firewall > Profiles and choose 'Add'. Give the profile a descriptive name and select Web Application (HTML) and Basic Defaults. Open the newly created profile by choosing 'Edit' then select 'Security Checks' from the right hand column.

Ensure that only the following security checks are enabled (all others should be unchecked):

*  Buffer Overflow - Log, Stats
*  Post Body Limit - Block, Log, Stats
*  HTML Cross-Site Scripting - Block, Log, Stats
*  HTML SQL Injection - Block, Log, Stats

![WAF Security Checks](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_wafsecuritychecks.png)

Next select 'Profile Settings' from the right hand column and ensure that the Default Response is set to:

```bash

application/octet-stream
```

Then check the box for 'Log Every Policy Hit'.

![WAF Profile Settings](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_wafprofilesettings.png)

Next, configure the WAF policy by going to Security > Citrix Web Application Firewall > Policies > Firewall and choose 'Add'. Give the policy a descriptive name and select the profile created in the previous step. For the expression, enter the following:

```bash

HTTP.REQ.HEADER("GW_VPN-WAF_Callout").EXISTS
```
**Note: the name of the header here must match the header in the HTTP Callout created earlier.**

Last, bind the WAF policy to the dummy load balancing virtual server created earlier by going to Traffic Management > Load Balancing > Virtual Servers and selecting the virtual server then choosing 'Edit'.

From the right hand column, select 'Policies' then click the '+' plus to add a policy. Select policy App Firewall and type Request. Select the policy created previously then select 'Bind' and 'Done'.

![WAF Policy Binding](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_wafpolbinding.png)

Alternatively, the WAF configuration can be created using the CLI as follows:

```bash

add appfw profile demo_appfw_profile -startURLAction none -denyURLAction none -fieldFormatAction none -bufferOverflowAction log stats -responseContentType "application/octet-stream" -logEveryPolicyHit ON -fileUploadTypesAction none

add appfw policy demo_appfw_policy "HTTP.REQ.HEADER(\"GW_VPN-WAF_Callout\").EXISTS" demo_appfw_profile

bind lb vserver dummy-vserver-here -policyName gw_appfw_policy -priority 100 -gotoPriorityExpression END -type REQUEST
```

## Advanced Authentication Settings

There are two configurations related to authentication - encrypting user credentials from the client to the ADC within nFactor and IP reputation based MFA.

**Encrypting User Credentials**

The following setting enables the ADC to encrypt the credential set when the user submits the form data using ECDHE algorithms. To enable this setting, navigate to Citrix Gateway > Global Settings > Authentication Settings > Change Authentication AAA settings and set 'Login Encryption' to **ENABLED**. 

![AAA Login Encryption](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaaloginenc.png)

Alternatively, this can be done from the CLI as follows:

```bash

set aaa parameter -loginEncryption ENABLED
```

**IP Reputation Based MFA**

IP Reputation can be built into the advanced authentication flow to prompt the user for an additional factor if the source address is flagged in the IP Reputation database or in a manually maintained dataset of addresses. 

**Important: the following configuration example uses CAPTCHA as a means to provide another factor of authentication, but any other MFA tool can be configured to provide additional verification for the user. As with all nFactor configurations, the policies, schemas, and policy labels shown here are simple examples - additional configuration can be added to meet any specific login use case.**

**Please see the references section for additional details on configuring TOTP PUSH as a factor as well as additional CAPTCHA configurations.**

A data set need to be created by going to AppExpert > Data Sets and selecting 'Add'. Create a data set with a descriptive name, a type of 'ipv4' and click 'Create'. 

![Malicious IP Data Set](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_dataset.png)

Next, two advanced authentication policies need to be created by going to Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Policy and select 'Add'.

Create the first policy with a descriptive name, action type of **NO_AUTHN** and expression of 'true'.

![Advanced Policy Good IP](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_advpol_good.png)

Create the second policy with a descriptive name, action type of **NO_AUTHN** and expression as follows:

```bash

CLIENT.IP.SRC.IPREP_IS_MALICIOUS || CLIENT.IP.SRC.TYPECAST_TEXT_T.CONTAINS_ANY("suspicious_ips")
```

**NOTE: the name of the previously created data set is used here.**

Next, a CAPTCHA login schema profile is created by going to Security > AAA - Application Traffic > Login Schema > Profiles Tab and selecting 'Add'. Give the profile a descriptive name then edit the Authentication Schema by selecting the 'pencil' edit icon. Browse to the LoginSchema directory, highlight **SingleAuthCaptcha.xml** and choose **Select**.

![Captcha Login Schema](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_captchaschema.png)

Next, create an authentication policy label for the Captcha schema by going to Security > AAA - Application Traffic > Policies > Authentication > Advanced Policies > Policy Label and selecting 'Add'. Give the PL a descriptive name and select the previously created captcha login schema. Bind the required LDAP action policy.

**Note: this example is re-using a previously created LDAP authentication action.**

![Captcha Policy Label](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_captchapl.png)

Create another policy label by selecting 'Add'. Give this PL a descriptive name, and set the login schema to LSCHEMA_INT. Next, bind the two previously created NO_AUTHN authentication policies.

![IP Check Policy Label](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_ipcheckpl.png)

Last, this IP Reputation check policy label needs to be set as the next factor of the previously created authentication policy that is already bound to a AAA or Gateway virtual server. Highlight the authentication policy, select 'edit binding' then set the new policy label as the 'Select Next Factor' field.

![Final Authentication Policy](/en-us/tech-zone/learn/media/poc-guides_protect-gateway-waf-bot-aaa_aaa_authpolicyfinal.png)

## Summary

Citrix ADC provides many built-in security protections that can be applied to protect Gateway or Authentication virtual servers running on the same appliance. These protections have no impact on typical users as they try to login to Citrix Gateway.

## References

For more information refer to:

[CTX216091](https://support.citrix.com/article/CTX216091) - supporting reCaptcha with nFactor

[Citrix Web Application Firewall PoC Guide](/en-us/tech-zone/learn/poc-guides/citrix-waf-deployment.html) – proof of concept deployment guide for Citrix Web Application Firewall

[nFactor for Citrix Gateway with Push Token](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-push-token.html) - proof of concept deployment guide for TOTP push tokens for Citrix Gateway
