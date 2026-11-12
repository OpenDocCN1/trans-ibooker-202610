
traffic is sent to all ports on the hub, it is possible that if several systems send data at the same time, the data could collide on the network. For this reason, all network ports on a hub (and any devices connected to those ports) are considered parts of a *single collision domain*. This also means that when you cascade a hub off another hub, all hubs are part of the same collision domain.

If you were using a switch to connect the five systems together, each port on the switch would create its own network segment (like a bridge breaks the network down into different network segments). When data is sent by a system connected to the switch, the switch sends the data only to the port that the destination system resides on. For this reason, if another system were to send data at the same time, the data would not collide. As a result, each port on the switch creates a *separate collision domain*.

## Broadcast Domain

A broadcast domain is a group of systems that can receive one another’s broadcast messages. When using a hub to connect five systems in a network environment, if one system sends a broadcast message, the message is received by all other systems connected to the hub. For this reason, all ports on the hub create a *single broadcast domain*.

Likewise, if all five systems were connected to a switch and one of the systems sent a broadcast message, the broadcast message would be received by all other systems on the network. Therefore, when using a switch, all ports are part of the same broadcast domain as well.

If you wanted to control which systems received broadcast messages, you would have to use a router that does not forward broadcast messages on to other networks. You could also use virtual LANs (VLANs) on a switch, with each VLAN being a different broadcast domain.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_54_0.png)

**EXAM TIP** Be sure to know the difference between a broadcast domain and collision domain for the CCT and CCNA certification exams. Also, remember that a router is the device you use to create multiple broadcast domains.

## Exercise 1-1: Identifying Network Components

In this exercise, you review the different network components by matching the component on the right with the definition on the left.

| Definition | Component |
|---|---|
| ____ A layer 3 device responsible for sending data from one network to another | A. Switch |
| ____ A device that detects suspicious activity and then takes corrective action | B. Collision domain |
| ____ A layer 2 device used to connect all systems to the network | C. DHCP |
| ____ A group of systems that can receive one another's broadcast messages | D. Firewall |
| ____ A service that assigns IP addresses to other systems and devices on the network | E. Router |
| ____ The point where the service provider equipment connects to your building | F. Hub |
| ____ A group of systems in which separate data transmissions could potentially collide | G. IPS |
| ____ Used to protect a system or device from unwanted traffic | H. Demarc |
| ____ A layer 1 device used to connect all systems to the network | I. Broadcast domain |

## Network Design Models

A well-designed network is hierarchical in structure, where each layer in the hierarchy is focused on a specific job function. A well-designed network is also modular, in the sense that it is easy to add components to the network, so that as the company grows, the network can grow. The well-designed network should also be flexible and resilient, meaning that it is always available and includes some form of redundancy.

When designing Cisco networks, we usually follow two common design models: the *three-tier architecture model* and the *collapsed core architecture model*. Other network architectures are discussed here as well.

## Three-tier Architecture

The three-tier architecture model is a common model used by Cisco network professionals to design a well-performing Cisco network. Following are the three layers:

- **Access layer** This layer is at the bottom of the hierarchy; it enables end users to connect to the network via switches. The access layer provides features that enable you to isolate traffic between different types of users and divide broadcast domains. A switch located on each floor of a building would represent this layer.
- **Distribution layer** The second layer from the bottom, or the middle layer, enables you to control who can access what part of the network by using features such as routing, access control lists (ACLs), and network access policies. A distribution layer could be a central layer 3 switch residing in two buildings, in which each switch could access the layer 3 switch of the other building to connect.
- **Core layer** The top layer of the model represents the backbone of the network. This layer is typically used to connect the networks of different buildings together. It includes a number of core routers that are responsible for delivering traffic to and from the network.

## Collapsed Core (Two-tier) Architecture

A small organization may not need to use a three-layer model, so it can combine the distribution and core layers into a single layer known as a *collapsed core layer*. This makes the collapsed core layer a two-layer model, comprising the *access layer* and the *collapsed distribution/core layer*. In a typical scenario, this may be used in a small company that uses only a few floors in a building for their network. Each floor would have a switch (access layer) that enables clients on that floor to connect to the network.

Each floor switch would then connect to a central switch in the building, which is connected to a router to access the WAN or Internet.

## Spine-leaf Architecture

Another example of a two-tier architecture is the *spine-leaf architecture*, in which the lower layer is the leaf layer that contains the access switches that enable devices to connect to the network. The leaf layer switches then connect to the spine layer switches, which connect all the switches to the rest of the network. The spine-leaf model is shown in Figure 1-6. Note that the leaf layer switches are access layer switches with a full mesh topology that connects to the spine layer switches.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_57_0.png)

Figure 1-6 Spine-leaf network topology

## Small Office/Home Office

A SOHO network is a totally different network topology that is designed for a very small number of devices that need access to a network and the Internet. A SOHO network typically involves a high-speed Internet connection into a home or office that connects to an ISP’s modem. The ISP’s modem is connected to a WAN/Internet port on a wireless router, which usually has four switch ports that you can use to connect clients using unshielded twisted pair (UTP) cabling. A wireless router in a SOHO network could also provide a number of services, such as being a DHCP server, a NAT device, a firewall, or a wireless access point.

## On-premises vs. Cloud

In most discussions of networking environments today, you will hear the terms *on-premises* versus *the cloud*. On-premises networking capabilities or devices exist on your physical network at your office location. If networking capabilities or devices reside in the cloud, this occurs at a cloud provider’s network. For example, you may have a wireless LAN controller that is used to configure multiple wireless access points from a central point as an on-premises device (in your office), or you may use a cloud service, which means that these services are provided to your location by a cloud provider. One of the benefits of cloud-based solutions is that the provider typically handles issues such as high availability of the service so you don’t have to.

## Network Topologies

When you connect devices into a network, you need to decide which topology to use. The *topology* of the network is the layout of the network, and you can choose from a handful of network topologies. Figure 1-7 shows examples of four topologies used by different media types.
