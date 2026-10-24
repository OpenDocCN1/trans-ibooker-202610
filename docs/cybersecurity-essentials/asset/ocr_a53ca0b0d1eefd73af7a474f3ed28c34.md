Therefore, when referring to the network portion of an IPv6 address, the first address in the block of contiguous addresses would begin with the network portion of the address followed by a string of all zeros:

```
2001: db8: a7:0000:0000:0000:0000:0000
```

and would end in the network address plus a string of “f16”s (for 11112 or 1510):

```
2001: db8: a7: ffff:ffff:ffff:ffff:ffff
```

It is also common to see ranges of IPv6 addresses written in CIDR notation. Under IPv6, this system uses the routing prefix portion of the address followed by the slash (/) character and a decimal number that represents the number of bits reserved by the prefix. Applying this to the block of addresses just described produces the following notation:

```
2001:0db8:00a7:/48
```

The /48 value is calculated by taking the number of places held by the routing prefix (12) and multiplying the number of places by the 4 bits required to represent each hexadecimal place.

# A Note About Leading Zeros

The leading zeros have been restored in this example for ease of calculations. They would not normally be in place in a real CIDR address.

The IPv6 transmission packet format is not the same as the IPv4 packet. Therefore, they are not interchangeable (IPv4 devices are not directly compatible with IPv6 devices). Newer computing devices and networking equipment possess IPv6-ready detection and connectivity capabilities.

While both IPv4 and IPv6 networks can be divided into subnetworks, there is no equivalent term for subnet masking in IPv6.

# Private IP Classes

Because the Internet is basically a huge TCP/IP network in which no two devices connected to it can have the same address, if companies utilized Internet routable addresses for every system, the world would have exhausted the IP addressing scheme many years earlier.