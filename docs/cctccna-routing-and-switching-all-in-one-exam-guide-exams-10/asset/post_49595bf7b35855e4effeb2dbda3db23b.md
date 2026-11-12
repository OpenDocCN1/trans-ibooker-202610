
| Network ID | First Valid Address | Last Valid Address | Broadcast Address |
|---|---|---|---|
| Subnet 1 |  |  |  |
| Subnet 2 |  |  |  |
| Subnet 3 |  |  |  |
| Subnet 4 |  |  |  |
| Subnet 5 |  |  |  |
| Subnet 6 |  |  |  |
| Subnet 7 |  |  |  |
| Subnet 8 |  |  |  |

New Subnet Mask: ________________

## Task 3: Subnetting a Class C Address

The network ID of the class B address is 222.100.10.0. You need to subnet this network into four subnets using the shortcut method.

| Network ID | First Valid Address | Last Valid Address | Broadcast Address |
|---|---|---|---|
| Subnet 1 |  |  |  |
| Subnet 2 |  |  |  |
| Subnet 3 |  |  |  |
| Subnet 4 |  |  |  |

New Subnet Mask: ________________

## Classful vs. Classless Addressing

Chapter 4 introduced the different class IP addresses. Each IP address that you use on a system falls into one of the three primary classes: class A, class B, or class C. This is known as classful addressing. Table 5-1 summarizes the three major address classes. Note that in the Octet Setup

column, N means network ID and H means host ID to identify the network ID and host ID portions of the address.

| Address Class | Value of First Octet | Octet Setup | Number of Hosts |
|---|---|---|---|
| Class A | 1-127 | N.H.H.H | 16,777,214 |
| Class B | 128-191 | N.N.H.H | 65,534 |
| Class C | 192-223 | N.N.N.H | 254 |

Table 5-1 Reviewing Classful IP Addressing

Everything that you have learned about the different address classes, including network IDs, host IDs, and default subnet masks, is based on classful addressing.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_230_0.png)

**EXAM TIP** For the exams, you need to remember that classful IP addressing divides the network ID and host ID portions of an IP address at an octet. For example, a class B address uses the first two octets as the network ID and the last two octets as the host ID.

## IP Subnet Zero

Another subnetting concept you should be familiar with for the Cisco CCT and CCNA exams is IP subnet zero. A number of years ago, some networking devices and protocols could not use the first and last subnets we created when we divided the network into multiple subnets. This was because some of the older routing protocols did not send the subnet mask information along with the network ID when the router shared information about the network. This meant that if the network was subnetted, let’s say into four subnets, when a router shared knowledge of the 192.168.1.0 network, the receiving router had no way to know if it was subnetted or the entire 192.168.1.0 network. Today’s routing protocols send the subnet mask

information with the route—something like 192.168.1.0/26—so there is no question that this is the first of four subnetted networks.

Cisco devices can use the first and last subnets when subnetting a network, because the **ip subnet-zero** command configured on the device indicates that we can use the first and last subnets. If the command was changed to **no ip subnet-zero**, we would not be allowed to use the first and last subnets on the router.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_231_0.png)

**NOTE** You will learn about some Cisco IOS commands in Chapter 7.

## Variable Length Subnet Masks

One of the problems with the subnetting technique we’ve covered so far is that each of the subnets we’ve created has the same number of IP addresses assigned to it. This sometimes can waste addresses, because there may not be a need for the same number of IP addresses within each subnet. As an example, in Figure 5-3, you can see a network with three subnets. Subnet 1 has 21 hosts, subnet 2 has 2 hosts, and subnet 3 has 56 hosts. Each host on each subnet will need an IP address within the subnet, and we want to subnet the network into three subnets while conserving addresses.

Network ID: 216.83.11.0

![](img/0eafdcebe219ef3756f0e57bcf1eb997_232_0.png)

## Figure 5-3 A VLSM scenario

To accomplish this goal, look at your network ID (216.83.11.0), and first work with the larger subnet—in this case, subnet 3. Subnet 3 requires 56 hosts, so to calculate the subnet information, you need to determine how many hosts bits you need to create a subnet that supports 56 hosts. You can use the following formula:

2^{host bits} − 2 = number of hosts supported

Based on that formula we need 6 bits for host bits in order to support the 56 hosts on subnet 3 as follows:

2^6 = 64 − 2 = 62 hosts supported on subnet 3

Keep in mind that using 6 bits for host bits gives you a few more host addresses than what you require, but you cannot go any lower, because five host bits would support only 30 (32 − 2) hosts. You subtract 2 because each subnet has two reserved addresses—the network ID and the broadcast address. Also note that although you are supporting 62 hosts with this

subnet, the number of addresses is 64—this is known as the block size (or subnet size).

Keep in mind that if there are 6 bits for host bits (six 0s below in the fourth octet), that means the two remaining bits are subnet bits (highlighted in bold next). With this information, you can now document the subnet mask for subnet 3 as follows:

```
11111111.11111111.11111111.11000000 = 255.255.255.192
```

Now to show what address range this is, you can plot the network on a grid showing the different IP addresses used in the fourth octet. Note that subnet 3 will be using a block of 64 addresses, but you can use only 62 of them (2 are reserved). This means numbers 0–63 are the addresses for subnet 3, as shown in Figure 5-4. Notice that this is documented as 216.83.11.0/26. The /26 indicates that 26 bits in the subnet mask have been enabled, and it controls how many addresses are used in this subnet (64).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_233_0.png)

## Figure 5-4 Plotting addresses for the largest VLSM subnet

Now look at the next highest number of hosts on a network, and you’ll see that subnet 1 requires 21 hosts (or 21 IP addresses). To calculate the subnet block for this, use the following formula:

```
2^5 = 32 − 2 = 30 hosts supported on subnet 1
```

The 30 hosts are a few more than the required 21, but this is okay, as described in previous examples. With this information, you can now document the subnet mask for subnet 1 as follows:

```
11111111.11111111.11111111.11100000 = 255.255.255.224
```

Now plot subnet 1 on the grid showing the different IPs in the fourth octet, as shown in Figure 5-5. Remember that subnet 1 will be using a block of 32 addresses, but two are unavailable. Because subnet 3 is using addresses 0–63, subnet 1 will start at 64 and go for 32 addresses, which will be to 95 (64 – 95 = 32 address block size).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_234_0.png)

## Figure 5-5 Plotting addresses for next VLSM subnet

This means numbers 64–95 are the addresses for subnet 1, as shown in Figure 5-5. Note that this is documented as 216.83.11.64/27. The /27 indicates that 27 bits in the subnet mask have been enabled, and it controls how many addresses are used in this subnet (32).

The last subnet to add to our planning grid is subnet 2. Because we need only two IP addresses on this subnet (one for each side of the WAN link), we need to use the following formula for two hosts:

```
2² = 4 − 2 = 2 hosts supported on subnet 2
```

This means we need to have only two host bits to support a subnet requiring only two IP addresses. This gives us the following IP configuration in binary:

```
11111111.11111111.11111111.11111100 = 255.255.255.252
```

Note that the block size is four (even though there are only two usable addresses). This means we will use the next addresses of 96–99 as the addresses in the fourth octet for this subnet (see Figure 5-6).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_235_0.png)

Figure 5-6 Plotting VLSM addresses for WAN link

Note that this is documented as 216.83.11.96/30. The /30 indicates that 30 bits in the subnet mask have been enabled, and it controls how many addresses are used in this subnet, which in this case is four (the block size).

The great news is that we have planned our IP address scheme for three subnets, and look at all of the addresses that are still available for future network changes! Addresses 100–255 are still available! Now that is conserving IP addresses and still getting the job done!

![](img/0eafdcebe219ef3756f0e57bcf1eb997_235_1.png)

TIP I have specified the blocks as contiguous addresses for visual purposes. In the real world, you may want to spread them out a bit so that you can accommodate growth without needing to change all the address blocks.

## Exercise 5-3: Using VLSM on a Network

In this exercise, you will calculate the VLSM for each different network segment based on the network diagram shown in Figure 5-7. The network address you have to subnet is 200.45.6.0.

Record your subnet information for each network segment:

Subnet #1: ________________
