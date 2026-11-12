book, which focuses on the configuration of static ACLs on IOS devices themselves.

![](bbox=[0.14, 0.17, 0.24, 0.23])

**EXAM TIP** Remember the filtering abilities of standard versus extended ACLs in Table 17-1. Be able to describe the use of a dynamic ACL.

# ACL Processing

ACLs are basically statements that are grouped together by either a name or a number. Within this group of statements, when a packet is processed by an ACL, IOS will go through certain steps in finding a match against the ACL statements. ACLs are processed *top-down* by IOS. A packet is compared to the first statement in the ACL, and if IOS finds a match between the packet and the statement, IOS will execute one of two actions included with the statement: *permit* or *deny*.

If IOS doesn’t find a match of packet contents to the first ACL statement, IOS will proceed to the next statement in the list, again going through the same matching process. If the second statement matches the packet contents, IOS executes one of the two actions. If there is no match on this statement, IOS will keep on going through the list until it finds a match. If IOS goes through the entire list and doesn’t find a match in the ACL statements to the ACL contents, the router will *drop* the packet.

The top-down processing of ACLs brings out the following very important points:

+   - Once a match is found, no further statements are processed in the list.
+   - The order of statements is important, since after the first match, the rest of the statements are not processed.
+   - If no match is found in the list, the packet is dropped.

# Statement Ordering