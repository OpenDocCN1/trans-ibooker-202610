+   - The summary route may contain subnets that are not in use, and the router may receive packets for routes that are not in use, although they do mathematically fall under the summary route. In this case the router will drop those packets.
+   - The router may choose a different pathway for the route because the pathway is more specific than that of the summary route.

Don’t be too worried about these routing issues at this point. I bring them up only to provide a complete advantages/disadvantages discussion. This chapter is more about the design behind the numbers, so let’s talk about how route summarization is performed.

To calculate the summary route, you first list out each of the network IDs and then convert them to their binary forms. Our four subnets can be listed as follows:

```
192.168.1.0/26      11000000.10101000.00000001.00000000
192.168.1.64/26     11000000.10101000.00000001.01000000
192.168.1.128/26    11000000.10101000.00000001.10000000
192.168.1.192/26    11000000.10101000.00000001.11000000
```

Next, you identify, from left to right, how many of the bits are the same with those four network IDs. That is your summary route:

```
192.168.1.0/26      11000000.10101000.00000001.00000000
192.168.1.64/26     11000000.10101000.00000001.01000000
192.168.1.128/26    11000000.10101000.00000001.10000000
192.168.1.192/26    11000000.10101000.00000001.11000000
```

Notice that in the first two bits of the fourth octet, the bits start to change for each subnet. All of the bits before that point are the common bits to create our summary route. When you convert the common bits back to decimal, you get a summary route of 192.168.1.0/24.

# Classless Interdomain Routing (CIDR)

Classless Interdomain Routing (CIDR), specified in RFC 2050, is an extension to VLSM and route summarization. With VLSM, you can summarize subnets back to the class A, B, or C network boundary. Looking