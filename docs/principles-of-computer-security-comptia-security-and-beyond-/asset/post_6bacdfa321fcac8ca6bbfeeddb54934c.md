
- e. Connect the switches together by selecting Copper Cross-Over (fourth from the left in the bottom-middle pane), clicking each switch, and selecting GigabitEthernet0/2 for each. Orange circles by each switch will turn to green triangles shortly thereafter, indicating a good connection.

3e–3f

### Step 3 Configure the PCs as follows:

- a. Click the first PC from the left (the one Packet Tracer labeled PC0).

- b. Click the Config tab at the top of the PC0 window that opened.

- c. In the pane on the left, under INTERFACE, click FastEthernet0.

- d. In the IP Configuration section, give PC0 an IP address of 10.1.0.100, with a subnet mask of 255.255.0.0. In the next lab exercise, you’ll add default gateway IP addresses to the PCs as well as routers to this topology.

  In an IPv4 address, all host bits can’t be 0s (because it represents the network ID) and all host bits can’t be 1s (because it represents a network broadcast address). Since the fourth octet here (representing the last 8 of 16 host bits for this network addressing scheme) can’t have a value of 0, which would match up nicely with the name PC0, 100 has been chosen as an alternative.

- e. Click the Desktop tab and then click the Command Prompt icon. Type ipconfig and press ENTER to verify that you’ve assigned the IP address and subnet mask correctly.

- f. Repeat these steps on the other three PCs, assigning IP addresses of 10.1.0.1 to PC1, 10.1.0.2 to PC2, and 10.1.0.3 to PC3. Each PC will, likewise, have a subnet mask of 255.255.0.0.

4b–4c, 4e–4h

### Step 4 Configure the switches, send traffic from the PCs through the switches, and examine what the switch learned, as follows:

- a. Click the first switch, labeled Switch0.

- b. Click the CLI tab at the top of the Switch0 window. If necessary, press ENTER until you see the Switch> prompt, which indicates that you are in user EXEC mode. In user EXEC mode, you can look at certain status information, but not much else. Type enable and press ENTER at the prompt to go from user EXEC mode to privileged EXEC mode (also called “enable mode” because of the command to get there). You’ll notice the prompt changes to Switch#. In privileged EXEC mode, you can copy, erase, configure, and display settings. Type configure terminal and press ENTER at the prompt to go to global configuration mode. You’ll notice the prompt changes to Switch(config)#. In global configuration mode, you can configure many items, including the clock, hostname, and passwords.

  In the Cisco IOS (Internetworking Operating System), you only need to type in enough of a command to uniquely identify it from other commands in the same mode; therefore, en would be the same as ena, enab, enabl, and enable. Also, you can type in one or more characters and press TAB to see the command autocomplete. Furthermore, if you enter in a few characters and type ?, the Cisco IOS will show you ways to complete the command. If you press the spacebar before the ? symbol, the Cisco IOS will show you what it is expecting after the current entry on the prompt.

- c. Give the switch a hostname, which will appear on the prompt, by typing hostname followed by the hostname. In this case, use the hostname S0 (for Switch0).

- d. Type END and press ENTER to go back to privileged EXEC mode. If you see output in the console, press ENTER again to get a prompt. To go down one mode at a time, type exit and press ENTER. To go all the way down to privileged EXEC mode from a higher mode, type END and press ENTER or just press CTRL-Z.

- e. Using the preceding steps, give the second switch a hostname of S1 (for Switch1).

- f. Open a command prompt on one PC by clicking it, selecting the Desktop tab, and then selecting Command Prompt.

- g. From each PC, ping the other three.

Comes next in the entry; for example, show i? displays entries that come after show that start with i. Pressing ? after whitespace shows what entries can come after the previous ones; for example, show ? displays entries that come after show. After the results, in both cases, your original entries are placed at the prompt, so you can resume typing as you were before you entered the ? symbol.

- i. If necessary, select File | Save and then name your file so that you can start the next lab exercise later or go back to this point of a completed version of Lab Exercise 9.01.

60 MINUTES

## Lab Exercise 9.02: Router Configuration

To connect different networks together, a router maintains a table called a routing table that contains destination networks and directions for the router, such as where to send the traffic to next. If the router’s table in RAM doesn’t have knowledge of a destination network that a packet needs to go to, the routing table might have a default route, which means a specific router interface on a different router to send all traffic with unknown destination networks to. That other router will have a better idea of how to get to the destination. Without knowledge of the destination network or a default route, a router will drop a packet and send an ICMP (Internet Control Message Protocol) error message back to the source.

You would never see a PC connected to a router. Connected to a router you’d find either a switch or another router. That little box we all have at home that everyone calls a “router” actually has switch functionality built inside of it. It’s like there’s a switch and router that are interconnected in that box. If you’ve ever connected a desktop to a port on that router, you’ve actually plugged the cable into a switch port, even though we call that little box a router.

**Note**

That box we call a router also contains a DHCP server, a DNS server, a NAT gateway, a firewall, an access point, and much more.

Today’s Internet backbone routers have around 800,000 routes in their routing tables. On internal routers of autonomous systems, there are, of course, much fewer. An autonomous system represents a collection of networks under one administrative control that presents a common routing policy to the Internet, like an ISP or major entity like Rochester Institute of Technology (RIT). RIT has a collection of internal networks. Why would multiple networks be preferred to a single network?

Well, for one, think about ARP. ARP requests (and all other broadcast traffic) will always reach and be processed by every single device on a network. Configuring multiple networks interconnected by routers instead of one big flat network helps to reduce the size of the broadcast domain. Instead of broadcasts tying up the bandwidth and processing of all devices on a network, the number of broadcasts that will proliferate through the network and the number of devices that can hear them will be greatly reduced. Routers never forward broadcasts.

It’s why at RIT each class is taught in a room by itself. If we added one humongous auditorium and had all classes there simultaneously, my students would have to listen to and try to understand all the other professors and classes. That would take their attention away from me and my lessons.

Another reason why multiple networks are preferred to one big flat network is for security purposes. Security at the router level in the form of an access control list (ACL) can be used to filter traffic by source IP address, destination IP address, protocol, and port (port here refers to a logical port, a connection endpoint between programs; for example, SSH servers send and receive over port 22). This allows you to control access to and from certain devices and resources much better than if everything was on the same network.

Having multiple networks is also a way to hierarchically design an entire network, and it even makes the troubleshooting process easier, by isolating traffic to a certain network. Entities like RIT register for and receive their own autonomous system number (ASN). Thus, they become autonomous
