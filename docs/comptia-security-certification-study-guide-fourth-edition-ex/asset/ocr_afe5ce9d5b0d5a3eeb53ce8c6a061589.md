For example, you may use a packet sniffer such as Wireshark (see Figure 8-14) to capture network traffic and realize that there are a number of TCP SYN messages coming from the same system that are destined for different ports on the same target system. This is a great indication that someone is doing a port scan on the network.

Monitoring traffic today is a little harder than in the past due to the security features of a network switch. Remember that a switch only forwards the traffic to the port that the destination system resides on, so having the packet-capturing software is not enough today. To capture traffic today, you must be running your network card in promiscuous mode, which means that your network card will receive all traffic. Normally, systems receive and process only traffic that has a destination MAC address in the header of the packet with its own MAC address or the broadcast address.

# FIGURE 8-14

Packet sniffers can help identify abnormal traffic.

![](bbox=[0.112, 0.402, 0.928, 0.859])