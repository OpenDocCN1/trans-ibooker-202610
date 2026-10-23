As with the Windows machines, the recommendations for the VM sizes are based on category and the operating system version. Please find the VM size recommendations per Linux OS flavor in Table 4.7. For greater visibility into the details of the Linux VMs instances, take a look at https://docs.microsoft.com/en-us/azure/virtual machines/linux/sizes-memory. As the amount allocated for compute resources increases, so does the cost; therefore, choosing the right size is important.

# TABLE 4.7 Linux Versions to Azure VM Size Recommendation

| Linux version | Category | Instance/Series |
|---|---|---|
| Ubuntu CoreOS | General Purpose | DS1V2, DS2V2, D2SV3, D4SV3, D1V2, D1, DS1, DS2 |
|  | Memory Optimized | DS11V2, E2SV3, DS11 |
| Red Hat Enterprise | General Purpose | DS2V2, D2SV3, D4SV3, D2V2, D2V3, D2, DS2 |
| Clear Linux OS | General Purpose | D1, D3, DS3, DS4 |
| SUSE Linux Enterprise | General Purpose | DS1V2, DS2V2, D1V2, D1, Ds1, DS2 |
|  | Memory Optimized | DS11V2, DS11 |

There are no limits or quotas that focus specifically on Azure VMs running Linux; they are the same that were covered previously in Table 4.4. Microsoft fully supports Linux, and there are no policies or practices that knowingly inhibit this operating system.

# Azure VM Extensions

Extensions are small programs or automation activities that are helpful for performing post-deployment tasks, security, monitoring, and automated deployments. If you were curious in the previous exercises where we created the Azure VM, there was a tab name Advanced, and on that tab there was a section that allowed you to select an extension. In the exercises I usually skip over those tabs, but you may have looked at them and wondered what all those features are. Consider accessing the portal and simulating the creation of a new Azure VM. First, notice that the list of installable extensions is different when selecting a Linux or Windows-based VM, as well. The region plays a role in the extension list, so again, here is another example of knowing what capabilities are available in each region prior to committing to one.

For Windows there are some nice extensions such as the PowerShell Desired State Configuration extension that will help in post-deployment activities to make sure the VM is