In Figure 4.1, you can see some Host (A) records that were self-created when those machines joined our domain. I also have another server running on my network that has not yet been domain joined, and so it has not self-registered into DNS. This server is called RA1, but if I log in to any other system on my network, I fail to contact my RA1 server, since that name is not yet plugged into DNS:

![](bbox=[0.13, 0.17, 0.87, 0.32])

Figure 4.2: RA1 server does not resolve

For now, I am going to choose not to join this server to the domain, so that we can manually create a DNS record for it and make sure that I am able to resolve the name properly after doing that. Back inside DNS Manager on your DNS server, right-click on the name of your domain listed under the Forward Lookup Zones folder, and then choose New Host (A or AAAA). Inside the screen to create a new host record, simply enter the name of your server, and the IP address that is configured on its network interface:

![](bbox=[0.13, 0.48, 0.87, 0.78])

Figure 4.3: Adding a new host record