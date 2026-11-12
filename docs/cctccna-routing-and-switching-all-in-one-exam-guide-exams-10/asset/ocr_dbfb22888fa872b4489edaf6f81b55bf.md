binary. A default class A decimal subnet mask in binary looks like the following:

| Decimal | 255 | 0 | 0 | 0 |
| Binary | 11111111 | 00000000 | 00000000 | 00000000 |

Once you’ve written out the subnet mask in binary in your work area, you can begin to calculate the new addresses of the four subnets. The first thing you need to do is steal, or “mask,” two bits from the host ID portion of the subnet mask and make them subnet bits by setting them to a 1 state. Remember that you’re taking two bits because of your earlier calculation. Because the last three octets of this subnet mask represent the host bits of a class A address, work from left to right and mask two additional bits to get the new binary table that follows:

| Decimal | 255 | 0 | 0 | 0 |
| Binary | 11111111 | 11000000 | 00000000 | 00000000 |

Notice that the two bits that are now enabled are in the second octet. Also notice that you always work from left to right in the subnet mask.

# Calculate the New Subnet Mask

Now you can calculate the new subnet masks for all four networks. To do this, you convert the new binary value in the preceding table of 11111111.11000000.00000000.00000000 to its decimal notation—you should get 255.192.0.0 as the new subnet mask of your four new subnets. Write that number down, because you’ll need it later.

The next step is to calculate the IP ranges for the four different networks, but before that, you need to know five pieces of information about each of the new subnets you are designing before this walkthrough is complete:

+   - **New subnet mask** After subnetting a network, you will have a new subnet mask that is used by all subnets you have created.
+   - **Network ID** To calculate the network ID for each network, all host bits are set to 0.
+   - **First valid address** To calculate the first valid address, you set the least significant host bit to 1; all other host bits are 0.