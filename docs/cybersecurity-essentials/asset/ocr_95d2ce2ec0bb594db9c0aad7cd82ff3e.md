the host address identifies an intelligent member within the network. Three classes of standard IP addresses are supported for LANs: Class A, Class B, and Class C:

+   - Class A addresses are reserved for large networks and use the last 24 bits (the last three octets or fields) of the address for the host address. The first octet always begins with a 0, followed by a 7-bit number. Therefore, valid Class A addresses range between `001.x.x.x` and `126.x.x.x`. This allows a Class A network to support 126 different networks with nearly 17 million hosts (nodes) per network.
+   - Class B addresses are assigned to medium-sized networks. The first two octets can range between `128.x.x.x` and `191.254.0.0`. The last two octets contain the host addresses. This enables Class B networks to include up to 16,384 different networks with approximately 65,534 hosts per network.
+   - Class C addresses are normally used with smaller LANs. In a Class C address, only the last octet is used for host addresses. The first three octets can range between `192.x.x.x` and `223.254.254.0`. Therefore, the Class C address can support approximately two million networks with 254 hosts each.

# Subnets

Sections of the network can be grouped together into subnets that share a range of IP addresses. A protective gateway is employed to act as an entry and exit point for the segmented subnet. These groups are referred to as *intranets*. An intranet requires that each segment have a protective gateway to act as an entry and exit point for the segment. In most cases, the gateway is a device called a *router* or a *switch*. A router is an intelligent device that receives data and directs it toward a designated IP address. A Layer 2 switch is also an intelligent device, but it directs the data toward a designated MAC address.

Some networks employ a firewall as a gateway to the outside. A firewall is a combination of hardware and software components that provide a protective barrier between networks with different security levels. Administrators configure the firewall so that it will only pass data to and from designated IP addresses and TCP/IP ports.

Subnets are created by *masking off* (hiding) the network address portion of the IP address on the units within the subnet. This, in effect, limits the mobility