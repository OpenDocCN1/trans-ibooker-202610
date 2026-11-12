
Translation table

| Inside local IP | Inside global IP | Inside local port | Inside global port |
|---|---|---|---|
| 192.168.1.10 | 200.200.200.7 | 50,000 | 50,000 |
| 192.168.1.11 | 200.200.200.7 | 50,000 | 50,001 |

![](img/0eafdcebe219ef3756f0e57bcf1eb997_775_0.png)

Figure 13-6 PAT example

![](img/0eafdcebe219ef3756f0e57bcf1eb997_775_1.png)

**NOTE** Note that you don’t have to restrict yourself to one type of address translation process. For instance, PAT is typically used for inside-to-outside connections, while static NAT is used for outside-to-inside connections.

## Port Address Redirection

The previous example showed PAT being carried out dynamically by the address translation device. In some situations, however, this will not work.

For instance, your ISP may assign you a single public IP address. You need to use this with PAT to enable inside users to access outside resources. However, you’ll have a problem if you want outside users to access an internal service, such as a web server. Dynamic PAT, unfortunately, won’t work in this situation.

Another solution to this problem is available: static PAT, often called *port address redirection* (PAR). Let’s look at a simple example to illustrate how PAR works. Assume that your ISP has assigned you a single public IP address: 199.199.199.1. You need to use this address for inside users to access the outside world, but you still need the outside world to access an internal web server. With static PAT, you set up your address translation device to look not only at the destination IP address (199.199.199.1), but also at the destination port number (80 for a web server). You create a static PAT entry such that when the address translation device sees this combination of address and port numbers, the device translates it to the inside local IP address and, possibly, the port number used for the service on this inside device.

For example, assume you are given the network shown back in Figure 13-5. In this example, your ISP has assigned you a single IP address, 200.200.200.1, and this address must be configured on your router’s S0 interface. This presents a problem in this example, since you have an internal web server that you want external users to access. Port address redirection can be used to overcome this problem. You would set up a static PAT entry on your router that would take TCP traffic sent to 200.200.200.1 on port 80 and redirect it to 10.1.1.1 on port 80. Without PAR, the router would try to process the web connection itself since this IP address is assigned to its own local interface.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_776_0.png)

**EXAM TIP** For the exam, you should remember that port address redirection enables you to redirect application traffic directed to one address to a different address.

## Advantages of Address Translation

As mentioned, network address translation devices are typically used to give you an almost inexhaustible number of addresses as well as to hide your internal network addressing scheme. Another advantage of address translation is that if you change ISPs or merge with another company, you can keep your current addressing scheme and make any necessary changes on your address translation device or devices, making your address management easier.

Another big advantage of address translation is that it gives you tighter control over traffic entering and leaving your network. For example, if you are using private addresses internally, all traffic entering and leaving must pass through an address translation device. Because of this restriction, it is much easier to implement your security and business policies.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_777_0.png)

**EXAM TIP** Remember the advantages of network address translation: conservation of public addresses, protection of resources (security) with private addresses from external devices, and no need for readdressing of devices when switching from one ISP to another.

## Disadvantages of Address Translation

Even though network address translation solves many problems and has many advantages, it also has its share of disadvantages. Here are three main issues:

- Each connection has an added delay.
- Troubleshooting is more difficult.
- Not all applications work with address translation.

Because address translation changes the contents of packets and, possibly, segment headers, as well as computing any necessary new checksum values, extra processing is required on each packet. This extra processing, obviously, will affect the throughput and speed of your connections. The more packets that pass through your address translation device needing translation, the more likely your users will notice the delay. Therefore, choosing the appropriate product for address translation becomes very important.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_778_0.png)

**EXAM TIP** Remember the disadvantages and limitations of address translation for the exam.

Whenever problems arise with connections involving address translation, it can be difficult to troubleshoot them. It is difficult to track down the real source and destination of a connection—you have to log into your address translation device and look at your translation tables. And if the packet is going through multiple layers of translation, possibly at both the source and destination sites, this can be a hair-pulling experience. Also, even though one of the advantages of address translation is that it hides your internal addressing scheme, it also creates security issues—an external hacker can more easily hide his identity and IP address by sending his packets through a translation device or multiple translation devices.

Probably the most difficult issue with address translation is that not all applications will work with it. For instance, some applications embed IP addressing or port information in the actual data payload (such as FTP), expecting the destination device to use this addressing information in the payload instead of what is in the packet and segment headers. This can pose a problem with address translation, since address translation, by default, doesn’t translate data payload information, only header information. Multimedia and NetBIOS applications are notorious for embedding addressing information in data payloads. In some instances, some vendors’ address translation devices can detect this process for certain applications and fix it when building the appropriate translation in the translation table. For instance, Cisco routers with the IOS firewall feature set and ASA security appliances support a fix-up process that covers many of these application issues, including embedded addressing information. However, if your product doesn’t support this feature, you’ll need to disable address translation for the affected devices.

## NAT Configuration

As mentioned, NAT can be static or dynamic. The configuration process is similar for both types. Probably the most difficult thing about configuring address translation is understanding the difference between the terms *inside* and *outside*. These terms refer to where your devices are located (inside) and where the external network (the Internet, for instance) is (outside). This is important when it comes to the configuration of address translation. On the IOS device, you must perform two basic configuration steps:

- Define the address translation type (Global Configuration mode commands).
- Define the location of devices (Interface Subconfiguration mode commands).

The following sections cover the configuration of both static and dynamic NAT.

## Configuring Static NAT

As mentioned, static NAT is typically used when devices on the outside of your network want to access resources, such as web, DNS, and e-mail servers, on the inside. Here are the two commands used to define the static translations for NAT:

```
Router(config)# ip nat inside source static
    inside_local_source_IP_address
    inside_global_source_IP_address
Router(config)# ip nat outside source static
    outside_global_destination_IP_address
    outside_local_destination_IP_address
```

The **inside** and **outside** parameters specify the direction in which translation will occur. For instance, the **inside** keyword specifies that the inside source local IP addresses are translated to an inside global IP address when *leaving* the network; and the destination global IP addresses are
