![](bbox=[0.13, 0.08, 0.87, 0.35])

# Figure 4.8: Resolving a name to IP

Did you know you can also use the ping command to perform the opposite? What if you want to query DNS to find out what hostname the 10.10.10.11 IP address maps back to?

```
ping -a 10.10.10.11
```

![](bbox=[0.13, 0.5, 0.87, 0.79])

# Figure 4.9: Querying DNS to find a hostname from an IP

This reverse lookup, converting IP addresses back to names, has shown us that the IP address 10.10.10.11 has a reverse pointer record mapping back to our DC2 server. This type of lookup is handled by a Reverse Lookup Zone inside DNS.