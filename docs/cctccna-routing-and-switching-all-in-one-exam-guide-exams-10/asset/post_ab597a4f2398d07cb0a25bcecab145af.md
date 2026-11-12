
![](img/0eafdcebe219ef3756f0e57bcf1eb997_693_0.png)

Figure 12-1 OSPF hierarchical design

![](img/0eafdcebe219ef3756f0e57bcf1eb997_693_1.png)

**NOTE** The backbone (area 0) in OSPF is required and non-0 areas (such as areas 1, 2, 3, and so on) must be connected to the backbone. The preferred connection is a physical connection; however, OSPF supports a feature called *virtual links* that enables an area to be logically connected to the backbone. A discussion of virtual links is beyond the scope of this book.

Area 2, for instance, doesn’t need to see all of the subnets of Area 1’s 172.18.0.0 network number, since only two paths exist out of Area 2 to the backbone. Area 2, however, needs to see all of its internal subnets to create optimized routing tables to reach its own internal networks within Area 2. Therefore, in a correctly designed OSPF network, each area should contain specific routes only for its own areas and summarized routes to reach other areas. By performing this summarization, the routers have a smaller topology database (they know only about links in their own area and the summarized routes) and their routing tables are smaller (they know only about their own area’s routes and the summarized routes). Through a correct hierarchical design, you can scale OSPF to very large sizes. Chapter 5 discussed route summarization.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_694_0.png)

**EXAM TIP** Note that the CCNA exam focuses on only single-area designs, and the material throughout the rest of the sections of this chapter covers only single-area concepts. The CCNP-level material, however, spends a lot of time on both single- and multi-area designs. Designing a multi-area OSPF network can become very complicated and requires a lot of networking knowledge and skill.

## Metric Structure

Unlike RIP, which uses hop count as a metric, OSPF uses cost. Cost is actually the inverse of the bandwidth of a link: the faster the speed of the connection, the lower the cost. The most preferred path is the one with the lowest accumulated cost value. By using cost as a metric, OSPF will choose more intelligent paths than RIP. (Metrics are discussed in more depth in the “OSPF Metric Values” section later in the chapter.)

Remember that on synchronous serial links, no matter what the clock rate of the physical link is, the bandwidth always defaults to 1544 Kbps. You’ll want to code this correctly with the **bandwidth** interface subconfiguration mode command (discussed in [Chapter 11](Chapter 11)). This is important if you have multiple synchronous serial paths to a destination, especially if they have different clock rates. OSPF supports load balancing of up to 16 equal-cost paths to a single destination; however, only 4 equal-cost paths are used by default. Remember that if you don’t configure the bandwidth metric correctly on your serial interfaces, your router may accidentally include paths with different clock rates, which can cause load-balancing issues.

For example, if you have one serial connection clocked at 1544 Kbps and another clocked at 256 Kbps and you don’t change the bandwidth values, OSPF will see *both* connections as 1544 Kbps and attempt to use 1544 Kbps when reaching a single destination across these links. This is because the default bandwidth on a serial link is 1544, no matter what the clock speed of the interface is. This can create throughput problems when the router is performing load balancing—half of the connections will go down one link and half down the other, creating congestion problems for the 256-Kbps connection. Therefore, remember that you should change bandwidth of the interface with the **bandwidth** command.

## OSPF Operation

As mentioned, OSPF is a link state protocol similar to what was generically described in [Chapter 11](Chapter 11). However, each link state protocol, such as OSPF and Intermediate System-Intermediate System (IS-IS), has its own unique features and characteristics. This section introduces you to how OSPF operates in a single-area design.

## Router Identities

Each router in an OSPF network needs a unique ID—this must be unique not just within an area, but within the entire OSPF network. The ID is used to provide a unique identity to the OSPF router. The ID is included in any OSPF messages the router generates that other OSPF routers will process.

The router ID is chosen according to one of the following criteria:

- The highest IP address on the router’s active loopback interfaces is used (this is a logical interface on a router).
- If no loopback interface exists with an IP address, the highest IP address on its active interfaces is used when the router boots up.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_696_0.png)

**NOTE** Loopbacks typically have a host mask (/32). Cisco supports other masks for loopback interfaces, but many vendors restrict you to a /32 mask.

The router ID is used by the router to announce itself to the other OSPF routers in the network. This ID must be unique. If no loopback interfaces are configured, the router will use the highest IP address from one of its active physical interfaces. Optionally, you can manually define the router ID, always ensuring how it will be defined no matter what interfaces exist on the router.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_696_1.png)

**EXAM TIP** Remember for the CCNA exam how a router acquires its router ID for OSPF: the active loopback with the highest IP address or the active physical interface with the highest IP address.

If no active interface exists, the OSPF process will not start, and therefore you will not have any OSPF routes in your routing table. It is highly recommended, therefore, that you use a loopback interface, because it is always up and thus the router can obtain a router ID and start OSPF.

## Finding Neighbors

Whereas RIP accepts routing updates from just about any other RIP router (unless RIPv2 with authentication is configured), OSPF has some rules concerning if and how routing information should be shared. First, before a router will accept any routing information from another OSPF router, the routers must build an adjacency between them, on their connected interfaces. When this adjacency is built, the two routers (on the connected interfaces) are called neighbors, indicating a special relationship between the two.

An OSPF router learns about its OSPF neighbors and builds its adjacency and topology tables by sharing link state advertisements (LSAs), which exist in different types. When learning about the neighbors to which a router is connected, as well as keeping tabs on known neighbors, OSPF routers will generate LSA hello messages every 10 seconds. When a neighbor is discovered and an adjacency is formed with the neighbor, a router expects to see hello messages from the neighbor. If a neighbor’s hello is not seen within the dead interval time, which defaults to 40 seconds, the neighbor is declared dead. When this occurs, the router will advertise this information, via an LSA message, to other neighboring OSPF routers.

In order for two routers to become neighbors, the following items must match on each router:

- The area number
- The hello and dead interval timers on their connected interfaces
- The OSPF password (optional), if it is configured
- The area stub flag, indicating the type of area (a stub is used to contain OSPF messages and routing information, which is beyond the scope of this book)
- Maximum transmission unit (MTU) sizes on the connected interfaces

If these items do not match, the routers will not form an adjacency and will ignore each other’s routing information.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_697_0.png)

**EXAM TIP** For the Cisco CCNA exam, remember that the hello process is used to discover and maintain a neighbor relationship with other OSPF routers. In order to form a neighbor relationship, two OSPF routers on the same segment must be in the same area, must have matching hello and dead interval timers, must have the same password if authentication is configured, must be of the same type (stub, not-so-stubby, or no stub), and must have the same MTU size configured on their interfaces.

Let’s assume that you turned on all your routers simultaneously on a segment. In this case, the OSPF routers will go through three states, called the *exchange process*, in determining whether they will become neighbors:

1. **Down state** The routers have not exchanged any OSPF information with any other router.
2. **Init state** A destination router receives a new router’s hello and adds it to its neighbor list (assuming that values in the preceding bullet points match). Note that communication is only unidirectional at this point.
3. **Two-way state** The new router receives a unidirectional reply (from the destination router) to its initial hello packet and adds the destination router to its neighbor database.

Once the routers have entered a *two-way* state, they are considered neighbors. At this point, an election process takes place to elect the designated router (DR) and the backup designated router (BDR) on the segment.

## Designated and Backup Designated Routers

An OSPF router will not form adjacencies to just any router. Instead, a client/server design is implemented in OSPF on *each* broadcast segment. For each multi-access broadcast segment, such as Ethernet, there is a DR and a BDR as well as other OSPF routers, called *DROTHERs*. As an example, if you have ten VLANs in your switched area, you’ll have ten DRs and ten BDRs. The one exception of a segment not having these two routers is on a WAN point-to-point link.
