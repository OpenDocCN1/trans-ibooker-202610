
the subnets to determine what network you are dealing with. The subnets are 138.97.0.0, 138.97.16.0, 138.97.32.0, and so on. Note that the IP given in the question is 138.97.25.10, which is on the 138.97.16.0 network. To answer the question, you have to pick an address that falls between 138.97.16.0 and 138.97.32.0.

8. D. CIDR notation is used to identify how many bits make up the network ID in the IP address. With CIDR notation, you specify the number of bits that make up the network ID by placing a forward slash (/) after the IP address and then the number of bits that make up the network ID: for example, 10.34.56.78/16.

9. B. Because /27 is being used to identify the number of bits that make up the subnet mask, you know the network is subnetted (class C has a /24 by default). In this example, the network that the router is on is the 216.83.11.64 network, and 216.83.11.87 is the only valid address on that network listed here.

10. D. Because /20 is used to identify the number of bits that make up the subnet mask, you know the network is subnetted (class B has a /16 by default). Using the shortcut method, you know that /20 gives four additional bits to the subnet mask with 16 being the increment. The network range you are working with is 131.107.48.0, and you want to know the last valid address of that subnet. A quick way to determine this is to use the increment to determine the next network ID of 131.107.64.0 and then work backward from that, finding the last valid address. The address before 131.107.64.0 is 131.107.63.255, which is the broadcast address of the 131.107.48.0 network. The address before that, 131.107.63.254, is the last valid address.

## Performance-based Answer

1. This is a tricky question, and you are sure to see similar questions to this on the Cisco exams where you need to identify network IDs, broadcast addresses, and host address. To answer this question, the first thing you do is focus on the address given of 200.15.36.0/28. This is a class C address with 4 subnet bits. Next, identify the value of the last bit of the 4 bits, which is the increment. In this case, it is 16. Then, write out on a piece of paper the different network IDs:

- 200.15.36.0
- 200.15.36.16
- 200.15.36.32
- 200.15.36.48
- 200.15.36.64
- 200.15.36.80

Now you know the different network IDs. The number before each of these network IDs would be the broadcast address of the previous network, and numbers in between are host addresses.

| Broadcast Address |  |
|---|---|
| 200.15.36.79 | 200.15.36.31 |

| Network ID |  |
|---|---|
| 200.15.36.16 | 200.15.36.48 |

| Host Address |  |
|---|---|
| 200.15.36.22 | 200.15.36.60 |

## Cisco Device Basics

In this chapter, you will

- Look at Cisco hardware and software
- Learn about memory types
- Connect to a Cisco device
- Start a Cisco device
- Learn about service-related tasks

Now that you understand networking concepts and TCP/IP addressing, let’s dive into the world of working with Cisco devices. In this chapter you will learn more about the hardware aspects and startup process of Cisco devices. Then, Chapter 7 will focus on how to navigate through the operating system and perform administrative tasks.

### Working with Cisco Devices and Software

Cisco provides several families of devices that provide different features and functionalities. For example, Catalyst products include a number of models of Cisco’s layer 2 and layer 3 switches. In this section, you will learn about some of Cisco’s different product lines that you may be working with.

### Types of Cisco Devices

A number of different types of Cisco devices are currently on the market, with each device supporting specific features, hardware modules, and interfaces. The following is a quick rundown of some of the devices you may encounter.

### Cisco Nexus Series Switches

Cisco’s Nexus series switches are designed for data centers to improve performance with high availability features that enable all systems to access network and storage resources. Nexus series switches run the NX-OS network operating system.

- **Cisco Nexus 9000 Series** These switches provide speeds of 1, 10, 25, 50, 100, and 400 Gbps in the data center. A number of features help automate the network, including software-defined networking (SDN).
- **Nexus 7000 Series** These modular switches offer performance of 10-, 40-, and 100-Gigabit networking. You can add 7000 series modules, such as the Cisco Network Analysis Module (NAM), which enables you to analyze packets and troubleshoot traffic issues.
- **Nexus 3000 Series** These Nexus 3000 series switches are designed for general layer 2 and layer 3 switching with support for 1, 10, 25, 40, 100, and 400 Gbps. Some models in this series support programmability and network telemetry features.

### Cisco Catalyst Series Switches

The Cisco Catalyst series switches are designed as LAN access switches and are used at the core and distribution layers.

- **Catalyst 9000 Series** These switches are designed for campus deployments and can provide access to a network via wired and wireless functionalities.
- **Catalyst 6800 Series** Designed as campus backbone switches, these devices offer a number of slots for inserting different hardware modules, and they support small form-factor pluggable (SFP) and SFP+ transceivers.
- **Catalyst 6500 E-Series** These enhanced switches support 80 Gbps in each slot.
- **Catalyst 3850, 3650, 2960, and 1000 Series** These switches are designed as access-layer switch models and support a number of different modules and configurations. For example, the 3650 modules support 1, 10, and 40 Gbps with SFP+ uplink ports to add an SFP+ transceiver of your choice.

### Cisco ASR Series Routers

Cisco Aggregation Services Router (ASR) series routers are used in large enterprise networks or service providers. They are best suited for high-bandwidth application environments because they handle 100 Gbps. ASR series routers are powered by Cisco’s IOS XR and IOS SE operating systems. Following are some popular ASR models:

- **Cisco 8000 Series** These routers are designed for performance with up to 400GB throughput. They are cloud enhanced and able to integrate into management tools.
- **Cisco ASR 9000 Series** These high-end routers are commonly used at the core layer or at the edge of the network. The ASR 9000 routers are powered by the IOS XR operating system.
- **Cisco ASR 1000 Series** These routers are designed to be used at the edge of the network, and they are powered by the IOS XE operating system.

### Cisco ISR Series Routers

The Integrated Services Router (ISR) series focuses on servicing small to midsize businesses with small to midsize networks. ISR routers are designed as edge routers to provide connectivity to branch offices. They are powered by the Cisco IOS operating system.

- **Cisco 4000 Series** These routers combine network, compute, and WAN services by supporting different connectivity options such as Gigabit Ethernet, T1/E1, and xDSL interfaces.
- **Cisco 1100 Series** These small, fanless routers provide connectivity and security features to connect branch offices to their main network.
- **Cisco 900 Series** These routers are designed for small offices and have four LAN ports as well as WAN ports and security services such as virtual private network (VPN), dynamic multipoint VPN (DMVPN), and a firewall.
- **Cisco 800 Series** These routers combine a switch and router together and add wireless capabilities.

### Other Cisco Devices

The list of Cisco devices that exists continues with devices that are designed to operate in a cloud environment or devices that provide a large-scale platform for service providers. The following are some other Cisco devices you should be familiar with for the Cisco CCT exam:

- **MDS 9000 Series** The Cisco MDS 9000 series of switches are multilayer switches used for connectivity to storage environments. They are designed to offer network designers the capabilities to build highly available, secure, and scalable storage networks.
- **Meraki Switches** Meraki switches are the Cisco product line focused on management of the switches from the cloud. From a central dashboard that is accessed from across the web, administrators are able to manage all the switches in the organization.
- **NCS 5500 Series** The Cisco *Network Convergence System* (NCS) is designed for high-scale data center aggregation offering 100 Gbps architectures. It is a router product that uses the IOS XR operating system providing a number of services, such as application hosting for large enterprises or service providers.
- **NCS 5000 Series** An older model NCS routing solution, the NCS 5000 series is a routing platform designed for enterprise networks and service providers. These routing devices provide 10 Gbps throughput using the Cisco IOS XR software.

### Identifying Device Model Numbers and Serial Numbers

You can identify the model and serial number of your Cisco device by looking at the stickers typically located on the back of the device chassis (though they are sometimes displayed on the front). In Figure 6-1, you can see that my device (the PID) is a WS-C3750G-24TS-S model. You can also see the serial number (SN): FDO1125Z6VQ.
