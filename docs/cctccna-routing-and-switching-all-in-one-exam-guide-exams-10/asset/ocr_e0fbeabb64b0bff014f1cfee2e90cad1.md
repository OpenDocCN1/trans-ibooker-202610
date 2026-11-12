7. To disable the port, type **shutdown** and press ENTER.

8. Enter the following commands to view the status on port 8:

```
VAN-SW1(config-if)#exit
VAN-SW1(config)#exit
VAN-SW1#show interface g1/0/8
GigabitEthernet1/0/8 is administratively down, line protocol is down (disabled)
  Hardware is Lance, address is 00d0.ba2a.9508 (bia 00d0.ba2a.9508)
  Description: Web Server
  MTU 1500 bytes, BW 100000 Kbit, DLY 1000 usec,
  reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive set (10 sec)
  Half-duplex, 100Mb/s
(output cut for briefness)
```

Notice that the interface is administratively down, has a description assigned to it, has a speed of 100 Mbps, and is half duplex.

9. Navigate back to the interface and set its speed to 1000.

10. Enable the interface by using the **no shutdown** command.

# Understanding Port Security

*Port security* is a switch feature that enables you to lock down switch ports based on the MAC address or addresses associated with the interface; this prevents unauthorized access to a LAN. For example, you could configure port security on port g0/5 for the MAC address of aaaa.aaaa.1111, specifying that the system with that MAC address is the only authorized device for that port. If any other system connects to that port, a security violation results. As an administrator, you control what should happen when a violation occurs, be it generating a notification about the issue, dropping traffic for the MAC address that caused the violation, or completely disabling the port where the violation occurred.

![](bbox=[0.14, 0.76, 0.24, 0.83])