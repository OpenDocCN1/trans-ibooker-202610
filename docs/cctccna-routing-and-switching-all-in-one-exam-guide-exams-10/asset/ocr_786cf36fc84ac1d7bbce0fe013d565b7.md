**loopback address** Any address that starts with 127.x.x.x in IPv4 or the address of ::1 in IPv6. The loopback address is used to test and verify that the TCP/IP software stack has been loaded and is functional on the system.

**loopback interface** A logical, virtual interface on a router. These interfaces are treated as physical interfaces on a router: you can assign addressing information to them, include their network numbers in routing updates, and even terminate IP connections on them, such as telnet and SSH. Loopback interfaces are commonly used for assigning a router ID to an OSPF router, for testing purposes, and for terminating tunnel connections such as GRE and IPsec.

**Media Access Control (MAC) address** The data link layer uses MAC, or hardware, addresses for communication. For LAN communications, each machine on the same connected media type needs a unique MAC address. A MAC address is 48 bits in length and is represented as a 12-digit hexadecimal number. To make it easier to read, the MAC address is represented in a dotted-hexadecimal format, like this: FF:FF:FF:FF:FF:FF.

**metric** A routing protocol will use a measurement called a metric to determine which path is the best path. Examples of metrics include hop count, cost, bandwidth, and delay.

**multicast frame** With a multicast frame, the destination MAC address denotes a group of devices, which could include no device, some devices, or all devices.

**Nagle** An algorithm that handles TCP congestion by enabling multiple characters to be sent in the same TCP segment instead of different segments, like telnet. This is useful for devices running at high CPU utilization to allow for remote CLI access to still be responsive.

**native VLAN** 802.1Q trunks support two types of frames: tagged and untagged. An untagged frame does not carry any VLAN identification information in it—basically, this is a standard, unaltered Ethernet frame. The VLAN that supports untagged frames is the native VLAN.