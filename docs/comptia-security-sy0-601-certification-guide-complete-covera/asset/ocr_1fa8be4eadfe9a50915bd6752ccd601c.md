# Switch

A switch is an internal device that connects all users in a LAN. The switch has a table listing the MAC addresses, also known as a Content Addressable Memory (CAM) table listing the hosts connected to it:

![](bbox=[0.1, 0.21, 0.85, 0.29])

Figure 7.6 – Switch

Once the switch has been installed, it builds up a routing table; each host is identified by their MAC address. Only one MAC address should be registered to a single port. The switch delivers the packet only to the host that requires the packet. Switches can be stacked when there are more users connected to the network than ports on the switch:

![](bbox=[0.1, 0.42, 0.85, 0.63])

Figure 7.7 – Network connections

A computer has an Ethernet cable that plugs into a wall jack, and then the wall jack is connected to the patch panel by cables that are laid under floors or above ceilings. A user cannot see them. From the patch panel, there is a cable that goes into one port on the switch. The Ethernet cabling is placed inside a conduit to protect the cable. It is very easy to plug a cable into a wall jack. Therefore, the network administrator must place security for each of these ports on the switch.