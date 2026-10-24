# Software-Defined Networking

Network virtualization changes the way we must look at network segmentation. A relatively new concept of software-defined networking (SDN) is emerging that essentially analyzes the connection between any two nodes and can filter that connection based on a defined policy. This micro-segmentation will make security more powerful when it is widespread but will certainly make certain management aspects more complex.

# Network Virtualization

Computer platforms have evolved enough that we can simulate hardware platforms such as servers, routers, and most any other network resource using software. Through software, a single piece of hardware can support multiple virtual instances, as shown in Figure 20.5. Each instance has the ability to function like the original host hardware. Virtual instances can be enabled as needed to handle demand and scalability, or to provide tremendous amounts of portability.

![](bbox=[0.22, 0.45, 0.86, 0.63])

FIGURE 20.5 Virtual Instances

Network virtualization is also a way to segment your network by creating overlay networks, (essentially a network built on top of another, physical or underlay, network). It is possible to use white box switches (generic routing and switching hardware) in these overlay networks.

Network virtualization can provide a virtual network completely separate from other network resources, creating a zone just as you would with traditional network hardware. Network virtualization can also be used to implement software-driven virtual network storage units. This is seen in storage area network (SAN) deployments.