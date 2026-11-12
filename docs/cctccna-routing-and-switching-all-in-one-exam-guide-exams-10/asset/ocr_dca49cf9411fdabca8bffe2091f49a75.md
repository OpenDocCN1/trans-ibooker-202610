solicitation message, asking the router to reply with an RA so the client can generate its interface address.

Information that can be obtained via stateless autoconfiguration include

+   - The network numbers (prefixes)
+   - The lifetime of the prefixes
+   - A flag that indicates the kind of autoconfiguration the end stations can perform (stateless or stateful)
+   - The default router for the prefix(es) and the lifetime for this value
+   - The default maximum transmission unit (MTU) size for the segment
+   - The maximum hop count allowed

Other information can also be obtained, but this list comprises the most common parameters.

# Routing and IPv6

As in IPv4, routers in IPv6 find best paths to destinations based on metrics and administrative distances; and like IPv4, IPv6 routers look for the longest matching prefix in the IPv6 routing table to forward a packet to its destination. The main difference is that the IPv6 router is looking at 128 bits when making a routing decision instead of 32 bits.

![](bbox=[0.13, 0.61, 0.25, 0.71])

**NOTE** RFC 2461 requires that a router must be able to identify the link-local address of each neighboring router, which is used in the routing process. Because of this, the use of global unicast addresses as a next-hop address is not recommended by the RFC.

# Supported Routing Protocols