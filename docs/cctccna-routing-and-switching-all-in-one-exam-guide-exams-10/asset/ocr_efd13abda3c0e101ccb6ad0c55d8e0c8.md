12. To view the port security details on the port, type the following:

```
VAN-SW1#show port-security interface g1/0/5
Port Security              : Enabled
Port Status                : Secure-shutdown
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 1
Configured MAC Addresses   : 1
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0030.F2E8.4203:1
Security Violation Count   : 1
```

Notice that the port status is Secure-shutdown, meaning that the port is shut down because of a security violation.

13. Use the show interfaces g1/0/5 command to view the interface stats on the port. Note the err-disabled on the first line, stating that this port was disabled due to error.

14. To enable the port once again, use the following commands:

```
VAN-SW1#config term
VAN-SW1(config)#interface g1/0/5
VAN-SW1(config-if)#shutdown
%LINK-5-CHANGED: Interface GigabitEthernet1/0/5, changed state to administratively down
VAN-SW1(config-if)#no shutdown
%LINK-5-CHANGED: Interface GigabitEthernet1/0/5, changed state to up
```

15. Use the show interface g1/0/5 command to see that the interface is up. Note that you cannot use the port from ComputerA, because the port security is still configured, so the port will get disabled again.

# Working with VLANs

A virtual LAN (VLAN) is a logical grouping of network devices in the same broadcast domain that can span across multiple switches. The top part of Figure 9-5 shows an example of a simple VLAN, where every device is in