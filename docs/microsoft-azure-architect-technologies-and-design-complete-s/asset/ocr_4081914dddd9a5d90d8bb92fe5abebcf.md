The final scenario is that the outbound IP address has no significance. In this scenario the outbound IP address is not configurable and doesn’t count against any subscription limits, but it can change many times for many reasons. A word of caution: if this last scenario is selected, although most cost efficient, there is no means to whitelist or perform any kind of dependable restricting of access by those resources.

# Connecting with On-Premise Resources

Figure 3.2 shown previously illustrates connectivity from Azure and on-premise resources completely. An Azure VNet can be configured to connect with an on-premise data center using one of the following:

**ExpressRoute**  An *ExpressRoute* instance is the only connection between an Azure VNet and an on-premise network that does not cross into the internet. The connection is private and managed directly by an ExpressRoute partner. This connectivity scenario is mostly reserved for large enterprises with large workloads. The solution is relatively expensive when compared to the others, so checking out the price in advance using the Azure calculator would be advised. See azure.microsoft.com/en-us/pricing/calculator.

**Point-to-Site Connection (VPN)**  A *point-to-site (P2S)* connection, on the other hand, is an inexpensive solution for connecting any supported Azure resource to one existing in another network. If you are just getting started or are a developer wanting to give it a test run, start here. P2S is typically implemented using the *Hybrid Connection Manager (HCM)*, which is based on Azure Relay, or you can just simply use Azure Relay itself. P2S sends traffic through an encrypted tunnel between a computer and a virtual or on-premise network. Figure 3.11 illustrates that P2S connections can be made from within a VNet to an on-premise server, from an Azure App service/Azure Function to an on-premise server, and from an Azure App service/Azure Function to a server within a VNet.

**Site-to-Site Connection (VPN)**  Like P2S, traffic passing through a *site-to-site (S2S)* connection is sent within an encrypted tunnel across the internet. The primary difference is that the connectivity happens between Azure VPN Gateway and an on-premise VPN device.

Which Azure to on-premise connection would you recommend for a company just getting started with Azure but doesn’t want their traffic to go across the internet?

+   A. Site-to-site
+   B. ExpressRoute
+   C. Point-to-point
+   D. Point-to-site
+   E. Both B and C

The answer is site-to-site. When the traffic cannot traverse the internet, ExpressRoute is the only solution that provides that capability.