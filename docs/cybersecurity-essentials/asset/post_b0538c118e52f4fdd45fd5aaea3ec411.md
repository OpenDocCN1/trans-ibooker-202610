
- Network Zone Rule: Identifies software through a specified zone in Internet Explorer.
- Hash Rule: Identifies programs based on their initial hash output.
- Certificate Rule: Identifies software based on its certificate signing.

# Lab Questions

1. What is the difference between whitelisting and blacklisting?
2. List the four types of rules associated with software restriction policies.
3. What are the three security levels and what is the default?

# Lab Question Answers

1. What is the difference between whitelisting and blacklisting?
   Whitelisting is the practice of assigning access to the known good and denying all others, while blacklisting involves allowing everyone except those on the known bad list.
2. List the four types of rules associated with software restriction policies.
   Path Rule, Network Zone Rule, Hash Rule, and Certificate Rule
3. What are the three security levels and what is the default?
   The three security levels are Disallowed, Basic User, and Unrestricted. The default security level is Unrestricted.

# Perimeter Security: Review Questions and Hands-On Exercises

*Review the summary points* before proceeding to the “Review Questions” and “Exam Questions” sections to make sure you are comfortable with each point. After completing the review, answer the review questions that follow to verify your knowledge of the material covered in this chapter.

## Summary Points

- There are two kinds of Internet Protocol traffic: Transmission Control Protocol (TCP) and User Datagram Protocol (UDP). When a TCP connection is established, data can be sent bidirectionally. This connection is highly reliable and features error checking but adds some bulk to the transmission. UDP is a much simpler, connectionless protocol where multiple messages are sent as packets in data chunks.
- *Unicast messaging* is the sending of messages to a single network address. *Broadcast messaging* refers to sending messages to all possible destinations. *Multicast messaging* sends messages using special address assignments to a specific group of addresses.
- In addition to the network address, many protocols may also specify a *port* or a port number. A protocol or application may respond to specific ports in different ways.
- Understanding ports is critical in managing network security. The easiest way to minimize unwanted scans or restrict access is by using specific ports and *port blocking*.
- *Routing* is simply the process of selecting the best pathway for transmitting data over a network or between networks.
- *Routers* deal with IP addresses mostly, but they can also deal with MAC addresses, which can be another way of restricting access.
- *A domain* is a unique realm assigned by an agent known as a *registrar* authorized by the Internet Corporation for Assigned Names and Numbers (ICANN).
- Most companies and residential users subscribe to Internet access services from an Internet Service Provider (ISP). This service is typically contracted for a specific service level or potential transfer speed and at least one IP address. However, the service may also include email, web hosting, some security services, or even a larger block of IP addresses.
- Many ISPs will *proxy* your web connections through caching servers or caching devices to minimize bandwidth utilization.
- Some ISPs will also block certain ports in a misguided attempt to minimize the impact of malware, viruses, and worms.
- The Internet Engineering Task Force (IETF) is the main standards organization for the Internet.
- A number of organizations are involved in maintaining network vulnerability resources not dissimilar from virus and malware resources. A variety of commercial entities have grown to serve this vital need, but many publicly available resources and standards are available.
- The real shift in security awareness came when the first computer viruses started making the rounds. With this shift, a single user could run a piece of malicious code and impact an entire company network and even be complicit in the furthering of that virus’s spread by unknowingly sharing that virus with every one of their contacts.
- Network hardware is the integral component in Internet access and can usually serve a dual purpose in providing a certain amount of network security.
- *Gateway* devices often provide good security capabilities and even out-of-the-box, unconfigured, they will usually provide “good enough” security for the average residential or small office user.
- Every standalone computer or local area network that connects to the Internet will connect through some type of gateway device. This gateway device is often a router but may also be a switch that features some protocol conversion (or translator) to provide Ethernet or wireless clients access to the technology the ISP provides.
- The router may also support a DMZ that provides relatively free access from external networks. Often, this is where web servers or mail servers are located so that offsite users can have easy access, while you can still control the content and hardware.
- A *proxy server* allows a client to make an indirect network connection. The client connects to the proxy server, with or without any conscious authentication, and makes a request for a resource from a different server. Then, the proxy server will handle the request by either returning the requested resource from a cache or by forwarding the request to the other server.
- Probably the most common use of this concept is with caching *web proxies* where a local server will cache web resources and then expire them intelligently.
- Web proxies help conserve Internet bandwidth and speed up connections, but they can also be used to filter content or even reformat that content.
- A *transparent proxy* makes the original IP address available but only in the HTTP headers. A transparent proxy is typically not something the user is even aware they are using.
- Malicious proxy servers have been used for various purposes including malware delivery, ad injection, and for just simple information gathering. Although the proxy server may not pass on your IP address, it will still know your IP address and could use this information somehow as well.
- It is common to put a cluster of web servers behind a *reverse proxy server* that will handle public requests for web resources and then forward them to one or more servers. To the requestor, this process appears as if those web servers are being accessed directly.
- The most common first line of defense in a network connected to the Internet is a *firewall*.
- A good firewall will provide *packet filtering* using defined rules to reject or accept both incoming and outgoing packets. Firewall rules are critical to security on most networks.
- The firewall is generally the place to start when implementing or evaluating Internet security. Most administrators configure the firewall features they need and then try to make them secure. However, security really needs to be the first consideration.
- Increasingly, routers and switches are being integrated into *smart switches* or even coupled with a traditional operating system to create devices referred to as *smart network appliances*.
- Security appliances are also known as *Unified Threat Management (UTM) devices* featuring gateway, antivirus, firewall, intrusion prevention and detection, and possibly more in a single product.
- *Data security* is deployed to prevent data misdirection through encryption or live capture. It is also implemented to prevent scanning of incoming packets or files, as well as monitoring and scanning data on a computer after it has been accepted.
- *Encryption* is a simple way to make sure data isn’t intercepted by a third party and used inappropriately. It is rapidly becoming an almost required step in securing any network.
- Security at all levels always involves some type of authentication process.
- Users can be authenticated by asking them to provide a username and password, by examining their MAC address, or by their network address if they are assigned a static IP address.
- After a user is authenticated, we generally want to determine their authorization, which essentially involves the resources that the authenticated user has permission to access and what actions they can perform.
- The most well-known authentication method is certainly password authentication, which is a *single-factor authentication* and represents the lowest level of security available.
- *Two-factor authentication* involves asking for another authentication component—ideally one that is something physical or in the user’s possession.
