Since much of the C2 implant’s execution depends on the VM, we will not be focusing much on it in this chapter. However, keep it in mind as an option since we are talking about different persistence methods that could allow for command execution on VMs.

# Opening management ports

As an alternative to using C2 implants on virtual machines, we can open management ports to our IPs to remotely access the VMs with credentials that we know. As we mentioned previously, there are several ways to get (or set) local administrator credentials for a VM, and we can use those credentials to remotely manage the system.

This is not an ideal solution as opening ports to the internet is generally a bad practice during a penetration test. Additionally, opening up a port to a specific IP address will indicate your IP address to an incident response team, which will make it pretty easy for them to sort out attribution to your system. This method can also be extended to Azure PaaS services (Azure SQL databases, key vaults, and more), but this is a particularly noisy method of maintaining access, so we would recommend avoiding it unless you really need to use it.

Another less noisy option is to specify an Azure service tag as the source when opening the port in the Network Security Group. Service tags allow us to define less suspicious backdoors. For example, modifying a network security group to allow RDP or SSH traffic from the AzureCloud service tag or the AzureCloud.UKSouth service tag could seem less suspicious to a security operations team, while still allowing connections from any Azure node whose range matches the specified source.

# Adding a managed identity

By adding a managed identity (system-assigned or user-assigned) to a VM, we can potentially grant ourselves access to additional Azure resources in the tenant for persistence. This would require us to go through the following steps:

+   1. Navigate to our target VM in the Azure portal.
+   2. Assign a system-assigned, or user-assigned, managed identity to the VM.
+   3. Assign additional rights to the managed identity at the subscription/management group and/or Azure AD levels. From a pentesting perspective, we need to be careful not to add permissions that make the environment vulnerable to wider attacks.