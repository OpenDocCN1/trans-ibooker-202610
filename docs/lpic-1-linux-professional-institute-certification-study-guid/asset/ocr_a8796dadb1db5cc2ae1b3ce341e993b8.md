With DNS, each network address is assigned a *domain name* (such as linux.org) that uniquely identifies the network, and each host in that network is assigned a unique *host name*, which is added to the domain name to uniquely identify the host on the network.

Thus, to find the host shadrach on the domain example.org, you’d use the DNS name shadrach.example.org. The DNS system uses servers to map host and domain names to the specific network addresses required to communicate with that server. Servers responsible for defining the network and host names for a local network interoperate with upper-level DNS servers to resolve remote host names.

To use DNS in your network applications, all you need to configure is the address of the DNS server that services your local network. From there, your local DNS server can find the address of any host name anywhere on the Internet.

# Dynamic Host Configuration Protocol

There’s one more network layer feature that we need to discuss before we move on to configuring the Linux system. Trying to keep track of host addresses for all of the devices on a large network can become cumbersome. Keeping individual IP address assignments straight can be a challenge, and often you’ll run into the situation where two or more devices accidentally are assigned the same IP address.

The *Dynamic Host Configuration Protocol* (DHCP) was created to make it easier to configure client workstations, which don’t necessarily need to use the same IP address all the time. With DHCP, the client communicates with a DHCP server on the network using a temporary address. The DHCP server then tells the client exactly which IP address, net-mask address, default gateway, and even DNS server to use. Each time the client reboots, it may receive a different IP address, but that doesn’t matter as long as it’s unique on the network.

These days, most home network routers include a DHCP server function, so all you need to do is set your Linux client to use DHCP and you’re done. You don’t need to know any of the “behind the scenes” details of the network addresses.

![](bbox=[0.132, 0.658, 0.224, 0.716])

Although DHCP is great for clients, it’s not a good idea to use for servers. Servers need to have a fixed IP address so that clients can always find them. While it’s possible to configure static IP addresses in DHCP, usually it’s safest to manually configure the network information for servers. This is called a *static host address*.

# The Transport Layer

The *transport layer* can often be the most confusing part of the network. Whereas the network layer helps get data to a specific host on the network, the transport layer helps get the data to the correct application contained on the host. It does that by using *ports*.