---
layout: doc
h3InToc: true
contributedBy: Tijl Van den Broeck
specialThanksTo: Dileep Reddem, Jason Poole, Anurag Gupta, Kiran SA, Amit Arora
description: Balancing user experience and usability is key to the success of a business application. Citrix ADC enables you to allow users to benefit from “trust this device” functionality, which reduces the amount of reauthentication required and can greatly improve the user experience. This guide describes how to implement the feature to ensure the success of your business critical applications.
tz_title: Implementing "Trust this device" with Citrix ADC nFactor Authentication
tz_products: citrix-networking;security
---
# Content Type : Implementing "Trust this device" with Citrix ADC nFactor Authentication

Balancing user experience and usability is key to the success of a business application. Citrix ADC enables you to allow users to benefit from “trust this device” functionality, which reduces the amount of reauthentication required and can greatly improve the user experience. This guide describes how to implement the feature to ensure the success of your business critical applications.

## Introduction

Application security is more important today than ever before. Limiting application access to only users that require it and ensuring that those users are who they say they are is an obvious and powerful step to preventing security issues. Many web applications rely on multi-factor authentication (MFA), or at least 2-factor authentication (2FA) to sure up this access. Security teams have long known that there is a tradeoff between security and usability and MFA is a case in point. You can lock down access and offer very tight security but forcing the user to re-authenticate every time they require a piece of information from an application is tedious for the user and diminishes the application experience greatly.

### Trust this device improves user experience
"Trust this device" is a simple feature that helps improve the user experience, by telling the application that for a short period of time it should not ask the user to authenticate. This improves the user experience and makes the user more productive while still offering the security needed to protect the digital assets. Citrix ADC enables you to enable this feature for your applications, and with remote access to corporate resources on the rise because of the Covid-19 pandemic, it is being used more prevalently. This solution brief describes how to implement the “trust this device” functionality on your Citrix ADC and protect all your applications deployed behind it.

## The implementation process
The deployment guide is a technical document that shows how trust this device functionality is achieved and how to set it up. Screen shots are included where feasible. Trust this device functionality pulls upon several powerful features of the Citrix ADC. The authentication flow is summarized in Figure 1 and the steps are described in the following 8 sections:
* nFactor loginSchema customization
* nFactor Flow
* Using global variable map 
* WebAuth
* Persistent Cookie
* Policy Encryption and Decryption
* Citrix Gateway with nFactor

![flowchart](/en-us/tech-zone/build/media/deployment-guides_implementing-trust-this-device-with-citrix-adc-nfactor-authentication_flowchart.png)

**Figure 1: Logic flow of "trust this device" functionality**

At the end of this page there is an appendix with links to additional resources (Example ns.conf file, LoginSchema) which you can use and edit to suit your environment.

### nFactor loginSchema
The look and feel of a login interface is important for a business to provide a more intuitive user experience. The Citrix ADC nFactor authentication framework is designed to be flexible and extensible with a clear separation of colors, backgrounds and themes in portal themes and custom login labels, text fields, radio buttons, login credentials as well in loginSchemas to fully customize the user interface display.

The loginSchema is an XML description of all elements required to build up the user interface. The `singleAuthPersist.xml` loginSchema, which is included in the appendix, is a customized version of the default `singleAuth.xml` and `PrefillUserFromExpr.xml` schemas. In this adapted schema, a new requirement has been added (lines 73-87):
``` 
<Requirement> 
  <Credential>
    <ID>persistdevice</ID>
    <Type>none</Type> 
  </Credential>
  <Label>
    <Text>trust_device</Text> 
    <Type>nsg-login-label</Type>
  </Label>
  <Input>
    <CheckBox>
      <InitialValue>false</InitialValue>
    </CheckBox>
  </Input>
</Requirement>
``` 
This translates into a GUI checkbox being shown where the user can select to trust, or not to trust his device for a set period of time (Figure 2). Additionally, you can customize the login interface to suit your environment.

To cope with multi-lingual requirements of the user interface, the loginSchema itself refers to a key/value-pair mapping. In the above XML snippet the label text "trust_device" is added but it has not yet been defined as a valid key/value-pair. At a minimum to support English language the key/value-pair must be added in the following file:
``` 
 /var/netscaler/logon/LogonPoint/receiver/js/ctxs.core.min.js
``` 
And append the following key/value-pair assignment at the end of the ctxs.core.min.js file:
``` 
"KB Questions and Answers not registered":"KB Questions and Answers not registered", Question:" Question",Answer:"Answer",OTPDeleteDevice:"Scan QR Code into the CitrixSSO App",trust_device:"Trust this device for 24h"})})(jQuery);
``` 
![screenshot 2nd factor](/en-us/tech-zone/build/media/deployment-guides_implementing-trust-this-device-with-citrix-adc-nfactor-authentication_screenshot_2nd_factor.png)

**Figure 2: Screen shot of login showing “Trust this device” check box**

To support other languages you can translate and insert similar key/value-pairs using the localization files below – which are already present on the Citrix ADC appliance:
``` 
/var/netscaler/logon/LogonPoint/receiver/js/localization/de/ctxs.strings.de.js - German
/var/netscaler/logon/LogonPoint/receiver/js/localization/en/ctxs.strings.js - English
/var/netscaler/logon/LogonPoint/receiver/js/localization/es/ctxs.strings.es.js - Spanish
/var/netscaler/logon/LogonPoint/receiver/js/localization/fr/ctxs.strings.fr.js - French
/var/netscaler/logon/LogonPoint/receiver/js/localization/ja/ctxs.strings.ja.js - Japanese
/var/netscaler/logon/LogonPoint/receiver/js/localization/zh-CN/ctxs.strings.zh-CN.js - Chinese
/var/netscaler/logon/LogonPoint/receiver/js/localization/nl/ctxs.strings.nl.js - Dutch/Flemish
``` 
### nFactor Flow
Citrix ADC’s nFactor authentication feature not only allows you to customize the loginSchema as discussed above, it also allows policy based authentication and nesting of authentication factors. This flexible nesting is used to select the subsequent authentication factors (2nd, 3rd or more).

Authentication policies are linked to an authentication virtual server and can be grouped in policy labels (or banks). Each policy hit invokes an authentication action and the success of that action will result in a successful or failed login, and can also invoke a next policy label (or factor). This is summarized in Figure 3 for the "trust this device" implementation.

In Citrix ADC software version 13.0 a new feature, nFactor Flow, was introduced that visualizes the logic of policy labels. This allows visual configuration of authentication flows, which is easier to follow (Figure 4). 

Figure 4 shows the implementation flow required to enable "Trust this device". There are, effectively 5 steps: 
1. Check and validate the presence of a persistency cookie. If it is present then an LDAP authentication is sufficient
1. If there is no valid persistency cookie, then LDAP authentication is performed and a RADIUS authentication is invoked as a subsequent authentication factor
1. At the RADIUS authentication the user can select "Trust this Device". If it is checked then Web Authentication is invoked to set a variable after which RADIUS Passthrough is invoked.
1. If the user did not select "Trust this device" then RADIUS Passthrough is directly invoked.
1. Lastly, in RADIUS Passthrough the RADIUS authentication is checked, if this is valid the user is successfully authenticated.

![policylabel logic](/en-us/tech-zone/build/media/deployment-guides_implementing-trust-this-device-with-citrix-adc-nfactor-authentication_policylabel_logic.png)

**Figure 3: Authentication Policy Label Flow Logic** 

**NOTE**: Setting the persistency cookie is not part of the nFactor Flow configuration. This is done later with a Rewrite Policy (see section Persistent Cookie).
### Global Variable Map
The configuration uses a global variable map to store the users’ choice for whether the device should be trusted or not. This is configured via the Citrix ADC GUI (Figure 5) or by the following CLI command:
``` 
add ns variable gvar_aaa_ persist_cookie_users -type "map(text(12),ulong,1000)" -init 0 -expires 60
``` 
In the Global variable map the data are stored as key-value pairs and it behaves like a hash-table. The Key definition is the username and the value is whether or not the “trust this device” box is selected.

NOTE: in the above configuration example the Key is limited to 12 characters so if your environment uses longer usernames, please make sure to increase the key length. The Value is set to "ulong" format (large integer) even though the value to be stored is only 0 or 1 corresponding to the boolean logic of true (box selected) or false (box not selected).

In this example, there is an expiration timer of 60 seconds. This should be sufficient for a customer to log in, check the box, execute a one time password (OTP) authentication with a RADIUS server and land at a website/application that sets a persistent cookie, but it can be set higher or lower if required.

![nfactor flow](/en-us/tech-zone/build/media/deployment-guides_implementing-trust-this-device-with-citrix-adc-nfactor-authentication_nfactor_flow.png)

**Figure 4: nFactor Flow - visualizing the Trust Device authentication flow**

![screenshot variable definition](/en-us/tech-zone/build/media/deployment-guides_implementing-trust-this-device-with-citrix-adc-nfactor-authentication_screenshot_variable_definition.png)

**Figure 5: Screen shot of the global variable map dialogue**

The variable map contains a maximum of 1000 entries. This does not mean it will only handle 1000 users, but rather it corresponds to a maximum of 1000 users in the configured 60 second timeframe. After a successful LDAP authentication, the variable is stored. If the variable is not referred to or called upon for 60 seconds the entry is removed. Further, if the map is full then the least frequently used key-value pair is overwritten.

**NOTE**: By default the value is set to 0. This means that by default the "trust this device" check box is NOT checked.

In order to set the variable you need to have a variable assignment which can be an action triggered on a rewrite policy. This rewrite policy for assignment can be bound to any virtual server with a real backend but a non-existing path can be set up solely for the purpose of intercepting this rewrite policy. The below assignment and rewrite policy will retrieve the username after "enable_persist=" from the POST body sent to the URL path "http://server/setvar"
``` 
add ns assignment assign_gvar_aaa_persistusers -variable ""$gvar_aaa_persist_cookie_users[HTTP.REQ.BODY(500).AFTER_STR(\"enable_persist=\")]" -set 1
add rewrite policy pol_rw_set_gvar_aaapersistusers "http.req.url.path.get(1).eq(\"setvar\")" assign_gvar_aaa_ persistusers
``` 

If you want to have an existing path then simply adjust the policy to fetch a dummy empty page from one of your backend servers.

**SECURITY CAUTION**: Please note the server you select for this purpose should not be publicly accessible. A private virtual server with a non-public IP address should be created especially for this purpose. It may re-use an existing backend but the virtual server itself and its setvar policy should not be exposed publicly. Malicious actors would be able to send requests to this "setvar" URL to enable persistency and bypass the 2nd factor.

### WebAuth
When a user selects the "trust device" checkbox, the nFactor feature will trigger a Web Authentication policy and will subsequently POST the username in the "enable_persist=username" format to a previously configured rewrite policy and variable assignment location.

The following lines of configuration configures both the policy labels for the 2nd factor authentication with the Web Authentication policy.
``` 
add authentication policy pol_auth_radius -rule TRUE -action 7.2.0.20
add authentication policy pol_auth_noauth_enable_persist -rule "HTTP.REQ.BODY(500).AFTER_STR(\"persistdevice=\").STARTSWITH(\"true\")" -action act_auth_ webauthpersist
bind authentication policylabel nf_Radius__nf_nFlowTrust -policyName pol_auth_noauth_enable_persist -priority 100 -gotoPriorityExpression next -nextFactor nf_RadiusPassthru__nf_nFlowTrust
bind authentication policylabel nf_Radius__nf_nFlowTrust -policyName pol_auth_passthru_radius -priority 110 -gotoPriorityExpression next -nextFactor nf_RadiusPassthru__nf_nFlowTrust
``` 

The nFactor policy evaluation will then move to the next authentication - RADIUS in this case - in the same page.

### Persistent Cookie
In the below config snippet the actual Web Authentication action is configured, note that the length_post_data_setvar is a named policy expression that is reused in the WebAuthAction to automatically determine the content-length of the "enable_persist=username" post body.
The WebAuthAction itself in this example is posting to a non-existent URL that is picked up by the rewrite policy so an "ok" response from the webserver will be a 404 page. In this case that is considered a successful authentication for WebAuth (and the nFactor configuration).
``` 
add policy expression length_post_data_setvar "(\"enable_persist=\" + AAA.LOGIN.USERNAME).LENGTH"
add authentication webauthaction act_auth_webauthpersist -serverip 7.2.0.132 -serverport 81 -fullreqexpr q{"POST / setvar HTTP/1.1" + "\r\naccept: */*\r\nHost: 7.2.0.132\r\nContent-Type: application/x-www-form-urlencoded\r\nContent-Length:" + length_post_data_setvar + "\r\n\r\ nenable_persist=" + AAA.LOGIN.USERNAME} -scheme http -successrule "HTTP.REQ.STATUS.EQ(404)"
``` 
After successful authentication of the user (and optional storing in the Global variable map if the user selected “trust device”), a response-rewrite policy is used (e.g. on the landing page of the accessed web application) to set and store a Persistent Cookie for that user. In the configuration below, the rewrite actions and policies are created to set the relevant persistent cookies on the users’ systems. The time out is set to 120 seconds but you may change this to any time value. The example screen shot in Figure 2 the cookie persistence lifetime is shown as 24 hours. This is equivalent to 86400s.

**NOTE**: The rewrite policies also checks for existing nFactor Persist cookies set or not (with cookie length as above). As a result, it only sets the persistent cookies once. The cookies must expire in order for them to be set again.
``` 
add rewrite action act_rw_insert_persistcookie insert_http_header Set-Cookie "\"nFactor-Persist=true; path=/; secure; HttpOnly; Max-Age=\" + 120 + \"; SameSite=Lax\""
add rewrite action act_rw_insert_persisttime insert_http_header Set-Cookie "\"nFactor-PersistCheck=\" + SYS.TIME.TYPECAST_TEXT_T.ENCRYPT + \" ;path=/; Secure; HttpOnly; Max-Age= \" + 120 + \"; SameSite=Lax\""
add rewrite action act_rw_insert_persistuser insert_http_header Set-Cookie "\"nFactor-PersistUser=\" + AAA.USER.NAME.ENCRYPT + \" ; Path=/; Secure; HttpOnly; Max-Age=\" + 120 + \"; SameSite=Lax\""
add rewrite policy pol_rw_insert_persistcookie "$gvar_aaa_persist_cookie_users[AAA.USER.NAME] == 1 && HTTP.REQ.COOKIE.NAMES.GET(\"nFactor-Persist\").LENGTH == 0" act_rw_insert_persistcookie
add rewrite policy pol_rw_insert_persisttime "$gvar_aaa_persist_cookie_users[AAA.USER.NAME] == 1 && HTTP.REQ.COOKIE.NAMES.GET(\"nFactor-PersistCheck\").LENGTH == 0" act_rw_insert_persisttime
add rewrite policy pol_rw_insert_persistuser "$gvar_aaa_persist_cookie_users[AAA.USER.NAME] == 1 && HTTP.REQ.COOKIE.NAMES.GET(\"nFactor-PersistUser\").LENGTH == 0" act_rw_insert_persistuser
``` 

### Encryption and Decryption
Note that both in the previous setting of the persistent cookies, as well as when cookies' presence are validate Citrix ADC’s Advanced Policy’s ENCRYPT and DECRYPT functions are used as following:
* The encrypted current system time of the Citrix ADC platform is encoded and encrypted and set in a persistent cookie. For validation the time is extracted again from the decrypted cookie and validated against the current system time including the additional 120 seconds (or whatever lifetime is set). This way there is additional protection to prevent malicious actors from manually manipulating cookie values or extending the timeframe for which the cookie is valid.
* There are scenarios where an attacker has both a persistent cookie from a low-level account and the login credentials to a high-level account. Without additional protection the attacker might combine the valid persistency cookie together with the valid username and password combination. In order to prevent this scenario the login username is also included as an encrypted cookie value to verify of login attempt. In short, the cookie must be associated with a specific user and cannot be transferred.

Encryption:
``` 
AAA.USER.NAME.ENCRYPT
``` 
Decryption:
``` 
HTTP.REQ.COOKIE.VALUE(\"nFactor-PersistUser\").DECRYPT
``` 

### Citrix Gateway with nFactor
The nFactor configuration built here, including the “trust this device” check box, can be linked to a Citrix Gateway virtual server as well. For this, a Citrix ADC Advanced or Premium license is required since a custom loginSchema is being utilized. Whilst nFactor is supported on Citrix ADC Standard Edition since 13.0 build 67.x custom loginSchemas are not supported on this license as per [nFactor for Gateway Authentication](https://docs.citrix.com/en-us/citrix-gateway/current-release/authentication-authorization/nfactor-for-gateway-authentication.html) documentation. The Citrix Gateway Advanced VPX licenses also do not support this type of deployment.

You can link the nFactor authentication very simply with the following configuration line:
``` 
add authentication authnProfile prf_auth_nfactor -authnVsName vs_auth_nfactor
``` 
The configuration in the appendix also contains the information to activate this on a Citrix Gateway virtual server behind a Content-Switch.

## Learn more
If you want to learn more about nFactor functionality; please visit the documentation site: [Multi Factor nFactor Authentication](https://docs.citrix.com/en-us/citrix-adc/current-release/aaa-tm/multi-factor-nfactor-authentication.html)

To learn more about the encryption/decryption functionality and advanced policy expressions please visit the following site: [Advanced Policy Evaluation with Complex Test Operations](https://docs.citrix.com/en-us/citrix-adc/current-release/appexpert/policies-and-expressions/advanced-policy-exp-evaluation-text/complex-text-operations.html)

## Appendix
* [SingleAuthPersist.xml](https://gist.github.com/tijlvdb/6b1c1117dea3ae8dc3a31f6e4af82932 "SingleAuthPersist.xml")
* [ns_conf_snip.conf](https://gist.github.com/tijlvdb/059c397fdcfbc23895c6fb74b5c30408 "ns_conf_snip.conf")



