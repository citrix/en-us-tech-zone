---
layout: doc
h3InToc: true
contributedBy: Loay Shbeilat
description: Learn about the different Azure instance type scale characteristics and how MSC I/O enhances the response time for your users. The document guides you through choosing the ideal Azure instance type for your workload and touches upon cost per user.
tz_title: The scalability and economics of delivering Citrix Virtual App and Desktop services on Azure
tz_products: citrix-virtual-apps-and-desktops;
---
# Design Decision: The scalability and economics of delivering Citrix Virtual App and Desktop services on Azure

The goal of this document is to provide guidance to enterprises that are moving towards deploying Citrix Virtual Applications and Desktops (CVAD) in the Microsoft Azure cloud. To provide the best possible advice to our customers, we decided to determine the answer to four key questions that impact Citrix architecture and design decisions

1.  What is the most efficient instance series for hosting CVAD
2.  What is the most cost-effective instance type in the most efficient family
3.  What impact does the Machine Creation Services I/O (MCSIO) cache have?
4.  How does Windows 10 Multisession scalability compare to Windows Server OS?

A more detailed paper is available from Citrix that goes into the specifics of the testing methodology and the performance results captured during the evaluation. This paper focuses on the high-level results and provides guidance on how to design an efficient Citrix implementation in the Microsoft Azure cloud.

To determine the performance, we used LoginVSI 4.1.32.1, which creates simulated sessions against a single Citrix server. The two workloads that we used for testing are described as follows:

-  Task Worker Workload – includes segments with Microsoft Office 2016 Outlook, Excel, Internet Explorer, Adobe Acrobat, and PDF Writer. The Task Worker workload does not place a high demand on the environment and represents users that do not access the system heavily.
-  Knowledge Worker Workload – includes segments with Microsoft Office 2016 Outlook, Word, PowerPoint, Excel, Adobe Acrobat, FreeMind, PhotoViewer, Doro PDF Writer and includes viewing of several 360p movies. The Knowledge Worker workload places a higher demand on the environment, including more use of the available memory, and represents users that access the system more heavily.

The number of users successfully completing the multi-session test provides a key performance indicator under real-world conditions. This value, referred to as the VSImax session count, is used for the comparative analysis. The Login VSI workloads calculate the VSImax session count by observing the response time of a single user on the system. VSImax is reached when the response time has diminished significantly below the expected threshold which was derived from the baseline value taken with only a single user on the system.

To provide conservative numbers which can be replicated consistently without specialized knowledge, all results here reflect test execution using default Citrix policies and unoptimized default settings for Windows and Office products. Both performance and density can be improved by applying Citrix optimization tools such as the [Citrix WEM](/en-us/workspace-environment-management/current-release.html) and [Citrix Optimizer](https://support.citrix.com/article/CTX224676).  

## What is the most efficient instance series?

To find the most efficient instance series, we needed test the different instance series without changing any other variables in the mix. The base image was Windows Server 2016 with the 1903.1 version of the Citrix VDA and a standard hard disk drive (HDD) 128 GB disk for the system C: drive. We selected the 8-core instance types for two primary reasons:

1)  They represent the workhorse of Azure instance types for hosted sessions and are generally the most popular size deployed
2)  They provide a good balance of CPU/RAM and minimal OS impact as opposed to a smaller 2-core system.

The following graph shows the instance family results along with the average cost per user per hour based on pay-as-you-go pricing for the Azure US-West-2 region where the tests were performed.

![8-Core Performance](/en-us/tech-zone/design/media/design-decisions_azure-instance-scalability_001.png)

### Analysis

Most of these instance types use the same processor, Intel(R) Xeon(R) CPU E5-2673 v4 @ 2.30 GHz. The primary difference being the amount of memory available to the virtual machine. More information about these different series can be found on [Microsoft's website](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/series/).

Generally speaking, the 8-core instances have fairly similar performance, especially when you consider the physical cores (D13\_v2, D4\_v2, L8s) compared to hyper-threaded cores (F8s\_v2, D8\_v3, E8\_v3). However, when the instance's hourly cost is considered, the D13\_v2, and F8s\_v2 instances provide the more efficient use. The E\_v3 and LS\_v1 series are less cost-efficient because Microsoft charges a higher premium for Memory optimized and Storage optimized instances. In situations were your user's applications are extremely memory or storage intensive, these instances often provide a good return on investment.

### Recommendations

If your typical user's applications are CPU-intensive and do not require significant memory to run, the most cost-efficient performance is the F series. Select the **F series** when you need excellent CPU response times and do not require a significant amount of memory. If your user's applications consume a fair amount of memory, use one of the D instance types depending on how much extra memory per core is required for your user's environment.

## What is the most cost-effective instance type in the most efficient family?

When we completed the broad test across families, we expected a single series to be a clear leader. However the results ended up convincing us that the two best instance families for extra testing were the D series and the F series when tested with standard HDD storage. The next step was to test the specific sizes ranging between 2 and 16 vCPUs within the D\_v2 and FS\_v2 families. The results of these tests are shown below.

![FS and D Series Performance](/en-us/tech-zone/design/media/design-decisions_azure-instance-scalability_002.png)

### Analysis

The graph above shows the results of those tests with the highest densities of 74 and 63 user sessions, for task worker and knowledge worker respectively, being obtained on the D14\_v2 instance type (16 cores, 112 GB RAM). Since pricing varies between the instance types, a higher density does not necessarily imply a lower cost per user.

The pricing model for Azure instances varies according to the region, the instance type, and the resources provided. The graphs above also includes the cost efficiency of each instance type based on VDA user densities achieved in the single-server testing. The costs reflect U.S. West 2 Pay-As-You-Go pricing for standard VM instances as of September 2019 and includes the cost of Microsoft Windows licensing.

As shown in the graph above, the D13\_v2 instance type shows the lowest cost per hour per user of $0.018 for a task worker, with the F16s\_v2 and F8s\_v2 coming in second with a cost of $0.019. As for knowledge worker, both the F16s\_v2 and F8s\_v2 instance types share the best hourly cost of $0.025, followed closely by the D13\_v2 instance type at $0.026.

### Recommendations

In the testing, the density results showed a clear benefit from the faster processors available with the FS v2-series instances when under the heavier workload of the knowledge worker. However, the FS v2 memory-to-core ratio is lower than the D v2-series ratios and we recommend using the FS v2-series instances only when the memory consumption for the workload is low. If users run applications that are memory-intensive, the D\_v2-series is the best choice.

When the cost per user is similar, such as the case with F8S\_v2 and F16s\_v2, select the smaller instance sizes, when either of the following conditions exist:

-  Need for resiliency: you want to impact fewer users during maintenance windows
-  Need for efficient power management: you want to power off unused machines quickly

Select the larger instance sizes when either of these conditions exist:

-  Need for reduced management: you want to manage fewer machines in the environment
-  Need reduced API calls: you need less API calls to Azure infrastructure for operations

## What impact does the Machine Creation Services I/O cache have?

The instance types used for testing were configured with standard storage rather than more expensive SSD storage for the system drive on the virtual machine. Because the instance types with SSD storage have smaller ephemeral disks where the page file is stored, even though the disks were faster, the scalability was lower because the instance did not have enough swapfile space available to support the need virtual memory under a higher load.

At the disk sizes that we are using, the HDD and SSD disks have similar IOPS performance (500). While the SSD disks have a more consistent performance, the additional cost is not always justified.

We decided then to consider the Machine Creation Services I/O (MCSIO) cache, as a way to achieve SSD-like performance with the larger standard disks. The tests were completed using the Citrix VDA version 1903.1 and Windows Server 2016 on a D5\_v2 (16 vCPU, 56 GB of RAM) instance type. The chart below shows the increase in user density gained by enabling the MCSIO cache with the Knowledge worker load.

![MCSIO Performance](/en-us/tech-zone/design/media/design-decisions_azure-instance-scalability_003.png)

### Analysis

When the operating system disk has no MCSIO cache enabled, the VSImax User score was 61 on a 128 GB HDD, 74 on a 64 GB SSD disk and 75 on a 128 GB SSD disk. Enabling the MCSIO cache on a standard HDD disk actually provided better performance than an SSD, with a 4 GB cache enabled on the 64 GB HDD the score increased to 76 and with a 2 GB cache the score increased slightly more to 77. The loss of the additional user between the 4 GB and 2 GB cache sizes is attributed to the additional RAM being used for the cache and not available for user workload.

While MCSIO contributes to a lower cost per user per hour, that number is not significant on its own. The real impact of MCSIO can be ascertained when looking at the end user experience. The graph below shows the average response time drop when using MCSIO.

![MCSIO Login Response](/en-us/tech-zone/design/media/design-decisions_azure-instance-scalability_004.png)

### Recommendations

If user experience is a driving factor when considering performance, we recommend enabling the MCSIO cache. When enabled, the recommendation is to use a standard disk with the 2GB cache since it provides the best improvement without affecting user density. However, the MCSIO cache must not be enabled on virtual machines that are memory constrained, such as the F or FS series instance types that are optimized for compute but have low memory-to-cpu core ratios.

## How does Windows 10 Multisession scalability compare to Windows Server OS?

With the release of both the Windows Server 2019 and Windows 10 Multi-session operating systems, we thought it would be best to provide some guidance about how the client operating system would impact the scalability. Both Windows Server 2019 and Windows 10 Multi-session operating systems require the newer Citrix VDA version 1906.1. Windows 10 Multisession is available with Azure Virtual Desktop (AVD) Entitlement and grants the tenant the base price of the VM (Linux pricing). That entitlement also extends the VM pricing to Windows Server 2016 and Windows Server 2019.

The graph below shows the density changes when compared against the same test runs with Windows Server 2016 using the Citrix VDA version 1906.1 on the same D4\_v2 (8 vCPU, 28 GB of RAM) instance. The prices below are using the Linux VM pricing in line with the AVD Entitlement required.

![Operating System Performance](/en-us/tech-zone/design/media/design-decisions_azure-instance-scalability_005.png)

### Analysis

When compared to the Windows Server 2016 results, the Windows Server 2019 provided a slightly lower user density for both the knowledge worker and the task worker, with a 7% decrease for task workers and a 12% decrease for knowledge workers.

Comparing Windows Server 2019 to Windows 10 Multisession workload resulted in 19% less task workers and 32% fewer Knowledge workers. This performance decrease is expected because Windows 10 is a full client version and is not optimized for server-based computing like Windows Server 2016 and Windows Server 2019.

One cost advantage of using Windows 10 Multisession is that it does not require RDS CAL licenses for clients to connect to the virtual machine. This cost advantage is not included in the calculations above since it is a Microsoft licensing cost in addition to the Azure cost per hour.

### Recommendations

When planning to upgrade from Windows Server 2016 to Windows Server 2019, expect to increase the number of virtual machines by approximately 10%. If you are planning on using Windows 10 Multisession for hosting applications that require the Windows client for compatibility, keep in mind the density will be lower, resulting in approximately 30% extra costs over the server operating systems. The Windows 10 Multisession though does allow users to access the Windows **Store**, something that the server operating systems do not have available.

## Conclusion

The Azure instance type that you select to deploy Citrix virtual application workloads is a critical element that determines the user density and scalability, and in turn the cost-per-user for an Azure delivery model. As shown, the different instance types in Azure have advantages for specific workloads, such as high computational requirements or extra memory. Usually, a D13\_v2 instance with standard HDD disks and a 2GB MCSIO cache enabled provides the best user performance at the lowest cost. Consider the Windows 10 Multisession operating system when you need Windows Store, application compatibility, or a true Windows client experience.

The Citrix on Azure results presented here represent only guidelines in configuring your Azure solution. If you do not have data about your specific user workloads, the numbers we provided here serve as your design estimates. Before making final sizing and deployment decisions, we strongly suggest that you run proof-of-concept tests on different Azure instance types using your own workloads, then use that data for your final designs.

### Learn more

For more information about deploying Citrix Virtual Apps workloads on Microsoft Azure Cloud Services, see the Citrix and Microsoft partner website at [https://www.citrix.com/global-partners/microsoft/resources.html](https://www.citrix.com/global-partners/microsoft/resources.html)
