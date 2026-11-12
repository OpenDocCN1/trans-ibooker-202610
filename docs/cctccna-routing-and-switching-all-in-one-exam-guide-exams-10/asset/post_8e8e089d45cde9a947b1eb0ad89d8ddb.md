
- NetFlow enables administrators to identify applications causing network congestion, diagnose slow performance, and verify that an application receives the appropriate amount of bandwidth based on its class of service (CoS).
- NetFlow benefits include network, application, and user monitoring; network planning; and accounting/billing.
- Minimally, a flow must contain a source IP address, a destination IP address, and an ingress interface. Traffic is considered in the same flow if the packets contain the same IP addressing, port numbers, and layer 3 protocol information.
- Sending flow information to a collector can be CPU-intensive.
- One concern with a collector is the number of devices and amount of flow information the collector will receive.
- Flow monitors are NetFlow components that enable you to globally define NetFlow parameters, such as the cache size and the number of unique flow records to collect, which are then applied to a respective interface.
- The **show ip cache flow** command displays the general NetFlow data captured by an IOS device.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully because there may be more than one correct answer. Choose the correct answer(s) for each question.

1. Which IOS command defines an NTP server?
   A. **ip ntp server**
   B. **ntp server**
   C. **ntp-server**
   D. **ip ntp-server**

2. Which IP addresses should be excluded from a DHCP server pool? (Choose two.)
   A. All the DHCP server’s IP addresses
   B. The subnet number
   C. The directed broadcast address
   D. DHCP client address

3. The ______ IOS command must be executed to enable the DHCP server and/or relay agent features if they were previously disabled.

4. Which of the following are components of SNMP? (Choose three.)
   A. Agent
   B. Manager
   C. NetFlow
   D. Syslog
   E. MIB

5. Enter the IOS command to enable logging to destinations other than the console: ______.

6. What are two of the common logging destinations used by administrators on Cisco devices? (Choose two.)
   A. TFTP server
   B. Terminal sessions
   C. Syslog servers
   D. RAM
   E. Flash

7. A NetFlow flow must minimally contain which items? (Choose two.)
   A. IP address
   B. Port numbers
   C. Ingress interface
   D. CoS

8. Version ______ is the most common implementation of NetFlow.

9. ______ translates one (and only one) IP address to another.
   A. NAT
   B. PAT
   C. PAR
   D. NAT and PAT

10. An ______ is a public IP address associated with an inside device.
    A. Inside global IP address
    B. Inside local IP address
    C. Outside global IP address
    D. Outside local IP address

11. Which command is used to define the local addresses that are statically translated to global addresses?
    A. ip nat inside source static
    B. ip nat inside
    C. ip nat inside source list
    D. ip nat pool

12. You have 30 internal machines that need to access the Internet: 192.168.1.32/27. You’ve been given six public IP addresses for this access: 199.1.1.41–199.1.1.46. Interface fastethernet0/0 is connected to the inside and serial1/0 to the ISP on your router. Enter the commands to perform PAT with this information.

13. When configuring the ip nat inside source command, which parameter must you specify to perform PAT?
    A. pat
    B. overload
    C. load
    D. port

14. When working with QoS, which DiffServ value is placed in the DSCP field by the IP phone for VoIP traffic by default?
    A. AF22
    B. ToS
    C. CS0
    D. EF

## Performance-based Questions

In the Cisco exams, performance-based questions require you to drag a box from the left side of the screen to its proper place on the right side of the screen to answer the questions. The following are some sample performance-based questions.

1. Using the following exhibit, write each characteristic listed on the left into the appropriate box on the right.

Resolves FQDNs to IP addresses
Used to automatically assign IP addresses to hosts
Uses UDP ports 67 and 68
Uses UDP port 53

DHCP

DNS

2. Using the following exhibit, place the DHCP phases in the correct order.

- DHCP Request
- DHCP Offer
- DHCP Acknowledge
- DHCP Discover

Phase 1
Phase 2
Phase 3
Phase 4

## Answers

1. **B.** Use the **ntp server** command to define an NTP server.

2. **B and C.** The network and directed broadcast addresses should be excluded as well as statically assigned IP addresses, including the default gateway address of the subnet/VLAN (the subnet number).

3. The **service dhcp** IOS command must be executed to enable the DHCP server and/or relay agent features if they were previously disabled.

4. **A, B, and E.** The agent, the NMS (manager), and MIBs are the three components of SNMP.

5. The **logging on** command enables logging to destinations other than the console.

6. **C and D.** Common logging destinations used by administrators are the logging buffer (RAM), the console terminal, and syslog servers.

7. A and C. Minimally, a flow must contain a source IP address, a destination IP address, and an ingress interface.

8. Version 5 is the most common implementation of NetFlow.

9. A. NAT translates only one IP address to another.

10. A. An inside global IP address is a public IP address assigned to an inside device.

11. A. The ip nat inside source static command configures static NAT translations.

12. Here are the commands to enter:

```
ip nat pool mypool 199.1.1.41 199.1.1.46
    netmask 255.255.255.248
    access-list 1 permit 192.168.1.32 0.0.0.31
    ip nat inside source list 1 pool mypool overload
    interface fastethernet0/0
      ip nat inside
    interface serial1/0
      ip nat outside
```

13. B. Use the overload parameter with the ip nat inside source command to set up PAT.

14. D. The DiffServ Expedited Forwarding (EF) value is placed in the DSCP field to indicate that the data needs low latency.

## Performance-based Answers

1. The following identifies the characteristics of DNS and DHCP.

DHCP
Used to automatically assign IP addresses to hosts
Uses UDP ports 67 and 68

DNS
Resolves FQDNs to IP addresses
Uses UDP port 53

2. The following is the correct order of the DHCP process.

Phase 1
DHCP Discover

Phase 2
DHCP Offer

Phase 3
DHCP Request

Phase 4
DHCP Acknowledge

## IPv6

In this chapter, you will

- Learn about the need for IPv6
- Learn about IPv6 enhancements and addressing
- Learn how to configure IPv6 on an interface
- Learn how to configure routing with IPv6

This chapter introduces you to the next generation of TCP/IP: IP version 6 (IPv6). Because of the many deficiencies found in IPv4, as well as the poor scalability for hierarchical addressing in IPv4, IPv6 was developed to meet the rapidly growing needs of small companies, corporations, and the explosive growth of the Internet, especially in emerging markets such as China and India. In this chapter you’ll discover why IPv6 is necessary, the different kinds of IPv6 addresses, routing with IPv6, and how to configure a basic IPv6 setup. The CCNP certification goes into much more depth than what you’ll find in this chapter.

### Necessity of IPv6

An IPv4 address is 32 bits in length; that means there are 2³² actual IP addresses, which equals 4.3 billion addresses. Not all of these are usable, however: only 3.7 billion of these are actually usable. Many addresses are reserved, such as research (239–254), broadcast (255), multicast (224–239), private (10, 172.16, and 192.168), and loopback addresses (127). And, of course, many of the usable addresses are already assigned, leaving very, very few addresses for new growth.

Unlike 32-bit IPv4 addresses, IPv6 uses a 128-bit address. This allows for 3.4 × 10³⁸ addresses, which is enough for many IP addresses for each person on Earth, and probably on multiple planets.

### Growth Issues

During the early-to-mid-1990s, concern began to grow about the diminishing number of IPv4 addresses. The temporary solution to this problem was to set aside an address space, called *private addresses*, which anyone could use in a public network. Recall from Chapter 4 that these addresses are defined in IETF RFC 1918: 10.0.0.0/8, 172.16.0.0–172.31.255.255, and 192.168.0.0–192.168.255.255. To access a public network, address translation is used to translate the private addressing information to a public address, commonly with static Network Address Translation (NAT) translations for internal services and dynamic overloading (in particular, Port Address Translation, or PAT) for user connection.

Many changes in the marketplace, however, are quickly reaching the point where address translation won’t be enough: there won’t be any public addresses left to translate to. Here are some valid reasons why companies are beginning to migrate to an IPv6 environment:

- Currently more than 3 billion people are connected to the Internet, and this is exponentially increasing based on fast-emerging technical markets such as China and India.
- More than 2 billion smart phones, tablets, and similar devices offer common data services such as e-mail and web browsing, and this number is expected to grow as more and more businesses implement mobile applications.
- More data services are being offered on consumer products, such as automobiles, household appliances, and industrial devices, and this number is expected to grow into the billions.

As you can see, it’s not a matter of *if* public addresses are going to be depleted, and not even *when* it’s going to happen, but *how soon* in the near future this is going to happen. Because IPv4 is only a 32-bit address, and because the designers did not foresee the growth of the Internet, we have
