# Chapter 16
Security Tools and Techniques

# Lab Exercises

+   16.01  Port Scanning with Nmap
+   16.02  Sockets with netcat (nc) and ncat
+   16.03  Packet Crafting with hping3
+   16.04  Packet Crafting with Scapy

Lab Analysis

Key Term Quiz

Securing your network involves not only the use of security tools but also the knowledge, skills, and techniques related to effectively, efficiently, and properly using them! Nmap is one of the most often used tools for both pentesters and cybercriminals. You’ll get knowledge and experience with it in this chapter.

You’ll also gain valuable knowledge and experience with netcat (nc), also known as the “TCP/IP Swiss Army Knife” for its many capabilities, including varied ways of creating different types of communication connections. Again, this is used by both the good guys and the bad guys.

Up until now in this book, all the network traffic you sent was created for you by the operating system. Using hping3 and Scapy, in this chapter, you’re finally going to have full control of data created. Although this technique is called packet crafting, in addition to crafting IP packets and ICMP packets, you’ll also be crafting Transmission Control Protocol (TCP) segments, User Datagram Protocol (UDP) datagrams, and more. Packet crafting, following the theme of tools and techniques in this chapter, is performed by both white