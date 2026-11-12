**channel service unit/data service unit (CSU/DSU)** A piece of hardware that sits between a network and a digital telephone line to translate data between the two formats. A CSU/DSU is most commonly used to attach a network router to a T1 or other digital telephone line.

**circuit-switched connection** Dialup connections that include analog modem and digital Integrated Services Digital Network (ISDN) connections.

**classful routing protocols** A routing protocol that understands only class subnets. Routing Information Protocol version 1 (RIPv1) and IGRP are examples. A classful protocol does not send subnet mask information in routing updates. RIP and IGRP can have subnet masks other than the default, but the subnet mask used must be the same for all subnets of a class address.

**Classless Interdomain Routing (CIDR)** An extension to VLSM and route summarization. With VLSM, you can summarize subnets back to the Class A, B, or C network boundary. CIDR takes this one step further and enables you to summarize a block of contiguous Class A, B, and C networks. This is commonly referred to as supernetting, which is supported by today’s classless protocols.

**classless routing protocol** These protocols do not have any issues accepting routing updates with any bit value for a subnet mask and allow nonconforming subnet masks, such as a default route. Classful routing can also accept a default route but requires the configuration of the **ip classless** command. However, this overrides the classful protocols’ mechanics. Classless routing protocols include RIPv2, EIGRP, OSPF, IS-IS, and BGP.

**coaxial (or coax) cable** One of three types of physical media that can be used at the OSI physical layer. In a coaxial, one strand (a solid-core wire) runs down the middle of the cable, surrounded round by insulation. There are two different types of commonly used Ethernet coaxial cables: thickwire and thinwire. *See also* twisted pair cable and fiber optic cable.

**collision domain** A segmented group of networked systems in which simultaneous data transmissions can collide with one another.