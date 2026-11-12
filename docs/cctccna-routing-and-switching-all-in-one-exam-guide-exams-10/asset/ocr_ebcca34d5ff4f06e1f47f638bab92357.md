+   - The **ipv6 router ospf process_ID** command takes you into the OSPFv3 routing process, and the **router-id** and **area** commands define the router’s ID and the areas the router is connected to. The **ipv6 ospf process_ID area area_number** command associates an interface to a particular OSPF routing process and area.
+   - The **ipv6 router eigrp autonomous** command takes you into the EIGRP for IPv6 routing process, and the **no shutdown** command enables the EIGRP for IPv6 process (this is required in certain versions of IOS). The **ipv6 eigrp autonomous_system** command associates an interface to a particular EIGRP for IPv6 autonomous system.

# Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully, as there may be more than one correct answer. Choose the correct answer(s) for each question.

1. You are configuring OSPF for IPv6 on your router that has the following interfaces configured:

G0/0: 192.168.2.1/24
S0/3/0: 192.168.3.1/24

You would like to configure OSPF to route traffic for the G0/0 interface only. What commands would you use to enable routing of IPv6 with OSPF on the G0/0 interface?

+   **A. R1(config)#ipv6 unicast-routing**
+     R1(config)#ipv6 ospf 10
+     R1(config-rtr)#router-id 0.0.0.1
+   **B. R1(config)#ipv6 ospf 10**
+     R1(config)#router-id 0.0.0.1
+   **C. R1(config)#ipv6 ospf 10**
+     R1(config)#network 192.168.2.0
+   **D. R1(config)#ipv6 unicast-routing**
+     R1(config)#ipv6 router ospf 10
+     R1(config-rtr)#router-id 0.0.0.1