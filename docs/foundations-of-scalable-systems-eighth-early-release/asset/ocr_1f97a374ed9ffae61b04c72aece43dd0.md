DNS servers are organized hierarchically. A small number of root DNS servers, which are highly replicated, are the starting point for resolving an IP address. When an Internet browser tries to find a web site, a network host known as the local DNS server that is managed by your employer or ISP, will contact a root DNS server with the requested host name. The root server replies with a referral to a so-called *authoritative* DNS server that manages name resolution for, in our banking example, .com addresses. There is an authoritative name server for each top-level Internet domain (e.g. .com, .org, .net, etc).

Next the local DNS server will query the .com DNS server, which will reply with the address of the DNS server which knows about all the IP addresses managed by mybank.com. This DNS is queried, and it returns the actual IP address we need to communicate with the application. The overall scheme is illustrated in Figure 3-2.

![](bbox=[0.13, 0.44, 0.84, 0.85])

Figure 3-2. Example DNS Lookup for mybank.com