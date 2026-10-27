
6. ☑ **C.** The TCP wrappers feature in Linux allows you to control which clients can connect to which services.
   ☒ **A, B, and D** are incorrect. Virtualization is the hosting of multiple operating systems in virtual machines on one physical machine. Cloud computing is having a provider host your servers for you while you connect to them remotely across the Internet for administration of the systems. A host-based firewall controls communication to the system.

7. ☑ **A.** A host-based IDS analyzes log files, file attributes, and dynamic data such as connections and memory on the system to identify suspicious activity.
   ☒ **B, C, and D** are incorrect. Network traffic and packet analysis are performed by network-based intrusion detection software. The TCP wrappers feature does not look for suspicious traffic; it simply controls access to services.

## Protecting Against Malware

8. ☑ **B.** Windows Server Update Services (WSUS) is a service that allows you to download, review, approve, and deploy patches to groups of systems on the network.
   ☒ **A, C, and D** are incorrect. Virus protection and host-based firewalls do not deal with patching a system. Using Windows Update on every system is an administrative nightmare, so WSUS should be used.

9. ☑ **D.** It is critical after installing antivirus software that you ensure the virus definitions are up to date. Most virus protection software can schedule the updating of virus definitions.
   ☒ **A, B, and C** are incorrect. Host-based firewalls have nothing to do with maintaining the antivirus software, and you should not disable the automatic updates of virus definitions; you should *enable* them. You also should not disable real-time protection, because it scans files as they are accessed.

10. ☑ **C.** Having an e-mail address available on the Internet, whether on a web page or submitted through someone’s web site, always runs the risk that the e-mail address could be sent unsolicited messages.
    ☒ **A, B, and D** are incorrect. Spyware, viruses, and phishing are not concerns associated with supplying an e-mail address.

## Device Security and Data Security

11. ☑ **B and D.** You should ensure that you are locking the screen and encrypting data on the mobile device to protect it from unauthorized access.
    ☒ **A, C, and E** are incorrect. You should have a password enabled instead of disabled, and never disable emergency calling, so that if the phone is locked, you can still call 911. Bluetooth should be disabled if not used, but most people use Bluetooth for wireless headsets.

12. ☑ **B.** When the share permission conflicts with the NTFS permission, the most restrictive is applied—in this case, that is Read.
    ☒ **A, C, and D** are incorrect. These are not the most restrictive permissions in the scenario.

## Performance-Based Questions

13. The following command in Linux is used to set the permissions on a file called test.txt so that the owner has Read, Write, and Execute permissions, while all others only have Read: `chmod 744 test.txt`

14. The following is the matching of endpoint security terms and their description. On the real exam you may be asked to drag the term onto the matching description.

![](img/3e3af33e2c753e920c09ecd40b5a457a_362_0.png)

15. The following is the order of environments that a new application should be tested in. Again, on the real exam you may be asked to drag the terms into the boxes to represent the correct order to verify code.

![](img/3e3af33e2c753e920c09ecd40b5a457a_362_1.png)

## Chapter 8: Securing the Network Infrastructure

### CERTIFICATION OBJECTIVES

- 8.01 Understanding Firewalls
- 8.02 Using Intrusion Detection Systems
- 8.03 Network Design and Administration Principles
- 8.04 Securing Devices
- ✓ Two-Minute Drill
- Q&A Self Test

A big part of security within organizations today comes from securing the network infrastructure. This involves making sure that the network used to connect an employee’s computer to the servers is as secure as possible against threats from outside or inside the network.

![](img/3e3af33e2c753e920c09ecd40b5a457a_363_0.png)

The last few chapters have been focused on what you can do to secure individual systems, which is very important, but you also want to be sure that the network you connect those systems to is as secure as possible. In order to create a secure network infrastructure for those systems, you must take a layered security approach (also known as defense in depth), which involves protecting the network infrastructure with firewalls, but also ensuring you use intrusion detection systems to identify suspicious network traffic.

This chapter will introduce you to topics such as firewalls, intrusion detection systems, and network access control. You will be tested on these technologies on your Security+ certification exam, so be sure you are comfortable with the content of this chapter.

### CERTIFICATION OBJECTIVE 8.01: Understanding Firewalls

In this section you learn about firewalls, proxy servers, and other types of security devices used to create a secure network infrastructure. Firewalls and proxy servers are two of the most common devices found in network environments today because they are responsible for ensuring that unauthorized persons outside the network (such as on the Internet) cannot gain access to your network.

### Firewalls

Firewalls are designed to protect systems on one side of the firewall from systems on the other side by analyzing packets that reach the firewall and determining whether each packet is allowed to pass through. You will configure rules on the firewall that indicate to the firewall which traffic is to pass through and which is to be blocked.

> **Exam Watch** For the Security+ exam, know that firewalls are examples of protective controls, as they have rules configured to control what type of traffic can enter the network. This chapter also discusses the intrusion detection system (IDS), which in general is considered a detective control—unless it is an active IDS, in which case it is known as an intrusion prevention system (IPS), but more on that later!

For example, as a general rule you should configure the firewall to block all traffic, meaning that no traffic can pass through. Once you have configured the “default” rule of blocking all traffic, you can configure exceptions to the rule, allowing select traffic to pass through. For example, if you have a web server that you want to expose to the Internet, you would block all traffic except TCP port 80, the port on which web server traffic runs (as shown in Figure 8-1).

![](img/3e3af33e2c753e920c09ecd40b5a457a_365_0.png)

Before we go any further into the firewall discussion, I should stress that there are two major classes of firewalls. A *host-based firewall* is a piece of software you install on a system that is used to protect that one system. A *network-based firewall* is placed at the edge of the network and controls what traffic is allowed to enter and leave the network. This chapter focuses on network-based firewalls.

### Types of Firewalls

Firewalls have progressed over time, and for the Security+ certification exam, it is important to understand the different types of firewalls that have emerged over the years.

### Packet-Filtering Firewall (Stateless)

The first type of firewall is a packet-filtering firewall, also known as a *stateless firewall*. A packet-filtering firewall can block or allow traffic (known as filtering traffic) based on the source or destination IP address and the source or destination port number, as shown in Figure 8-2.

![](img/3e3af33e2c753e920c09ecd40b5a457a_365_1.png)

### Stateful Packet Inspection Firewall

Packet-filtering firewalls look like a great type of firewall at first, but they are not all that intelligent because it is easy for a hacker to spoof a packet so that it meets the rules of the firewall. For example, if you open port 80 on a packet-filtering firewall, any packets destined for port 80 will bypass the firewall.

Like packet-filtering firewalls, a stateful packet inspection firewall can filter traffic based on the source and destination IP address or port number, but can also look at the context of the conversation and determine if the packet is supposed to be received at that point in the conversation. If the firewall receives a packet in the correct context of the conversation and the packet follows one of the rules, it allows the packet into the network. Stateful packet inspection firewalls use rules to filter traffic as well, but they also are smart enough to know the context of the conversation.

An example of a stateful packet inspection firewall knowing about the context of a conversation is that if a hacker tries to send malicious commands to the firewall with a destination port of 80 and the hacker has not performed a three-way handshake first, the firewall says, “Nope, sorry, you are not allowed in because I don’t see that we have established a connection.” Stateful packet inspection firewalls know that before TCP communication can occur, there needs to be a three-way handshake.

> **Exam Watch** A stateful packet inspection firewall filters traffic by the layer-3 and layer-4 headers (like a packet-filtering firewall, also known as stateless), but in addition can filter traffic by knowing what packets are expected during certain phases of the conversation.

### Application-Layer Firewall

The next type of firewall, the application-layer firewall, implements features of both the packet-filtering firewall and the stateful packet inspection firewall, but also can filter traffic based on the payload data of the packet.

Not only does an application-layer firewall implement packet-filtering and stateful packet inspection features, but it also can inspect the application data in the packet, known as the payload. This means that an application-layer firewall can deny packets containing suspicious commands. For example, the application-layer firewall can check that the destination port is 80, but also ensure that there is no HTTP put command being executed. Or the firewall could ensure that data is downloaded (get) from an FTP server, but not uploaded (put) to the server (see Figure 8-4).
