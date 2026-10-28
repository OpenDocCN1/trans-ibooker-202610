
Now, eventually all organizations should have the following safeguards in place; and the sooner, the better:

- **A well-trained staff.** Your crew needs to be savvy to face the many threats the Internet throws at them. For one, bringing one’s own online-capable devices into work may create issues with organizational security and should be addressed.
- **Update awareness.** Out-of-date security software is usually useless. Make sure yours doesn’t get to that point. Apply reliable vendor patches whenever they arrive.
- **Secure B2B networks.** Even if your organization is up to speed when it comes to the latest in cybersecurity, doing business with a compromised party may infect your systems as well. The Internet at large is not the only potential infected agent. Care must be taken with robust protocols and digital hygiene must be considered in B2B communications as well.
- **Compartmentalization.** All data in your organization should be accessible on a need-to-know basis only. Only trusted IT staff should be granted with user accounts which allow software installation.

## A Refresher on Network Security

Many cryptographic attack vectors are coming from an online environment. It makes sense to know at least the basics of current-day network hardware security which we’ll delve into next.

### Networks and Routers

Like you probably know, an interconnected series of devices sharing data using specific protocols constitutes a *network*. This can be a large corporate solution consisting of thousands of devices or a small home network consisting of exactly one router and a mere handful of devices, usually including a desktop computer (i.e., a typical *local area network*, or *LAN*). The Internet is basically one large network of servers and clients.

Now, a *router* is an important piece of hardware which transmits data between devices in a network. Website data, email, and other information coming in from the Internet enter a router in the form of *data packets*. All of these packets contain destination information (i.e., an *Internet protocol address* or *IP address*) which naturally belong to the device which made the request. Within a matter of seconds, these data packets find their way from the Internet into the router, which directs them toward a connected device.

There are a few important settings in routers which you need to pay close attention to; if such a device is compromised, it can infect a whole network, after all.

- **First, you need to access the router’s console.** This is usually done by typing in a *default IP address* into a web browser on a device connected to the network (see Figure 6-1). There are a variety of these IP addresses as they vary between hardware manufacturers. A one-size-fits-all solution isn’t unfortunately available. For example, some routers accept *192.168.1.1*, while others work with *192.168.0.1*.
- In addition to the default IP address, you also need a username and a password to access the router console. See Table 6-1 for some common combinations.

![](img/df9b6c40c665f58cab0c435444990c63_123_0.png)

Figure 6-1. A partial router console screen of a Zyxel-brand device

**Table 6-1. Some common default router usernames and passwords as of Q2 2020**

| Manufacturer | Default IP Address | Default Username | Default Password |
|---|---|---|---|
| Asus | 192.168.1.1 | admin | admin |
| Belkin | 192.168.2.1 | admin | admin |
| Digicom | 192.168.1.254 | admin | Michelangelo |
| Huawei/SUN/ZTE | 192.168.0.1 | admin | admin |
| Samsung | 192.168.0.1 | admin | password |
| Telco Systems | 192.168.0.1 | telco | telco |
| Zcom | 192.168.0.1 | root | admin |
| Zyxel | 192.168.1.1 | admin | 1234 |

Usually the router’s backside contains a sticker with the default username and password. If none of the preceding combinations work and/or you can’t find the aforementioned sticker, refer to the manufacturer’s website.

- **Next, you need to change the router’s default password into a stronger one.** Remember the makings of a strong password: length, complexity, and not using a dictionary for inspiration.
- **When using Wi-Fi, make sure the strongest available encryption is in use.** Navigate to your router console’s wireless page; it’s usually listed under *Wireless LAN* or something to that effect.
- For your Wi-Fi encryption, you’ll probably have the choice of *Wi-Fi Protected Access (WPA)* protocol in its three incarnations. The original WPA aside, you have the ubiquitous *WPA2* and, in the case of newer routers, the *WPA3* of 2018. While WPA2 has vulnerabilities, it’s not practically obsolete like WPA. Always choose the latest possible version of this protocol.
- Many older network devices also offer *Wired Equivalent Privacy (WEP)* as an option. This standard was introduced in 1999 and came originally with mere 64-bit key length. WEP was later bolstered with 128- and 256-bit key lengths, but it remains a compromised protocol you should avoid. If your hardware only offers you this method for your Wi-Fi security, it’s time to upgrade your routers.
- **Disable remote access and Universal Plug and Play.** The last thing you want is someone outside of your network to administer your router settings. In some devices remote access might be enabled by default. Use the console to untick the box on this feature which is usually listed simply under *Remote Management* or *Remote MGMT*. Another feature you should seriously consider disabling is *Universal Plug and Play (UPnP)*. For one, a router with UPnP enabled may offer a method for hackers to bypass your device’s firewall entirely.
- **Switch off Wi-Fi Protected Setup (WPS) as soon as you can.** This feature exists to facilitate a quick one-button solution for setting up a wireless network. Aimed mostly at the less geeky users, WPS is actually a major security problem in many cases. The system is implemented using either a four-digit PIN code or a push-button-connect approach. The latter is very susceptible to brute-force attacks, while having a malicious actor on the premises of your router neutralizes the former’s effectiveness.
- **Get acquainted with network address translation (NAT).** This is a technique which takes a single public IP address and shares it with a multitude of connected devices using a router/firewall. For example, a router in a network might have the local IP of 192.168.1.1 and a public one of, say, 86.1.70.30 as assigned by your Internet service provider, visible for all the websites you or your employees visit using this setup. Now, under NAT the router will assign local IP addresses for any devices connected to it, for example, 192.168.1.2 for your desktop and 192.168.1.3 for your colleague’s tablet (see Figure 6-2). These IP addresses will remain clandestine from the public at large.

![](img/df9b6c40c665f58cab0c435444990c63_126_0.png)

Figure 6-2. A simple local area network using the NAT technique

Better security aside, NAT allows for the conservation of limited amount of IPv4 addresses. Using a 32-bit address space, the IPv4 specification limits the global number of different public IP addresses to some 4.2 billion, still largely in use. Without unique IP addresses, the Internet can’t function properly.

- How to make sure your router is running on NAT?
  Make sure the device uses *routing* instead of *bridging* for its mode of operation. This is usually the default setting. With bridging, you lose NAT in exchange for somewhat faster traffic flow; this is generally not recommended.
- **Update your router’s firmware.** The firmware in a router is a writeable chunk of nonvolatile memory which holds the device’s operating system. If you’re running your device on old firmware, chances are you’re either missing out on some good features or flat-out compromising your security. You’ll find the latest firmware updates on your router’s manufacturer’s website usually under *Support ► Firmware*.
- **Make MAC filtering your best friend.** All networking devices, including routers and smartphones, have a unique *media access control (MAC)* address. It consists of six groups of hexadecimal values separated by colons and looks something like this: *b8:66:4c:f4:3d:6c*. For now, a MAC is allocated from a 48-bit address space. This allows for some 281 trillion unique strings and is designed to last us until 2080. A newer 64-bit implementation for MACs has been devised, but not yet widely adopted.
- *MAC filtering* is a technique where only safe hardware, identified through their unique MAC addresses, is allowed to access a local area network. In other words, a MAC filter is a user-maintained whitelist which blocks everything except your trusted devices; you administer it usually from your router console under
