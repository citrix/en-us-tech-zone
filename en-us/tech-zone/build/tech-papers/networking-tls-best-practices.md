---
layout: doc
---
# Citrix Networking SSL / TLS Best Practices

## Contributors

**Author:** [Jake Rutski](https://twitter.com/JRutski), [Steven Wright](https://twitter.com/stevenwrightuk)

## Overview

This Tech Paper provides the steps necessary to validate the existing SSL\TLS configuration of a vServer running on a Citrix ADC and ways to ensure that best practices are applied. We will cover configuration items such as the certificate chain bound to the vServer, cipher suite settings and disabling older protocols that are vulnerable to attack.

There are many tools that can be used to validate the configuration of a public-facing site protected by Citrix ADC - one such tool is the [**SSL Server Test by Qualys SSL Labs.**][SSLLabs] It performs a robust series of tests against your server and provides an easy to read score card that you can use to make improvements to your configuration. The scan is free and only takes about a minute to complete.

[SSLLabs]: https://www.ssllabs.com/ssltest/index.html

Qualys actively develop the SSL test and as such the tests are likely to change in the future as new protocols are created and new vulnerabilities are found. Because of this, it is good practice to test sites regularly to make sure that any new vulnerabilities are not exposed.

_**Note:** SSLLabs will post the server URL being tested along with the final score on their public dashboard, unless the option to **Do not show the results on the boards** is chosen._

## Configuration Items that Should be Validated

-  **Certificates** - Is the full chain provided and trusted? Is the signature algorithm secure?
-  **Protocols, Keys and Cipher Support** - Which SSL and TLS protocol versions are supported? Which cipher suites are preferred and in what order? Do the provided cipher suites support forward secrecy?
-  **TLS Handshake Simulation** - Determines which protocol and cipher will be negotiated by several different clients and browsers
-  **Protocol Details** - Is Secure Renegotiation supported? Is strict transport security (HSTS) supported?
-  **Known Vulnerabilities** - Is the server vulnerable to attacks such as POODLE, BEAST or TLS downgrade?

Once the SSLLabs test is completed a letter grade is presented along with a point scale for each of 4 categories:

  1 Certificate
  2 Protocol Support
  3 Key Exchange
  4 Cipher Strength

Each of the above categories receives a numerical score that is then averaged into a total score. There are also special cases or configurations that will limit the final grade or take points away - such as when a certificate is not trusted, or if SSLv3 is enabled. Full documentation on how SSL Labs tests are graded can [be found here.][Grading]

[Grading]: https://github.com/ssllabs/research/wiki/SSL-Server-Rating-Guide

## Implementation Concerns

Some of the configuration steps noted in this article may cause connectivity issues with older clients and browsers. For example, Windows XP SP2 does not support SHA256 certificates; older web browsers may not support TLS1.2 or ECC ciphers. In cases where support is missing, the client may experience error messages or an inability to display the site.

_**Note:** See the Firmware Notes section for required builds and other notes regarding specific ADC firmware_

## Basic Steps - GUI

The following are general steps that should be taken first to ensure a high score on the SSL Labs test.

-  Ensure that the ADC is running a recent firmware release - 10.5 build 67 or later is recommended

    -  Disable SSLv3 on all vServers (or SSL profiles if using SSL Default Profile) in the **SSL Parameters** section of the vServer configuration

  ![SSLv3-Disabled](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_sslv3-disable.png)

-  Ensure that the certificate chain is complete and trusted
    -  Certificates are not always signed by a CA that every endpoint inherently trusts; often times they are signed by an intermediate CA
    -  The intermediate certificate should be installed on the ADC then linked to the server certificate that is bound to the vServer
    -  Intermediate certificates should be provided by the vendor that provided the server certificate, often in a 'certificate bundle'; they can also usually be found on the vendors public site
    -  There may be multiple intermediate certificates that need to be installed and linked; in order for the server certificate to function, the client must receive a certificate chain that ends with a CA certificate that the client already trusts
    -  The root CA certificate used to sign the intermediate certificate will likely be trusted by all clients
    -  To install the Intermediate certificate, go to: _Traffic Management > SSL > Certificates > CA Certificates_ and choose _Install_ (_**Note:** earlier builds of Citrix ADC do not have the 'CA Certificates' option in the GUI_)
    -  Once the intermediate certificate is installed, it can be linked to the server certificate by selecting the certificate and choosing _link_ from the action menu.
    -  If the correct intermediate certificate is installed, it should automatically populate in the linking menu.

![CA-Certificate-Install](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_ca-cert.png)

![Certificate-Link](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_cert-link.png)

![Certificate-Linked](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_cert-link-2.png)

-  Ensure that TLSv1.2 is enabled on all vServers (or SSL profiles if using SSL Default Profile) in the **SSL Parameters** section of the vServer configuration

![TLSv12-Enabled](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_tlsv12-enable.png)

-  Allow secure renegotiation
    -  Go to _Traffic Management > SSL_ and select **Change advanced SSL settings**
    -  Set _Deny SSL Renegotiation_ to **NONSECURE** to allow only clients that support RFC 5746 to renegotiate

![Secure-Renegotiate](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_secure-renegotiate.png)

-  Create a DH key to be used by the DHE cipher suites
    -  _**Note:** creating and binding a DH key is optional, slower and only useful for older clients that lack ECDHE support. If a DH key is not bound, DHE cipher suites will be ignored._
    -  Go to _Traffic Management > SSL_ and select **Create Diffie-Hellman (DH) key**
    -  In the _DH Filename_ field, enter a filename for the key file (Note: the default path on the appliance is /nsconfig/ssl/)
    -  Enter the desired DH key size in _DH Parameter Size_ - either 1024 or 2048 (Note: 4096 bit DH keys are not currently supported)
    -  _**Note:** DH key size is expected to be the same size as the RSA key and in most cases 2048 bit_
    -  Choose either the 2 or 5 random number Generator
    -  Click _Create_ - key generation may take some time

![DH-Generate](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_dh-generate.png)

-  Bind the DH key to the vServer
    -  On the selected vServer, open the _SSL Parameters_ section and click the edit button (pencil in the top right corner)
    -  Check the box to **Enable DH Param**
    -  Set the File Path to the previously created key file

![DH-Bind](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_dh-bind.png)

-  Create a custom cipher group that provides Forward Secrecy (FS)
    -  Go to _Traffic Management > SSL > Cipher Groups_ and choose **Add**
    -  Enter a name for the Cipher group
    -  Click **+ Add** then expand the **+ ALL** section - select the following ciphersuites:
        -  TLS1.3-CHACHA20-POLY1305-SHA256
        -  TLS1.3-AES128-GCM-SHA256
        -  TLS1.3-AES256-GCM-SHA384
        -  TLS1.2-ECDHE-ECDSA-AES256-SHA384
        -  TLS1.2-ECDHE-ECDSA-AES256-GCM-SHA384
        -  TLS1-ECDHE-ECDSA-AES256-SHA
        -  TLS1.2-DHE-RSA-AES256-GCM-SHA384
        -  TLS1.2-ECDHE-ECDSA-AES128-GCM-SHA256
        -  TLS1.2-ECDHE-RSA-CHACHA20-POLY1305
        -  TLS1.2-ECDHE-ECDSA-CHACHA20-POLY1305
        -  TLS1.2-ECDHE-RSA-AES256-GCM-SHA384
    -  Click the **>** right arrow to move the ciphers from the _Available_ column to the _Configured_ column
    -  Click **Create**

![Create-Group](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_create-group.png)

-  Bind the Cipher Group to the vServer
    -  On the selected vServer, open the _SSL Ciphers_ section and click the edit button (pencil in the top right corner)
    -  Remove any existing ciphers or cipher groups by using the _Remove All_ button or the _-_ icon on each item
    -  Select the **Cipher Groups** radio button and choose the previously created Cipher Group
    -  Click OK

![Add-Group](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_add-group.png)

![Add-Cipher-Group](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_add-cipher-group.png)

-  Enable Strict Transport Security (HSTS) on the vServer
    -  On the selected vServer, open the _SSL Parameters_ section and click the edit button (pencil in the top right corner)
    -  Check the box to enable **HSTS**
    -  Enter **157680000** in the _Max Age_ field
    -  **Note:** this flag is available in builds 12.0 35 and later. For earlier builds, [see this article to add HSTS support.][LegacyHSTS]

[LegacyHSTS]: https://support.citrix.com/article/CTX205221

![HSTS-Enable](/en-us/tech-zone/build/media/tech-papers_networking-tls-best-practices_hsts-enable.png)

## Basic Steps - CLI

The following are general steps that should be taken first to ensure a high score on the SSL Labs test. In all of the CLI examples below, the name of the SSL vServer is listed as **Ex-vServer** - it should be replaced with the name of the SSL vServer in your environment.

-  Ensure SSL3 is disabled and TLS1.2 is enabled on a virtual server named _Ex-vServer_

```bash
set ssl vserver Ex-vServer -ssl3 DISABLED -tls1 ENABLED -tls11 ENABLED -tls12 ENABLED
```

-  Allow secure renegotiation

```bash
set ssl parameter -denySSLReneg NONSECURE
```

-  Create and bind a DH key to a virtual server named _Ex-vServer_

```bash
create ssl dhparam DH_Key_Name_Here.key 2048 -gen 2

set ssl vServer Ex-vServer -dh ENABLED -dhFile DH_Key_Name_Here.key
```

-  Create a custom cipher group that prefers ECDHE and ECDSA cipher suites

```bash
add ssl cipher New_APlus_CipherGroup

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.3-CHACHA20-POLY1305-SHA256

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.3-AES128-GCM-SHA256

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.3-AES256-GCM-SHA384

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-ECDSA-AES256-SHA384

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-ECDSA-AES256-GCM-SHA384

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1-ECDHE-ECDSA-AES256-SHA

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-DHE-RSA-AES256-GCM-SHA384

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-ECDSA-AES128-GCM-SHA256

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-RSA-CHACHA20-POLY1305

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-ECDSA-CHACHA20-POLY1305

bind ssl cipher New_APlus_CipherGroup -cipherName TLS1.2-ECDHE-RSA-AES256-GCM-SHA384
```

-  Unbind the default cipher group from the vServer and bind the custom group

```bash
unbind ssl vServer Ex-vServer -cipherName DEFAULT

bind ssl vServer Ex-vServer -cipherName New_APlus_CipherGroup

bind ssl vServer Ex-vServer -eccCurveName ALL
```

-  Enable Strict Transport Security (HSTS) on a virtual server named _Ex-vServer_

```bash
set ssl vServer Ex-vServer -HSTS ENABLED -maxage 157680000
```

## Additional Settings

### SHA1 Certificates

Certificates that are signed with SHA1 are considered weak, and will prevent a high grade in the SSLLabs test. If any certificates are SHA1 signed, they should be renewed with a SHA256 certificate and installed on the ADC.

### DNS CAA

DNS Certification Authority Authorization (CAA) allows CAs to validate if they are authorized to issue certificates for a domain and provide a contact if something goes wrong.

This is configured on DNS servers, rather than the ADC appliance.

## Firmware Notes

As new vulnerabilities are discovered, they will be tested for by SSLLabs so frequent testing is recommended. Some vulnerabilities are addressed by Citrix ADC code enhancements.

-  Minimum required version: 10.5 build 67

-  The ROBOT vulnerability was addressed in builds **12.0 build 53, 11.1 build 56, 11.0 build 71 and 10.5 build 67** - [more details are available here](https://support.citrix.com/article/CTX230238)

-  The HSTS (Strict Transport Security) flag became available in **12.0 build 35** - prior builds required a rewrite policy to insert the HSTS header. You **cannot** use both as this will cause 2 headers to be inserted which is not allowed.

-  Support for TLS1.2 was added to the VPX appliances in **10.5 build 57**; it was available in earlier builds for appliances with dedicated SSL hardware

-  Support for TLS1.3 was added in **12.1 build 49.23** - it must be enabled in the vServer\SSLProfile, and TLS1.3 ciphers (listed) must be bound

-  ECC certificate support was added to the VPX appliances in **12.0 build 57**; it was available in earlier builds for appliances with dedicated SSL hardware

-  The Zombie POODLE vulnerability was addressed in builds **12.1 build 50.31, 12.0 build 60.9, 11.1 build 60.14, 11.0 build 72.17, and 10.5 build 69.5**; this vulnerability only affects MPX\SDX appliances with Nitrox SSL hardware, meaning that MPX\SDX appliances with Coleto Creek are not vulnerable; disabling CBC-based cipher suites will also mitigate this vulnerability. [Please see this CTX article for more information](https://support.citrix.com/article/CTX240139)

-  The cipher list has been modified to address CBC weaknesses, thus removing 0xc028 and 0x39 ciphers
