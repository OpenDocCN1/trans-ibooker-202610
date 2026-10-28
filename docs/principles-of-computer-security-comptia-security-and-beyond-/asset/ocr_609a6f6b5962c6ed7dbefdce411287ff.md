This time, you’ll notice that the output shows that there were 10 matches for the second ACL statement, and those packets were let into the network. These were, of course, the two sets of five ICMP Echo Requests sent by Router1. The routers send five ICMP Echo Requests by default, unlike the Windows PCs that send four.

Packets sourced by Router1’s GigabitEthernet 0/0/0 interface, 10.3.0.98, did not meet the first ACL statement that blocks traffic sourced from the 10.1.0.0/16 network. As a result, these packets met the second permit any statement and were let through.

If you want to reset the ACL counters and try this again, from enable mode enter the following command:

```
clear access-list counters
```

2a–2j

# Step 2 Remove the standard ACL from the preceding step. Then, configure, apply, and test a new standard ACL that denies a single host.

+   a. From global configuration mode on Router2, enter no access-list 1 to remove the current ACL.

+   b. Enter access-list 1 deny 10.1.0.1 to block just PC1 from 10.4.0.0/16.

+   c. Enter access-list 1 permit any to permit any other traffic that doesn’t meet the preceding statement.

+   d. Enter END to go back to enable mode. The new access-list 1 is automatically applied as an outbound ACL to interface GigabitEthernet0/0/0 since we never removed the application of the previous access-list 1.

+   e. From PC0 (10.1.0.100), send pings to PC2 (10.4.0.2) and PC3 (10.4.0.3). The pings should succeed since just PC1 (10.1.0.1) is being blocked now, not the entire 10.1.0.0/16 subnet.

+   f. From PC1 (10.1.0.1), send pings to PC2 (10.4.0.2) and PC3 (10.4.0.3). The pings should fail since PC1 is being blocked now, not the entire 10.1.0.0/16 subnet.

+   g. Go back to Router2 and enter show access-lists. You’ll notice that