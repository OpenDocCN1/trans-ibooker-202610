for VLAN 20. If RP-B fails, RP-A notices this after missing the hello messages from RP-B. Then RP-A promotes itself and starts forwarding frames that are destined to the virtual MAC address.

![](bbox=[0.12, 0.16, 0.85, 0.55])

Figure 11-8 HSRP example

Let’s assume, however, that RP-B does not fail but instead its interface vlan40 fails (connected to the core), as shown in Figure 11-9. Without HSRP running, RP-B would detect the failure and generate an ICMP redirect message to RP-A. This would enable RP-A to handle the redirected traffic. However, if RP-A and RP-B are participating in an HSRP group, ICMP redirects are disabled. This means that RP-B still functions as the active routing device and handles all traffic sent to the virtual MAC address. The problem, however, is that after the layer 3 routing protocol has converged, the traffic still reaches its destination. But to reach the destination, the traffic must pass through both RP-B and RP-A, thus introducing unnecessary latency.