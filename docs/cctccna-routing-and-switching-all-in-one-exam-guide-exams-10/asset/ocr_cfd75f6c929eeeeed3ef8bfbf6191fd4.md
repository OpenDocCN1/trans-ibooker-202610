going through their different states. So, when STP is running, PortFast ports on the same switch can still forward traffic among themselves, limiting your STP disruption somewhat. However, if these devices wanted to talk to devices connected to other switches, they would have to wait until STP completed and the root and designated ports had moved into a forwarding state.

PortFast is a great option to configure when you want to use Preboot Execution Environment (PXE) boot with your workstations, where the device booting is trying to contact a DHCP server right away at bootup. If you are not using PortFast in this scenario, the workstation will not be able to contact the DHCP server before the 50-second timers of STP have expired. PortFast is the solution!

![](bbox=[0.14, 0.39, 0.24, 0.45])

**CAUTION** Ports connected to non-switch devices should be configured with PortFast, such as PCs, servers, and routers. However, make sure that you don’t enable PortFast on a port connected to another layer 2 switch, because you may inadvertently be creating a layer 2 loop, which can bring down the network.

# PortFast Configuration

PortFast works with all versions of STP supported by Cisco switches. Configuring the PortFast feature is simple, and you can enable it globally or on an interface-by-interface basis. To enable it globally, use this command:

```
Switch(config)# spanning-tree portfast default
```

This command enables PortFast on all non-trunking ports on the switch.

To enable PortFast on an interface, use this configuration:

```
Switch(config)# interface type [slot_#]/port_#
Switch(config-if)# spanning-tree portfast [trunk]
```