one port is over-utilized and fault tolerance on the link can handle the workload if one port fails the other ports in the group.

# Putting It All Together

Let’s take a look at a typical setup for a wireless network using an AP to provide network access to clients on the network that are part of different VLANs. In Figure 15-4, you can see a Cisco switch that contains two VLANs: VLAN 10 for the accounting department and VLAN 20 for the marketing department.

![](bbox=[0.12, 0.31, 0.85, 0.75])

Figure 15-4 A single AP providing multiple wireless networks

To give each department wireless access, we connect an access point to the switch, but notice in the figure that the access point is connected to a port that is outside the two VLANs. This is because we need the AP to deliver traffic for both VLANs. So we connect the AP to the switch, and