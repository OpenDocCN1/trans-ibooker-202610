The components that make up a virtual network can be observed, created, and modified using the ESXi Host Client.

To see a high-level overview of vSwitch0 (the switch that was automatically created as we installed ESXi), expand the Networking drop-down menu from the Navigator pane in the ESXi Host Client. Then, select vSwitch0 and expand the vSwitch topology pane, and a graphic representation of the vSwitch will be shown (see Figure 12-3).

![](bbox=[0.14, 0.29, 0.91, 0.63])

Figure 12-3. vSwitch Topology

# Virtual Switch

vSwitch0 has two port groups: VM Network and Management Network, both of which were connected to a single physical adapter (see Figure 12-4) and were created automatically when we installed ESXi. The physical adapter is the NIC on the Raspberry Pi.