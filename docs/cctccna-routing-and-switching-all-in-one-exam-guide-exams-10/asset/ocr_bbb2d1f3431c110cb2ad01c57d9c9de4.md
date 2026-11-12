example, on a 2960, that would be FastEthernet 0/1 or Gigabit 0/1.

After going through this selection process, the switch will have one, and only one, port that will become its root port.

![](bbox=[0.14, 0.21, 0.24, 0.27])

EXAM TIP You’ll be quizzed on the four steps used to choose a root port on a switch.

# Designated Port

You now know that each switch has a single root port that it uses to reach the root switch. In addition to each switch having a root port, each segment also has a single port that it uses to reach the root, and this port is called a designated port. Imagine, for example, that a segment has two switches connected to it. Either one or the other switch will forward traffic from this segment to the rest of the network.

The third step in running STP is to elect a designated port on a single switch for each segment in the network. The switch (and its port) that is chosen should have the best path to the root switch. Here are the steps taken by switches in determining which port on which switch will be chosen as the designated port for a particular LAN segment:

+   1. The connected switch on the segment with the lowest accumulated path cost to the root switch will be used.
+   2. If there is a tie in accumulated path costs between two switches, the switch with the lowest switch ID will be chosen.
+   3. If it happens that it is the same switch, but with two separate connections to the LAN segment, the switch port with the lowest priority is chosen.
+   4. If there is still a tie (the priorities of the ports on this switch are the same), the physically lowest numbered port on the switch is chosen.