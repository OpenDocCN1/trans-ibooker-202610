+   - **FIN** The finish flag specifies that you would like to finalize, or end, the connection. This is how a TCP connection is ended the polite way—it is like saying goodbye to end a phone conversation.
+   - **RST** The reset flag is used to end a TCP conversation impolitely. This is like hanging up the phone without saying goodbye.

# TCP Header

Every packet that is sent using TCP has a TCP header assigned to it, which contains TCP-related information such as the source port, destination port, and the TCP flags. Figure 3-5 displays the different fields in the TCP header.

![](bbox=[0.14, 0.37, 0.86, 0.68])

Figure 3-5 The TCP header

Here are quick descriptions of each field in the TCP header:

+   - **Source Port** This 16-bit field identifies the port number of the sending system.
+   - **Destination Port** This 16-bit field identifies the port number the packet is destined for on the destination system.