# Network Address Allocation

The automatic way of allocating IP addresses is to use a server called the **Dynamic Host Configuration Protocol (DHCP)** server. This is a server with a database of IP addresses that can allocate to requesting hosts. There is a four-stage process, and it is known as *D-O-R-A.*

# IP Version 4 – Lease Process

IP version 4 observes the following steps to perform the lease:

+   1. **Discover**: When the client boots up, it sends a broadcast to find a DHCP server and can be identified by inserting its **Media Access Control (MAC)** address into the broadcast packet.
+   2. **Offer**: If the client is lucky enough to find a DHCP server, it then receives an offer packet from the DHCP server. If there are two DHCP servers, it will receive two offers.
+   3. **Request**: The client replies to the DHCP server that it wants to obtain the address from.
+   4. **Acknowledgment**: The final packet from the DHCP server is the acknowledgment that included the IP address, subnet mask, default gateway, and DNS address.

To release and renew an IP address using the command line, we would run the `ipconfig /release` command and then the `ipconfig/renew` command.

# IP Version 4 Lease Process – Troubleshooting

A DHCP client will not always obtain an IP address because maybe it cannot connect to the DHCP server. Another reason is that the address pool is exhausted. In these cases, the local machine will allocate an **Automated Private IP Address (APIPA)**, starting with `169.254.x.x`. This is an excellent aid to troubleshooting, as it lets the network engineer know that the client cannot contact the DHCP server.