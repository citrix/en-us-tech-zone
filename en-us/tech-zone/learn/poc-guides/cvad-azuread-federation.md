---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of Microsoft AAD Federated Authentication for Citrix Virtual Apps and Desktops with Citrix ADC
---
# Proof of Concept Guide: Microsoft AAD Federated Authentication for Citrix Virtual Apps and Desktops with Citrix ADC

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Sachin Shetty, John Ashman

## Overview

Use of the Cloud to deliver Enterprise services continues to grow. Cloud ervices inherit the benefits built into robust cloud infrastructure including resiliency, scalability, and global reach. Azure Active Directory (AAD) is Microsoft Azure hosted directory service and provides those same cloud benefits to Enterprises.  AAD allows Enterprises to host their employee identities in the cloud to utilize the securely access services hosted On Premises or in the Cloud.

Citrix Virtual Apps and Desktops delivers virtual apps and desktops using resources hosted On Premises or in the Cloud.  Citrix ADC provides secure remote access to those virtual apps and desktops and also may be hosted On Premises or in the Cloud. Together along with the Citrix Federated Authentication Service they can utilize AAD to authenticate user access to Citrix Virtual Apps and Desktops from anywhere.

![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_aad-idp-cvad-fas-adc-sp-architecture1.png)]

## Azure Active Directory Setup

To set up _ perform the following steps:
For more information refer to [TITLE](/en-us/LINK.html).

1.  Create a new
1.  Populate Site Details:
    *  Site

## Citrix Virtual Apps and Desktops Setup

To set up _ perform the following steps:
For more information refer to [TITLE](/en-us/LINK.html).

1.  Create a new
1.  Populate Site Details:
    *  Site

## Citrix Federated Authentication Service Setup

To set up FAS perform the following steps:
For more information refer to [TITLE](/en-us/LINK.html).

1.  Load the Citrix Virtual Apps and Desktops ISO image on the FAS Virtual Machine
1.  Select FAS to begin the installation
![FAS Installation](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0009.png)]
1.  Read the Citrix License Agreement & click Next
1.  Select the installation directory & click Next
1.  Update the host firewall to allow port 80 & click Next
![FAS Ports](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0010.png)]
1.  Click Finish
1.  Review the settings you made & click Install
1.  After installation success click Finish again.
![FAS Finished](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0011.png)]
1.  Under "C:\Program Files\Citrix\Federated Authentication Service" share the PolicyDefinions directory contents and the en-us subdirectory
![FAS Policy Definitions Copy](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0012.png)]
1.  Under "C:\Program Files\Citrix\Federated Authentication Service" paste them to the Domain Controller at C:\Windows\PolicyDefinions and ..\en-US respectively.
    Files include:
    *  PolicyDefinitions\CitrixBase.admx
    *  PolicyDefinitions\CitrixFederatedAuthenticationService.admx
    *  PolicyDefinitions\en-US\CitrixBase.adml
    *  PolicyDefinitions\CitrixFederatedAuthenticationService.adml
![FAS Policy Definitions Paste](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0013.png)]
1.  Open Server Manager > Tools > Group Policy Management
    *  a. Right-click to create new or edit an existing Group Policy Object
    *  b. Navigate to Computer Configuration > Policies > Administrative Templates > Citrix Components > Authentication
    *  b.

## Citrix ADC  Setup

To set up _ perform the following steps:
For more information refer to [TITLE](/en-us/LINK.html).

1.  Create a new
1.  Populate Site Details:
    *  Site

## Citrix Workspace client Setup

To set up _ perform the following steps:
For more information refer to [TITLE](/en-us/LINK.html).

1.  Create a new
1.  Populate Site Details:
    *  Site

## Troubleshooting

The

### Connectivity

Verify that Citrix SD-WAN system requirements have been met and check external firewall rules to ensure required ports are open.

*  Ports – if
*  DNS – if  

### Logs

Verify that Citrix SD-WAN system requirements have been met and check external firewall rules to ensure required ports are open.

*  Ports – if
*  DNS – if  

### Ctx articles

[Citrix ADC Commands to Find the Policy Hits for Citrix Gateway Session Policies](https://support.citrix.com/article/CTX138840) – view authentication policy or session policy hits
[Citrix ADC Commands to Find the Policy Hits for Citrix Gateway Session Policies](https://support.citrix.com/article/CTX207162) – common resolutions to a common access error “Cannot Complete Your Request”

## Summary

Citrix Virtual Apps and Desktops has been a resillient  technology for decades. By integrating AAD as the IDP and Citrix ADC as the Service Provider Enterprise can have even more reliable service by incorporating cloud hosted Identity.

To learn more about  or Citrix ADC pricing and packing visit the Citrix web site and to learn more about its technical capabilities visit Citrix Tech Zone.

## References

For more information refer to:

[TITLE](https://URL.html)

[Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) – Learn how the federated authentication service, integrated with Citrix Workspace, utilizes virtual smartcards to provide single sign-on to Windows-based resources
