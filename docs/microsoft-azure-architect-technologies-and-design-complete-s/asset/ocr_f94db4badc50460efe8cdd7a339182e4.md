If this is a chosen implementation requirement, then the DNS server software and configuration of the server identified in the Add DNS Server text box is required. As shown also in Figure 3.52, the Azure VMs in the VNet will need a reboot to recognize the DNS server settings.

# Hybrid Azure Networking

Hybrid networking has been described in numerous contexts, and this chapter’s introduction includes a solid description of hybrid networking. In summary, a hybrid Azure network is a combination of two networks, most commonly an on-premise network and a network that is hosted on Azure using an Azure virtual network. Two fundamental advantages of hybrid Azure networking are the increase of available data center capacity and the ability to upgrade the sophistication of the infrastructure at a reasonable price.

There are numerous methods for configuring a hybrid network connection, such as site-to-site, point-to-site, and the Hybrid Connection Manager (HCM). HCM uses Azure Relay to create a secure connection between Azure App Service and any accessible endpoint. You will configure HCM in a later exercise, but, for now, know that it is a useful tool for making simple and secure connections between Azure and on-premise resources. Numerous other connectivity options have been already mentioned in the “VNets and Subnets” section. Additionally, there is more to come in Chapter 6.

Before we continue, here’s a question for you:

True or false: A custom DNS is required to create a connection between a VNet and an on-premise network.

The answer is false. A custom DNS is necessary if the connections will use friendly names instead of IP addresses to connect with servers between a VNet and on-premise network.

In this section, the additional hybrid Azure networking topics are discussed in more detail.

+   - ExpressRoute
+   - Site-to-site (S2S) VPN gateway
+   - Forced tunneling

# ExpressRoute

ExpressRoute is the only way to connect to an on-premise network from and to Azure without traversing over the internet. All other connection methods do. A benefit of ExpressRoute is its built-in redundancy. The redundancy is realized with ExpressRoute circuits, where a circuit consists of two connections to two Microsoft Enterprise edge (MSEE) routers. These routers support layer 3 connectivity and implement Border Gateway