address. Common protocols that use RARP are Bootstrap Protocol (BOOT P) and DHCP.

**RJ-45** This connector is located on the end of twisted-pair cables. It looks like a telephone connector but is wider, because there are eight pins instead of the four pins found with telephone cable (which uses an RJ-11 connector).

**rollover cable** A type of cable used for console connections that resembles an Ethernet CAT 5 cable; however, a rollover cable is proprietary to Cisco and will not work for other types of connections. The rollover cable has eight wires inside its plastic shielding. Each side of the rollover cable reverses the wires compared to the other side: pin 1 on one side is wired to pin 8 on the other side; pin 2 is wired to pin 7, and so on.

**ROM Monitor (ROMMON)** On a Cisco router or switch, the ROM Monitor loads a bootstrap program that allows for low-level diagnostic testing of the IOS device, performs the password recovery procedure, and can perform an emergency upgrade if the IOS image is corrupted or missing in flash.

**root bridge or switch** When STP is running, a spanning tree, which is basically an inverted tree structure, is first created. At the top of the tree is the root bridge. From the root bridge are branches (physical Ethernet connections) connecting to other switches, and branches from these switches connect to other switches, and so on. The layer 2 device with the lowest bridge ID (bridge priority + MAC address) is elected as the root.

**root port** In STP, each non-root switch needs to select a single port it will use to reach the root switch. This is the root port, which has the best accumulated path cost to the root bridge or switch.

**router** A networking device that functions at the network layer. Because routers operate at a higher layer than layer 2 devices and use logical addressing, they provide many advantages. Routers perform the following functions: define logical addressing schemes, contain broadcasts and multicasts, find layer 3 paths to destinations, connect different media types,