# EXERCISE 8.1

# Determining the Network Environment

This exercise will demonstrate how to quickly assess the network configuration and programs for your Linux system without having to dig through lots of configuration files. To document your system network information, follow these steps:

+   1. Log in as root, or acquire root privileges by using su or sudo with each of the commands that follow.
+   
+   2. Type ip address show to display the current network interfaces on your system. You will most likely see a loopback interface (named lo) and one or more network interfaces. Write down the IP (called inet) and IPv6 (called inet6) addresses assigned to each network interface, along with the hardware address and the network mask address.
+   
+   3. If your system has a wireless network card, type iwlist wlan0 scan to view the wireless access points in your area.
+   
+   4. If your system has a wireless network card, type iwconfig to display the current wireless settings for your network interface.
+   
+   5. Type route to display the routes defined on your system. Note the default gateway address assigned to your system. It should be on the same network as the IP address assigned to the system.
+   
+   6. Type cat /etc/resolv.conf to display the DNS settings for your system.
+   
+   7. Type netstat –l to display the programs listening for incoming network connections. The entries marked as unix are using the loopback address to communicate with other programs internally on your system.
+   
+   8. Type ss –anpt to display the processes that have active network ports open on your system.

# Summary

Connecting Linux systems to networks can be painless if you have the correct tools. To connect the Linux system you’ll need an IP address, a netmask address, a default router, a host name, and a DNS server. If you don’t care what IP address is assigned to your Linux system, you can obtain those values automatically using DHCP. However, if you are running a Linux server that requires a static IP address, you may need to configure these values manually.