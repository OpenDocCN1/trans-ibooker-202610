networks, a dedicated DHCP server or servers are usually set up in the data center and/or distribution layers of the network.

To view the DHCP pool information, use the **show ip dhcp pool** command:

```
IOS# show ip dhcp pool
Pool 1:
  Utilization mark (high/low)      : 85 / 15
  Subnet size (first/next)         : 24 / 24
  Total addresses                  : 254
  Leased addresses                 : 11
  Pending event                    : none
1 subnet are currently in the pool :
Current index      IP address range          Leased addresses
10.1.1.12           10.1.1.1 - 10.1.1.254      11
```

With this command, you can view the total number of available addresses, the configured address range, and the number of currently leased addresses.

To view the current IP addresses assigned to DHCP clients, use the **show ip dhcp binding** command:

```
IOS# show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address      Client-ID/               Lease expiration      Type
                 Hardware address/
                 User name
10.1.1.1/24      0063.6973.636f           Mar 29 2014 04:36 AM   Automatic
```

This command lists the IP addresses assigned to the clients: the IP-to-MAC address bindings.

When assigning IP addresses to clients, IOS will first ping the IP address to ensure that it hasn’t been assigned to anyone else; additionally, IOS will examine its ARP table to ensure that the IP address isn’t in use by any other device. To view these conflicts, use the **show ip dhcp conflict** command:

```
IOS# show ip dhcp conflict
IP address      Detection method         Detection time         VRF
172.16.1.32     Ping                     Feb 16 1998 12:28 PM
172.16.1.64     Gratuitous ARP           Feb 23 1998 08:12 AM
```