
However, at the tightest security levels, that firewall configuration might enrage network users as they typically want zero-configuration protocols, such as Universal Plug and Play (UPnP) enabled. As with other aspects of business, use of security controls is governed by the needs of the users and the business.

Before you can configure a network or a network appliance, you have to evaluate your needs as well as consider acceptable use policies and balance all of this against the available network resources. The network administrator who fails to consider all of these things will become unpopular with both management and staff quickly. A sound and organized approach to network design and security is imperative for efficient and effective management of the network. Many questions must be answered before you can create and organize a network properly:

- **Resources:**
  - What are the available network resources?
  - How many users will be sharing those resources?
  - If you are working with limited bandwidth, you will need to consider traffic management and QoS features.
- What are the most critical network services?
  - Configure QoS to prioritize those services.
- What is considered acceptable use to management?

It can be difficult to filter all the kinds of content that users can access. Sure, you can block access to Facebook, but what about streaming video? Netflix? Sporting events? You can spend time determining where your bandwidth was used and adding new blocking rules to keep users from accessing bandwidth intensive applications. On the other hand, you could simply set up QoS policies that fairly share bandwidth so that in periods of high demand, everyone gets their fair share of the resources. If that fair share will still support streaming, then maybe that’s OK.

Often management wants to make sure users can’t do anything but their work. So, the acceptable use policy is simple: only work-related activities are to be tolerated on the network. The degree to which you can block all nonwork-related access will largely be determined by the network hardware available. Hardware procurement is done according to business needs (per policy).

- Which remote access needs to be supported?
  - VPN support will depend on the network hardware.
- Do you need to run an email server behind the firewall that can also be accessed by Internet users who aren't connecting through a VPN?
- How will you segment the network to minimize risks?

## Network Appliances

Increasingly, routers and switches are being integrated into smart switches, or even coupled with a traditional operating system to create smart network appliances. The degree to which each of these devices functions as a router varies, but often they offer a great deal of router functionality coupled with software that can ease the management of the network.

Generally, network appliances offer significant firewall capability, but many are specialized and focus on a single task, such as email, spam, or antivirus and malware detection. These devices can offer a strong arsenal of tools against attacks and ease the chore of managing access and network security.

Security appliances are also known as unified threat management (UTM) devices featuring gateway, antivirus, firewall, intrusion prevention and detection, and possibly more in a single product, as shown in Figure 21.7. Not only are many security features presented in a unified platform, but the solutions are streamlined and simplified to minimize the need for extensive training or security knowledge. Enterprises are increasingly relying on these appliances and expect their IT staff to manage them effectively.

![](img/2459befd842d6fed299fabe6da3512d0_532_0.png)

FIGURE 21.7 A UTM Device

Probably one of the more complex aspects of a network appliance is choosing one. There are a number of big names in the industry—names such as Cisco, Barracuda, and Juniper—that are popular with midsize companies. However,

many other companies—such as WatchGuard, Cyberroam, and Check Point—offer solutions for smaller organizations.

Beyond purchase price, it is important to evaluate a prospective device by carefully looking at the amount of training required, support for all of the features required by the network, as well as remote access and ongoing support and subscription costs.

Although UTM devices may seem like the best solution to Internet security issues, they also represent a single point of failure on the network and can potentially have vulnerabilities themselves. The device must also be able to handle all the network traffic or it will become a network bottleneck.

Selecting an appliance that is appropriate for your network needs is crucial, and you must consider many factors to ensure the product will be able to handle the demands of your networks rather than just focusing on security features.

## Proxy Servers

A proxy server is a barrier that prevents outsiders from entering a local area network and prevents insiders from directly connecting to outside resources, as illustrated in Figure 21.8. Instead, it allows clients to make indirect network connections that are routed through it. The client connects to the proxy server, with or without any conscious authentication, and makes a request for a resource from a different server. The proxy server will handle the request by either returning the requested resource from its own cached copy or by forwarding the request to the other server (after potentially modifying the request).

![](img/2459befd842d6fed299fabe6da3512d0_533_0.png)

FIGURE 21.8 Operation of a Proxy Server

All addressing information sent to the Internet will use the IP address of the proxy server. Because the IP address of the client that's requesting the resource isn't used, an outside intruder has no way of accessing the local host.

When fully configured, the proxy server will supply the client with the addresses and port numbers for Internet services (HTTP, FTP, and so on) that are available to it. Client access is configured from the local web browser. Most operating systems and web browsers provide for proxy configuration.

Probably the most common use of this concept is with caching web proxies where a local server will cache web resources and then expire them intelligently. Web proxies help conserve Internet bandwidth and speed up connections, but they can also be used to filter content or even reformat that content.

A proxy server might be an anonymous proxy that provides client anonymity by concealing their original IP address.

A transparent proxy makes the original IP address available but only in the HTTP headers. Typically, the user is not even aware they are using a transparent proxy. Many ISPs will transparently proxy connections so they can provide a customized “404 page” or search result filter.

A distorting proxy acts in a manner very similar to a transparent proxy, but it includes incorrect IP information in the HTTP headers.

Data communication through proxies is typically unencrypted and can be hacked. So, you need to be careful using nonlocal proxy services.

Malicious proxy servers have been used for various purposes including malware delivery, ad injection, and for simple information gathering. While these proxy servers may not pass on your IP address, they will know your IP address and could use this information for other purposes.

There are a great many open proxies on the Internet offering to conceal your IP address while you browse the Web in the name of privacy, etc. Before considering such a service, research them well and only use proxy servers that are known and trusted.

It is common to put a cluster of web servers behind a reverse proxy server that handles public requests for web resources and then forwards them to one or more of the servers, as illustrated in Figure 21.9. To the requestor, this process appears as if those web servers are being accessed directly. This arrangement can be beneficial for load balancing, effectively distributing requests for a single site among a number of servers.
