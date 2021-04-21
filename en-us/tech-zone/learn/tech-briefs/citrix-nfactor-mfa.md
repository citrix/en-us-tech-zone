---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Dan Feller
description: Learn how you can implement various Multifactor Authentication methods with Citrix ADC nFactor Authentication.
---
# Tech Brief: Multifactor Authentication with Citrix nFactor

## Introduction

Weak or stolen passwords are a leading cause of breaches in Enterprise networks. They can lead to loss of Intellectual Property, loss of Personally identifiable information (PII) and result in a significant impact on the business. Multifactor Authentication (MFA) is one of the best security measures to guard against identity vulnerabilities.

Typically there are three types of authentication used to identify users:

1)  Something you know (for example, password) - this type is historically the most common type used. Users enter a user name and a password that only they know. Passwords can be strengthened against attacks from bad actors, with greater amounts of characters and with more types of characters. However, users are only human and may keep them simple or avoid changing them regularly. Then if the endpoint is infected with malware, it is only a matter of time until unrelenting algorithms figure out the password and compromise systems and data that the user has access to.

2)  Something you have (for example, a digital key, physical, or virtual smartcard) - this type is a common second factor, particularly with the US government. Users are issued a smartcard, and after entering their user name and password, a private certificate and key are extracted and validated. Physical cards are inserted into readers attached to endpoints, or virtual cards are installed on the endpoint for the user to copy and pasted into the authentication form.

3)  Something you are (for example, fingerprint scanner) - this type is a third method that focuses on using biometrics to uniquely identify a user. Historically biometrics have seen slower mainstream adoption due to expense to implement and complexity, yet they are a powerful option for high-security environments.

Multifactor authentication pertains to using two or more of these types of authentication to verify user identity and mitigate the risk of bad actors obtaining access to Enterprise environments.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_intro.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_intro.png)

## Overview

The Citrix ADC supports various multifactor authentication methods. It provides an extensible and flexible approach to configuring them with nFactor authentication.

It also supports various application delivery technologies that can utilize multifactor authentication, including Content Switching, Traffic Management Load Balancing, Full VPN, and Gateway proxy. It can be employed in on-premises, Cloud, and Hybrid environments.

This brief describes multifactor authentication using five pairs of methods with Citrix Gateway. It focuses on using the methods with Citrix Virtual Apps and Desktops on-premises environments and with Citrix Workspace.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_overview.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_overview.png)

## nFactor

nFactor uses Citrix ADC AAA Virtual Servers to deploy multifactor authentication. They bind to advanced policies and actions, grouped in factors, to implement authentication methods. The interface to users requesting authentication credentials, and the variables that store their input, are defined in a login schema.

nFactor can be configured through the CLI, through the GUI manually, or through the visualizer tool in the GUI. Pertinent configuration elements include:

*  Visualizer - a tool available in the Citrix ADC GUI to aid with the configuration of nFactor to implement MFA flows for a multitude of authentication requirements.
*  Citrix ADC AAA Virtual Server - "Factor 0" is the starting point for MFA, which is referenced by Gateway, LB, or Content Switch Virtual Servers that rely on it for authentication.
*  Factor - factors, which are bound to the Citrix ADC AAA Virtual Server, act as a "bucket" to contain a set of policies and pertinent schema. (Also known as Policy Labels when the Visualizer is not used)
*  Login Schema - the "landing page" for each authentication factor includes field types and variables referenced throughout the flow.
*  Policy - an object that maps to authentication actions and includes an expression to determine when it’s a match.
*  Action - Defines the various authentication methods. SAML, OAuth, Certificate, LDAP, and so on.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_nfactor.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_nfactor.png)

## Methods

Citrix ADC supports many authentication methods. For more information, see [Citrix ADC Authentication Methods](/en-us/citrix-adc/current-release/aaa-tm/authentication-methods.html). In this brief, we focus on using domain credentials, representing something the user knows, with five variations of something the user has.

1.  Native OTP

2.  Push Token

3.  Email OTP

4.  Group Extraction

5.  Device Certificate

### Native OTP

Native OTP or "One Time Pin" works by the Citrix ADC, having users register with an app that supports OTP and sharing a key with it. Then it uses the current time along with that key to generate a string of numbers, at regular intervals, that only the user's OTP app has (for example Microsoft Authenticator, or Citrix SSO app). By default, it uses a six-digit OTP code that is valid for 30 seconds.

In our scenario, in order to successfully authenticate, the user enters their domain credentials followed by the OTP code. After successful validation by the Citrix ADC, the user's credentials are relayed to the target delivery systems, and their apps sessions can be established with single sign-on.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_nativeotp.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_nativeotp.png)

For more information regarding how to try it out in your environment, see: [Proof of Concept Guide: nFactor for Citrix Gateway with Native OTP Authentication](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-native-otp.html)

### Push Token

With Push Token, the user also does an initial registration with the Citrix ADC. Yet, with this method, the user is not required to copy and paste a code. Instead, the ADC sends a PUSH notification over mobile delivery networks (APNS for Apple devices or GCM for Android devices). Then the user simply has to accept a popup from the Citrix SSO app to complete the second factor. Again, once the user's credentials are relayed to the target delivery systems and their apps, sessions can be established with single sign-on.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_pushtoken.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_pushtoken.png)

For more information regarding how to try it out in your environment, see: [Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Push Token](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-push-token.html)

### Email OTP

Email OTP works like Native OTP, yet the OTP code is sent as an email rather than to an app. This method is valuable for user groups that do not have mobile devices. It works in a similar fashion in that the code generated expires at regular intervals, and the user must copy and paste it into a field along with their credentials.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_emailotp.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_emailotp.png)

For more information regarding how to try it out in your environment, see: [Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Email OTP](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-email-otp.html)

### Group Extraction

Group Extraction is the same type of authentication as entering domain credentials, yet it is able to route to other types by extracting the user's group membership. Then, using the earlier example, admins can designate a group as mobile users or non-mobile users to determine whether their second factor is Push Token and Email OTP. Alternatively, they can designate groups of users according to their security persona and match the number and type of authentication methods according to the groups' risk profile.

Examples of user groups include:

*  Normal-security-group for individuals that have lower security requirements by the nature of their job or limited data access and are located within the bounds of the corporate security perimeter. This group may only require 1 factor.

*  Elevated-security-group for third-party workers or contractors who do not have had background checks done and have higher security requirements. This group may require two or more factors.

*  High-security-group for employees that perform critical jobs and require special government clearance or industry approval. This group may require two or more factors and contextual verifications such as source IP address.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_groupextraction.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_groupextraction.png)

For more information regarding how to try it out in your environment, see: [Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Group Extraction](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-group-extraction.html)

### Device Certificate

Device Certificate relies on the availability of a unique certificate on the endpoint. The Citrix ADC validates that the certificate was issued by a designated Certificate Authority. There are various methods to manage issuing and revoking the certificates. Once in place, it can provide a seamless second authentication factor that requires little or no input from the user.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_devicecertificate.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_devicecertificate.png)

For more information regarding how to try it out in your environment, see: [Proof of Concept Guide: nFactor for Citrix Gateway Authentication with Device Certificate](/en-us/tech-zone/learn/poc-guides/nfactor-citrix-gateway-device-certificate.html)

## Workspace service

Once you have a nFactor flow setup, you can integrate with Workspace service. Within Workspace service under Identity and Access Management, the Citrix Gateway service provides settings required to integrate with the Citrix ADC using OAuth.

1.) Citrix Workspace - configure the Citrix Gateway service to point to the Citrix ADC Virtual Server. Change the Workspace authentication method to Citrix Gateway

For more information regarding how to try it out in your environment, see: [Tech Insight video: Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html)

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_citrixworkspaceiam.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_citrixworkspaceiam.png)

2.) Citrix ADC nFactor - create an Oauth policy using tenant information obtained from Workspace service. Bind it to the pertinent AAA Virtual Server with a higher priority than the nFactor flow. Update the "Logon Point" landing page theme with the Citrix Workspace look and feel.

For more information regarding how to try it out in your environment, see: [Customizing the on-premises Citrix Gateway authentication page to look identical to Citrix Cloud logon page](https://support.citrix.com/article/CTX258331)

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_citrixadcoauth.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_citrixadcoauth.png)

Once configured, users continue to access their Workspace service domain (for example, `https://<customerdomain>.cloud.com`) and are automatically redirected to the Citrix ADC FQDN. Upon successful authentication, Citrix ADC relays the status for the user name back to the Workspace, and the user is presented with their resources.

[![Citrix nFactor MFA](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_workspace.png)](/en-us/tech-zone/learn/media/tech-briefs_citrix-nfactor-mfa_workspace.png)

## Summary

With Citrix nFactor, Enterprises can implement reliable multifactor authentication and fortify the primary entrance to their environments. They can implement this security improvement all while maintaining a good user experience.
