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

In this Proof-of-Concept guide, you experience the role of a Citrix administrator who creates a connection between the organization’s edge SD-WAN device to the Citrix SIA Cloud via IPsec tunnels.

This guide showcases how to perform the following actions:

*  Configuration of a new site on the Orchestrator for the Proof of concept
*  Deployment of the site via Seamless automation of IPsec tunnels to Citrix SIA cloud from the Orchestrator
*  Verification of the IPsec tunnel in both the Orchestrator and Citrix SIA Cloud platform
*  Installation of SIA software agents (also known as Cloud Connector Agent) on the laptop with a specific Security Group and exercise security policies for Internet traffic
*  Demonstration of various SIA+SD-WAN integration use-cases made available in this PoC guide.
*  Configure Web security policies, CASB, Malware Protection policies to allow/deny access to certain websites or categories via the Citrix SIA Console.
*  Initiate web traffic and verify the block and allow functionality
*  Show Reporting and Analytics

## Benefits

### Networking and Deployment Use-cases/Benefits

*  **Seamless SIA for managed devices and reliable, secure DC workloads access via SD-WAN Virtual Path** On managed devices behind SD-WAN, it is easy to use the Citrix SIA agent to have secure internet access. The SD-WAN overlay forwards the branch to data center workload traffic securely and reliably.

*  **SD-WAN’s multi-wan link reliable IPsec tunnel for local subnets based Secure Internet access of agentless devices** BYOD or personal laptops that are not managed by the enterprise can be secured via Citrix SD-WAN + Citrix SIA's highly reliable IPsec tunnel. The reliability is achieved via the tunnel having multiple wan links.

*  **Simple security posture for Guest domains in a branch via DNS redirection or IPsec tunnel** With a separate tunnel/Local subnet for guest domains and related security group mapping

### Benefits of Integration/Automation/Management between Citrix SIA and SD-WAN

![Benefits 1 - Automated & Resilient Connectivity on Day 0](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_benefit1.png)
![Benefits 2 - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_benefit2.png)

## Citrix SD-WAN + SIA Overall Topology

![Topology - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_integrationtopology.png)

## Citrix SD-WAN + SIA Integration Use cases

![Use Cases Illustration - Simplified Operations for Day N](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_integrationusecases.png)

## PoC Prerequisites

### General Pre-Requisites

*  Citrix SD-WAN 110/210 for an On-Premises hardware based PoC.
    *  Can be any appliance you choose for the PoC. All Citrix SD-WAN appliances support the Citrix SIA offering.
    *  **Note:** PoC can also be performed on an Azure VPX based SD-WAN with a windows VM behind the VM on the LAN with/without a Citrix SIA agent (also known as Citrix SIA Cloud Connector)
*  A Windows or MAC Laptop
*  Agent Windows/MAC MSI download from Citrix SIA cloud platform

### Network Requirements

*  Port / Firewall settings to SIA – Outbound connections

![Network Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_networkprereqs.png)

### Orchestrator - Custom Application to bypass SIA agent traffic from IPsec tunnel in the Citrix SD-WAN

    Recommended Best Practice:

    It is a recommended best practice that the SIA agent traffic from corporate managed devices be bypassed from the local IPsec tunnel 
    of the SD-WAN in the branch edge. 
    This allows direct proxy to the Citrix SIA where enterprise OU’s or security groups can be exercised directly
    via cloud connectors on the managed devices.

To allow for the Citrix SIA agent to seamlessly Call Home to the Citrix SIA gateway cluster and PoP’s, it is critical to bypass the cloud connector based traffic from the IPsec tunnel so that the registration of the cloud connector happens and the proxy is made available directly from the connector.

    Note : 
    It is NOT desired to have the SIA agent traffic go through the IPsec tunnel.

**Points to consider in creation of the SD-WAN Bypass custom application:**

The custom application is based on the port list mentioned in the preceding section Ports List of PoC Prerequisites.
All the specific IP/Protocols in the Orchestrator bypass custom application, is sent via the Internet Service.

**Few things to get handy from the Citrix SIA platform are:**

*  Citrix SIA CloudGateway node IP’s part of the gateway cluster
    *  The Citrix SIA agent connects to the gateway nodes over port 443. Our configuration of bypass custom application ensures that the agent traffic is sent via the Internet service and bypassed from the IPsec tunnel

*  Citrix SIA Reporter node IP is part of the node group management. The reporter is contacted for sending stats/reporting from the cloud connector (SIA Agent)

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_ccbypassorchestrator.png)

**Node in Orange –** Reporter Node with IP – 104.225.171.47 and represented with a different icon (before node name).

**Node(s) in Green –** Cloud PoP nodes part of a CSIA gateway cluster with IPs 104.225.164.53 and 104.225.181.63 with the globe icon. A CSIA gateway cluster is an aggregate of two or more CSIA gateway nodes.

    Note :
    1. When doing your PoC, check the nodes as per your account and apply the Orchestrator bypass rules accordingly.
    2. The accounts almost usually always have a reporter node but the gateway ndoes can be one or more. 
    3. Ensure bypass to all gateway nodes so that they are accounted during the bypass (traffic can go to any CloudGateway node).

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_orchestratorbypasspolicy.png)

### Orchestrator – Internet service cost to be made higher than SIA service cost (45) IF ALL Internet traffic is to make through the IPsec tunnel

While creating the Citrix Secure Internet Access service, an administrator can use the default SIA group to select the traffic to be steered through the IPsec tunnel.

The administrator can either route “ALL APPS” or specific applications through the IPsec tunnel as part of the Citrix SIA service.

*  **Route ALL APPS via Citrix SIA Service in Orchestrator under SIA service Default SIA Group**

![Route ALL Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allappsrouteviasiaservice.png)

*  **Route Specific APPS via Citrix SIA Service in Orchestrator under SIA service Default SIA Group**

![Route Specific Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_specificappsrouteviasiaservice.png)

    Note
    If specific apps are chosen, then only specific application routes are created with the Citrix SIA as the service type to be 
    steered through the IPsec Tunnel. 
    The Citrix SIA service is INTRANET type service by default

**Important Recommendation when using Citrix SIA service for ALL APPS routing along with Internet Service**
If an administrator chooses “ALL APPS” to be routed via Citrix SIA, a DEFAULT route 0.0.0.0/0 created with Citrix SIA service with a cost of 45 by default

An Internet service exists or is created on the SD-WAN for a specific use case. For instance - Internet service to breakout CSIA agent related traffic, Internet service to breakout Orchestrator traffic or a brownfield deployment where an internet service exists already for some scenarios. Note this, when creating the SIA service with the Internet service (for proper routing)

**General Recommendation :**

**Recommendation 1 :** If the CSIA service is used for ALL apps alongside the Internet service, then create specific custom applications or use the DPI engine with specific applications to be steered via the Internet service.

**Recommendation 2 :** If the CSIA service is to be used for some applications and Internet service as the default service for Internet, ensure that specific applications are chosen for the Citrix SIA service breakout.

**Issues you may face if "ALL APPS" is chosen for routing via both CSIA and Internet service and how to overcome that:**

Since by design, the Internet Service cost is 5 and the CSIA service cost is 45, ALL traffic prefers to be routed over Internet service instead of CISA service. So it is good to be aware of routing when Internet and CSIA services co-exist in the deployment.

 Use one of the preceding 2 recommendations. Or you can modify the internet service cost as “50” so that CSIA service is more preferred (with a cost of 45)

![Route Specific Apps via Citrix SIA from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_inetallappschangecost.png)

## Configuration of a PoC 110/210 for Citrix SIA + Citrix SD-WAN Demo

**Assumption :** It is assumed that the Orchestrator is already configured with an MCN.

### Orchestrator 110/210 site Configuration for demonstration of IPsec from the branch edge in PoC

1.  Create a site in the Orchestrator and provide:
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

    For this PoC guide demo:

    *  Deployment mode type: Gateway Mode
    *  Interfaces – 1 LAN and 2 WAN Interfaces
    *  WAN Links – 2 (One static IP and 1 DHCP based)
  
    LAN Interface definition

    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as LAN and Security as Trusted
    *  Choose 1/1 as the LAN interface
    *  Enter the LAN IP as 192.168.9.118
    *  Apply Done and Save

    ![210 LAN Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_laninterface210.png)

    WAN Link 1 - Interface definition
    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as WAN and Security as Trusted
    *  Choose 1/2 as a WAN interface
    *  Enter the WAN IP as 192.168.1.199
    *  Apply Done and Save

    ![210 WAN Link 1 Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_waninterface210.png)

    WAN Link 2 - Interface definition
    *  Select Deployment mode as Edge (Gateway)
    *  Choose Interface type as WAN and Security as Trusted
    *  Choose 1/3 as a WAN interface
    *  Select DHCP Client so the WAN link gets auto addressed
    *  Apply Done and Save

    ![210 WAN Link 2 Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan2interface210.png)

    WAN Link 1 - Access Interface definition
    *  Select access type as Public Internet
    *  Select Auto Detect
    *  Enter the speed as appropriate (based on the PoC link made available)
    *  15 Mbps Upload/Download
    *  Select the WAN1 interface and provide the Gateway (Access Interface IP is auto populated)

    ![210 WAN Link 1 Access Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan1accessint.png)

    WAN Link 2 - Access Interface definition

    *  Select access type as Public Internet
    *  Select Auto Detect
    *  Enter the speed as appropriate (based on the PoC link made available)
    *  10 Mbps Upload/Download
    *  Select the WAN2 interface and the Access interface is auto populated (as it is a DHCP link)

    ![210 Wan Link 2 Access Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_wan2accessint.png)

    Deploy Config/Software to initiate Change management and activation

    *  Click Deploy Config Software
    *  Click Stage and allow staging to complete
    *  Click Activate and allow activate to complete

    ![210 new site Interface creation details from Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_deploystageactivate210.png)

## How to configure SD-WAN for IPsec Tunnel (Automated from Orchestrator)

### Create a Secure Internet Access Service

*  Go to All Sites -> Delivery Services -> Secure Internet Access Service

*  Adjust the provisioning on the Secure Internet Access service at the global level and readjust the overall internet link provisioning

        Note : At a global level, without the provisioning value for the SIA service, the SIA site automated IPsec provisioning fails

*  Provide some provisioning percentage on the Internet link for the Secure Internet Access service: For this demo we give 30%

        BEST PRACTICE : Without this percentage provisioning, the site provisioning fails. So, it is recommended that we provide the 
        percentage before creating the site for SIA service

*  Click the gear icon to add the new site for Citrix SIA service

        Note : Creating an SIA service internally creates an automatic INTRANET service with the routing domain chosen during the SIA 
        site configuration and PRESERVE ROUTE TO INTRANET SERVICE which is the IGNORE WAN LINK status knob automatically

![Citrix SIA Service Creation Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaservicecreation.png)

### Create a Secure Internet Access Service

*  Add a site by clicking the “+Site” button
*  After you add the new site, configure the following sections.
    *  Select the Tunnel Type as “IPsec”
    *  You can create an IPsec tunnel with the Citrix SIA cloud nodes. In the PoC, we use an IPsec tunnel automated configuration between Citrix SD-WAN and Citrix SIA

        ![Citrix SIA Service Tunnel Type](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunneltype.png)

### Let AUTO be the default PoP selection and select sites to create the Citrix SIA tunnel with

*  Choosing AUTO selects the closest 2 PoP’s for the IPsec tunnel (based on the geo location of the site created in the Orchestrator)
*  Maximum of 2 PoPs are selected for creating a redundant ACTIVE-STANDBY tunnel to the 2 different PoPs
  
        Note : Ensure to provide the proper location of the branch in the Orchestrator so that the closest PoP selection is done
*  Select the site that must be automatically provisioned from the SD-WAN site to the SIA Cloud PoP(s)
*  Click “Review”

    ![Citrix SIA Service Tunnel site selection](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnelpopsiteselect.png)

*  Click “Save”

    ![Citrix SIA Service Tunnel and Save](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnelsave.png)

### Verify service provisioning

*  Once the site is added, and if the service provisioning was provided in the Delivery services section, the Site Provisioning is successful.
    *  Verify the Tunnel type is: IPsec
    *  Region Count: 2 (If you have 2 or more PoPs available in your account. If your account has ONLY 1 PoP, then you see only 1 during the Citrix SIA configuration in Orchestrator)
    *  Status: Site Provisioning Success

        ![Citrix SIA Service Verify Provisioning](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaverifyprovisioning.png)

### Steering of traffic via Citrix SIA Tunnel (IPsec Type)

*  Click “Default SIA Group”

    ![Citrix SIA Service Routing Group](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiasaveprovisioning.png)

*  **Routing "ALL APPS" via Citrix SIA service –** If ALL Internet traffic is to go through the Citrix SIA IPsec tunnel. A default 0.0.0.0/0 is created via the Citrix SIA service in this case

    ![Citrix SIA Service Routing for All Apps](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allappsrouteviasiaservice.png)

*  **Application/Custom Application/Group Specific –** If there are only specific applications that need to be routed through the SIA service. In this case app routes are created in the SD-WAN

    ![Citrix SIA Service Routing for Specific Apps](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_specificappsrouteviasiaservice.png)

### Verify what automation is done as part of IPsec tunnel provisioning in Orchestrator

Click the Info icon to know what details have been automated for the IPsec tunnel from the SD-WAN side

    Note : 
    Once the Citrix SIA IPsec tunnel is provisioned and saved, the Citrix SD-WAN Orchestrator configures the Citrix SIA IPsec tunnel with 
    tunnel endpoint addresses, IKE/IPsec authentication and encryption settings and the WAN Links on which the Citrix SIA 
    IPsec tunnel should be enabled

*  Some of the following attributes are displayed by clicking the "i" icon on the tunnel once provisioned
    *  Local IPs: Static WAN IP address of the 2 WAN Links is listed
    *  Peer Node IPs displayed - Tunnel remote endpoints (Auto Selected during tunnel creation based on Geo Location of the site)
    *  Local LAN subnet displayed - which is the Local VIP subnet of the SD-WAN's LAN interface
    *  Version is IKEv2
    *  Encryption – AES256
    *  MD5/Auth HASH – SHA256
    *  PFS/IKE Group – Modp 1024 (Group2)
  
      ![Citrix SIA Service Tunnel automated data](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiatunnelautodata.png)

### Perform the Deploy Config/Software

*  Stage and Activate the configuration to enable the IPsec tunnel establishment between the Citrix SD-WAN and the Citrix SIA cloud PoP
  
    ![Citrix SIA Service Tunnel Change Staging](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiatunnelstageactivate.png)

   ![Citrix SIA Service Tunnel Change Activate](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiatunnelstageactivate2.png)

## How to verify Citrix SD-WAN and Citrix SIA IPsec tunnel formation

### From Citrix SIA Configuration page

Once the admin adds an SIA site in the Orchestrator , an automated tunnel configuration is initiated. The API automation creates an IPsec tunnel between the Citrix SD-WAN and the Citrix SIA CloudGateway node.
From the Citrix SIA side, for establishing a tunnel, we need two things. The creation of a tunnel under Connect Device to Cloud -> Tunnels -> IPsec tunnel and then a Local Subnet creation under Network -> Local Subnets

    Note:
    The IPsec Tunnel Name, IKE/IPsec encryption and Authentication settings, IKE Version2, Local and Remote ID including the 
    Pre-Shared Key are auto generated by the API without the need of any manual intervention

![Citrix SIA Service IPsec Tunnel Config Verify](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiatunnelconfig.png)

The Local subnets are also automatically created, and the tunnel newly created in the SIA cloud is attributed to the local subnet appropriately.
Note:  
The policy (Security Group) enabled is “Default” and we can change the policy in the Citrix SIA platform by editing the Local Subnets and choosing a group of administrative choice

    Note:
    1. SSL Decryption is disabled by Default
    2. Default Policy is “Default”

![Citrix SIA Service Tunnel Local Subnets](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiatunnellocalsubnets.png)

### From Orchestrator Page

*  Goto **All Sites -> Network Config Home -> Delivery Services -> Secure Internet Access Service**
*  Click the Info icon
    *  You can verify the Tunnel state with local and remote endpoint IPs
    *  Status of the Tunnel with statistics of packets inbound and outbound

![Citrix SIA Service Post Deploy Orchestrator Status](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_postdeploysdwotunnelstats.png)

You can also verify the status of the IPsec tunnel by going to:

*  **Specific site -> Reports -> Real Time -> IPsec Tunnel -> Retrieve Latest Data**

 ![Citrix SIA Service IPsec tunnel stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaipsectunnstats.png)

### To verify what Cloud IP’s are hosted by your account in the Citrix SIA Portal (Which is the Proxy IP for hosts using a CSIA agent or IPsec tunnel)

In Citrix SIA Portal, Go to **Home -> Node Collection Management -> Node Groups -> Node-Cluster with Gateway Type**

 ![Citrix SIA View Cloud Nodes](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaviewcloudnodes.png)

### Verify that HOST traffic is proxied to Citrix SIA via IPsec Tunnel

After the tunnel is up and running, verify the proxy IP using `http://ipchicken.com` or `http://whatsmyip.com`

If you are either using a Citrix SIA Cloud Connector (SIA Agent) or an IPsec tunnel (without agent), the proxy IP is ONE of the Cloud Nodes provided by the Citrix SIA Platform for the account used

    Note:   Here in this case it is 104.225.181.63 

![Citrix SIA Service Host Proxy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

## PoC use-case 1: Pre-Requisites

### Download and Install Cloud connector (SIA Agent) with a specific Security Group (Windows Connector)

    Note: 
    You cannot create a new Security Group but only modify the default ones and change the names to create and apply the security
     policies to a specific group

    For instance, in this case we are modifying Group 7 (Not taken and still default) and making changes to the Group name
     “PoC_Demo_Group”

![Citrix SIA Security Group Creation](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_siasecuritygroupnameforpoc.png)

*  **Connector configuration pre-requisite for Proxy**

    *  Go to Proxy & Caching -> Proxy Settings -> Settings
    *  Click Enable Proxy Settings as “YES”
    *  Ensure the User Authentication Method is “Local User credentials + Cloud Connectors”
    *  Ensure the Connector registration method is “Standard Registration + SAML”
    *  Leave others as default

        ![Citrix SIA Agent Proxy Settings](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_cisaagentproxysettings.png)

*  **Click Connect Device to Cloud -> Cloud Connectors**
  
    *  Click Configure Connector Download

        ![Citrix SIA Agent Download Settings](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaagentdownloadsettings.png)

    *  Select the following values.
  
        *  Use HTTP PAC – YES
        *  Security Group – PoC_Demo_Group
        *  Register Over SSL – Default
        *  Captive Portal – Yes
        *  Gateway Admin - Enabled

        ![Citrix SIA Agent Set Values Before Download](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiadownloadvalues.png)

*  **Click Windows Cloud Connector Download Button and choose Windows 8/10 64 bit**

        Note :
        Please ensure you perform the access of Citrix SIA Cloud platform via Google Chrome browser. With Firefox, the installer 
        may not download as a .msi (in which case you need to manually change the name of the file removing .html from the file)

    ![Citrix SIA Agent Download Installer](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_downloadcsiaagentwindows.png)

    *  Let the installer download and then double-click the installer in the download pane or from the downloads folder where it got stored

        ![Citrix SIA Agent Install CSIA Agent on Windows](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-installcsiaagent.png)

    *  After double-click, a PoP up is presented. Click “More Info” and then Click “Run Anyway”
        *  clicking on Run Anyway starts installing the msi file
        *  Finish the MSI file installation allowing all further administrative operations

        ![Citrix SIA Agent run installer](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_runcisaagentinstaller.png)

    *  Once the msi installation is complete, look for “services” in the Windows search and open the services window.
    *  Look for a service by name IBSA. This confirms that the Citrix SIA Agent service running on the host machine.

        ![Verify Citrix SIA Agent status From Windows Services](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_verifycsiaservicestatus.png)

    *  Verify that with the Cloud Agent Service running, the proxy IP accessing www.ipchicken.com or www.whatsmyip.com is that of CloudGateway nodes of the Citrix SIA account

            Note: 
            At this point, with the Cloud agent successfully registered, the IP must change to one of the Cloud Node PoP IPs

            If the IP you have got is that of the service provider NAT IP, you may debug why the SIA agent/Cloud Connector failed.

        ![Citrix SIA Service Host Proxy After CSIA Agent Install](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_hostproxyipviacsiatunnel.png)

    *  If the Cloud registration is successful from the Cloud Connector (SIA Agent), notice the user name and all other details with a successful agent registration

            Note:
            If you are NOT seeing your user name and other details, the User agent registration must have some issues

        *  Goto Users Groups and Devices -> Cloud Connected Devices

            ![Citrix SIA Agent Registration Successful](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaagentregistrationsuccess.png)

    *  With successful Agent registration, notice the agent traffic on the Citrix SIA Reporter under Real-time Dashboard or Events logs

           Note: 
            If you have an Agent installed, notice the user name to explicitly show up with the Group that was set during the agent 
            download/install including the Private Source IP

        ![Citrix SIA Agent Traffic Reporting](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaagentrafficreporting.png)

## PoC use-case 1: Users with Cloud Connector (SIA Agent) + Citrix SD-WAN in a branch

In this use-case, we use the Citrix SIA Cloud Connector (SIA Agent) to breakout and proxy to the Citrix SIA Cloud directly using the Internet service so that the DC workloads can be accessed reliably via the Overlay Virtual Path of Citrix SD-WAN

Without Citrix SIA, we would need to backhaul enterprise sensitive SaaS applications to the data center for providing security. But this induces latency considerably thereby deteriorating the application delivery and experience.

With Citrix SIA, the backhaul is no longer necessary. The Citrix SIA agent helps proxy the enterprise SaaS connections to Citrix SIA Cloud directly. The Citrix SIA then creates a secure service edge infrastructure at the Branch/Edge.

    Recommended Best Practice:
    It is a recommended best practice that the Cloud connector traffic from corporate managed devices be bypassed from the local IPsec 
    tunnel of the Citrix SD-WAN in the branch edge.
    This allows direct proxy to the Citrix SIA where enterprise OU’s or security groups can be exercised directly via cloud connectors on the managed devices.

    To allow for the Citrix SIA cloud connector to seamlessly Call Home to the Citrix SIA gateway cluster and PoP’s, it is critical to 
    bypass the cloud connector based traffic from the IPsec tunnel.

*  Create a new Custom APP with the following IP/PORT/Protocol Lists

   ![Citrix SIA Agent Bypass from IPsec tunnel App in Orchestrator](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_csiaagentbypasstraffictunnel.png)

*  Associate the custom App to INTERNET Breakout so that the SIA or Cloud Connector agent traffic can be bypassed from the tunnel and sent directly via Internet Service

### Web Security (Category) - Configuration of Web Security Category Policies from Citrix SIA portal

*  Click **Web Security -> Web Security Policies -> Web/SSL Categories**
*  Choose the Group as "PoC_Demo_Group" as the SIA Agent is installed for that group)

    ![Associate Security Group to Web Security](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_associatesecgrptowebseccisa.png)

*  Enable Blocking of Friendship and Gambling Categories

    ![Enable Web Security for Category](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_enablewebsecforcategory.png)

*  SAVE the settings to "PoC_Demo_Group" ONLY

    ![Save Web Security Settings](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_savewebsecuritysettings.png)

### Verification of Web Security Category Policy enforcement and Reporting to check traffic status

*  Open an Incognito window of a browser
*  Access 777.com (A gambling site)

   ![Access 777 website from Gambling](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_access777website.png)

As soon as the site is accessed, the splash page comes up blocking the traffic (as enforced by the administrator on the PoC_Demo_Group policy

You can also see that the Group name is visible including the end host private local IP and the user name

*  The description also states the category accessed

   ![777 Website Blocked Gambling](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_777websiteblocked.png)

*  The reporting event logs indicate the block of access to the Gambling site

   ![777 Website Blocked Reporting Dashboard](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_777trafficreporting.png)

*  Open an Incognito window of a browser
*  Access facebook.com (A Friendship Category site)

As soon as the site is accessed, the splash page comes up blocking the traffic (as enforced by the administrator on the PoC_Demo_Group policy

You can also see that the Group name is visible including the end host private local IP and the user name

*  The description also states the category accessed

   ![Facebook Website Blocked Friendship Category](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_facebookaccessblockedfriendshipcategory.png)

*  The reporter also shows that Facebook from Friendship category is blocked.

   ![Facebook Website Reporting Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_facebookblockedreportingstats.png)

### Allow list: Configuration of allow list (Web Security) Policies from Citrix SIA portal

Create a simple allow list to allow ONLY Facebook from the entire Friendship category that is currently BLOCKED

*  Go to **Web Security -> Web Security Policies -> Allow List**

   ![allow list Section](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allowlistcreation.png)

*  Add an allow list in the URL/IP Range stating facebook.com and click in “+Add”

   ![allow list Rule Add](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allowlistruleadd.png)

*  Notice the allow list showing the URL added to be allowed

   ![allow list Rule Addition Confirmation](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_ruleaddverify.png)

### Verification of Web Security allow list Policy enforcement and Reporting to check traffic status

*  Open an incognito browser tab and access facebook.com
*  Facebook seems allowed, but you can see that it is NOT fully loaded as a page
    NOTE: This is because there are other related URLs that facebook.com is dependent upon, which also need to be in the allow list for the entire page/domain to open properly

        Best Practice :
        For Allow/Block List – SCRAPE and add all domains if being allowed

   ![Facebook Block Without Scrape](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_facebookblockwithoutscrape.png)

*  Go to the allow list under Web Security
    *  **Web Security -> Web Security Policies -> allow list**
    *  Click Scrape
    *  Type facebook.com and Click Scan
    *  Add all domains to the allow list

       ![How to Scrape an Allow List](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_scrapeallowlist.png)

*  Now open facebook.com in an incognito browser and it opens fully

   ![Post Scrape Allow List works well for Facebook](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_facebookopenswellpostscrape.png)

*  Notice from the reporter event logs the Facebook.com is NOW allowed and no more blocked as it was added to the allow list
*  allow list takes more precedence than the Category blocking

   ![allow list Traffic Reporting Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allowlistreportingstats.png)

### CASB - Configuration of CASB Policies from Citrix SIA portal

Create a CASB rule to enable safe search on Google Browser and disable gmail.com access

*  Go to **CASB -> CASB App Controls -> Cloud App Controls**
    *  Select Group as “PoC_Demo_Group”
    *  Under **Search Engine Controls -> Google Safe Search Enforcement**
    *  Select “Enforce Safe Search for Current Group”
    *  Under Google Controls
        *  Enable Block Google Drive
    *  Click “SAVE”

   ![CASB App Controls](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_casbappcontrols.png)

### Verification of CASB Policy enforcement and Reporting to check traffic status

Google Drive Blocking control:

*  Access drive.google.com in an incognito browser tab
*  Soon as we access Google Drive, a splash page pops up. This is due to the Google Drive CASB control exercised in the PoC_Demo_Group Security Group

   ![CASB Google Drive Blocked](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_casbgoogledriveblockedaccess.png)

*  The reporter event logs also indicate access to Google Drive blocked
*  Category states “Google Drive Blocked” as it comes from CASB controls

   ![CASB access traffic reporting stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_reportergdriveblockedstats.png)

Google Browser Safe Search enablement:

*  Access google.com in an incognito browser tab
*  Soon as we access, google.com opens up with a search bar
*  Enter Citrix or any keyword and initiate search
*  Notice that the search results are filtered, as the safe search is ON and is indicated as soon as the keyword is entered in the engine

   ![Google Safe Search CASB Control](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_googlesafesaerch.png)

### MALWARE Defense - Configuration of Anti Malware Policies from Citrix SIA portal

    Note: SSL Decryption must be enabled (If Cloud connector is used and configured to auto install the root certificate as part of 
    agent installation, this feature is enabled by default)

*  Go to **Web Security -> Malware Defense -> Cloud Malware Protection**
*  The Content engine must be ready
*  Click Block on Scan error to be Enabled

   ![Malware Defense Settings Check](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_malwaredefensesettingscheck.png)

### Verification of Anti Malware Policy enforcement and Reporting to check traffic status

*  Access <https://www.eicar.org/?page_id=3950>
*  Scroll down and click download the 68-byte file

   ![Prepare Malware Download Eicar](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_preparemalwaredownloadeicar.png)

*  Once the file download is initiated, a splash screen pops up. The page is blocked due to a Virus or Malware detection.

   ![Malware Download Blocked Access](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_malwaredownloadblockedaccess.png)

Verifying via CSIA Reporting Section

*  Reporter also indicates the Malware protection enforced

   ![Malware Reporting Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_malwarereportingstats.png)

### DLP (Data Loss Prevention)- Configuration of DLP Policies from Citrix SIA portal  

*  Ensure that that DLP Content and Analysis engine is ready and config is done appropriately
    *  Click “Yes” for content analysis and data loss prevention
    *  Ensure that content analysis engine is ready
    *  For the PoC, allow default selection for Content Engines and Analysis engines

    ![DLP Settings Check](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpsettingscheck.png)

*  Create a DLP rule so that Data Loss Prevention can be enforced on unauthorized content upload/download/both
    *  Go to Data Loss Prevention tab
    *  Clcik DLP Rules
    *  Click “+Add Rule”
    *  Under General Information Tab:
    *  Ensure Rule Enabled is “YES”
    *  Provide a name for the rule
    *  Enable HTTP Methods of PUT a POST so we can enforce DLP on uploads
    *  Leave content engines default

    ![DLP Rule Create](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlprulecreate.png)

*  Under DLP tabs from Network Sources to Content Types
    *  Choose the policy as “Include All except selected items”
    *  Allow Defaults in the Search Criteria tab
    *  Select Action as “BLOCK”

   ![DLP Action to Block](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpactiontoblock.png)

### Verification of DLP (Data Loss Prevention) Policy enforcement and Reporting to check traffic status

*  Goto <https://dlptest.com> (a site to verify Data Loss Prevention tests)
*  Click Sample Data and view some sample information
*  Click XLS File (This downloads the file)

   ![DLP Test site download Excel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_downloaddlpsensitivefiletotest.png)

*  See the file gets downloaded with the name “sample-data.xls” under Downloads folder (or folder you have selected)
*  Use this file to test DLP in a new site next

*  Now open a new browser incognito tab and type `http://dataleaktest.com`
    *  Click Upload Test
    *  In the Upload test page, scroll down and Click “SSL ON”
    *  After clicking on SSL ON, you can see the message on the black screen “System Ready for Next DLP Test with SSL ON”

   ![Data leak site DLP test with SSL ON](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dataleaksitesslondlptest.png)

*  Now Click Browse
    *  Select the file we previously downloaded file sample-data.xls and UPLOAD

   ![Upload DLP sensitive File](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_uploaddlpsensitivefile.png)

*  On uploading the sensitive file (containing SSN), a splash screen pops up. The page is blocked due to Unauthorized content detection (DLP)

   ![DLP Sensitive Data Access Restricted](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpsensitivedatarestricted.png)

*  Verify Reporter Logs for DLP events:
    *  Click More Filters
    *  Choose log Type as DLP and click “Search”
    *  Notice that the DLP upload gets blocked

   ![DLP Reporting Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpreportingstats.png)

## PoC use-case 2:  Pre-Requisite - Uninstall the Cloud Connector

*  To start the Citrix SIA agent uninstallation process, right-click the previously downloaded Citrix SIA installer (from the folder it was downloaded)
    *  Select uninstall

   ![Initiate CSIA agent uninstall](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_uninstallinitiateagent.png)

*  Select YES for “Are you sure you want to uninstall this product”

   ![Accept Uninstallation agent](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_agentuninstallaccept.png)

*  Follow the uninstallation process and administratively say “Yes” for uninstalling if asked
*  Go to Services in windows and check the IBSA service is uninstalled properly and does not exist

   ![Verify CSIA Agent Uninstalled in Windows Services](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_verifywindowsservicesagentuninstalled.png)

*  Verify the proxy IP
    *  The Proxy IP is one of the Citrix SIA CloudGateway node IP’s. This is because, the test laptop we are using for PoC is behind the Citrix SD-WAN whose IPsec tunnel is still up and running with the Citrix SIA cloud service
  
    *  Once the agent is uninstalled all the traffic goes through the IPsec tunnel

## PoC use-case 2: Users or devices without Cloud Connector (SIA Agent) in a non-guest network + Citrix SD-WAN in a branch via IPsec Tunnel (Basic NON-SSL Decryption based security)

In this use-case, we use the Citrix SIA IPsec tunnel to secure for unmanaged devices like BYOD, Personal and Guest devices behind a branch SD-WAN. Managed devices with the Citrix SIA in the branch agent bypass the tunnel and connect directly to the Citrix SIA platform.

### Recommended Best Practice for the IPsec Tunnel

      It is a recommended best practice that the in case where there are no Cloud Connector agents in devices, a branch managed by Citrix 
      SD-WAN uses the IPsec tunnel to manage the security policies for those devices securely with the Citrix SIA platform.

    Note:
    The tunnels that get configured by default do not have SSL decryption enabled by default. With just the plain IPsec 
    tunnel, we only get the following features:

*  Web Security (Category based)
*  Allow List
*  Block List

SSL decryption via tunnel is covered in the next PoC use case for exercising full security via IPsec tunnel

### Web Security (Category Block) – Configure Web Security rules for Category in the Citrix SIA platform like Gambling and Friendship (Social Media)

Before verification of policy enforcement via the IPsec tunnel, create the security policies in the Citrix SIA cloud portal

*  Go to **Web Security -> Web Security Policies -> Web/SSL Categories**
*  Click Default Security Group as the IPsec tunnel is created in general with Default Security Group

        Note:
        If you want to change the group, you must manually do it in Local Subnets for the entry that has the IPsec tunnel

   ![Use case 2 Web Security Group](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_websecgroup2.png)

*  Enable CATEGORY blocking via IPsec tunnel for Gambling and Friendship categories

  ![Enable Web Security for Category Use case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_enablewebsecforcategory.png)

### Web Security (allow list) – Configure allow list for specific URL among BLOCKED category

*  Add an allow list to Friendship category to allow Linkedin ONLY but block all other social media categories

        Note:
        As defined best practice, scrape and resolve other URL dependencies before adding to an Allow/Block List to completely 
        exercise the functionality of URL web filtering

   ![Allow List Use Case 2 Configuration](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_allowlist2linkedin.png)

   ![Allow List Saved Config Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_postconfigallowlinkedinlist.png)

### Web Security (Block List) – Configure Block List for specific URL

*  Configure a specific block list via URL web filtering for a site like notpurple.com

    *  Go to **Web Security -> Web Security Policies -> Block List**
    *  Add a web URL “notpurple.com” to the block list

   ![Block List Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_blockedlistcase2.png)

### Web Security Category Block Verification of traffic and policy enforcement via the IPsec Tunnel

*  Open an incognito browser tab and access Gambling site 777.com from the Gambling category
*  A splash page is presented due to the block list policy enforcement

   ![Web Category 777 blocked via IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-webcategoryblockviatunnel.png)

*  From the reporter we can see in even logs that the access to 777.com is categorically Blocked due to GAMBLING section
    *  Click Reporting and Analytics -> Logs -> Event Logs
    *  Click Search
    *  If needed, **Click More Filters**, and select **Action -> Blocked** to filter all the logged events that are blocked

   ![Web Category 777 blocked via IPsec Tunnel Reporting Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_blockedreportingstatsviatunnel.png)

*  Open an incognito browser tab and access site facebook.com from the Friendship category
*  Notice that the site does not open (Facebook is inaccessible as it is blocked)

   ![Web Category Facebook Blocked Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_facebookcategoryblockedviatunnel.png)

*  From the reporter we can see in even logs that the access to facebook.com is categorically Blocked due to FRIENDSHIP section

    *  Click Reporting and Analytics -> Logs -> Event Logs
    *  Click Search
    *  If needed, Click **Click More Filters**, and select **Action -> Blocked** to filter all the logged events that are blocked

   ![Web Category Facebook Blocked Reporting Stats Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_reportingfacebookblockstats.png)

### Web Security allow list Verification of traffic and policy enforcement via the IPsec Tunnel

*  We had initially configured LinkedIn to be exempt from the Friendship category to be blocked via selective allow list configuration.
*  Access linkedin.com from the Friendship category in an incognito browser tab
*  We can see that access is allowed and the page opens up

   ![Allow List Linkedin Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_linkedinallowedbutcategoryblockedviatunnel.png)

*  The reporter also shows the traffic to be allowed and NOT blocked due to the allow list configuration taking precedence to category blocking

   ![Linkedin Reporter Stats Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_linkedinallowedreportingstats2.png)

### Web Security Block List Verification of traffic and policy enforcement via the IPsec Tunnel

*  Finally, access a specific website notpurple.com which is in the block list and see that when accessed over the tunnel gets blocked
*  Open an incognito browser tab and access notpurple.com
*  We get an immediate splash page blocking access to the site

   ![Blocked List Use Case 2](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_blockedlistnotpurpleviatunnel.png)

*  The reporter also shows the traffic to be BLOCKED as it is explicitly configured as part of the block list

   ![Blocked List Use Case 2 Reporter Stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_blockedlistviatunnelreporterstats.png)

## PoC use-case 3: Pre-requisites for enabling SSL decryption on the IPsec tunnel

The tunnels that get configured generally come disabled with SSL decryption, unless explicitly set.  Once the IPsec tunnel is enabled for SSL decryption, it is capable of performing ALL Advanced security features like Malware Defense, DLP, CASB and so on.

However, there are a few pre-requisites, without which Advanced security CANNOT be enabled on the IPsec tunnel with the Citrix SIA cloud

    Important 3 pre-requisites:

    1. Enable Transparent SSL Proxy under "Proxy/Caching -> SSL Decryption -> General Settings"
    2. Enable SSL decryption on the IPsec tunnel in the Local Subnets
    3. Download and Install the ROOT Certificate from the Citrix SIA Cloud and install on the end user device under trusted root 
    authorities (To enable SSL decryption by the platform)

### Enable Transparent SSL Decryption under Proxy/Caching -> SSL Decryption

For enabling the IPsec tunnel to allow for SSL decryption, the transparent SSL decryption must be enabled.

    Note: 
    SSL decryption can be applied for ALL IPsec tunnels depending on how we administer it. We can either choose SSL decryption for ALL 
    subnets or manually enable SSL decryption per Local Subnet of an IPsec tunnel.

For this demo PoC, we enable an explicit tunnel with SSL decryption and choose from General settings the value “Local subnets with SSL decryption enabled”

*  Enable Proxy SSL Decryption – YES
*  Perform SSL Decryption on – All Destinations
*  Enable Transparent SSL Decryption – YES
*  Perform Transparent SSL Decryption on – Local Subnets with SSL Decryption Enabled
*  Leave others to default

   ![Proxy and SSL Decryption Settings for IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_ssldecryptionsettingsviatunnel.png)

### Enable SSL decryption on the IPsec tunnel in the Local Subnets

   Note :

    Enable the decryption on the tunnel after it is configured in the SIA portal (From Orchestrator)

*  Click **Network -> Local Subnets -> Quick Edit Local Subnets** (As displayed in the following snapshot)

   ![Quick Edit Local Subnet](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_quickeditlocalsubnettunnel.png)

*  All IPsec tunnels created, need a local subnet that defines the local LAN subnet of the end hosts protected by the IPsec tunnel.
*  Choose the IPsec tunnel based on the name and the local subnet
*  The local subnet is automatically created with the local LAN subnet of the Citrix SIA site
*  Click the check box SSL to enable SSL decryption

   ![Enable Decryption](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_enabletunnelssldecryption.png)

*  While in the Quick edit window, double-click Default policy
*  Default policy indicates the security group that is applied to ALL the traffic falling under the Local Subnet (with LAN network defined)
*  Select "PoC_Demo_Group"
*  Also, double-click Login Group and select "PoC_Demo_Group"

   ![Select Non-Default Security Group for PoC](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_selectcustomsecuritygroupfortunnel.png)

### Download and Install the ROOT Certificate from the Citrix SIA Cloud and install on the end user device under trusted root authorities

Installing the Citrix SIA SSL root certificate on the browser of unmanaged devices is a MUST for SSL decryption.
SSL decryption is needed for applying advanced security capabilities like CASB, Malware defense, DLP and so on.

    Note:
    Installation of the ROOT certificate is a MANUAL process or must be performed via GPO (Group policy object) or MDM 
    or the enterprise presenting a splash page determining how to install the root certificate on the initiating devices

*  Go to **Proxy & Caching -> SSL Decryption -> Actions -> Generate and Download New MITM Root Certificate**

   ![Generate and Download Root SSL Certificate](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_generatedownloadcert.png)

*  A certificate is downloaded. Click the downloaded New MITM Root Certificate

   ![Install SSL Certificate on Unmanaged Device](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_installsslcertonunmanageddevice.png)

*  Click “Open” upon the double-click of the certificate

   ![Click Open the Certificate](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_clickopnsslcert.png)

*  Verify the certificate is issues by “Network Security” and is valid

   ![Verify Issuer on SSL Certificate](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_verifyissueronsslcert.png)

*  This takes us to the wizard. Under **Store location, select “Current User” and Click “Next”**

   ![Select Current User](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_selectcurrentsersslcert.png)

*  Click “Place all certificates in the following store” and Click “Browse”

   ![Browse Folder for SSL Certificate ](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_browsefolderforcert.png)

*  Click “Trusted Root Certification Authorities” and Click “OK”

   ![Select Trusted Root Authorities](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_selectrustedrootauthorities.png)

*  Notice after Step 6; the certificate store gets updated with “Trusted Root Certification Authorities”
Click “Next”

*  Click “Finish”

   ![Finish Certificate Installation](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_finishsslcertificateinstallation.png)

*  Notice that a window pops up indicating that the import was successful. Click OK for the popped-up window and then Click “OK” in the certificate install window

   ![SSL Certificate Import Successful](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_sslimportsuccessful.png)

*  Once installed, confirm that a certificate by Issuer “Network Security” is present in Trusted Root Certification Authorities.

        In Windows : Search for certmgr in the search box and you can open the certificate manager
        1. Navigate and Click Trusted Root Certification Authorities
        2. Click Certificates
        3. Verify a certificate by name “Network Security” (Highlighted)

If the certificate is found, then the installation is successful and the pre-requisites are complete to test SSL decryption via the IPsec tunnel

   ![SSL Certificate in Windows Certificate Manager](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_sslcertinwindowsmanager.png)

## PoC use-case 3: Users or devices without Cloud Connector (SIA Agent) in a non-guest network + Citrix SD-WAN in a branch via the IPsec Tunnel (Full security via IPsec tunnel and SSL Decryption)

In this use-case, we will use the Citrix SIA IPsec tunnel with SSL decryption and exercise Advanced security features from the Citrix SIA cloud like CASB, Malware Defense, DLP and so on.

Citrix SIA IPsec tunnel allows for unmanaged devices like BYOD, Personal and Guest devices to be managed securely behind a branch SD-WAN. The devices with the agent bypass the tunnel and get enforced of policies.

    Recommended Best Practice for IPsec Tunnel
    It is a recommended best practice that the in case where there are no Cloud Connector agents in devices, a branch managed by Citrix 
    SD-WAN uses the IPsec tunnel to manage the security policies for those devices securely with the Citrix SIA platform.

### CASB – Configuration of CASB policies from Citrix SIA Portal (Orchestrator)

We create a CASB rule to enable safe search on the Google Browser and also disable gmail.com access

*  Go to **CASB -> CASB App Controls -> Cloud App Controls**
    *  Select Group as “PoC_Demo_Group”
    *  Under **Search Engine Controls -> Google Safe Search Enforcement**
*  Select “Enforce Safe Search for Current Group”
*  Under Google Controls
    *  Enable Block Google Drive
*  Click “SAVE”

   ![CASB App Controls](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_casbappcontrols.png)

### Verification of CASB Policy enforcement and Reporting to check traffic status

Google Browser Safe Search enablement

*  Access google.com in an incognito browser tab
*  Soon as we access, we get google.com with a search bar
*  Enter Citrix or any keyword and initiate search
*  We see that the search results are filtered as the safe search is ON and is indicated as soon as the keyword is entered in the engine

   ![CASB access traffic reporting stats](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_googlesafesaerch.png)

Google Drive Blocking control

*  Access drive.google.com in an incognito browser tab
*  Soon as we access, we get a splash page indicating the access to the Google Drive is blocked due to the CASB control exercised in the PoC_Demo_Group Security Group

   ![CASB Google Drive Blocked](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_casbgoogledriveblockedssltunnel.png)

*  The reporter event logs also indicate access to Google Drive blocked
*  Category states “Google Drive Blocked” as it comes from CASB controls

   ![CASB Google Drive Blocked via SSL tunnel IPsec](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_repoterstatsssltunnelipsec.png)

### MALWARE Defense - Configuration of Anti Malware Policies from Citrix SIA portal

MALWARE Protection Configuration:  

    Note : 
    SSL Decryption must be enabled (If Cloud connector is used and configured to auto install the root certificate as part of agent installation and this feature is be enabled by default)

*  Goto Web Security -> Malware Defense -> Cloud Malware Protection
*  The Content engine must be ready
*  Click Block on Scan error to be Enabled

   ![Malware Config via SSL Enabled IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_malwareconfigviasslenabledtunnel.png)

### Verification of Anti Malware Policy enforcement and Reporting to check traffic status

*  Access <https://www.eicar.org/?page_id=3950>
*  Scroll down and click download the 68 byte file

   ![Prepare Malware Download Eicar](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_preparemalwaredownloadeicar.png)

Verification

*  Once the file download is initiated, a splash screen pops up. The page is blocked due to Virus detection.

   ![Verify Malware download via SSL enabled IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_verifymalwareviassltunnel.png)

*  Reporter also indicates the Malware protection enforced

   ![Malware Reporter Stats via SSL Enabled IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-reportermalwaresslipsectunnel.png)

### DLP (Data Loss Prevention)- Configuration of DLP Policies from Citrix SIA portal

Data Loss Prevention Configuration

*  Click Data Loss Prevention Tab -> DLP Settings
*  Click “Yes” for content analysis and data loss prevention
*  Ensure that that DLP Content and Analysis engine is ready and config is done appropriately
*  For the PoC, allow default selection for Content Engines and Analysis engines

    ![DLP Settings Check](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpsettingscheck.png)

*  Create a DLP rule so that Data Loss Prevention can be enforced on unauthorized content upload/download/both
    *  Go to Data Loss Prevention tab
    *  Click DLP Rules
    *  Click “+Add Rule”
    *  Under General Information Tab:
    *  Ensure Rule Enabled is “YES”
    *  Provide a name for the rule
    *  Enable HTTP Methods of PUT and POST so we can enforce DLP on uploads
    *  Leave content engines default

   ![DLP Rule Create](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlprulecreate.png)

    *  Under DLP tabs from Network Sources till Content Types
        *  Choose the policy as “Include All except selected items”
    *  Allow Defaults in the Search Criteria tab
    *  Select Action as “BLOCK”

   ![DLP Action to Block](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpactiontoblock.png)

### Verification of DLP (Data Loss Prevention) Policy enforcement and Reporting to check traffic status

*  Goto <https://dlptest.com> (a site to verify Data Loss Prevention tests)
*  Click Sample Data and view some sample information
*  Click XLS File (This downloads the file)

   ![DLP Test site download Excel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_downloaddlpsensitivefiletotest.png)

*  See the file gets downloaded with the name “sample-data.xls” under Downloads folder (or folder you have selected)

*  Now open a new browser incognito tab and type `http://dataleaktest.com`
    *  Click Upload Test
    *  In the Upload test page, scroll down and Click “SSL ON”
    *  After clicking on SSL ON, you can see the message on the black screen “System Ready for Next DLP Test with SSL ON”

   ![Data leak site DLP test with SSL ON](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dataleaksitesslondlptest.png)

*  Now Click Browse
    *  Select the file we previously downloaded file sample-data.xls and UPLOAD

   ![Upload DLP sensitive File](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_uploaddlpsensitivefile.png)

*  On uploading the sensitive file (containing SSN), a splash screen pops up. The page is blocked due to Unauthorized content detection.

   ![DLP Sensitive Data Block via SSL enabled IPsec Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_dlpblockedviasslenabledtunnel.png)

Verify Reporter Logs for DLP events:

*  Click More Filters
*  Choose log Type as DLP and click “Search”
*  The DLP upload gets blocked and a splash page is presented to the user

   ![DLP reporter stats via SSL enabled Tunnel](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_repoterdlpsslenabledtunnel.png)
