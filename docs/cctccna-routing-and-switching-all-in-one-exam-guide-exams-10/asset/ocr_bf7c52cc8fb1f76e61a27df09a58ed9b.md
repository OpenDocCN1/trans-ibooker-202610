measurement called a *metric* to determine which path is the best path to place in the routing table.

Table 11-2 lists some common metrics, the routing protocols that use them, and brief descriptions of the metrics. As you can see, some routing protocols use only a single metric. For instance, RIP uses hop count as a metric and OSPF uses cost. Other routing protocols use multiple metric values to choose a best path to a destination. For instance, EIGRP can use bandwidth, delay, reliability, load, and MTU when choosing a best path to a destination.

| Metric | Routing Protocols | Description |
|---|---|---|
| Bandwidth | EIGRP | The capacity of the links in Kbps (T1 = 1554) |
| Cost | OSPF | Measurement in the inverse of the bandwidth of the links |
| Delay | EIGRP | Time it takes to reach the destination |
| Hop count | RIP | How many layer 3 hops away from the destination |
| Load | EIGRP | The path with the least utilization |
| MTU | EIGRP | The path that supports the largest frame sizes |
| Reliability | EIGRP | The path with the least amount of errors or downtime |

Table 11-2 Routing Protocol Metrics

# Routing Troubleshooting Commands

Once you have configured your routing protocol, you can use a variety of commands to verify and troubleshoot the routing configuration and operation. Here are some example commands:

+   - clear ip route
+   - show ip protocols
+   - show ip route
+   - debug ip rip

# The clear ip route Command