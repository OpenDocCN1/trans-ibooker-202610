+   m. Type **tcp.flags="S"** to turn the ACK flag off, by specifying just the SYN flag should be on.
+   
+   n. Type **tcp.dport=11210** to change the destination port in the TCP segment to 11210.
+   
+   Change the display filter in Wireshark on the Windows 10 VM to **tcp.port==11210**.
+   
+   o. One at a time, type
+   `ip.display()`
+   `tcp.display()`
+   
+   to take one more look at the IP packet’s and TCP segment’s values, as shown in Figure 16-37.