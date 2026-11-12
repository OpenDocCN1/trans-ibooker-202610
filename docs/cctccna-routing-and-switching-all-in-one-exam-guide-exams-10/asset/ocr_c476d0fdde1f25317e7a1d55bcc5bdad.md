default). This makes sense, since PC-E and PC-F, connected to SwitchB, are in the same VLAN.

![](bbox=[0.1, 0.14, 0.88, 0.58])

# Figure 10-1 Trunking without VTP pruning

Figure 10-1 shows a second VLAN with two members: PC-C and PC-D. If PC-C generates a local broadcast, SwitchA will obviously send to this to PC-D’s port. What doesn’t make sense is that SwitchA will flood this broadcast out its trunk port to SwitchB, considering that no devices on SwitchB are in this VLAN. This is an example of wasting bandwidth and resources. A single broadcast isn’t a big problem in this example; however, if a video multicast stream at 5 Mbps was coming from PC-A, the network could experience throughput problems on the trunk, since a switch treats a multicast just like a broadcast—it floods it out all ports associated with the source port’s VLAN.

You could use one of two methods to fix this problem: static VLAN pruning or dynamic VLAN pruning. With a static configuration, you would