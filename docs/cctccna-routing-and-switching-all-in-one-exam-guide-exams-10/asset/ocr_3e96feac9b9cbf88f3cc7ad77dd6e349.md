```
Subnet 1: 190.45.00011111.11111110 = 190.45.31.254
Subnet 2: 190.45.00111111.11111110 = 190.45.63.254
Subnet 3: 190.45.01011111.11111110 = 190.45.95.254
Subnet 4: 190.45.01111111.11111110 = 190.45.127.254
Subnet 5: 190.45.10011111.11111110 = 190.45.159.254
Subnet 6: 190.45.10111111.11111110 = 190.45.191.254
Subnet 7: 190.45.11011111.11111110 = 190.45.223.254
Subnet 8: 190.45.11111111.11111110 = 190.45.255.254
```

# Subnetting Class C Networks

When subnetting a class C network, you follow the same steps, but because you are doing all of your manipulation of bits in the last octet, you will see some different numbers for things like the network IDs and the broadcast address. With class A and class B, those addresses usually end in a 0 or a 255.

In this scenario, you have a class network address of 202.78.34.0 and you need to divide the network into 12 subnets.

# Set Up the Work Area

The first step is to set up your work area on paper. Write down what you know already:

```
IP Range: 202.78.34.0
Subnet mask: 255.255.255.0
Subnet mask (binary): 11111111.11111111.11111111.00000000
```

# How Many Bits to Make Subnets?

Your next step is to decide how many bits from the host ID portion of the subnet mask you want to convert into subnet bits. Remember this formula:

2^{masked bits} = number of subnets

So, 2² = 4 subnets, which is not enough—you need 12 subnets. If you try 2³, you get 8 subnets, which is not enough. Using 2⁴ gives you 16 subnets, which meets your goal, but with 4 additional subnets for future growth.

Now that you have determined you want to convert four bits from host bits to subnet bits in the subnet masks, you get this: