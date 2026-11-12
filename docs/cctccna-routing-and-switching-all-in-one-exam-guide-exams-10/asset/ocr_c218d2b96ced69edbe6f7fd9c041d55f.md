back at our example, if you have a class C network 192.168.1.0/24 and subnet it with a 26-bit mask, you have created four subnets. Using route summarization, you can summarize these four subnets back to 192.168.1.0/24. CIDR takes this one step further and enables you to summarize a block of contiguous class A, B, and/or C network numbers. This practice is commonly referred to as *supernetting*. Today’s classless protocols support supernetting. However, it is most commonly configured by Internet service providers (ISPs) that use the Border Gateway Protocol (BGP) for routing.

Figure 5-9 shows an example of CIDR. In this example, a router is connected to four class C networks: 192.168.0.0/24, 192.168.1.0/24, 192.168.2.0/24, 192.168.3.0/24. It is summarizing those routes as 192.168.0.0/22.

![](bbox=[0.12, 0.39, 0.88, 0.68])

Figure 5-9 CIDR example

Let’s break down how this is performed with CIDR. As I mentioned, CIDR is an extension of VLSM and route summarization so the technique to summarize those class C networks is the same as that with route summarization. Looking at the network addresses in binary, you identify the common bits to determine the summary network. Looking at the binary that follows, you can see that all the bits found in the first two octets of the four networks are the same. In the third octet, the first 6 bits are the same as well, totaling 22 bits that are common to all networks (so that is our