
![FIGURE 4.2 TCP three-way handshake](images/c04f002.png)

**[FIGURE 4.2](#c04.xhtml#R_c04-fig-0002) TCP three-way handshake**

TCP's connection-oriented nature makes it a reliable protocol, because it is guarantees delivery by acknowledging packets, checking for missing or corrupted packets, and requesting retransmission, when necessary. Due to the additional overhead associated with the three-way handshake, acknowledging packets, and error checking and correction, TCP is generally a slower protocol than connectionless protocols like UDP. TCP is used by Hypertext Transfer Protocol (HTTP), Hypertext Transfer Protocol Secure (HTTPS), and File Transfer Protocol (FTP), and other protocols that require reliable data transfer and extensive error checking.

### User Datagram Protocol

In contrast with TCP, UDP is a connectionless protocol. There is no overhead associated with establishing a connection, maintaining a connection, or terminating a connection, making UDP simpler, faster, and more efficient than TCP.

UDP is often referred to as a “best-effort” communications protocol. It offers none of the features of TCP — no error detection or correction, no sequencing, no flow control mechanisms — and does not use a pre-established session. In short, UDP is considered an unreliable protocol, which is fine for certain applications. The protocol's low overhead permits quick data transmission, which is ideal for real-time streaming of audio and video, where speed is more important than ensuring that every packet arrives perfectly. UDP is also commonly used by Domain Name System (DNS), Dynamic Host Control Protocol (DHCP), Simple Network Management Protocol (SNMP), and Voice over IP (VoIP), among other applications. The ongoing rise of Internet of Things (IoT) technologies is also leading to an increased use of UDP, as sensor data is typically ideal for sending via UDP vs. TCP.

### Layer 4 Attack Vectors

Attack vectors unique to the transport layer would include attacks utilizing TCP and UDP. One specific example would be the SYN flood attack that drains a target's network memory resources by continuously initiating TCP-based connections, but not allowing them to complete; this creates a DoS that prevents legitimate connections to the target. Trojans and other malware also tend to target specific TCP and UDP ports. Session hijacking is another common attack that can operate at layer 4 (as well as other layers).

## Layer 5: Session Layer

The session layer facilitates the logical, persistent connection between systems on a network. This layer controls the dialogue (connection) between a local application and a remote application. The session layer is responsible for establishing, coordinating, and terminating a session efficiently and effectively between end-user application processes.

The session layer is commonly implemented in applications that use remote procedure calls (RPCs) to handle requests and responses that occur between applications. RPCs are the mechanism for application environments to make service requests and service responses between applications on various networked devices. In other words, RPCs allow a procedure (a sequence of tasks or instructions) to be created on a client and performed on a server device — the session layer manages this process.

There are three primary modes of operation to choose from when a session is established:

* **Full-duplex mode:** This is when data is sent over a connection between two devices in both directions at the same time. Full-duplex channels can be constructed either as a pair of simplex links or using one channel designed to permit bidirectional simultaneous transmissions. If multiple devices need to be connected using full-duplex mode, many individual links are required because one full-duplex link can connect only two devices.
* **Half-duplex mode:** Half-duplex has the capability of sending data in both directions, but in only one direction at a time. While this may seem like a step down in capability from full-duplex, it is widely used and successful across single network media such as cable, radio frequency, and Ethernet, as examples. The communications work well when the devices take turns sending and receiving.
* **Simplex mode:** This mode involves using a communication channel that operates as a one-way street. An example of simplex construction is where a fiber optics run or a network cable as a single strand sends data and another separate channel receives it.

### Layer 5 Attack Vectors

What sorts of attacks target the session layer? This layer and the presentation layer aren't popular targets for common attacks. Routing or packet-based attacks (lower on the OSI model) and application-level attacks (higher on the OSI model) offer “low-hanging fruit” for attackers, leaving the session and presentation layers at an awkward level of abstraction for identifying vulnerabilities. Still, given the layer's functions, you can hypothesize an attack where authentication to establish a session across the network is compromised. There are also specific session layer protocols, such as NetBIOS and Network File System (NFS), that may be vulnerable to DoS and root privilege attacks, for example. Weaknesses in the deprecated Secure Sockets Layer (SSL) protocol and less secure versions of Transport Layer Security (TLS) are also prime targets at the session layer.

## Layer 6: Presentation Layer

The purpose of the presentation layer is to translate data between the lower layers and the application layer. At lower levels, data is sent as datagrams and packets. The presentation layer is the first layer in the OSI model at which data structure and presentation are evident, as this layer is responsible for applying coding and conversion functions to data being presented to the application layer.

Here are some examples of the coding and conversion at this layer:

* Common data representation formats, such as the use of standard image, sound, and video formats
* Common data compression schemes, such as JPEG and MPEG
* Common data encryption and decryption schemes

These functions enable the interchange of application data between different types of computer systems. In short, the presentation layer ensures that formats are converted and presented to the application layer as needed.

### Layer 6 Attack Vectors

The most common attacks at this layer involve attacks on the encryption schemes themselves. Cryptography and cryptanalytic attacks are covered in [Chapter 3](#c03.xhtml), “Security Architecture and Engineering.”

## Layer 7: Application Layer

The application layer is the highest layer of the OSI model and the closest layer to the end user. This layer supports network access for software applications and provides an interface for the user. The software applications do not reside at the application layer. Instead, this layer facilitates communication through the lower layers to establish connections with applications at the other end of the network connection. Web browsers (e.g., Chrome, Safari, Firefox), HTTP, FTP, and Telnet all rely on the application layer to function properly.

### Layer 7 Attack Vectors

Attack vectors specific to the application layer vary wildly, and most security breaches occur at this layer. To begin the list, consider weaknesses in application layer protocols such as HTTP, FTP, Simple Mail Transfer Protocol (SMTP), and SNMP. Every attack on the user interface falls into this category, as well as web-based attacks such as HTTP flooding or input validation attacks. SQL injection and cross-site scripting (XSS) are among the most common attacks that operate at the application layer.
