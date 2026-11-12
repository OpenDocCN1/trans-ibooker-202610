when the primary fails and the standby routing device starts handling traffic for the broadcast domain, the end stations still think they’re talking to the same routing device.

# Types of Routing Devices

Every HSRP group contains the following types of routing devices that perform certain roles:

+   - Virtual routing device
+   - Active routing device
+   - Standby routing device
+   - Other HSRP routing devices

The role of the virtual routing device is to provide a single RP that’s always available to the end stations. It is not a real RP because the IP and MAC addresses of the virtual RP are not physically assigned to any one interface on any of the routing devices in the broadcast domain.

The role of the active and standby routing devices is based on the priority of the routing devices in the HSRP group. The routing device with the *highest* priority is elected as the active routing device, and the one with the second highest priority is elected as the standby routing device. If the priorities are the same, the IP address of the routing device is used as a tiebreaker. In this situation, the routing device with the *higher* IP address is elected for the role.

The active routing device is responsible for forwarding all traffic destined to the virtual routing device’s MAC address, and the standby routing device keeps tabs on the active routing device by looking for HSRP multicast messages, called *HSRP hellos*. The active routing device generates a hello every 3 seconds. If the standby routing device does not see any hellos for 10 seconds from the active routing device, the standby device promotes itself and begins performing the functions of the active device. Like the active device, the standby device also announces itself every 3 seconds so that if it fails, one of the other HSRP routers in the standby group can assume the standby routing device role.

The other routing devices in the HSRP group, if any exist, listen for the hello multicasts from the standby and active routing devices to ensure that they are performing their respective roles. When the active routing device