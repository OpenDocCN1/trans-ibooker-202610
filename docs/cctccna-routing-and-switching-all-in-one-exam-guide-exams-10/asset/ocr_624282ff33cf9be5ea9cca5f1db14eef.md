![](bbox=[0.15, 0.07, 0.85, 0.4])

# Figure 10-15 PVST+ and RSTP optimized

![](bbox=[0.14, 0.47, 0.25, 0.56])

NOTE Based on the design in Figure 10-15, you should make sure the default gateway for VLAN 1 is Switch-A (assuming it’s a layer 3 switch) and for VLAN 2 is Switch-B (assuming it’s a layer 3 switch). If you don’t configure it this way, but you have the default gateway associated with Switch-A, VLAN 2’s traffic will have to go from the access layer switch to Switch-B, and then across the EtherChannel to Switch-A before leaving the subnet. You can learn more about this in Cisco’s CCNP Switching course.

Remember that the topology in Figure 10-15 is created by you: you must manually change each switch’s priority to create the desired topology. The commands to accomplish this include the following: