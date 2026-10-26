+   - **ARP Poisoning:** Address Resolution Protocol (ARP) operates at Layer 2 of the OSI reference model using **Media Access Control (MAC)** addresses. ARP poisoning is an attack in which a LAN is flooded with fake MAC addresses so that they can be linked to legitimate IP addresses. Once this happens, they can steal the data going to those IP addresses. This can only happen on a LAN.
+   
+   Example: The diagram below shows the arp cache with dynamic arp replies. In the exam, this is measured as arp poisoning:

| IP Address | Physical Address | Type |
|---|---|---|
| 192.168.1.10 | 24-f5-a2-a9-1b-a7 | dynamic |
| 192.168.1.56 | 00-17-c8-67-7e-97 | dynamic |
| 192.168.1.80 | 4a-02-29-ed-03-82 | dynamic |
| 192.168.1.81 | 42-7f-4e-09-de-53 | dynamic |
| 192.168.1.84 | ec-b5-fa-19-75-3a | dynamic |
| 192.168.1.85 | 20-47-ed-97-3b-3a | dynamic |
| 192.168.1.87 | 90-02-18-90-f1-11 | dynamic |
| 192.168.1.96 | ac-04-0b-3f-84-4a | dynamic |
| 192.168.1.14 | 20-47-ed-e4-87-2a | dynamic |

Figure 9.9 – ARP poisoning

+   - The best way to prevent ARP poisoning is to use arp -s to insert static entries into the ARP cache. ARP spoofing allows an attacker to intercept data frames on a network, modify traffic, or stop all traffic.

+   - **MAC Spoofing Attack:** MAC spoofing is the theft of a MAC address of another networked device, which is then used to gain access to the network; for example, a wireless access point that uses MAC filtering.

+   - **MAC Flooding:** This is where an attacker floods a switch with a fake IP Address to port mapping so that it consumes the limited memory that a switch has. Example: The following output shows the IP Addresses, the physical address, also known as the MAC address, and the port number on the switch. You can see that they are all using the same port. Each port should only have one MAC address; therefore, it is a MAC flooding attack.