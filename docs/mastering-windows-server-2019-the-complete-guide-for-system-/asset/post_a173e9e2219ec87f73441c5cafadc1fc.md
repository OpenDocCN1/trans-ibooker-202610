
After doing that, WEB1 will reach over to the DHCP server and ask for an address, and you can see that I have now been assigned the reserved address of 10.10.10.150:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_192_0.png)

Figure 4.21: Reserving 10.10.10.150 for WEB1

This will always be the IP address of the WEB1 server from this point forward, unless I change my DHCP reservation or somehow change the MAC address of WEB1. This could possibly happen if I were to install a new NIC into WEB1.

> You can also create DHCP reservations for objects other than Windows devices in your network. Since all you need is the MAC address of the device (and every device with a network adapter has a MAC address), it is easy to create reservations for devices such as print servers, copy machines, security alarm systems, wireless access points, and more.

## DHCP failover

Never too much of a good thing? Just like the creation of multiple domain controller servers creates good redundancy for Active Directory, the DHCP servers in your network can be tied together into a failover pair to create their own form of high availability. While it is true that the DHCP server role often happens to co-exist alongside the AD DS and DNS roles, this is not a requirement. You already know that it is easy to store DNS zones right inside Active Directory so there is automatic replication of zone information, but this mentality is not true of DHCP scopes.

DHCP is a useful tool inside domain environments, or outside of domain environments. As such, it is not as tightly integrated a role as DNS. So when we create DHCP failover, we need to take a more manual approach, and whether or not the DHCP role happens to be hosted on top of a domain controller makes no difference.

## Two DHCP servers

In a DHCP server failover environment, two DHCP servers can be configured and pointed at each other, and they will then replicate DHCP lease information between themselves, always keeping this information up to date. That way, if one DHCP server goes offline, the other can pick up the slack and continue issuing IPs and renewing the leases of those IPs.

I said "two" DHCP servers above, and this is an important point of clarification. With Windows Server DHCP failover, you can only connect two DHCP servers together. No more. It is also important to note, as we progress into the world of IPv6, that DHCP failover is only intended for use with IPv4 scopes. The failover of DHCP servers wouldn't be necessary in the IPv6 world anyhow, as most IPv6 implementations are stateless, and the only information that DHCP needs to give to IPv6 clients are the options, which could simply be configured on multiple DHCP servers, and the clients care not from which server they pull that option information.

## Hot standby mode

There are two different modes of operation that can be employed by DHCP failover: hot standby mode or load sharing mode. Hot standby, as the name indicates, is more of a primary/failover mentality. One DHCP server is primary and is always responsible for an IPv4 scope, unless it becomes unavailable. In that case, the failover DHCP server starts taking over until the primary can be restored.

Hot standby mode is useful for branch offices that are connected back to the primary network via a WAN of some sort. The local branch offices could have their own local DHCP server that handles DHCP primarily, but if that DHCP server were to go offline, DHCP requests would reach over the WAN link and grab a lease from the standby DHCP server sitting in your main site. In this kind of deployment, you could easily configure one single DHCP server in the main site to be the hot standby for multiple branch offices, as it is easy to configure multiple scopes on a DHCP server.

## Load sharing mode

Alternatively, indeed much more common, is load sharing mode. This is the default mode of operation for DHCP failover. In load sharing mode, two DHCP servers are configured with the same scope of information and then both service incoming clients. In doing so, they share the load between servers, both responding and also replicating information between themselves, so both are always aware of which IP addresses have been handed out to clients.

In load sharing mode, you want both DHCP servers to be located in the same physical site. They need to have very fast communication between them to keep replication data straight. I have seen DHCP failover configured across sites before, and can tell you definitively that it can cause IP conflicts and similar issues. Keep them in the same site.

## Configuring DHCP failover

Now we know about DHCP failover, how do we set it up? You already know that I have DC1 and DC2 in my test lab environment. Both of these servers are already domain controllers and DC1 is also a DNS server, though those facts have no bearing on DHCP configuration. These could be domain controllers, or simply domain-joined servers, or even non-domain-joined servers, and DHCP would have all of the same options for failover. Since I already have these two servers available to me, I am going to use them to create a DHCP scope that is replicated between both. This is an important point to clarify, that DHCP failover is configured on a per-scope basis. You don't necessarily tie DC1 and DC2 together, but rather inside the DHCP scope properties you establish failover between DC1 and DC2 for a particular scope (or scopes). This implies, then, that you have the ability to create DHCP scopes that are running in failover mode, and additionally create DHCP scopes on those same servers that are not doing any form of failover.

> Make sure the clocks are in sync! If your DHCP servers are domain-joined, their clocks should always be kept in sync automatically by Active Directory. If your DHCP servers are not domain-joined, you will want to ensure that the system clocks on both servers are using the same time source to always retain continuity.

DC1 already has the DHCP role installed, and I already created a quick scope on it way back when I first set up this test lab, so that my Win10 client computer could grab a DHCP address and communicate with the rest of my network. DC2, however, does not yet have the DHCP role installed, so the first step is to go ahead and add that role.

Whenever creating new DHCP servers in a network, the role must be installed of course, but there is a second critical piece of the puzzle to enable them to be recognized as DHCP servers. It is fairly obvious, but after installing the DHCP role onto a server, Server Manager will prompt you to Complete DHCP configuration. This pulls up a small wizard with basically just one step to accomplish — Authorize your new DHCP server. This mini-wizard can be called via Server Manager, or if you missed the prompt there and opened up right into the DHCP management tool, you can alternatively authorize a DHCP server by right-clicking on the server name inside the DHCP management tool, and then selecting Authorize.

OK, we now have two authorized DHCP servers in our environment, and a scope is already present on DC1. This scope is servicing clients already, handing out IP addresses in our network. Creating failover for this scope is quite easy. Open up DHCP management on DC1, and navigate to find your IPv4 scope. Right-click on the scope itself, and select Configure Failover...:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_195_0.png)

Figure 4.22: Configuring DHCP failover

Next, select which scopes you want to configure into failover. As I mentioned before, DHCP failover configuration is individual per DHCP scope. If your DHCP server has multiple scopes already established, you can select to configure failover on all or only some of them. Then on the second screen, we need to define the failover DHCP server. You can type out the name of the server on this screen, but what I like to do is click that Add Server button, which then displays any servers in your environment that are recognized as authorized DHCP servers. By using the button and then selecting the second DHCP server from the list, it gives you one more validation that you have properly authorized the second server, DC2 in my case, in your network. You can see this in Figure 4.23:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_196_0.png)

Figure 4.23: Selecting DC2 for DHCP failover
