
## Exercise 14-2: Configuring OSPFv3 Routing

In this exercise, you will remove the IPv6 static routes created in the previous exercise and then configure the OSPFv3 routing protocol to share the IPv6 routing tables between the two routers.

- Ensure that all Cisco routers are up and running.
- Console into VAN-R1 and remove the static route by entering the following commands:
- Verify that the route has been removed with the do show ipv6 route command.
- Connect to the console port of LA-R1 and remove the static route with the following commands:
- From the LA-R1 router, see if you can ping the address of 2001:aaaa:bbbb:1::1. (You should not be able to, because the route is gone that gives the router knowledge of how to reach that system.)
- To configure OSPFv3 on VAN-R1, enter the following commands:

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#no ipv6 route 2001:aaaa:bbbb:3::/64
```

```
LA-R1#enable
LA-R1#config term
LA-R1(config)#no ipv6 route 2001:aaaa:bbbb:1::/64
```

- To configure the interfaces for area 0, enter the following commands:

```
VAN-R1(config)#ipv6 unicast-routing
VAN-R1(config)#ipv6 router ospf 1
VAN-R1(config-rtr)#router-id 0.0.0.1
VAN-R1(config-rtr)#exit
VAN-R1(config)#interface g0/0
VAN-R1(config-if)#ipv6 ospf 1 area 0

VAN-R1(config-if)#interface s0/3/0
VAN-R1(config-if)#ipv6 ospf 1 area 0
VAN-R1(config-if)#exit
VAN-R1(config)#exit
```

- To configure OSPFv3 on LA-R1, enter the following commands:

```
LA-R1>enable
LA-R1#config term
LA-R1(config)#ipv6 unicast-routing
LA-R1(config)#ipv6 router ospf 1
%OSPFv3-4-NORTRID: OSPFv3 process 1 could not pick a router-id, please configure manually
LA-R1(config-rtr)#router-id 0.0.0.2
LA-R1(config-rtr)#exit
LA-R1(config)#interface g0/0
LA-R1(config-if)#ipv6 ospf 1 area 0
LA-R1(config-if)#interface s0/3/0
LA-R1(config-if)#ipv6 ospf 1 area 0
LA-R1(config-if)#exit
LA-R1(config)#exit
LA-R1#
01:51:40: %OSPFv3-5-ADJCHG: Process 1, Nbr 0.0.0.1 on Serial0/3/0 from LOADING to FULL, Loading Done
LA-R1#
```

- From LA-R1, enter ping 2001:aaaa:bbbb:1::1 to test connectivity to the :1 subnet. It should be successful, because OSPF has shared the routing tables between the two routers.

- Use the following commands to verify the configuration of OSPFv3 on both routers:

```
show ipv6 interface brief
show ipv6 route
show ipv6 ospf
```

## Chapter Review

This chapter focused on an introduction to IPv6. Because of the limited number of remaining addresses in IPv4, IPv6 was designed to bring the Internet into its next generation. IPv6 addresses use eight sets of four hexadecimal addresses (16 bits in each set), separated by a colon (:), like this: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx. If you have successive fields of zeroes in an IPv6 address, you can represent them by using two colons (::). However, you can use this shorthand only once in the address.

IPv6 addresses come in three basic types: anycast, multicast, and unicast. An anycast address is very different from an IPv4 broadcast: it represents a one-to-the-nearest interface, where many interfaces can share the same address. A multicast address is similar to a multicast in IPv4: one-to-many. A unicast address represents a single interface. Global unicast addresses are 2000::/3. A loopback is ::1. Site-local private addresses range from FEC:: through FFF::, and link-local addresses range from FE8:: through FEB::.

Many of the IPv4 fields in the IP header have been deprecated and are no longer used today. The IPv6 header has been simplified and streamlined to make it more efficient for today’s networks. Some protocols, such as ARP and IGMP, have also been deprecated. Their functions are now performed by ICMPv6, which also has some new functions, including router, DNS server, and neighbor discovery.

The most common way of assigning static addresses to an interface is to use the EUI-64 method. DHCPv6 and stateless autoconfiguration enable a device to acquire an address dynamically. Stateless autoconfiguration accomplishes this by requesting that a router give the subnet ID and the device using EUI-64 to acquire an interface ID dynamically.

Routing protocols supported in IPv6 include static, RIPng, OSPFv3, IS-IS for IPv6, MP-BGP4, and EIGRP for IPv6. RIPng uses UDP port 521 for its connections. Its destination multicast address is FF02::9. The most common transition methods to move from IPv4 to IPv6 include dual stacking and 6to4 tunneling. In dual stacking, a device runs both protocol stacks. In 6to4 tunneling, IPv6 packets are encapsulated in an IPv4 packet to move across an IPv4 backbone to another IPv6 network.

You must first execute the ipv6 unicast-routing command to enable IPv6. An address must be assigned to each interface, typically using the EUI-64 method, for it to process IPv6 packets. RIPng must be configured globally and then enabled on a per-interface basis. A tag is used to specify the RIPng routing process to which an interface belongs. Like RIPng, OSPFv3 and EIGRP must be configured globally and then enabled on a per-interface basis. In the OSPFv3 Global Configuration, you define the router ID and the area numbers; you then associate the interfaces to the areas for an OSPF process. For EIGRP for IPv6, the Global Configuration requires you to configure an autonomous system number and, possibly, bring up the routing process with the no shutdown command.

## Quick Review

### Necessity of IPv6

- IPv6 addresses are 128 bits in length.
- IPsec is built into the IPv6 protocol and allows for device roaming without losing connectivity.

### IPv6 Addressing

- IPv6 addresses use eight sets of four hexadecimal addresses (16 bits in each set), separated by a colon (:), like this: xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx.
- If you have successive fields of zeroes in an IPv6 address, you can represent them using two colons (::), but this can be used only once in an address.
- An anycast address represents the nearest interface to a device, where many devices can share an anycast address. Multicast addresses begin with FF.
- Global unicast addresses are public addresses, similar in purpose to IPv4 public addresses. Global unicast addresses begin with 2 or 3, for example 2000 or 2001.
- Site-local/unique local addresses are similar in purpose to IPv4 private addresses and always start with FEC:: through FFF::. It is common to see reference to FEC0 for site-local/unique local addresses.
- Link-local addresses are similar in purpose to APIPA addresses in IPv4 and start with FE80.
- The subnet ID is the first 64 bits and the interface ID is the last 64 bits. EUI-64 enables dynamic creation of the interface ID portion by using the MAC address on the interface.

### IPv6 Enhancements

- The IPv6 header has been updated and simplified because some of the fields in the IPv4 header have no relevance today.
- ICMPv6 has been enhanced. ICMPv6 takes over the responsibilities of the IPv4 ARP and IGMP protocols, and it also performs neighbor, router, and DNS server discovery services.

### Address Assignment

- DHCPv6 and stateless autoconfiguration enable interfaces to acquire their addressing dynamically.
- Stateless autoconfiguration enables an interface to learn the subnet ID from a router and dynamically create the interface ID using the EUI-64 method.

### Routing and IPv6

- Supported routing protocols for IPv6 include static, RIPng, OSPFv3, IS-IS for IPv6, MP-BGP4, and EIGRP for IPv6.
- RIPng is based on RIPv2. The all RIP routers multicast address is FF02::9 and is sent via UDP to port 521.
- In dual stack, a device runs both the IPv4 and IPv6 protocol stacks. And 6to4 tunneling tunnels IPv6 packets in an IPv4 payload to connect two IPv6 networks via an intermediate IPv4 network.

### IPv6 Configuration

- The **ipv6 unicast-routing** command globally enables IPv6 and must be the first IPv6 command executed on the router. The **ipv6 address** command assigns the prefix, the length, and the use of EUI-64 to assign the interface ID.
- The **ipv6 router rip tag** command enables an RIPng routing process on the router. The **ipv6 rip tag enable** command associates an interface to a particular RIPng routing process. The **show ipv6 rip** command displays the configuration of the RIPng routing process. The **show ipv6 route rip** command displays the RIPng routes in the IPv6 routing table.
