# Command

# Definition

| Command | Definition |
|---|---|
| show ip ospf database | Displays all of the IP routing protocols that are configured. |
| show ip ospf neighbor | Displays the routes in the routing table. |
| show ip route | Displays the collection of OSPF links learned from other OSPF routing devices. |
| debug ip ospf packet | Displays the router ID, the ID of the DR and BDR, timer values, the number of neighbors and the number of adjacencies. |
| show ip ospf interface | Displays all of your router's OSPF neighbors. |
| show ip protocols | Displays the OSPF packets as they are sent or received. |

# Answers

+   1. **B.** OSPF is a classless, not a classful, protocol.
+   2. **C.** OSPF uses cost as a metric.
+   3. **B.** An OSPF’s router ID is based on the highest IP address on its loopback interface, if configured, or the highest IP address on its active interfaces.
+   4. **B.** OSPF hellos are sent every 10 seconds.
+   5. **A.** The correct answer is 172.16.0.1. If the OSPF router ID has not been explicitly configured, the router ID will be the IP address of the loopback interface with the highest IP assigned. If there are no loopback adapters configured, the router ID will be the highest IP address assigned to an active interface.
+   6. **B.** The hello and dead interval timers, the area number, the OSPF router, the area type, and the MTU sizes must match on a segment for routers to form a neighbor relationship.
+   7. **C.** The OSPF process ID is locally significant.
+   8. **D.** When looking at the routing table after using the **show ip route** command, you will see the administrative distance of a route. OSPF uses an administrative distance of 110.