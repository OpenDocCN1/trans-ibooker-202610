# Let’s Do This!

Extended ACLs can filter by three or four criteria: source IP, destination IP, protocol, and port, with port being the only optional parameter. This gives you more granular control over the rules.

Let’s say we want to block HostA from accessing the Apache Web Server on ServerA, but not any other services on ServerA, as shown in Figure 10-2.

![](bbox=[0.115, 0.26, 0.88, 0.67])

FIGURE 10-2 Extended ACL topology example

We need an extended ACL in this case. Let’s walk through an ACL that’s extended. The extended ACL statement starts off the same way as a standard ACL statement, with access-list.

For an extended ACL, the number of the ACL falls within the inclusive range of 100 to 199. In this case, we’re using 100. Next we have, just like in a standard ACL, the keyword permit or deny, depending on what we’re trying to do: