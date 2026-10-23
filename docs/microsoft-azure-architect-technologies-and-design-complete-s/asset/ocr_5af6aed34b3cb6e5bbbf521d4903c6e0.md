# FIGURE 8.4 Recovery Services vault

![](bbox=[0.14, 0.14, 0.86, 0.54])

# Migrating Azure Virtual Machines

In Exercise 3.3 you created your first Azure VM. You did this in Chapter 3 because it became a little complicated to discuss virtual networks, subnets, and NSGs without having something to apply those concepts to. You can have a virtual network without Azure VM in it, but you cannot have Azure VM without a virtual network. Therefore, before you attempt to migrate an on-premise workload to an Azure virtual machine, you will need a virtual network and a subnet like the one you created in Exercise 3.2. It is certainly possible to run a vast variety of both Windows Server and Linux versions on Azure VM. The source location of those on-premise machines can be running on VMware, Hyper-V, Azure Stack, and even AWS. Yes, there is a documented procedure that walks you through moving your IaaS workloads from AWS to Azure. Why not? The point here is that each of those sources requires some different steps to prepare and migrate. Most of them were discussed and provided in the previous chapters and discussions. For a hands-on example of migrating and deploying a single Hyper-V virtual machine instance to an Azure virtual machine, continue to the next section.

# Hyper-V/VMware

Both Hyper-V and VMware are server virtualization management software. These products allow you to allocate a specific number of CPUs, an amount of Memory, an amount of hard