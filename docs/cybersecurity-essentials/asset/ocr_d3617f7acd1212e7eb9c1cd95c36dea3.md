# Software-Based Local Firewalls

Computers connected directly to the Internet are vulnerable to attacks from outsiders. One way to protect standalone computers from outside attacks is to install a local firewall. A firewall is a device or a program that is placed between a local device and an outside network such as the Internet.

Local software-based firewalls can be installed on an individual computer to protect it from malicious activities introduced through the Internet connection. In some cases, the operating system provides a built-in firewall option that can be used to protect the local computer.

# A Word About Firewalls

> Local software firewalls are designed to provide protection from outside attacks by preventing unwanted connections from Internet devices. Software-based firewall services are designed to protect individual computers that are directly connected to the Internet through dial-up, LAN, or high-speed Internet connections.

The firewall inspects all traffic going to and coming from the outside connection and can be configured to control traffic flow between the Internet and the local device based on desirable properties, as illustrated in Figure 9.1. Firewalls are configured so they will only pass data to and from designated IP addresses and TCP/UDP ports.

Normally, firewall filters are configured around services recognized by the TCP and UDP networking protocols. These protocols use port numbers to identify specific processes such as HTTP or FTP and are used to refer incoming packets to a software application that will process them. Many of the port numbers are standardized and are referred to as well-known ports. Similarly, their associated applications are called well-known services.

Table 9.1 lists several well-known port numbers and their associated services. The Internet Assigned Numbers Authority (IANA) has assigned standard port numbers ranging from 0 to 1023 to specific services. Port numbers from 1024 through 49151 are called registered ports and are used in vendor applications. Ports 49152 through 65535 are dynamic, or ephemeral, ports and are used by computer applications to communicate with other applications.