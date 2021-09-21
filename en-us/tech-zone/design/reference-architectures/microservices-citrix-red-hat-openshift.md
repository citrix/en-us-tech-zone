---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Priyanka Sharma
description: Learn how to design an environment to support cloud-native microservices with Citrix and Redhat Openshift.
tz_title: Microservices-Based Application Delivery with Citrix and Red Hat OpenShift Reference Architecture
tz_products: citrix-networking
---
# Reference Architecture: Microservices-Based Application Delivery with Citrix and Red Hat OpenShift

## Overview

CompanyA has always used Monolithic architectures to develop system applications predominantly hosted on-premises. They have suffered from issues with uptime and inconsistent performance, particularly for remote users, which was exacerbated during the pandemic. As part of their effort to move to the cloud, they intend to use a Microservices architecture. This architecture allows them to develop new applications with greater resiliency and scalability.

CompanyA decided to build a pair of redundant multi-cloud Red Hat OpenShift (RHOS) clusters. They are hosted in Microsoft Azure and Amazon AWS, with Citrix providing load balancing for microservice instances. This allows them to provide a resilient environment for remote users to access critical business web services with consistently good performance.

This reference architecture explains how CompanyA is planning their environment, to host a cloud-native platform, for developing new applications or migrating legacy ones.

## Introduction

Company A decided to develop their cloud-native microservices-based application delivery with Citrix and RHOS to bring several benefits to their enterprise and ultimately increase productivity.

### Cloud Native

Cloud-Native applications are developed to take advantage of the distributed and scalable nature of the cloud. It includes many benefits around Enterprise productivity, operational efficiency, and user experience.

Benefits of cloud-native

*  Containerized applications are portable between host infrastructures
*  Allows agile, continuous development, and delivery
*  Scales with cloud host infrastructures
*  Supports efficient software development process

### Red Hat OpenShift

Red Hat OpenShift is an enterprise Kubernetes container platform that helps companies deploy, operate, and secure microservice applications across hybrid clouds.

Benefits of Red Hat OpenShift (RHOS)

*  Efficiently manages Kubernetes cloud-native environments for the development and operation of business-critical Enterprise applications
*  Improves the productivity of development teams
*  Increases revenue by introducing new services to existing customers promptly
*  Reduces operating expense by spending less time on administration and support

For more information, see [What is Red Hat OpenShift?](https://www.RedHat.com/en/technologies/cloud-computing/OpenShift)

### Citrix

Citrix provides flexible topologies for traffic management in microservices environments, depending on requirements, including Full Mesh, ServiceMesh Lite, Single-Tier, and Dual-Tier.

Topologies:

*  Full Mesh – full mesh clusters include support for various microservices that need east-west communication between microservices within the cluster and north-south (N-S) communication outside of the cluster.
*  Service Mesh Lite- an Ingress solution typically performs L7 proxy functions for north-south traffic. The Service Mesh lite architecture uses the same Ingress solution to manage east-west (E-W) traffic and overcome the limitations of Kubernetes built-in service.
*  Single-Tier – single-tier clusters contain microservices that run as redundant replicas and have north-south traffic delivered by external load balancers.
*  Dual-Tier – dual-tier architectures also have north-south traffic delivered by external load balancers. Yet, also microservices have a networking component attached to support communication using additional networking protocols and optimizations not provided by native cluster services.

For more information, see: [How to accelerate your journey to microservice-based applications](https://www.youtube.com/watch?v=dnG6TXeVQUY)

Benefits of Citrix Ingress Controller (CIC)

*  Standard Kubernetes Ingress solutions provide load balancing only at layer 7 (HTTP or HTTPS traffic), while the CIC also supports TCP, TCP-SSL, and UDP traffic
*  The CIC works seamlessly across multiple clouds or on-premises data centers

Benefits of Citrix ADC VPX

*  Citrix ADC VPX provides enterprise-grade traffic management policies like rewrite and responder policies for efficiently load balancing traffic at layer 7, which Kubernetes does not provide
*  Citrix ADC VPX also supports Global Server Load Balancing (GSLB)

Benefits of Citrix CPX

*  Citrix CPX enables a Citrix ADC to be deployed as a data plane proxy either as an Ingress gateway or sidecar in the xDS-based service mesh.
*  It provides layer 7 traffic management between microservices inside the Kubernetes cluster, whereas Kubernetes only supports Layer 4.

For more information, see [Citrix Developer Docs](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/)

## Success Criteria

Company A has defined a list of success criteria that formed the basis for the overarching design.

Note: Company A deploys an Apache web service in a production pilot for remote user validation.

[![Success Criteria](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_successcriteria.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_successcriteria.png)

## Conceptual Architecture

Based on the preceding requirements, CompanyA created the following high-level conceptual architecture. This architecture meets all initial requirements while giving CompanyA the foundation to expand to more use cases in the future.

[![Conceptual Architecture](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_conceptualarchitecture.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_conceptualarchitecture.png)

The architecture framework is divided into multiple layers. The framework provides a foundation for understanding the technical architecture for the microservices infrastructure. All layers flow together to create a complete, end-to-end solution.

At a high level:

**User Layer:** The user layer describes the end-user environment and endpoint devices used to connect to resources.

Users can connect to the web service securely using the Citrix Workspace app. Alternatively, users can connect to the web service using a standard browser with HTTP/HTTPs transport.

**Access Layer:** The access layer describes how users access web services and north-south flows are delivered.

*  The primary FQDN of the web service resolves to name servers hosted on the Citrix ADC VPX.
*  Citrix ADC `VPXes` running GSLB respond to Domain Name Service (DNS) queries with a public IP address of the Content Switch Virtual Server with the least connections.
*  The Virtual Server is configured by the Citrix Ingress Controller to forward connections to the cluster hosted Citrix CPX with the least connections
*  The cluster-hosted Citrix CPX accepts the connections and responds to the Citrix ADC VPX. It establishes a flow to the web service over which payload is delivered.

**Resource Layer:** The resource layer specifies the applications delivered to users in the form of microservices in this reference architecture.

*  Four Apache web services are hosted as RHOS Pods. They are deployed through the RHOS operator hub.

**Control Layer:** The control layer defines how the resources are managed and monitored.

*  Red Hat OpenShift is used to build the cluster and deploy, manage, and monitor the microservice resources.

**Host Layer:** The hosting layer defines the underlying infrastructure that hosts the resources, including memory, storage, and compute.

*  Microsoft Azure and Amazon AWS are the public IaaS used to host the RHOS Cluster and microservices.

The next sections provide greater detail into specific design decisions for CompanyAs microservices-based application eelivery with Citrix and Red Hat OpenShift.

## User Layer

The User Layer is where users request and access target resources on supported endpoints.

[![User Layer](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_userlayer.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_userlayer.png)

Users can connect to the web service securely using the Citrix Workspace app.

*  Citrix Workspace – the web service is published as an application in Citrix Workspace. The Citrix Workspace App installed on the user’s endpoint starts a proxy connection to the published application in Citrix Cloud
*  Citrix Secure Internet Access – the connection toward the cloud-hosted Citrix ADC VPX is proxied by Citrix Secure Internet Access for full-stack inspection, including SWG, DLP, CASB, and NGFW
*  Citrix Web Application and API Protection – the connection to the cloud-hosted Citrix ADC VPX is proxied and inspected by Citrix Web Application and API Protection web application firewall signatures.

## Access Layer

The Access Layer is where network delivery components are hosted to coordinate and direct user sessions requests to Control and Resource components.

[![Overview](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_accesslayer.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_accesslayer.png)

### Citrix CPX

CompanyA decided to implement a 2-tier architecture and use the Citrix CPX to manage the delivery of service traffic within the cluster. The CPX receives user traffic requests from the cloud-hosted Citrix ADC VPX and balances the traffic load between microservice instances. The Citrix CPX is deployed, by the cluster admin, through YAML file configuration using RHOS controls. The Citrix CPX is deployed in both the AWS and Azure clusters in the same manner.

### Citrix Ingress Controller

CompanyA decided to use the Citrix Ingress Controller (CIC) to manage Citrix cloud-native networking within their RHOS cluster. The Citrix Ingress Controller is used to manage ingress cluster traffic flow. It uses global cluster custom resource domains (CRDs) to obtain and monitor Citrix CPX and service status. Based on this information, it dynamically configures the Citrix ADC VPX to load balance and route traffic to Citrix CPXes within the cluster.

### Citrix ADC VPX

CompanyA decided to use the Citrix ADC VPX to manage their North-South traffic flows and to implement Global Server Load Balancing (GSLB) between Azure and AWS clusters.

**North-South** traffic is managed by Citrix ADC VPXes hosted at the AWS and Azure cluster sites, respectively. IP addressing information and access secrets are provided in the CIC setup, allowing it to configure load balancing and content switching policies.

**GSLB** traffic is also be managed by Citrix ADC VPXes hosted at the AWS and Azure cluster sites, respectively.

*  DNS for the Apache microservice is configured through the company’s global DNS service [AWS Route 53](https://aws.amazon.com/route53/)
*  CNAME records map to respective authoritative DNS (ADNS) services hosted on Citrix ADC VPXes in Azure and AWS, respectively.
    *  apacheservice.CompanyA.com
    *  apacheservice.AWS.CompanyA.com
    *  apacheservice.Azure.CompanyA.com
*  GSLB Load Balancing Method – Citrix ADC GSLB supports a various load balancing methods. CompanyA has decided to use the Canary method primarily to support high uptime with their continuous development cycle. Method options:
    *  Local first: In a local first deployment, when an application wants to communicate with another application, it prefers a local application in the same cluster. When the application is not available locally, the request is directed to other clusters or regions
    *  Canary: Canary release is a technique to reduce the risk of introducing a new software version in production by first rolling out the change to a small subset of users. In this solution, canary deployment can be used when you want to roll out new versions of the application to selected clusters before moving it to production
    *  Failover: A failover deployment is used to deploy applications in an active/passive configuration when they cannot be deployed in active/active mode
    *  Round trip time (RTT): In an RTT deployment, the real-time status of the network is monitored and dynamically directs the client request to the data center with the lowest RTT value
    *  Static proximity: In a static proximity deployment, an IP-address-based static proximity database is used to determine the proximity between the client’s local DNS server and the GSLB sites. The requests are sent to the site that best matches the proximity criteria
    *  Round-robin: In a round-robin deployment, the GSLB device continuously rotates a list of the services bound to it. When it receives a request, it assigns the connection to the first service in the list and then moves that service to the bottom of the list
*  GSLB Services – The Citrix ADC VPX, in each site, monitors and manages traffic distribution to the Citrix CPX instances hosted within the respective clusters.

For more information, see [Multi-cluster ingress and load balancing solution using the Citrix ingress controller](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/multicluster/multi-cluster/)

## Resource Layer

Resources include various microservices applications, available through the RHOS Operator Hub, that can be developed internally or obtained through a third-party vendor, depending on requirements. CompanyA has decided to deploy the Apache web application.

For more information, see [Understanding RHOS Operator Hub](https://docs.openshift.com/container-platform/4.6/operators/understanding/olm-understanding-operatorhub.html)

## Control Layer

The controller layer includes essential management components to coordinate the delivery of microservices.

Red Hat OpenShift
CompanyA has chosen to use Red Hat OpenShift, version 4.7, to deploy and manage their Kubernetes cluster.

## Host Layer

RHOS clusters are supported on various hosting platforms On-Premises, Cloud, or Hybrid Cloud.

### Azure

CompanyA decided to host one of their RHOS environments in a Microsoft Azure tenant. The RHOS cluster used the Azure CLI to build the cluster.

Key requirements:

*  Azure Red Hat OpenShift requires a minimum of 40 cores to create and run an OpenShift cluster
*  An Azure Red Hat OpenShift cluster consists of 3 master nodes and three or more worker nodes.
*  Azure CLI version 2.6.0 or later

For more information see [Azure Openshift cluster](https://docs.microsoft.com/en-us/azure/OpenShift/tutorial-create-cluster)

### AWS

CompanyA decided to host a second RHOS environment in an AWS tenant. The RHOS cluster used the AWS quick start process to build the cluster.

Key requirements:

*  The Quick Start process requires a Red Hat subscription
 The tenant must allow provisioning of Amazon EC2 `M4.xlarge` instance
*  Red Hat entitlement limits and AWS instance limits were set to support the deployment of 3 masters instances and 3 worker nodes

For more information, see [Red Hat OpenShift on AWS – Reference Deployment](https://aws.amazon.com/quickstart/architecture/OpenShift/)

## References

Many document links are available to understand better Citrix cloud-native networking concepts, Kubernetes microservices, and the Red Hat OpenShift platform.

Find links to pertinent **Red Hat** References here:

*  [Red Hat OpenShift](https://www.RedHat.com/en/technologies/cloud-computing/OpenShift)

*  [Red Hat Login](https://cloud.RedHat.com/OpenShift/create)

*  [Red Hat OpenShift on AWS](https://aws.amazon.com/quickstart/architecture/OpenShift/)

*  [Create an Azure Red Hat OpenShift 4 cluster](https://docs.microsoft.com/en-us/azure/OpenShift/tutorial-create-cluster)

*  [Developer Sandbox for Red Hat OpenShift](https://developers.RedHat.com/developer-sandbox/activities)

*  [Red Hat OpenShift installation resources](https://developers.RedHat.com/products/OpenShift/overview)

*  [Microservices-Based Application Delivery with Citrix and Red Hat OpenShift – blog](https://www.OpenShift.com/blog/microservices-based-application-delivery-with-citrix-and-red-hat-OpenShift)

*  [Azure Red Hat OpenShift pricing](https://azure.microsoft.com/en-us/pricing/details/openshift/#pricing)

Find links to pertinent **Citrix** References here:

*  [Microservices-Based Application Delivery with Citrix and Red Hat OpenShift](https://www.OpenShift.com/blog/microservices-based-application-delivery-with-citrix-and-red-hat-OpenShift)

*  [Citrix Developer Docs](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/)

*  [Citrix ADC and OpenShift 4 Solution Brief – product documentation](https://en-us/advanced-concepts/implementation-guides/citrix-adc-and-OpenShift-solution-brief.html)

*  [Enable OpenShift router sharding support with Citrix ADC](https://www.citrix.com/blogs/2019/09/11/enable-OpenShift-router-sharding-support-with-citrix-adc/)

*  [Accelerate your journey to microservices](https://www.citrix.com/solutions/application-delivery-controller/microservices/)

*  [Configure Citrix ADC to load balance an OpenShift control plane](https://www.citrix.com/blogs/2020/08/20/configure-citrix-adc-to-load-balance-an-OpenShift-control-plane/)

*  [Deploy Citrix API gateway using Red Hat OpenShift Operator](https://www.citrix.com/blogs/2020/08/27/deploy-citrix-api-gateway-using-red-hat-OpenShift-operator/)

*  [Deploy the Citrix ingress controller as an OpenShift router plug-in](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/deploy/deploy-cic-OpenShift/)

*  [Multi-cluster ingress and load balancing solution using the Citrix ingress controller](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/multicluster/multi-cluster/)

*  [Multi-cloud and multi-cluster ingress and load balancing solution with Amazon EKS and Microsoft AKS clusters](https://developer-docs.citrix.com/projects/citrix-k8s-ingress-controller/en/latest/deploy/multi-cloud-ingress-lb-solution/)

*  [Best Practices for Cloud-Native Application Delivery with Citrix and Red Hat](https://www.youtube.com/watch?v=UNBtcgaIKCA)

 [How to accelerate your journey to microservice-based applications](https://www.youtube.com/watch?v=dnG6TXeVQUY)

## Appendix

### Terminology

Find descriptions of common RHOS and Microservice terminology.
[![RHOS References](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_rhosterminology.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_rhosterminology.png)
