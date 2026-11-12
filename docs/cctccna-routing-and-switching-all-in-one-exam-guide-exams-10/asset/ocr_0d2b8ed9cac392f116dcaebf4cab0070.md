Unlike TCP and UDP, ICMP doesn’t use ports. Instead, ICMP uses message types. And where TCP and UDP extended ACLs enable you to specify both source and destination ports, ICMP enables you to enter an ICMP message. Table 17-8 shows some of the common ICMP messages and brief descriptions. You can enter the ICMP message by either its name or its number. If you omit the ICMP message type, all message types are included.

| Message Type | Message Description |
|---|---|
| administratively-prohibited | Indicates the packet was filtered |
| echo | Used by ping to check a destination |
| echo-reply | Response to an echo message created by ping |
| host-unreachable | Subnet is reachable, but the host is not responding |
| net-unreachable | Network/subnet is not reachable |
| traceroute | Filters on traceroute information when ICMP is used |

Table 17-8 Common ICMP Messages

![](bbox=[0.14, 0.52, 0.24, 0.6])

**TIP** If you execute the **no access-list** command, followed by the ACL number, the entire ACL and its referenced commands are deleted. What most administrators don’t realize, or forget, is that if you preface any numbered ACL statement with the **no** parameter, it has exactly the same effect: the entire ACL is deleted. For example, executing the **no access-list 100 permit tcp any any** command causes the router basically to ignore everything after the **100** parameter, causing the router to execute the command as if it were **no access-list 100**!

![](bbox=[0.14, 0.82, 0.24, 0.9])