
# Azure Load Balancer Configuration

## Overview and Tier Selection

The Azure Load Balancer has two tiers, with differing functionality. Basic is the cheaper of the tiers, with more limited functionality, and Standard being the higher of the tiers. Because our servers are in Availability Zones, we must use a Standard Load Balancer.

## Creating the Load Balancer

To create the Load Balancer, navigate to the Load Balancers blade in Azure and select the Add button, to create a new Load Balancer. In the first section of the Create Load Balancer page, shown in Figure [7-13](#394392_3_En_7_Chapter.xhtml#Fig13), we specify the Subscription and Resource Group that we want to create the Load Balancer in. This should match the Resource Group that your VMs are in. ![Figure 7-13 ¶ Create a Load Balancer (Part 1)](images/394392_3_En_7_Chapter/394392_3_En_7_Fig13_HTML.jpg)

Figure 7-13 ¶ Create a Load Balancer (Part 1)

We then provide a name for the Load Balancer and specify the region it should be created in. This should match the location of your VMs. We also select the tier of Load Balancer that we wish to create.

## Network Configuration

In the next section of the page (Figure [7-14](#394392_3_En_7_Chapter.xhtml#Fig14)), we specify the network details for the Load Balancer. Specifically, we specify the vnet that it should reside in, which should match the vnet of your VMs. We also specify if it should use a static or dynamic IP Address. For our purposes, we need a static IP Address, so we need to specify what that address should be. Finally, we specify if the public IP address of the Load Balancer should be created in an Availability Zone, or if it should use a zone-redundant path. ![Figure 7-14 ¶ Create a Load Balancer (Part 2) ](images/394392_3_En_7_Chapter/394392_3_En_7_Fig14_HTML.jpg)

Figure 7-14 ¶ Create a Load Balancer (Part 2)

On the Tags page, we can specify key/value pairs in line with company policies, before reviewing our configuration on the Review page.

## Backend Pool

We now need to create a Backend Pool for the Load Balancer. This can be achieved by navigating to the Backend Pools tab of the Load Balancer’s blade in the Azure Portal and using the Add button, to invoke the Add Backend Pool page, shown in Figure [7-15](#394392_3_En_7_Chapter.xhtml#Fig15). ![Figure 7-15 ¶ Add a Backend Pool](images/394392_3_En_7_Chapter/394392_3_En_7_Fig15_HTML.jpg)

Figure 7-15 ¶ Add a Backend Pool

## Health Probe

We can now create a Health Probe, which will perform connectivity checks to each node. We can do this by navigating to the Health Probes tab on the Load Balancer’s blade, shown in Figure [7-16](#394392_3_En_7_Chapter.xhtml#Fig16). Here, we are specifying a name for the probe and providing a protocol and unused port that the health check will use to connect to the nodes. We then specify how frequently the health check should run and how many concurrent failures result in a node as being marked unhealthy. If a node is marked as unhealthy, the Load Balancer will not send it any requests. ![Figure 7-16 ¶ Add a Health Probe](images/394392_3_En_7_Chapter/394392_3_En_7_Fig16_HTML.jpg)

Figure 7-16 ¶ Add a Health Probe

## Load Balancing Rule

Next, we will configure a new load balancing rule by using the Add button on the Load Balancer Rules tab of the Load Balancer blade. In the first section of this blade (Figure [7-17](#394392_3_En_7_Chapter.xhtml#Fig17)), we will specify a name for the rule, whether the Load Balancer should use IPv4 or IPv6. The Frontend IP address is the IP Address of the Load Balancer that we configured earlier. HA ports allow all ports on the backend pool to be load balanced, but we don’t want to do this, so we will leave the box unchecked and select TCP as the protocol. ![Figure 7-17 ¶ Configure a Load Balancing Rule (Part 1)](images/394392_3_En_7_Chapter/394392_3_En_7_Fig17_HTML.jpg)

Figure 7-17 ¶ Configure a Load Balancing Rule (Part 1)

In the second section of this page, shown in Figure [7-18](#394392_3_En_7_Chapter.xhtml#Fig18), we will configure the port that the Load Balancer will be listening on, followed by the port that the servers in the backend pool will be listening on. In our case, this will be port 1433 for both. We will choose the backend pool of servers and the health probe that we just created. ![Figure 7-18 ¶ Configure a Load Balancer Rule (Part 2) ](images/394392_3_En_7_Chapter/394392_3_En_7_Fig18_HTML.jpg)

Figure 7-18 ¶ Configure a Load Balancer Rule (Part 2)

Session persistence specifies how successive requests from clients should be handled. If we select Client IP, then successive requests from the same IP address will be handled by the same server. If we choose Client IP and protocol, then successive requests from the same client will be handled by the same server, but only if the same protocol is used to connect. For Availability Groups, however, session persistence is not required, so we will choose None. This indicates that successive requests by the same client can be handled by any server in the backend pool.

The Idle Timeout indicates the minimum length of time that a connection will be kept open, without clients sending messages, and we will leave TCP reset disabled. Finally, we will select Floating IP, which changes the IP address mapping schema, allowing for multiple listeners, which is required for the Availability Group Listener.

You should repeat this process to add a frontend port and load balancing rule for the core Cluster.

## Availability Group Listener

We will then be in a position where we can configure our Availability Group Listener. We will do this from within the Cluster Manager. From the context menu of the Sales Role, drill through Add Resource and choose Client Access Point. This will cause the New Resource Client Access Point wizard to be displayed. On the first page of this wizard, provide a name for the Client Access Point, and assign an IP Address. Because we created each VM on a different subnet, we will need to provide an IP Address for each subnet, as shown in Figure [7-19](#394392_3_En_7_Chapter.xhtml#Fig19). ![Figure 7-19 ¶ Create New Client Access Point – Client Access Point Page](images/394392_3_En_7_Chapter/394392_3_En_7_Fig19_HTML.jpg)

Figure 7-19 ¶ Create New Client Access Point – Client Access Point Page

When we move to the confirmation page of the wizard, our network settings will be validated. The Confirmation page displays a summary of what we are going to configure.

Once the Client Access Point has been created, we should use the Dependencies tab of Properties dialog box, of the Availability Group resource (shown under Other Resources in the Resource tab of the role) to add a dependency on the Client Access Point, as shown in Figure [7-20](#394392_3_En_7_Chapter.xhtml#Fig20). ![Figure 7-20 ¶ Add a Dependency](images/394392_3_En_7_Chapter/394392_3_En_7_Fig20_HTML.jpg)

Figure 7-20 ¶ Add a Dependency

You should also repeat this process to make the Client Access Point dependent on the IP Address resources.

## Cluster Integration with Load Balancer

Next, we need to configure the Cluster to work with the Azure Load Balancer. We can do that using the script in Listing [7-2](#394392_3_En_7_Chapter.xhtml#PC2). This script updates each of the IP Address resources, with the Load Balancer IP and health probe port.

**Note**
Be sure to update the script to match your own configuration before running it.

```powershell
$ClusterNetworkName = 'AlwaysOnAzure'
$LoadBalancerIP = '10.0.10.10'
[int]$LoadBalancerProbePort = 50001
Get-ClusterResource 'IP Address 10.0.0.35' | Set-ClusterParameter -Multiple @{"Address"="$LoadBalancerIP";"ProbePort"=$LoadBalancerProbePort;"SubnetMask"="255.255.255.0";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
Get-ClusterResource 'IP Address 10.0.0.35' | Set-ClusterParameter -Multiple @{"Address"="$LoadBalancerIP";"ProbePort"=$LoadBalancerProbePort;"SubnetMask"="255.255.255.0";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
```

Listing 7-2
Integrate the Cluster with the Load Balancer

**Tip**
