For more detailed troubleshooting, you can use **debug** commands. If you want to view the adjacency process that a router builds to other routers, use the **debug ip ospf adj** command:

```
```
Router# debug ip ospf adj
172.16.255.11 on Ethernet0, state 2WAY
OSPF: end of Wait on interface Ethernet0
OSPF: DR/BDR election on Ethernet0
OSPF: Elect BDR 172.16.255.10
OSPF: Elect DR 172.16.255.11
        DR: 172.16.255.11 (Id) BDR: 172.16.255.10 (Id)
OSPF: Send DBD to 172.16.255.11 on Ethernet0
        seq 0x10DB opt 0x2 flag 0x7 len 32
OSPF: Build router LSA for area 0, router ID 172.16.255.11
```
```

In this example, you can see the election process for the DR and BDR and the sharing of links (DBDs) with the DR.

If two routers have misconfigured the authentication type for OSPF, such as clear-text passwords on one and MD5 on the other, you’ll see the following with the previous **debug** command:

```
```
OSPF: Rcv pkt from 192.168.1.1, Serial1/0:
Mismatch Authentication type. Input packet specified
                type 0, we use type 1
```
```

However, if you have mismatched the passwords (keys) on the two OSPF routers, you’ll see something like this:

```
```
OSPF: Rcv pkt from 192.168.1.1, Serial1/0 :
Mismatch Authentication Key - Clear Text
```
```

# The debug ip ospf events Command

If you want to view OSPF events on your router, use the **debug ip ospf events** command:

```
```
Router# debug ip ospf events
4d02h: OSPF: Rcv hello from 192.168.1.100 area 0 from Serial0 192.168.1.100
4d02h: OSPF: End of hello processing
```
```