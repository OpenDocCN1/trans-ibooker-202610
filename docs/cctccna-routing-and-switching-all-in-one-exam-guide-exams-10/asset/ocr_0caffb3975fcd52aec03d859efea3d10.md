forwarding traffic while still providing redundancy—if the active routing device in either group fails, the other routing device promotes itself to an active state.

# HSRP Operation

As mentioned in the previous section, only one routing device actually forwards traffic for an HSRP group. Using a priority scheme, one routing device is elected as the forwarding router and the others perform as backups for a group. Each routing device has a default priority of 100, which you can manipulate. The routing device with the highest priority in the group is elected as the active router, and the other routing devices are placed in standby mode. The active routing device responds to any ARP packets from end stations and replies with the virtual MAC address of the group.

Each HSRP group must have a unique virtual IP address and a virtual MAC address, which means these addresses must be unique across different groups. This MAC address is 0000.0c07.acXX. The 0000.0c is Cisco’s vendor code. The 07-ac is HSRP’s well-known address. The XX is the group number (in hexadecimal) for the HSRP group. Therefore, each HSRP group must have a unique address to ensure that the MAC address is unique in a VLAN.

![](bbox=[0.14, 0.56, 0.24, 0.63])

**EXAM TIP** For the exam, you’ll be expected to know that the HSRP virtual MAC address begins with 0000.0c07.acXX, where the last two digits represent the HSRP group number, in hexadecimal. The active/master routing device for the VLAN is responsible for processing traffic sent to the virtual MAC and virtual IP addresses in the subnet/VLAN.

With HSRP, the end stations would perform an ARP with the virtual IP address, requesting the virtual MAC address of the default gateway routing device. Note that in this setting, the end stations are completely unaware of the actual routing devices handling traffic destined for a virtual router. Even