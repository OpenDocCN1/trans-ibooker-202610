# Secure Protocols and Their Use Cases

In this section, we are going to look in detail at the various *secure protocols* and their use cases. The Security+ exam wants you to select the correct secure protocol for a given scenario to make your environment secure.

As most protocols use TCP ports, I will only mention the UDP ports, and therefore you can assume that if something is not labeled UDP, it is TCP. We will look at the secure protocols and their use cases:

| Protocol | UDP | Port | Use Cases |
|---|---|---|---|
| Secure Shell (SSH) |  | 22 | Secure remote access |
| Secure Copy Protocol (SCP) |  | 22 | Secure copy to UNIX/LINUX |
| SSH File Transfer Protocol (SFTP) |  | 22 | Secure FTP download |
| DNSSEC | TCP/UDP | 53 | Secure DNS Traffic |
| Kerberos |  | 88 | Secure authentication |
| Simple Network Management Protocol Version 3 (SNMP v3) | UDP | 162 | Secure status and reports of network devices |
| Lightweight Directory Access Protocol Secure (LDAPS) |  | 636 | Manages directory service information securely |
| Hypertext Transport Protocol Secure (HTTPS) |  | 443 | Secure Web Browser |
| Transport Layer Security (TLS)/Secure Socket Layer (SSL) |  | 443 | Secure Data in Transit |
| Internet Protocol Security (IPSec) | UDP | 500 | Secure session for VPN or between two hosts |
| Secure Simple Mail Transfer Protocol (SMTPS) |  | 587 | Secure SMTP |
| Secure Internet Message Access Protocol (IMAP4) |  | 993 | Secure IMAP4 |
| Secure Post Office Protocol 3 |  | 995 | Secure POP3 |
| Secure/Multipurpose Internet Mail Extensions (S/MIME) |  | 993 | Encrypt or digitally sign email |
| File Transfer Protocol Secure (FTPS) |  | 989/990 | Download large files securely |
| Remote Desktop Protocol (RDP) |  | 3389 | Microsoft remote access |
| Session Initiated Protocol (SIP) |  | 5060/61 | Connects internet-based calls |
| Secure Real Time Protocol (SRTP) |  | 5061 | Secure voice traffic |

Let's look at each of them:

+   - **SSH:** This was invented to replace Telnet so that commands could be run securely; it is commonly used when you want remote access to network devices. It can be used as a command-line tool or in a **Graphical User Interface (GUI)**, but it is not browser-based.
+   - **SCP:** This is used to transfer files securely between hosts in a Linux environment.
+   - **SFTP:** This allows us to encrypt authentication and download files securely so that they cannot be tampered with. It is secure as it is packaged with SSH.
+   - **DNSSEC:** To prevent someone from gaining access to DNS records, DNSSEC was introduced to protect DNS traffic. Each DNS record is digitally signed, creating an RRSIG record to protect against attacks, guaranteeing you that they are valid and their integrity has been maintained. This prevents DNS poisoning.