
The way forced tunneling works is through the configuration of the user-defined routing table entries. Take a look back at Figure 3.13. By default any destination IP address that does not match an existing route defaults to the address prefix of 0.0.0.0/0 and is assigned a NextHopType of Internet. This means that even after the configuration of an S2S connection and the VPN gateway, traffic to the internet from an Azure VM hosted in the VNets will go directly to the internet. To force that traffic through the VPN gateway, the S2S connection must be set to the -NextHopType for 0.0.0.0/0 to VirtualNetworkGateway.

There currently is no Azure Portal interface to perform this action. The recommended way to set the NextHopType value is to use PowerShell. The following PowerShell script adds forced tunneling to the S2S VPN gateway:

```powershell
New-AzRouteTable -Name "CSHARPGUITAR-DB3-ROUTE-TABLE" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG" \
    -Location "North Europe"

$rt = Get-AzRouteTable -Name "CSHARPGUITAR-DB3-ROUTE-TABLE" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG"

Add-AzRouteConfig -Name "ForcedTunnelRoute" \
    -AddressPrefix "0.0.0.0/0" \
    -NextHopType VirtualNetworkGateway \
    -RouteTable $rt

Set-AzRouteTable -RouteTable $rt

$vnet = Get-AzVirtualNetwork -Name "CSHARPGUITAR-VNet-A" \
    -ResourceGroupName "CSHARPGUITAR-DB3-RG"

Set-AzVirtualNetworkSubnetConfig -Name "csharp" -VirtualNetwork $vnet \
    -AddressPrefix "10.0.0.0/24" \
    -RouteTable $rt

Set-AzVirtualNetwork -VirtualNetwork $vnet
```

You can find this PowerShell script here:

github.com/benperk/ASA/blob/master/Chapter03/ForcedTunnelRoute.ps1

It is not possible to change the default routing rules provided by Azure. To implement any kind of change, it is necessary to create a new routing table and add, update, or delete routes from there. This is what the previous PowerShell script does. Once the routing table named CSHARPGUITAR-DB3-ROUTE-TABLE is created, it is bound to the csharp subnet, which exists in the CSHARPGUITAR-VNET-A virtual network. Remember that routes with a Source value equal to User take a higher precedence over a route with the same prefix created by default. This is the case here since after adding the previous route for the VirtualNetworkGateway there would be two routes with an AddressPrefix value of 0.0.0.0/0.

Execute the next PowerShell cmdlet to see the effective routes after the addition of the route that implemented forced tunneling:

```powershell
Get-AzEffectiveRouteTable -NetworkInterfaceName csharpguitar-vm810 `
-ResourceGroupName CSHARPGUITAR-DB3-RG | Format-Table
```

Figure 3.60 provides the output of the cmdlet.

**Figure 3.60 Forced tunneling route table example**

| Name | DisableBgpRoutePropagation | State | Source | AddressPrefix | NextHopType | NextHopIpAddress |
|---|---|---|---|---|---|---|
|  | False | Active | Default | {10.0.0.0/16} | VnetLocal | {} |
| ForcedTunnelRoute | False | Active | VirtualNetworkGateway | {10.5.0.0/24} | VirtualNetworkGateway | {40.115.104.24} |
|  | False | Active | VirtualNetworkGateway | {0.0.0.0/0} | VirtualNetworkGateway | {} |
|  | False | Invalid | Default | {0.0.0.0/0} | Internet | {} |
|  | False | Active | Default | {10.1.0.0/16} | VnetGlobalPeering | {} |

Lastly, recognize in the PowerShell script that the subnet to which the forced tunnel is applied was provided. Recall that CSHARPGUITAR-VNET-A has two subnets, csharp and guitar. The script applied the forced tunnel route only to the csharp subnet and therefore to all the Azure VMs within that subnet. It wasn’t applied to the guitar subnet. That means that although the VPN gateway is bound to CSHARPGUITAR-VNET-A, routes are applied to the subnet, and the Azure VMs in the guitar subnet will have internet traffic routed straight to the internet. They are not forced tunneled through the S2S connection through the on-premise network.

## Additional Azure Networking Products

The remainder of this chapter covers some networking products that haven’t fit into any other previous discussion points. These are networking-like products but are not always mentioned when discussing networking. The additional networking components covered here are as follows:

- Azure Application Gateway (not WAF)
- Azure Load Balancer
- Azure Front Door
- Azure Content Delivery Network
- Traffic Manager
- Azure Relay/Hybrid Connection Manager

The first product is Application Gateway, which has been discussed previously; however, only the WAF portion of it was discussed. Application Gateway provides many more capabilities other than a WAF.

### Application Gateway

Since the late 1990s running a web application on multiple servers behind some kind of network balancing device has been the norm. This architectural configuration is referred to as a *web farm*. It makes a lot of sense to have multiple servers running the same application from a redundancy perspective. There is no single point of failure. Since the 1990s those network balancing devices have become much more sophisticated and have enhanced their capabilities to more than just a simple device that directs traffic to a group of servers configured behind it.

The web application firewall capability found in the Application Gateway product is world-class. It provides network capabilities primarily focused on OSI level 7, which means it can make routing decisions based on attributes contained within HTTP requests such as request header values and requested resource path. This provides additional capabilities over other network appliances that are restricted to the content available in a socket (source IP, source port, destination IP, destination port).

In addition to a WAF, the Application Gateway virtual appliance can act as a load balancer, an SSL termination point, and a redirector, plus many other features. Many of those additional features are discussed in more detail. If you have not already completed Exercise 3.10, consider doing it now. The following content expects that you have some knowledge of what Application Gateway is and how it is configured.

### SSL Termination

*Secure Sockets Layer (SSL)* is the old, legacy name for a procedure that encrypts the transmission of data between a client and a server using the HTTPS protocol. These days *Transport Layer Security (TLS)* is the proper term. It encrypts the same as SSL but is more secure. SSL is the most common term used; it is probably OK to continue using it because people will know what you’re talking about, but the days of SSL are over. The encryption/decryption of data between a client and server does have some overhead in terms of resource utilization. At some point between the two, there may be a device that is configured to terminate the secure connection, which will eliminate encryption/decryption, resulting in faster transactions and less resource consumption.

The termination of this secure connection is called *SSL termination* or *SSL offloading*, and it usually happens on an edge device like a gateway, intelligent firewall, or frontend server. By edge device, I mean a network device that sits at the entry point of a network, and after the transmission enters the network, encryption is no longer necessary. It is assumed that the network is secure, and therefore the infrastructure doesn’t need to take the additional performance hit and overhead of SSL decryption communicating in a secure tunnel once inside. In addition to the improved performance, the termination of the secure connection and the decryption of the request content provide opportunities to route the request more intelligently. Attributes such as headers, the URL, cookies, and the like can be used for routing rules and logic.

Application Gateway provides the capability for both SSL termination and end-to-end SSL encryption. Some customers or applications may require encryption from end to end. Applications with highly sensitive scope or those that transmit highly sensitive data may justify this encryption type when it’s required for compliance reasons. What is really neat about Application Gateway is that, even with end-to-end encryption, it is still possible to access the request attributes. Application Gateway still terminates the SSL session upon arrival; however, it encrypts the outbound traffic again before sending it onto the backend pools. Recall from Exercise 3.10 where you created a backend pool (for example, CSHARPGUITAR-WAF-BACKEND-POOL) and added an Azure VM (for example, CSHARPGUITAR-V2) to it. Configure SSL termination in Exercise 3.13 using the application gateway created previously in Exercise 3.10 (for example, CSHARPGUITAR-SN1-AGWAF).

### EXERCISE 3.13: Configuring SSL Termination on an Application Gateway

1. Log in to the Azure Portal at portal.azure.com.
2. Navigate to an existing application gateway (for example, CSHARPGUITAR-SN1-AGWAF), select Listeners from the navigation menu, and click + Basic, which renders the Add Basic Listener blade.
3. Enter a name (for example, CSHARPGUITAR-LISTENER-HTTPS), enter port_443 for the Frontend Port name, enter 443 as the port, and select the HTTPS protocol.
4. Upload the csharpguiternet.pfx certificate, downloaded from github.com/benperk/ASA/tree/master/Chapter03/Ch03Ex12. Or create your own using the New-SelfSignedCertificate PowerShell cmdlet.
5. Provide the name (for example, CSHARPGUITARNET), enter the password Csh@rPguitarN3t, and click the OK button. The form should resemble something similar to Figure 3.61.
6. Next add an HTTP setting. Click the HTTP settings from the navigation menu. Click + Add. Provide a name (for example, CSHARPGUITAR-HTTPS-SETTING). Select HTTPS as the protocol. Provide a certificate name (for example, CSHARPGUITAR-HTTPS-CER). Select the CER certificate and download a test CER from github.com/benperk/ASA/tree/master/Chapter03/Ch03Ex12. Leave the rest at the defaults and then click the OK button.
