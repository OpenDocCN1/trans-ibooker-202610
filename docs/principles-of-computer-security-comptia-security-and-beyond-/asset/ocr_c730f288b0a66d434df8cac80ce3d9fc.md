network via any number of routers and allows the return traffic to make its way back to the device that initiated the traffic on the originating network. Just like highways allow you to go from Rochester to Staten Island, routers allow your network traffic to reach devices on other networks that could be in the same building, another state, or another country or continent.

60 MINUTES

# Lab Exercise 9.01: Switch Configuration

When a PC sends an Ethernet frame into a switch, the switch examines the destination MAC address in the frame and then checks to see if it knows which port a device with that destination MAC address is connected to. If the switch knows where the device with that MAC address is, the switch sends the frame out of just that corresponding port. If the switch doesn’t know where the destination MAC address is, the switch floods the frame out of all ports, except the port on which the frame originated.

# Note

When referring to routers and switches, the terms port and interface are very often used interchangeably. Technically speaking, though, a port is a physical unit that a connector is plugged into, while an interface is a logical representation of that port, with a name like GigabitEthernet0/0/0, for example.

So now the obvious question is, how does the switch learn where MAC addresses are in the first place? The switch starts off knowing nothing, but as frames are sent into the switch, the switch starts learning. Refer to Figure 9-1.