+   - Understand how to use certain scans in conjunction with other scans for intelligence gathering

# Lab Materials and Setup

The materials you need for this lab are

+   - The *Principles of Computer Security: CompTIA Security+ and Beyond* textbook

+   - The Kali Linux VM you installed in Chapter 1

+   - The Windows 10 VM you installed in Chapter 1

+   - A web browser with an Internet connection

# Let’s Do This!

At Layer 4 of the OSI Model, TCP establishes a connection between two systems (referred to as client and server in this section) before any data is transferred, requires every message to be acknowledged, and guarantees data delivery through the TCP three-way handshake.

In nontechnical terms, the client says, “Hey, I want to talk to you!” The server says back, “I want to talk to you as well, and, yes, you can talk to me!” Then the client says, “Sure, let’s rock!”

Flags in the TCP header represent specific control information, each represented by a single bit. Turning a flag on (or setting it) means giving that bit a value of 1. Turning a flag off (or clearing it) means giving that bit a value of 0. There are six standard flags in the TCP header (as well as three specialty ones).

We’re going to focus on two of the standard flags now and the others later. Furthermore, we’ll be looking at a simplified TCP header, focusing only on the relevant fields and values.

In Step 1 of the TCP three-way handshake, as shown in Figure 16-1, the client sets the SYN (synchronize) flag on, by placing a 1 in that bit position, and comes up with a pseudo-randomly generated sequence number, which is placed in the Sequence Number field. Let’s say it’s 9. This step is simply referred to as SYN because of the SYN flag that is on.