Now that you have calculated the new subnet mask, the network ID, the first valid address, and the broadcast address for your four new subnets, the only additional information you need is the last valid address that may be assigned to hosts on each subnet. To calculate the last valid host address of each subnet, simply subtract 1 from the broadcast address by disabling the least significant bit (the far-right host bit). To view the binary and decimal representations for our last valid address of each network, take a look at the following table:

|  | First Octet (Decimal) | Second Octet (Binary) | Third Octet (Binary) | Fourth Octet (Binary) | Calculation |
|---|---|---|---|---|---|
| Original IP | 10 | 0 | 0 | 0 |  |
| Subnet #1 | 10 | 00111111 | 11111111 | 11111110 | 10.63.255.254 |
| Subnet #2 | 10 | 01111111 | 11111111 | 11111110 | 10.127.255.254 |
| Subnet #3 | 10 | 10111111 | 11111111 | 11111110 | 10.191.255.254 |
| Subnet #4 | 10 | 11111111 | 11111111 | 11111110 | 10.255.255.254 |

You have now calculated all of the information required to configure the four physical network segments that you have created. The following table summarizes the configuration for each of the four network segments, and Figure 5-2 displays how these four network segments will be configured.

|  | Network ID | First Valid Address | Last Valid Address | Broadcast Address | Subnet Mask |
|---|---|---|---|---|---|
| Subnet 1 | 10.0.0.0 | 10.0.0.1 | 10.63.255.254 | 10.63.255.255 | 255.192.0.0 |
| Subnet 2 | 10.64.0.0 | 10.64.0.1 | 10.127.255.254 | 10.127.255.255 | 255.192.0.0 |
| Subnet 3 | 10.128.0.0 | 10.128.0.1 | 10.191.255.254 | 10.191.255.255 | 255.192.0.0 |
| Subnet 4 | 10.192.0.0 | 10.192.0.1 | 10.255.255.254 | 10.255.255.255 | 255.192.0.0 |