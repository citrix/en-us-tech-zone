---
layout: doc
h3InToc: true
contributedBy: Mayank Singh
specialThanksTo: Vivekananthan Devaraj, Martin Zugec, Allen Furmanski, Harsh Gupta, Nitin Mehta, Victor Cataluna
description: Learn how to migrate your on-premises Citrix Virtual Apps and Desktops (CVAD) environment to CVAD service on Citrix Cloud using the Automated Configuration tool.
---
# Migrating Citrix Virtual Apps and Desktops from on-premises to Citrix Cloud

## Overview

In this deployment guide, you learn how to migrate an on-premises Citrix Virtual Apps and Desktops environment to a Citrix Virtual Apps and Desktops service in Citrix Cloud. The components that manage and control access to the environment move to Citrix Cloud. Having these components in Citrix Cloud, offloads the effort of managing and updating these components to Citrix. In Citrix Cloud, these components are deployed adhering to best practices and kept evergreen with the latest updates and security patches. Added benefits of the cloud include guaranteed uptime and flexible subscription options. The resources that host the sessions continue to run in their original resource location(s).

## Audience

This guide is intended for Citrix administrators, technical professionals, IT decision-makers, partners, and consultants who are assessing migration strategies to move their customer managed on-premises environment to a Citrix Virtual Apps and Desktops service on Citrix Cloud. The document is for users who are

*  Familiar with the administration of a Citrix Virtual Apps and Desktops or XenApp and XenDesktop site.
*  Familiar with the administration of a StoreFront environment.
*  It’s also helpful if you understand how to deploy Citrix Gateway or NetScaler Gateway.

## Typical existing architecture

The most typical architecture for an on-premises Citrix Virtual Apps and Desktops environment is:

[![Typical on-premises deployment](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_01.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_01-large.png)

The existing setup enables secure remote access to the resources (virtual apps and desktops) hosted in the customer’s on-premises data center to their users (internal or external) via a StoreFront and Citrix Gateway. The users access the setup via the Citrix Workspace app or a browser, from a device of their choice, using the Citrix Gateway URL. The customer’s Active Directory, application data, and use data all reside in the data center.

## Desired state options after migration

The CTO of the company wants to have the Citrix management components moved to Citrix Cloud and be managed by Citrix. The workloads are to remain in the on-premises data center.
Now the admin must decide where the access control components will reside after the migration.

There are 3 options available to the admins:

1.  Move the access layer to Citrix Cloud and utilize the Citrix Gateway service.

    The admin chooses this option when the customer is willing to fully realize the benefits of the Citrix Workspace. This would be an option if the Gateway was being used exclusively for the CVAD deployment.

    [![Migration to cloud with Citrix Gateway and Citrix Workspace in Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_02.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_02-large.png)

2.  Retain the on-premises Citrix Gateway and utilize the Citrix Workspace service

    The admin chooses this option when the customer wants to extend the benefits of Citrix Workspace to their end users while retaining control of the Gateway, as it may be used for other services in the customer environment.

    [![Migration to cloud with on-premises Gateway and Citrix Workspace in Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_03.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_03-large.png)

3.  Retain the existing on-premises Gateway and StoreFront combination

    The admin chooses this option if they want to keep control of both these components or if end users must continue to access the original Gateway URL to access their resources.

    [![Migration to cloud with on-premises Gateway and Citrix Workspace in Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_04.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_04-large.png)

## Migration methodologies

Once the admin decides the desired state after the migration from the options in the preceding section, they must choose which migration method to use:

1.  **Using Automated Configuration tool**

    The admin chooses this option, when the Automated Configuration tool supports their environment. This approach simplifies the migration process.

1.  **Manual**

    The admin chooses this option if they are unable to use the tool, for example, when the existing environment is not on one of the LTSR releases or on one of the latest two Current Releases.

In this guide we are looking into the Automated Configuration tool based migration process.

## Prerequisites and data to be collected before migration

As a pre-requisite, customers must ensure the network connectivity to establish communication from the customer hosted on-premises environments to Citrix Cloud. Refer to the [Citrix documentation](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html) for complete requirements.

### Pre-requisites for machines that host the Cloud Connectors

The Citrix Cloud Connectors act as the communication channel between the components hosted in Citrix Cloud and the components hosted in the resource location. The cloud connectors act as a proxy for the delivery controller in Citrix Cloud. To install Citrix Cloud Connectors in your environment, you require (at least two) Windows Server 2012 R2 or later server machines/VMs. You require static IPs for these machines. Windows installation and domain join of these machines must be done in advance. Ensure the host names of these machines help you to identify them and their location easily. The machines the Citrix Cloud Connectors run on must have network access to all the virtual machines that are to be made available to end users.

The system requirements for the Cloud Connectors are [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html).

Review the guidance on the cloud connector installation [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#installation-considerations-and-guidance).

Some requirements for Citrix Cloud Connector installation (installer performs checks for these) are:

1.  The Citrix Cloud Connector machine must have outbound Internet access on port 443, and port 80 to only *.digicert.com. The port 80 requirement is for X.509 certificate validation. See more info [here](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#certificate-validation-requirements).
1.  Microsoft .NET Framework 4.7.2 or later must be pre-installed on the machine.
1.  Time on the machine must be synced with UTC.

The [Cloud Connector Connectivity Check Utility](https://support.citrix.com/article/CTX260337) tool can be used to test the reachability of the Citrix Cloud and its related services.

### Pre-requisites for Automated Configuration tool

The pre-requisites that are needed for the tool can be found [here](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html#pre-requisites) under the pre-requisites section.

Return to this doc once you have completed the steps in the **Complete Pre-requisites for Exporting from on-premises site** section.

The following data needs to be collected:

*  Hosting connections and respective credentials
*  Zone mappings
*  Active directory credentials
*  Authentication point and provider
*  StoreFront configurations
*  Citrix Gateway configuration for each site or data center
*  If you have a Citrix Virtual Apps and Desktops service subscription, then use those credentials (skip to the [Install Citrix Cloud Connectors](#install-cloud-connectors-in-on-premises-resource-locations) section).
    *  Else follow the below steps to create a new subscription.

## Set up a basic Citrix Cloud Environment

1.  If the organization, does not have one, create a new Citrix Cloud account, by following the instructions in the **Create Citrix Cloud Account** section on this [page](/en-us/tech-zone/learn/poc-guides/cvads.html#deployment-steps). Continue with the next step once the section is complete.

1.  Log in to the Citrix Cloud account and then invite one or more administrators to the Citrix Cloud account. **Note**: Even if other administrators in the organization have access to your Citrix account on Citrix.com, they still need to be invited to the Citrix Cloud account. To do this, from the **Citrix Cloud management console**, click the **hamburger menu** in the top left corner and select **Identity and Access Management**. For more information, see [Add administrators to a Citrix Cloud account](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management/add-admins.html).

    ![Citrix Cloud - Identity and Access Management](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_05.png)

1.  Return to the main page by clicking on **Citrix Cloud** on the top of the page and then request a trial for the Citrix **Virtual Apps and Desktops service** by clicking on the **Request Trial** button in the tile.

    [![Citrix Cloud - Available Services](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_06.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_06-large.png)

    **Note**: If you know who your account team is, reach out to them to get the trial approved. If you are unsure who your account team is, continue to the next step.

1.  Click **Request a Call**

    ![Citrix Cloud - Request a call](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_07.png)

1.  **Enter your details** and in **Comments** section specify **“Citrix Virtual Desktops service trial”**. Click **Submit**. Citrix Sales will contact you to give you access to the service.

    ![Citrix Cloud - Request CVAD trial](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_08.png)

**Note**: This is not immediate, a Citrix sales rep will reach out.

## Install Cloud Connectors in on-premises resource location(s)

1.  Log in to one of the machines that has been prepared for hosting the cloud connector as a local administrator over RDP.
1.  **Open a browser** and go to the URL: [Citrix Cloud](https://citrix.cloud.com/).
1.  The follow the steps in the **Create a Resource Location** section of this [guide](/en-us/tech-zone/learn/poc-guides/cvads-windows-virtual-desktops.html#create-a-new-resource-location), login as a full Citrix Cloud administrator.
1.  Repeat the steps for all the resource locations and Cloud Connectors in your environment.

**Note**: You can also install the Cloud Connector using the [command line](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html#command-line-installation).

The installation of the Cloud Connectors registers your on-premises domain to Citrix Cloud under the **Identity and Access Management** section.

[![Active Directory association after Cloud Connector installation](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_09.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_09-large.png)

### Deploy Certificates for Cloud Connectors

If either on-premises StoreFront or Citrix ADCs are to be used after migration, then certificates are needed on the Cloud Connectors. The Cloud Connector runs the XML and the STA services on port 80 by default as these communications are typically INTERNAL. To configure encryption for these traffic types, certificates must be deployed on the Cloud Connectors and these two services must be bound to those certificates.

[![cloud-migration-strategies-Image-9](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_10.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_10.png)

Both public and self-signed certificates can be used as the customer-hosted StoreFront and on-premises Citrix ADCs need to trust the certificates.

### Enable TLS on Cloud Connectors to secure XML traffic

Refer to the [Citrix Support article](https://support.citrix.com/article/CTX221671) for detailed information on how to enable SSL on Cloud Connectors to secure the XML traffic. The XML Service is used for application and desktop resource enumeration including handling user name and password data from StoreFront to Cloud Connectors, therefore, it must be encrypted.

**Note**: Cloud Connectors cannot traverse domain-level trusts. If deploying resources in different domains, install Cloud Connectors in each user domain.

## Migration Steps

[![Migration to cloud with Citrix Gateway and Citrix Workspace in Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_02.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_02-large.png)

Once your Citrix Virtual Apps and Desktops service trial / subscription has been activated continue with the following steps:

As indicated in the migration methodologies section, the migration steps discussed in this guide use the Automated Configuration tool. Depending on whether the provisioning scheme of the Citrix on-premises environment is PVS or MCS, the procedure changes.

Follow the steps in the Automated Configuration tool steps from the section linked [here](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html#export-your-on-premises-site-configuration).

Once the migration of the control layer is complete and verification is done, return here and continue with the following steps.

### Migration steps - Configure machines running VDAs to register to the Cloud Connectors

Once the Automated Configuration tool has been run, then the machines hosting resources (running VDAs) must be configured to register with the Cloud Connectors.

#### For the MCS and PVS based machines (Pooled and Server OS) machines

The **ListOfDDCs** registry key on the golden image / virtual disk must be updated.

**Power on the golden image**, open the **Registry editor** and navigate to **HKLM\Software\Citrix\VirtualDeliveryAgent** key, and **update the ListOfDDCs with Cloud Connector host names** in their respective resource locations.

[![Update ListOfDDCs registry key on VDAs](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_11.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_11-large.png)

Shut down the machine running the golden image for MCS or virtual disk for PVS. Take a new snapshot and update the machine catalog with the new snapshot.

#### For manual machine catalogs

For catalogs that don’t use either of the provisioning methods, AD Group Policy can be used to update the registry key.

Open the **Group Policy Management Console** and create a **new GPO**.

**Edit the Policy**, select **Computer Configuration** and then **Citrix Policies**. On the right pane, select **New** under **Citrix Computer Policies**.

[![Create a policy for ListOfDDcs registry key update](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_12.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_12-large.png)

Name the Policy as **VDA Migration** and select the **Controllers** setting from the list and click **Add** to update.

[![Select Controllers setting](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_13.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_13-large.png)

Enter the **FQDN of the Cloud Connectors**, with space as a delimiter.

[![Enter Cloud Connector FQDNs](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_14.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_14-large.png)

Set the **Enable auto update of Controller** option to **Allowed**. This setting allows VDAs to update the list of controllers with newly added Cloud Connectors. Although auto-update is not used for initial registration, the auto-update downloads and stores the ListOfDDCs in a persistent cache on the VDA when initial registration occurs. This process is done on each resource machine running a VDA.

[![Enable Auto Update of Controllers](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_15.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_15-large.png)

Refer to the [VDA registration product documentation](/en-us/citrix-virtual-apps-desktops/manage-deployment/vda-registration.html#auto-update) for additional details on how auto-update works and its exceptions.

On the **Filters** page, select the **Delivery Group(s)** on which this policy needs to be applied.

[![Select Delivery Group for filtering the policy](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_16.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_16-large.png)

Check the **Enable this policy** check box and click **Create** to complete the policy creation.

![Enable and create the policy](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_17.png)

Increase the **priority** of the policy to apply the settings.

[![Increase the priority of the policy](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_18.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_18-large.png)

Once the Group Policy setting is updated, the machines start to register with the Cloud Controllers.

## Configuring the User Access layer

With everything ready the configuration of the access layer can be performed. One of the three options discussed in the earlier sections are possible.

1.  Adopting Citrix Workspace and Citrix Gateway service in Citrix Cloud.
1.  Adopting Citrix Workspace service while retaining on-premises Citrix Gateway.
1.  Retaining both on-premises StoreFront and Gateway.

## User Access Layer - Citrix Workspace and Citrix Gateway service in Citrix Cloud

To configure access via the Citrix Workspace and Citrix Gateway services navigate to **Workspace Configuration** from the **hamburger menu** on the top left of the Citrix Cloud console.

The **Access tab** shows the **Workspace URL** which is ready to use by the end-users. The first part of the workspace URL is customizable. You can change the URL from, for example, `https://example.cloud.com` to `https://<desired_URL>.cloud.com`

[![Change Workspace URL](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_19.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_19-large.png)

Enable connectivity using the Gateway service by clicking the ellipses for the desired resource location and selecting **Configure Connectivity**. (Perform this step and the following steps for each resource location that is being migrated).

Select **Gateway Service** radio button and click **Save**.

![Choose Gateway Service](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_20.png)

The **Authentication tab** allows for configuration of the authentication mechanism. Choose the desired method.

![Choose Authentication method](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_21.png)

The **Customize tab** in Workspace Configuration allows you to customize the Workspace appearance and preferences.

Click **Service Integrations tab**, ensure that the **Virtual Apps and Desktops On-Premises Sites** tile lists one or more sites and is **Enabled**.

Users can now login to the Workspace URL that was configured and login to the Workspace for accessing the on-premises resources.

## User Access Layer - Citrix Workspace service with on-premises Gateway

First the on-premises Gateway is configured to enable external access.

1.  **Connect to the on-premises Citrix ADC** from a browser and **login as an administrator**. In the **Integrate with Citrix Products** section, click **XenApp and XenDesktop**.

    [![Open Citrix ADC console](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_22.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_22-large.png)

1.  Follow the wizard and provide the required **details for FQDN and SSL Certificate** for the configuration.

    [![Enter Gateway FQDN and SSL cert details](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_23.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_23-large.png)

1.  Enter **Workspace URL** in the **StoreFront URL** field. Click **Retrieve Stores**. Enter the **Active Directory Domain** in the **Default Active Directory Domain** text box. Enter **the URLs of the Cloud Connectors** as `http://<cloudconnector.FQDN>` or `https://<cloudconnector.FQDN>` (if SSL certificates are configured). Click **Test STA Connectivity**, ensure it passes.

    ![Enter StoreFront Details and test STA](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_24.png)

1.  Complete the configuration of the Gateway, no need to provide Authentication and session policy details.

To configure access via the Citrix Workspace service, **login to Citrix Cloud** and navigate to **Workspace Configuration** from the **hamburger menu** on the top left of the Citrix Cloud console.

The **Access tab** shows the **Workspace URL** which is ready to use by the end-users. The first part of the workspace URL is customizable. You can change the URL from, for example, `https://example.cloud.com` to `https://<desired_URL>.cloud.com`

[![Change Workspace URL](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_19.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_19-large.png)

Enable connectivity using the on-premises Gateway by clicking **the ellipses for the desired resource location** and selecting **Configure Connectivity**. (Perform this step and the following steps for each resource location that is being migrated).

Select **Traditional Gateway Service** radio button and click **Add**.

![Choose Traditional Gateway](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_25.png)

Click **Test STA** to confirm connectivity to the Cloud Connector based STA services. Once it passes. Click **Save**.

![Test STA connectivity](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_26.png)

If the test fails, check the binding of Citrix Cloud Connectors as Secure Ticket Authority (STA) servers to Citrix Gateway. For more information, see [CTX232640](https://support.citrix.com/article/CTX232640).

The setup is now ready for users to connect via the configured Workspace URL.

## User Access Layer - On-premises StoreFront and Gateway

The StoreFront servers need to communicate with Cloud Connectors for resource enumeration from Citrix Cloud. The Cloud Connectors are installed with SSL certificates to ensure that the XML and STA traffic is encrypted and secure.

Configure the **store** on StoreFront Servers with **Cloud Connectors**. If you are creating a new store add the Cloud Connectors on the **Delivery Controllers** page. If you are using the existing store, select the **Manage Delivery Controllers** option and **update the FQDNs of the Cloud Connectors**.

[![Update Cloud Connector FQDN in Controller field](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_27.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_27-large.png)

Enable the **Remote Access** option to integrate the Store service with the on-premises Gateway to enable external access for this store.

![Enable Remote Access](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_28.png)

Configure the **trusted domain** for authentication and apply the customizations required as per the organization requirements. Access the StoreFront URL internally and verify the access.

For external access, we need to verify the **Security Ticket Authority** details. If not added in previous steps add those details now.

[![Configure STA](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_29.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_29-large.png)

Configure the CallBack URL if necessary and finish the StoreFront configuration.

Now configure the on-prem Gateway. Perform the first 3 steps in the on-premises Gateway configuration in the [preceding section](#user-access-layer---citrix-workspace-service-with-on-premises-gateway). Once done, return here and follow the remaining steps.

Configure the **Active Directory Domain** details for the authentication.

![Add Active Directory Domain Details](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_30.png)

Configure the **Session Policies** to complete the gateway configuration. Also, apply the necessary themes with the required customization.

[![Configure Session Policies](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_31.png)](/en-us/tech-zone/build/media/deployment-guides_cvads-migration_31-large.png)

On-premises StoreFront and Gateway configuration are successfully completed. The users can now seamlessly access their resources as they used to before the migration using the **StoreFront URL**.

That completes the deployment guide to migrate an on-premises Citrix Virtual Apps and Desktops environemnt to the Citrix Cloud using the Citrix Virtual Apps and Desktops service.

### Call to action

Request a trial of Citrix Virtual Apps and Desktops service, click [here](https://www.citrix.com/products/citrix-virtual-apps-and-desktops/form/inquiry/).

Try the Automated Configuration tool, click [here](https://www.citrix.com/downloads/citrix-cloud/product-software/automated-configuration.html).

## References

[Citrix Virtual Apps and Desktops service product documentation](/en-us/citrix-virtual-apps-desktops-service.html)

[Reference Architecture for Citrix Cloud Virtual Apps and Desktops Service](/en-us/tech-zone/design/reference-architectures/virtual-apps-and-desktops-service.html)

[Cloud Connector connectivity requirements](/en-us/citrix-cloud/overview/requirements/internet-connectivity-requirements.html)

[Cloud Connector sizing guide](/en-us/citrix-virtual-apps-desktops-service/install-configure/install-cloud-connector/cc-scale-and-size.html)

[Cloud Connector Technical Details](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details)

[Deployment scenarios for Cloud Connector with Active Directory domains](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/technical-details.html#deployment-scenarios-for-cloud-connectors-in-active-directory)

[Cloud Connector installation](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/installation.html)

[Citrix Cloud Identity and Access Management](/en-us/citrix-cloud/citrix-cloud-management/identity-access-management.html)

[Automated Configuration tool POC guide](/en-us/tech-zone/learn/poc-guides/citrix-automated-configuration.html)

[Machine Catalog creation and types](/en-us/citrix-virtual-apps-desktops-service/install-configure/machine-catalogs-create.html)

[Citrix Policies](/en-us/citrix-virtual-apps-desktops-service/policies.html)

[Cloud Connector Updates](/en-us/citrix-cloud/citrix-cloud-resource-locations/citrix-cloud-connector/cloud-connector-updates.html)

[How to install SSL Certificate on Cloud Connectors](https://support.citrix.com/article/CTX221671)
