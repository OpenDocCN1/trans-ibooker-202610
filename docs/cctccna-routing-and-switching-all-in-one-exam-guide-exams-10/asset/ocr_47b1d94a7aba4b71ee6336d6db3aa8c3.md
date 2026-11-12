choose port A as the root port. Note that all the switches in the network are simultaneously running STP and figuring out for themselves where the root switch is and which of their own ports should be the root port. This is also true for choosing a designated port on a segment, discussed in the next section.

![](bbox=[0.17, 0.21, 0.88, 0.58])

Figure 10-9 Root ports

# Choosing Designated Ports for Each Segment

After the root ports are chosen, each switch will figure out, on a segment-by-segment basis, whether its connected port to the segment should be a designated port or not. Remember that the designated port on a segment is responsible for moving traffic back and forth between the segment and the rest of the layer 2 network. The segments themselves, of course, are completely unaware of this process of choosing a designated port—the switches are figuring this out.

When choosing a designated port, the first thing that is examined is the accumulated path cost for the switch (connected to the segment) to reach the root. For two switches connected to the same segment, the switch with