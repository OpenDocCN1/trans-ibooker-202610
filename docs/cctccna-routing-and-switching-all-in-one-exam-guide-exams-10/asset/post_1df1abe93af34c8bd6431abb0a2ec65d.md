
## ICMP Router Discovery Protocol Issues

The Internet Control Message Protocol Router Discovery Protocol (IRDP) is not a routing protocol like OSPF or RIP; instead, it is an extension to ICMP that enables an end station to automatically discover the default gateways connected to the same VLAN. IRDP is covered in RFC 1256. In this environment, the routing devices periodically generate special multicast packets that announce the router’s existence to the clients. This time period is usually between 5 and 10 minutes. Learned information usually has a maximum lifetime of 30 minutes on the client if no more IRDP messages are received from the advertising routing device. The multicast packet includes the routing device’s address and a lifetime value.

With IRDP, end stations can dynamically discover other routing devices when their primary default gateway fails. However, this may take up to 30 minutes, based on the lifetime value in the original multicast packet from the routing device. And even if you may consider using IRDP with your access layer devices, most end-station IP stacks do not support IRDP.

## Routing Protocol Issues

To overcome these two previous problems, you may be able to run a routing protocol on the end station—if the client supports this type of function. With IP, the only routing protocol that most end stations might support is RIP or OSPF. In RIP or OSPF, the end station could make intelligent decisions about which layer 3 routing device to use to access other subnets. However, the issue with RIP is that its convergence is very slow—it could take up to 180 seconds before an alternative routing device is chosen when the current primary routing device fails. With TCP sessions, this would cause a timeout. Because of this, as well as all the additional overhead that RIP creates, this solution is not very desirable for your end stations—and this assumes that your end stations and other network devices support a routing protocol such as RIP.

## User Device Issues

In most campus environments, end stations are assigned a single IP address for the default gateway (which is usually done via DHCP). In this environment, if the routing device (default gateway) were to fail, the end station would lose its capability to access other networking devices outside of its VLAN. Unfortunately, there is no redundancy in this implementation, because an end station can have only one default gateway address configured (whether it is assigned via DHCP or statically configured).

## Hot Standby Router Protocol

HSRP is a Cisco-proprietary protocol that provides a single definition of a default gateway on the end station and provides layer 3 redundancy for overcoming the issues of IRDP, Proxy ARP, and end-station routing protocols. Unlike the four previous solutions, HSRP is completely transparent to the end stations—you do not have to perform any additional configuration on the end stations themselves. HSRP enables Cisco routing devices to monitor one another’s status, which provides a very quick failover when a primary default gateway fails. This is done by establishing HSRP groups.

With HSRP, a group of routing devices represents a single virtual default gateway. This virtual default gateway has a virtual IP address and a virtual MAC address. If the primary routing device fails, another routing device in the HSRP group takes over and processes the frames sent by the end stations to the virtual MAC address.

An advantage of HSRP groups is that different subnets (VLANs) can have different default gateways, thus providing load balancing. Also, within each HSRP group is a primary default gateway and the capability to use multiple routers to perform a backup function. You can have up to 256 standby groups per routing device, providing up to 255 default gateways. Routing devices can provide backup for multiple primary default gateways. Each standby group keeps track of the primary routing device that’s currently forwarding traffic sent to the virtual MAC address. Note that only one routing device is actually forwarding traffic with HSRP.

Another nice feature of HSRP is that you can customize it based on the size of your network. For instance, if you have a VLAN with 1000 devices in it, you can set up two HSRP groups: one group for 500 devices and another group for the other 500 devices. You can then assign routing devices to each group. For example, if you had only two routing devices, you could have the first routing device be the active routing device for group 1 but the standby for group 2, and vice versa for the second routing device. Through this process, you can have both of your routing devices forwarding traffic while still providing redundancy—if the active routing device in either group fails, the other routing device promotes itself to an active state.

## HSRP Operation

As mentioned in the previous section, only one routing device actually forwards traffic for an HSRP group. Using a priority scheme, one routing device is elected as the forwarding router and the others perform as backups for a group. Each routing device has a default priority of 100, which you can manipulate. The routing device with the highest priority in the group is elected as the active router, and the other routing devices are placed in standby mode. The active routing device responds to any ARP packets from end stations and replies with the virtual MAC address of the group.

Each HSRP group must have a unique virtual IP address and a virtual MAC address, which means these addresses must be unique across different groups. This MAC address is 0000.0c07.acXX. The 0000.0c is Cisco’s vendor code. The 07-ac is HSRP’s well-known address. The XX is the group number (in hexadecimal) for the HSRP group. Therefore, each HSRP group must have a unique address to ensure that the MAC address is unique in a VLAN.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_668_0.png)

**EXAM TIP** For the exam, you’ll be expected to know that the HSRP virtual MAC address begins with 0000.0c07.acXX, where the last two digits represent the HSRP group number, in hexadecimal. The active/master routing device for the VLAN is responsible for processing traffic sent to the virtual MAC and virtual IP addresses in the subnet/VLAN.

With HSRP, the end stations would perform an ARP with the virtual IP address, requesting the virtual MAC address of the default gateway routing device. Note that in this setting, the end stations are completely unaware of the actual routing devices handling traffic destined for a virtual router. Even when the primary fails and the standby routing device starts handling traffic for the broadcast domain, the end stations still think they’re talking to the same routing device.

## Types of Routing Devices

Every HSRP group contains the following types of routing devices that perform certain roles:

- Virtual routing device
- Active routing device
- Standby routing device
- Other HSRP routing devices

The role of the virtual routing device is to provide a single router that’s always available to the end stations. It is not a real router because the IP and MAC addresses of the virtual router are not physically assigned to any one interface on any of the routing devices in the broadcast domain.

The role of the active and standby routing devices is based on the priority of the routing devices in the HSRP group. The routing device with the *highest* priority is elected as the active routing device, and the one with the second highest priority is elected as the standby routing device. If the priorities are the same, the IP address of the routing device is used as a tiebreaker. In this situation, the routing device with the *higher* IP address is elected for the role.

The active routing device is responsible for forwarding all traffic destined to the virtual routing device’s MAC address, and the standby routing device keeps tabs on the active routing device by looking for HSRP multicast messages, called *HSRP hellos*. The active routing device generates a hello every 3 seconds. If the standby routing device does not see any hellos for 10 seconds from the active routing device, the standby device promotes itself and begins performing the functions of the active device. Like the active device, the standby device also announces itself every 3 seconds so that if it fails, one of the other HSRP routers in the standby group can assume the standby routing device role.

The other routing devices in the HSRP group, if any exist, listen for the hello multicasts from the standby and active routing devices to ensure that they are performing their respective roles. When the active routing device
