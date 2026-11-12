**NOTE** 802.1Q is a standardized trunking method that inserts a 4-byte field into the original Ethernet frame and recalculates the FCS. Newer Cisco switches support only 802.1Q trunking and not the older ISL protocol. The native VLAN contains untagged frames, even on trunk connections.

One advantage of using this tagging mechanism is that, since you are adding only 4 bytes, your frame size will not exceed 1518 bytes, and thus you could actually forward 802.1Q frames through the access port connections of switches, since these switches would forward the frame as a normal Ethernet frame.

# Configuring Trunks

This section covers the setup of trunk ports on your switches using the 802.1Q trunking protocol. Configuring a port as a trunk port enables it to carry VLAN traffic from one switch to another across the trunk port. Before we get into the configuration, however, you should first be familiar with a protocol that is used to form a trunk between two devices: the Dynamic Trunk Protocol.

# Dynamic Trunking Protocol

Cisco’s proprietary *Dynamic Trunking Protocol* (DTP) is used to form and verify a trunk connection dynamically between two Cisco switches using a trunk connection. It supports five trunking modes, shown in Table 9-2.

| DTP Mode | Generate DTP Messages | Default Frame Tagging |
|---|---|---|
| On or trunk | Yes | Yes |
| Desirable | Yes | No |
| Auto | No | No |
| No-negotiate | No | Yes |
| Off | No | No |

Table 9-2 DTP Modes and Operatio