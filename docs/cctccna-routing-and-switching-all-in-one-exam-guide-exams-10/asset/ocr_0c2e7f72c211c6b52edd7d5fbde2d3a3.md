+   - **Address Resolution Protocol (ARP)** IPv4 uses ARP to resolve IP addresses to MAC addresses. ARP no longer exists as a separate protocol in IPv6; ICMPv6 now performs its functions.
+   - **Internet Group Management Protocol (IGMP)** IPv4 uses this protocol so that routing devices can learn if there are multicast devices in a VLAN that want to receive a multicast stream and to forward the necessary stream or streams to the VLAN. IGMP no longer exists as a separate protocol in IPv6; ICMPv6 now performs its functions.

ICMPv6 has taken over the role of other protocols as well and also has new functionality. Here are some examples of the new functions performed in ICMPv6:

+   - **Router solicitation and advertisement** Enables devices to query routing devices on a segment using a multicast message or routing devices to advertise themselves via multicast messages; also allows for the redirection of end stations to the best gateway for a destination network
+   - **DNS solicitation and advertisement** Enables devices to query for DNS servers on a segment using a multicast message or DNS server to advertise themselves via multicast messages
+   - **Neighbor solicitation and advertisement** Commonly referred to as *neighbor discovery*, determines the data link layer address for neighbors on the same link (the replacement of IPv4’s ARP), finds neighboring routers, and keeps track of the reachability of neighbors via multicast messages

![](bbox=[0.14, 0.71, 0.24, 0.77])

**EXAM TIP** For the exam, remember that IPv6 does not have broadcast traffic, so ICMPv6 uses unicast and multicast traffic.