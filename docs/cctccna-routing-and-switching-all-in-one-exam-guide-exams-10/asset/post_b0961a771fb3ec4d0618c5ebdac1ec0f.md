
![](img/0eafdcebe219ef3756f0e57bcf1eb997_123_0.png)

Figure 2-7 Each layer of the OSI model adds a header.

Generically speaking, *protocol data unit* (PDU) describes data as it passes through each layer of the OSI model. At each layer, a new header is added to the data. Table 2-3 describes the PDU terms used at the various layers. For instance, as data is passed from the session layer to the transport layer, the transport layer *encapsulates* the data PDU by adding a layer 4 header. At this point the data is now referred to as a segment. As the PDU information is passed down, each layer adds its own header, and at that point the data uses a different term in the model. For example, once the layer 3 header is added, the data is no longer termed a segment; it is now considered a *packet*. Likewise, once layer 2 header information is added, the data is known as a *frame*. This is important for you to understand, because when you read Chapters 9 and 10 you will notice that we do not use the term packet—a switch runs at layer 2, so therefore it works with frames instead of packets.

| PDU Terms | OSI Reference Model Layers |
|---|---|
| Data | Application, presentation, and session layers |
| Segment | Transport layer |
| Packet | Network layer |
| Frame | Data link layer |
| Bits | Physical layer |

Table 2-3 PDU Terms and Layers

![](img/0eafdcebe219ef3756f0e57bcf1eb997_124_0.png)

EXAM TIP You’ll need to know the layer designations for the PDU terms in Table 2-3, and you should remember that computers deal with data in binary (bits). A quick mnemonic you can use to remember PDU terms is “Do Sergeants Pay For Beer?” (data, segment, packet, frame, bits).

Once the physical layer is reached, the bits of the data link layer frame are converted into a physical layer signal—a voltage, light source, radio wave, or other source according to the type of physical medium that is employed. When the destination receives the information, it goes through a reverse process of de-encapsulating information—basically stripping off the headers and trailers of the PDU information at each layer as the information is passed up from layer to layer of the OSI model.

Figure 2-8 shows an example of the processes used for encapsulating and de-encapsulating PDUs as data is passed down and back up the layers. In this example, you can see how the application, presentation, and session layers create the data PDU. As this information is passed down from layer to layer, each layer adds its own header (and possibly trailer, as is the case with most layer 2 protocols).

![](img/0eafdcebe219ef3756f0e57bcf1eb997_125_0.png)

Figure 2-8 Encapsulation and de-encapsulation processes

The next few sections will help you better understand the process that devices go through as information is transmitted between computers. The next section covers the details as to how information is encapsulated and sent down the protocol stack and then placed on the wire to the destination. The section following that covers the reverse process: how the information is de-encapsulated at the destination and delivered to the application at the application layer. The last part of the chapter looks at a more complex environment, where bridges, routers, and hubs are involved in the communication process to get information from the source to the destination.

## Going Down the Protocol Stack

This section covers the basic mechanics as to how information is processed as it’s sent down the protocol stack on a computer. Consult the diagram shown earlier in Figure 2-8 to follow along with this process as PC-A sends data to PC-B. In this example, assume that the data link layer is Ethernet and the physical layer is copper.

The first thing that occurs on PC-A is that the user, sitting in front of the computer, creates some type of information, or data, and then sends it to another location (PC-B). This includes the actual user input (application layer), as well as any formatting information (presentation layer). The application (or operating system) at the session layer then determines whether or not the data's intended destination is local to this computer (possibly a disk drive) or a remote location. In this instance, the user is sending the information to PC-B. We’ll assume that the user is executing a telnet connection.

The session layer determines that this location is remote and has the transport layer deliver the information. A telnet connection uses TCP/IP and reliable connections (TCP) at the transport layer, which encapsulates the data from the higher layers into a segment. With TCP, as you will see in Chapter 3, only a header is added. The segment contains such information as the source and destination port numbers.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_126_0.png)

**CAUTION** Based on RFC standards, the TCP or UDP source port number really should be above 49,151, but not all operating systems follow this standard verbatim—in many cases, the source port number will be above 1023.

The transport layer passes the segment down to the network layer, which encapsulates the segment into a packet. The packet adds only a header, which contains layer 3 logical addressing information (source and destination address) as well as other information, such as the upper-layer protocol that created this information. In this example, TCP created this information, so this fact is noted in the packet header, and PC-A places its IP address as the source address in the packet and PC-B’s IP address as the destination. This helps the destination, at the network layer, to determine whether the packet is for itself and which upper-layer process should handle the encapsulated segment. In the TCP/IP protocol stack, the terms *packet* and *datagram* are used interchangeably to describe this PDU. As you will see in Chapter 3, many protocols are involved in the TCP/IP protocol stack—Address Resolution Protocol (ARP), TCP, UDP, Internet Control Message Protocol (ICMP), Open Shortest Path First (OSPF), Enhanced Interior Gateway Routing Protocol (EIGRP), and many others.

The network layer then passes the packet down to the data link layer. The data link layer encapsulates the packet into a frame by adding both a header and a trailer. This example uses Ethernet as the data link layer medium, which was discussed in Chapter 1. The important components placed in the Ethernet frame header are the source and destination MAC addresses, as well as a frame check sequence (FCS) value so that the destination can determine whether the frame is valid or corrupted when it is received. In this example, PC-A places its MAC address in the frame in the source field and PC-B’s MAC address in the destination field. FCS is discussed in more depth in Chapter 1.

The data link layer frame is then passed down to the physical layer. At this point, remember that the concept of “PDUs” is a human concept that we have placed on the data to make it more readable to us, as well as to help deliver the information to the destination. However, from a computer’s perspective, the data is just a bunch of binary values, 1s and 0s, or bits. The physical layer converts these bits into a physical property based on the cable or connection type. In this example, the cable is a copper cable, so the physical layer will convert the bits into voltages: one voltage level for a bit value of 1 and a different voltage level for a 0.

## Going Up the Protocol Stack

For the sake of simplicity, assume PC-A and PC-B are on the same piece of copper. Once the destination PC receives the physical layer signals, the physical layer translates the voltage levels back to their binary representation and passes these bit values up to the data link layer.

The data link layer reassembles the bit values into the original data link frame (Ethernet). The network adapter examines the FCS to make sure the frame is valid and examines the destination MAC address to ensure that the Ethernet frame is meant for itself. If the destination MAC address doesn’t
