
10.0.255.255 and is a total of 65,535 unique addresses. When the subnet is created, you must then also select its address range, which may be something like 10.0.0.0/24. All of this is covered in the “VNets and Subnets” section later in the chapter. The point is that the subnet’s IP address range is within the VNet’s IP address range. Azure resources are added to the subnet and are the method where their segmentation is achieved. It is possible to have multiple subnets per VNet.

The Azure resources contained within a VNet probably need to communicate with resources outside the subnet. Azure VNets provide many options for securely connecting to resources outside the subnet. Azure resources within a subnet can connect using the following capabilities, all of which are discussed in detail later in this chapter:

- VNet to VNet
- VNet to on-premise
  - Site-to-site (VPN gateway)
  - ExpressRoute
- VNet to Azure resource not in a VNet
  - Directly using the global endpoint
  - Using subnet delegation and service endpoints
- VNet to internet

If the configuration allows access, no mainstream or semi-mainstream scenario prevents resources within an Azure VNet from connecting with other networks accessible over the internet, assuming the configuration to allow the access has been made.

When it is determined that connectivity with a resource in another location is required, you might want to use a host name instead of the IP address. In this case, Azure provides two options. The first is to use the Azure-provided name resolution services. It should be already understood that DNS is the service that provides the translation of a host name to an IP address. Without DNS there would be no domain names, and accessing a website would require the client to know the IP address, which is not so friendly. If, however, how DNS works is not clear, then you can get up to speed in the “Using Azure DNS” section. The Azure platform also provides the capability to create, configure, and manage your own DNS instance on an Azure VM within a VNet.

From a security perspective, which is of upmost importance, *network security groups (NSGs)* control access into (inbound) and out of (outbound) each subnet using NSGs.

## Where Are NSGs Bound by Default for Azure VMs?

By default, an NSG is bound to the network interface card of the Azure VM. It is recommended that this default setting is changed and the NSG bound to the subnet instead. You can see how to do this in Exercise 3.9 later in this chapter.

An NSG can restrict traffic based on a protocol, port, and specific IP address. This is a tight and secure way to manage what can access the resources within the subnet. Outbound traffic can be controlled, as well. The outbound request originates from one of the IP addresses in the subnet, and it will pass any restrictions placed elsewhere; therefore, making sure the traffic is flowing over the expected port using the expected protocol could also be warranted. This is covered in greater detail in the “Network Security” section.

Once the VNet is configured, you can sleep well knowing that it is being monitored and supported by Microsoft. To fully understand this complexity, you need to look at this from an Azure (or any other cloud hosting company) perspective. Neither entity can predict when a customer creates a VNet if it is for fun, for testing, or if it will be a full-blown data-intensive bandwidth-hungry monster. From that perspective. Microsoft has you covered from three perspectives. The first is that the network is monitored for bandwidth and will scale as your workload requires it. Second, if your workload is in a section of the data center with other customers and they start consuming massive amounts of network capacity, it gets handled. Finally, the network is constantly monitored from a health and wellness perspective and gets auto-healed when telemetry-based thresholds are breached.

## Connecting with Other Azure Resources

Recall from Figure 3.3 that some Azure resources must reside within a VNet, some can exist in a VNet, while other Azure resources cannot exist in a VNet or do not by default. Table 3.4 provides a compact view of which Azure resources can/cannot be deployed into an Azure VNet. As this list changes frequently, conduct a search on the internet for the most current information. Table 3.4 contains those items most likely to be covered when taking the Azure Solutions Architect Expert exam.

### TABLE 3.4 Azure Resource VNet Support

| Type | Azure Resource | VNet Deployable |
|---|---|---|
| Web | Azure App Service | × |
| Compute | Azure VM | ✓ |
| Networking | Azure Application Gateway (web application firewall) | ✓ |
| Web | Azure Logic Apps | ✓ |
| Security | Azure Key Vault | × |
| Data | SQL Azure | × |
| Web | API Management | ✓ |
| Networking | Azure Firewall | ✓ |

### TABLE 3.4 Azure Resource VNet Support (continued)

| Type | Azure Resource | VNet Deployable |
|---|---|---|
| Containers | Azure Kubernetes Services (AKS) | ✓ |
| Data | Azure Cosmos DB | × |
| Web | App Service Environment | ✓ |
| Storage | Azure Storage | × |
| Data | Managed Azure SQL | ✓ |
| Messaging | Azure Event Hubs | × |

This doesn’t mean these Azure resources cannot connect with each other simply because one exists within a VNet while the other doesn’t. It means that if there is a need for it, additional actions are required to make it happen. Take, for example, a common scenario where there is a web application with a SQL server database backend. There are numerous scenarios that one can design to deploy this, as depicted in Figure 3.7.

- App Service web app and managed SQL Azure instance
- Azure VM and managed SQL Azure instance
- Azure VM and an on-premise backend data source
- App Service web app and an on-premise backend data source
- App Service web app and SQL Azure

As listed in Table 3.4, an App Service instance does not exist within a VNet, while a managed SQL Azure instance does. Remember from Chapter 1 that a managed SQL Azure instance is the full version of the SQL Server DBMS running on an Azure VM. Since Azure VMs must run within a VNet, so must a managed SQL Azure instance. Also mentioned in Chapter 1, an Azure App Service instance runs within a sandbox. This means things like spawning child processes or writing to the Windows Registry are not supported. If your web application requires unsupported capabilities on an App Service instance, then an option could be to run it on an Azure VM. An Azure VM can connect to a managed SQL Azure instance or an on-premise backend data source. It is also possible to connect an Azure App Service instance to an on-premise data source and an Azure SQL database that is not in a VNet.

## FIGURE 3.7 Different web application/database solutions

![](img/95043b3df926bcdadadb082b49262037_172_0.png)

Each of those solutions must be deliberated and a decision made based on which solution bests meets the requirements of the application. An important point about running an Azure resource that is not in a VNet is that the resource exposes a global endpoint, which means it is accessible to the internet and suggests that routing is likely different. In addition, some additional security questions arise. Indeed, routing is different; however, the Microsoft networking infrastructure is intelligent enough to determine whether the resource being requested is an Azure one. If it is, the traffic takes more hops, but it remains on the Microsoft backbone. Additionally, having the endpoint of an internal or highly sensitive website globally identifiable is not something every customer would want. Although there are steps that can be taken to secure the endpoint, like a WAF or IP restrictions, why expose that attack vector for no reason? There have been many cases where that is a red flag, which is an immediate showstopper for many customers.

A solution could be an internal load balancer App Service Environment (ILB ASE) or an Azure VM to run the web application and restrict the access using an NSG. But that is not all; there is another, more recent feature called virtual network service endpoints. Before we discuss service endpoints in detail, let’s do an exercise. In Exercise 3.2 you will create an empty Azure VNet and enable service endpoints.
