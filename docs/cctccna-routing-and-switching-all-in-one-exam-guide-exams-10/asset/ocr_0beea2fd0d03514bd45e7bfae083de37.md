![](bbox=[0.12, 0.08, 0.78, 0.52])

Figure 14-6 IPv6 header

Following these eight fields are the extension headers, if any exist. These replace the IPv4 Options field. The number of extensions is not fixed, so the total length of the Extension Header Information field (list of extensions) is variable. As you can see from Figures 14-5 and 14-6, the IPv6 header has been simplified (with fewer fields).

# ICMPv6

ICMPv6 has a protocol number of 58. Like ICMP in IPv4, ICMPv6 is used to help perform diagnostic tests and identify problems (echo, echo reply, destination unreachable, hop count exceeded, and so forth). However, a lot more functionality has been added to ICMPv6.

In a sense, ICMPv6 is like the “Swiss Army knife” of IPv6 protocols. It takes over the responsibility that other, separate, protocols in IPv4 perform: