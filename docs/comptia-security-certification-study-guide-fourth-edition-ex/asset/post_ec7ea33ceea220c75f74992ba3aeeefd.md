
- Network devices such as switches should be hardened as well. Be sure to update the firmware on the routers and switches. In highly secure environments, disable unused ports on the switch and configure port security on used ports, which is a feature that associates a particular MAC address with a port.
- Ensure you are using secure management protocols on network devices such as SSH instead of Telnet.
- The 802.1X standard is known as a port access control standard and controls who has access to a wired or wireless network by using a central authentication server such as RADIUS.
- Of the many software tools you can use to help with system hardening, two common methods are security templates and group policies. You can use a security template to help create your security baseline, and group policies are a method used to apply restrictions on the system.
- Look to common server hardening techniques, such as only allowing zone transfers to a specific host for your DNS servers, placing public servers such as DNS and web servers in the DMZ, and disabling unused features on the web server.

## TWO-MINUTE DRILL

### Understanding Operating System Hardening

- The goal with operating system hardening is to reduce the attack surface of a system by removing unnecessary software and disabling unneeded services.
- Ensure you disable unwanted user accounts and rename default account names.
- Plan your patch management strategy. It is important that patches are tested before being deployed and that you are patching systems on a regular basis.

### System Hardening Procedures

- Start your hardening procedures by hardening your network devices such as routers and switches. The first step is to update the firmware on the device.
- Disable any unused ports on the network switch so that unauthorized persons cannot connect to an available port.
- Implement port security on the switch to limit which system can connect to a particular port on the switch. Also, use MAC filtering to filter which systems can send data to other systems on the network.
- 802.1X is a network access control standard that allows you to control who can connect to the network by using a central authentication server such as a RADIUS server.
- Group policies are a method to configure all the systems from one central location in an Active Directory environment. You can configure group membership and services running, and you can control the desktop appearance through group policies.
- Security templates are a great way to create a security baseline. A security template is a text file that has policy settings in it that can then be imported into systems or Active Directory.
- A security baseline is a standard configuration that has been approved as being the configuration needed to place a system in a secure state.
- To establish and maintain a good security posture, you must first apply a security baseline and then continuously monitor the state of the system with tools such as vulnerability scanners. The final step is to fix any errors in the configuration of the system as a result of the monitoring.

### Server Hardening Best Practices

- All servers should be hardened and patched.
- Web servers should be hardened, but also ensure that the web server is placed in a DMZ (if it’s an Internet server) and is protected by the firewall. Also, disable web server features that are not going to be used.
- To secure your DNS servers, place them in the DMZ and have port UDP 53 open on the firewall to allow the DNS queries to reach the DNS server. Be sure to limit zone transfers on the DNS server to only your secondary DNS servers, and block TCP 53 at the firewall if you are not doing zone transfers to secondary DNS servers on the Internet.
- In highly secure environments, configure DHCP reservations, which associate each IP address with a specific MAC address. This will ensure that an unauthorized system on the network does not receive an IP address from the DHCP server.
- SMTP servers should have SMTP relaying disabled, whereas the FTP server should implement authentication to control who has access to the files on the FTP server.

## SELF TEST

The following questions will help you measure your understanding of the material presented in this chapter. As indicated, some questions may have more than one correct answer, so be sure to read all the answer choices carefully.

### Understanding Operating System Hardening

- 1. Which of the following actions is performed during system hardening?
  A. MAC filtering
  B. Disabling unnecessary services
  C. Enabling port security
  D. Configuring 802.1X authentication
- 2. Your manager has read about the need to uninstall unnecessary software and disable unnecessary services from a system. What is the purpose of performing these hardening techniques?
  A. To close ports on the system
  B. To assess vulnerability
  C. Fuzzing
  D. To reduce the attack surface
- 3. A software vendor has found out about a critical vulnerability within its software product that causes a severe security risk to the system. The software vendor will ship which type of remedy that should be applied to systems immediately?
  A. Patch
  B. Service pack
  C. Hotfix
  D. Update
- 4. You are planning a security assessment strategy for all systems and mobile devices used within the organization. When assessing mobile devices such as phones, what should you look for?
  A. Ensure the phone is password protected.
  B. Ensure no texting software is installed.
  C. Ensure the phone is not running a mobile OS.
  D. Ensure the phone is not configured for e-mail.

### System Hardening Procedures

- 5. Which of the following security technologies involves controlling access to a wired or wireless network using a central authentication server such as RADIUS?
  A. Port security
  B. 802.1X
  C. MAC filtering
  D. Firewall
- 6. What feature of a network switch allows you to control which system can be physically connected to a specific network port by its MAC address?
  A. 802.1X
  B. MAC filtering
  C. Firewall
  D. Port security
- 7. A new network administrator in the office has been reading about the company requirement that all systems have the initial security baseline applied. She is looking at a listing of 50 different policy settings that need to be applied and is wondering if there is an easy way to deploy the settings. What should she do?
  A. Configure the settings in the local security policies.
  B. Import a registry file.
  C. Use a security template.
  D. Build a macro.
- 8. What type of reporting mechanism should a system or application use to notify the administrator of an event that requires immediate attention?
  A. Alert
  B. Trend
  C. Log
  D. Alarm

### Server Hardening Best Practices

- 9. Your company has a primary DNS server at its head office and a secondary DNS server at two other offices around the world. What should you do to secure the DNS data?
  A. Allow zone transfers only to the head office DNS server.
  B. Limit zone transfers to the IP addresses of the secondary servers.
  C. Block TCP port 53 on the firewall in the head office.
  D. Block UDP port 53 on the firewall in the head office.
- 10. Which of the following identifies a security concern with SMTP servers?
  A. Relaying of messages
  B. Zone transfers
  C. E-mail spoofing
  D. Invalid address assignment
- 11. Your manager would like to implement additional security measures on the DHCP server. What actions would you recommend? (Choose two.)
  A. Disable zone transfers.
  B. Modify the scope to include only one address for each host on the network.
  C. Deactivate the scope.
  D. Configure an address reservation for each of the addresses in the DHCP scope.
  E. Disable DHCP.

## Performance-Based Question

12. Using the exhibit, match the mitigation technique on the left side to the term or description that best matches on the right side.

| Mitigation technique | Term or description |
|---|---|
| Disable unused interfaces | Used to authenticate someone before they can connect to a wired or wireless network |
| Reduce attack surface | Limit what system can connect to an interface on a switch |
| Port security | Used to apply a security baseline |
| Limit systems that can connect to wireless network | MAC filtering |
| 802.1X | Unhealthy systems are connected to a segment to obtain missing patches |
| Security templates | Protect DNS data |
| Remediation | Patch systems + firewalls + network segmentation + malware protection |
| Limit zone transfers | Protect against clear text passwords traveling the network |
| Security layers | A network switch hardening technique |
| Implement SSL/TLS | Prevent former employees from logging into the network |
| Disable unnecessary accounts | Limit vulnerabilities on a system |
