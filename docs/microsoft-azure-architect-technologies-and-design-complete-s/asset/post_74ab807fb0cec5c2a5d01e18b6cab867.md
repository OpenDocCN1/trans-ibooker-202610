
## EXERCISE 3.5 (continued)

d. Enter the network interface. It should default to the one bound to the Azure VM.
e. For the source IP address, enter the private IP address of CSHARPGUITAR-VM1, for example, 10.0.1.4.
f. For the local port, enter 5555.
g. For the destination IP address, enter 10.0.0.4, which is the private IP address of the Azure VM created in Exercise 3.3 (for example, CSHARPGUITAR-VM).

6. Click the Next Hop button. Notice, as shown in Figure 3.24, that the output shows the next hop type that is used to route the request. You can then confirm that the address prefix for the given next hop type is the one expected. If not, you can add a user-defined route.

### Figure 3.24 The next hop type is used to route a request from the source IP address to the destination IP address.

![](img/95043b3df926bcdadadb082b49262037_195_0.png)

Later in this chapter, and in many places throughout this book, you will learn more about routing, networking, and Azure VMs.

## Azure Networking Patterns

There are some fairly common Azure networking patterns that customers typically implement. Here is a short list of the most popular ones:

- Isolated
- Hybrid network
- DMZ
- Hub and spoke

An isolated network pattern is like the one created in Exercise 3.2. It was illustrated in Figure 3.17 and is the simplest form of network to implement. It is simply a single VNet with connectivity to the internet. It might encounter limitations as the consumption and deployment of workloads into it increase. Azure networking limits are discussed in the next section, but keep in mind that this networking pattern is usually where customers begin. Knowing the other patterns will help you configure and design with this growth in mind.

Hybrid networks, as discussed earlier in this chapter, can be visualized by reviewing Figure 3.2. This is simply the connectivity between on-premise compute or data resources with the same or similar resources contained within an Azure VNet. (Hybrid connections will be discussed later in this chapter and will cover some additional details about forced tunneling.)

A demilitarized zone (DMZ) is an approach that adds a layer of security around your deployed workloads. As shown in Figure 3.25, notice that, in this pattern, Azure VMs no longer have public IP addresses. They cannot be accessed from the internet or from on-premise directly. To make the Azure VMs reachable from another network like an on-premise network, a VPN gateway, an application gateway (web application firewall [WAF]), or a load balancer will be configured in front of them. Those network hardware components are often referred to as a *network virtual appliance (NVA).* NVAs can restrict and route traffic for the Azure VMs configured behind it.

The hub and spoke pattern is a variation of the DMZ where the routing can span multiple subscriptions, VNets, DMZs, and/or workloads. As shown in Figure 3.26, the internet traffic has been routed to a specific WAF. Perhaps there needs to be a different set of NSGs applied for connectivity via the internet when compared to connectivity coming from an on-premise endpoint. Then, from the WAF, there are three different VNets represented. Each could have their own subscription. In another scenario, the VNets may play a specific role for the deployed solution.

One reason for splitting VNets into different subscriptions is the separation of costs. Production, testing, and development environments might need to separately monitor usage, data storage, and costs. Assume that the VNets in the previous figure represent just that. This pattern can also provide an isolation perspective. The three environments—production, testing, and development—can be completely isolated from each other. It is important that any kind of performance testing on a test environment does not impact the production system. There are many different patterns, designs, and approaches. It really depends on what your application requires for security and necessary capabilities. The point is to keep in mind that the first default VNet you create will likely not be the end solution. Consider all the options and don’t knowingly implement any pattern that can restrict your growth. You can achieve this by having the long-term pattern for your application in mind at design time and building the VNet and surrounding infrastructure with that in mind.

### Figure 3.25 An Azure DMZ networking pattern

![](img/95043b3df926bcdadadb082b49262037_197_0.png)

### Figure 3.26 An Azure hub and spoke networking pattern

![](img/95043b3df926bcdadadb082b49262037_198_0.png)

## Azure Networking Limits

Sometimes, there is a misconception that the cloud equals limitless capacity and bandwidth. Those with that perception will quickly discover that limitless capacity and bandwidth are not realistic in any computing environment. The cloud offers great capacity and bandwidth with very fast scale-out and up times, but compute capacity (hardware) and bandwidth are not infinite. Table 3.10 describes some of these limits within the Azure Networking context per region and per Azure subscription.

### Table 3.10 Azure Networking Limits

| Category | Resource | Default Limit |
|---|---|---|
| Networking | VNets | 1,000 |
| Networking | Subnet per VNet | 3,000 |
| Networking | Private IP address per VNet | 65,536 |
| Networking | Network security groups | 5,000 |
| Networking | User-defined route tables | 200 |
| Public IP addresses | Dynamic | 1,000 |
| Public IP addresses | Static | 1,000 |
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

## VNet to VNet Integration

In Exercise 3.6 you will create a second VNet in a different region and resource group. This is referred to as peering. Actually, when VNets are connected to each other in different regions, it's called global VNet peering. When the VNets exist within the same region, it is simply VNet peering. Remember, as mentioned in Exercise 3.2, only resources in the same resource group as the VNet can exist within it. The resource group created in Exercise 3.2 was named CSHARPGUITAR-DB3-RG, and it is in the North Europe region. A summary of the resources contained in that resource group follows. These are the resources that the new VNet created here will connect to:

- **VNet:** CSHARPGUITAR-VNET-A
- **Subnet:** CSHARP (10.0.0.0/24)
- **Azure VM:** CSHARPGUITAR-VM
- **Region:** North Europe (DB3)

Although the VNet can also be in another subscription, in this scenario, both VNets will be in the same subscription but will exist in a different region and resource group.

Before starting the exercise, there is a point that needs to be discussed. When you created the first VNet in Exercise 3.2, the portal logic provided the address space for the VNet and address range of the subnet. Are those defaults enough for you? Might you need more than 65,536 addresses for the VNet and about 256 addresses for the subnet? How do you calculate the optimal CIDR even if you know the number you need? Well, remember in Chapter 2 when we discussed conditional access policies? I specifically mentioned that you need to know which operating systems your customer or employees use, as well as the locations they commonly use for access, to effectively create the policy. How many IP addresses your solution requires isn’t something Azure or Microsoft can anticipate, so you need to know the answers to these questions, just like you need to know your access control policy requirements. You can find a table that shows how many IP addresses are made available per CIDR prefix here:

```
en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
```

Keep in mind that Azure needs four to five IP addresses to manage the infrastructure per subnet, but this matters only if you are creating very small subnets. A mathematical equation to calculate how many IP addresses exist per CIDR prefix is 2 to the power of (address length minus prefix length). For IPv4, the address length is always 32. For example, if the prefix is /29, the equation is 2^(32−29) = 2^3 = 8 addresses. In Exercise 3.6, the default range and space are used, but keep in mind to ask or at least think about whether these will be enough. Be aware that you can only change the number of addresses by deleting the VNet or if the VNet is empty.

## EXERCISE 3.6

### Configuring Global VNet Peering

1. Log in to the Azure Portal at portal.azure.com.
2. In the search box at the top of the portal, enter Resource Groups, click Resource Groups from the drop-down, and then click + Add.
3. Select the desired subscription and provide a different resource group name. In this example, I create the resource group named CSHARPGUITAR-SN1-RG. SN1 is the designation for South Central US. You can name the resource group as desired; however, make sure it is not in the same region as the VNet created in the earlier exercise.
4. In the search text box at the top of the portal, enter Virtual Networks, click Virtual Networks in the drop-down list, and choose + Add.
5. Enter CSHARPGUITAR-VNET-B for the VNet name. Take note of the default address space for the VNet (10.1.0.0/16) and the default address range for the subnet (10.1.0.0/24). Select the subscription and resource group as done in step 3, set the subnet name to csharp, leave the defaults, and then click the Create button.
