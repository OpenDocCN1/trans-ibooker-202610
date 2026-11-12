on the far right side. Just like figuring out the value of an octet with IPv4, you can calculate the 4 bits used by taking the decimal number (7 is the example in the figure) and turning on (set the bit to 1) the 4-, 2-, and 1-bit placeholders. This will create a decimal value of 4 + 2 + 1 = 7. Looking at the chart in the figure, you’ll see that the decimal value of 7 is also the hex value of 7, which has a binary value of 0111. Knowing that each character is based off a 4-bit value, you could use a conversion table to help you figure out what each hex value is in binary.

To start the table, I list 16 decimal numbers in order from 0 through 15 (because hexadecimal address is 16 based). I then list each hexadecimal value in order, from 0 to 9 (that is only 10 numbers; we need 6 more), then the tenth hex value is A and continues to F to make the 16 values in the hex system. Finally, I create a third column, which is the binary value of each of those hex values—you can cheat and use the corresponding decimal number of a hex value to figure out what the binary should be. For example, the hex value of “C” corresponds to a decimal value of 12, and to calculate 12 in binary is 1100 (8 + 4 + 0 + 0).

This means that the first block in the IPv6 of fe80 has the 16-bit pattern of

```
1111 1110 1000 0000
```

# Types of IPv6 Addresses

Recall from Chapter 4 that many types of IPv4 addresses exist: unicast, broadcast, multicast, research, private, and so on. IPv6 also has different types of addresses. Following are the three main types:

+   - **Anycast** This is very different from an IPv4 broadcast. An anycast address is used for one device to talk to the nearest device that has that address assigned to its interface (one-to-the-nearest address), where many interfaces can share the same address. These addresses are taken from the unicast address space but can represent multiple devices, such as multiple default gateways. For example, using an anycast address as a default gateway address on your routers, user devices have to know of only one address, and you don’t need to configure a protocol such as Hot Standby Router Protocol (HSRP) or Virtual Router Redundancy Protocol (VRRP).