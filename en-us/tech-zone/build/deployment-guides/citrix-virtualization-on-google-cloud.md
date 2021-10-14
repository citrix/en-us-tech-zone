---
layout: doc
h3InToc: true
author: Sameer Sharma
Special thanks: Rick Dehlinger
contributedBy: 
---

# Citrix virtualization on Google Cloud

## Deployment and Configuration Guide

## Introduction

This step-by-step Deployment and Configuration guide is intended for people who install, configure, deploy, and maintain Citrix virtualization systems on Google Cloud. You can think of this guide as a double click down from the [Citrix Virtualization on Google Cloud Reference Architecture](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html) document, also available on Citrix TechZone. The Reference Architecture helps you decide **the virtualization system's architecture**. We implement the [Cloud Forward](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html#the-cloud-forward-design-pattern) design on Google Cloud to keep this Deployment and Configuration guide focused and succinct. The design outlined here may not match the architecture of your system perfectly, but it should help you through most of the tasks required to create a functional Citrix virtualization system on Google Cloud. To help guide you on this journey of understanding, we are breaking down each of the major steps into **goals**, followed by step-by-step guides on **one way** to accomplish our **prime goal: build a Citrix virtualization system on Google Cloud**.

As you take this journey with us, we encourage you to share your comments, contributions, questions, suggestions, and feedback along the way. Drop us a line via email to [Citrix on Google SME working group](mailto:caceb231.citrix.onmicrosoft.com@amer.teams.ms).

Citrix offers self-paced and instructor lead versions. For more information, see the following Citrix Education website at [https://training.citrix.com/learning](https://training.citrix.com/learning). Or contact your Citrix Customer Success Manager for more information.

## Deployment Overview

The following list provides an overview of the goals and **tasks** necessary to create a Citrix virtualization system on Google Cloud:

1.  Define the deployment architecture
2.  Prepare Google Cloud Project
3.  Configure network and network services
4.  Create virtual machines
5.  Configure access to virtual machine consoles
6.  Deploy Active Directory
7.  Create/initialize Citrix Cloud resource location
8.  Configure Citrix Virtual Apps and Desktops service
9.  Test launch virtual app and desktop resources

## 1 Define the deployment architecture

Before you build a Citrix virtualization system on **ANY** platform, you need to figure out **WHAT** you’re building – the deployment architecture. This can be a challenge given the flexibility of Citrix’s technology stack, but we break down the most important design decisions to consider in the [Citrix Virtualization on Google Cloud Reference Architecture](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html) document. We hghly recommend you spend some time in the Reference Architecture document before you build your production system as the optimal architecture for your use case may be different than what we guide you through here. In this Deployment and Configuration Guide, we are guiding you through building a virtualization system that is based on the [Cloud Forward](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html#the-cloud-forward-design-pattern) design pattern as illustrated in Figure 1:

![cloud-forward-deployment-architecture](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-forward-deployment-architecture.png)

Figure 1: Cloud Forward Deployment Architecture

## 2 Prepare Google Cloud Project

The goal of this section is to create or gain access to and prepare the Google Cloud project you are using as your Citrix Cloud Virtual Apps and Desktops Service [Resource Location](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html). When complete, you have:

-  A brief introduction to the main components of Google Cloud

-  Obtain access to a Google Cloud project with the Project Owner role assigned to your user

-  Prepared the Google Cloud project (enabled API’s, created a Citrix service account, enabled or updated Cloud Build service account) such that Citrix Cloud has what it needs for Virtual Desktop Agent (VDA) fleet creation and lifecycle management

> **Note**:
>
> This section provides a walk-through of the requirements outlined in Citrix’s documentation for [creating a resource location on Google Cloud](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html). While these requirements were complete and correct at the time of writing, you want to refer to the product documentation for the most current requirements.

## **Google Cloud Primer**

Google Cloud consists of services and resources, including Google Cloud Storage (GCS) and Google Compute Engine (GCE) virtual machines (VMs), hosted in Google’s data centers around the globe and vary according to [geographic regions](https://cloud.google.com/about/locations#network). Data centers are referenced by their global region such as Central US, Western Europe, and East Asia where resources are hosted. A zone is an isolated location within a region. The zone determines what computing resources are available and where your data is stored and accessed. A region can have three or more zones. For example, the us-west1 region denotes a region on the west coast of the United States that has three zones us-west1-a, us-west1-b, and us-west1-c.

[Google Billing](https://cloud.google.com/billing/docs) accounts are a prerequisite for creating Google Project and deploying the Compute Engine instances. The Google billing is linked to a Google payments profile. In a GCP environment, billing accounts can be linked to one or more projects which are aligned to an organization (Figure 3). It is important to ensure that the proper billing structure is in place in your organization before proceeding forward. Billing in GCP is based on usage, similar to other public cloud vendors. Google does not charge for virtual machines while they are in terminated state; however, Google charges for storing the preserved state of suspended virtual machines. Google offers committed use discounts (1 or 3 years) for virtual machine usage which is ideal for workloads with predictable resource needs. Also, Google offers sustained use discounts which automatically applies the discount for specific Compute Engine used more than 25% a month. However, the sustained use discounts are not applicable to the E2 and A2 machine instances. Google also provides sizing recommendations for VMs instances using built-in intelligence.

![google-billing](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-billing.png)

Figure 2

GCP organizes the resources into a project. All [GCP projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects) are assigned to a single user project creator role by default. It is recommended to create extra project members and identity access and management roles to control access to the GCP project. A project consists of a set of users, a set of APIs, billing, authentication, and monitoring settings for the APIs. The GCP project has the following components:

-  A project name, which you can provide **or** assigned automatically

-  A project ID, which you can provide **or** assigned automatically

-  A project number, non-customizable **and** assigned automatically

### 2.1 Create or Access Google Project

The goal of this task is to ultimately gain access to the Google Cloud Project that contains your Citrix virtualization assets. If someone else in your organization is responsible for creating projects, they need to assign you the Identity Access Management (IAM) “Owner” role inside the project for you to complete the rest of this goal. If you’re the one creating the project, the following steps guides you through the process.

The following steps are completed from within the Google Cloud Console ([https://console.cloud.google.com](https://console.cloud.google.com)). Make sure that you’re logged into this console with the valid user account – it can sometimes be confusing if you’re logged into the (Chrome) browser with multiple accounts.

1.  Click **Select a project** from the drop-down list

    ![google-cloud-platform-dashboard](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-platform-dashboard.png)

2.  Click **New Project**

    ![google-cloud-platform-new-project](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-platform-new-project.png)

3.  Input a unique project name (e.g., citrixcloud)

4.  Click **Browse** and select your organization

5.  Click **Create**

    ![google-cloud-platform-create](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-platform-create.png)

6.  Validate the project is successfully created

    ![google-cloud-platform-project-info](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-platform-project-info.png)

### 2.2 Enable Google Cloud API’s

Citrix Cloud interacts with your Google Cloud project by using several different API’s. These APIs are not necessarily enabled by default, but they are necessary for Citrix VDA fleet creation and lifecycle management. For Citrix Cloud to function, the following Google APIs must be enabled on your project:

-  Compute Engine API

-  Cloud Resource Manager API

-  Identity and Access Management (IAM) API

-  Cloud Build API

-  \* Cloud Domain Name System (DNS) API

> ***Note**:
>
> The Cloud DNS API is required to configure DNS services for your project, which are necessary to setup Active Directory on Google Cloud. If your infrastructure team manages the Active Directory and DNS infrastructure components, you can effectively skip this step. The other four APIs are used by Citrix Cloud and must be enabled.

The Google APIs can be enabled from the [APIs and Services/Dashboard or APIs and Services/Library](https://cloud.google.com/apis/docs/getting-started) pages if you prefer to use the graphical console. This can also be completed **quickly** by using the Google Cloud Shell as described:

1.  Click the Google Shell icon located in the top right-hand corner of the Google Console:

    ![google-shell-icon](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-shell-icon.png)

2.  The following Cloud Shell appears within the Google Console:

    ![google-shell-console](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-shell-console.png)

3.  Paste the following four commands into Cloud Shell, if the Cloud Shell prompts, click **Authorize**:

    gcloud services enable compute.googleapis.com

    gcloud services enable cloudresourcemanager.googleapis.com

    gcloud services enable iam.googleapis.com

    gcloud services enable cloudbuild.googleapis.com

    gcloud services enable dns.googleapis.com

    ![google-cloud-shell](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-shell.png)

### 2.3 Create/update service accounts

Google Identity and Access Management (IAM) allows you to grant granular access to specific Google Cloud resources. The **WHO** has access to **WHICH** resources, and **WHAT** they can do with those resources. Service accounts ‘live’ inside projects, similar to other resources you provision on Google Cloud.

Citrix Cloud uses **two** separate service accounts within the Google Cloud project:

-  **Cloud Build Service Account** – provisioned **automatically** once the Google APIs are enabled, the Cloud Build service account is identifiable with an email address that begins with the Project ID, for e.g., [\<project-id\>@cloudbuild.gserviceaccount.com](mailto:112233445566@cloudbuild.gserviceaccount.com). The Cloud Build Account requires the following three roles:
  
    -  Cloud Build Service Account (assigned by default)

    -  Compute Instance Admin
  
    -  Service Account User
  
-  Citrix Cloud Service Account - the Service Account is used by Citrix Cloud to authenticate to Google Cloud using a pre-generated key. The Service Account enables Citrix Cloud to access the Google project, provision and manage machines. The Service Account is created **manually** and identifiable with an email address, for e.g., [\<my-service-account\>@\<project-id\>.iam.gserviceaccount.com](mailto:serviceaccount@xxx-deployment-iam.gserviceaccount.com). By applying the principle of least privilege, grant the following roles to the Service Account:

    -  Compute Admin

    -  Storage Admin

    -  Cloud Build Editor

    -  Service Account User

    -  Cloud Datastore User

#### 2.3.1 Update the roles assigned to the Cloud Build Service Account

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **IAM & Admin**

3.  Click **IAM**

    ![google-console-iam-admin](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-console-iam-admin.png)

4.  Locate the Cloud Build service account, identifiable with an email address that begins with the Project ID, for e.g., [\<project-id\>@cloudbuild.gserviceaccount.com](mailto:112233445566@cloudbuild.gserviceaccount.com). The Cloud Build Service Account role is applied by default.

    > **Note**:
    >
    > If you are unable to view the Cloud Build account, then it may require you to enable the Cloud Build account manually by following steps:

    a. Click the hamburger menu

    b. Scroll down until you locate **CI/CD** menu section

    c. Click **Cloud Build**

    d. Click **Settings**

    e. Locate **Service Accounts**, under the Status column, click the drop-down menu and change from **DISABLED** to **ENABLED**

    f. Return to the IAM & Admin menu section and locate the Cloud Build Service Account

5.  Click the pencil icon to edit the Cloud Build account roles

    ![pencil-icon-to-edit](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_pencil-icon-to-edit.png)

6.  Click **Add Another Role**

    ![google-shell-add-another-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-shell-add-another-role.png)

7.  Click **Select a role** drop-down menu

    ![google-cloud-shell-select-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_google-cloud-shell-select-role.png)

8.  Click drop-down list and input **Compute Instance Admin**

9.  Click **Compute Instance Admin (v1)** from the list

    ![compute-instance-admin-v1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-v1.png)

10.  Click **Add Another Role**

     ![compute-instance-admin-add-another-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-add-another-role.png)

11.  Click **Select a role** drop-down menu

     ![compute-instance-admin-select-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-select-role.png)

12.  Click drop-down list and input **Service Account User**

13.  Click **Service Account User** from the list

     ![service-account-iam-admin](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-service-account.png)

14.  Click **Save**

     ![compute-instance-admin-save](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-save.png)

15.  Validate the roles are assigned successfully

     ![compute-instance-admin-assigned-successfully](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-instance-admin-assigned-successfully.png)

#### 2.3.2 Create and assign roles to Citrix Cloud Service Account

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **IAM & Admin**

3.  Click **Service Accounts**

    ![service-account-iam-admin](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-account-iam-admin.png)

4.  Click + **Create Service Account**

    ![create-service-account](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-service-account.png)

5.  Input a unique **Service account name**

6.  The **Service account ID** will auto populate based on the Service account name entered

7.  Provide a description for the Service account name

8.  Click **Create and Continue**

    ![create-and-continue](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-and-continue.png)

9.  Click **Select a role** drop-down menu

    ![select-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-role.png)

10.  Click drop-down list and input **Compute Admin**

11.  Click “Compute Admin” from the list

     ![compute-admin](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-admin.png)

12.  Click **Add Another Role**

     ![compute-add-another-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-add-another-role.png)

13.  Click **Select a role** drop-down menu

     ![compute-select-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-select-role.png)

14.  Click drop-down list and input **Storage Admin**

15.  Click **Storage Admin** from the list

     ![compute-storage-admin](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-storage-admin.png)

16.  Click **Select a role** drop-down menu

     ![compute-storage-admin-select-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_compute-storage-admin-select-role.png)

17.  Click drop-down list and input **Cloud Build Editor**

18.  Click **Cloud Build Editor** from the list

     ![build-editor](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_build-editor.png)

19.  Click **Add Another Role**

     ![build-editor-add-another-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_build-editor-add-another-role.png)

20.  Click drop-down list and input **Service Account User**

21.  Click **Service Account User** from the list

     ![build-editor-service-account](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_build-editor-service-account.png)

22.  Click **Add Another Role**

     ![build-editor-service-account-add-another-role](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_build-editor-service-account-add-another-role.png)

23.  Click drop-down list and input **Cloud Datastore User**

24.  Click **Cloud Datastore User** from the list

     ![datastore-user](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_datastore-user.png)

25.  Click **Continue**

     ![datastore-user-continue](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_datastore-user-continue.png)

26.  Click **Done**

     ![datastore-done](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_datastore-done.png)

27.  Navigate to IAM main console

     ![navigate-iam](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_navigate-iam.png)

28.  Identify the Service Account created

29.  Validate the five roles are assigned successfully

     ![build-editor-validate](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_build-editor-validate.png)

> **Note**:
>
>If your organization intends to deploy Citrix VDA’s onto a Google Cloud Shared Virtual Private Cloud (VPC), there are some additional permissions the Citrix Cloud service account requires that are NOT covered here. To prepare for deploying VDA’s on Shared VPC’s, see “[Google Cloud Platform environments, Shared Virtual Private Cloud](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html#shared-virtual-private-cloud)”.

#### 2.3.3 Generate and save Citrix Cloud service account keys

In this task, you’ll generate and download the service account keys that Citrix Cloud uses when configuring the hosting connection and resources later in this document. Make sure you treat the JSON file securely.

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **IAM & Admin**

3.  Click **Service Accounts**

    ![service-account](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-account.png)

4.  Click the previously created Service Account

5.  There are likely no keys created yet

    ![service-no-keys](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-no-keys.png)

6.  Click **Keys** tab

7.  Click **Add Key**

8.  Select **Create new key**

    ![service-create-keys](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-create-keys.png)

9.  Select key type **JSON** (default)

10.  Click **Create**

     ![service-create-json](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-create-json.png)

11.  The JSON private key automatically downloads. Locate the JSON file and store it securely -- it is required when creating the hosting connection between Citrix Cloud and Google Cloud in upcoming steps.

12.  Click **Close**

     ![service-create-json-close](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_service-create-json-close.png)

## 3 Configure/verify network and network services

This segment covers the networking services required to host a Citrix Cloud “[Resource Location](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location.html)” on Google Cloud. Completion of the last goal leaves you with an empty project on Google Cloud, into which we are building a Citrix virtualization system. This section aims to layer in the networking-related services needed to complete our prime goal.
Building and configuring a network becomes complex relative to the size of the organization's underlying system requirements. In essence, a Citrix virtualization system on Google Cloud requires the following to function:

-  A Virtual Private Cloud (VPC) to interconnect Citrix VDAs and Citrix Cloud Connectors.

-  A method for Citrix Cloud Connectors (and optionally VDAs) to interact with Citrix Cloud's managed web services (APIs). Both only require outbound connectivity, which is commonly provided by Google's **Cloud NAT** service.

-  A method for Citrix Cloud Connectors to communicate with **Google Cloud's APIs**. A Google Cloud VPC feature called Google private access must be enabled to allow communication.

-  A method for Citrix Cloud Connectors and VDAs to communicate with Active Directory and other network resources, including the Internet. A common way to provide DNS in a Citrix virtualization system is with the **Google Cloud DNS** service.

-  A network layer securing using Google's VPC firewall.

Your organization may have a different way to provide the necessary functionalities we describe here, but if the goals can be completed and functionally verified, you should be well on your way to building the Citrix virtualization environment.

### 3.1 Create/access a network for VDA’s and Cloud Connectors

On Google Cloud, virtual machine resources need a [VPC](https://cloud.google.com/vpc#section-5) network for communication. A VPC ‘lives’ inside a specific Google Cloud project and can be optionally shared [between multiple projects](https://cloud.google.com/vpc/docs/shared-vpc) as needed. Google calls the latter feature “[Shared VPC](https://cloud.google.com/vpc/docs/shared-vpc)”, and while Citrix Cloud supports [deploying Citrix VDA’s on Shared VPC networks](https://docs.citrix.com/en-us/citrix-virtual-apps-desktops-service/install-configure/resource-location/google.html#shared-virtual-private-cloud), the setup and configuration is outside of the scope of this simplified guide.

A VPC is a project global construct, meaning it can span not only zones inside regions, but also span regions around the globe. At least one subnet is required in each region where virtual machines will be deployed, though on Google Cloud, a subnet often spans zones.

For this deployment guide a VPC network is used to provide the internal (private) connectivity between the three Google zones (us-west1-a, us-west1-b and us-west1-c). A VPC (Figure 3) is a virtual version of a physical network implemented within the Google’s worldwide network. A VPC is considered a global resource and it is not associated with any Google region or zone. A VPC network can be created using auto mode or custom mode. The auto mode VPC network creates a single subnet per region automatically when the network is created. As new regions become available, new subnets in those regions are automatically added to the auto mode network. A custom mode VPC network requires all subnets to be created manually.

Note also that there is a [cost](https://cloud.google.com/vpc/network-pricing) associated with traffic flows on Google Cloud. The VPC ingress traffic is free. The egress traffic to the same zone, to a different GCP service in the same region, and to Google products are all free; however, the egress traffic between zones in the same region or regions within the US costs $0.01/GB.

![virtual-private-cloud](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_virtual-private-cloud.png)

Figure 3 Virtual Private Cloud

The goal of this task is to create a functional VPC that will be used to build a Citrix Cloud Resource Location on Google Cloud. This task can be considered complete when the Citrix Cloud service account (created earlier) can provision a virtual machine on a functional VPC.

#### 3.1.1 Create a VPC network and subnet

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **VPC network**

3.  Click **VPC networks**

    ![vpc-networks](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks.png)

4.  Click **Create VPC Network**

    ![vpc-networks-create](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-create.png)

5.  Input a unique name for the VPC network

6.  Input a description for the VPC network

    ![vpc-networks-input-description](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-input-description.png)

7.  Select **Custom**

8.  Input a unique name for the Subnet name

9.  Input a description for the Subnet

10.  Select **us-west1** region

11.  Input the following IP address range: **10.240.1.0/24**

12.  Select **On** for the **Private Google access** section – this allows Citrix Cloud Connectors to communicate with Google Cloud API’s.

13.  Click **Done**

     ![vpc-networks-click-done](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-click-done.png)

14.  Select **Regional** under the **Dynamic routing mode** section

15.  Click **Create** to complete the VPC network creation process

     ![vpc-networks-creation-process](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-creation-process.png)

16.  Validate the VPC network created successfully

     ![vpc-networks-create-successfully](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-create-successfully.png)

#### 3.1.2 Configure outbound connectivity to Citrix Cloud

Citrix Cloud is built based on a suite of API’s which are securely accessed over the Internet. The Citrix Cloud Connector virtual machines must be able to communicate to Citrix Cloud to function. While there are many ways to accomplish this goal, for this guide we’re going to use Google’s [Cloud NAT](https://cloud.google.com/nat/docs/overview) service to facilitate communication between Citrix Cloud and Google Cloud. Cloud NAT allows certain resources without external IP addresses to create outbound connections to the internet. If there is a requirement to establish a connection between the on-premises data center and GCP, then [Cloud Interconnect](https://cloud.google.com/network-connectivity/docs/interconnect) provides a low latency and high availability connection. Cloud Interconnect enables internal IP address communication, which means internal IP addresses are directly accessible from both networks. However, the traffic between on-premises network and the GCP network doesn’t traverse the public internet. You can choose either deploying a dedicated interconnect or using a partner interconnect to provide connectivity between on-premises data center and GCP.

Before configuring Cloud NAT, we must configure the Cloud Router as it is used by Cloud NAT.

> **Note**:
>
> The roles/compute.networkAdmin role gives you permissions to create a NAT gateway on Cloud Router, reserve and assign NAT IP addresses, and specify subnetworks (subnets) whose traffic should use network address translation by the NAT gateway.

##### 3.1.2.1 Step 1: Deploy a Google Cloud Router

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **Hybrid Connectivity**

3.  Click **Cloud Routers**

    ![cloud-routers](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-routers.png)

4.  Click **Create Router**

    ![cloud-routers-create](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-routers-create.png)

5.  Input a unique name for the Cloud Router

6.  Input a description for the Cloud Router

7.  Select the network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

8.  Select the region **us-west1**

9.  Input the Google ASN value. You can use any private ASN value (64512 - 65534, 4200000000 – 4294967294)

10.  Input the BGP peer keepalive interval value **20** (default)

11.  Click **Create**

     ![create-cloud-router](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-cloud-router.png)

12.  Validate the Cloud Router is created successfully

     ![cloud-routers-validated-successfully](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-routers-validated-successfully.png)

##### 3.1.2.2 Step 2: Deploy Google Cloud NAT

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **Network Services**

3.  Click **Cloud NAT**

    ![cloud-nat](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-nat.png)

4.  Click **Get Started**

    ![cloud-nat-get-started](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-nat-get-started.png)

5.  Input a unique name for the Cloud NAT

6.  Select the VPC network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

7.  Select the region **us-west1**

8.  Select the Cloud Router deployed in Step 1

9.  Click **Create**

    ![cloud-nat-gateway](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-nat-gateway.png)

10.  Validate the Cloud NAT is created successfully

     ![cloud-nat-validated](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-nat-validated.png)

##### 3.1.2.3 Configure DNS Services via Google Cloud DNS

Domain Name System (DNS) services are a baseline requirement for any connected network to provide name resolution and a minimum requirement for Microsoft Active Directory functionality. To support a functional Citrix Cloud Resource Location, Citrix Cloud Connectors and VDA’s must locate and communicate with both Active Directory and Citrix Cloud. Google Cloud DNS is a high-performance and resilient global Domain Name System (DNS) service that publishes the domain names to the global DNS. Cloud DNS consists of public zones and private managed DNS zones. A public zone is visible to the public internet, while a private zone is visible only from a more specified Virtual Private Cloud (VPC). You are deploying Cloud DNS to provide name resolution to the **ctx.lab** domain with a private forwarding zone type. The following private DNS servers IP addresses are manually configured as you progress through this document:

-  **10.240.1.2**

-  **10.240.1.3**

### Steps to create DNS zones

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **Network services**

3.  Click **Cloud DNS**

    ![cloud-dns](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-dns.png)

4.  Click **Create Zone**

    ![cloud-dns-zone](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-dns-zone.png)

5.  Select **Private** under the Zone type

6.  Input a unique name for the Zone name: **citrix-on-gcp-zone**

7.  Input a unique DNS name: **ctx.lab**

8.  Input a unique Description: **Forwarding zone to integrate Google DNS with Active Directory**

9.  Click the Options drop down and select **Forward queries to another server**

10.  Under the Networks section, click the drop down and select the network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**.

11.  Enter the first DNS IP Address: **10.240.1.2**

12.  Click **Add Item**

13.  Enter the second DNS IP Address: **10.240.1.3**

14.  Click **Create**

     ![cloud-dns-create-zone](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-dns-create-zone.png)

15.  Validate the DNS Zone created successfully

     ![cloud-dns-zone-validate](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-dns-zone-validate.png)

### 3.2 Configure network layer security using Google Cloud VPC firewall

Every production and/or Internet connected system needs to have multiple layers of security implemented for obvious reasons. This is often based on various sorts of a firewall capability, and on Google Cloud that often starts with the Google Cloud VPC firewall. VPC firewall rules allow an ingress and egress traffic to be either allowed or denied based on a flexible set of defined policies attached to the VPC and virtual machines. The VPC firewall rules are defined at the network level, and connections are allowed or denied on a per instance basis. The VPC firewall provides protection between instances within the same VPC network and between an instance and other VPC networks.

A Citrix Cloud Resource Location needs some basic firewall rules to function. The following table summarizes the protocol, ports, and network tags that are required/allowed for this deployment to be functional. The tasks which follow will guide you through creating firewall rules to match:

![table-protocol-network-tags](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_table-protocol-network-tags.png)

Table 1

We use the target network tags to apply these rules to the virtual machines we are creating later in this guide.

#### 3.2.1.1 Create a firewall rule to allow internal traffic to Active Directory

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **VPC network**

3.  Click **Firewall**

    ![vpn-network-firewall](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpn-network-firewall.png)

4.  Click **Create Firewall Rule** option

    ![firewall-create-rule](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_firewall-create-rule.png)

5.  Input a unique name for the firewall rule: **citrix-allow-internal-dc**

6.  Input a description: **Allow internal traffic between instances**

7.  Select the **Network** created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

8.  Set the Direction of Traffic option to **Ingress**

9.  Set the Allow on match option to **Allow**

10.  Input the network target tags that are configured under the deploying the Google Compute Instance: **dc**

     ![create-a-firewall-rule](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-a-firewall-rule.png)

11.  Set the Source IP ranges to **10.240.1.0 / 24**

12.  Select the **Specified protocol and ports** option

13.  Select the **tcp** checkbox and enter the allowed ports: **88, 135, 389, 445, 464, 636, 3268, 3269, 5985, 9389, 49152-65535**

14.  Select the **udp** checkbox and enter the allowed ports: **88, 123, 389, 464**

15.  Click **Create**

     ![firewall-create-rule-udp-tcp](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_firewall-create-rule-udp-tcp.png)

#### 3.2.1.2 Create a firewall rule to allow forwarding from Google Cloud DNS

1.  Click **Create Firewall Rule** option

    ![create-firewall-rule-allow-dns](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-rule-allow-dns.png)

2.  Input a unique name for the firewall rule: **citrix-allow-external-dns**

3.  Input a description: **Allow forwarding from Google DNS**

4.  Select the **Network** created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

5.  Set the Direction of Traffic option to **Ingress**

6.  Set the Allow on match option to **Allow**

7.  Input the network target tags that will be configured under the deploying the Google Compute Instance: **dns**

    ![create-firewall-rule-dns](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-rule-dns.png)

8.  Set the Source IP ranges to **35.199.192.0/19** and **10.240.1.0 / 24**

9.  Select the **Specified protocol and ports** option

10.  Select the **tcp** checkbox and enter the allowed ports: **53**

11.  Select the udp checkbox and enter the allowed ports: **53**

12.  Click **Create**

     ![create-firewall-ip-range](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-ip-range.png)

#### 3.2.1.3 Create a firewall rule to allow traffic from Cloud Connector to VDA

1.  Click create Firewall Rule option

    ![create-firewall-rule-vda](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-rule-vda.png)

2.  Input a unique name for the firewall rule: **citrix-allow-internal-cc-vda**

3.  Input a description: **Allow traffic from Cloud Connector to VDA**

4.  Select the **Network** created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

5.  Set the Direction of Traffic option to **Ingress**

6.  Set the Allow on match option to **Allow**

7.  Input the network target tags that are configured under the deploying the Google Compute Instance: **vda**

    ![create-a-firewall-rule-vda](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-a-firewall-rule-vda.png)

8.  Set the Source IP ranges to **10.240.1.0 / 24**

9.  Select the **Specified protocol and ports option**

10.  Select the **tcp** checkbox and enter the allowed ports: **80, 443, 1494, 2598, 8008**

11.  Select the **udp** checkbox and enter the allowed ports: **1494, 2598, 16500-16509**

12.  Click **Create**

     ![create-a-firewall-rule-vda-ip-range](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-a-firewall-rule-vda-ip-range.png)

#### 3.2.1.4 Create a firewall rule to allow traffic from VDA to Cloud Connector

1.  Click on **Create Firewall Rule option**

     ![create-firewall-vda-to-cloud](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-vda-to-cloud.png)

2.  Input a unique name for the firewall rule: **citrix-allow-internal-vda-cc**

3.  Input a description: **Allow traffic from VDA to Cloud Connector**

4.  Select the **Network** created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

5.  Set the Direction of Traffic option to **Ingress**

6.  Set the Allow on match option to **Allow**

7.  Input the network target tags that will be configured under the deploying the Google Compute Instance: **cc**

    ![create-firewall-rule-vda-to-cloud](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-rule-vda-to-cloud.png)

8.  Set the Source IP ranges to **10.240.1.0 / 24**

9.  Select the **Specified protocol and ports option**

10.  Select the **tcp** checkbox and enter the allowed ports: **80**

11.  Click **Create**

     ![create-firewall-rule-vda-to-cloud-ip-range](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-firewall-rule-vda-to-cloud-ip-range.png)

If you’ve made it this far, you should now have the foundation of your network on Google Cloud created! Now you can begin building resources on the VPC.

## 4 Create virtual machines

Now that you’ve got a functional Google Cloud project and core network services in place, it’s time to create the virtual machine resources needed to have a functional Citrix Cloud resource location. As described in “[Reference Architecture: Citrix Virtualization on Google Cloud](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html)”, a functional resource location requires the following:

-  Active Directory domain services

-  Citrix Cloud Connectors

-  One or more Citrix VDAs

With a production class deployment of the “[Cloud Forward](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/citrix-google-virtualization.html#the-cloud-forward-design-pattern)” design pattern, these resources are deployed on virtual machine instances which are attached to a VPC, similar to what we created (or identified) earlier in this guide. They’re also deployed in a highly available manner, splitting multiple VM instances between Google Cloud zones in the region where the Citrix Cloud resource location will be deployed.

In this section of the Deployment and Configuration guide, we provide a bit more background detail on Google Cloud resource and service types, followed by the process of creating the virtual machine instances.

### About Google Cloud virtual machine resources

Google Cloud provides reliable, high performance block storage for use by virtual machine instances. Block storage is presented to virtual machines as [Persistent Disks](https://cloud.google.com/persistent-disk). The persistent disks, available either as standard hard drive or solid-state drives (SSD), are durable network storage devices that your instance can access as though they were physical disks in a desktop or a server. Google Compute Engine instances by default have a single root persistent disk with an operating system. Extra disks can be added later for instances where applications require more local storage. The choices for disk expansion are Standard or SSD persistent disks, Local SSDs, and Cloud Storage Buckets. Local SSD is not recommended for Citrix Virtual Apps deployment, as the data stored on the local SSD persists only until you stop or delete the instance. The persistent disks are located independently from the virtual machine instances, so there is an option to detach or move persistent disks to retain data even after the VM instance is deleted. The persistent disks performance scales automatically with size -- this allows you to resize the persistent disks or add more persistent disks to an instance to meet performance to storage space requirements. The persistent disk has no per I/O cost, so there is no need to estimate monthly I/O to calculate the budget for what you spend on disks. SSD persistent disks have a read IOPS of 40,000 and write of 30,000 per instance compared to Standard persistent disks which only have IOPS of 3,000 for read and 15,000 for write. The SSD persistent disk is recommended for deploying Citrix Virtual Apps workloads. When estimating storage capacity, remember that Virtual Apps and Desktops deployments have two storage needs: (1) storage for Virtual Apps servers and applications and (2) storage for user profiles. Deploying a Virtual Apps server master image requires approximately 50 GB of storage, and this can vary based on installed applications and the Windows Server operating system version. For example, the Windows Server 2016 OS boot size is 50 GB while the Windows Server 2012 R2 is 32 GB. Citrix recommends creating a new Virtual Apps master image to minimize the required capacity instead of migrating an existing on-premise image.

On Google Cloud, the service that runs virtual machine instances is referred to as Google Compute Engine. Compute engine offers predefined virtual machines configurations for every need, from small micro instances to instances with 96 vCPUs or 624 GB of memory, in standard, high-memory, and high CPU configurations. The following compute resources are relevant to any Virtual Apps deployment on GCP:

-  Compute Optimized – C2

-  Predefined Machine Type – has a predefined number of vCPUs and memory and is charged at a set price described in the [pricing](https://cloud.google.com/products/calculator?skip_cache=true) page.

-  Custom Machine Type – provide the flexibility to configure the vCPU and memory for specific needs and potentially reducing the cost.

When deploying Windows desktop operating systems via Citrix Virtual Desktops in a non-Microsoft public cloud, [Microsoft licensing](https://cloud.google.com/compute/docs/instances/windows/ms-licensing) requires that the virtual machine instances are not deployed on shared infrastructure. To accomplish this on GCP, [Sole Tenant Nodes](https://cloud.google.com/compute/docs/nodes/sole-tenant-nodes) (“STN”) or Google Cloud VMware Engine (GCVE) are required. STN and GCVE are beyond the scope of this deployment guide.

Using the Microsoft License Mobility enables the deployment of Windows Server applications (Citrix Virtual Apps), such as Remote Desktop Services (RDS), in GCP while using your existing Microsoft licenses. It is recommended to review your Microsoft licensing agreements with Microsoft prior to starting this deployment. Microsoft Windows Server instances require an internet network connection to activate with the GCP KMS host kms.windows.googlecloud.com. The standard grace period for Windows Server instances to register with the KMS host is 30 days. After 30 days, the instances will cease functioning. Alternatively, you can also bring your own Windows KMS licensing into GCP and host the required licenses. In this guide, we will not be deploying a Microsoft RDS server using the 30-days grace period for validation.

#### 4.1 Create virtual machine instances

As discussed previously, the Cloud Forward design pattern requires three different types of virtual machine instances to be deployed (Active Directory, Citrix Cloud Connector, Citrix VDA). Using the details in the table  repeat the steps outlined to create and start each virtual machine, then generate and store credentials to use to log in to each virtual machine instance the first time.

![tabel2-virtual-machine-instances](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_tabel2-virtual-machine-instances.png)

Table 2

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **Compute Engine**

3.  Click **VM Instances**

    ![vm-instances](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-instances.png)

4.  Click **Create Instance**

    ![vm-create-instances](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-create-instances.png)

5.  Input a name: **dc1**

6.  Select the region: **us-west1**

7.  Select the zone: **us-west1-a**

8.  Select the **General-purpose** tab

9.  Select **N2 series**

10.  Select **n2-standard-2** (follow the corresponding compute sizing listed for each virtual machine in the Table 2)

     ![vm-dc1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-dc1.png)

11.  At Boot disk click **Change**

     ![vm-boot-disk](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-boot-disk.png)

12.  Click **Public Image** tab

13.  Select **Windows Server** operating system

14.  Select **Windows Server 2019 Datacenter** (for this deployment we are using the evaluation license)

15.  Select **Balanced persistent disk**

16.  Provide the required size in GB: **50**

17.  Click **Select**

     ![vm-public-images](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-public-images.png)

18.  Expand the **management, security, disks, networking, sole tenancy** blade

     ![vm-blade](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-blade.png)

19.  Expand **Networking** section and input the corresponding virtual machine network tags: **dc dns**

20.  Input a hostname as listed in the Table 2: dc1.ctx.lab

     ![dc-dns-networking](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_dc-dns-networking.png)

21.  Select the network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

22.  The Subnetwork should auto populate

23.  Select **Ephemeral IP Address (Custom)** under the Primary internal IP section

24.  Input the IP Address **10.240.1.2** under the Custom ephemeral IP address

        > **Note**:
        >
        > The Server VDA master image requires the IP address to be assigned automatically. To allow the Server VDA master image to obtain IP address automatically select Ephemeral (Automatic).

25.  Set the External IP to **None**

26.  Click **Done**

     ![dc-dns-network-interface](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_dc-dns-network-interface.png)

27.  Click **Create**

     ![dc-dns-network-interface-create](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_dc-dns-network-interface-create.png)

28.  Once the machine has been created and powered on, the next step is to create a default username and password.

        > **Note**:
        >
        > The ‘Set Windows password’ function in Google Cloud sets/resets a strong password using the username you specify. If the account exists, it resets the password. If it doesn’t, Google Cloud creates the user in the local security database of the Windows instance, then creates the password. The user Google Cloud creates (or updates) is a local administrator on the instance.

        There are multiple ways to access the ‘Set Windows password function’ – here’s one of them, and it starts by clicking the VM instance to view instance details.

        ![vm-instances-view-details](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-instances-view-details.png)

29.  Under the VM instance details section, click on **Set Windows password**

     ![vm-instances-details](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-instances-details.png)

30.  Input the username **admin**

31.  Click **Set**

        > **Note**
        >
        > If during creation of the password you are prompted an error “***Windows password could not be set. Try again. If you just created this instance, allow 10 minutes for it to be ready.***”, we recommend allowing the suggested time before attempting to create the password.

        ![vm-set-new-password](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-set-new-password.png)

32.  By default, a unique new password is auto generated by Google Cloud and cannot be changed from the console. If a custom password is required, you need perform the password change within the Windows operating system.

33.  You can either copy it down manually or Click on the copy icon. Store the password securely as it will be required to log in to the virtual machine console in the upcoming section.

34.  Click **Close**

     ![vm-set-new-windows-password](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-set-new-windows-password.png)

     After following steps 1 through 35 to build each VM as indicated in the table at the beginning of this section, the results should be similar to the image:

     ![vm-instances-tabel-result](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-instances-tabel-result.png)

35.  If you are unable to view the Network tags column, click the column display option icon

36.  Select **Network tags** from the list

37.  Click OK

     ![vm-network-tags](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-network-tags.png)

Once you’ve created the virtual machine instances that will be used to build the Citrix Cloud resource location, you’re now ready to connect to and configure each instance.

## 5 Configure access to virtual machine consoles

Now that we’ve gone through the process of creating the Windows virtual machine instances, to complete the build, you need a way to remotely access the consoles of these virtual machines to configure them. Google Cloud handles remote console access by relying upon network connectivity to the virtual machine instance, and a ‘remote console interface’ running inside the virtual machine and accessed across the network. For Windows virtual machines, that means using an RDP client to connect to the RDP listener running inside the instances. For non-Windows machines, this is usually handled by SSH.

The tools and techniques you use to gain access to the VM consoles can differ based on the operating system and network location of your workstation. They can also differ based on how your organization handles security. We’ll outline a couple common techniques and caveats in a moment, but the goal of this section is simple in concept: *ensuring you’ve got the ability to establish remote console connections to the virtual machines in your Google Cloud project.*

Some common techniques for establishing remote console access include the following:

1.  Using an RDP client to establish a direct connection between an administrative workstation and VM’s on the same network

2.  Using public IP addresses on one or more VM’s, and establishing RDP connections to the public addresses from an administrative workstation via the Internet

3.  Establishing a connection to a ‘jump box’ or ‘bastion host’, then accessing the VM within the Google Cloud project using the preferred RDP client. Administrators often connect to the jump box via an external IP address.

4.  Using Google Cloud’s Identity Aware Proxy (IPA) TCP forwarding feature, plus a tool like IAP Desktop

If your workstation is already on the same network as the VM’s in your Google Cloud project, you should be able to simply connect to them via IP address, assuming you’ve allowed RDP access via your firewall rules. If your administrative workstation is NOT on the same network, then you can consider using a jump box with an external IP address assigned. If you do this, however, make sure you restrict access to the RDP listener (TCP 3389) to only allow access from the public IP address of your administrative workstation.

The most secure way to provide remote console access is to use Google Cloud’s [Identity Aware Proxy](https://cloud.google.com/iap/docs/concepts-overview) TCP forwarding feature, plus IAP Desktop. Google Cloud’s Identity Aware Proxy (IAP) TCP forwarding feature allows you to control who can access the administrative interfaces such as SSH and RDP to the VM’s in your project via the public Internet. IAP prevents these services from being directly exposed to the Internet. IAP also allows you to control WHO can access these services based on Google Cloud IAM roles as IAP performs authentication and authorization prior to allowing access. IAP Desktop is a Windows only open-source tool that puts a user-friendly UI on top of IAP and your RDP client, making remote console access simple and effective.

This deployment guide uses the IAP service and IAP Desktop app to securely access the virtual machines for configuration. You can still use the IAP service with a non-Windows endpoint using the Google Cloud SDK and [gcloud command](https://cloud.google.com/iap/docs/using-tcp-forwarding#gcloud), but that is outside of the scope of this guide.

Configuring Identity Aware Proxy is a two-step process: the first step is to configure the Firewall to allow ingress TCP traffic, and the second step is to configure the Identity Aware Proxy.

### Configure the Google Cloud firewall to allow ingress TCP traffic

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **VPC network**

3.  Click **Firewall**

     ![vpc-networks-firewall](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-firewall.png)

4.  Click **Create Firewall Rule**

     ![vpc-networks-firewall-rule](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vpc-networks-firewall-rule.png)

5.  Input a unique name for the Firewall rule: **allow-iap-access**

6.  Input a description for the Firewall rule: **Allow IAP Access**

7.  Select the VPC network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

8.  Select **Ingress** traffic

     ![vm-ingress-create-firewall-rule](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-ingress-create-firewall-rule.png)

9.  In the Targets field, select **All instances in the network**

10.  Set the Source IP ranges to **35.235.240.0/20**

11.  Select **Specified protocols and ports**

12.  Select the **tcp** checkbox

13.  Click **Create**

     ![vm-ingress-ip-ranges](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-ingress-ip-ranges.png)

14.  Validate the Firewall rule is created

     ![vm-ingress-firewall-validation](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_vm-ingress-firewall-validation.png)

#### 5.1 Enable and Configure the Identity Aware Proxy

1.  Click the hamburger icon located in the top left-hand corner of the Google Console

2.  Navigate to **IAM & Admin**

3.  Click **Identity-Aware Proxy**

    ![identity-aware-proxy](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_identity-aware-proxy.png)

4.  If prompted, click **Enable API**

    ![identity-aware-proxy-enable-api](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_identity-aware-proxy-enable-api.png)

5.  Click **Go to Identity-Aware Proxy**

    ![identity-aware-proxy-go-to](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_identity-aware-proxy-go-to.png)

    You may get this screen after clicking:

6.  Click **SSH and TCP Resources tab**

7.  To update member permissions on resources, select all the VM instances created earlier.

8.  Click **Add Principal**

    ![identity-aware-proxy-add-principal](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_identity-aware-proxy-add-principal.png)

9.  To grant users, groups, or service accounts access to the resources, in the New principals field, specify their email addresses. If you are the only user testing this feature, you can enter your own email address.

10.  To grant the members access to the resources through Cloud IAP's TCP forwarding feature, in the **Role** drop-down list, select **Cloud IAP**”

11.  Select **IAP-secured Tunnel User**

     ![iap-secured-tunnel-user](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-secured-tunnel-user.png)

12.  Click **Save**

     ![iap-add-principals](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-add-principals.png)

### 5.2 Install, configure, and use IAP Desktop for remote console access

Once the Identity Aware Proxy has been enabled, the next step is to connect to the deployed virtual machine instances using IAP Desktop, a Microsoft Windows only open-source tool available on [GitHub](https://github.com/GoogleCloudPlatform/iap-desktop/). Once you have downloaded and installed the IAP Desktop, proceed to launch it and follow the steps to configure the IAP Desktop.

1.  Click **Sign in with Google**

    ![iap-desktop](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-desktop.png)

2.  Select the Google account associated with Google Cloud Platform. Depending on your account configuration, you may must provide Username, Password, and a token.

    ![iap-choose-account](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-choose-account.png)

3.  Upon a successful authentication, you are prompted with the following permission window. Select the **See, edit, configure and delete your Google Cloud Platform data**

4.  Click “Continue”

    ![iap-desktop-access](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-desktop-access.png)

5.  Select the **Google Cloud Project**

6.  Click **Add Project**

    ![iap-add-project](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-add-project.png)

7.  All the machines created under the **Deploying Google Compute Instance** section earlier are enumerated:

    ![iap-project-explorer](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-project-explorer.png)

8.  To log in to the virtual machine instance, right Click on the target virtual machine

9.  Select **Connect as user…**

    ![iap-connect-as-user](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-connect-as-user.png)

10.  Click **User a different account**

11.  Enter the username “**admin**”

12.  Enter the unique password previously auto generated for the machine you are connecting

13.  Click **OK**

     ![iap-enter-your-credentials](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-enter-your-credentials.png)

14.  Upon successful authentication, you should be able to log in to the virtual machine

     ![iap-remote-desktop-ssh](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_iap-remote-desktop-ssh.png)

Once you’ve completed this goal (using IAP, a jump box, or some other technique) you have a functional method for connecting to the remote consoles of your virtual machines. In the next section, you’ll use this method to configure the virtual machines into a functional Citrix Cloud resource location.

## 6 Deploy Active Directory into Google Cloud

As we discuss extensively in the Reference Architecture, Microsoft Active Directory Domain Services (ADDS) is a core infrastructure component required to support a Citrix virtualization system. Microsoft Active Directory enables you to manage authentication and authorization for the Citrix Cloud workloads. For an optimal user experience, Citrix leading practices strongly recommend deploying Active Directory in the same region as your Citrix Cloud Connectors and VDA’s.

Microsoft Active Directory can be deployed to Google Cloud in multiple ways, including:

-  Deploying a new Microsoft Active Directory on Google Cloud

-  Extending an existing [Microsoft Active Directory to Google Cloud](https://cloud.google.com/managed-microsoft-ad/docs/best-practices)

-  Using [Google Managed Service for Microsoft Active Directory](https://cloud.google.com/managed-microsoft-ad), a highly available, hardened service running on Google Cloud Platform

This guide deploys a new Microsoft Active Directory on Google Cloud and integrates this new AD instance with Citrix Cloud.

### 6.1 Install the Active Directory Domain Services Role

> **Note:**
>
> Step 1 through 21 is required to be performed on both **DC1** and **DC2** virtual machines.

1.  log in to **DC1** virtual machine using the local administrator credentials generated in the previous step

     ![log in to DC1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_log in-to-dc1.png)

2.  Click **Start** menu

3.  Click **Server Manager**

     ![Click server manager](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-server-manager.png)

4.  Click **Add roles and features**

     ![Add roles and features](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-add-roles-and-features.png)

5.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_before-you-begin.png)

6.  Accept the default option, click **Next**

     ![click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-installation-type.png)

7.  Select the destination server on which the role is installed. Confirm the IP address displayed is that of the selected server. Else, close the Server Manager and retry. Click Next.

     ![Select destination server](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-destination-server.png)

8.  The basic requirement to promote this server into a domain controller is Active Directory Domain Services.

     ![Select server tools](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-server-tools.png)

9.  When prompted, click **Add Features**

     ![Add features](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_add-features-active-directory-domain-services.png)

10.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-server-tools.png)

11.  The features for this role are ready to be installed. The basic features required for this service are selected by default. Click **Next**.

     ![Select features](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-features-click-next.png)

12.  Click **Next** to continue

     ![Active Directory Domain Services](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_active-directory-domain-srevice.png)

13.  Click **Install**

     > **note**
     >
     >It is recommended to select the "Restart the destination server automatically if required" option.

     ![Click install](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_confirm-installation-selection.png)

14.  At this point the installation starts

     ![Installation progress](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_installation-progress.png)

15.  Upon successful installation, the installation process displays **Configuration required. Installation succeeded on dc1**

16.  Click Close to exit out of the **Add Roles and Features** installation process

     ![Click close](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-close-to-exit-out-the-add-roles-and-features.png)

17.  Before starting the process of promoting the server to Domain Controller, you must configure a complex local administrator password to meet the password complexity requirement.

18.  Right click the Windows menu and select **Windows PowerShell (Admin)**

     ![Select Windows PowerShell](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-windows-powershell.png)

19.  User Account Control prompts you for permission, Click **Yes**

     ![User account control prompt](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_user-account-control-prompt.png)

20.  Input the following command in the Windows PowerShell console, press “Enter” to execute the command:
     **net user Administrator P@$$w0rd!@# /passwordreq:yes**

21.  Click the close Windows icon to exit out of the Windows PowerShell console

     ![Windows PowerShell console](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_windows-powershell-console.png)

### 6.2 Configure the Primary Domain Controller

1.  Click the **Notification** icon

2.  Click **Promote this server to a domain controller**

     ![Promote this server to a domain controller](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_promote-server-domain-controller.png)

3.  Select **Add a new forest**

4.  Input a unique Root domain name: **ctx.lab**

5.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_deployment-configuration.png)

6.  Specify a new Directory Services Restore Mode (DSRM) password that meets Microsoft Windows password complexity requirement: **P@$$w0rd!@#**

     > **note**
     >
     > DSRM mode helps gain access to the Active Directory environment if all domain administrator accounts lose access or if the Domain Controller fails.

7.  Repeat the password: P@$$w0rd!@#

8.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_domain-controller-option.png)

9.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_dns-option.png)

10.  The NetBIOS domain name automatically populates, you can change the name if necessary

11.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_additional-options.png)

12.  Accept the default folder path and click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_paths.png)

13.  Review your selections and click **Next**

     ![Review options](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_review-options.png)

14.  The Domain Controller promotion wizard performs a prerequisite validation and upon successful it displays **All prerequisite checks passed successfully**. Once it is done, click **Install**

15.  Click **Install** and this completes the Domain Controller promotion process.

     ![Click install](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_prerequistites-check.png)

### 6.3 Configure the Secondary Domain Controller

> **note**
>
> **Complete Step 1 through 21** listed under the Installing the Active Directory Domain Services Role section before proceeding with configuring the Secondary Domain Controller. Once completed, proceed to log in to **DC2** virtual machine using the generated local administrator credentials.

1.  Click the **Notification** icon

2.  Click **Promote this server to a domain controller**

     ![Promote this server to a domain controller](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_promote-server-domain-controller-dc2.png)

3.  Select option **Add a domain controller to an existing domain**

4.  Enter the Primary Domain Controller fully qualified domain name (FQDN): **dc1.ctx.lab**

5.  Click **Select**

     ![Click select](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_deployment-configuration-dc2.png)

6.  Input the Domain Administrator username: **ctx\administrator**

7.  Input the Domain Administrator password: **P@$$w0rd!@#**

8.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_credentials-for-deployment-operation.png)

9.  Another window pops up. Select the **ctx.lab** domain

10.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-ctx-lab.png)

11.  Back at the Deployment Configuration page, click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_deployment-configuration-click-next.png)

12.  Specify a new Directory Services Restore Mode (DSRM) password that meets Microsoft Windows password complexity requirement: **P@$$w0rd!@#**

     > **Note**
     >
     > DSRM mode helps gain access to the Active Directory environment if all domain administrator accounts lose access or if the Domain Controller fails.

13.  Repeat the password: **P@$$w0rd!@#**

14.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_domain-controller-option-click-next.png)

15.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_dns-option-click-next.png)

16.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_additional-options-click-next.png)

17.  Accept the default folder path and click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_default-folder-path-click-next.png)

18.  Review your selection and click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_review-options-click-next.png)

19.  The Domain Controller promotion wizard performs a prerequisite validation, and upon successful validation, it displays “All prerequisite checks passed successfully”. Once it is done, click **Install**

     ![Click install](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_prerequistites-validation-click-install.png)

20.  Click **Close** to restart the Secondary Domain Controller to apply the configuration

     ![Cick close](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_active-directory-domain-configuration-click-close.png)

### 6.4 Create a new test User Account

A domain user account is recommended to be used to assign and launch any resources published by Citrix infrastructure. In this step, we create a ‘regular’ account to use to test the system in an goal we cover later in this document.

1.  Log in to DC1 virtual machine using the following domain credentials:

     Username: ctx\administrator

     Password: P@$$w0rd!@#

     ![Log in to DC1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_login-to-dc1-virtual-machine.png)

2.  Click Tools menu located in the top right-hand corner of the Server Manager console

3.  Select Active Directory Users and Computers

     ![Select Active Directory Users and Computers](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-active-directory-users-computers.png)

4.  Expand the ctx.lab domain

5.  Select **Users**

6.  Select **New**

7.  Select and click **User**

     ![Click user](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-and-click-user.png)

8.  Input First Name: **user**

9.  Input Last Name: **one**

10.  Input User logon name: **user1**

11.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_user-details-click-next.png)

12.  Input password: P@$$w0rd!@#

13.  Repeat the password: P@$$w0rd!@#

14.  Uncheck **User must change password at next logon**

15.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_user-login-click-next.png)

16.  Review the user details and click **Finish**

     ![Click finish](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_user-creation-click-finish.png)

17.  Confirm that the user creation is successful

     ![Successfull](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_user-creation-successful.png)

### 6.5 Add extra VM’s to Active Directory

Before we can begin configuring the other VM’s in our environment, they need to be joined to Active Directory. Repeat the steps on the other Windows VM instances in your Google Cloud project to prepare them for further configuration.

> **Note**
>
> **Steps 1 – 13** are required to be performed on both the **Citrix Cloud Connector** servers and the **Server VDA** master image machine. Assuming you followed our naming guidance earlier in this document, that means you are repeating this process on cc1, cc2, and mcs instances.

1.  Log in to **CC1** virtual machine using the local administrator credentials generated in the previous step

     ![Log in to CC1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_login-to-cc1-vm.png)

2.  Launch Server Manager and click **Local Server**

3.  Click **Workgroup**

     ![Click workgroup](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-workgroup.png)

4.  Click **Change**

     ![Click change](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-change.png)

5.  Select **Domain** option and input the domain name: **ctx.lab**

6.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-domain-click-ok.png)

7.  Input the domain administrator username: **ctx\administrator**

8.  Input the domain administrator password: **P@$$w0rd!@#**

9.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_domain-change-click-ok.png)

10.  Upon successful authentication, you receive a messaging welcoming you to the domain. Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_welcome-message-click-ok.png)

11.  Click **OK** again to restart the computer

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_restart-computer-clcik-ok.png)

12.  Click **Close**

     ![Click close](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_system-properties-click-close.png)

13.  Click **Restart Now**

     ![Click restart](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-restart-now.png)

Repeat **step 1 through 13** for the second Cloud Connector (CC2) and the Server VDA Master Image.

## 7 Create and initialize Citrix Cloud resource location

The goal for this section of the guide is to walk the reader through the process of accessing the Citrix Cloud Console, integrating it with your Google Cloud project and resources, and preparing to publish applications and desktops.

Citrix Cloud is a platform that hosts and administers Citrix services. It connects to your resources through [connectors](https://docs.citrix.com/en-us/citrix-cloud/citrix-cloud-resource-locations/resource-locations.html) on any cloud or infrastructure you choose (on-premises, public cloud, private cloud, or hybrid cloud). It allows you to create, manage, and deploy workspaces with apps and data to your end-users from a single console.

This deployment guide requires you to have a Citrix Cloud account with an active CVADS subscription. After [signing up for Citrix Cloud](https://docs.citrix.com/en-us/citrix-cloud/overview/signing-up-for-citrix-cloud/signing-up-for-citrix-cloud.html), you can request service trials for Citrix Virtual Apps and Desktops service within the console. You need both a Citrix Cloud account and active subscription before you are able to successfully complete this goal.

### Citrix Cloud Resource Location

Resource locations contain the resources required to deliver cloud services to your subscribers. You manage these resources from the Citrix Cloud console. To create a resource location, install at least two Cloud Connectors in your domain. The resource location is wherever the resources reside, whether that’s a public or private cloud. In the case of this deployment guide, the resources reside on Google Cloud Platform. The choice of location can be impacted by proximity to subscribers or proximity to data. This guide deploys a single Server VDA resource location in Google Cloud Platform region us-west1 with one Citrix Cloud Connectors installed each  in us-west1-a and us-west1-b zone for high availability.

### 7.1 Create Citrix Cloud resource location

The goal of this first task is to create the resource location we are installing Citrix Cloud Connectors into. These tasks can be completed from any web browser, though you want to consider performing them from the remote console of one of your Cloud Connector VM instances. While not strictly necessary, this will save you some time as you’ll be teed up for installing the Citrix Cloud Connector software in the next task.

1.  Navigate to [https://cloud.citrix.com](https://cloud.citrix.com)

2.  Log in to the Citrix Cloud console using your username

3.  Input your password

4.  Click **Sign In**

     ![Sign in](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_resource-location-sign-in.png)

5.  Enter the verification code provided by your authenticator app

6.  Click **Verify**

     ![Click verify](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_resource-location-verify.png)

7.  Select your Citrix Cloud account when a **Select a Customer** screen is presented

     ![Select customer](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_resource-location-select-customer.png)

8.  Click **Edit or Add New** under the Resource Location section.

     > **Note:**
     >
     > A single Resource Location is created by default.

     ![Edit or Add New](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_resource-location-edit-add-new.png)

9.  Click the three horizontal ellipsis on the top left corner of the My Resource Location

10.  Select **Manage Resource Location**

     ![Manage resource location](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_manage-resource-location.png)

11.  Input a unique name for the resource location: **Server VDA – Google US-West1**

12.  Click **Confirm**

     > **Note:**
     >
     > By default, Citrix Cloud installs updates on each connector, one at a time, when these updates become available. To ensure that updates are installed in a timely manner without unduly affecting your users’ Citrix Cloud experience, you can schedule these updates for a preferred schedule.

     ![Manage resource location](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-confirm-manage-resource.png)

13.  The result should be similar to the image:

     ![Manage resource location result](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_manage-resource-location-result.png)

#### Citrix Cloud Connector

The Citrix Cloud Connector is a Citrix component that serves as a channel for communication between Citrix Cloud and your resource locations, enabling cloud management without requiring any complex networking or infrastructure configuration. The Cloud Connector authenticates and encrypts all communication between Citrix Cloud and the resource locations. All Cloud Connector outbound only communication is initiated using the standard HTTPS port (443) and the TCP protocol. For continuous availability and to manage load Citrix recommends at least two Cloud Connectors in each resource location. Since each Cloud Connector is stateless, the load can be automatically distributed across all available Cloud Connectors.

### 7.2 Install Citrix Cloud Connectors

The next step is to install the Citrix Cloud Connector software on your cloud connector VM instances. Installing Cloud Connectors into a resource location effectively ‘initialize’ the resource location and make it usable by Citrix Cloud and its related services. Complete the following steps on each VM instance you intend to use as Cloud Connectors. You want to log into each remote console using a domain user who’s a local Administrator on the VM instance. If you’ve been following this guide, you can use the domain credential to log in to each of the Cloud Connector virtual machines:

Username: **ctx\administrator**

Password: **P@$$w0rd!@#**

> **Note:**
>
> **Step 1 through 19** is required to be performed on both the **Citrix Cloud Connector** VM’s – CC1 and CC2.

1.  Log in to **CC1** virtual machine using the domain credentials provided

     ![Log in to cc1](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_login-to-cc1.png)

2.  Navigate to [https://cloud.citrix.com](https://cloud.citrix.com) and log in to the Citrix Cloud console using your username

3.  Input your password

4.  Click **Sign In**

     ![Sign in](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_citrix-cloud-signin.png)

5.  Enter the verification code provided by your authenticator app

6.  Click **Verify**

     ![Verify](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_citrix-cloud-verify.png)

7.  Select your Citrix Cloud account when a **Select a Customer** screen is presented

     ![Select customer](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_citrix-cloud-select-customer.png)

8.  Click **Edit or Add New** under the Resource Location section.

     > **Note:**
     >
     > A single Resource Location is created by default.

     ![dit or Add New](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_citrix-cloud-edt-or-add-location.png)

9.  Click **Cloud Connectors**

     ![Cloud Connectors](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-cloud-connectors.png)

10.  Click **Download**

     ![Download](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-connectors-download.png)

11.  Click **Save**

     ![Save](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-cloud-connectors-save.png)

12.  Click **Open Folder**

     ![Open folder](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-cloud-connectors-opne-folder.png)

13.  Right click the executable and select **Run as administrator**

     ![Run as administrator](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-connectors-run-administrator.png)

14.  Click **Sign In**, provide username and password if prompted

     ![Sign in](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-connectors-signin.png)

15.  Click the drop-down list and select the corresponding Citrix Cloud Organization ID

16.  Click **Install**

     ![Install](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-connectors-install.png)

17.  The installation begins and displays the progress

     ![Installing](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_displays-the-progress.png)

18.  Once the installation completes, the installer performs a final connectivity check to verify communication between the Cloud Connector and Citrix Cloud.

     ![installation completes](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_testing-service-connectivity.png)

19.  The following screen is displayed once the connectivity test is successful. Click **Close**

     ![connectivity test is successful](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_conectivity-test-successfull.png)

Repeat step **1 through 19** for the second Cloud Connector (CC2).

### 7.3 Verify Citrix Cloud resource location

Once you’ve installed Citrix Cloud Connectors into your Citrix Cloud tenant, you should be ready to begin configuring the Virtual Apps and Desktops service. You can verify readiness to begin configuring the CVAD service by checking the following:

Navigate to the appropriate resource location, click into it. You should see your Cloud Connectors, they should be listed in the correct resource location, and they should show a green status:

![Verify](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_cloud-connectors-verify.png)

Now that you’ve successfully created and initialized your Citrix Cloud resource location on Google Cloud, it’s time to configure the Virtual Apps and Desktops service.

## 8 Configure Citrix Virtual Apps and Desktops service

The Citrix Virtual Apps and Desktops service (formerly known as the XenApp and XenDesktop service) offers a virtual app and desktop solution, provided as a cloud service that gives IT control of virtual machines, applications, and security while providing anywhere access for any device. End users can use applications and desktops independently of the device’s operating system and interface. Citrix Virtual Apps and Desktops delivers secure virtual apps and desktops to any device through any public cloud and / or on-premises hypervisors such as Citrix Hypervisor, Microsoft Hyper-V, and VMware vSphere. Citrix Virtual Apps and Desktops service is typically deployed in a single site with a one or more resource locations, which is considered as a zone. Citrix manages the user access and management services and components in Citrix Cloud. The applications and desktops that are being delivered to users reside on machines in one or more resource locations.

The Citrix Virtual Apps and Desktops service (commonly referred to as “CVADS” for short) needs a bit of preparation to start serving users. The goal of this section is to configure CVADS to establish a connection with Google Cloud and deploy published applications and desktops for users.

### 8.1 Access the Virtual Apps and Desktops Service console

1.  Navigate to [https://cloud.citrix.com](https://cloud.citrix.com) and log in to the Citrix Cloud console using your **username** and **password**. Provide the **token** when prompted

2.  Click the hamburger menu located on the top right section

    ![“IAP-secured Web App User](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_hamburger-menu.png)

3.  Select **My Services**

4.  Click **Virtual Apps and Desktops**

    ![Virtual Apps and Desktops](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-virtual-apps-desktops.png)

5.  Click **Manage** to access the Virtual Apps and Desktops Service console

6.  You can now view the Virtual Apps and Desktops Service options

    ![view the Virtual Apps and Desktops Service](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_view-virtualapps-desktops-service.png )

### 8.2 Configure the Hosting Connection and Resources

For Citrix Cloud to create and manage app and desktop resources on Google Cloud, we need to configure a hosting connection and define resources for our Google Cloud project. Using the JSON key for the service account we created earlier, this task is completed from the CVADS/Manage console:

1.  Click Hosting

    ![Click Hosting](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-hosting.png)

2.  Click Add Connection and Resources

    ![Click Add Connection](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-add-connection-resources.png)

3.  Click the drop-down list and select **Google Cloud Platform**

4.  Click **Import Key** and select the JSON key you generated under the [Generating JSON Key](#_Generating_JSON_Keys) section.

5.  Service Account ID field should auto-populate once you import the key

6.  Select the zone name: **Server VDA – Google US-West1**

7.  Input a unique connection name: **GCP-Connection**

8.  Select **Citrix provisioning tools (Machine Creation Services or Citrix Provisioning)**

9.  Click **Next**

    ![Click Next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next.png)

10.  The project field should default to your Google Project. If not, click the drop-down list and select the corresponding Google Project

11.  Select the region: **us-west1**

12.  Click **Next**

     ![Click Next12](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next12.png)

13.  Input a unique name for the network resource being created: **GCPNetwork**

14.  Select the Virtual Network created in the [Virtual Private Cloud](#_Steps_to_create) section: **citrixcloudnetwork**

15.  Confirm the subnet

16.  Click **Next**

     ![Click Next16](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next16.png)

17.  Accept the default option and click **Next**

     ![Click Next17](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next17.png)

18.  Review and confirm the connection parameters

19.  Click **Finish**

     ![Click finish](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-finish.png)

      The result should be similar to the image:

     ![Click result](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_rusult.png)

Now that you’ve got a functional hosting connection and resources, you’re ready to begin preparing the virtual machines that actually execute the workloads for your users – the Citrix VDA’s.

### 8.3 Prepare the ‘master image’ for your first catalog of Citrix VDA’s

For this Deployment and Configuration Guide, we’re going to build your first non-persistent multi session Server OS. [Image Management Reference Architecture](https://docs.citrix.com/en-us/tech-zone/design/reference-architectures/image-management.html) document provides an overview of product functionality and design architecture for an image management environment to ensure efficient delivery of application and desktop workloads for an organization. Before the machine catalog can be provisioned, the Virtual Delivery Agent (VDA) software must be installed on the master image to allow the machine and the resources it is hosting to be made available to users. Also, the VDA enables the machine to register with the Citrix Cloud Connector.

1.  Log in to MCS virtual machine using the following domain credentials

    Username: **ctx\administrator**

    Password: **P@$$w0rd!@#**

     ![connect as user](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_connect-as-user.png)

2.  Navigate to [https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/product-software/citrix-virtual-apps-and-desktops-2106.html](https://www.citrix.com/downloads/citrix-virtual-apps-and-desktops/product-software/citrix-virtual-apps-and-desktops-2106.html)

     ![Navigate to](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_navigate-to.png)

3.  You need a Citrix account to log in and download the Multi-session OS Virtual Delivery Agent. If you do not have a Citrix account, you can choose **Create Citrix Account**

     ![Create Citrix Account](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-citrix-account.png)

4.  Upon successful log in, download the Multi-session OS Virtual Delivery Agent

     ![download Virtual Delivery Agent](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_download-virtual-delivery-agent.png)

5.  Click **Save**

     ![click save](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-save.png)

6.  Click **Open Folder**

     ![click open folder](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-open-folder.png)

7.  Right click the VDA server software installer and select **Run as administrator**

     ![Run administrator](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_run-administrator.png)

8.  Select **Create a master MCS image**

9.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3.png)

10.  Accept the default options and click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_accept-default-options-click-next.png)

11.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3-11.png)

12.  Input the first Cloud Connector fully qualified domain name: **cc1.ctx.lab**

13.  Click **Test connection** to validate the communication between the VDA and Cloud Connector and ensure that a green check mark is displayed

14.  Click **Add**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-add-8.3-14.png)

15.  Input the second Cloud Connector fully qualified domain name: **cc2.ctx.lab**

16.  Click **Test connection** to validate the communication between the VDA and Cloud Connector and ensure a green check mark is displayed

17.  Click **Add**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-add-8.3-17.png)

18.  Confirm both the Cloud Connectors are added successfully

19.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3-19.png)

20.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3-20.png)

21.  Accept the default firewall setting. Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3-21.png)

22.  Review the summary screen. Click **Install**

     > **Note:**
     >
     > The installer requires multiple reboots

     ![Click install](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-install.png)

23.  Uncheck the **Collect diagnostic information** option

24.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.3-24.png)

25.  Click **Finish**. The installer reboot the machine to complete the installation process. Proceed to shut down the machine.

     ![Click finish](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-finish-8.3-25.png)

### 8.4 Create Machine Catalogs

1.  Click **Machine Catalogs**

     ![Click machine catalogs](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-machine-catalogs.png)

2.  Click **Create Machine Catalog**

     ![Create machine catalogs](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_createmachine-catalogs.png)

3.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-3.png)

4.  Select **Multi-session OS**

5.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-5.png)

6.  Select **Machines that are power managed (for example, virtual machines or blade PCs)**

7.  Select **Citrix Machine Creation Services (MCS)**

8.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-8.png)

9.  Click your master image: **mcs**

10.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-10.png)

11.  How many virtual machines do you want to create: 2

12.  Select **us-west1-c**

13.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-13.png)

14.  Input the account naming scheme: **VDA##**

15.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-15.png)

16.  Click **Enter credentials**

     ![Click enter credentials](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-enter-credentials.png)

17.  Input username: **ctx\administrator**

18.  Input password: **P@$$w0rd!@#**

19.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-19.png)

20.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.4-20.png)

21.  Input a unique Machine catalog name: **GCP – Hosted Apps Catalog**

22.  Input a description for administrators: **Hosted Apps in Google Cloud Platform**

23.  Click **Finish**

     ![Click finish](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-finish-8.4-23.png)

24.  Click Hide progress to return to the Citrix Cloud Console

     ![Click hide progress](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-hide-progress.png)

25.  Allow the Machine Catalog creation process to complete

     ![Allow the Machine Catalog creation process](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_allow-machine-catalog-creation-process.png)

26.  Double click on the Machine Catalog to view the virtual machines

     ![View virtual machines](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_view-virtual-machines.png)

27.  Select both the virtual machines

28.  Click **Start** to power on the machines

     ![Click Start to power on the machines](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_clickstart-power-on-the-machines.png)

29.  Click **Yes**

     ![Click yes](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-yes-8.4-29.png)

30.  The **Power State** column should display **On**

31.  The **Registration State** column should display **Registered**

     ![Registration state](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_registration-state-column.png)

32.  Navigate to Google Cloud Console. Select **Compute Engine** and **VM Instances**

     ![Google Cloud Console](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_navigate-to-google-cloud-console.png)

33.  The **vda01** and **vda02** virtual machines are now successfully created in **us-west1-c** zone.

     ![virtual machines](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_virtual-machines-successfully-created.png)

### 8.5 Create Delivery Groups

1.  Click **Delivery Groups**

     ![Click delivery groups](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-delivery-groups.png)

2.  Click **Create Delivery Group**

     ![Create delivery groups](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_create-delivery-group.png)

3.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.5-3.png)

4.  Increase the value to **2**

5.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.5-5.png)

6.  Select the **Allow any authenticated users to use this delivery group** option

7.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.5-7.png)

8.  Click **Add** drop-down list

9.  Select and click **From Start menu…**

     ![Click from start menu](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-from-start-menu.png)

10.  Allow the applications to be enumerated from the Server VDAs, scroll down and select **Notepad**

11.  Select **Paint**

12.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-ok-8.5-12.png)

13.  Review your application selection

14.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next8.5-14.png)

15.  Click **Add** to configure Published Desktop

     ![Click add](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-add-configure-published-desktop.png)

16.  Input a unique Display name: **Windows Shared Desktop**

17.  Input a description: **Windows Shared Desktop Test**

18.  Click **OK**

     ![Click ok](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-ok-8.5-18.png)

19.  Review the Desktops configuration

20.  Click **Next**

     ![Click next](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-next-8.5-20.png)

21.  Input unique Delivery Group name: **GCP-Hosted Apps Delivery Group**

22.  Input Delivery group description: **Delivery Group for Hosted Apps in Google Cloud Platform**

23.  Click **Finish**

     ![Click finish](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-finish-8.5-23.png)

24.  Double click on the Delivery Group to view the virtual machines

     ![Double click on the Delivery Group ](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_double-click-delivery-group.png)

25.  The Delivery Group column should display the configured Delivery Group name

26.  The **Power State** column should display On

27.  The **Registration State** column should display **Registered**

     ![Delivery group registration state](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_delivery-group-registration-state.png)

### 8.6 Configure Workspace UI and session proxy settings

This deployment guide uses Citrix Gateway service to provide secure remote access solution with diverse Identity and Access Management (IdAM) capabilities, delivering a unified experience into Virtual apps and Desktops. You access the Citrix Gateway Service using a customizable URL. Upon successful authentication, you access your virtualized resources via Citrix Workspace Service. Citrix Workspace is a service that, upon successful authentication, provides you with available resources (apps and desktops) that you can launch.

#### Configuring Citrix Workspace Service

1.  Navigate to [https://cloud.citrix.com](https://cloud.citrix.com) and log in to the Citrix Cloud console using your **username** and **password**. Provide the **token** when prompted

2.  Click the hamburger menu located on the top right section

     ![hamburger menu](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_hamburger-menu.png)

3.  Select **Workspace Configuration**

     ![Workspace configuration](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_select-workspace-configuration.png)

4.  If disabled, toggle on the Enable button

5.  Click Edit to customize the Workspace URL

     ![Workspace url](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-edit-customize-workspace-url.png)

6.  Input a unique Workspace URL of your choice: gcplab.cloud.com

     > **Note:**
     >
     > Do not enter the URL shown in this example. Choose your own unique Workspace URL and the Citrix Cloud  validates if the URL is available.

7.  Agree to the changes, which can take up to 10 minutes to be accessible

8.  Click **Save**

     ![Click save](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-save-workspace-configuration.png)

The result should be similar to the image:

![Workspace configuration result](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_workspace-configuration-result.png)

## 9 Test launch virtual app and desktop resources

1.  Navigate to the Workspace URL you customized in the Configuring Citrix Workspace Service section. At the login screen input the username: **ctx\user1**

2.  Input the password: **P@$$w0rd!@#**

3.  Click **Log On**

     ![Click log on](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-log-on.png)

4.  You are recommended to download and install the latest Citrix Workspace app or click **Use web browser** to use the HTML5 to start the application in a new browser tab

     ![Use web browser](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-use-web-browser.png)

5.  Expand **Apps**

6.  Click **All Apps**

7.  Choose the application you would like to launch (e.g., Notepad)

     ![Choose the application](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_choose-the-application.png)

8.  The publish application should launch on a separate browser tab

     ![Publish application](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_publish-application.png)

9.  Expand **Desktops**

10.  Click **All Desktops**

11.  Click Windows Shared Desktop to launch

     ![Windows shared desktop to launch](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_click-windows-shared-desktop-launch.png)

12.  The published desktop should launch on a separate browser tab

     ![published desktop](/en-us/tech-zone/learn/media/citrix-virtualization-on-google-cloud_published-desktop.png)
