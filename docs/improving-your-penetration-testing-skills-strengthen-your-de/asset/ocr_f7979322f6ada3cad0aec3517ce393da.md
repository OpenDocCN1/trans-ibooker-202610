```
Architecture : x86
System Language : pt_PT
Domain : WORKGROUP
Logged On Users : 2
Meterpreter : x86/windows
meterpreter >
```

# Port forwarding with Meterpreter

Discussing pivoting is never complete without talking about port forwarding. In this recipe, we will continue from our previous pivoting recipe and see how we can port forward the data and requests from the attacking machine to the internal network server via the target node. An important thing to note here is that we can use port forwarding to access various services of the internal server.

# Getting ready

We will start with the same scenario, which we discussed in the previous recipe. We have compromised the Linux server, and we have added the route information to forward all the data packets sent on the network through the Meterpreter session. Let's take a look at the route table:

```
msf > route

IPv4 Active Routing Table
======================

Subnet Netmask Gateway
------ ------- -------
10.0.0.0 255.255.255.0 Session 2

[*] There are currently no IPv6 routes defined.
msf >
```

So, our table is all set. Now we will have to set up port forwarding so that our request relays through to reach the internal network.