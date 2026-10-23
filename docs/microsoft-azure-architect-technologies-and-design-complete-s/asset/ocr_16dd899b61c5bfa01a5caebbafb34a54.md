In the next section we discuss subnets, but you couldn’t have missed that one of the required fields for the VNet was the CIDR subnet address range. Generally, the default address range for a subnet in Azure is 10.0.0.0/24, which means the available IP address range is from 10.0.0.0 to 10.0.0.255. That range of IP addresses is commonly referred to as a virtual network private address space. These IP addresses provide an additional means to identify the resources within a VNet. As an administrator, you know which Azure resources exist in which VNet and what range of IP addresses they have and can therefore create restrictions based on the resources within that range of IP addresses.

In Exercise 3.2, you selected Microsoft.Web, which means the VNet will delegate support to Azure App Service because that is the resource provider to which Azure App Service instances are bound. There were numerous others like Microsoft.EventHub, Microsoft.KeyVault, Microsoft.AzureCosmosDB, and Microsoft.Sql. Why service endpoints are so powerful is that once you configure one on an Azure resource that is not deployed to an Azure VNet, then only IP addresses from the subnet (which are private IP addresses) will be allowed to connect to that resource. This is shown in Figure 3.9.

# FIGURE 3.9 Connectivity between a VNet and Azure resources using a service endpoint

![](bbox=[0.15, 0.46, 0.85, 0.83])

Also, no access from the internet is permitted because the IP addresses won’t be in that range either. I want to call out the statement “from the configured VNet.” The question is,