able to be divided into multiple virtual local area networks (VLANs). You will learn about VLANs later in this chapter.

After you unbox the switch, you’ll appreciate that it does not require any configuration to perform its job role, which is to act as a connectivity point for all devices on the network. The switch is ready to perform that job out of the box; simply give it power and then connect the systems to the switch, and it will forward traffic to each of the connected devices as needed.

Unlike a router, a switch, by default, doesn’t require that you apply configuration settings. There are, however, a number of features that you can configure on the switch that enable you to tweak how the switch operates and to use advanced features that are not configured by default.

# Switch Functions

Switches are layer 2 devices—that is, they operate at the data link layer of the Open Systems Interconnection (OSI) model. Although a switch has many capabilities, they do have core functions: they learn, forward, and avoid loops (more on those three functions in a bit). Switches make all of their switching decisions based on the destination Media Access Control (MAC) address in the header of the frame that is received, and all of the processing is performed in hardware by using application-specific integrated circuits (ASICs). ASICs are specialized processors built to perform very few specific tasks. Because they do only a few things, ASICs are much more cost-effective than generic processors, such as the one in your PC. Cisco, like most networking vendors, extensively uses ASICs throughout its switching products.

A switch has the following three main functions:

+   - **Address learning/MAC learning** When a computer sends a frame to another computer and that frame reaches the switch, the switch records the sending MAC address and the port that system is connected to in the MAC table of the switch. Any time the switch receives a frame, it checks the MAC table to see what port the destination system is connected to.
+   - **Filtering/forwarding** Switches filter traffic by forwarding the frame only to the port of the destination system—again, drawing on information the switch has recorded in its MAC table.