![](bbox=[0.12, 0.09, 0.82, 0.34])

Figure 4.15 – ARP request

Now that we understand how ARP works, we can start working on creating our own ARP scanner with Scapy to find out the MAC address of these devices. You might be wondering why we need an ARP scanner. Well, knowing the MAC addresses of a device can help us perform a man-in-the-middle attack, which we will perform in Chapter 5, Man in the Middle Attacks.

# ARP scanner using Scapy

The ARP protocol works on the Ethernet layer, so using Scapy, we will import the Ethernet layer. Let’s import the layers and functions we will use:

```
from scapy.all import Ether, ARP, srp
```

If all the bits of a MAC address are set to 1, it means that the packet is a broadcast and it should go to every device in the network. Scapy uses hexadecimal representation, so we will create the following variable to denote the broadcast address:

```
broadcast = "FF:FF:FF:FF:FF:FF"
```

Then, we can create an Ethernet layer packet and put the destination as broadcast.

We will also need to define the ip range we want to scan. In my case, I want to scan my local network:

```
ip_range = "192.168.74.1/24"
```