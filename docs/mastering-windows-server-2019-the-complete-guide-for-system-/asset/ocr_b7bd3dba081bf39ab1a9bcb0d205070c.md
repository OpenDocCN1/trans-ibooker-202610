Now that we understand what sections of the address are going to be used for what purposes, how do we go about assigning the individual device ID numbers for all of the computers, servers, and other devices on our network? You could start with number 1 and go up from there. Another idea is to calculate out the old IPv4 addresses into hex and use this as the last 32 bits of the address—open up Windows Calculator on your computer, drop down the menu, and change it into Programmer mode. This is a quick and easy tool that you can use to convert decimal into hexadecimal, and vice versa. Let's take the example of my web server that is running on 192.168.1.5. I want to implement IPv6 inside my network, and I want my server's IPv6 addresses to reflect the original IPv4 address in the device ID section of the new address. In my calculator, if I type in 192 and then click on HEX, it will show me the corresponding hexadecimal to the decimal of 192, as shown in Figure 7.1:

![](bbox=[0.11, 0.315, 0.87, 0.68])

Figure 7.1: Using Windows Calculator to convert decimal to hexadecimal

If we do that with each of the octets in our IPv4 address, we will see the following:

```
192 = C0
168 = A8
1 = 01
5 = 05
```