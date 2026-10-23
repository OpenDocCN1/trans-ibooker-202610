# TABLE 3.10 Azure Networking Limits (continued)

| Category | Resource | Default Limit |
|---|---|---|
| Load balancers | Load balancers | 1,000 |
| Express route | Circuits | 10 |
| Application gateway | Gateways | 1,000 |
| Traffic Manager | Profiles | 200 |
| Azure Firewall | Data throughput | 30 Gbps |

The limits are mostly the defaults and can be increased via a support ticket if more is required. The default settings are there to protect applications from running away with all the resources, which might indirectly impact other customers in the same region; this is commonly referred to as a noisy neighbor. The list provided in Table 3.10 is a summary. There are many more default limits. The limits change often, usually to increase the benefits to the customer.

```
docs.microsoft.com/en-us/azure/azure-subscription-service-limits#networking-limits
```

In the following sections there will be many exercises that will increase your knowledge of networking topics. The best way to learn is to configure and consume the products and features.

# VNet to VNet Integration

In Exercise 3.6 you will create a second VNet in a different region and resource group. This is referred to as peering. Actually, when VNets are connected to each other in different regions, it's called global VNet peering. When the VNets exist within the same region, it is simply VNet peering. Remember, as mentioned in Exercise 3.2, only resources in the same resource group as the VNet can exist within it. The resource group created in Exercise 3.2 was named CSHARPGUITAR-DB3-RG, and it is in the North Europe region. A summary of the resources contained in that resource group follows. These are the resources that the new VNet created here will connect to:

+   - **VNet:** CSHARPGUITAR-VNET-A
+   - **Subnet:** CSHARP (10.0.0.0/24)
+   - **Azure VM:** CSHARPGUITAR-VM
+   - **Region:** North Europe (DB3)