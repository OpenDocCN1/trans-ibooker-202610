![](bbox=[0.14, 0.11, 0.24, 0.17])

**EXAM TIP** Remember the advantages and disadvantages of OSPF for the exam.

Knowing the advantages and disadvantages of any routing protocol is useful when it comes to picking a protocol. Typically, OSPF is used in large enterprise networks that have either a mixed-routing–vendor environment or a policy that requires an open standard for a routing protocol, which gives a company flexibility when it needs to replace any of its existing routers.

![](bbox=[0.14, 0.43, 0.24, 0.49])

**TIP** Typically, when you have more than 50 routers, Cisco recommends that you use a more advanced routing protocol such as Open Shortest Path First (OSPF) or Enhanced Interior Gateway Routing Protocol (EIGRP). In a mixed-vendor environment, there is basically one choice between these two: OSPF.

# Hierarchical Design: Areas

To provide scalability for very large networks, OSPF supports two important concepts: autonomous systems and areas. *Autonomous systems* (ASs) were introduced in Chapter 11. Within an AS, *areas* are used to provide hierarchical routing. An area is a group of contiguous networks. Basically, areas are used to control when and how much routing information is shared across your network. In flat network designs, such as those that use IP RIP, if a change occurs on one router (perhaps a flapping route problem), it affects *every* router in the entire network. With a correctly