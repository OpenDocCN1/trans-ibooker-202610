switches and that there are no intervening layer 3 devices. Logically speaking, VLANs are also subnets.

**virtual private network (VPN)** A special type of secure network that is used to provide a secure, protected tunnel or connection across a public network, such as the Internet. The network part of the term refers to the use of a public network, such as the Internet, to implement the WAN solution. The virtual part of the term hides the public network from the internal network components, such as users and services. The private part of the term specifies that the traffic should remain private—not viewable by eavesdroppers in the network. This is accomplished using encryption to keep the data confidential.

**Virtual Router Redundancy Protocol (VRRP)** This protocol performs a function similar to Cisco’s proprietary HSRP. The one major downside to HSRP is that it is a proprietary protocol; VRRP, however, is an open standard and is defined in IETF RFC 2338. Like HSRP, VRRP has end stations that use a virtual router for a default gateway. One main difference between VRRP and HSRP is that HSRP uses a virtual IP address for the default gateway, whereas VRRP can use either a virtual IP address or the interface address of the master router. If a virtual IP address is used, an election process takes place to choose a master router. VRRP is supported for Ethernet media types as well as in VLANs and MPLS VPNs.

**virtual type terminal (VTY)** A logical line on a Cisco device that is used to manage telnet and SSH connections.

**VLAN Trunk Protocol (VTP)** A proprietary Cisco protocol used to share VLAN configuration information between Cisco switches on trunk connections. VTP enables switches to share and synchronize their VLAN information, which ensures that your network has a consistent VLAN configuration.

**voice over IP (VoIP)** A group of technologies that are designed to carry voice and video over an IP network.

**VTP client mode** A switch in VTP client mode cannot make changes to its VLAN configuration itself—it requires a switch in VTP server mode to