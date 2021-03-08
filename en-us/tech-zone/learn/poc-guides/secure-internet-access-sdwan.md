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

In this Proof-of-Concept guide, you will experience the role of a Citrix administrator who will create a connection between the organization’s edge SD-WAN device to the Citrix SIA Cloud via IPSec tunnels. 

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

*  **SD-WAN’s multi-wan link reliable IPSec tunnel for local subnets based Secure Internet access of agentless devices** With devices BYOD or personal laptops that are not managed by the customer, SD-WAN’s highly reliable (tunnel reliability via multiple wan links) will be able to exercise the Citrix SIA cloud based security policies to maintain the enterprise security posture

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
    *  Can be any appliance you choose for the PoC. Citrix SIA will work with ANY form-factor
    *  **Note:** PoC can also be performed on an Azure VPX based SD-WAN with a windows VM behind the VM on the LAN with/without a SIA software agents (also known as Cloud Connector Agent)
*  A Windows or MAC Laptop
*  Agent Windows/MAC MSI download from Citrix SIA cloud platform

### Network Requirements

*  Port / Firewall settings to SIA – Outbound connections

![Network Pre-Requisites](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_networkprereqs.png)

### Orchestrator - Custom Application to bypass SIA agent traffic from IPSec tunnel in the Citrix SD-WAN

**Recommended Best Practice:**
It is a recommended best practice that the SIA agent traffic from corporate managed devices be bypassed from the local IPSec tunnel of the SD-WAN in the branch edge. This allows direct proxy to the Citrix SIA where enterprise OU’s or security groups can be exercised directly via cloud connectors on the managed devices.
To allow for the Citrix SIA agent to seamlessly call home to the Citrix SIA gateway cluster and PoP’s, it is critical to bypass the cloud connector based traffic from the IPSec tunnel so that the registration of the cloud connector happens and the proxy is made available directly from the connector.

**Note :** It is NOT desired to have the SIA agent traffic go through the IPSec tunnel as there will be some issues with agent registration and policies to be exercised on the initiated traffic from the endpoint with the SIA agent

**Points to consider in creation of the SD-WAN Bypass custom application:**
The custom application will be based on the port list mentioned above in section Ports List of PoC Prerequisites.
All the bypassed specific IP/Protocols tagged as a custom application in the SD-WAN will be sent via Internet Service instead of letting it pass by the IPSec tunnel with Citrix SIA tunnel endpoint

<u>Few things to get handy from the Citrix SIA platform are:</u>

*  Citrix SIA Cloud Node IP’s part of the gateway cluster that will be reached by the cloud connector (SIA Agent) over port 443 as a proxied connection that needs to be sent via Internet service and bypassed from the tunnel

*  Citrix SIA Reporter node IP part of the node group management that will be reached for sending stats/reporting from the cloud connector (SIA Agent) that needs to be sent over Internet service and bypassed from the tunnel

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_ccbypassorchestrator.png)

**Node in Orange –** Reporter Node with IP – 104.225.171.47 and represented with a different icon (before node name).

**Node(s) in Green –** Cloud PoP nodes part of a CSIA gateway cluster with IPs 104.225.164.53 and 104.225.181.63 with the globe icon. A CSIA gateway cluster is an aggregate of two or more CSIA gateway nodes.

**Note :** When doing your PoC, check the nodes as per your account and apply the Orchestrator bypass rules accordingly. The accounts almost usually always have a reporter node but the gateway ndoes can be one or more. Ensure to place bypass to all gateway nodes so that they are accounted during the bypass (since the traffic can go to any cloud gateway node).

![Bypass CSIA Agent Orchestrator Policy](/en-us/tech-zone/learn/media/poc-guides_secure-internet-access-sdwan_orchestratorbypasspolicy.png)

### Orchestrator – Internet service cost to be made higher than SIA service cost (45) IF ALL Internet traffic is to make through the IPSec tunnel
