
You should also be familiar with the PDU terms used at the various OSI Reference Model layers—segment/transport layer, packet or datagram/network layer, frame/data link layer, and bits/physical layer. You should also be able to describe how devices communicate with other devices at the various layers: what layer 2 or layer 3 addresses are used between various devices from the source to the destination. Remember that MAC addresses can change from link to link, but layer 3 addresses, such as TCP/IP addresses, typically do not.

## Chapter Review

The OSI Reference Model defines the process of connecting two layers of networking functions. The application layer provides the user interface. The presentation layer determines how data is represented to the user. The session layer is responsible for setting up and tearing down connections. The transport layer is responsible for the mechanics of connections, including guaranteed services. The network layer provides a logical topology and layer 3 addresses: routers operate here. The data link layer defines MAC addresses and how communication is performed on a specific media type: switches, bridges, and NICs operate here. The physical layer defines physical properties for connections and communication: repeaters and hubs operate here. Wireless solutions are defined at the physical layer.

The transport layer sets up and maintains a session layer connection and provides for reliable or unreliable delivery of data, flow control, and multiplexing of connections. Reliable connections typically go through a handshake process to establish a connection. Acknowledgments are used to provide reliable delivery. Port or socket numbers are used for connection multiplexing. Ready/not ready signals and windowing are used to implement flow control. Windowing is more efficient than ready/not ready signals.

The network layer defines logical addresses, finds paths to destinations based on the network component of the address, and connects different layer 2 media types together. Routers are used to contain broadcasts. Routers use their routing table, which has a list of destination network numbers, to assist them when finding a destination. If a destination is not found in the routing table, the router drops traffic for this destination.

The data link layer defines hardware addressing. MAC addresses are 48 bits in length in hexadecimal. The first 24 bits (six digits) are the organizationally unique identifier (OUI). MAC addresses need to be unique only on a logical segment. In a unicast, one frame is sent to all devices on a segment, but only a single device will process it. In a multicast, one frame is sent to a group of devices. In a broadcast, one frame is sent to all devices.

A protocol data unit (PDU) describes data and its overhead. A PDU at the application layer is referred to as data, the transport layer PDU is called a segment, the network layer PDU is called a packet or datagram, the data link layer PDU is called a frame, and the physical layer PDU is called bits. As traffic goes down the protocol stack, each layer encapsulates the PDU from the layer above it. At the destination, a de-encapsulation process occurs as the traffic moves back up the protocol stack.

## Quick Review

### Introduction to the OSI Reference Model

- The OSI Reference Model provides the following advantages: it promotes interoperability, defines how to connect adjacent layers, compartmentalizes components, allows a modular design, serves as a teaching tool, and simplifies troubleshooting.

### Layers of the OSI Reference Model

- The application layer (layer 7) is responsible for initiating a network request, or servicing that network request. The presentation layer (layer 6) defines the format of the information. The session layer (layer 5) is responsible for the setup and teardown of the dialog. The transport layer (layer 4) handles the mechanics of reliable or unreliable services. The network layer (layer 3) is responsible for logical addressing and routing. Routers function at layer 3. The data link layer (layer 2) assigns physical (MAC) addresses and defines how devices on a specific media type communicate with each other. Bridges, switches, and NICs operate at layer 2. The physical layer (layer 1) handles all physical properties for a connection. Hubs and repeaters function here.

- The data link layer defines MAC addresses, the physical or hardware topology, and the framing used; it provides for connection-oriented and connectionless services. MAC addresses are 48 bits in length and are represented in hexadecimal. The first six digits are the OUI (vendor code), and the last six digits represent the NIC within the OUI. A unicast is sent to one destination on a segment, a multicast is sent to a group of devices, and a broadcast (FF:FF:FF:FF:FF:FF) is sent to all devices.

- The network layer defines logical addresses, finds paths to destinations using the network number in the logical address, and connects different media types together. Routers function at the network layer. A routing table contains information about destination network numbers and how to reach them. Routers contain broadcasts, allow for scalability through hierarchical designs, make better decisions for reaching a destination than bridges, can switch packets on the same interface using VLANs, and can implement advanced features such as QoS and filtering.

- The transport layer sets up and maintains a session connection, segments data into smaller payloads, provides for reliable or unreliable transport of data, implements flow control, and multiplexes connections. Reliable connections use sequence numbers and acknowledgments. TCP is an example. Reliable transport protocols use a handshake process to set up a connection. Unreliable services don’t use a connection setup process. UDP is an example. Multiplexing of connections is done with port or socket numbers. Flow control can be implemented with ready/not ready signals or windowing. Windowing is more efficient. The size of the window affects your throughput. Depending on the size, a source can send X segments before having to wait for an acknowledgment.

### Encapsulation and De-encapsulation

- A protocol data unit (PDU) describes data and its overhead. Each layer has a unique PDU: as data is sent down the protocol stack, it is encapsulated at each layer by adding a header and, possibly, a trailer. The destination de-encapsulates the data as it goes back up the protocol stack.

- The transport layer PDU is a segment, the network layer PDU is a packet or datagram, the data link layer PDU is a frame, and the physical layer PDU is bits.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully, as there may be more than one correct answer. Choose the correct answer(s) for each question.

1. The OSI Reference Model provides for all of the following except which one?
A. Defines the process for connecting two layers together, promoting interoperability between vendors
B. Allows vendors to compartmentalize their design efforts to fit a modular design, which eases implementations and simplifies troubleshooting
C. Separates a complex function into simpler components
D. Defines eight layers common to all networking protocols
E. Provides a teaching tool to help network administrators understand the communication process used between networking components

2. Put the following OSI Reference Model layers in the correct order, from high to low: (a) session, (b) presentation, (c) physical, (d) data link, (e) network, (f) application, (g) transport.
A. c, d, e, g, a, b, f
B. f, a, b, g, d, e, c
C. f, b, g, a, e, d, c
D. f, b, a, g, e, d, c

3. The _________ layer of the OSI model provides physical addressing.
A. Transport
B. Network
C. Data link
D. Physical

4. Sue has noticed that the first half of the MAC address (24 bits) is the same for all the network cards on your computers. What does the first 24 bits of a MAC address represent?
A. AUI
B. NIA
C. RTP
D. OUI

5. The network layer handles all of the following problems except ________.
A. Broadcast problems
B. Conversion between media types
C. Hierarchy through the use of physical addresses
D. Splitting collision domains into smaller ones

6. ________ are used to provide a reliable connection.
A. Ready/not ready signals
B. Sequence numbers and acknowledgments
C. Windows
D. Ready/not ready signals and windowing

7. Connection multiplexing is done through the use of a ________ number.
A. Socket
B. Hardware
C. Network
D. Session

## Performance-based Questions

The Cisco exams have performance-based questions for which you must drag items from the left side of the screen to its proper place on the right side of the screen in order to answer the question. Following are some sample performance-based questions.

1. Match the device with the layer of the OSI Reference Model that the device functions at.
