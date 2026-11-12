
```
Switch(config)# errdisable recovery cause bpduguard
```

Notice that you are in Global Configuration mode when executing this command.

Optionally, you can have the switch periodically do this by configuring the command with an interval, specified in seconds:

```
Switch(config)# errdisable recovery interval seconds
```

This automatically clears all errdisable states, including BPDU violations, on all ports every x seconds. However, if a BPDU violation occurs again, the port is placed back into an errdisable state.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_587_0.png)

**EXAM TIP** For the CCNA exam, know that BPDU Guard immediately disables a port by placing it in an error disabled state when a BPDU is received on a port configured for PortFast.

## Per-VLAN Spanning Tree+ (PVST+)

STP doesn’t guarantee an optimized, loop-free network. For instance, take a look at the network shown in Figure 10-6, an example of a network that is poorly designed. In this example, the network has two VLANs, and the root switch is Switch-8. The Xs are ports placed in a blocked state to remove any loops. If you look at this configuration for VLAN 2, it definitely isn’t optimized. For instance, VLAN 2 devices on Switch-1, if they want to access VLAN 2 devices on Switch-4, have to go to Switches-2, 3, 6, 9, 8, and then 4. Likewise, VLAN 1 devices on either Switch-5 or Switch-7 that want to access VLAN 1 devices on Switch-4 must forward their traffic first to Switch-8 and then to Switch-4.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_588_0.png)

**Figure 10-6 STP and VLANs**

When one instance of STP is running, this is referred to as a *Common Spanning Tree (CST)*. Cisco also supports a process called *Per-VLAN Spanning Tree Plus (PVST+)*. With PVST+, *each* VLAN has its own instance of STP, with its own root switch, its own set of priorities, and its own set of BPDUs. In this scenario, the BPDUs have an additional field that is a component of the switch or bridge ID with these three subfields: switch priority, extended system ID, and the switch’s MAC address. The extended system ID is a new field and carries the VLAN ID (VID) for the instance of STP.

With the addition of this field, it is possible to have different priorities on switches in different VLANs; thus you have the capability of having multiple root switches—one per VLAN. Each VLAN in PVST+, by default, will develop its own loop-free topology. Of course, PVST+, just like CST, doesn’t create an optimized, loop-free network; however, you can make STP changes in *each* VLAN to optimize traffic patterns for each separate VLAN. It is highly recommended that you tune STP for each VLAN to optimize it. Another advantage that PVST+ has is that if STP changes are occurring in one VLAN, they do not affect other instances of STP for other VLANs, making for a more stable topology. Given this, it is highly recommended that you implement VTP pruning to prune off VLANs from trunks of switches that are not using those VLANs. Pruning was discussed earlier in this chapter in the section “VTP Pruning.”

The downside of PVST+ is that because each VLAN has its own instance of STP, more overhead is involved: more BPDUs and STP tables are required on each switch. Plus, it makes no sense to use PVST+ unless you tune it for your network, which means more work and monitoring on your part.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_589_0.png)

**EXAM TIP** Remember for the CCNA exam that PVST+ supports one instance of STP per VLAN, enabling you to tune for the most optimal paths for each VLAN. CST supports one instance of STP for *all* VLANs.

## Simple STP Example

To help you get more familiar with the workings of 802.1d STP, let’s take a look at an example of STP in action. Use the network shown in Figure 10-7 as a starting point and assume that these switches do not support Rapid STP (RSTP), discussed later in the chapter, but only 802.1d STP. I’ll also assume that there is only one VLAN. The ports on each switch are labeled with a letter and a number. The letter is the port designator, and the number is the cost of the port as a BPDU enters the port.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_590_0.png)

**Figure 10-7 STP example network**

### Electing the Root Switch

The first thing that occurs once all these switches are booted up is the election of the root switch. The switches share BPDUs with one another to elect the root. In this example, all of the switches are using the default priority (32,768). Remember that the switch with the lowest switch ID is elected as root. Since all of the switches have the same priority, the switch with the lowest MAC address, which is Switch-1, is chosen as the root switch. Based on the election process, the new network topology looks like that shown in Figure 10-8.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_591_0.png)

**Figure 10-8 Root switch election**

### Choosing Root Ports for Each Switch

After the root switch is elected, each non-root switch must choose a root port that it will use to reach the root. Let’s take this one switch at a time so that you can see the decision process in detail. With Switch-1, which is the root switch, there are no root ports—if you recall, all ports on the root switch are designated ports.

Switch-2 has two ports to use to reach the root: E and F. When Switch-1 generates its BPDUs on ports I and J, the original path cost is set to 0. As these BPDUs are received by other switches, the receiving switch increments the path cost by the cost of the port on which the BPDU was received. As the BPDU comes into port E, Switch-2 increments the path cost to 20, and for port F, a cost of 10. The first check that Switch-2 makes is to compare the path costs. Port F has the best path cost and therefore is chosen as the root port, which is shown as RP in Figure 10-9. Switch-3 also has two paths to reach the root: via ports C and D. Port C’s accumulated path cost is 10, while D’s cost is 70. Therefore, port C is chosen as the root port. Switch-4 also has two ports to use to access the root: H and G. Port H has an accumulated path cost of 30, while G has a cost of 50, causing Switch-4 to choose port H as the root port. Switch-5’s two ports, A and B, have accumulated path costs of 10 and 40, respectively, causing Switch-5 to choose port A as the root port. Note that all the switches in the network are simultaneously running STP and figuring out for themselves where the root switch is and which of their own ports should be the root port. This is also true for choosing a designated port on a segment, discussed in the next section.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_592_0.png)

**Figure 10-9 Root ports**

### Choosing Designated Ports for Each Segment

After the root ports are chosen, each switch will figure out, on a segment-by-segment basis, whether its connected port to the segment should be a designated port or not. Remember that the designated port on a segment is responsible for moving traffic back and forth between the segment and the rest of the layer 2 network. The segments themselves, of course, are completely unaware of this process of choosing a designated port—the switches are figuring this out.

When choosing a designated port, the first thing that is examined is the accumulated path cost for the switch (connected to the segment) to reach the root. For two switches connected to the same segment, the switch with the lowest accumulated path cost will be the designated switch for that segment, and its port connected to that segment becomes a designated port.

Going back to our network example, let’s start with the easiest segments: B and C. For Switch-1, the accumulated path cost for LAN Segment-B is 0, Switch-2 is 20, and Switch-5 is 10. Since the root switch (Switch-1) has the lowest accumulated path cost, its local port (J) becomes the designated port for LAN Segment-B. This process is also true for LAN Segment-C—the root switch has the lowest accumulated path cost (0), making port I on Switch-1 the designated port for LAN Segment-C.

LAN Segment-A has two choices: Switch-3’s port D and Switch-4’s port H. Switch-3 has the lower accumulated path cost: 10 versus Switch-4’s 50. Therefore, Switch-3’s port D becomes the designated port for LAN Segment-A.

LAN Segment-D also has two choices for a designated port: Switch-5’s port B and Switch-4’s port G. Switch-5 has an accumulated path cost of 10, and Switch-4 has a cost of 30. Therefore Switch-5’s port B becomes the designated port for LAN Segment-D.

Figure 10-10 shows the updated STP topology for our network, where DP represents the designated ports for the LAN segments.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_593_0.png)
