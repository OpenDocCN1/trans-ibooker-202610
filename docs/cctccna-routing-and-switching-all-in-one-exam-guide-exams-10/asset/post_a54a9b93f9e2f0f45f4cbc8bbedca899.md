
## Subnetting and VLSM

In this chapter, you will

- Learn about subnetting networks
- Learn some subnetting shortcuts
- Understand the difference between classful and classless addressing
- Learn about Variable Length Subnet Masks (VLSMs)

In Chapter 4 you learned the basics of TCP/IP addressing. In this chapter, you’ll learn how to divide one network address range into multiple network ranges, or *subnets*, so that you can assign each address range to a separate network (subnet) in your internetwork (network made up of multiple networks).

You may need to use multiple networks as a result of your business operating in several different physical locations, for example. Or, if you’re working in one location, you may want to cut down on broadcast traffic being sent across the entire network by segmenting your network into multiple segments, placing a router in between each network segment.

Each segment will need its own network address range so that the router can route (send) the data from one network to another. If, for example, you had two physical networks but kept the single IP range without subnetting, the router would “logically” think that all systems were on the same network and would never try to route data from one side to the other.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_207_0.png)

**CAUTION** If you do not subnet a network range that is divided into multiple physical network segments, the Internet Protocol (IP) will logically decide that all systems are on the same network, and it will not try to route the data across the router separating the network segments. As a result, you will be able to communicate with hosts on your segment, but not with the other segments.

### Understanding Subnetting

You subnet a network ID to break the single logical network address into multiple logical network addresses so that you can assign a logical network ID and related addresses to each physical network segment on your network. In order to do this, you have to manipulate the subnet mask at the bit level and essentially take host bits away and convert them to subnet bits.

In this section, you will walk through how to subnet class A, class B, and class C network addresses. If you are new to subnetting, the best way to learn it is to perform each calculation on a blank piece of paper as we walk through it.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_208_0.png)

**NOTE** Subnetting is one of those topics that most IT professionals tend to avoid because of how tedious it is to sit down and calculate the required information to configure newly created subnets.

### Subnetting Class A Networks

In this section, you will walk through subnetting a class A network address. In this example, you have an address range of 10.0.0.0 and you will divide the network into four subnetworks, or *subnets*. The physical network structure is shown in Figure 5-1. Subnetting involves taking some of the host bits from the subnet mask and using them as additional network bits, which will give you more networks. With subnetting, you are taking additional host bits and “masking” them (flagging them as network bits) by setting the bits to a 1 state so that they are no longer representing the host ID portion of the subnet mask. This creates additional networks, but it results in fewer hosts on each of the networks.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_209_0.png)

Figure 5-1 Subnetting to match this physical network structure

To begin, you need to know how many host bits are required to make the desired number of networks. To calculate how many bits are needed to subnet into four networks (subnets), you use the formula:

2^masked bits = number of subnets

For example, if you are looking for four subnets, the formula would be 2^2 = 4 networks, or subnets. So you would steal, or mask, two of the host bits in the subnet mask and make them network bits—these are called subnet bits.

Now that you know that two bits must be masked to create the four subnets, the next step is to look at your IP range of 10.0.0.0 and ask yourself, “What is the default subnet mask of this address?” You should come up with 255.0.0.0 because it is a class A address. Once you know the default subnet mask, your next step is to break the subnet mask down into binary. A default class A decimal subnet mask in binary looks like the following:

| Decimal | 255 | 0 | 0 | 0 |
| Binary | 11111111 | 00000000 | 00000000 | 00000000 |

Once you’ve written out the subnet mask in binary in your work area, you can begin to calculate the new addresses of the four subnets. The first thing you need to do is steal, or “mask,” two bits from the host ID portion of the subnet mask and make them subnet bits by setting them to a 1 state. Remember that you’re taking two bits because of your earlier calculation. Because the last three octets of this subnet mask represent the host bits of a class A address, work from left to right and mask two additional bits to get the new binary table that follows:

| Decimal | 255 | 0 | 0 | 0 |
| Binary | 11111111 | 11000000 | 00000000 | 00000000 |

Notice that the two bits that are now enabled are in the second octet. Also notice that you always work from left to right in the subnet mask.

### Calculate the New Subnet Mask

Now you can calculate the new subnet masks for all four networks. To do this, you convert the new binary value in the preceding table of 11111111.11000000.00000000.00000000 to its decimal notation—you should get 255.192.0.0 as the new subnet mask of your four new subnets. Write that number down, because you’ll need it later.

The next step is to calculate the IP ranges for the four different networks, but before that, you need to know five pieces of information about each of the new subnets you are designing before this walkthrough is complete:

- **New subnet mask** After subnetting a network, you will have a new subnet mask that is used by all subnets you have created.
- **Network ID** To calculate the network ID for each network, all host bits are set to 0.
- **First valid address** To calculate the first valid address, you set the least significant host bit to 1; all other host bits are 0.
- **Broadcast address** To calculate the broadcast address for each subnet, you set all host bits to 1.
- **Last valid address** To calculate the last valid address, you set the least significant host bit to 0; all other host bits are 1.

You know that the new subnet mask is 255.192.0.0, so you can start by calculating the network ID of each of the four subnets.

### Calculate the Network ID

To calculate the network IDs for each of the subnets, you need to determine all of the on/off states of the number of bits that you have converted from host bits to subnet bits. For example, two bits were used as subnet bits to create more networks, so there are four possible on/off states with two bits: 00, 01, 10, and 11. These are listed out in your binary work area and should look like the following table:

|  | First Octet (Decimal) | Second Octet (Binary) | Third Octet (Binary) | Fourth Octet (Binary) |
|---|---|---|---|---|
| Original IP | 10 | 0 | 0 | 0 |
|  | 10 | 00000000 | 00000000 | 00000000 |
|  | 10 | 01000000 | 00000000 | 00000000 |
|  | 10 | 10000000 | 00000000 | 00000000 |
|  | 10 | 11000000 | 00000000 | 00000000 |

Next, you add in the remaining 0s to the bits that represent the host ID portion. Remember that the original network ID was 10.0.0.0, so the first octet will start with 10, no matter what you change in the binary, because you are starting your work with the second octet.

The next step is to bring the 10 down to the first octet; each network ID will start with 10 because that is what it was originally. After bringing the 10 down in the first octet, you then calculate the network ID of each of the two networks by leaving all host bits set to 0 (the nonbolded bits), as shown in the following table:
