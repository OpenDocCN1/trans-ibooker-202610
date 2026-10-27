The public interface is connected to the Internet and gets an IP address (a public IP address). The other interfaces on the NAT device are assigned private addresses, and all systems inside the network have a private address in that same range. When someone on the private network tries to surf the Internet, for example, the packet first goes from the client computer to the NAT device, which switches the private source IP address of the packet to the IP address of the public interface on the NAT device. The packet is then sent out on the Internet so that as far as anyone on the Internet is concerned, it is the NAT device surfing the Internet. The rest of the systems remain hidden on the private network.

Ad hoc networking involves a topology with no central server or device to create the network; you simply connect two devices together, such as two laptops on the fly, to create a network. A good example of this is an ad hoc wireless network where there is no wireless access point, just two laptops with wireless network cards that connect directly with one another so that the laptop users can share information.

# Proxy Servers

By definition, a proxy server is a server that performs a function on behalf of another system. A proxy server is responsible for making requests for Internet resources on behalf of the systems within the organization (because it is using NAT technology). Figure 8-11 and the following list detail how a proxy server works:

+   1. The workstations (clients) on the network are configured to send Internet requests to the proxy server.
+   2. A workstation submits a request for a web page on the Internet to the proxy server.
+   3. The proxy server then connects to the Internet site on behalf of the user and transmits the request on to the Internet web server.
+   4. The Internet web site sends the reply to the proxy server.
+   5. The proxy server sends the reply to the original workstation that made the request.

![](bbox=[0.072, 0.694, 0.753, 0.9])