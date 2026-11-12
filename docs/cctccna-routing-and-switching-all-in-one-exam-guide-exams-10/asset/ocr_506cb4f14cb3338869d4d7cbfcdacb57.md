*trunk* port (more on trunk ports later in this chapter). Second, remember that a VLAN is a broadcast domain, so if you want to optimize traffic, it is recommended that a VLAN not exceed 500 devices.

# Scalability

Through segmentation of broadcast domains, VLANs increase your network’s scalability. Because VLANs are a logical construct, a user can be located anywhere in the switched network and still belong to the same broadcast domain. If you move a user from one switch to another switch in the same switched network, you can still keep the user in his or her original VLAN. This includes a move from one floor of a building to another floor or from one part of the campus to another. The limitation is that the user, when moved, must still be connected to the same layer 2 network.

![](bbox=[0.14, 0.43, 0.26, 0.52])

**NOTE** VLANs provide for location independence in a switched network: many logical networks can use the same network infrastructure. This flexibility makes adding, changing, and moving of networking devices a simple process. It also enables you to group people together, perhaps according to their job function, which makes implementing your security policies easier. Logically separating people using VLANs provides additional security, since traffic must traverse a layer 3 device to go from one VLAN to another, where you can use an access control list to filter traffic.

# VLANs and Traffic Types

Many network administrators use VLANs not only to separate different types of user traffic (commonly separated by job function), but also to separate based on the type of traffic, placing network management,