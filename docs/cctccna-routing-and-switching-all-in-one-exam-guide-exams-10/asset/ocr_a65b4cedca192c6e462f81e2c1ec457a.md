numbers, to assist them when finding a destination. If a destination is not found in the routing table, the router drops traffic for this destination.

The data link layer defines hardware addressing. MAC addresses are 48 bits in length in hexadecimal. The first 24 bits (six digits) are the organizationally unique identifier (OUI). MAC addresses need to be unique only on a logical segment. In a unicast, one frame is sent to all devices on a segment, but only a single device will process it. In a multicast, one frame is sent to a group of devices. In a broadcast, one frame is sent to all devices.

A protocol data unit (PDU) describes data and its overhead. A PDU at the application layer is referred to as data, the transport layer PDU is called a segment, the network layer PDU is called a packet or datagram, the data link layer PDU is called a frame, and the physical layer PDU is called bits. As traffic goes down the protocol stack, each layer encapsulates the PDU from the layer above it. At the destination, a de-encapsulation process occurs as the traffic moves back up the protocol stack.

# Quick Review

# Introduction to the OSI Reference Model

+   - The OSI Reference Model provides the following advantages: it promotes interoperability, defines how to connect adjacent layers, compartmentalizes components, allows a modular design, serves as a teaching tool, and simplifies troubleshooting.

# Layers of the OSI Reference Model

+   - The application layer (layer 7) is responsible for initiating a network request, or servicing that network request. The presentation layer (layer 6) defines the format of the information. The session layer (layer 5) is responsible for the setup and teardown of the dialog. The transport layer (layer 4) handles the mechanics of reliable or unreliable services. The network layer (layer 3) is responsible for logical addressing and routing. Routers function at layer 3. The data link layer (layer 2) assigns physical (MAC) addresses and defines how devices on a specific media type communicate with each other. Bridges, switches, and NICs operate at layer 2. The physical layer