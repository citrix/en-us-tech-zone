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
