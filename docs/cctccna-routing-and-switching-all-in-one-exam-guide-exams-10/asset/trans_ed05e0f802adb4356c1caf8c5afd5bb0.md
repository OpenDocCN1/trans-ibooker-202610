
discarding state. When the root port fails, the switch can speed up convergence by using the alternate port.

### American Standard Code for Information Interchange (ASCII)
A representation of standard alphabetic and other keyboard characters in a computer-readable, binary format.

### analog modem
A device used to connect to a remote network via a standard public switched telephone network (PSTN) line. Although there are many different types and makes of modems, they can be categorized into three groups: single external, single internal, and multiline rack mounted.

### anycast address
This IPv6 address identifies one or more interfaces (not devices), sometimes called nodes. An anycast is a hybrid of a unicast address and a multicast address. With a unicast, one packet is sent to one destination; with a multicast, one packet is sent to all members of the multicast group; and with an anycast, a packet is sent to any one member of a group of devices that are configured with the anycast address. By default, packets sent to an anycast address are forwarded to the closest interface (node), which is based on the routing process employed to get the packet to the destination. Given this process, anycast addresses are commonly referred to as one-to-the-nearest addresses.

### application layer
The seventh layer, or topmost layer, of the OSI Reference Model, the application layer provides the interface that a person uses to interact with the application.

### application-specific integrated circuit (ASIC)
These specialized processors can do very few tasks but do them extremely well. Processors, on the other hand, can perform many tasks but are not necessarily optimized for these tasks. Many types of networking hardware, including switches, use ASICs.

### asynchronous communication
Communication that does not require both the sender and the receiver to be online and participating in the communication at the same time.

### Asynchronous Transfer Mode (ATM)
A packet-switching network technology for LANs and WANs that can handle voice, video, and data transmissions simultaneously.

### authentication
The process of a user proving her identity to a system so that she can then access a system and its resources. Users are typically authenticated to the network after supplying correct username and password information.

### authorization
The process by which the system grants a user access to a resource such as a file or printer. Authorization follows authentication.

### Automatic Private IP Address (APIPA)
This IP address has the first two octets of 169.254.x.y and is automatically assigned by the system when a Dynamic Host Configuration Protocol (DHCP) server cannot be found on the network.

### autonomous system (AS)
A group of networks under a single administrative control, which could be your company’s network, a division within your company, or a group of companies’ networks.

### AutoSecure
An IOS feature on newer model routers, such as the Cisco ISR series, that enables you to add a basic security configuration on your router. It is a privileged EXEC script similar to the System Configuration Dialog: where the latter creates a basic configuration for a router, AutoSecure focuses only on security functions for securing a router.

### backbone
The main cable that interconnects networks.

### backdoor
A method a hacker uses to gain access to an unauthorized system. This backdoor may be available because of a vulnerability in the system or as a result of a previous attack.

### backup
The process of saving files to a separate location, usually an offline storage location, such as a different system or the cloud.

### backup port
Used in RSTP, a backup port on a segment could be used to leave a segment, even though there is already an active designated port for the segment. When a designated port fails, you can use a switch with a backup port to speed up convergence.

### bandwidth
The amount of data that the media can transfer. Bandwidth is usually measured in bits per second (bps).

### bandwidth domain
All of the devices on the same layer 2 physical segment are said to be in the same bandwidth domain. The more devices you have on a physical segment, the less bandwidth each device has. You can use routers or switches to create separate bandwidth domains.

### baseband transmission
A technique used to transmit encoded signals over cable using digital signaling at the same frequency. *See also* broadband transmission.

### Basic Service Set Identifier (BSSID)
This is the wireless network name—commonly known as the SSID. When the network is made up of one access point, it is referred to as the BSSID, but if multiple access points make up the wireless network, it is known as an Extended Service Set Identifier (ESSID).

### binary representation
Any value represented with just 0s and 1s.

### bits
A basic binary representation of information as a single protocol data unit (PDU). Two bit values—on (1) and off (0)—are used by computers to encode information. Bits are physical layer (layer 1) PDUs.

### blocking state
When STP is enabled, ports go into a blocking state under one of three conditions: when a root switch is elected, when a switch receives a Bridge Protocol Data Units (BPDU) on a port that indicates a better path to the root than the port the switch is currently using, and when a port is not a root port or a designated port. A port in a blocking state will remain there for 20 seconds by default. During this state, the only thing the port is doing is listening to and processing BPDUs on its interfaces.

### bootstrap program
This program brings the router or switch up and determines how IOS image and configuration files will be found and loaded, based on the configuration register and/or the existence of any boot commands in the configuration file.

### BPDU Guard
A feature used on ports configured with the PortFast feature. If a PortFast port receives a BPDU, the switch’s BPDU Guard feature immediately disables the port. Remember that PortFast is used on non-switch ports to keep them in a forwarding state; the assumption is that a PortFast port is not connected to a switch and therefore shouldn’t be receiving BPDUs.

### bridge
A device that solves the layer 2 bandwidth and collision problem by performing a switching function in software. A bridge supports only half-duplex connections. It typically supports 2–16 ports and performs store-and-forward switching.

### bridge (or switch) ID
Each layer 2 device running STP is assigned a unique identifier, which is also used in the BPDUs that the layer 2 devices advertise. The bridge ID has two components: the bridge’s or switch’s priority (2 bytes) and the bridge’s or switch’s MAC address (6 bytes).

### Bridge Protocol Data Unit (BPDU)
For STP to function, the switches need to share information with each other. BPDUs are sent out as multicasts every 2 seconds by default, and only other layer 2 devices are listening to this information. Switches use BPDUs to learn the topology of the network, including loops.

### broadband transmission
A technique used to transmit encoded signals over differing frequencies using analog signaling. *See also* baseband transmission.

### broadcast
A PDU sent to all devices. The destination MAC address denotes all devices on a segment (FFFF.FFFF.FFFF). A destination IP address of 255.255.255.255 represents all devices.

### broadcast domain
A group of systems that can receive one another’s broadcast messages.

### broadcast storm
An issue that occurs when there are so many broadcast packets on the network that the capacity of the network bandwidth approaches or reaches saturation.

### bus topology
A network topology that connects all computers to a single shared cable. In a bus topology, if one computer fails, the network fails. *See also star topology.*

### cable modem
A network device that connects to a network or the Internet using standard TV coaxial cable.

### Carrier Sense Multiple Access/Collision Detection (CSMA/CD)
A media access control method used in an Ethernet environment, where only one NIC can successfully send a frame at a time but all NICs can simultaneously listen to information on the wire. Before an Ethernet NIC puts a frame on the wire, it will first sense the wire to ensure that no other frame is currently on it. The NIC must go through this sensing process because the Ethernet medium supports multiple access—another NIC may already have a frame on the wire. If the NIC doesn’t sense a frame on the wire, it will transmit its own frame; if it detects a frame on the wire, the NIC will wait for the completion of the transmission for the frame on the wire and then transmit its own frame. If two or more machines simultaneously sense the wire and see no frame, and both place their frames on the wire, a collision will occur. When a collision occurs, the systems will detect the collision and resend their data at varying intervals.

### Challenge Handshake Authentication Protocol (CHAP)
An identity verification protocol that uses a three-way handshake process to perform authentication for a Point-to-Point Protocol (PPP) connection. First, the source sends its username (not its password) to the destination. The destination sends back a challenge, which is a random value generated by the destination. Both sides then run the source’s username, the matching password, and the challenge through the MD5 hashing function. The source will then send the result of this function to the destination. The destination compares this value to the hashed output that it generated—if the two values match, the password used by the source must be the same password used by the destination, and thus the destination will permit the connection.
