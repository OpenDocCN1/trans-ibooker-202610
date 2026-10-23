# FIGURE 3.11 The network flow of a point-to-site connection

![](bbox=[0.21, 0.16, 0.84, 0.45])

# Network Traffic Filtering

The most common or popular means for filtering traffic on Azure, and likely on-premise, uses NSGs (aka security rules). As shown in Figure 3.10, an NSG restricts (filters) inbound and outbound traffic based on a set of security rules. Another Azure feature that is helpful for filtering network traffic is an application security group (ASG). An ASG provides the ability to group a set of Azure VMs and define NSGs specifically for that group. Take Figure 3.12, for example. Notice that all three Azure VMs exist in the same subnet; two of them are associated with an ASG named CSHARP-ASG, and one is associated with an ASG named GUITAR-ASG.

Look again at Figure 3.10 and notice the Destination column. Later when you create an NSG and an ASG, you will find that the supported values for Destination are Any, IP Addresses, VirtualNetwork, and of course Application Security Group. And just like any NSG, the rule will be applied when a request to that destination is sent. For example, you want to allow inbound internet traffic on both the ASGs, but for Azure VMs in the CSHARP-ASG, only HTTPS (port 443) is supported. In that case, the NSG would deny HTTP (port 80) when the destination is CSHARP-ASG. That security rule would not apply to the Azure VM in the GUITAR-ASG.

# NSGs and ASGs for App Services, Azure Storage, and More

Many Azure resources that are not deployed into an Azure VNet do have some capability for restricting access based on IP address ranges or configured service endpoints. Those capabilities can be found as a feature for a resource in the Azure Portal.