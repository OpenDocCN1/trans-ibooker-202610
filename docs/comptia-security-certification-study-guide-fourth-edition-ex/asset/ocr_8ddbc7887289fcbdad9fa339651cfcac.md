It is important to note that the hacker may spoof a frame or a packet in order to bypass an access control list. For example, most wireless networks implement MAC filtering, where only certain MAC addresses are allowed to access the wireless network. Once the hacker finds out what MAC addresses are allowed on the network, they then spoof their MAC address to look like one of those addresses. A hacker may also spoof the IP address to bypass a filter on a router that only allows traffic from specific IP addresses to pass through the router.

exam watch
Spoofing is the altering of the source address to make the information look like it came from someone else.
IP spoofing and MAC spoofing are popular methods used by hackers to bypass filters placed on firewalls and wireless networks.

The following are examples of programs that can spoof packets:

+   Nemesis  A Linux packet-crafting program that creates different types of packets such as ARP and TCP packets.

+   Hping2  A Linux packet-crafting program that is used to create ping packets that use TCP instead of ICMP packets. Here’s an example of using hping2 to bypass a firewall that is blocking ICMP traffic:

```
hping2 -c 3 -s 53 -p 80 -S 192.168.2.200
```

+   Macchanger  A popular Linux program used to modify your MAC address on the system. Macchanger is a common program used by hackers to spoof their MAC address in order to gain access to the wireless network. Here’s an example:

```
macchanger --mac 11:22:33:44:55:66 wlan0
```

# Eavesdropping/Sniffing

A very popular type of attack is an eavesdropping attack, also known as sniffing. With an eavesdropping attack, the hacker captures network traffic and is able to view the contents of the packets traveling along the network. The packets may contain sensitive information such as credit card numbers or usernames and passwords (see Figure 4-5).

In Chapter 1 you learned that a network switch filters traffic and sends only the data to the port on the switch where the destination system resides. This type of filtering helps protect against eavesdropping because it essentially takes the opportunity away from the hacker to capture network traffic. However, the hacker can poison the MAC address table on the switch with bogus entries so that the switch stops trusting the MAC address table and then starts flooding all frames to all ports—resulting in the hacker now receiving a copy of all traffic!