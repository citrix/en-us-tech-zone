---
layout: doc
h3InToc: true
contributedBy: Karthick Srivatsan
specialThanksTo: Matthew Brooks
description: Learn how to set up Citrix Secure Internet Access in conjunction with Citrix SD-WAN to provide secure access to SaaS and Web applications anywhere, reliably and securely.
---
# Tech Brief: Deploy Citrix SIA with Citrix SD-WAN

## Introduction

This Tech Brief explains how to deploy Citrix SIA with Citrix SD-WAN

## Overview

Citrix Workspace has been used successfully by enterprises of all sizes for nearly three decades. Customers have choices in how they license, deploy, integrate, and manage these technologies. This flexibility allows Citrix technologies to serve various use cases, business types, integration requirements, and deployment models. Broad enterprise adoption has led to a diverse set of deployments to meet use cases, and the evolution of networking technologies. Many different factors drive deployment selection including location of data centers (on-prem, cloud or hybrid model), users, branches, management service, and choices for networking connectivity.

## Scope

In this Proof-of-Concept guide, you will experience the role of a Citrix administrator who may create a connection between the organization’s edge SD-WAN device to the Citrix SIA Cloud via IPSec tunnels.

This guide showcases how to perform the following actions:

*  Configuration of a new site on the Orchestrator for the Proof of concept
*  Deployment of the site via Seamless automation of IPSec tunnels to Citrix SIA cloud from the Orchestrator
*  Verification of the IPSec tunnel in both the Orchestrator and Citrix SIA Cloud platform
*  Installation of SIA software agents (also known as Cloud Connector Agent) on the laptop with a specific Security Group and exercise security policies for Internet traffic
*  Demonstration of various SIA+SD-WAN integration use-cases made available in this PoC guide.
*  Configure Web security policies/CASB/Malware Protection policies to allow/deny access to certain websites or categories via Citrix SIA Console.
*  Initiate web traffic and verify the block and allow functionality
*  Show Reporting and Analytics

## Benefits

### Networking and Deployment Use-cases/Benefits

*  **Seamless SIA for managed devices and reliable, secure DC workloads access via SD-WAN Virtual Path** With Citrix SIA software agents (also known as Cloud Connector Agent) based managed devices from the branches managed by SD-WAN, it is easy to use the agents to have secure internet access and yet forward the branch to datacenter workload traffic via Secure and reliable virtual path for seamless application experience and security for internet access

*  **SD-WAN’s multi-wan link reliable IPSec tunnel for local subnets based Secure Internet access of agentless devices** With devices BYOD or personal laptops that are not managed by the customer, SD-WAN’s highly reliable (tunnel reliability via multiple wan links) is able to exercise the Citrix SIA cloud based security policies to maintain the enterprise security posture

*  **Simple security posture for Guest domains in a branch via DNS redirection or IPSec tunnel** With a separate tunnel/Local subnet for guest domains and related security group mapping

### Benefits of seamless Integration/Automation/Management between Citrix SIA and SD-WAN

![Benefits 1 - Automated & Resilient Connectivity on Day 0](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_benefit1.png)
![Benefits 2 - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_benefit2.png)

## Citrix SD-WAN + SIA Overall Topology

![Topology - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_integrationtopology.png)

## Citrix SD-WAN + SIA Integration Use cases

![Usecases Illustration - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_integrationusecases.png)

## PoC Prerequisites

### General Pre-Requisites

*  Citrix SD-WAN 110/210 for an on-premise hardware based PoC.
    *  Can be any appliance you choose for the PoC. Citrix SIA works with ANY form-factor
    *  **Note:** PoC can also be performed on an Azure VPX based SD-WAN with a windows VM behind the VM on the LAN with/without a SIA software agents (also known as Cloud Connector Agent)
*  A Windows or MAC Laptop
*  Agent Windows/MAC MSI download from Citrix SIA cloud platform

### Network Requirements

*  Port / Firewall settings to SIA – Outbound connections

![Network Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_networkprereqs.png)

### Orchestrator - Custom Application to bypass SIA agent traffic from IPSec tunnel in the Citrix SD-WAN

    Recommended Best Practice:

    It is a recommended best practice that the SIA agent traffic from corporate managed devices be bypassed from the local IPSec tunnel of the SD-WAN in the branch edge. This allows direct proxy to the Citrix SIA where enterprise OU’s or security groups can be exercised directly via cloud connectors on the managed devices.

To allow for the Citrix SIA agent to seamlessly call home to the Citrix SIA gateway cluster and PoP’s, it is critical to bypass the cloud connector based traffic from the IPSec tunnel so that the registration of the cloud connector happens and the proxy is made available directly from the connector.

    Note : It is NOT desired to have the SIA agent traffic go through the IPSec tunnel as there are some issues with agent registration and policies to be exercised on the initiated traffic from the endpoint with the SIA agent

**Points to consider in creation of the SD-WAN Bypass custom application:**

The custom application is based on the port list mentioned above in section Ports List of PoC Prerequisites.
All the bypassed specific IP/Protocols tagged as a custom application in the SD-WAN is sent via Internet Service instead of letting it pass by the IPSec tunnel with Citrix SIA tunnel endpoint

**Few things to get handy from the Citrix SIA platform are:**

*  Citrix SIA Cloud Node IP’s part of the gateway cluster that is contacted by the cloud connector (SIA Agent) over port 443 as a proxied connection that needs to be sent via Internet service and bypassed from the tunnel

*  Citrix SIA Reporter node IP part of the node group management that is contacted for sending stats/reporting from the cloud connector (SIA Agent) that needs to be sent over Internet service and bypassed from the tunnel

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_ccbypassorchestrator.png)

**Node in Orange –** Reporter Node with IP – 104.225.171.47 and represented with a different icon (before node name).

**Node(s) in Green –** Cloud PoP nodes part of a CSIA gateway cluster with IPs 104.225.164.53 and 104.225.181.63 with the globe icon. A CSIA gateway cluster is an aggregate of two or more CSIA gateway nodes.

**Note :** When doing your PoC, check the nodes as per your account and apply the Orchestrator bypass rules accordingly. The accounts almost usually always have a reporter node but the gateway ndoes can be one or more. Ensure to place bypass to all gateway nodes so that they are accounted during the bypass (since the traffic can go to any cloud gateway node).

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_orchestratorbypasspolicy.png)

### Orchestrator – Internet service cost to be made higher than SIA service cost (45) IF ALL Internet traffic is to make through the IPSec tunnel

While creating the Citrix Secure Internet Access service, there is a default SIA group where we provide the definitions on what traffic is steered through the IPSec tunnel.

The administrator can either route “ALL APPS” or specific applications through the IPSec tunnel as part of the Citrix SIA service.

*  **Route ALL APPS via Citrix SIA Service in Orchestrator under SIA service Default SIA Group**

![Route ALL Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allappsrouteviasiaservice.png)

*  **Route Specific APPS via Citrix SIA Service in Orchestrator under SIA service Default SIA Group**

![Route Specific Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_specificappsrouteviasiaservice.png)

**Note** If specific apps are chosen, then only specific application routes are created with the Citrix SIA as the service type to be steered through the IPSec Tunnel. The Citrix SIA service is INTRANET type service by default

**Important Recommendation when using Citrix SIA service for ALL APPS routing along with Internet Service**
If an administrator chooses “ALL APPS” to be routed via Citrix SIA, then this means that there is a DEFAULT route 0.0.0.0/0 created with Citrix SIA service with a cost of 45 ( Note that the Citrix SIA service-based routes are with cost 45)

An Internet service may have already been created or may need to be created on the SD-WAN for a specific usecase. For instance - Internet service to breakout CSIA agent related traffic, Internet service to breakout Orchestrator traffic or a brownfield deployment where the administrator may have internet service already for some scenarios to which SIA is being configured. The administrator has to importantly note the routing here.

**General Recommendation :**

**Recommendation 1 :** If the CSIA service is to be used for ALL apps but some usecases need Internet service, then create specific custom applications or use DPI engine with specific applications to be steered via Internet service. This helps application steering more contextual (upon classification)

**Recommendation 2 :** If CSIA service is to be used for some applications and Internet service as default route, ensure that specific applications are chosen for Citrix SIA service breakout.

**Issues you may face if "ALL APPS" is chosen for routing via both CSIA and Internet service and how to overcome that:**

    Sometimes you may have missed to add specific apps to either services. This means you have configured both CSIA and Internet service with "ALL APPS" which installs a default route with the services. Since by design, Internet Service cost is 5 and the CSIA service cost is 45, ALL traffic prefers to be routed over Internet service and you might see that CSIA service may not function as expected.

    To avoid this, it is recommended to use one of the above 2 recommendations. Or you could modify the internet service cost as “50” so that CSIA service is more preferred (with a cost of 45)

![Route Specific Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_inetcostchangeforallapps.png)

## Configuration of a PoC 110/210 for Citrix SIA + Citrix SD-WAN Demo

**Assumption :** Note that it is assumed that the Orchestrator is already configured with an MCN with which the 110 branch forms a virtual path to showcase the demo of SD-WAN in the PoC.

### Orchestrator 110/210 site Configuration for demonstration of IPSec from the branch edge in PoC

1.  Create a new site in the Orchestrator and provide:
    *  Site Name
    *  Choose On-Premises
    *  Provide the site address (Location from where the PoC is done)

    ![Route Specific Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_110sitecreationdetails.png)

2.  Enter the necessary details for the new site added
    *  Enter the device model – 210
    *  In this guide the reference device is a 210. Based on your device in the PoC select appropriately
    *  Enter the Sub-Model: LTE
    *  Enter the device edition: SE
    *  Enter the Site Role: Branch

    ![210 new site creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_sitedetails210.png)

3.  Enter the Interface details to define LAN/WAN interfaces.

    For the purpose of this PoC guide demo

    *  Deployment mode type: Gateway Mode
    *  Interfaces – 1 LAN and 2 WAN Interfaces
    *  WAN Links – 2 (One static IP and 1 DHCP based)
  
    LAN Interface definition

    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as LAN and Security as Trusted
    *  Choose 1/1 as the LAN interface
    *  Enter the LAN IP as 192.168.9.118
    *  Apply Done and Save

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_laninterface210.png)

    WAN Link 1 - Interface definition
    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as WAN and Security as Trusted
    *  Choose 1/2 as a WAN interface
    *  Enter the WAN IP as 192.168.1.199
    *  Apply Done and Save

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_waninterface210.png)

    WAN Link 2 - Interface definition
    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as WAN and Security as Trusted
    *  Choose 1/3 as a WAN interface
    *  Select DHCP Client so the WAN link gets auto addressed
    *  Apply Done and Save

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan2interface210.png)

    WAN Link 1 - Access Interface definition
    *  Select access type as Public Internet
    *  Select Auto Detect
    *  Enter the speed as appropriate (based on the PoC link made available)
    *  15 Mbps Upload/Download
    *  Select the WAN1 interface and provide the Gateway (Access Interface IP is auto populated)

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan2interface21.png)

    WAN Link 2 - Access Interface definition

    *  Select access type as Public Internet
    *  Select Auto Detect
    *  Enter the speed as appropriate (based on the PoC link made available)
    *  10 Mbps Upload/Download
    *  Select the WAN2 interface and the Access interface is auto populated (as it is a DHCP link)

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan2accinterface210.png)

    Deploy Config/Software to initiate Change management and activation

    *  Click on Deploy Config Software
    *  Click on Stage and allow staging to complete
    *  Click on Activate and allow activate to complete

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_deploystageactivate210.png)

## How to configure SD-WAN for IPSec Tunnel (Automated from Orchestrator)

### Create a Secure Internet Access Service

*  Go to All Sites -> Delivery Services -> Secure Internet Access Service

*  Adjust the provisioning on the Secure Internet Access service at the global level and re-adjust the overall internet link provisioning

        Note : At a global level, without the provisioning value for the SIA service, the SIA site automated IPSec provisioning fails

*  Provide some provisioning percentage on the Internet link for the Secure Internet Access service: For this demo we give 30%

        BEST PRACTICE : Without this percentage provisioning, the site provisioning fails. So, it is recommended that we provide the percentage before creating the site for SIA service

*  Click on the gear icon to add the new site for Citrix SIA service

        Note : Creating an SIA service internally creates an automatic INTRANET service with the routing domain chosen during the SIA site configuration and PRESERVE ROUTE TO INTRANET SERVICE which is the IGNORE WAN LINK status knob automatically

![Citrix SIA Service Creation Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaservicecreation.png)

### Create a Secure Internet Access Service

*  Add a site by clicking the “+Site” button
*  After you add the new site, configure the sections, like below.
    *  Select the Tunnel Type as “IPSec”
    *  You can create an IPSec tunnel with the Citrix SIA cloud nodes but for the purpose of this PoC, we use an IPSec tunnel automated configuration

        ![Citrix SIA Service Tunnel Type](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunneltype.png)

### Let AUTO be the default PoP selection and select sites to create Citrix SIA tunnel with

*  Choosing AUTO selects the closest 2 PoP’s for the IPSec tunnel (based on the geo location of the site created in the Orchestrator)
*  Maximum of 2 PoPs are selected for creating a redundant ACTIVE-STANDBY tunnel to the 2 different PoPs
  
        Note : Ensure to provide the proper location of the branch in the Orchestrator so that the closest PoP selection is done
*  Select the site that needs to be automatically provisioned from the SD-WAN site to the SIA Cloud PoP(s)
*  Click on “Review”

    ![Citrix SIA Service Tunnel site selection](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnelpopsiteselect.png)

*  Click on “Save”

    ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnelsave.png)

### Verify service provisioning

*  Once the site is added, and if the service provisioning was provided in the Delivery services section, the Site Provisioning is successful.
    *  Verify the Tunnel type is: IPSEC
    *  Region Count: 2 (If you have 2 or more PoPs available in your account. If your account has ONLY 1 PoP, then you may see only 1 here)
    *  Status: Site Provisioning Success

        ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaverifyprovisioning.png)

### Steering of traffic via Citrix SIA Tunnel (IPSec Type)

*  Click on “Default SIA Group”

    ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiasaveprovisioning.png)

*  **Routing "ALL APPS" via Citrix SIA service –** If ALL Internet traffic is to go through the Citrix SIA IPSec tunnel. A default 0.0.0.0/0 is created via Citrix SIA service in this case

    ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaallapps.png)

*  **Application/Custom Application/Group Specific –** If there are only specific applications that need to be routed through the SIA service. In this case app routes are created in the SD-WAN

    ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

### Verify what automation is done as part of IPSec tunnel provisioning in Orchestrator

Click on the Info icon to know what details have been automated for the IPSec tunnel from the SD-WAN side

    Note : 
    Once the Citrix SIA IPSec tunnel is provisioned and saved, the Citrix SD-WAN Orchestrator configures the Citrix SIA IPSec tunnel with tunnel endpoint addresses, IKE/IPSec authentication and encryption settings and the WAN Links on which the Citrix SIA IPSec tunnel should be enabled

*  Some of the below attributes are displayed on clicking the "i" icon on the tunnel once provisioned
    *  Local IPs: Static WAN IP address of the 2 WAN Links are listed
    *  Peer Node IP’s displayed - with which the tunnel will be formed (Auto Selected during tunnel creation based on Geo Location of the site)
    *  Local LAN subnet displayed - which is the Local VIP subnet of LAN interface of the SD-WAN
    *  Version is IKEv2
    *  Encryption – AES256
    *  MD5/Auth HASH – SHA256
    *  PFS/IKE Group – Modp 1024 (Group2)
  
      ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

### Perform the Deploy Config/Software

*  Stage and Activate the configuration to enable the IPSec tunnel establishment between the Citrix SD-WAN and the Citrix SIA cloud PoP
  
    ![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

## How to verify Citrix SD-WAN and Citrix SIA IPSec tunnel formation

### From Citrix SIA Configuration page

With the automated tunnel configuration initiation from the SD-WAN, as soon as the new SIA site is added, the API automation starts, and the Citrix SIA Cloud IPSec tunnel endpoint is created with the settings and values.
From Citrix SIA side, for establishing a tunnel, we need two things. The creation of a tunnel under Connect Device to Cloud -> Tunnels -> IPSec tunnel and then a Local Subnet creation under Network -> Local Subnets

    Note:
    The IPSec Tunnel Name, IKE/IPSec encryption and Authentication settings, IKE Version2, Local and Remote ID including the Pre-Shared Key are auto generated by the API without the need of any manual intervention

![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

The Local subnets are also automatically created, and the tunnel newly created in the SIA cloud is attributed to the local subnet appropriately.
Note:  
The policy (Security Group) enabled is “Default” and we can change the policy in the Citrix SIA platform by editing the Local Subnets and choosing a group of administrative choice

    Note:
    1. SSL Decryption is disabled by Default
    2. Default Policy is “Default”

![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

### From Orchestrator

*  Goto All Sites -> Network Config Home -> Delivery Services -> Secure Internet Access Service
*  Click on the Info icon
    *  You can verify the Tunnel state with local and remote endpoint IPs
    *  Status of the Tunnel with statistics of packets inbound and outbound

![Citrix SIA Service Tunnel save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaspecificapps.png)

You can also verify the status of the IPSec tunnel by going to :

 Specific site -> Reports -> Reatime -> IPSec Tunnel -> Retrieve Latest Data

 ![Citrix SIA Service IPSec tunnel stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnstats.png)

### To verify what Cloud IP’s are hosted by your account in the Citrix SIA Portal (Which is the Proxy IP for hosts using CISA agent or tunnel)

In Citrix SIA Portal, Go to Home -> Node Collection Management -> Node Groups -> Node-Cluster with Gateway Type

 ![Citrix SIA View Cloud Nodes](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaviewcloudnodes.png)

### Verify that HOST traffic is proxied to Citrix SIA via IPsec Tunnel

After the tunnel is up and running, you can verify by either using <https://ipchicken.com> or <https://whatsmyip.com> to verify the Internet visible IP from the end device

If you are either using a Citrix SIA Cloud Connector (SIA Agent) or through an IPSec tunnel (without agent), the proxy IP shows up as ONE of the Cloud Nodes provided by the Citrix SIA Platform

    Note:   Here in this case it is 104.225.181.63 

![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 1: Pre-Requisites

### Download and Install Cloud connector (SIA Agent) with a specific Security Group (Windows Connector)

    Note: 
    You cannot create a new Security Group but only modify the default ones and change the names to create and apply the security policies to a specific group

    For instance, in this case we are modifying Group 7 (Not taken and still default) and making changes to the Group name “PoC_Demo_Group”

![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  **Connector configuration pre-requisite for Proxy**

    *  Go to Proxy & Caching -> Proxy Settings -> Settings
    *  Click Enable Proxy Settings as “YES”
    *  Ensure the User Authentication Method is “Local User credentials + Cloud Connectors”
    *  Ensure the Connector registration method is “Standard Registration + SAML”
    *  Leave others as default

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  **Click on Connect Device to Cloud -> Cloud Connectors**
  
    *  Click on Configure Connector Download

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Select the below values.
  
        *  Use HTTP PAC – YES
        *  Security Group – PoC_Demo_Group (created just above)
        *  Register Over SSL – Default
        *  Captive Portal – Yes
        *  Gateway Admin - Enabled

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  **Click on Windows Cloud Connector Download Button and choose Windows 8/10 64 bit**

        Note :
        Please ensure you perform the access of Citrix SIA Cloud platform via Google Chrome browser. With Firefox, the installer may not download as a .msi (in which case you need to manually change the name of the file removing .html from the file)

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Let the installer download and then double click the installer in the download pane or from the downloads folder where it got stored

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  After double click, a PoP up is presented. Click on “More Info” and then click on “Run Anyway”
        *  Clicking on Run Anyway starts installing the msi file
        *  Finish the MSI file installation allowing all further administrative operations

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Once the msi installation is complete, search for “services” in windows and then search for a service by name IBSA (This is the Citrix SIA Agent service running on the host machine now)

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Verify that with the Cloud Agent Service running, the proxy IP accessing www.ipchicken.com  or www.whatsmyip.com is that of cloud gateway nodes of the Citrix SIA account

            Note: 
            At this point, with the Cloud agent successfully registered, the IP must change to one of the Cloud Node PoP IPs

            If the IP you have got is that of the service provider NAT IP, you may debug why the SIA agent/Cloud Connector failed.

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  If the Cloud registration is successful from the Cloud Connector (SIA Agent), notice the username and all other details with a successful agent registration

            Note:
            If you are NOT seeing your username and other details, the User agent registration must have some issues

        *  Goto Users Groups and Devices -> Cloud Connected Devices

            ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  With successful Agent registration, notice the agent traffic on the Citrix SIA Reporter under Realtime Dashboard or Events logs

           Note: 
            If you have an Agent installed, notice the username to explicitly show up with the Group that was set during the agent download/install including the Private Source IP

        ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 1: Users with Cloud Connector (SIA Agent) + Citrix SD-WAN in a branch

In this use-case, we use the Citrix SIA Cloud Connector (SIA Agent) to breakout and proxy to the Citrix SIA Cloud directly using Internet service so that the DC workloads can be accessed reliably via Overlay Virtual Path of Citrix SD-WAN

This alleviates the need to backhaul sensitive applications that need high end firewall on the DC side, inducing latency considerably and deteriorating application and user experience.
With Citrix SIA, the backhaul is no longer necessary and the agent helps proxy the connections to Citrix SIA Cloud to exercise the enterprise security posture creating a secure service edge infrastructure at the Branch/Edge.

    Recommended Best Practice:
    It is a recommended best practice that the Cloud connector traffic from corporate managed devices be bypassed from the local IPSec tunnel of the Citrix SD-WAN in the branch edge. This allows direct proxy to the Citrix SIA where enterprise OU’s or security groups can be exercised directly via cloud connectors on the managed devices.

    To allow for the Citrix SIA cloud connector to seamlessly call home to the Citrix SIA gateway cluster and PoP’s, it is critical to bypass the cloud connector based traffic from the IPSec tunnel so that the registration of the cloud connector happens and the proxy is made available directly from the connector.

*  Create a new Custom APP with the below IP/PORT/Protocol Lists

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Associate the custom App to INTERNET Breakout so that the SIA/Cloud Connector agent traffic can be bypassed from the tunnel and sent directly via Internet Service

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security (Category) - Configuration of Web Security Category Policies from Citrix SIA portal

*  Click on : Web Security -> Web Security Policies -> Web/SSL Categories
*  Choose the Group as "PoC_Demo_Group" (Since Cloud Connector/SIA Agent is installed for that group)

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Enable Blocking of Friendship and Gambling Categories

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  SAVE the settings to PoC_Demo_Group ONLY

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of Web Security Category Policy enforcement and Reporting to check traffic status

*  Open an Incognito window of a browser
*  Access 777.com (A gambling site)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

As soon as the site is accessed, the splash page comes up blocking the traffic (as enforced by the administrator on the PoC_Demo_Group policy

You can also see that the Group name is visible including the end host private local IP and the username

*  The description also states the category accessed

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporting event logs indicate the block of access to the Gambling site

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Open an Incognito window of a browser
*  Access facebook.com (A Friendship Category site)

As soon as the site is accessed, the splash page comes up blocking the traffic (as enforced by the administrator on the PoC_Demo_Group policy

You can also see that the Group name is visible including the end host private local IP and the username

*  The description also states the category accessed

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporter also shows that Facebook from Friendship category is BLOCKED

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### ALLOW LIST: Configuration of Allow List (Web Security) Policies from Citrix SIA portal

Create a simple ALLOW List to allow ONLY Facebook from the entire Friendship category that is currently BLOCKED

*  Go to Web Security -> Web Security Policies -> Allow List

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Add an allow list in URL/IP Range stating facebook.com and click in “+Add”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Notice the Allow list showing the URL added to be allowed

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of Web Security Allow List Policy enforcement and Reporting to check traffic status

*  Open an incognito browser tab and access facebook.com
*  Facebook seems allowed, but you can see that it is NOT fully loaded as a page
    NOTE: This is because there are other related URLs that facebook.com is dependent upon, which also need to be in the ALLOW list for the entire page/domain to open properly

        Best Practice :
        For Allow/Block List – SCRAPE and add all domains if being allowed

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Go to the Allow List under Web Security
    *  Web Security -> Web Security Policies -> Allow List
    *  Click on Scrape
    *  Type facebook.com and click on Scan
    *  Add all domains to the ALLOW List

       ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Now open facebook.com in an incognito browser and it opens fully

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Notice from the reporter event logs the Facebook.com is NOW allowed and no more blocked as it was added to the allow list
*  Allow list takes more precedence than the Category blocking

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### CASB - Configuration of CASB Policies from Citrix SIA portal

Create a CASB rule to enable safe search on Google Browser and disable gmail.com access

*  Go to CASB -> CASB App Controls -> Cloud App Controls
    *  Select Group as “PoC_Demo_Group”
    *  Under Search Engine Controls -> Google Safe Search Enforcement
    *  Select “Enforce Safe Search for Current Group”
    *  Under Google Controls
        *  Enable Block Google Drive
    *  Click “SAVE”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of CASB Policy enforcement and Reporting to check traffic status

Google Drive Blocking control:

*  Access drive.google.com in an incognito browser tab
*  Soon as we hit enter, a splash page pops up indicating the access to google drive is blocked. This is due to the CASB control exercised in the PoC_Demo_Group Security Group

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporter event logs also indicate access to google drive blocked
*  Category states “Google Drive Blocked” as it comes from CASB controls

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

Google Browser Safe Search enablement:

*  Access google.com in an incognito browser tab
*  Soon as we hit enter, google.com opens up with a search bar
*  Enter Citrix or any keyword and hit enter
*  Notice that the search results are filtered, as the safe search is ON and is indicated as soon as the keyword is entered in the engine

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### MALWARE Defense - Configuration of Anti Malware Policies from Citrix SIA portal

    Note: SSL Decryption needs to be enabled (If Cloud connector is used and configured to auto install the root certificate as part of agent installation, this feature is enabled by default)

*  Goto Web Security -> Malware Defense -> Cloud Malware Protection
*  The Content engine needs to be ready
*  Click Block on Scan error to be Enabled

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of Anti Malware Policy enforcement and Reporting to check traffic status

*  Access <https://www.eicar.org/?page_id=3950>
*  Scroll down and click download the 68-byte file

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Once the file download is initiated, a spalsh screen pops up indicating Virus detected and page is blocked

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

Verifying via CSIA Reporting Section

*  Reporter also indicates the Malware protection enforced

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### DLP (Data Loss Prevention)- Configuration of DLP Policies from Citrix SIA portal  

*  Ensure that that DLP Content and Analysis engine is ready and config is done appropriately
    *  Click “Yes” for content analysis and data loss prevention
    *  Ensure that content analysis engine is ready
    *  For the PoC, allow default selection for Content Engines and Analysis engines

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Create a DLP rule so that Data Loss Prevention can be enforced on unauthorized content upload/download/both
    *  Go to Data Loss Prevention tab
    *  Click on DLP Rules
    *  Click on “+Add Rule”
    *  Under General Information Tab:
    *  Ensure Rule Enabled is “YES”
    *  Provide a name for the rule
    *  Enable HTTP Methods of PUT a POST so we can enforce DLP on uploads
    *  Leave content engines default

    ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Under DLP tabs from Network Sources to Content Types (change only policy as below):
    *  Choose policy as “Include All except selected items”
    *  Allow Defaults in Search Criteria tab
    *  Select Action as “BLOCK”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of DLP (Data Loss Prevention) Policy enforcement and Reporting to check traffic status

*  Goto <https://dlptest.com> (a site to verify Data Loss Prevention tests)
*  Click on Sample Data and view some sample information
*  Click on XLS File (This downloads the file)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  See the file gets downloaded with the name “sample-data.xls” under Downloads folder (or folder you have selected)
*  Use this file to test DLP in a new site next

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Now open a new browser incognito tab and type <http://dataleaktest.com>
    *  Click on Upload Test
    *  In the Upload test page, scroll down and click on “SSL ON”
    *  After clickin on SSL ON, you can see the message on the black screen “System Ready for Next DLP Test with SSL ON”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Now click on Browse
    *  Select the file we previously downloaded in step “d” with filename sample-data.xls and UPLOAD

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Once the dataleaktest to test HTTPS upload of SSN contained file, a spalsh screen pops up indicating Unauthorized content detected and page is blocked (because DLP kicked in)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Verify Reporter Logs for DLP events:
    *  Click on More Filters

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Choose log Type as DLP and Hit “Search”
    *  Notice that the DLP upload gets blocked

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 2:  Pre-Requisite - Uninstall the Cloud Connector

*  The cloud connector agent that was downloaded and installed, the same agent should be right-clicked.
    *  Select uninstall

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Select YES for “Are you sure you want to uninstall this product”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Follow the uninstallation process and administratively say “Yes” for uninstalling if asked
*  Go to Services in windows and check if the IBSA service is uninstalled properly
*  IBSA agent service should not exist

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Verify the proxy IP
    *  The Proxy IP should still be one of the cloud gateway node IP’s. This is because,  the test laptop we are using for PoC is behind the Citrix SD-WAN whose IPSec tunnel is still up and running with the Citrix SIA cloud service
  
    *  Once the agent is uninstalled all the traffic goes through the IPSec tunnel

## PoC use-case 2: Users or devices without Cloud Connector (SIA Agent) in a non-guest network + Citrix SD-WAN in a branch via IPSec Tunnel (Basic NON-SSL Decryption based security)

In this use-case, we use the Citrix SIA IPSec tunnel to breakout and proxy to the Citrix SIA Cloud directly using Citrix SIA service. Citrix SIA IPSec tunnel allows for unmanaged devices like BYOD/Personal and Guest devices to be managed and enforced of enterprise security policies when they connect in a branch. The devices with the agent bypass the tunnel and get enforced of policies.

### Recommended Best Practice for IPSec Tunnel

      It is a recommended best practice that the in case where there are no Cloud Connector agents in devices, a branch managed by Citrix SD-WAN uses the IPSec tunnel to manage the security policies for those devices securely with the Citrix SIA platform.

    Note:
    The tunnels that get configured generally come disabled with SSL decryption, unless explicitly set. Also, with just the plain IPSec tunnel, we get features that do not involve SSL decryption but based on URL’s (full or regex-based URL’s) like below:

*  Web Security (Category based)
*  Allow List
*  Block List

SSL decryption via tunnel is covered in the next PoC use case for exercising full security via IPSec tunnel

### Web Security (Category Block) – Configure Web Security rules for Category in the Citrix SIA platform like Gambling and Friendship (Social Media)

Before verification of policy enforcement via IPSec tunnel, create the security policies in the Citrix SIA cloud portal

*  Go to Web Security -> Web Security Policies -> Web/SSL Categories
*  Click on Default Security Group as the IPSec tunnel is created in general with Default Security Group

        Note:
        If you want to change the group, you must manually do it in Local Subnets for the entry that has the IPSec tunnel

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Enable CATEGORY blocking via IPSec tunnel for Gambling and Friendship categories

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security (Allow List) – Configure allow list for specific URL among BLOCKED category

*  Add an allow list to Friendship category to allow Linkedin ONLY but block all other social media categories

        Note:
        As defined best practice, scrape and resolve other URL dependencies before adding to an ALLOW/BLOCK list to completely exercise the functionality of URL web filtering

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security (Block List) – Configure Block list for specific URL

*  Configure a specific BLOCK list via URL web filtering for a site like notpurple.com

    *  Go to Web Security -> Web Security Policies -> Block List
    *  Add a web URL “notpurple.com” to the BLOCK List

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security Category Block Verification of traffic and policy enforcement via the IPSec Tunnel

*  Open an incognito browser tab and access Gambling site 777.com from the Gambling category we blocked and verify that it is blocked over the IPSec tunnel
*  A blocked splash page is presented due to the block list policy enforcement
*  This time we are seeing this happen using the Default Group name which is associated with the tunnel

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  From the reporter we can see in even logs that the access to 777.com is categorically Blocked due to GAMBLING section
    *  Click on Reporting and Analytics -> Logs -> Event Logs
    *  Click on Search
    *  If needed, click on More Filters, and select Action -> Blocked to check ALL logs with BLOCKED action

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Open an incognito browser tab and access site facebook.com from the Friendship category we blocked and verify that it is blocked over the IPSec tunnel
*  Notice that the site does not open (Facebook is inaccessible as it is blocked)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  From the reporter we can see in even logs that the access to facebook.com is categorically Blocked due to FRIENDSHIP section

    *  Click on Reporting and Analytics -> Logs -> Event Logs
    *  Click on Search
    *  If needed, click on More Filters, and select Action -> Blocked to check ALL logs with BLOCKED action

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security Allow List Verification of traffic and policy enforcement via the IPSec Tunnel

*  We had initially configured LinkedIn to be exempt from the Friendship category to be blocked via selective ALLOW LIST configuration.
*  Access linkedin.com from the Friendship category in an incognito browser tab
*  We can see that access is allowed and the page opens up

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporter also shows the traffic to be allowed and NOT blocked due to the ALLOW list configuration taking precedence to category blocking

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Web Security Block List Verification of traffic and policy enforcement via the IPSec Tunnel

*  Finally, access a specific website notpurple.com which is in the Blocklist and see that when accessed over the tunnel gets blocked
*  Open an incognito browser tab and access notpurple.com
*  We get an immediate splash page blocking access to the site

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporter also shows the traffic to be BLOCKED as it is explicitly configured as part of the BLOCK LIST

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 3: Pre-requisites for enabling SSL decryption on the IPSec tunnel

The tunnels that get configured generally come disabled with SSL decryption, unless explicitly set.  Once IPSec tunnel is enabled for SSL decryption, it is capable of performing ALL Advanced security features like Malware Defense, DLP, CASB etc.

However, there are a few pre-requisites, without which Advanced security CANNOT be enabled on the IPSec tunnel with the Citrix SIA cloud

    Important 3 pre-requisites:

    1. Enable Transparent SSL Proxy under Proxy/Caching -> SSL Decryption -> General Settings
    2. Enable SSL decryption on the IPSec tunnel in the Local Subnets
    3. Download and Install the ROOT Certificate from the Citrix SIA Cloud and install on the end user device under trusted root authorities (To enable SSL decryption by the platform)

### Enable Transparent SSL Decryption under Proxy/Caching -> SSL Decryption

For enabling IPSec tunnel to allow for SSL decryption, the transparent SSL decryption must be enabled.

    Note: 
    SSL decryption can be applied for ALL IPSec tunnels depending on how we administer it. We can either choose SSL decryption for ALL subnets or manually enable SSL decryption per Local Subnet of an IPSec tunnel.

For the purpose of this demo PoC, we enable an explicit tunnel with SSL decryption and choose from General settings the value “Local subnets with SSL decryption enabled”

*  Enable Proxy SSL Decryption – YES
*  Perform SSL Decryption on – All Destinations
*  Enable Transparent SSL Decryption – YES
*  Perform Transparent SSL Decryption on – Local Subnets with SSL Decryption Enabled
*  Leave others to default

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Enable SSL decryption on the IPSec tunnel in the Local Subnets

    Note: 
    Selection of SSL decryption in an IPSec tunnel is already raised as an enhancement which will be available while configuring the Secure Internet Access service. 

    The workaround is to enable the decryption on the tunnel after it is configured in the SIA portal (In Orchestrator)

*  Click on Network -> Local Subnets -> Quick Edit Local Subnets (As displayed in the snapshot below)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  All IPSec tunnels created need local subnet that defines the local LAN subnet of the end hosts that will be protected with IPSec.
*  Choose the tunnel depending on the name and the local subnet (would be an auto created tunnel via Orchestrator with IP Address representing the local LAN subnet of the device needed to be protected with IPSec tunnel)
*  Click the checkbox SSL to enable SSL decryption

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  While in Quick edit window, double click on Default policy
*  Default policy indicates the security group that is applied to ALL the traffic falling under the Local Subnet (with LAN network defined)
*  Select PoC_Demo_Group
*  Also, double click on Login Group and select PoC_Demo_Group

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Download and Install the ROOT Certificate from the Citrix SIA Cloud and install on the end user device under trusted root authorities

This is the most important part for enabling the browser to trust the root certificate of Citrix SIA cloud to perform the SSL decryption.
All the devices that are not managed via the connector, if need to be administered with advanced security policies like CASB, Malware defense, DLP etc will need the installation of the root certificate from the Citrix SIA cloud portal.
Note that sometimes there might also be use cases where the CSIA agents (Cloud Connectors) might be overridden to all go through the tunnel for a singular policy enforcement on a branch edge for an enterprise.

    Note:
    Installation of the ROOT certificate is a MANUAL process or needs to be performed via GPO (Group policy object) or MDM or the enterprise presenting a splash page determining how to facilitate install of the root certificate on the end devices

*  Go to Proxy & Caching -> SSL Decryption -> Actions -> Generate and Download New MITM Root Certificate

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  A cert is downloaded. Click on the downloaded New MITM Root Certificate

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Click on “Open” upon the double click of the certificate

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  This opens a certificate install page. Verify that Issued to and by is from “Network Security” (Citrix SIA generated Certificate) and validity is appropriate (should be a valid cert).

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  This takes us to the wizard. Under Store location, select “Current User” and click on “Next”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Click on “Place all certificates in the following store” and click on “Browse”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Click on “Trusted Root Certification Authorities” and click on “OK”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Notice after Step 6; the certificate store gets updated as below with “Trusted Root Certification Authorities”
Click on “Next”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Click on “Finish”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Notice that a window pops up indicating that the import was successful. Click on OK for the popped-up window and then click on “OK” in the certificate install window

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  After certificate install, we now need to confirm if the certificate called “Network Security” is placed as part of Trusted Root Certification Authorities in the certificate manager

        In Windows : Search for certmgr in the search box and you can open the certificate manager which is the pane as below
        1. Navigate and Click on Trusted Root Certification Authorities
        2. Click on Certificates
        3. Verify a certificate by name “Network Security” (Highlighted)

If the certificate is found, then the installation is successful and the pre-requisites are complete to test SSL decryption via IPSec tunnel

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 3 : Users or devices without Cloud Connector (SIA Agent) in a non-guest network + Citrix SD-WAN in a branch via IPSec Tunnel (Full security via IPSec tunnel including SSL Decryption)

In this use-case, we will use the Citrix SIA IPSec tunnel along with SSL decryption to breakout and proxy to the Citrix SIA Cloud directly using Citrix SIA service and exercise Advanced security features from the Citrix SIA cloud like CASB/Malware Defense/DLP etc.

Citrix SIA IPSec tunnel allows for unmanaged devices like BYOD/Personal and Guest devices to be managed and enforced of enterprise security policies when they connect in a branch. The devices with the agent bypass the tunnel and get enforced of policies.

    Recommended Best Practice for IPSec Tunnel
    It is a recommended best practice that the in case where there are no Cloud Connector agents in devices, a branch managed by Citrix SD-WAN uses the IPSec tunnel to manage the security policies for those devices securely with the Citrix SIA platform.

### CASB – Configuration of CASB policies from Citrix SIA Portal (Orchestrator)

We create a CASB rule to enable safesearch on Google Browser and also disable gmail.com access

*  Go to CASB -> CASB App Controls -> Cloud App Controls
    *  Select Group as “PoC_Demo_Group”
    *  Under Search Engine Controls -> Google Safe Search Enforcement
*  Select “Enforce Safe Search for Current Group”
*  Under Google Controls
    *  Enable Block Google Drive
*  Click “SAVE”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of CASB Policy enforcement and Reporting to check traffic status

Google Browser Safe Search enablement

*  Access google.com in an ingocnito browser tab
*  Soon as we hit enter, we get google.com with a search bar
*  Enter Citrix or any keyword and hit enter
*  We see that the search results are filtered as the safe search is ON and is indicated as soon as the keyword is entered in the engine

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

Google Drive Blocking control

*  Access drive.google.com in an ingocnito browser tab
*  Soon as we hit enter, we get a splash page indicating the access to google drive is blocked due to the CASB control exercised in the PoC_Demo_Group Security Group

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  The reporter event logs also indicate access to google drive blocked
*  Category states “Google Drive Blocked” as it comes from CASB controls

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### MALWARE Defense - Configuration of Anti Malware Policies from Citrix SIA portal

MALWARE Protection Configuration :  

    Note : 
    SSL Decryption needs to be enabled (If Cloud connector is used and configured to auto install the root certificate as part of agent installation and this feature is be enabled by default)

*  Goto Web Security -> Malware Defense -> Cloud Malware Protection
*  The Content engine needs to be ready
*  Click Block on Scan error to be Enabled

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of Anti Malware Policy enforcement and Reporting to check traffic status

*  Access <https://www.eicar.org/?page_id=3950>
*  Scroll down and click download the 68 byte file

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

Verification

*  Once the file download is initiated, a spalsh screen pops up indicating Virus detected and page is blocked

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Reporter also indicates the Malware protection enforced

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### DLP (Data Loss Prevention)- Configuration of DLP Policies from Citrix SIA portal

Data Loss Prevention Configuration

*  Click on Data Loss Prevention Tab -> DLP Settings
*  Click “Yes” for content analysis and data loss prevention
*  Ensure that that DLP Content and Analysis engine is ready and config is done appropriately
*  For the PoC, allow default selection for Content Engines and Analysis engines

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Create a DLP rule so that Data Loss Prevention can be enforced on unauthorized content upload/download/both
    *  Go to Data Loss Prevention tab
    *  Click on  DLP Rules
    *  Click on “+Add Rule”
    *  Under General Information Tab :
    *  Ensure Rule Enabled is “YES”
    *  Provide a name for the rule
    *  Enable HTTP Methods of PUT an POST so we can enforce DLP on uploads
    *  Leave content engines default

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  Under DLP tabs from Network Sources till Content Types (change only policy as below) :
        *  Choose policy as “Include All except selected items”
    *  Allow Defaults in Search Criteria tab
    *  Select Action as “BLOCK”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

### Verification of DLP (Data Loss Prevention) Policy enforcement and Reporting to check traffic status

*  Goto <https://dlptest.com> (a site to verify Data Loss Prevention tests)
*  Click on Sample Data and view some sample information
*  Click on XLS File (This downloads the file)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  See the file gets downloaded with the name “sample-data.xls” under Downloads folder (or folder you have selected)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Now open a new browser incognito tab and type <http://dataleaktest.com>
    *  Click on Upload Test
    *  In the Upload test page, scroll down and click on “SSL ON”
    *  After clickin on SSL ON, you can see the message on the black screen “System Ready for Next DLP Test with SSL ON”

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Now click on Browse
    *  Select the file we previously downloaded with filename sample-data.xls and UPLOAD

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Once the dataleaktest to test HTTPS upload of SSN contained file, a spalsh screen pops up indicating Unauthorized content detected and page is blocked (because DLP kicked in)

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

Verify Reporter Logs for DLP events:

*  Click on More Filters

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

*  Choose log Type as DLP and Hit “Search”
*  The DLP upload gets blocked and a splash page is presented to the user

   ![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)
