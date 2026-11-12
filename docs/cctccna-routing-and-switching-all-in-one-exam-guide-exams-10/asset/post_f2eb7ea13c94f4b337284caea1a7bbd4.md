
**TIP** Routing between VLANs is a great use of Layer 3 switches (switches that have routing functionality as well). You could have a Layer 3 core switch, with all the VLANs configured, performing the job of routing internally.

You can also purchase specialized NICs for PCs or servers, such as file servers or virtualization servers, that support trunking. For instance, suppose you want multiple VLANs to access a file server. You could use a normal NIC and set it up with an access port connection to a switch. Because this is an access port connection, the server could belong only to one VLAN. The users in the same VLAN, when accessing the server, would have all their traffic switched via Layer 2 devices to reach it. Users in other VLANs, however, would require that their traffic be routed to this server via a router, since the file server is in a different broadcast domain.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_524_0.png)

**TIP** If the same VLANs are on two connected switches, use a trunk connection between the switches to enable the associated VLANs on each side to communicate with each other. Trunk connections are commonly used on routers so that routers, via subinterfaces, can route between the VLANs. The trunking encapsulation, however, must match between the two trunking devices (such as using 802.1Q on both sides, or Cisco’s proprietary ISL on both sides).

If throughput is a big concern, you may want to buy a trunk NIC for the file server. Configuring this NIC is different from configuring a normal NIC on a file server. For each VLAN in which you want the file server to participate, you would create a virtual NIC, assign your VLAN identifier and Layer 3 addressing to the virtual NIC for the specific VLAN, and then associate it with the physical NIC. Once you have created all of these logical NICs on your file server, you need to set up a trunk connection on the switch to the server. And once you have done this, members of VLANs in the switched network will be able to access the file server directly without going through a router. These trunk-capable NICs are common enough today that you may even see them in certain PCs, since the NICs are commonly used to support virtualization.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_525_0.png)

**TIP** A good example of a device that may need a trunk-capable NIC is a DHCP server, because it may need to assign IP addresses to users across multiple VLANs. If you don’t have a trunk-capable NIC, but users are spread across multiple VLANs, you could use the IP helper feature on a Cisco router connected to the users’ VLANs and have the router forward the DHCP broadcasts to the DHCP server located in a different VLAN. Another example of a device that may need a trunk-capable NIC is a server in a data center: this is commonly necessary to support the many virtual machines (VMs) running on the server.

## Trunking Example

Figure 9-8 shows an example of a trunk connection between SwitchA and SwitchB in a network that has three VLANs. In this example, PC-A, PC-F, and PC-H belong to one VLAN; PC-B and PC-G belong to a second VLAN; and PC-C, PC-D, and PC-E belong to a third VLAN. The trunk between the two switches is also tagging VLAN information so that the remote switch understands the source VLAN of the originator.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_526_0.png)

*Figure 9-8 Trunking example*

Let’s take a look at an example of the use of VLANs and the two different types of connections by using the network shown in Figure 9-9. In this example, PC-C generates a local broadcast. When SwitchA receives the broadcast, it examines the incoming port and knows that the source device is from the gray VLAN (the access port connections are marked with dots). Seeing this, the switch knows to forward this frame only out of ports that belong to the same VLAN: this includes access port connections with the same VLAN identifier and trunk connections. On this switch, one access port connection belongs to the same VLAN, PC-D, so the switch forwards the frame directly out this interface.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_527_0.png)

*Figure 9-9 Broadcast traffic example*

The trunk connection between SwitchA and SwitchB handles traffic for multiple VLANs. A VLAN tagging mechanism is required to differentiate the source of traffic when moving it between the switches. For instance, assume that no tagging mechanism took place between the switches. PC-C generates a broadcast frame, and SwitchA forwards it unaltered to PC-D and then SwitchB across the trunk. The problem with this process is that when SwitchB receives the original Ethernet frame, it has no idea what port or ports to forward the broadcast to, since it doesn’t know the origin VLAN.

As shown in Figure 9-9, SwitchA tags the broadcast frame, adding the source VLAN to the original Ethernet frame (the broadcast frame is tagged). When SwitchB receives the frame, it examines the tag and knows that this is meant only for the VLAN to which PC-E belongs. Of course, since PC-E is connected via an access port connection, SwitchB first strips off the tagging and then forwards the original Ethernet frame to PC-E. This is necessary because PC-E has a standard NIC and doesn’t understand VLAN tagging. Through this process, both switches maintain the integrity of the broadcast domain.

## IEEE 802.1Q

Inter-Switch Link (ISL), a Cisco proprietary protocol, is being phased out in Cisco’s products and being replaced with the IEEE 802.1Q trunking standard, which was introduced in 1998. One of the advantages provided by the IEEE standard is that it allows trunks between different vendors’ devices, whereas ISL is supported only on certain Cisco devices. Therefore, by using 802.1Q you should be able to implement a multivendor trunking solution without having to worry about whether or not a specific type of trunk connection is or is not supported. The 2960 and better switches, along with Cisco’s higher-end switches such as the 6500 series, support 802.1Q. Actually, the new switch models support only 802.1Q trunking—they don’t support ISL at all. 802.1Q trunking is supported on switch ports that are capable of either Fast Ethernet or Gigabit Ethernet speeds.

802.1Q trunks support two types of frames: tagged and untagged. An untagged frame does not carry any VLAN identification information in it—basically, this is a standard, unaltered Ethernet frame. The VLAN membership for the frame is determined by the switch’s port configuration: if the port is configured in VLAN 1, the untagged frame belongs to VLAN 1. This VLAN is commonly called a native VLAN. A tagged frame contains VLAN information, and only other 802.1Q-aware devices on the trunk will be able to process this frame.

One of the unique aspects of 802.1Q trunking is that you can have both tagged and untagged frames on a trunk connection, such as that shown in Figure 9-10. In this example, the white VLAN (PC-A, PC-B, PC-E, and PC-F) uses tagged frames on the trunk between SwitchA and SwitchB. Any other device that is connected on this trunk line would need to have 802.1Q trunking enabled to see the tag inside the frame to determine the source VLAN of the frame. In this network, a third device is connected to the trunk connection: PC-G. This example assumes that a hub connects the two switches and the PC together.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_529_0.png)

*Figure 9-10 802.1Q trunk and native VLAN*

PC-G has a normal Ethernet NIC and obviously wouldn’t understand the tagging and would drop these frames. However, this presents a problem: PC-G belongs to the dark VLAN, where PC-C and PC-D are also members. Therefore, in order for frames to be forwarded among these three members, the trunk must also support untagged frames so that PC-G can process them. To set this up, you would configure the switch-to-switch connection as an 802.1Q trunk but set the native VLAN as the dark one, so that frames from this VLAN would go untagged across it and would allow PC-G to process them.

One restriction placed on an 802.1Q trunk configuration is that it must be the same on both sides. In other words, if the dark VLAN is the native VLAN on one switch, the switch at the other end must have the native VLAN set to the dark VLAN. Likewise, if the white VLAN is having its frames tagged on one switch, the other switch must also be tagging the white VLAN frames with 802.1Q information.
