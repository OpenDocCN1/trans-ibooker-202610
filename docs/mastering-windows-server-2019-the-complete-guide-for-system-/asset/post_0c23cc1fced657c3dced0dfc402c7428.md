
As you can see in *Figure 8.2*, in the course of the setup wizards, you will have to define one NIC as **External**, and the other as **Internal**. Any more NICs that exist in that server will not be used by DirectAccess, unfortunately. Maybe this is something that will change in future versions!

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_350_0.png)

Figure 8.2: Defining NICs during DirectAccess configuration

## To NAT or not to NAT?

Now that you have decided to roll with two NICs in your DirectAccess server, where do we plug in the external NIC? There are two common places that this external network interface can be connected to, but depending on which you choose, the outcome of your DirectAccess environment can be vastly different. Before we talk about the actual placement of the NIC, I would like to define a couple of protocols that are important to understand, because they pertain very much to answering this question about NIC placement. When your DirectAccess laptop makes a connection to the DirectAccess server, it will do so using one of the three IPv6 transition tunneling protocols. These protocols are **6to4**, **Teredo**, and **IP-HTTPS**. When the DA client connects its DA tunnels, it will automatically choose which of these protocols is best to use, depending on the user's current internet connection. All three protocols perform the same function for a DirectAccess connection: their job is to take the IPv6 packet stream coming out of the laptop and encapsulate it inside IPv4 so that the traffic can make its way successfully across the IPv4 internet. When the packets get to the DirectAccess server, they are decapped so that the DA server can process these IPv6 packets.

### 6to4

DA clients will only attempt to connect using 6to4 when a remote laptop has a true public internet IP address. This hardly ever happens these days, with the shortage of available internet IPv4 addresses, and so 6to4 is typically not used by any DirectAccess client computers. In fact, it can present its own set of challenges when users are connecting with cell phone cards in their laptops, and so it is common practice to disable the 6to4 adapter on client computers as a DirectAccess best-practice setting.

### Teredo

When DA clients are connected to the internet using a private IP address, such as behind a home router or a public Wi-Fi router, they will attempt to connect using the Teredo protocol. Teredo uses a UDP stream to encapsulate DA packets, and so, as long as the user's internet connection allows outbound UDP 3544, Teredo will generally connect and is the transition protocol of choice for that DirectAccess connection.

### IP-HTTPS

If Teredo fails to connect, such as in the case where the user is sitting in a network that blocks outbound UDP, then the DirectAccess connection will fall back to using IP-HTTPS, pronounced IP over HTTPS. This protocol encapsulates the IPv6 packets inside IPv4 headers, but then wraps them up inside an HTTP header and encrypts them with TLS/SSL, before sending the packet out over the internet. This effectively makes the DirectAccess connection an SSL stream, just like when you browse an HTTPS website.

## Installing on the true edge – on the internet

When you plug your DirectAccess server's external NIC directly into the internet, you grant yourself the ability to put true public IP addresses on that NIC. In doing this, you enable all three of the preceding transition tunneling protocols, so that DirectAccess client computers can choose between them for the best form of connectivity. When installing via the true edge method, you would put not only one but two public IP addresses on that external NIC. Make sure that the public IP addresses are consecutive as this is a requirement for Teredo. When your DirectAccess server has two consecutive public IP addresses assigned to the external NIC, it will enable the Teredo protocol to be available for connections.

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_352_0.png)

The NIC does not necessarily have to be plugged directly into the internet for this to work. Depending on your firewall capabilities, you might have the option to establish a *Bridged DMZ* where no NATing is taking place. You need to check with your firewall vendor to find out whether or not that is an option for your organization. In this scenario, you are still able to configure true public IP addresses on the external NIC, but the traffic flows through a firewall first, in order to protect and manage that traffic.

## Installing behind a NAT

It is much more common for the networking team to want to place the external NIC of your DirectAccess server behind a firewall, inside a DMZ. This typically means creating a NAT to bring this traffic into the server. While this is entirely possible and better protects the DirectAccess server itself from the internet, it does come with a big downside. When you install a DA server behind a NAT, Teredo no longer works. In fact, the DirectAccess configuration wizards will recognize when you have a private IP address listed on the external NIC and will not even turn on Teredo.

When Teredo is not available, all of your DirectAccess clients will connect using IP-HTTPS. So why does it even matter if Teredo is unavailable? Because it is a more efficient protocol than IP-HTTPS. When Teredo tunnels packets, it simply encapsulates IPv6 inside IPv4. The DirectAccess traffic stream is already and always IPsec-encrypted, so there is no need for the Teredo tunnel to do any additional encryption. On the other hand, when IP-HTTPS tunnels packets, it takes the already-encrypted IPsec traffic stream and encrypts it a second time using SSL. This means all of the packets that come and go are subject to double encryption, which increases processing and CPU cycles, and makes for a slower connection. It also creates additional hardware load on the DirectAccess server itself, because it is handling twice the amount of encryption processing.

This is a particularly apparent problem when you are running Windows 7 on client computers, as double encryption processing will cause a noticeably slower connection for users. DirectAccess still works fine, but if you sit a Teredo-connected laptop next to an IP-HTTPS-connected laptop, you will notice the speed difference between the two.

Thankfully, in Windows 8 and Windows 10, there have been some countermeasures added to help with this speed discrepancy. These newer client operating systems are now smart enough that they can negotiate the SSL part of the IP-HTTPS tunnel by using the NULL encryption algorithm, meaning that IP-HTTPS is not doing a second encryption and IP-HTTPS performance is now essentially on par with Teredo.

However, this only works for the newer client operating systems (Win7 will always double encrypt with IP-HTTPS), and it still doesn't work in some cases. For example, when you enable your DirectAccess server to also provide VPN connectivity, or if you choose to employ a **One-Time-Password (OTP)** system alongside DirectAccess, then the NULL algorithm will be disabled because it is a security risk in these situations, and so even your Windows 8 and Windows 10 computers will perform double encryption when they connect via IP-HTTPS. You can see where it would be beneficial to have Teredo enabled and available so that any computers that can connect via Teredo will do so.

To summarize, you can certainly install your DirectAccess server's external NIC behind a NAT, but be aware that all DA client computers will connect using the IP-HTTPS protocol, and it is important to understand the potential side-effect of implementing it in this way.

## Network Location Server

This major component in a DirectAccess infrastructure is something that does not even exist on the DA server itself, or at least it shouldn't if you are setting things up properly. The **Network Location Server (NLS)** is simply a website that runs inside the corporate network. This website does not need to be available for access over the internet; in fact, it should not be. The NLS is used as part of the inside/outside detection mechanism on DirectAccess client computers, similar to the way that Trusted Network Detection works for Always On VPN. Every time a DA client gets a network connection, it starts looking for the NLS website. If it can see the site, then it knows that you are inside the corporate network, and DirectAccess is not required, so it turns itself off. However, if your NLS website cannot be contacted, it means you are outside the corporate network, and DirectAccess components will start turning themselves on.

This prerequisite is easily met; all you need to do is spin up a VM and install IIS on it to host this new website, or you can even add a new website to an existing web server in your network. There are only two things to watch out for when setting up your NLS website. The first is that it must be an HTTPS site, and so it requires an SSL certificate. We will discuss the certificates used in DA, including this one, in the next section of this chapter. In addition to making sure that the website is accessible via HTTPS, you must also make sure that the DNS name you are using to contact this website is unique. You want to do this because, whatever name you choose for the NLS website, that name will not be resolvable when client computers are outside the corporate network. This is by design, because you obviously don't want your DA clients to be able to successfully contact the NLS website when they are working remotely, as that would then disable their DirectAccess connection.
