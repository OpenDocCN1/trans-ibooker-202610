**unicast frame** With a unicast frame, the destination MAC address denotes a single device.

**unshielded twisted pair (UTP)** A four-pair copper wire, where each pair is periodically twisted. It is cheap to install and troubleshoot, but it is susceptible to electromagnetic interference (EMI) and radio frequency interference (RFI), and cable distances are limited to a short haul.

**User Datagram Protocol (UDP)** This protocol provides an unreliable connection at the transport layer. UDP doesn’t go through a three-way handshake to set up a connection—it simply begins sending the information. Likewise, UDP doesn’t check to see if sent segments were received by a destination; in other words, it doesn’t have an acknowledgment process. Typically, if an acknowledgment process is necessary, the application layer will provide this verification.

**user EXEC mode** An access level for Cisco devices that provides basic access to IOS, with limited command availability (basically simple monitoring and troubleshooting). If you see a > character at the end of the prompt information, you know that you are in user EXEC mode.

**variable-length subnet masking (VLSM)** A subnet design strategy that enables you to have more than one mask for a given subnet when subnetting the original class A, B, or C network address. Classful protocols such as RIP v1 and IGRP do not support VLSM. Deploying VLSM requires a routing protocol that is classless, such as BGP, EIGRP, IS-IS, OSPF, or RIP v2. VLSM provides two major advantages: more efficient use of addressing and the ability to perform route summarization.

**virtual circuit (VC)** A logical connection between two devices. Therefore, many VCs can exist on the same physical connection. VCs can provide full connectivity at a much lower price than using leased lines. VCs are used in Asynchronous Transfer Mode (ATM), Frame Relay, and X.25.

**virtual LAN (VLAN)** A group of networking devices in the same broadcast domain that are not restricted to any physical boundary in the switched network, assuming that all the devices are interconnected via