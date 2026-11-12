preemption, a new routing device with a higher priority can usurp an existing active or standby routing device.

If the routing device enters the *speak state*, the device propagates multicast messages so that it can participate in the election process for the standby or active role. These hellos are sent out periodically so that other routing devices in the group know about everyone’s existence. Note that for a routing device to enter this state, it must have the virtual IP address configured on it.

Based on the routing device’s priority, it becomes either a *standby* or *active* routing device. In a standby state, the routing device is the next in line to assume the role of the active routing device if the active routing device fails. In an active state, the routing device is responsible for forwarding all traffic sent to the virtual MAC address of the broadcast domain. There can be only one active and one standby routing device. Both of these routing devices generate periodic hellos to other routing devices in the group to guarantee that end stations always have a default gateway that can forward their traffic if either of them fails.

It’s important to point out that if you don’t configure preemption, the first routing device that comes up takes on the active role and the second routing device takes on the standby role. Therefore, if you’re setting up load balancing between routing devices so that certain routing devices handle traffic for certain VLANs and other routing devices handle traffic for other VLANs, you’ll want to use preemption so that whenever a failed routing device comes back online, it resumes its former role.

# HSRP Configuration

The configuration of HSRP is a simple process. The following sections discuss its configuration as well as how to optimize it for larger networks.

# Basic Configuration

Only one command, **standby**, is necessary to enable HSRP. Execute one of the following **standby** commands on the routing device’s interface. Use a subinterface for a trunk port and a VLAN interface for an internal routing device, such as a layer 3 switch.