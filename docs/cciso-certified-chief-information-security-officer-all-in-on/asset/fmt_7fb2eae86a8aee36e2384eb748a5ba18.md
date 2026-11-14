
## LAN Components and Topology

A typical LAN consists of one or more external connections enabled by border equipment and an internal network that is usually segmented to achieve performance and/or security objectives. The internal network may be a wired network, almost always Ethernet nowadays, or wireless, or a combination of wired and wireless. [Figure 4-5](#ch4.xhtml#fig4_5) depicts a typical network. Some of the components shown in the diagram are introduced here while others are discussed later in this section.

![Images](Images/F04-05.jpg)

**Figure 4-5** A typical LAN

External users and customers access the organization’s network via the Internet, with the “last mile” being terminated at the organization’s premises by an Internet router that routes and controls traffic to and from the organization’s LAN. The way the various devices are physically arranged is called the network *topology*. [Figure 4-5](#ch4.xhtml#fig4_5) does not show a specific topology. There are many possible networking topologies, including bus, ring, tree, star, mesh, and hybrid. Most modern LANs use star topologies.

Every device on the network has an Internet Protocol (IP) address. Data is routed between devices using these IP addresses. Routers are devices that connect similar or different networks together and “route” traffic using IP addresses.

All data communication within and between LANs use standard message formats which are defined by *protocols*. The message contains all of the information that each device and application needs in order to process the message in accordance with the protocol. Each message consists of a *packet*, which contains the following information:

- Identification of the source or originator of the message
- Identification of the destination of the message
- Control information, which may include sequence numbers, flags, pointers, length, checksums, and so on
- Data (but not always—some messages just contain control information with no data)

Based on information in the packet, the various devices on the network route and/or act on the data. Here is a brief summary of some of the most common network devices:

- **Internet router** This is the device at the perimeter of the LAN that provides the interface to the WAN link.
- **Router** Sometimes the Internet router is combined with the border router and can even perform as an external firewall, filtering traffic depending on the organization’s security policy. The router performs routing of data between network segments and may perform other functions such as network address translation (NAT), discussed in an upcoming section.
- **Firewall** Firewalls are specialized equipment used to enforce a security policy such as allowing or denying certain types of traffic. Discussed in more detail later in the section, firewalls play an important role in securing the organization’s network. In [Figure 4-5](#ch4.xhtml#fig4_5) a firewall is used to control traffic at the border of the LAN and specifically filter traffic going to/from the organization’s mail server, externally facing website, and internally segmented network.
- **Switch** Switches operate at OSI Layer 2 (more on the OSI model later) and allow the network to be configured in segments, which makes better use of the available bandwidth and can be used to create security zones.

### IP Addressing

Addresses enable how data is sent and received throughout Ethernet networks. Much like the address of a house enables mail to be delivered to the intended recipient, IP addressing enables data to be sent to the right place on a network. And just like houses are addressed using multiple elements (such as city name, street name, house number), components on the network are addressed using two elements: *network* and *node* (or *host*). In fact, every IP address contains two parts: the network identifier and the node identifier.

In IPv4 the IP address is composed of 32 bits consisting of four octets. There are public and private IP addresses; public IP addresses are routed throughout the Internet and private IP addresses are used within corporations, organizations, and government entities (private IP addresses cannot be routed on the Internet). Public IP addresses are assigned by the Internet Assigned Numbers Authority (IANA), which is the organization responsible for managing IP addresses worldwide. Private IP addresses are freely assigned by any company, organization, or individual for their own use. The following are the ranges of private IP addresses that are nonroutable on the Internet:

- 10.0.0.0–10.255.255.255
- 172.16.0.0–172.31.255.255
- 192.168.0.0–192.168.255.255

IP addresses are used in combination with a *subnet mask*, which identifies which portion of the IP address specifies the network and which portion specifies the node (or host). The mask, like the IP address, is 32 bits long. Each bit is turned on (1) or off (0) and reads left to right. Bits turned on indicate which parts of the address specify the network; bits turned off specify the node. The subnet mask is defined as part of the address using Classless Inter-Domain Routing (CIDR) notation as follows:

- The IP address followed by a slash (/)
- Decimal number (from 0 to 32) indicating the number of bits of the IP address that constitute the network ID

For example, the address 192.168.0.10/24 indicates that the first 24 bits of the address (starting at the left) is the network address and the remaining bits is the address of the node or host.

### Network Address Translation

Internet routers are not configured to route private IP addresses across the Internet. So, how is data routed across the Internet to devices within a network if the private address is not used? For this, IPv4 uses network address translation (NAT).

Using NAT, routers at the border of the private organization are configured to map external (public) IP addresses to internal (private) IP addresses. There are many types of NAT, the most basic of which provides a one-to-one mapping of external addresses to internal addresses. We don’t cover all the various types of NAT in this book, but they each provide a way to traverse public and private networks in a manner that is transparent to the source and destination hosts.

### The Move to IPv6

IPv4 uses 32-bit addressing, which enables over 3.7 billion addresses. That would seem to be enough, but considering there are about 6 billion people on earth (and who knows how many elsewhere), it would seem that someday we will run out of addresses. To solve this, IPv6 was developed and released in 2012. IPv6 uses 128 bits, which supports 2 to the 128th power (or 340,282,366,920,938,463,463,374,607,431,768,211,456) addresses. That should be enough. However, adoption of IPv6 has been slow even though adopting it now would simplify some things, such as eliminating the need for NAT. Currently IPv6 has been adopted by about 30 percent of the world. Reasons for the slow adoption vary, but in many cases IPv4 represents a huge investment that has already been made in equipment that cannot be easily upgraded to IPv6. Also, many people and organizations in the industry have mastered and tamed IPv4 and are reluctant to make such a change. IPv4 will not be shut down anytime soon, but eventually the industry will catch up and 128-bit addressing will be the norm.

### Network Protocols and Communications

Communication between devices on a modern network can occur only if there is a common model that they all follow. The industry most commonly uses the Open Systems Interconnection (OSI) model (or a variant called the TCP/IP model) as a way of establishing and describing how every device on the network will communicate with each other. The OSI model has the following features:

- Explains how networking takes place and is used to architect network services and protocols
- Allows for interoperability between vendor products by defining interface standards
