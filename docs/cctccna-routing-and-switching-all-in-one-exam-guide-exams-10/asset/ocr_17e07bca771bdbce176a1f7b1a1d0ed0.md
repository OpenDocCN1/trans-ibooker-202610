+   - **Streamlined encapsulation** The IPv6 encapsulation is simpler than that of IPv4, providing faster forwarding rates by routers and better routing efficiency. No checksums are included, reducing processing on endpoints. No broadcasts are used, reducing utilization of devices within the same subnet. Quality of service (QoS) information is built into the IPv6 header, where a flow label identifies the traffic; this alleviates intermediate network devices from having to examine contents inside the packet, the TCP/UDP headers, and payload information to classify the traffic for QoS correctly.
+   - **Addressing capabilities** As with IPv4, addresses can be assigned statically or obtained via Dynamic Host Control Protocol (DHCPv6). However, unlike IPv4, IPv6 supports stateless autoconfiguration, which enables a device to acquire addressing automatically without implementing a DHCP server solution.
+   - **Transition capabilities** Various solutions exist to enable IPv4 and IPv6 to coexist successfully when migrating between the two. One method, dual stack, enables you to run both protocols simultaneously on an interface of a device. A second method, tunneling, enables you to tunnel IPv6 over IPv4, and vice versa, to transmit an IP version of one type across a network using another type. Cisco supports a third method, Network Address Translation-Protocol Translation (NAT-PT), to translate between IPv4 and IPv6 (sometimes the term *Proxy* is used instead of Protocol).

![](bbox=[0.14, 0.63, 0.24, 0.7])

**EXAM TIP** For the exam, make sure you are familiar with the enhanced features of IPv6 in the bulleted list.

# IPv6 Addressing

What scares most administrators about IPv6 addresses is how different they appear when compared to IPv4 addresses. Learning to deal with an address