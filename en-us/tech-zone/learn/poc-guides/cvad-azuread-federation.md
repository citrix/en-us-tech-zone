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

![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0001.png)

## AD and AAD Setup

To set up Active Directory (AD) and Azure Active Directory (AAD) perform the following steps:

### AD

#### Alternative UPN Suffix

1.  Login to your AD domain controller.
1.  Open Server Manager > Tools > Active Directory Domains and Trusts
1.  Right-click, select Properties and enter the UPN Suffix for users corresponding to one of your AAD domains.
![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADAlternativeUPNSuffix.png)

#### AD Users

1.  On your AD domain controller open Server Manager > Tools > Active Directory Users and Computers.
1.  Right-click and select New > User, or edit an existing one
1.  Under Properties > Account set the UPN to the new Suffix.
![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADUser.png)

#### Microsoft Azure Active Directory Connect

1.  From your AD domain controller or other virtual server where you will host the Microsoft Azure Active Directory Connect process.
1.  Download the executable from the Microsoft download site [Microsoft Azure Active Directory Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) and launch it.
1.  You will be promoted to accept making changes to the virtual machine and accept a license agreement on the welcome page.
![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADConnectWelcome.png)
1.  You will be prompted to login as a Global AAD admin as well as a Domain Services admin.
1.  For installation on a single AD virtual machine you can follow express settings. After it verifies UPN Suffixes it makes a full sync of all users, groups, and contacts.

See [using Azure AD Connect express settings](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-express) for more information.

#### Certificate Authority

For this POC we assume you have a Certificate Authority, including Web Enrollment, installed on an AD DC. If not navigate to Server Manager > Add roles and features and follow prompts to install Active Directory Certificate Services.  See below for more information.

1.  Launch MMC
1.  Select Add/Remove Snap-in > Certificates > Computer Account > Ok
1.  Right-click Personal > All Tasks > Request New Certificate
1.  Click Next and select Active Directory Enrollment Policy
1.  Select Domain Controller Authentication and click Enroll

See [Microsoft Certificate Authority Installation](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) for more information.

### Azure Active Directory

1.  Login to the [Azure Portal](https://portal.azure.com) as a global admin.
1.  Navigate to Azure Active Directory > Enterprise Applications
1.  Select New application
1.  Select Non-gallery application ![AAD Non-gallery application](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-AADNonGalleryApplication.png)
1.  Enter a unique name and select Add
1.  Select Single Sign-On > SAML and select the pencil icon to edit the Basic SAML Configuration
1.  Enter the FQDN of the Citrix ADC gateway virtual server in the Identifier field.
1.  Enter the FQDN with the uri /cgi/samlauth added in the Reply URL field ![Basic SAML Configuration](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-AADBasicSAMLConfiguration.png)
1.  Click save
1.  Capture the following to be entered in the Citrix ADC SAML configuration:
    *  Under SAML Signing Certificate - download Certificate (base64)
    *  Under Setup Citrix FAS - Login & Logout URL
![AAD settings](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-AADcapturesettings.png)
1.  Select Users and groups > Add user and select existing users or groups that will have access to Citrix Virtual Apps and Desktops using their AAD UPN ![Basic SAML Configuration](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-AADUsersandGroups.png)

See [Azure AD Connect sync](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-sync-whatis) for more information.

## Citrix ADC Setup

To set up the Citrix ADC perform the following steps:

1.  Login to the Citrix ADC UI
1.  Navigate to Traffic Management > SSL> Certificates > All Certificates to verify you have the certificate your domain certificate installed.  In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.
1.  Navigate to Security > AAA - Application Traffic > Virtual Servers and select Add
1.  Enter the following fields and click OK:
    *  Name - a unique value
    *  IP Address Type - Non Addressable
![Basic SAML Configuration](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCAAAVserver.png)
1.  Select No Server Certificate, select the domain certificate, click Select, Bind, and Continue
1.  Select No Authentication Policy, and select Add
1.  Enter a name, set Action Type to SAML, and select Add Action
1.  Enter the following fields and click OK:
    *  Name - a unique value
    *  Unselect Import Metadata
    *  Redirect URL - Paste the Login URL copied from the AAD config
    *  Single Logout URL - paste the Logout URL copied from the AAD config
    *  Logout binding - Redirect
    *  IDP Certificate Name - select Add, enter a name, select Certificate File Name > local, and select the SAML Signing Certificate (base64) downloaded from AAD
![SAML Signing Certificate](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCAAAVserver.png)
1.  NEXT

See [Citrix ADC](/en-us/citrix-adc/13.html) for more information.

## Citrix Virtual Apps and Desktops Setup

To integrate Citrix Virtual Apps and Desktops components with FAS perform the following steps:

### StoreFront

#### Enable FAS on StoreFront

*  Open Powershell as an administrator and run:
    *  Get-Module "Citrix.StoreFront.*" -ListAvailable | Import-Module
    *  $StoreVirtualPath = "/Citrix/Store"
    *  $store = Get-STFStoreService -VirtualPath $StoreVirtualPath
    $auth = Get-STFAuthenticationService -StoreService $store
    *  Set-STFClaimsFactoryNames -AuthenticationService $auth -ClaimsFactoryName "FASClaimsFactory"
    *  Set-STFStoreLaunchOptions -StoreService $store -VdaLogonDataProvider "FASLogonDataProvider"

See [Enable the FAS plug-in on StoreFront stores](/en-us/federated-authentication-service/install-configure.html#enable-the-fas-plug-in-on-storefront-stores) for more information.

#### Configure StoreFront for Citrix Gateway

1.  Login to the StoreFront virtual machine (also configured as FAS and DDC in our POC) and launch the StoreFront GUI
1.  Select Manage Authentication Methods
1.  Enter the following fields and click OK:
    *  Name - a unique value
    *  IP Address Type - Non Addressable

### Delivery Controller

Next configure the Desktops Delivery Controller to trust the StoreFront servers that can connect to it.

*  Open Powershell as an administrator and run
    *  asnp citrix* (provided you do not have all Citrix snap-ins loaded) See [Install and set up FAS](https://docs.citrix.com/en-us/linux-virtual-delivery-agent/current-release/configuration/federated-authentication-service.html#install-and-set-up-fas) for more information
    *  Set-BrokerSite -TrustRequestsSentToTheXmlServicePort $true

See [Configure the Delivery Controller](/en-us/federated-authentication-service/install-configure.html#configure-the-delivery-controller) for more information.

## Citrix Federated Authentication Service Setup

To set up FAS perform the following steps:

1.  Load the Citrix Virtual Apps and Desktops ISO image on the FAS Virtual Machine
1.  Select FAS to begin the installation
![FAS Installation](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0009.png)
1.  Read the Citrix License Agreement & click Next
1.  Select the installation directory & click Next
1.  Update the host firewall to allow port 80 & click Next
![FAS Ports](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0010.png)
1.  Click Finish
1.  Review the settings you made & click Install
1.  After installation success click Finish again.
![FAS Finished](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0011.png)]
1.  Under "C:\Program Files\Citrix\Federated Authentication Service" share the PolicyDefinions directory contents and the en-us subdirectory
![FAS Policy Definitions Copy](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0012.png)
1.  Under "C:\Program Files\Citrix\Federated Authentication Service" paste them to the Domain Controller at C:\Windows\PolicyDefinions and ..\en-US respectively.
    Files include:
    *  PolicyDefinitions\CitrixBase.admx
    *  PolicyDefinitions\CitrixFederatedAuthenticationService.admx
    *  PolicyDefinitions\en-US\CitrixBase.adml
    *  PolicyDefinitions\CitrixFederatedAuthenticationService.adml
![FAS Policy Definitions Paste](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0013.png)
1.  Open Server Manager > Tools > Group Policy Management
    *  a. Right-click to create new or edit an existing Group Policy Object that applies to all pertinent VDAs and Delivery Controllers. (We use the Default Domain Controllers policy for the POC. For production you would typically create a new policy or edit another pertinent policy.)
![GPO](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0014.png)
    *  b. Navigate to Computer Configuration > Policies > Administrative Templates > Citrix Components > Authentication
    *  C. Right-click on Federated Authentication Service.
    *  d. Select edit
    *  e. Select Show DNS
    *  f. Enter the FQDN of the FAS server, click Ok twice, and close the Group Policy Management editor.
![FAS GPO](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0015.png)
    *  g. Navigate to each Delivery Controller, and VDA), open a MS-DOS prompt as Administrator, and run "gpupdate /force"
![FAS GPO update](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0016.png)
    *  h. To verify it's been applied open regedit.exe and navigate to: /Computer\HKLM\SOFTWARE\Policies\Citrix\Authentication\UserCredentialService\Addresses Address1 entry set to the fqdn applied through the GPO. If it does not appear you may need to reboot the respective virtual machine.
![FAS GPO registry](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0017.png)
    *  i. Next return to the FAS virtual machine to begin the service installation. (We host FAS, StoreFront, and the DDC on the same VM for the POC. For production you would typically host them on different VMs for improved scalability and supportability.)
    *  j. Run the Citrix Federated Authentication Service program. Select each of the five steps and following instructions:
       *  i. Deploy certificate templates
       *  ii. Setup a certificate authority
       *  iii. Authorize this service - for this step you will need to return to the CA to issue a  pending request. The CA is hosted on the Domain Controller in this POC example.
       *  iv. Create Rule - here specify the CA and certificate already configured. Also filter the VDAs and users that should be allowed to use the FAS service.
       *  v. (Connect to Citrix Cloud - in this guide we use OnPremises Citrix Virtual Apps and Desktops)

See [FAS documentation](/en-us/federated-authentication-service.html) for more information.

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
