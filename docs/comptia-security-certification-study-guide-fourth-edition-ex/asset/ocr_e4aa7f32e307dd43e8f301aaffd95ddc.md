|  | Octet 1 | Octet 2 | Octet 3 | Octet 4 |
|---|---|---|---|---|
| IP address #1 | 192 | 168 | 1 | 15 |
| Subnet mask | 255 | 255 | 255 | 0 |
| IP address #2 | 192 | 198 | 45 | 10 |

Identifying Two Systems on Different Networks Using the Subnet Masks

How does your system know which router to use? The answer is “the default gateway.” The default gateway is the IP address of the router that can send data from your network.

To communicate on the Internet, your system will need to be configured with an IP address, a subnet mask, and a default gateway. If you need to communicate only with other systems on your network, you will need only an IP address and a subnet mask.

# Address Classes

Every IP address belongs to a distinct address class. The Internet community defined these classes to accommodate networks of various sizes. The class to which the IP address belongs initially determines the network ID and host ID portions of the address, along with the number of hosts that are supported on that network. The different class addresses are named class A, class B, class C, class D, and class E. This section details each class of addresses.

# Class A Addresses

A class A address has a default subnet mask of 255.0.0.0, which means that the first octet is the network ID and the last three octets belong to the host ID portion of the address. Each octet can contain 256 possible values (0–255), so a class A address supports 16,777,216 hosts on the network (256 × 256 × 256). Actually, there are only 16,777,214 valid addresses to use on systems because two addresses are reserved on each IP network: the addresses with all host bits set to 0’s (the network ID) and with all host bits set to 1’s (the broadcast address). So with a class A address, you will not be able to assign n.0.0.0 or n.255.255.255 (where n is your network ID) to any hosts on the network.

You can always identify a class A address because the value of the first octet falls between 1 and 126. An address that starts with 127 is a class A address as well, but you are not allowed to use any address that starts with 127 because it is reserved for the loopback address (more on the loopback address later in this chapter). For example, the IP address of 12.56.87.34 is a class A address because the first octet is 12, which falls within the range of 1 to 126.

# Class B Addresses

Class B addresses have a default subnet mask of 255.255.0.0, which means that the first two octets are the network ID and the last two octets are the host ID portion of the address. This means that we can have 65,536 hosts (256 × 256) on the network. Oh, but wait! Don’t forget to take off the two reserved addresses, so that gives us 65,534 addresses that can be assigned to hosts on the network.

Due to the number of hosts that are supported on a class B address, you usually find that a medium-sized company has a class B address. You can identify a class B address because the first octet starts with a number that falls between 128 and 191.