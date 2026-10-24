certainly consume incoming bandwidth. However, if the network replies to the ICMP requests, then outgoing bandwidth will be consumed as well.

Sadly, it is also possible to send malformed PING packets maliciously. Older systems, for example, could not handle a packet larger than 65,535 bytes, and any packet larger than that would crash the computer. Modern routers should examine these packets for size and fragmentation and enforce rules that prevent any problems from a “ping of death.”

While it is typically good practice to allow ICMP traffic on public networks for a variety of reasons, it is critical that your firewall block ICMP traffic, and for most networks, you may even want to set the firewall to not even reply to these requests itself. A PING request should look something like the one depicted in Figure 23.2:

```
ping www.google.com
```

![](bbox=[0.231, 0.399, 0.697, 0.552])

# FIGURE 23.2 PING

# Traceroute

A TRACEROUTE utility will display the *route* (path) that packets will travel from one IP to another. As with PING, the round-trip time of the ICMP packets (also known as *latency*) is measured; however, with traceroute this time is measured between each successive host in the route.

Each segment of the route is known as a *hop*. Traceroute will send three packets and measure the time required to complete each hop. The cumulative latency at each hop is recorded, usually in milliseconds, along with the IP and host name (acquired through a reverse lookup) of each node.

This can be a great way to look at latency issues between hosts and evaluate the connection to determine where excessive latency exists or a route fails.

Traceroute is available in most operating system command-line interfaces. Windows uses the TRACERT utility. Some utilities, such as tcptraceroute, may use TCP packets.