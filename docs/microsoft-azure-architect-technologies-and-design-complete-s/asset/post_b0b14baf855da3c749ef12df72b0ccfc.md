
| Linux OS | Linux Version |
|---|---|
| Clear Linux OS | Basic, containers, machine learning |
| SUSE Linux Enterprise | 15, 12 SP4 |
| Debian | 8, 9 |

There is a concept referred to as *blessed images* or *endorsed distributions* in the context of Linux. As stated earlier, it is possible to build any machine using any OS and configuration and attempt to deploy it to Azure. The key word there is *attempt*. There are so many possible configurations that it would be inconceivable at this time to have 100% coverage. I once tried unsuccessfully to deploy a Linux OS that was not blessed. When you deploy and experience some kind of issue, the place where you tend to turn to is the Serial Console. The Serial Console lets you make a hardware connection to the VM instead of using SSH, which requires a network connection. The Serial Console is listening on a virtual console named tty0 by default. The Linux image I was deploying was configured to listen on tty1, and I couldn’t connect to it. Only by chance was I able to figure that out, but this is an example of what happens when you do not use an endorsed or recommended image. There are many “one-offs” that can occur and delay your deployment or, worse, completely prevent it. It is therefore most prudent that your application targets one of the Azure Marketplace images; however, there are more Linux flavors that are considered blessed and endorsed that do not exist in the Azure Marketplace. They are listed in Table 4.6.

## TABLE 4.6 Additional Endorsed Linux Flavors

| Linux OS | Linux Version |
|---|---|
| CentOS | 6.3, 7.0 |
| CoreOS | 494.4 |
| Debian | 7.9, 8.2 |
| Oracle Linux | 6.4, 7.0 |
| Red Hat Enterprise | 6.7, 7.1, 8.0 |
| SUSE Linux Enterprise | SLES for SAP, 11 SP4, 12 SP1 |
| openSUSE | Leap, 42.2 |
| Ubuntu | 12.04 |

As with the Windows machines, the recommendations for the VM sizes are based on category and the operating system version. Please find the VM size recommendations per Linux OS flavor in Table 4.7. For greater visibility into the details of the Linux VMs instances, take a look at https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-memory. As the amount allocated for compute resources increases, so does the cost; therefore, choosing the right size is important.

## TABLE 4.7 Linux Versions to Azure VM Size Recommendation

| Linux version | Category | Instance/Series |
|---|---|---|
| Ubuntu CoreOS | General Purpose | DS1V2, DS2V2, D2SV3, D4SV3, D1V2, D1, DS1, DS2 |
|  | Memory Optimized | DS11V2, E2SV3, DS11 |
| Red Hat Enterprise | General Purpose | DS2V2, D2SV3, D4SV3, D2V2, D2V3, D2, DS2 |
| Clear Linux OS | General Purpose | D1, D3, DS3, DS4 |
| SUSE Linux Enterprise | General Purpose | DS1V2, DS2V2, D1V2, D1, Ds1, DS2 |
|  | Memory Optimized | DS11V2, DS11 |

There are no limits or quotas that focus specifically on Azure VMs running Linux; they are the same that were covered previously in Table 4.4. Microsoft fully supports Linux, and there are no policies or practices that knowingly inhibit this operating system.

## Azure VM Extensions

Extensions are small programs or automation activities that are helpful for performing post-deployment tasks, security, monitoring, and automated deployments. If you were curious in the previous exercises where we created the Azure VM, there was a tab name Advanced, and on that tab there was a section that allowed you to select an extension. In the exercises I usually skip over those tabs, but you may have looked at them and wondered what all those features are. Consider accessing the portal and simulating the creation of a new Azure VM. First, notice that the list of installable extensions is different when selecting a Linux or Windows-based VM, as well. The region plays a role in the extension list, so again, here is another example of knowing what capabilities are available in each region prior to committing to one.

For Windows there are some nice extensions such as the PowerShell Desired State Configuration extension that will help in post-deployment activities to make sure the VM is configured in the same way in every case. This is important once your workloads get rather complicated and require automated deployments, which are discussed in more detail in the next section. There are anti-malware, cloud security, and other security-related agents that can be deployed, configured, and run on your Azure VM as an extension. When you create your initial Azure VM, you configure all these environment-specific capabilities and then capture your image for use with later automated or manual deployments.

Microsoft provides a lot of monitoring capabilities; however, it fully supports other companies with more specific monitoring capabilities through this extension feature. Some third-party monitoring products available for installation are Datadog, APM Insight, and Dynatrace. Monitoring is covered in more detail in Chapter 9 but will focus on the Azure platform–based capabilities and not third-party extensions in IaaS. If you have an interest in learning more about these extensions, check out this online document:

```
docs.microsoft.com/en-us/azure/virtual-machines/extensions/overview.
```

## Automated Deployment

Deployment and migrations are covered in Chapter 8, which will target ARM and code deployments (aka content deployments). As you already know, there are many ways to deploy an application and many components that need to be deployed to make it work. If any portions of those deployment tasks can be automated, it decreases the amount of required effort. Consider in many of the previous exercises, after the provisioning of the Azure VM was complete, you were requested to connect via RDP or Bastion to the server and manually install IIS using some PowerShell cmdlets. That is an acceptable approach if you have only one or two servers to deploy that need IIS; however, if you were to deploy 50 or 100, then that option really isn’t worth considering. It is not realistic to manually log in to 50+ servers and make manual configurations to each of them. Exercise 4.6 is a similar approach to realize the same outcome, where you create an image and use it as the baseline for all future deployments. Using automated scripting is another option to consider and is useful when you deploy your Azure VMs with PowerShell as well. There are even scenarios when a combination of both these capabilities add great value.

An example of a scenario where both a custom image and an automated deployment script are useful is when there is no public image that has the required utilities installed to run your script. For example, if you wanted to run an Az PowerShell cmdlet, then the image must have those cmdlets installed prior to executing. This currently requires that the following PowerShell installation command be run first; you may remember this from the previous chapter.

```
Install-Module -Name Az -AllowClobber -Scope AllUsers
```

As mentioned in the previous section, there is an option on the Advanced tab of the Azure VM creation blade called Extensions. Clicking Select An Extension To Install opens a window to select an extension to install. The one used for executing custom scripts is named Custom Script Extension for Windows and Custom Script for Linux. When building a Windows OS VM, you can save the following PowerShell cmdlet to a file named, for example, iis.ps1 and upload it when configuring the build properties of the Azure VM in the portal.

```
Set-AzVMExtension `
    -ExtensionName IIS `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"Add-WindowsFeature `
        -name Web-Server -IncludeManagementTools"}'
```

Then, once the Azure VM is created, IIS will be installed using an extension. You can also install, for example, SQL Server and the .NET Framework using extensions. From a Linux VM perspective, in addition to the extensions, there is also the feature on the Advanced tab to implement cloud-init scripts, which can configure users and security and install software packages. The creativity customers and developers have within this tool is greatly varied, and the wonderful point about Azure is it provides a platform to realize that creativity. I recognize this section is a bit abstract. I simply point out this feature as an option to pursue and consider when you are deploying your application to an Azure VM.

You should now have a good understanding of how to create an Azure VM whether it be Windows or Linux. You should also, if asked, know which versions of those operating systems are endorsed and what trying to deploy an image that is not endorsed could entail. You should also know what the different categories/series of VMs mean, such as memory optimized and general purpose. Given a table that shows a list of different VMs with OS, CPU requirements, and memory requirements, you need to know which ones are endorsed by Azure and if the requested resources breach any quota or resource limits.

## Azure Dedicated Hosts

When you provision an Azure virtual machine, you receive the compute from a pool of existing virtual machines running on a host. The available pool of compute capacity is commonly used by all Azure customers. Be confident, however, that there is no content or configuration that remains behind after the deallocation occurs. The virtual machine is completely cleaned before being placed back into the pool of available resources. If you wanted or needed all of your virtual machines to run on its own host aka physical machine, which was not deallocated or provisioned from a pool of shared compute resources, then you can choose an Azure dedicated host. Visit this site for more information about this product offering:

docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts.

The cost of the Azure dedicated host is more than running in the shared hosting environment. This is because you would be charged for all the compute power available on the host instead of only the consumed compute power of the host. An advantage of using an Azure dedicated host is the ability to control any infrastructure change that may impact
