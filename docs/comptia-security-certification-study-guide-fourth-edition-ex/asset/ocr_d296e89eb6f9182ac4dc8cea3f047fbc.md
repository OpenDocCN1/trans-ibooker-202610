# Cisco Standard Access Lists

A standard access list is assigned a number from 1 to 99 and can permit or deny traffic based only on the source IP address. Two steps are needed to configure standard access lists—you must first define the access list and then apply it to an interface on the router.

The following syntax is used to create two rules in standard access list 23, which is denying packets with the source IP address of 192.168.10.7 and any packets with a source address on the 10.0.0.0 network. The last rule is permitting all other traffic. Note that the permitting-all-other-traffic rule is last because if it were first, then all traffic would be allowed (the first rule that matches a packet is the rule the packet follows).

```
enable
configure terminal
access-list 23 deny 192.168.10.7 0.0.0.0
access-list 23 deny 10.0.0.0 0.255.255.255
access-list 23 permit 0.0.0.0 255.255.255.255
```

Now that I have created access list 23, I need to apply that to an interface for inbound or outbound communication. I can block unwanted traffic from entering the network with an inbound rule, but I can also block traffic from leaving the network with an outbound rule. The following commands are used to apply access list 23 to my Fast Ethernet interface for inbound communication:

```
interface FastEthernet0/0
ip access-group 23 in
```

Cisco routers have an implicit deny all rule at the bottom of the access list, which is why I put at the bottom my own rule to permit all traffic. This means that unless otherwise specified, all traffic will be allowed.

# Cisco Extended Access Lists

Extended access lists are configured in a similar way to standard access lists, with the exception that their assigned numbers start at 100 and above. The extended access list can control traffic based on the source and destination IP addresses, but also based on the protocol information in the packet.

The following command outlines the basic idea of a rule being added to an extended access list. Notice that you start with the access-list command, assign a number to the access list, and specify whether you are permitting or denying traffic. What is new here is that you can specify a protocol such as TCP, UDP, or IP if you want. Then you specify the source IP address of the packet the rule is to apply to and the source wildcard mask. You then specify the destination IP address and the destination wildcard mask. Finally, add an operator such as EQ for “equals” and then the port number.