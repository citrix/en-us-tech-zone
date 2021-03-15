---
layout: doc
h3InToc: true
Author: Arnaud Pain
description: Learn how to migrate your on-premises Citrix ADM (Aplication Delivery Management) to Citrix Cloud.
---
# Migrating Citrix ADM from on-premises to Citrix ADM (Aplication Delivery Management) service

## Overview

In this document, you'll discover how to migrate Citrix ADM (Aplication Delivery Management) on-premises to Citrix ADM (Aplication Delivery Management) service. Migrating to cloud resources modernizes your deployment, providing enhanced elasticity, scalability, and management.

The guidance documented here is based on a deployment in a Citrix approved lab environment. The initial and final deployments represent typical customer environments.

### The best migration path

In our experience and testing, the best migration path follows these steps:

1.  [Set up a basic Citrix Cloud environment](/en-us/tech-zone/build/deployment-guides/citrix-adm-service-migration.html#set-up-a-basic-citrix-cloud-environment).
2.  [Deploy ADM Agent](/en-us/tech-zone/build/deployment-guides/citrix-adm-service-migration.html#deploy-adm-agent).
3.  [Migrate to ADM Service](/en-us/tech-zone/build/deployment-guides/citrix-adm-service-migration.html#migrate-to-adm-service).
4.  [Validation](/en-us/tech-zone/build/deployment-guides/citrix-adm-service-migration.html#validation).


### Audience

We've written this document for users who are

*  Familiar with the administration of a Citrix ADM (Aplication Delivery Management).

It's also helpful if you know Citrix Cloud fundamentals and understand Citrix ADM (Aplication Delivery Management) service.

## Set up a basic Citrix Cloud environment

For more information on onboarding process see the [Getting Started](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/getting-started.html) section.
During the initial configuration of the ADM Agent Service, you will need to provide the **Service URL** and **Activation Code** that are provided during the initial configuration in Citrix Cloud.

 ![Activation Code](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_01.png)


## Deploy ADM Agent

More details can be found [here](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/getting-started/install-agent-on-premises.html).

1. Download the agent image as instructed in [Getting Started](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/getting-started.html).
2. Import the agent image file to vSphere.
3. From the Console, configure the initial network configuration options as show in the below example:

![ADM Agent Network](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_02.png)

4. After completing the initial network configuration, save the configuration settings.

![ADM Agent Save](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_03.png)

5. When prompted, log on using the default (**nsrecover/nsroot**) credentials.

![ADM Agent Log](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_04.png)

6. Run the script **/mps/register_agent_cloud.py**.

![ADM Agent register](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_05.png)

7. Enter the **Service URL** and the **Activation Code** that was provided in Citrix Cloud during initial configuration.

![ADM Agent Service URL and Activation Code](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_06.png)

8. You will be prompted to change ADM Agent default password.

![ADM nsroot password change](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_07.png)

9.  After update of the Agent Password and successful registration, the agent will restart to complete the installation process.

![ADM Agent restart](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_08.png)

>**Note:**
>
>As we will migrate from on-premises ADM, you do not need to continue the Agent configuration and can just click on Skip.

## Migrate to ADM service

After the onboarding in done, the next step is to upgrade on-premises ADM to a Firmware that include the script that will be used to migrate.
You can migrate on-premises Citrix **ADM 13.0 64.35 or a later version** to Citrix Cloud. If your ADM has 12.1 or an earlier version, you must first upgrade to **13.0 64.35 or a later version** and then migrate to Citrix Cloud. For more information, see the [Upgrade section](https://docs.citrix.com/en-us/citrix-application-delivery-management-software/current-release/upgrade.html).

>**Note:**
>
>At the time of writing this documentation, the latest version is 13.0 Build 76.29.


Once your ADM is on the required version, you can start the process for the migration, the next step is to deploy an on-premises Agent.

# Configure ADM Agent service

To enable communications between Citrix ADC instances and Citrix ADM, you must configure an agent. Citrix ADM agents are, by default, automatically upgraded to latest build. You can also select a specific time for the agent upgrade. For more information, see [Configuring agent upgrade settings](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/setting-up/configuring-agent-upgrade-settings.html).

* If your existing on-premises ADM (standalone or HA pair) has no on-premises agents configured, you must configure at least one agent for ADM service.
* If your existing on-premises ADM (standalone or HA pair) has configured with on-premises agents for multisite deployments, you must configure the same number of agents for ADM service.
  
For more information on configuring an agent, see the [Getting Started](https://docs.citrix.com/en-us/citrix-application-delivery-management-service/getting-started.html) section.

1. Connect to Citrix Cloud.

![Citrix Cloud](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_09.png)

2. Click on the **Home** icon and select **Identity and Access Management**.

![Citrix Cloud IAM](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_10.png)

3. Click on **API Access** tab.

![Citrix Cloud API](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_11.png)

4. Provide a **name** for Secure client and click on **Create Client**.

![ADM Create Client](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_12.png)

5. Click on **Download**.

![ADM Secure Client Download](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_13.png)


# License

f you use your on-premises ADM deployment as a license server for ADC instances, you will need before the migration to reallocate your licenses to ADM Service.
In fact, during the migration process, the ADC license configuration will be updated to point to ADM Agent Service instead of your ADM on-premises.

1. Connect to Citrix Cloud ADM Service.
2. Navigate to **Networks > Licenses**.

![ADM License 1](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_14.png)

3. Take not of your Host ID and go to [http://www.mycitrix.com](http://www.mycitrix.com) to reallocate your licenses.
4. Ensure you licenses are present in ADM Service before starting the migration.

![ADM License 2](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_15.png)


# Migrate

The secureclient.csv download form previous steps need to be uploaded to primary ADM, this can be done using a tool like Winscp. Copy the client ID and secret CSV file, for example, in the /var directory.

>**Note:**
>
>For an ADM HA pair, copy the CSV file in the primary node.

![Secure Client](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_16.png)

We recommend to update to ADM 76.x or later builds as the migration scripts (**servicemigrationtool.py** and **config_collect_onprem.py**) are available as part of the build, available in **/mps/scripts**.

>**Note:**
>
>Ensure that the on-premises ADM has internet connectivity during migration.

1. Using an SSH client, log on to the on-premises ADM.

![ADM SSH](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_17.png)

2. Enter in **Shell**
   
>**Note:**
>
>For an ADM HA pair, log on to the primary node.

3. Validate if the CSV file is present.

![ADM SSH validation](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_18.png)

4. Run the following commands to complete the migration:
    a. cd /mps/scripts
    b. python servicemigrationtool.py

For example: *python servicemigrationtool.py /var/secureclient.csv*

![ADM Migration](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_19.png)

After you run the script, it checks the prerequisites and then proceeds with the migration. The script first checks for the license availability. The following message is displayed only if you have lesser ADM service license than the on-premises license.

![Migration less licenses](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_20.png)

If you select **Y**, the migration continues by licensing the VIP randomly. If you select **N**, the script stops the migration.
If you have the unsupported ADC instance version for the pooled license server, the following message is displayed:

![Migration Unsupported](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_21.png)

If you select **Y**, the migration process continues by changing the license server. If you select **N**, the script prompts if you want to proceed with rest of the migration. The script stops the migration if you select **N**.
If you have the supported ADC instance version for the pooled license server, the following message is displayed:

![Migratin License change](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_22.png)

>**Note:**
>
>You will only see above the Primary Node IP Address.

If you select **Y**, the migration process continues by changing the license server.
Depending upon the on-premises configuration, the approximate time for the migration to complete is between a few minutes and a few hours. After the migration is complete, you see the following message:

![Migration Finished](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_23.png)

The migration is successful once all the ADC and SD-WAN WANOP instances and their respective configurations are successfully moved to ADM service. 

## Validation

After successful migration, the on-premises Citrix ADM stops processing the following instance events:
* SSL certificates
* Syslog messages
* Backup
* Agent cluster
* Performance reporting
* Configuration audit
* Emon scheduler

![Migration end](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_24.png)

You can connect to Citrix ADM Service and ensure you see your ADC(s) instance(s).

![Validation](/en-us/tech-zone/build/media/deployment-guides_citrix-adm-service-migration_25.png)
