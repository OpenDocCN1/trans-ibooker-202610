
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

New Subnet Mask: __________

## Task 3: Subnetting a Class C Network

You have a class C network ID of 202.15.67.0, and you need to divide this network into four subnets. Use some paper and calculate the network ID, first valid address, last valid address, and broadcast address of each of the four subnets. Don’t forget to determine the new subnet mask of these networks as well. Fill in the following table when you have completed your work on paper:

| Network ID | First Valid Address | Last Valid Address | Broadcast Address |
|---|---|---|---|
| Subnet 1 |  |  |  |
| Subnet 2 |  |  |  |
| Subnet 3 |  |  |  |
| Subnet 4 |  |  |  |

New Subnet Mask: __________

## Chapter Review

In this chapter, you learned that from time to time you may need to divide a network range into multiple network blocks (subnets) to follow the physical structure of the network. Because the Cisco CCNA certification exam will have a number of questions that rely on your knowledge of subnetting, be sure to study this chapter well.

To subnet a network, you will need to know how many networks or hosts per network are required. You can then convert host bits in the subnet mask to become subnet bits, essentially altering your subnet mask and creating more networks while supporting less hosts per network.

The Variable Length Subnet Mask (VLSM) feature enables you to have a different subnet mask for each subnet on the network; this lets you make better use of addresses.

## Quick Review

### Understanding Subnetting

- The purpose of subnetting is to break one network ID into multiple subnetworks (subnets) so that you can follow the physical structure of the network.
- With subnetting, you take host bits from the subnet mask and turn them into network bits—thus creating more networks but fewer machines per network.
- To determine how many bits to take from the host ID portion of the subnet mask, use the formula 2^{masked bits} − 2 = number of networks.
- For each subnet created, you must calculate the network ID, first valid address, last valid address, broadcast address, and new subnet mask.

### Subnetting Shortcuts

- Look at the value of the last bit that was used to subnet and use that as your increment value.
- Using the increment value, list out all network IDs, and then add one to that to get the first valid address of each network.
- To determine the broadcast address of a subnet, use the number that is one less than the next network ID. The last valid address for a subnet will be one number less than the broadcast address.

### Classful vs. Classless Addressing

- In classful addressing, the network ID falls into one of the default network IDs of a class A, class B, or class C address.
- In classless addressing, the network ID is altered from a normal classful address.

### Variable Length Subnet Masks

- Creating subnets of the same size for each of your subnets is going to waste addresses when designing your network.
- VLSM enables you to use a different subnet mask for each subnet, so that you can make better use of your available addresses.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully, as there may be more than one correct answer. Choose the correct answer(s) for each question.

- 1. You have a network ID of 131.107.0.0 and you would like to subnet your network into six networks. What is your new subnet mask?
  A. 255.224.0.0
  B. 255.255.224.0
  C. 255.192.0.0
  D. 255.255.192.0
- 2. You want to divide your network into eight networks. How many bits will you need to take from the host ID portion of the subnet mask?
  A. 2
  B. 3
  C. 4
  D. 6
- 3. In binary, how do you calculate the broadcast address of a network range?
  - A. All host bits set to 0
  - B. All host bits set to 0, except for the low-order bit
  - C. All host bits set to 1, except for the low-order bit
  - D. All host bits set to 1
- 4. In binary, how do you calculate the network ID of a network range?
  - A. All host bits set to 0
  - B. All host bits set to 0, except for the low-order bit
  - C. All host bits set to 1, except for the low-order bit
  - D. All host bits set to 1
- 5. The last valid address of a subnet is always
  - A. One more than the broadcast address
  - B. One less than the broadcast address
  - C. One more than the network ID
  - D. One less than the network ID
- 6. You need to assign an IP address to the router that uses the fourth valid address of 200.56.88.96/27. Which address would you use?
  - A. 200.56.88.68
  - B. 200.56.88.104
  - C. 200.56.88.97
  - D. 200.56.88.100
- 7. Your system has an IP address of 138.97.25.10/20. Which of the following is a system on your network?
  - A. 138.97.56.8
  - B. 138.97.17.10
  - C. 138.97.15.77
  - D. 138.97.33.76
- 8. Which of the following is an example of CIDR notation?
  - A. 16/10.34.56.78
  - B. 10.34.56.78
  - C. 10.34.56.0
  - D. 10.34.56.78/16
- 9. Your router has an IP address of 216.83.11.65/27. You want to connect a new system on the network. Which of the following addresses would you assign to that new system?
  - A. 216.83.11.45
  - B. 216.83.11.87
  - C. 216.83.11.95
  - D. 216.83.11.96
- 10. You are configuring the gigabit interface on the Cisco router with the last valid IP address of the 131.107.48.0/20 network. What address would you use?
  - A. 131.107.48.254
  - B. 131.107.255.254
  - C. 131.107.64.254
  - D. 131.107.63.254

## Performance-based Question

1. You are helping a junior administrator to understand the IP addressing scheme used on your network. You have subnetted your network to support 16 subnets with the address scheme of 200.15.36.0/28. Looking at the different addresses listed on the left side of the following exhibit, identify if the address is a broadcast address, network ID, or host address.

- 200.15.36.22
- 200.15.36.16
- 200.15.36.79
- 200.15.36.60
- 200.15.36.31
- 200.15.36.48

Broadcast Address

Network ID

Host Address

## Answers

- 1. B. Given that you are dealing with a class B address, the default subnet mask is 255.255.0.0. Therefore, if you take three bits (needed for six networks) from the host ID, you will get a new subnet mask of 255.255.224.0.
- 2. B. You need to mask three bits to get a minimum of eight networks. The formula to calculate how many bits you need to take from the host ID portion of the subnet mask is 2^masked bits = number of networks. So, in this case, 2^3 = 8 networks.
- 3. D. The broadcast address is calculated by converting all host bits to 1.
- 4. A. The network ID is determined by setting all host bits to 0.
- 5. B. The last valid address is always one less than the broadcast address.
- 6. D. This is a tricky question. When answering these types of questions (and you will get them on the exam), you should first look at the address you were given—200.56.88.96/27. First focus on the /27; this indicates that 27 bits are enabled in the subnet mask. This is a class C address, which normally has only 24 bits enabled, so it tells you that this is a subnetted network. You know there are three extra bits that have been converted to subnet bits. Using the shortcut method, you know that the value of the last bit that was subnetted (the third extra bit) is 32. To answer the question, you should list out all the network IDs for the scenario on paper, using the increments: 200.56.88.0, 200.56.88.32, 200.56.88.64, 200.56.88.96, and so on. You can stop when you see the network that applies to this question—the 200.56.88.96 network (or subnet). Now you can answer the question about the fourth valid address of that subnet: 97 is the first valid address, 98 is second, 99 is third, and 100 is the fourth valid address.
- 7. B. Another tricky question, but you can answer it similar to question 6. Looking at the address, you can see that it starts with 138, which is a class B address. Class B addresses have a default subnet mask of 255.255.0.0, which means there are 16 bits enabled in the subnet mask—this is /16 in CIDR notation. Because the address given is using a /20 instead of a /16, you know that the question is a subnetted example. With the four extra bits, that means the increment is 16 (the value of the last enabled bit of the four subnetted bits), so you list out
