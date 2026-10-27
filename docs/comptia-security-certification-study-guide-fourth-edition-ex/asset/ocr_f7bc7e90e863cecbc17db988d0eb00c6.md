A scenario where IPSec could be used is if you need to encrypt all network traffic, or maybe all network traffic between a group of systems.

exam watch For the exam, remember that the AH provides data integrity and authentication but not data confidentiality (encryption). ESP provides data integrity, authentication, and data confidentiality within IPSec.

+   - **Secure Shell (SSH)** SSH is designed to be a secure replacement to Telnet and provides authentication and encryption services. SSH can be used to create an encrypted channel so that communication through the channel is encrypted. A common scenario where SSH is used is for remote access to switches and routers.
+   - **FTP Secure (FTPS)** FTP Secure, also known as FTP-SSL, should be used as a secure replacement to FTP because it encrypts FTP communication using either the SSL or TLS security protocol.
+   - **SSH FTP (SFTP)** SFTP is an extension on SSH that allows secure transfer and management of files through an SSH channel. Note that this is not the same as FTPS, which uses SSL or TLS to secure FTP traffic.
+   - **Simple Network Management Protocol v3 (SNMPv3)** This version should be used over earlier versions of SNMP because it provides authentication and communication encryption features. SNMP is used for monitoring and managing devices on the network, such as routers and switches. You should review those devices and verify they are using SNMPv3 instead of version 1 or version 2.
+   - **Secure Post Office Protocol/Internet Message Access Protocol (Secure POP/IMAP)** When you have an e-mail client on the Internet that will connect to your company e-mail server using POP or IMAP, which send data in an unencrypted format (including username and password), you should look to use Secure POP or Secure IMAP to encrypt the communication. Both Secure POP and Secure IMAP use TLS or SSL to secure e-mail communication.
+   - **Secure Copy Protocol (SCP)** Like SFTP, SCP runs on top of an SSH channel in order to encrypt the communication used to transfer a file.
+   - **Wireless** You should encrypt wireless communication with WPA2.

exam watch For the exam, be sure to know the protocols used to encrypt different types of traffic. Remember that TLS is replacing SSL, SSH should be used instead of Telnet, and HTTPS should be used instead of HTTP if security is a concern.