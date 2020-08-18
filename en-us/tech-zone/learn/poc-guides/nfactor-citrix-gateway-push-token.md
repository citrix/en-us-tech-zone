---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of Citrix Workspace Push Authentication with Citrix Gateway
---
# Proof of Concept Guide: Citrix Workspace Push Authentication with Citrix Gateway

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** [Daniel Feller](https://twitter.com/djfeller)

## Introduction

Time Based One Time Passwords (TOTP) are an increasingly common method to provide an authentication that can increase security posture in conjunction with other factors. TOTP with PUSH takes advantage of mobile devices by allowing users to receive and accept authentication validatation requests at their finger tips. The exchange is secured by applying a hash to a shared key, distributed during setup.

Citrix Gateway supports PUSH authentication for a variety of services including web services, VPN, and Citrix Virtual Apps and Desktops. In this POC Guide we will demonstrate using it for authentication in a Citrix Virtual Apps and Desktops environment.

Citrix Workspace integrates with Citrix Gateway to support TOTP PUSH for Workpspace authentication.

![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_conceptualarchitecture.png)

## Overview

The guide will demonstrate how to implement a Citrix Workspace Proof of Concept environment using two factor authentication with Citrix Gateway. It will use LDAP to validate Active Directory credentials as the first factor and use Citrix Workspace Push Authentication as the second factor. It will use a Citrix Virtual Apps and Desktops published virtual desktop to validate connectivity.

It makes assumptions about the completed installation and configuration of the following components:

*  Citrix Gateway installed, licensed and configure with an externally reachable vServer bound to a wildcard certificate.
*  Citrix Gateway integrated with a Citrix Virtual Apps and Desktops using Ldap for authentication
*  Citrix Cloud account established
*  Endpoint with Citrix Workspace App installed
*  Mobile device with Citrix SSO App installed

## Citrix Gateway

### nFactor

1.  Login to the Citrix ADC UI
1.  Navigate to Traffic Management > SSL> Certificates > All Certificates to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.
1.  Navigate to Security > AAA - Application Traffic > Virtual Servers and select Add
1.  Enter the following fields and click OK:
    *  Name - a unique value
    *  IP Address Type - Non Addressable
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_000.png)
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Navigate to ___

1.  Select No OAuth IDP Policy, and select Add
1.  Enter a name for the policy, enter true for the expression, and select Add next to Profile
1.  Populate the following fields and click OK:
    *  Name - a unique value
**We will enter values in the following fields to integrate with Citrix Cloud - PUSH Service**
    *  Login to Citrix Cloud and navigate to Identity and Access Management from the hamburger menu on the top left.
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_000.png)
    *  Client ID - copy & paste the Client ID from the Citrix Cloud page
    *  Client Secret - copy & paste the Client ID from the Citrix Cloud page
    *  Customer ID - copy & paste the Client ID from the Citrix Cloud page
![PUSH Authentication](/en-us/tech-zone/learn/media/poc-guides_nfactor-citrix-gateway-push-token_000.png)
1.  Click Create

## User Endpoint

Now we will test PUSH by registering a mobile device and authenticating into our Citrix Virtual Apps and Desktops environment.

### Registration with Citrix SSO

1.  _
1.  _

### Citrix Virtual Apps and Desktops Authentication, Publication, and Launch

1.  Navigate to

## Summary

With Citrix Workspace and Citrix Gateway Enterprises can improve their security posture by implementing multi-factor authentication without making the user experience complex. Users can get access to all of their Workspaces resources by entering their standard domain user and password and simply confirming their identify with the push off a button in the  Citrix SSO app on their mobile device.

## References

For more information refer to:

[Authentication Push](/en-us/tech-zone/learn/tech-insights/authentication-push.html) – watch a Tech Insight video regarding the use of TOTP to improve authentication security for your Citrix Workspace

[Authentication - On-Premises Citrix Gateway](/en-us/tech-zone/learn/tech-insights/gateway-idp.html) – watch a Tech Insight video regarding integrating with On Premises Citrix Gateway to improve authentication security for your Citrix Workspace
