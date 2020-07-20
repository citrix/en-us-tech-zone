---
layout: doc
description: Learn how to implement a Proof of Concept environment consisting of Microsoft AAD Federated Authentication for Citrix Virtual Apps and Desktops with Citrix ADC.
---
# Proof of Concept Guide: Microsoft AAD Federated Authentication for Citrix Virtual Apps and Desktops with Citrix ADC

## Contributors

**Author:** [Matthew Brooks](https://twitter.com/tweetmattbrooks)

**Special Thanks:** Sachin Shetty, John Ashman

## Introduction

Use of the Cloud to deliver Enterprise services continues to grow. Cloud services inherit the benefits built into cloud infrastructure including resiliency, scalability, and global reach. Azure Active Directory (AAD) is the Microsoft Azure hosted directory service and provides those same cloud benefits to Enterprises.  AAD allows Enterprises to host their employee identities in the cloud and securely access services also hosted in the Cloud or On Premises.

Citrix Virtual Apps and Desktops delivers virtual apps and desktops using resources hosted On Premises or in the Cloud.  Citrix ADC provides secure remote access to those virtual apps and desktops and also may be hosted On Premises or in the Cloud. Together along with the Citrix Federated Authentication Service they can utilize AAD to authenticate user access to Citrix Virtual Apps and Desktops from anywhere.

![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_0001.png)

## Overview

The guide will demonstrate how to implement a Proof of Concept environment consisting of Microsoft AAD Federated Authentication for Citrix Virtual Apps and Desktops with Citrix ADC using SAML. AAD will act as the Identity Provider (IDP) while Citrix ADC will act as the Service Provider (SP).

It makes assumptions about the installation or configuration of certain components:

*  An Active Directory Server is installed On Premises and you can login as Domain Admin
*  An Azure tenant is available with a P2 license and you can login as a Global Admin
*  A Citrix ADC appliance has been installed, is licensed, has a Citrix Gatewy virtual server and is configured to provide access to an On Premises Citrix Virtual Apps and Desktops environment. Use Version 13 build 60 or higher.
*  A Delivery Controller, StoreFront, and VDA are installed and configured to delivery virtual apps or desktops for domain users. Use version 2006 or higher.
*  A virtual machine is available or another server has enough capacity to install FAS. The DDC, FAS, and StoreFront are all installed on the same server in this POC.
*  The Remote Client is able to launch a virtual app or desktop using the Workspace App or browser. Use Windows Version 20.6.0.38(2006) or higher.

## AD and AAD Config

To configure Active Directory (AD) and Azure Active Directory (AAD) perform the following steps:

### AD

#### Alternative UPN Suffix

1.  Login to your AD domain controller.
1.  Open Server Manager > Tools > Active Directory Domains and Trusts
1.  Right-click, select Properties and enter the UPN Suffix for users corresponding to one of your AAD domains.
![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADAlternativeUPNSuffix.png)

#### AD Users

1.  On your AD domain controller open Server Manager > Tools > Active Directory Users and Computers.
1.  Right-click and select New > User, or edit an existing one
1.  Under Properties > Account set the UPN to the new Suffix. ![AAD-IDP + CVAD + FAS + ADC-SP architecture](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADUser.png)

#### Microsoft Azure Active Directory Connect

Azure AD Connect is a tool for connecting on premises identity infrastructure to Microsoft Azure AD. It will allow us to copy AD users to AAD with a UserPrincipalName (UPN) mapped to our AAD domain.

1.  Login to your AD domain controller or other virtual server where you will host the Microsoft Azure Active Directory Connect process.
1.  Download the executable from the Microsoft download site [Microsoft Azure Active Directory Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594) and launch it.
1.  You will be promoted to accept making changes to the virtual machine and accept a license agreement on the welcome page. ![AD Connect](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADConnectWelcome.png)
1.  You will be prompted to login as a Global AAD admin as well as a Domain Services admin.
1.  For installation on a single AD virtual machine you can follow express settings. After it verifies UPN Suffixes it makes a full sync of all users, groups, and contacts.

See [using Azure AD Connect express settings](https://docs.microsoft.com/en-us/azure/active-directory/hybrid/how-to-connect-install-express) for more information.

#### Certificate Authority

For this POC we assume you have a Certificate Authority, including Web Enrollment, installed on an AD DC. If not navigate to Server Manager > Add roles and features and follow prompts to install Active Directory Certificate Services. See [Microsoft Certificate Authority Installation](https://docs.microsoft.com/en-us/windows-server/networking/core-network-guide/cncg/server-certs/install-the-certification-authority) for more information.

1.  Next launch MMC
2.  Select Add/Remove Snap-in > Certificates > Computer Account > Ok
3.  Right-click Personal > All Tasks > Request New Certificate
4.  Click Next and select Active Directory Enrollment Policy
5.  Select Domain Controller Authentication and click Enroll ![AAD Non-gallery application](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADdomaincontrollerauth.png)

### Azure Active Directory

1.  Login to the [Azure Portal](https://portal.azure.com) as a global admin
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
1.  Select Users and groups > Add user and select existing users or groups that will have access to Citrix Virtual Apps and Desktops using their AAD UPN
![Basic SAML Configuration](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-AADUsersandGroups.png)

## Citrix ADC Config

To configure the Citrix ADC perform the following steps:

1.  Login to the Citrix ADC UI
1.  Navigate to Traffic Management > SSL> Certificates > All Certificates to verify you have your domain certificate installed. In this POC example we used a wildcard certificate corresponding to our Active Directory domain. See [Citrix ADC SSL certificates](/en-us/citrix-adc/13/ssl/ssl-certificates.html) for more information.
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
![IDP Certificate](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCAAAVserver.png)
    *  Signing Certificate Name - select the domain certificate the ADC uses to sign requests to AAD.
    *  Issue Name - enter the FQDN of the Citrix ADC Gateway
1.  Select create to create the action
1.  Enter true for the expression
1.  Select create again to create the policy ![Authentication Policy](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCcreateauthenticationpolicy.png)
1.  Select bind to bind the policy to the Vserver ![Authentication vServer](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCauthvserver.png)
1.  Click continue to complete the configuration of the authentication vServer
1.  Next navigate to Citrix Gateway > Virtual Servers and edit the pertinent vServer
1.  If you have an existing basic policy bound under Basic Authentication select it, check the policy and select Unbind, confirm, and close.
1.  From the menu on the right select Authentication Profile, select Add, enter a name and click the right arrow under Authentication Virtual Server, check the policy auth vServer just created and click create. ![Create Authentication Profile](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCcreateauthprofile.png)
1.  Click Ok and now you will have the completed AAA vServer bound to the Gateway vServer. ![Create Authentication Profile](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-ADCauthprofile.png)
1.  Navigate to Citrix Gateway > Policies > Session and select the Workspace App policy with the "CitrixReceiver" expression and make the following changes:
    *  Under Published Applications clear the field Single Sign-on Domain, and uncheck Global Override
    *  Under Client Experience from the Credential Index drop down select Secondary
1.  Repeat those steps for the Workspace for web policy with the "CitrixReceiver").NOt expression, click Edit, and click Edit again for the Profile.

See [Citrix ADC](/en-us/citrix-adc/13.html) for more information.

## Citrix Virtual Apps and Desktops Config

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
1.  Select Manage Authentication Methods from the menu on the right
1.  Select Pass-through from Citrix Gateway
1.  Select the down arrow next to the gear, and select Configure Delegated Authentication
1.  Check Fully delegate credential validation to Citrix Gateway and click Ok twice ![Create Authentication Profile](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-StoreFrontdelegatedauth.png)
1.  Select Manage Authentication Methods from the menu on the right
1.  Edit the pertinent Citrix Gateway entry
1.  Under Authentication Settings the Callback URL must be configured if it is not done already. Typically you can update the internal DNS or for a single StoreFront instance update the local host file to map the private IP of the Gateway vServer to the FQDN. ![Create Authentication Profile](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-StoreFrontcallback.png)

### Delivery Controller

Next configure the Desktops Delivery Controller to trust the StoreFront servers that can connect to it.

*  Open Powershell as an administrator and run
    *  asnp citrix* (provided you do not have all Citrix snap-ins loaded) See [Install and set up FAS](https://docs.citrix.com/en-us/linux-virtual-delivery-agent/current-release/configuration/federated-authentication-service.html#install-and-set-up-fas) for more information
    *  Set-BrokerSite -TrustRequestsSentToTheXmlServicePort $true

See [Configure the Delivery Controller](/en-us/federated-authentication-service/install-configure.html#configure-the-delivery-controller) for more information.

## Citrix Federated Authentication Service Config

To configure FAS perform the following steps:

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
    *  c. Right-click on Federated Authentication Service.
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

## Citrix Workspace client validation

To validate the POC login from the Workspace client by performing the following steps:

### Workspace for Web

1.  Open a browser and navigate the domain FQDN managed by the Citrix ADC. You will notice the Citrix Gateway redirects to AAD.
1.  Login with the UPN of a user configure to be part of the FAS environment ![Login](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-RemoteWebredirectAADuser.png)
1.  Verify the users virtual apps and desktops are enumerated and launch while being logged in with the AAD user ![Logged in](/en-us/tech-zone/learn/media/poc-guides_cvad-azuread-federation_000-RemoteWebloggedin.png)

## Summary

Citrix Virtual Apps and Desktops has been a resillient  technology for decades. By integrating AAD as the IDP and Citrix ADC as the Service Provider Enterprise can have even more reliable service by incorporating cloud hosted Identity.

To learn more about  or Citrix ADC pricing and packing visit the Citrix web site and to learn more about its technical capabilities visit Citrix Tech Zone.

## References

For more information refer to:

[Federated Authentication Service](/en-us/tech-zone/learn/tech-insights/federated-authentication-service.html) – Learn how the federated authentication service, integrated with Citrix Workspace, utilizes virtual smartcards to provide single sign-on to Windows-based resources
