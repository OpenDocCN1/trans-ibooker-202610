![](bbox=[0.15, 0.07, 0.82, 0.44])

| MAC Table |
|---| 
| port1....dddd.dddd.dddd |
| port2....aaaa.aaaa.aaaa |
| port3....bbbb.bbbb.bbbb |
| port4....cccc.cccc.cccc |

Figure 1-1 A switch is a central connectivity device on the network.

You will learn more about switches in Chapter 9.

# Router

A switch is used to connect all systems together in a LAN type of setup, but what if you want to send data from your network to another network or across the Internet? That is the job of a router. A router sends, or routes, data from one network to another until the data reaches its final destination. Note that although switches look at the MAC address to decide where to forward a frame, routers use the IP address to determine what network to send the data to.

The router has a routing table, which is a list of all the networks that it knows how to reach. A router also includes a gateway of last resort (GWLR) setting that is used if the router does not have a route to the destination in its routing table.

Looking at Figure 1-2, you can see two routers labeled R1 and R2. Note that R1 does not have a physical connection to the 14.0.0.0 network. If computer A wanted to send data to computer D, here’s what would happen: