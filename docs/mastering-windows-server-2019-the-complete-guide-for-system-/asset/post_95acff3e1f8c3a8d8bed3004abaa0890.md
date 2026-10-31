
## AOVPN server components

Now that we understand what is needed on the client side to make Always On VPN work, what parts and pieces are necessary on the server/infrastructure side in order to allow these connections to happen? Interestingly, the Always On component of AOVPN has nothing to do with server infrastructure; the Always On part is handled completely on the client side. Therefore, all we need to do on the server side is make sure that we can receive incoming VPN connections. If you currently have a workforce who are making successful VPN connections, then there is a good chance that you already have the server infrastructure necessary for bringing AOVPN into your environment.

### Remote Access server

Obviously, you need a VPN server to host VPN connections, right? Well, not so obviously. In Windows Server, the role that hosts VPN, AOVPN, and DirectAccess connections is called the Remote Access role, but you can actually get Always On VPN working without Windows Server as your Remote Access server. Since the Always On part is client-side functionality, this enables VPN server-side infrastructures to be hosted by third-party vendors. Even though that is technically accurate, it's not really what Microsoft expects; nor is it what I find in the field. In reality, those of us interested in using Microsoft Always On VPN will be using Microsoft Windows Server to host the Remote Access role, which will be the inbound system that our remote clients connect to.

A lot of people automatically assume that AOVPN is married to Windows Server 2019 because it's a brand-new technology and Server 2019 was just released, but that is actually not the case at all. You can host your VPN infrastructure (the Remote Access role) on Server 2019, Server 2016, or even Server 2012 R2. It works the same on the backend, giving clients a place to tap into with their VPN connections.

After installing the Remote Access role on your new Windows Server, you will find that the majority of the VPN configuration happens from the Routing and Remote Access (RRAS) console. While configuring your VPN, you will find that there are multiple protocols that can be used to establish a VPN connection between client and server, and you should have at least a brief understanding of what those different protocols are. I'll list them here in order of strongest and most secure, all the way down to don't touch this one!

### IKEv2

The newest, strongest, and all-in-all best way to connect your client computers via VPN or AOVPN, IKEv2 is the only way to connect the AOVPN device tunnel. IKEv2 requires machine certificates to be issued to your client computers in order to authenticate. This generally means that if you want clients to connect via IKEv2, those clients will be domain-joined. It is very important to note that IKEv2 uses UDP ports 500 and 4500 to make its connection, which is sometimes a hindrance and blocked by firewalls.

### SSTP

Considered to be the fallback method for connecting AOVPN connections, SSTP uses an SSL stream to connect. Because of this, it requires an SSL certificate to be installed on the Remote Access server but does not require machine certificates on the client computers. SSTP uses TCP port 443, and so it is able to connect even from inside very restrictive networks where IKEv2 may fail (because of IKEv2's reliance on UDP).

### L2TP

Not generally used for AOVPN deployments, L2TP is able to establish VPN connections by using certificates or a pre-shared key. Since you already have two better protocols at your disposal, you shouldn't be using this one.

### PPTP

While still a valid configuration option inside RRAS, stay away from this guy! PPTP is no longer a secure method of tunneling, and if you are still running VPN connections based on PPTP, you essentially need to consider those traffic streams to be unencrypted and clear-text over the internet.

### Certification Authority (CA)

Machine certificates, user certificates, SSL certificates... Oh, my! Clearly, you need to be familiar with working with and deploying certificates to make use of Always On VPN. This is becoming more and more common with newer, well-secured technologies of any flavor. The major requirement here is that you will need to have PKI inside your environment and at least one Windows CA server to issue the necessary certificates. The following is a list of the places in which certificates could be used by an AOVPN infrastructure:

- **User certificates**: These are the certificates issued to your VPN users from an internal CA, used for authentication of the user tunnel
- **Machine certificates**: These are certificates issued to your workstations (mostly laptops) from an internal CA, used for authentication of the device tunnel
- **SSL certificate**: Installed on your Remote Access server to validate the incoming traffic for SSTP VPN connections
- **VPN and NPS machine certificates**: Your Remote Access server, as well as your NPS servers, which we will talk about in just a minute, require machine certificates issued from your internal CA

### Network Policy Server (NPS)

NPS is basically the authentication method for VPN connections. When a VPN connection request comes in, the Remote Access server hands that authentication request over to an NPS server to validate who that user is and also to verify that the user has permissions to log in via the VPN.

Most commonly when working with Microsoft VPN connections, we configure NPS so that it allows only users who are part of a certain Active Directory Security Group. For example, if you create a group called VPN Users and then point NPS to that group, it will only allow users whom you have placed inside that group to make successful VPN connections.

NPS is another Windows Server role that can be hosted on its own system or spread across multiple servers for redundancy. As with the Remote Access role, there is no Server 2019 requirement for NPS. You could easily deploy it on previous versions of Windows Server just as well.

In small environments that have just a single Remote Access server, it is common to co-host the NPS role right on the same server that is providing VPN connectivity.

### DirectAccess

Throughout our discussion about Always On VPN, I mentioned Microsoft DirectAccess a couple of times. DirectAccess is another form of automatic VPN-like connectivity, but it takes a different approach than that of Always On VPN. Where AOVPN simply uses expected, well-known VPN protocols and does some crafty magic to automatically launch those otherwise traditional VPN tunnels, DirectAccess tunnels are quite proprietary. Tunnels are protected by IPsec and are essentially impenetrable and also unable to be impersonated. I find that security teams love the protections and complexity surrounding DA tunnels because it is a connection platform that attackers have no idea how to tamper with or how to replicate.

In my experience, at this point in the game, Microsoft DirectAccess is the most common reason that administrators deploy the Remote Access role on a Windows Server instance. As stated, the easiest way to think about DirectAccess is to think of it as an automatic VPN. Similar to a VPN, its purpose is to connect users' computers to the corporate network when they are outside the office. Different from a VPN, however, is the method that employees use in order to make this connection possible. DirectAccess is not a software component; it is a series of components that are already baked into the Windows operating system, working in tandem to provide completely seamless access for the user. What do I mean by seamless? In the same way that AOVPN connects without user interaction, there is nothing the user needs to do to make DirectAccess connect. It does that all by itself. As soon as the mobile computer receives an internet connection, whether that connection is home Wi-Fi, public internet at a coffee shop, or a cell phone hotspot connection, DirectAccess tunnels automatically build themselves using whatever internet connection is available, without any user input.

Whether using Always On VPN or DirectAccess, when your computer connects automatically it saves you time and money. Time is saved because the user no longer has to launch a VPN connection. Money is saved because time equals money, but also because having an *always-on* connection means that patching, security policies, and the management of those remote computers always happens, even when the user is working remotely. You no longer have to wait for users to get back into the office or for them to choose to launch their manual VPN connection in order to push new settings and policies down to their computers; it all happens wherever they are, as long as they have internet access. Clearly, with the advent of two different remote access technologies that are both focused on automatic connectivity for remote users, Microsoft is clearing the way for a more productive workforce. The terms **user-friendly** and **VPN** have never gone hand-in-hand before, but in the latest versions of the Windows operating systems, that is exactly the goal.

DirectAccess has actually been around since the release of Windows Server 2008 R2, and yet I regularly bump into people who have never heard of it. In the early days, it was quite difficult to deploy and came with a lot of awkward requirements, but much has changed over the past few years and DirectAccess is now easier than ever to deploy, and more beneficial than ever to have running in your environment.
