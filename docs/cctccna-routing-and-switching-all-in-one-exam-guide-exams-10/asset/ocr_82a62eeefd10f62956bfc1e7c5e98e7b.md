| STP | Description |
|---|---|
| PVST+ | 802.1d per VLAN with Cisco-proprietary extensions (PortFast, UplinkFast, BackboneFast) |
| PVRST+ | 802.1w (RSTP) per VLAN |
| Multiservice Transport Platform (MSTP) | 802.1s, referred to as multiple STP, combines Cisco's PVST+ with IEEE standards |

# Table 10-4 STP Types

This book focuses only on PVRST+, and briefly at that. To enable PVRST+, use the following command:

```
Switch(config)# spanning-tree mode rapid-pvst
```

![](bbox=[0.14, 0.41, 0.24, 0.47])

**EXAM TIP** Know for the CCNA exam that the **spanning-tree mode rapid-pvst** command enables RSTP on an IOS switch.

Once enabled, you can view the STP on a per-VLAN basis with this command:

```
Switch# show spanning-tree vlan VLAN_# [detail]
```

Here’s an example of this command: