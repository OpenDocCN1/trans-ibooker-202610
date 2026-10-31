
6. What is the standard recommendation and default setting for Maximum Client Lead Time when configuring Load balanced DHCP failover?
**Answer:** One hour

7. Which Windows Server roles can IPAM tap into?
**Answer:** Active Directory, DNS, DHCP, and NPS! We didn't talk about NPS in this chapter because we have not yet covered any NPS material, but it is a fourth role that can report data into IPAM.

## Chapter 5: Group Policy

1. Are screensaver settings computer or user configuration?
**Answer:** User configuration

2. Do domain-level or OU-level links process first?
**Answer:** Domain-level links process before OU-level links, meaning that OU-level links will overwrite domain-level links when in conflict

3. What is the special GPO setting that forces user settings to apply to any user on a given computer?
**Answer:** Group Policy loopback processing

4. What type of GPO filtering do you configure inside the GPO itself, such as with a mapped network drives policy?
**Answer:** Item-level targeting

5. True or False — It is possible for a user to override a Group Policy preference.
**Answer:** True

6. What is the default timer between Group Policy background refresh cycles?
**Answer:** 90 minutes

## Chapter 6: Certificates in Windows Server 2019

1. What is the name of the role inside Windows Server 2019 that allows you to issue certificates from your server?
**Answer:** Certification Authority

2. What kind of CA server is typically installed first in a domain environment?
**Answer:** Enterprise Root CA

3. Should you install the Certification Authority role onto a domain controller?
**Answer:** No, this is not a recommended scenario

4. After creating a new certificate template, what next step needs to be taken before you can issue certificates to your computers or users from that new template?
**Answer:** The new certificate template must be published

5. What is the general name of the GPO setting that forces certificates to be issued without manual intervention by an administrator?
**Answer:** Certificate Auto-enrollment

6. An SSL certificate will only be able to validate traffic properly if it shares ______ key information with the webserver.
**Answer:** Private key

7. What is the primary piece of information that a public certification authority needs in order to issue you a new SSL certificate (hint: you generate this from your webserver)?
**Answer:** Certificate Signing Request (CSR)

## Chapter 7: Networking with Windows Server 2019

1. How many bits in length is an IPv6 address?
**Answer:** 128 bits.

2. Re-write the following IPv6 address in condensed form: 2001:ABCD:0001:0002:0000:0000:0000:0001
**Answer:** 2001:ABCD:1:2::1

3. What is the name of the command that is similar to trace route, but displays the local NIC that traffic is flowing out of?
**Answer:** PATHPING

4. True or False—On a server with multiple NICs, you can input a default gateway address onto each of those NICs.
**Answer:** False. Doing so will cause routing issues. You should only ever have one Default Gateway address on a system, no matter how many NICs it has.

5. What is the PowerShell cmdlet that can be used to create new routes on a Windows Server?
**Answer:** New-NetRoute

6. Which Windows Server operating systems can be used with an Azure Network Adapter in order to connect them straight to Azure virtual networks?
**Answer:** Windows Server 2019, 2016, and 2012 R2

## Chapter 8: Remote Access

1. What does AOVPN stand for?
**Answer:** Always On VPN

2. What are the two primary protocols used for connecting AOVPN clients?
**Answer:** IKEv2 and SSTP

3. In which version of Windows 10 was AOVPN released?
**Answer:** Windows 10 1607

4. In what special instance would an AOVPN client be required to be joined to your domain?
**Answer:** When you want to utilize the AOVPN Device Tunnel

5. Does DirectAccess require your corporate internal network to be running IPv6?
**Answer:** No, your internal network can be completely IPv4

6. What is the name of the internal website that DirectAccess clients check in with in order to determine when they are inside the corporate network?
**Answer:** Network Location Server (NLS)

7. What role does a Web Application Proxy server hold in a federation environment?
**Answer:** WAP can be implemented as an ADFS proxy

## Chapter 9: Hardening and Security

1. What is the name of the anti-malware product built into Windows Server 2019?
**Answer:** Windows Defender Antivirus

2. When a domain-joined computer is sitting inside the corporate LAN, which Windows Defender Firewall profile should be active?
**Answer:** The Domain profile

3. Other than the Domain profile, what are the other two possible firewall profiles inside Windows Defender Firewall?
**Answer:** Public and Private

4. When creating a firewall rule to allow IPv4 ping replies, what protocol type must you specify inside your inbound rule?
**Answer:** ICMPv4

5. What is the easiest way to push standardized Windows Defender Firewall rules to your entire workforce?
**Answer:** Group Policy

6. A virtual machine whose virtual hard disk file is encrypted is called a...?
**Answer:** Shielded VM

7. What is the name of the Microsoft technology that parses domain controller information in order to identify pass-the-hash and pass-the-ticket attacks?
**Answer:** Advanced Threat Analytics

## Chapter 10: Server Core

1. True or False—Server Core is the default installation option for Windows Server 2019.
**Answer:** True

2. True or False—You can utilize PowerShell to change a Server 2019 instance from *Server Core* mode to *Desktop Experience* mode.
**Answer:** False, switching back and forth is not possible in Windows Server 2019

3. When sitting at the console of a freshly booted Windows Server 2019 Server Core instance, what application do you see on the screen?
**Answer:** Command Prompt

4. What cmdlet can be used to view the current networking configuration on a Server Core instance?
**Answer:** Get-NetIPConfiguration

5. Which PowerShell cmdlet can be used to configure the hostname of a Server Core instance?
**Answer:** Rename-Computer

6. Name some of the management tools that can be used to remotely interface with a Server Core instance.
**Answer:** PowerShell, Server Manager, RSAT, and Windows Admin Center

7. What is the name of the utility built into Server Core that can be launched to provide quick task links for configuring IP addresses, hostnames, and domain membership?
**Answer:** Sconfig.exe

## Chapter 11: PowerShell

1. What is the fastest way to get from Command Prompt into PowerShell?
**Answer:** Simply type the word powershell and press Enter

2. What is the cmdlet that will display all available PowerShell cmdlets?
**Answer:** Get-Command

3. What PowerShell cmdlet can be used to connect your PowerShell prompt to a remote computer?
**Answer:** Enter-PSSession

4. What file extension does a PowerShell scripting file have?
**Answer:** .PS1

5. To which setting is the DEP configured on a fresh Windows Server 2019 instance?
**Answer:** RemoteSigned

6. What key on your keyboard can be used to auto-populate the remainder of a cmdlet or filename when working in a PowerShell prompt?
**Answer:** Tab

7. Which service must be running on a system before it can be connected to by a remote PowerShell connection?
**Answer:** The WinRM service

## Chapter 12: Redundancy in Windows Server 2019

1. Which technology is more appropriate for making web server traffic redundant – Network Load Balancing or failover clustering?
**Answer:** Network Load Balancing

2. In Network Load Balancing, what do the acronyms DIP and VIP stand for?
**Answer:** Dedicated IP address and virtual IP address

3. What are the three NLB modes?
**Answer:** Unicast, Multicast, and Multicast IGMP

4. In Windows Server 2019, is Network Load Balancing a role or a feature?
**Answer:** NLB is a feature

5. What roles are most often used with failover clustering?
**Answer:** Hyper-V and file services

6. What type of small device can now be used as a cluster quorum witness (this is brand new as of Server 2019)?
**Answer:** A USB memory stick

7. True or False—Storage Spaces Direct requires the use of SSD hard drives.
**Answer:** False, you may use any type of hard drive with S2D

## Chapter 13: Containers and Nano Server

1. A Windows Server container can run a base OS that is one of two different types: what are they?
**Answer:** Server Core and Nano Server

2. Compared to a Windows Server container, what type of container provides even greater levels of isolation?
**Answer:** Hyper-V container

3. True or False—In Windows Server 2016, you could run both Windows and Linux containers on the same Windows Server host platform.
**Answer:** False, the ability to run Windows and Linux containers is new as of Windows Server 2019
