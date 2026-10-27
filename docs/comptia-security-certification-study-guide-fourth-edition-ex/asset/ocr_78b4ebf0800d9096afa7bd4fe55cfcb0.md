Over the years different types of port scans have been developed. The following list provides a few of the major port scan types:

+   - **TCP connect scan** A TCP connect scan performs a three-way handshake with each port to determine if the port is open. The following nmap command shows how to do a TCP connect scan against the entire 192.168.2.0 network:
+   
+     ```
+     nmap -sT 192.168.2.0/24
+     ```
+   - **SYN scan (half open)** Because a TCP connect scan does a full three-way handshake, it creates more traffic than necessary. You can do a SYN scan, which is also known as a half-open scan, or stealth scan, because it does not complete a full three-way handshake. The scanning software sends a SYN message to the port, and if the target system replies with a SYN-ACK, then the connection is dropped. The idea here is that a reply came back, so the port is most likely open, and there is no need to send the final ACK message of a three-way handshake. This scan is also known as a stealth scan because there is less traffic generated in hopes of avoiding detection, and you are not fully opening a connection to the port. The following command displays how to do a SYN scan with nmap:
+   
+     ```
+     nmap -sS 192.168.2.0/24
+     ```
+   - **Inverse scan** An inverse scan gets its name because it replies in the opposite way than you would expect. With an inverse scan, if the port is open, no reply is sent back, but if the port is closed, then a reset (RST) is sent back (see Figure 20-8). The different types of inverse scans include a null scan, which sends a packet to the port with no flags set, and an XMAS scan, which sends a packet with the PSH, URG, and FIN flags set. A FIN scan sends a packet with only the FIN flag set. Notice that the name of the scan comes from what flags have been set in the packet. The following commands show the FIN scan and the XMAS scan being used by nmap:
+   
+     ```
+     nmap -sF 192.168.2.0/24
+     nmap -sX 192.168.2.0/24
+     ```

Note that inverse scans do not work on Windows systems and, as a result, respond with no ports being open. Normally, you will use SYN scans or TCP connect scans on Windows networks.

# FIGURE 20-8

Inverse scans respond the opposite way of what you would expect.

![](bbox=[0.28, 0.78, 0.85, 0.92])