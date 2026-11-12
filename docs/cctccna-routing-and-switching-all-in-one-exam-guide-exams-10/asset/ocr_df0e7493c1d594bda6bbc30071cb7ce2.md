```
Switch(config)# errdisable recovery cause bpduguard
```

Notice that you are in Global Configuration mode when executing this command.

Optionally, you can have the switch periodically do this by configuring the command with an interval, specified in seconds:

```
Switch(config)# errdisable recovery interval seconds
```

This automatically clears all errdisable states, including BPDU violations, on all ports every x seconds. However, if a BPDU violation occurs again, the port is placed back into an errdisable state.

![](bbox=[0.14, 0.39, 0.24, 0.45])

**EXAM TIP** For the CCNA exam, know that BPDU Guard immediately disables a port by placing it in an error disabled state when a BPDU is received on a port configured for PortFast.

# Per-VLAN Spanning Tree+ (PVST+)

STP doesn’t guarantee an optimized, loop-free network. For instance, take a look at the network shown in Figure 10-6, an example of a network that is poorly designed. In this example, the network has two VLANs, and the root switch is Switch-8. The Xs are ports placed in a blocked state to remove any loops. If you look at this configuration for VLAN 2, it definitely isn’t optimized. For instance, VLAN 2 devices on Switch-1, if they want to access VLAN 2 devices on Switch-4, have to go to Switches-2, 3, 6, 9, 8, and then 4. Likewise, VLAN 1 devices on either Switch-5 or Switch-7 that want to access VLAN 1 devices on Switch-4 must forward their traffic first to Switch-8 and then to Switch-4.