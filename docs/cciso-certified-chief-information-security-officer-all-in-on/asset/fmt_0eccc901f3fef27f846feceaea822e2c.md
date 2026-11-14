
# WANs and LANs

WANs use telecommunication circuits suitable for communication over long distances (tens or thousands of miles). Up until the last decade, these technologies included circuit- or packet-switched networks that use transport protocols including Asynchronous Transfer Mode (ATM), SONET, Frame Relay, and X.25. However, in recent years these technologies have been largely replaced by IP-based technologies including *metro Ethernet* *(MetroE)* and *virtual private LAN service* *(VPLS)*. LANs use communication equipment and devices suitable for shorter distances. However, both LANs and WANs use networking protocols including Transmission Control Protocol/Internet Protocol (TCP/IP) to manage the delivery of information from a source, across the networks, to the intended destination.

One major difference between WANs and LANs is that WANs are usually provided by a third-party WAN service provider or carrier, whereas LANs are usually operated and maintained in-house by the organization.

## WAN Topologies

The service provider provides the WAN infrastructure using a choice of topologies, including

- **Point-to-point** A point-to-point topology consists of a single path or circuit between two endpoints. In this case the organization connects to the service provider’s network that provides a single path through the network to the other side, which may be a peer facility or another LAN.
- **Hub and spoke** A hub and spoke arrangement is used when the organization’s facility requires connections to multiple locations. In this case, while using multiple point-to-point connections is an option, hub and spoke provides a lower-cost alternative. The organization connects to a hub that maintains connections with multiple sites. This topology is also called *single homed*, which refers to the use of a single hub. A *dual-homed* arrangement uses two hubs at the organization’s facility, each of which maintains its own spoke of connections to the other locations for redundancy and improved performance.
- **Full mesh** A full-mesh network is also used to provide connectivity between multiple locations. It is implemented when each location (node) has circuits connecting it to every other location. This arrangement provides the greatest redundancy and flexibility.

The choice of which topology to select depends on the nature of the business, performance considerations, and costs. Point-to-point is the simplest and lowest cost option but has limited flexibility to adjust for capacity and offers no redundancy. Mesh can be the most expensive but provides better redundancy and control for load balancing and performance optimization.

## WAN Technologies

The portion of the WAN link that interfaces with (or terminates at) the organization’s LAN perimeter is called the *local loop* and is sometimes referred to as the “last mile” of the connection. [Figure 4-4](#ch4.xhtml#fig4_4) provides an illustration.

![Images](Images/F04-04.jpg)

**Figure 4-4** WAN last-mile components

The local loop terminates at the organization’s demarcation point, which can be as simple as a junction block used to connect the local loop to the facility. The *data communications equipment (DCE)* is a device that interfaces the local loop with the *data terminal equipment (DTE)*, which can be a computer or router that sits at the perimeter of the LAN. The characteristics of the local loop connection are dependent on the type of communication link and the service provider network used.

Service provider networks can implement any of the topologies discussed previously. They also can use a variety of connection technologies as follows:

- **Leased lines** These are physical point-to-point communication lines connecting two locations. Dedicated lines are expensive but are still widely used. These lines are leased from providers. The T1 links carry data at 1.544 Mbps and T3 links (also called Digital Signal 3 or DS3) are 44.736 Mbps. T3 links carry 28 multiplexed T1 channels.
- **Circuit-switched networks** These networks provide a dedicated circuit path between the two ends of a call. A common circuit-switching technology is the Integrated Services Digital Network (ISDN), which is a switched telephone network that supports data and voice communications. ISDN was widely used in the early 2000s but has largely been replaced by much faster services like cable, fiber, or DSL. Another example of circuit-switched communications is the public switched telephone network (PSTN) used primarily for voice and low-speed data communications.
- **Packet-switched networks** Packet switching uses a shared network instead of dedicated circuits provided by leased lines or circuit switching. Since the network is shared between many service provider customers, data is placed into *packets*, which are *routed* to their proper destinations. Packet-switched networks can also be used to provide virtual circuits, which are logical connection paths between two network devices. Examples of packet switching connections include X.25, Frame Relay, ATM, and Metro Ethernet. The former technologies are still in use but are diminishing in favor of Metro Ethernet and other Ethernet-based technologies.

Other WAN communication technologies include DSL, cable and FTTP, satellite, and wireless:

- **DSL** *Digital subscriber lines* provide data communication over telephone lines. Although many such circuits have been supplanted in recent years by cable or fiber services, DSL is still widely used and available in many areas. Asymmetrical DSL (ADSL) refers to DSL with asynchronous speeds whereby data travels downstream faster than upstream. ADSL is usually used by residential users. With symmetrical DSL (SDSL) data travels upstream and downstream at the same rate. Higher-speed versions of DSL are high data rate DSL (HDSL) and very high data rate DSL (VDSL). Speeds can reach up to 1000 Mbps and beyond as DSL technology continues to evolve. SDSL is usually used by businesses.
- **Cable and FTTP** The term *cable* or *cable Internet* refers to broadband Internet access provided by cable television providers. Such providers also act as the *Internet service provider (ISP)*. Cable uses either broadband electrical or fiber to the premises (FTTP) technology as the medium of the last-mile link to the user’s (organization or residence) facility. Communication rates are usually asynchronous and can reach up to 1 Gbps per link.
- **Satellite** Business use of satellites for Internet and business-to-business connectivity is growing despite some technical limitations. Modern satellite links can provide downlink speeds of up to 1000 Gbps and uplink speeds of up to 1000 Mbps. Satellite communication performance can be hindered by weather, which can impact the line of sight necessary for communication. And satellite communication suffers from latency due to the long distances and hops between the satellites, ground stations, and premise equipment. New technologies such as low-Earth orbit (LEO)–based services may result in better performance and may lower costs in the near future.
- **Wireless** Wireless WANs differ from the more ubiquitous wireless LANs in that they use cellular technologies enabling communication over longer distances. Wireless LANs use cellular technologies such as 2G, 3G, 4G LTE, and 5G. Since wireless WANs are not subject to the physical protections of wired technologies, they typically use authentication and encryption to protect communications from interception or eavesdropping.

## LAN Infrastructure
