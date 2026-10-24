
## Chapter 8 · Protecting the Inner Perimeter

**4.** If a User tries to access a file that has an Allow – Read & Execute permission, and a Deny – Read & Execute permission assigned to them from different sources, will they be able to access the file?

**Answer:** No. In most cases, a Deny permission overrides the Allow permission. The Allow permission granted is voided, and the User will not have access.

**5.** If you remove all permissions, Allow and Deny, from a specific user will they be able to access the file or folder?

**Answer:** Depending on the group or groups they are associated with, yes. Users are generally added to the Users groups, and as such they are given the default Allow – Read & Execute, Read, and Write on most files.

**6.** As a basic user, can you change the permissions on any file or object?

**Answer:** No. A user without administrative properties cannot change any file or folders permissions. A user is allowed to change only the permissions of files to which they are considered the owner.

## Protecting Remote Access

*As you learned earlier* in Part II, computers do not have to be connected to other computers to be at risk. Although many computers might not be part of a corporate network, almost all modern personal computers at least occasionally get attached to the largest data network in the world: the Internet. As such, these standalone computing and control devices are exposed to corruption and exploitation from remote sources.

- Analyze and Differentiate Between Anti-Virus/Anti-Malware Products.
- Secure the Web Browser of a Standalone Computing Device.
- Configure and Test a Local Firewall Installation.
- Explain the Importance of Application Security.
- Audit Local Operating System Services and Events.
- Establish a Local Security Policy on a Standalone Host Device.
- Describe the Importance of Conducting Local Updates and Patch Maintenance Activities.

### Protecting Local Computing Devices

There are nine basic steps for protecting local computing devices from Internet-based threats:

- 1. Use a secure connection.
- 2. Establish and configure a firewall to control the flow of information between the computing device and the Internet.
- 3. Install and use anti-malware on the local computer.
- 4. Remove unnecessary software from the computer.
- 5. Disable any nonessential services running on the computer.
- 6. Disable unnecessary OS default features.
- 7. Secure the web browser.
- 8. Apply operating system and application software updates and patches.
- 9. Require strong passwords.

### Using a Secure Connection

One of first steps in securing an Internet connection is to implement security options on the local router. If a particular connection does not include a router, you may want to consider installing one, as well-configured routers offer one of the best initial lines of defense. Routers and other network connectivity devices are covered in detail in Part III.

Some basic items to consider when configuring a router’s security features include:

- Change the login username and default password. (The defaults are published in the user’s setup instructions and, therefore, are known to everyone.)
- For wireless network connections, change the default SSID setting.
- Configure the wireless network with the highest level of encryption available—preferably WPA2-AES for maximum data confidentiality.
- Identify trusted wireless connections by conducting MAC address filtering.

### Establishing and Using a Firewall

You should always establish and configure a firewall to control the flow of information between the computing device and the Internet. For standalone or local computers, this can be addressed through the local software firewall available with the operating system and/or through the protective router. Local firewall configurations are discussed later in this chapter.

### Installing and Using Anti-Malware Software

Anti-malware software can be installed using an inclusive malware-prevention product or by combining different types of specific prevention programs, such as antivirus and antispyware products from different vendors. Different malware types and prevention methods are discussed in detail at the end of this chapter.

### Removing Unnecessary Software

Keeping unused software products on a computer provides additional avenues of possible attack and exploitation. If you don’t know what a suspected software program does, research it and get rid of it if it is not important to the operation of that system.

### Disabling Nonessential Services

Some viruses are designed to exploit nonessential services in order to migrate from device to device. In particular, disengage any file-sharing or device-sharing services that are running, unless they are somehow required for proper operation of the system (this is almost never the case in nonconsumer usage).

Photo-sharing and music-sharing services should always be disabled, while file and printer should be disabled unless required to pass information from one device to another to perform work tasks. (This would pertain to a networked computer, which is discussed in detail in Part IV.)

### Disabling Unnecessary OS Default Features

As mentioned earlier, Autorun is a highly exploitable feature of the Microsoft line of operating systems. When this feature is enabled, the OS will detect the presence of the removable media and execute its contents. If the SD card or CD/DVD contains a virus, it will automatically be executed and infect the host computer.

### Securing the Web Browser

The web-browsing class of application software has attracted an increasing number of attacks. Initial browser configurations may not offer much in the way of security. As such, it is usually necessary to configure a new browser’s security options to safeguard the system from attack through this portal. Steps for securing different web browsers are discussed later in this chapter.

## Software-Based Local Firewalls

Computers connected directly to the Internet are vulnerable to attacks from outsiders. One way to protect standalone computers from outside attacks is to install a local firewall. A firewall is a device or a program that is placed between a local device and an outside network such as the Internet.

Local software-based firewalls can be installed on an individual computer to protect it from malicious activities introduced through the Internet connection. In some cases, the operating system provides a built-in firewall option that can be used to protect the local computer.

### A Word About Firewalls

> Local software firewalls are designed to provide protection from outside attacks by preventing unwanted connections from Internet devices. Software-based firewall services are designed to protect individual computers that are directly connected to the Internet through dial-up, LAN, or high-speed Internet connections.

The firewall inspects all traffic going to and coming from the outside connection and can be configured to control traffic flow between the Internet and the local device based on desirable properties, as illustrated in Figure 9.1. Firewalls are configured so they will only pass data to and from designated IP addresses and TCP/UDP ports.

Normally, firewall filters are configured around services recognized by the TCP and UDP networking protocols. These protocols use port numbers to identify specific processes such as HTTP or FTP and are used to refer incoming packets to a software application that will process them. Many of the port numbers are standardized and are referred to as well-known ports. Similarly, their associated applications are called well-known services.

Table 9.1 lists several well-known port numbers and their associated services. The Internet Assigned Numbers Authority (IANA) has assigned standard port numbers ranging from 0 to 1023 to specific services. Port numbers from 1024 through 49151 are called registered ports and are used in vendor applications. Ports 49152 through 65535 are dynamic, or ephemeral, ports and are used by computer applications to communicate with other applications.
