It is not possible to use Azure DNS to purchase a domain name; a service named App Service Domains is the place to make that purchase. Azure DNS is used to host information about domains as records. If you do choose to purchase a domain from Microsoft, using App Service domains, the process creates what is called a DNS zone.

# Azure Public DNS

A DNS zone is synonymous with having a public DNS. Typically, the management of DNS records has been performed using the portal provided by the domain provider, like GoDaddy. A DNS zone supports the movement of that capability into the Azure Portal. The benefit is that you have one less portal to access and one less profile and credentials required to perform tasks.

This is where the two DNS records mentioned earlier come into play with Azure: NS and SOA. An NS entry into the DNS records is necessary to migrate the DNS management into the Azure Portal. The hosting provider does usually provide a group of four name servers; however, if you would like to move this to Azure, then Azure provides you with the name servers, and the name server records at the domain hosting company would need to be updated with Azure’s. Additionally, an NS record must be added to the DNS zone. When the NS record is created, the SOA record type is also automatically inserted. A start of authority (SOA) record notifies that the maintenance of the domain will no longer be happening at the parent domain hosting company; instead, it is being delegated to the authority identified in the SOA record.

# Azure Private DNS

Private DNS hosting is a feature that allows the configuration of DNS entries for Azure VMs hosted in VNets to be accessible using a domain name. This is different than what was performed in Exercise 3.11 in two ways. First, the domain name of the Azure VM is publicly accessible; anyone in the world with internet access can reach the endpoint.

The other difference is that the provided domain name ends with azure.com, which is not your own. The custom domain bound to an Azure VM can be something like private.csharpguitar.net. Then any Azure VM bound to this would be accessible using csharpguitar-v1.private.csharpguitar.net. Remember, though, that this domain resolved using an internal private DNS and will work only for servers within the VNet.

In a hybrid scenario, you may need to make custom modification to a DNS server, which is not possible using the procedures just reviewed. The next section covers how to implement a scenario like that.

# Dedicated DNS Server in VNet

Azure does support creating and hosting a custom DNS server within a VNet. This is helpful for when you want to connect with resources existing on-premise using a friendly name instead of IP addresses, and vice versa. This is additionally the means for performing the same across VNets. As shown in Figure 3.51, DNS queries performed on-premise can be routed to Azure VMs hosted in a VNet and across multiple VNets.