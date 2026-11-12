Specific subnet ranges are reserved for private IP networks:

+   - 10.0.0.0 to 10.255.255.255
+   - 172.16.0.0 to 172.31.255.255
+   - 192.168.0.0 to 192.168.255.255

These private IP addresses can't be used for Internet traffic; they work only on local networks.

As you can imagine, with the popularity of the Internet, it didn't take long for IANA to run out of available public IP address networks. However, in a brilliant move, the idea of network address translation (NAT) saved the day. A NAT server can take an entire private IP network and assign it a single public IP address on the Internet. This is how you can connect your entire home network to a single ISP Internet connection and everything works.

# ## Default Router

With IP and IPv6, devices can communicate directly only with other devices on the same physical network. To connect different physical networks together, you use a *router*. A router passes data from one network to another. Devices that need to send packets to hosts on remote networks must use the router as a go-between. Usually a network will contain a single router to forward packets to an upper-level network. This is called a *default router* (or sometimes, a *default gateway*). Network devices must know the local default gateway for the network to be able to forward packets to remote hosts.

Thus, for a device to communicate in an IP network, it must know three separate pieces of information:

+   - Its own host address on the network
+   - The netmask address for the local physical network
+   - The address of a local router used to send packets to remote networks

Here's an example of what you would need:

+   - Host address: 192.168.20.5
+   - Netmask address: 255.255.255.0
+   - Default gateway: 192.168.20.1

With these three values in hand you're almost ready to configure your Linux system for working on the Internet. There's just one more piece of the puzzle you'll need to worry about, and we'll look at that in the next section.

# ## Host Names

With all of these IP addresses, it can be impossible trying to remember just what servers have what addresses. Fortunately for us, yet another network standard is available that can help out. The *Domain Name System* (DNS) assigns a name to hosts on the network.