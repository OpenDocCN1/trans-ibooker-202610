![](bbox=[0.14, 0.11, 0.24, 0.17])

**EXAM TIP** For the exam, remember that global addresses have a first digit of a 2 or 3, and are currently usually 2000 or 2001. An interface can be assigned multiple IPv6 addresses—this includes link-local, global unicast, and/or anycast addresses.

# Modified EUI 64

The interface ID is typically composed of a part of the MAC address of the interface. When this is done, the interface ID is commonly called an extended unique identifier 64 (EUI-64). Figure 14-4 shows an example of the frame with an EUI-64 format. The organizationally unique identifier (OUI), which is the first 24 bits of the MAC address on a network card, is mapped into the first 24 bits of the interface ID. The seventh bit in the highest order byte is set to 1, indicating that the interface ID is unique across the site, or 0, indicating that it is unique within the local scope only. The OUI mapping is followed by the 16-bit value of FFFE. The last 24 bits of the MAC address are then mapped into the last part of the interface ID.

![](bbox=[0.12, 0.57, 0.87, 0.88])