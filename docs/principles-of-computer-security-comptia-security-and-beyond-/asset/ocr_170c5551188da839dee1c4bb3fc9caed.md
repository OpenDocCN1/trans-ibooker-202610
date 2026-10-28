![](bbox=[0.25, 0.09, 0.75, 0.6])

FIGURE 16-4 Vint Cerf and Jonathan S. Weissman at Rochester Institute of Technology (RIT)

I got a nice chuckle out of him, but he also told me his actual logic. If a closed port didn’t respond, there would be a period of unnecessary latency, where the client would simply be waiting to hear back from the server. A TCP timer would eventually kick in and close the connection, but that could take a lot of time. Sending the RST leaves nothing to the imagination and is an explicit way of saying, “Sorry, we’re closed!” Of course, Vint Cerf and Bob Kahn designed the TCP/IP suite long before hacking, cybercrime, and cybersecurity became everyday terms.

An open port will, of course, respond back with a TCP segment that turns on the SYN and ACK flags. If that weren’t the case, you’d never connect to a