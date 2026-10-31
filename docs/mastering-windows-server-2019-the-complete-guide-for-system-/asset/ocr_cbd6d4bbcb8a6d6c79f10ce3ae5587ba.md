The following is our completed route add statement:

```
route add -p 192.168.16.0 mask 255.255.255.0 10.10.10.1 if 5
```

![](bbox=[0.13, 0.16, 0.87, 0.28])

Figure 7.16: Running the complete route statement

If you now run a route print command, you can see our new 192.168.16.0 route listed in the Persistent Routes section of the routing table, and we can now send packets into that subnet from this new server. Whenever our server has traffic that needs to go into the 192.168.16.x subnet, it will send that traffic out via the internal NIC, toward the router running on 10.10.10.1. The router then picks up the traffic from there and brings it into the 192.168.16.0 subnet:

```
===============================================
Persistent Routes:
Network Address      Netmask      Gateway Address   Metric
0.0.0.0              0.0.0.0      10.10.10.1        Default
192.168.16.0         255.255.255.0 10.10.10.1        1
===============================================
```

Figure 7.17: Sending traffic to the 192.168.16.0 subnet

# Deleting a route

Occasionally, you may key in a route statement incorrectly. The best way to handle this is to simply delete the bad route and then rerun your route add statement with the correct syntax. There are possibly other reasons why you might need to delete routes every now and then, so you'll want to be familiar with this command. Deleting routes is much simpler than adding new ones. All you need to know is the subnet ID for the route that you want to remove and then simply route delete <SUBNET_ID>. For example, to get rid of our 192.168.16.0 route that we created while we were working inside the Command Prompt, I would simply issue this command:

```
route delete 192.168.16.0
```