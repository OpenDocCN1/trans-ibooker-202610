In this example, all interfaces are placed in area 0.

As you can see, OSPF is very flexible in enabling you to specify which interface or interfaces will participate in OSPF and to which area they will belong.

![](bbox=[0.143, 0.219, 0.215, 0.278])

VIDEO 12.01. The digital resources that accompany this book contain a multimedia demonstration of configuring OSPF on your routers.

# Loopback Interfaces

A loopback interface is a logical, virtual interface on a router. By default, the router doesn’t have any loopback interfaces, but they can be easily created. All IOS platforms support loopback interfaces, and you can create as many of these interfaces as you need. These interfaces are treated as physical interfaces on a router: you can assign addressing information to them, include their network numbers in routing updates, and even terminate IP connections on them, such as telnet and SSH.

![](bbox=[0.143, 0.603, 0.242, 0.663])

EXAM TIP For the Cisco CCNA exam, remember the value of a loopback interface. A loopback interface is a logical interface that always remains up. Use the interface loopback command to create it.

Here are some reasons you may want to create a loopback interface:

+   - To assign a router ID to an OSPF router
+   - To use for testing purposes, since this interface is always up
+   - To terminate special connections, such as GRE tunnels or IPsec connections, since this interface is always up