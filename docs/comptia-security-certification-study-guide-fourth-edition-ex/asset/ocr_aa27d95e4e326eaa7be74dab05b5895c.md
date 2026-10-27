# Class C Addresses

Class C addresses have a subnet mask of 255.255.255.0, which means that the first three octets are the network ID and the last octet is the host ID. Having only one octet as the host ID means that a class C address can support only 254 hosts (256 − 2) on the network.

You can identify a class C address because it has a value for the first octet that ranges between 192 and 223. For example, an IP address of 202.45.8.6 is a class C address because 202 falls between 192 and 223. We also know that this system has a subnet mask of 255.255.255.0 because it is a class C address.

# Class D Addresses

Class D addresses are used for special types of applications on the network known as multicasting applications. These applications send data to a number of systems at the same time by sending data to the multicast address, and anyone who has registered with that address will receive the data. A multicast address is what class D addresses are used for, so you will not be assigning them specifically to hosts on the network for normal network communication.

Class D addresses have a value in the first octet that ranges from 224 to 239. With that many ranges, class D has the potential for 268,435,456 unique multicast groups that users can subscribe to from a multicast application.

# Class E Addresses

The funny thing about class E addresses is that they were designed only for experimental purposes, so you will never see a class E address on a network. Class E addresses have a first octet with a value that falls in the range of 240 to 247.

# INSIDE THE EXAM

# Remember Address Classes?

Although the Network+ certification exam tests you on IP addressing and configuration concepts, you still need to know the concept of address classes to answer related questions on the Security+ certification exam. The following paragraphs review the key information about address classes.

Class A addresses have an IP address in which the first octet is between 1 and 126. Class A addresses also have a default subnet mask of 255.0.0.0. Also note that this subnet mask can be displayed as /8 at the end of the address—for example, 12.0.0.10/8 means that the first 8 bits make up the subnet mask.

Class B addresses have an IP address in which the value of the first octet is between 128 and 191. Class B addresses have a default subnet mask of 255.255.0.0 or can be displayed as /16 at the end of the address.

Class C addresses have an IP address in which the value of the first octet is between 192 and 223. In addition, class C addresses have a default subnet mask of 255.255.255.0, which can be displayed as /24 at the end of the address.