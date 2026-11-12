# Internet Control Message Protocol

The Internet Control Message Protocol (ICMP) enables systems on a TCP/IP network to share status and error information. You can use the status information to detect network trouble. ICMP messages are encapsulated within IP datagrams so that they can be routed throughout a network. Two programs that use ICMP messages are ping and tracert.

You can use ping to send ICMP echo requests to an IP address and wait for ICMP echo responses. Ping reports the time interval between sending the request and receiving the response. With ping, you can determine whether a particular IP system on your network is functioning correctly. You can use many different options with the ping utility.

![](bbox=[0.14, 0.39, 0.24, 0.45])

EXAM TIP Remember for both the CCT and CCNA exams that ICMP is the protocol in the TCP/IP protocol suite that is responsible for error and status reporting. Programs such as ping and tracert use ICMP.

Tracert traces the path taken to a particular host. This utility can be useful in troubleshooting internetworks. Tracert sends ICMP echo requests to an IP address while it increments the TTL field in the IP header by a count of 1, after starting at 1, and then analyzing the ICMP errors that are returned. Each succeeding echo request should get one unit further into the network before the TTL field reaches 0 and an “ICMP time exceeded” error message is returned by the router attempting to forward it.

# ICMP Types and Codes

ICMP does not use port numbers, but instead uses ICMP types and codes to identify the different types of messages. For example, an echo request message that is used by the ping request uses ICMP type 8, while the ping reply comes back with an ICMP type 0 message.

Some of the ICMP types are broken down to finer levels with different codes in the type. For example, ICMP type 3 is a “destination unreachable”