small MTU size (and you don’t know what that number is). This defaults to n for no.

![](bbox=[0.14, 0.17, 0.24, 0.24])

**EXAM TIP** The Cisco CCT and CCNA exams expect you to know how to troubleshoot communication issues with the computer as well. When troubleshooting PC problems, first determine whether the user can ping the loopback address of their PC: **ping 127.0.0.1**. If this fails, you know something is wrong with the TCP/IP protocol stack installation on the PC. Next, have the user try to ping the configured IP address. If this fails, you know that something is wrong with their IP address configuration. Next, have the user ping the default gateway. If this fails, either something is wrong with the configured default gateway address, the default gateway itself, the subnet mask value configured on the user’s PC, or the layer 2 switch connecting them together (perhaps a mismatch in the VLAN on the router and PC interfaces of the switch).

# Using Traceroute

One limitation of **ping** is that it will not tell you where, between you and the destination, layer 3 connectivity is broken. The **traceroute** command, on the other hand, will list each router along the way, including the final destination. Therefore, if a layer 3 connection problem exists, **traceroute** will tell you at least where the problem begins. Like the **ping** command, **traceroute** has two versions: one for user EXEC mode and one for privileged EXEC mode. The following two sections cover the two different versions.

# Simple traceroute Command

The simple **traceroute** command, which works at both user and privileged EXEC modes, has the following syntax:

```
Router> traceroute destination_IP_address_or_host_name
```