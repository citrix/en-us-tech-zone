---
layout: doc
h3InToc: true
contributedBy: Matt Brooks
specialThanksTo: Priyanka Sharma
description: Learn how to design an environment to support cloud native microservices with Citrix and Redhat Openshift.
tz_title: Microservices-Based Application Delivery with Citrix and Red Hat OpenShift®
Reference Architecture
tz_products: citrix-networking;citrix-service-providers;citrix-workspace;google-cloud-platform;other;security;third-party-content
---
# Content Type : Microservices-Based Application Delivery with Citrix and Red Hat OpenShift® - Reference Architecture

## Overview

CompanyA has always used Monolithic architectures to develop system applications predominantly hosted on premises. They have suffered from issues with up time, and inconsistent performance, particularly for remote users which has been exacerbated during the pandemic. Now as part of their effort to move to the cloud they intend use a Microservices architecture for the development of new applications in order to take advantage of the resiliency, and scalability benefits.

CompanyA decided to build a pair of redundant multi-cloud Red Hat® OpenShift® (RHOS) clusters, hosted in Microsoft Azure and Amazon AWS, with Citrix to provide load balancing and routing to microservice nodes. This will allow them to provide a resilient environment for the remote user to access business critical web services with consistently good performance.

This reference architecture explains how CompanyA is planning their environment to ensure they can have a cloud native platform to develop new applications, or in the future migrate legacy ones, that is cost-effective and allows end-users to work remotely with good performance.
Introduction
Company A decided to develop their cloud native Microservices-Based Application Delivery with Citrix and RHOS to bring several benefits to their enterprise and ultimately increase productivity.

## Cloud Native

Cloud Native applications are developed to take advantage of the distributed and scalable nature of the cloud. It includes many benefits around Enterprise productivity, operational efficiency, and user experience.

Benefits of cloud native

* Containerized applications are portable between host infrastructures
* Allows agile, continuous development and delivery
* Scales with cloud host infrastructures
* Supports efficient software development process

## Red Hat® OpenShift®

Red Hat® OpenShift® is an enterprise Kubernetes container platform that helps companies deploy, operate and secure microservice applications across hybrid clouds.

Benefits of Red Hat® OpenShift® (RHOS)

* Efficiently manages Kubernetes cloud native environments for the development and operation of business-critical Enterprise applications
* Improves the productivity of development teams
* Increases revenue by introducing new services to existing customers in a timely manner
* Reduces operating expense by spending less time on administration and support

For more information see [What is Red Hat OpenShift®?](https://www.RedHat.com/en/technologies/cloud-computing/OpenShift)

## Citrix

Citrix provides flexible topologies for traffic management, in microservices environments, depending on requirements, including Full Mesh, Single-Tier, Dual-Tier and ServiceMesh Lite.

Topologies:

* Full Mesh – full mesh clusters include support for a variety of microservices that need east-west communication between microservices within the cluster as well as north-south communication outside of the cluster.
* Service Mesh Lite- an Ingress solution typically performs L7 proxy functions for north-south (N-S) traffic. The Service Mesh lite architecture uses the same Ingress solution to manage east-west traffic as well overcoming limitations of Kubernetes built-in service.
* Single-Tier – single-tier clusters contain microservices that run as redundant replicas and have north-south traffic delivered by external load balancers.
* Dual-Tier – dual-tier architectures also have north-south traffic delivered by external load balancers, yet also microservices have a networking component attached to support communication using additional networking protocols and optimizations not provided by native cluster services.

For more information see: [How to accelerate your journey to microservice- based applications](https://www.youtube.com/watch?v=dnG6TXeVQUY)


[![Overview](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_00.png)](/en-us/tech-zone/design/media/reference-architectures_microservices-citrix-red-hat-openshift_00.png)
