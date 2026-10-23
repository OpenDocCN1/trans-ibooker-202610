
6. Select the My Items tab at the top of the browser window, and you will see the image you just created. Select it and enter a username and password. Select the Allow Selected Ports radio button and then enable HTTP (80), HTTPS (443), and RDP (3389). Click the Review + Create button, and review the details, specifically the Networking section. Click the Create button. If you used the same resource group and subscription as you did in the previous exercise, the networking values default, for example, to VNET = CSHARPGUITAR-VNET-B and Subnet = csharp.

7. Once deployed, get the public IP address of the VM and navigate to it via a browser.

Now that you have an image that, as in my example created for Exercise 4.6, responds with, “Hello from CSHARPGUITAR-SC.” You can use this image you created any number of times as you create redundancy and failover solutions. The image is also available for usage with a virtual machine scale set (VMSS), which will be discussed in more detail later in the chapter. Let’s switch gears now and look a little more at the two most popular supported operations systems available for Azure VMs: Windows and Linux. An Azure Solutions Architect Expert must grasp which version of each operating system is supported on Azure. Know, too, that the OS has limits imposed by the subscriptions, CPU, IOPS, RAM, and storage available. You can expect some questions about this on the exam. For example, which of the following types of VMs cannot be deployed to Azure?

## Windows

The Windows Server operating system is one of the most used software products for running enterprise-level workloads. In this section and in the following one focused on Linux Azure VMs, we’ll focus on the supported OS versions, a subset of recommended VM sizes for those OS versions, and some quota limits specific for the OS and version. Recall from Exercise 4.5 that you selected a prebuilt image of Windows Server 2019 Datacenter Server Core from the Azure Marketplace. Take a look at the following 10 supported Windows images available via the Azure Marketplace in the portal:

- Windows Server 2008 R2 SP1
- Windows Server 2012 Datacenter
- Windows Server 2012 R2 Datacenter
- Windows Server 2016 Datacenter
- Windows Server 2016 Datacenter - Server Core
- Windows Server 2016 Datacenter - with Containers
- Windows Server 2019 Datacenter
- Windows Server 2019 Datacenter Server Core
- Windows Server 2019 Datacenter Server Core /w Containers
- Windows Server 2019 Datacenter with Containers

There are some recommended sizes of an Azure VM based on each of these operating system versions. The recommendations are arranged based on the categories that were presented previously in Table 4.2. Therefore, it is important to know into which category your application falls. Table 4.3 provides a list of recommended Azure VM sizes based on the selected workload category. The values in the Instance/Series column, in my example, DS1V2, represent the grouping of many compute components such as CPU speeds, CPU-to-memory ratio, temp storage ranges, maximum data disks, IOPS, and network bandwidth throughput. The ratio between each of those components is different for each series and therefore has an intended target application category. You can find the specifics of each Windows OS Azure VM here:

```
docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-memory.
```

As an Azure Solutions Architect Expert, you would be expected to recommend the category and VM series when given the specifications of the workload being deployed onto an Azure VM.

### TABLE 4.3 Windows Versions to Azure VM Size Recommendation

| Windows Version | Category | Instance/Series |
|---|---|---|
| 2008 R2 SP1 | General Purpose | DS1V2, DS2V2, D2SV3, D1V2, D1, DS1, DS2 |
| 2012 Datacenter |  |  |
| 2012 R2 Datacenter | Memory Optimized | DS11V2, E2SV3, DS11 |
| 2016 Datacenter * | General Purpose | DS1V2, DS2V2, D4SV3, D1V2, D1, DS1, DS2 |
| 2019 Datacenter * | Memory Optimized | DS11V2, E2SV3, DS11 |

If you see an asterisk (*), it symbolizes all variants of the Windows versions. As covered later in the “Migrating Azure Virtual Machines” section, it is possible to bring your own on-premise VM to the Azure platform. This means that almost any OS and its configuration that you can export and convert to the virtual hard disk (VHD) format can be deployed to an Azure VM. That sounds a bit easier than actually doing it. Sure, you can deploy a VM running Windows Server 2003; however, some capabilities such as cluster failovers, Azure VM agents, or VM extensions will not work or be supported. Some other common Windows Server features that are not supported on Azure are included in the following list:

- Wireless LAN Service
- SNMP Services
- Dynamic Host Configuration Protocol (DHCP)
- Storage manager for SAN
- Hyper-V
- DirectAccess
- Network Load Balancing
- BitLocker Drive Encryption
- RRAS
- Rights Management Services
- Peer Name Resolution Protocol
- Windows Deployment Services

A primary reason for limits and quotas is to prevent the accidental consumption of resources that results in a high, unexpected charge. Most of the limits are considered soft, which means if you need more, then you can get more by contacting Microsoft and requesting more. These limits are typically bound to a subscription or a region. I have seen many customers creating multiple subscriptions or deploying to multiple regions to get around the soft limits instead of realizing that the limit, which is there for protection, can be increased. Contacting Microsoft to get the soft limits increased would make managing your workloads on Azure more intuitive. Moving resources to other subscriptions or regions when those resources are all part of the same solution makes things harder to manage. There are, however, some hard limits, imposed on almost all customers that you must adhere to. Those numbers are big, and most companies wouldn’t need (or couldn’t afford) so much. They also are not always documented. This is because if the hard limit isn’t hard-coded into the product, then it could be increased even more than a documented limit, if the business case is justified. Table 4.4 describes some subscription limits that are related to Azure VMs.

### TABLE 4.4 Azure VM Limits

| Azure Resource | Soft/Default Limit | Hard/Max Limit |
|---|---|---|
| Virtual machines | 25,000 per region | Contact Microsoft |
| Virtual networks | 100 per region | 1000 per region |
| Managed disks | 50,000 per region | Contact Microsoft |
| Storage accounts | 250 per region | Contact Microsoft |
| Virtual machine cores | 30 per region | Contact Microsoft |

Also note that these limits change often. The ones listed in Table 4.4 and most of the numerical and relationship limits existed when the Azure Solution Architect Expert exam was created. Learning these will help you answer any question on the exam regarding this topic. By relationship matching I am referring to Table 4.3 where the links between OS version, category, and VM size are displayed.

The limit of 30 cores per region in Table 4.4 refers to a specific scenario where the limit applies to specific instance/series types. For example, you cannot have more than 30 cores of series A and D VMs in the same region. Additionally, there is a limit of 30 with the Dv2 and F series VMs in the same region. You can, however, have 10 cores of A1 and 20 cores of D1 in the same region equaling 30. You could also have 30 cores of D1 and 30 cores of F1 in the same regions because the instance/series are not grouped together in the limits logic. This is an important point to know when using IaaS; however, I wouldn’t expect such a question on the Azure Solutions Architect Expert exam, so just keep it in mind as you progress to being not only a certified Azure Solutions Architect Expert but also a tenured and highly competent one.

## Linux

Linux is rapidly becoming the most utilized operating system for running small to medium workloads. Microsoft is helping to support that growth by providing the means for its simple implementation onto the Azure platform. As you have already built an Azure VM, you know how easy that was. The only difference from the previous exercise where you built the Windows VM is that you would choose a Linux image from the drop-down box instead of a Windows one. The other steps are the same; there is no apparent attempt to make deploying Windows VMs easier than Linux. The image defaults to a Ubuntu image. Currently there are six flavors of Linux OS offerings in the Azure Marketplace.

- Ubuntu Server
- Red Hat Enterprise 7
- Container Linux by CoreOS
- Clear Linux OS
- SUSE Linux Enterprise
- Debian

Table 4.5 lists the Azure Marketplace images available for each of those flavors.

### TABLE 4.5 Azure Marketplace Linux Versions

| Linux OS | Linux Version |
|---|---|
| Ubuntu Server | 14.04 LTS, 16.04 LTS, 18.04 LTS |
| Red Hat Enterprise | 7.2, 7.3, 7.6 |
| CoreOS | Alpha, Beta, Stable 7.5 |
