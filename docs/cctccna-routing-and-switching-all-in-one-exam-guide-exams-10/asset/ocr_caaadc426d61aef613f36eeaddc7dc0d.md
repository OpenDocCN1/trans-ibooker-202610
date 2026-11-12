This section introduces you to the binary representation of an IP address and ensures that before you move on to Chapter 5, you feel comfortable with converting binary values to decimal and address classes. You’ll also learn about types of addresses, address schemes, and TCP/IP ports.

# Understanding Binary

You know from the previous discussion that the IP address is a 32-bit address divided into four 8-bit octets. The four octets are normally displayed as decimal values, but they also have a binary representation that looks like this:

```
11000000 10101000 00000001 00001111
```

Notice that there are four sets of 8 bits (1 or 0), which make up the 32 bits (8 × 4 sets) of an IP address. Let’s take a look at how you can determine the binary representation of an octet.

The values of the 8 bits within each octet are shown in Table 4-3. You can see that the first bit in an octet (far right) has a decimal value of 1, the second bit has a decimal value of 2, the third bit has a decimal value of 4, and the values keep doubling with each additional bit. You can also see that the eighth bit has a decimal value of 128.

| Bit | 8 | 7 | 6 | 5 | 4 | 3 | 2 | 1 |
|---|---|---|---|---|---|---|---|---|
| Value | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |

Table 4-3 Values Associated with Each Bit in an Octet

![](bbox=[0.12, 0.71, 0.25, 0.8])

NOTE The first bit is known as the least significant bit, or low-order bit, while the eighth bit is known as the most significant bit, or high-order bit.