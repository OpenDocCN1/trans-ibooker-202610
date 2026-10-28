Note In some types of binary operations, you will be often equating 1 for true and 0 for false.

The third important numerical system is known as hexadecimal, base-sixteen, or simply hex. In addition to the decimal range of 0 to 9, this system also uses the letters A to F to represent values between 10 and 16, that is, 11 in the decimal system is represented by the letter B in hex. This means the hexadecimal system has a total of 16 different symbols for representing values. Hex bodes well to high informational density: with only two digits, we can summon numbers up to 255. In binary, we would need eight digits.

# Converting Decimal to Binary

We’ll start with decimal to binary conversion. Let’s take the decimal value of, say, 86, and transport it into the magical realm of binary notation. The end result is 1010110. And how, you may ask? Well, think of it this way: binary numbers are collections of flags with only two positions – on and off. Binary notation starts at 1 and advance to the left with each value having twice that of the one to its right.

In our example, we start with the “flag” at number 64 (see Table 3-1). Does 64 go into 86? Oh yes. What about 32? No, then it’s too big. 16? Yes. We are now at 80. We just have to find the remaining 6 and we do that by setting the flags at 4 and 2 to “heck yes” and we’re done. We have thus demonstrated that 86 in decimal is indubitably 1010110 in binary. We may denote it as follows: (86)₁₀ = (1010110)₂

Table 3-1. A simple demonstration of converting the decimal number 86 to binary

| Decimal value | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| Binary value | 1 | 0 | 1 | 0 | 1 | 1 | 0 |