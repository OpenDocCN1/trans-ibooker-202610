The **track** parameter is used to enter the interface that you want the HSRP RP to track. If this interface fails, for whatever reason, the active routing device decrements its HSRP priority by the configured value. Note that *decrement_value* is optional and, if omitted, defaults to a decrement of 10 for the priority.

# HSRP Verification

To verify the overall operation of HSRP, use the **show standby** command on the RP:

```
Switch# show standby
Vlan 1 - Group 1
    Local state is Active, priority 110, may preempt
    Hellotime 3 holdtime 10
    Next hello sent in 0:00:01
    Hot standby IP address is 172.16.10.1 configured
    Active router is local
    Standby router is 172.16.10.2 expires in 0:00:07
    Standby virtual mac address is 0000.0c07.ac01
Tracking interface states for 3 interfaces, 3 up:
    Up  Vlan1 Priority decrement: 10
```

In the output, you can see that the active routing device is 172.16.10.1 and the standby RP is 172.16.10.2.

For a shorter description, add the **brief** parameter to the preceding command:

```
Router# show standby brief
                Active          Standby Group
Interface Grp Prio P State    addr            addr        addr
Vlan1     1   100     Standby 172.17.10.2 local        172.16.10.254
```

In this example, the router for VLAN1 is in a standby state and the virtual IP address for the standby group is 172.16.10.254.

For additional troubleshooting, you can use the **debug standby** command from privileged EXEC mode. This command displays all HSRP messages that have been sent and received by the RP.

# Other Protocols