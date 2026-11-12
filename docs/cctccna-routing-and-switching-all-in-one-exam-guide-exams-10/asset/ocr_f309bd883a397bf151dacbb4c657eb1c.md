**EXAM TIP** For the CCNA exam, remember the numbers you can use for IP ACLs. Standard ACLs can use numbers ranging from 1 to 99 and 1300 to 1999, and extended ACLs can use numbers ranging from 100 to 199 and 2000 to 2699.

The *conditions* in an ACL statement tell the router what contents in the packet need to match in order for the router to execute the action (**permit** or **deny**). The conditions can include matching of IP addresses and protocol information. When IOS compares a packet to the conditions, if it finds a match, no more ACL statements are processed; if it doesn’t find a match, IOS proceeds to compare the packet to the next ACL statement in the list.

# Matching on Addresses: Wildcard Masks

When dealing with IP addresses in ACL statements, you can use *wildcard masks* to match on a range of addresses instead of manually entering every IP address that you want to match on.

First, a wildcard mask is *not* a subnet mask. Like an IP address or a subnet mask, a wildcard mask is composed of 32 bits. Table 17-3 compares the bit values in a subnet mask and a wildcard mask. With a wildcard mask, a *0* in a bit position means that the corresponding bit position in the address of the ACL statement *must* match the same bit position in the IP address in the examined packet. A *1* in a bit position means that the corresponding bit position in the address of the ACL statement does *not* have to match the bit position in the IP address in the examined packet. In other words, the wildcard mask and the address in the ACL statement work in tandem. The wildcard mask tells the router which addressing bits in the address of the ACL statement must match the bits in the packet to which it is being compared.

| Bit Value | Subnet Mask | Wildcard Mask |
|-----------|-------------|---------------|
| 0 | Host component | Must match |
| 1 | Network component | Ignore |

**Table 17-3** Subnet Mask versus Wildcard Mask