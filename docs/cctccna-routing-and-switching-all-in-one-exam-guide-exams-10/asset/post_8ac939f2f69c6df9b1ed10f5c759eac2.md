
Having only one octet as the host ID means that a class C address can support only 254 hosts (256 − 2) on the network.

A class C address has a value for the first octet that ranges between 192 and 223. For example, an IP address of 202.45.8.6 is a class C address, because 202 falls between 192 and 223. You also know that this system has a subnet mask of 255.255.255.0 because it is a class C address.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_179_0.png)

**EXAM TIP** You’ll need to know how to identify class C addresses for your exam. Class C addresses have an IP address in which the value of the first octet is between 192 and 223. In addition, remember that class C addresses have a default subnet mask of 255.255.255.0.

## Class D Addresses

Class D addresses are used for special types of applications on the network —*multicasting applications*. Multicasting applications send data to a number of systems at the same time by sending data to the multicast address, and any system that has registered with that address will receive the data. Because class D addresses are used for multicast addresses, you will not be assigning them specifically to hosts on the network for normal network communication.

Class D addresses have a value on the first octet that ranges from 224 to 239. With that many ranges, class D has the potential for 268,435,456 unique multicast groups that users can subscribe to from a multicast application.

## Class E Addresses

The funny thing about class E addresses is that they were designed for experimental purposes only, so you will never see a class E address on a network. Class E addresses have a first octet with a value between 240 and 247.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_180_0.png)

**EXAM TIP** It’s guaranteed that you will see questions on identifying class A, B, and C addresses on both exams. Be sure to know the characteristics of each class such as the range of addresses in that class, the number of host supported, and the default subnet mask.

## Exercise 4-4: Identifying Address Classes

In this exercise, you will practice identifying address classes for different IP addresses and their associated subnet masks. Fill in the following table:

| IP Address | Subnet Mask | Address Class |
|---|---|---|
| 27.56.89.234 |  |  |
| 196.79.123.56 |  |  |
| 130.49.34.23 |  |  |
| 109.189.109.200 |  |  |
| 189.90.23.100 |  |  |
| 126.34.100.12 |  |  |
| 14.198.120.100 |  |  |

## Rules for Defining Classes

I am often asked how the designers of IPv4 came up with the address ranges for each class. The designers determined the classes by having the boundaries of each class defined with the following rules:

- *Class A addresses always begin with a 0.* This gives binary values of **00000001** to **01111111**, or 1 to 127 when converted to decimal. Note that you cannot have an address of 00000000, so this range starts at 1.
- *Class B addresses always begin with 10.* This gives binary values of **10**000000 to **10**111111, or 128 to 191 when converted to decimal.
- *Class C addresses always begin with 110.* This gives binary values of **110**00000 to **110**11111, or 192 to 223 when converted to decimal.
- *Class D addresses always begin with 1110.* This gives binary values of **1110**000 to **1110**1111, or 224 to 239 when converted to decimal.
- *Class E addresses always begin with 11110.* This gives binary values of **11110**000 to **11110**111, which is 240 to 247 when converted to decimal.

## Loopback Address

You have learned that you are not allowed to have a host assigned an IP address that has a value of 127 in the first octet. This is because the class A address range of 127 has been reserved for the loopback address.

The loopback address is used to refer to the local system, also known as the *localhost*. If you want to verify that the TCP/IP software has initialized on the local system even though you may not have an IP address, you can ping the loopback address, which is typically referred to as 127.0.0.1.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_181_0.png)

**NOTE** You can test your own local system from a command line by typing **ping 127.0.0.1**, **ping localhost**, or **ping loopback** to verify that the TCP/IP protocol stack is functioning on your system.

## Private Addresses

Another type of address you need to be aware of is a *private address*. A private address can be assigned to a system but cannot be used for any kind of Internet connectivity. Private addresses are *nonroutable* on the Internet, so any system using them will not be able to route data across the Internet. The following three address ranges are private:

- 10.0.0.0–10.255.255.255
- 172.16.0.0–172.31.255.255
- 192.168.0.0–192.168.255.255

Due to the growth of the Internet, it became impossible for every device that needed Internet access to use a public IP address as we were running out of addresses. So private addresses are used inside networks because they are not seen by other systems on the Internet; we use a Network Address Translation (NAT) device to translate the private address to a public address.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_182_0.png)

**EXAM TIP** Remember for the CCT/CCNA exams that one of the major design considerations with networks today is to use private addresses internally and use a NAT device to translate the private address, which is nonroutable on the Internet, to a public address that is routable on the Internet. This essentially enables a large number of computers running private addresses to share the public address of the NAT device to gain Internet access.

## APIPA

Systems today support the *Automatic Private IP Addressing* (APIPA) feature. When a client boots up and cannot contact a DHCP server, the
client will configure itself automatically with a 169.254.x.y address. If there is something wrong with the DHCP server and none of the systems on the network can obtain an address, all of the clients will assign themselves an address within the 169.254 address range and will then be able to communicate with one another (because they are all on the same network).

APIPA does not assign a default gateway, so you will be unable to access resources on a remote network and the Internet—but you can still communicate with systems on your network. When troubleshooting to determine why a machine cannot communicate on the network, watch for systems that have the 169.254.x.y address range, because this indicates that they could not find a DHCP server.

## Invalid IP Addresses for Hosts

A few IP addresses cannot be assigned to hosts on the network. You may wonder, for example, why a class C address can have only 254 hosts and not 256, as would seem more likely, since an 8-bit number can have 256 possible values. The reason for this seeming discrepancy is that two of the addresses are lost from the available host pool. The first is an address that has all bits set to 0s in the host ID, which signifies the network ID of the network. The second is an address that has all bits set to 1s in the host ID, which signifies the broadcast address for the network. So, for example, in the class C network 200.158.157.x, the addresses 200.158.157.0 (the network ID) and 200.158.157.255 (the broadcast address) are not available to assign to hosts on the network, reducing the available number of hosts from 256 to 254.

Another illegal address is any system that has the first octet of 127; remember that this is reserved as the loopback address, and you will not be able to assign the address to any host. The following summarizes illegal addresses:

- **Any address starting with 127** An IP address that starts with 127 is reserved for the loopback address and cannot be assigned to a system. An example of this illegal address is 127.50.10.23.
- **All host bits set to 0** You are not allowed to assign a system an IP address that has all of the bits in the host ID portion set to 0 because this is the network ID. An example of this illegal address is 131.107.0.0.
- **All host bits set to 1** You are not allowed to assign a system an IP address that has all the host bits set to 1 because this corresponds to the broadcast address of the network. An example of this type of illegal address is 131.107.255.255.
- **A duplicate address** You are not allowed to assign a system an address that another system is using because this results in a duplicate IP address error.

Not being able to route data across the Internet when using these addresses will not pose a problem, because, realistically, these private addresses will be sitting behind a NAT server that will translate the private address to a public address that can be routed on the Internet. Chapter 13 offers more information on NAT.

## Exercise 4-5: Understanding Valid Addresses

In this exercise, you will practice identifying valid addresses by recording whether each of the following addresses is valid. A valid address can be assigned to a system on the network. If an address is invalid, specify why.
