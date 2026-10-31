
Whenever a company configures their internal DNS to match their external DNS, this is commonly referred to as **split-brain DNS**. Years ago, there were articles published on the internet by Microsoft warning administrators against doing this, as it caused confusion and problems with some Microsoft roles and technologies that those companies would try to deploy in their environments. As time went on, many people either didn't know or simply ignored this recommendation, and so the world is now awash with networks where split-brain DNS exists. Many of these environments are so populated and complex that they will never be changed at this point, and so Microsoft and the world had no choice but to adapt. Thankfully in today's world, most roles or services that you deploy are written in a way that they accommodate for possible split-brain DNS out of the box, but some still require special considerations.

One example is Microsoft DirectAccess, which we will cover in *Chapter 8, Remote Access*. When deploying DirectAccess, there is a critical piece of client-side technology called the Name Resolution Policy Table that must be populated based on your internal DNS architecture. For companies using a .local domain internally, this configuration is a piece of cake and no special considerations are needed. For anyone with split-brain DNS, this sometimes turns into a convoluted mess and for anyone not well-versed in the intricacies of DirectAccess, simply having split-brain DNS in your environment could very well mean that you are never actually able to get DirectAccess to work in your environment.

At first glance, you would think that administration would be made easier if internal DNS zones matched public DNS zones, but this simply isn't true. The opposite, in fact, is what you will find. Whenever you publish any external service, you will then have to make special considerations with your internal DNS to make that service work as intended from inside the network, because internal DNS zones will typically trump external. Split-brain DNS ends up being more work on a daily basis.

As the saying goes, you can lead a horse to water but can't make him drink. You have now been informed that utilizing the same DNS zone inside your network as what you use on the internet is bound to cause you some headaches. What you decide to do with this information on future builds, well that's up to you...

## Types of DNS zones

You are now familiar with creating different types of DNS records, but that information is only going to enable you to create new records inside of an existing DNS zone. At present in our test lab, we have only one DNS zone available to us, contoso.local, which was created automatically when we built the contoso.local domain. As of right now, computers who are using one of my contoso.local domain controllers as their DNS server are only able to look up DNS records that I have plugged into my DNS zone.

With DNS Server provided by Windows Server 2019, you can certainly build out many different DNS zones, to increase name resolution capabilities in your network. There are plenty of different reasons why you might want to create additional DNS zones, and you should understand what types of zones are available to implement. Let's take a minute and discuss the different types of zones available to us.

## Active Directory Integrated Zones

This is not a zone "type," per se, but rather an option that can be selected when you create some types of DNS zones. When creating that new zone, you will see a checkbox that states Store the zone in Active Directory. When selected, this means that the new zone you are creating is going to be stored inside Active Directory, rather than being stand-alone on the DNS server from which you are creating the zone. Being stored inside AD means that this new zone is capable of being replicated automatically to all domain controller servers in your domain, which means that your DNS zone is highly available and more quickly accessible across your entire network. In general, if the type of zone you are creating allows storage inside Active Directory, do it.

After opening up the DNS Manager tool on your domain controller, you'll notice that any current DNS zones are listed beneath folders entitled Forward Lookup Zones and Reverse Lookup Zones. What's the difference?

## Forward Lookup Zones

Zones configured inside Forward Lookup Zones are your traditional DNS zones. Their purpose is to take an incoming DNS request, such as from a client computer, and turn that DNS name request into an IP address that is handed back to the client computer. Forward Lookup Zones are almost always where DNS administrators will be working from.

## Reverse Lookup Zones

If "reverse" is the opposite of "forward," then the same must be true within a DNS context. Reverse Lookup Zones are responsible for mapping IP addresses backward into names. You may not even realize that these happen in your environment, but you can easily test it out anytime. You likely all know how to use the ping command, which is relying on DNS to turn a name into an IP address. That, of course, is being handled by a Forward Lookup Zone, like we see here:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_177_0.png)

## Figure 4.8: Resolving a name to IP

Did you know you can also use the ping command to perform the opposite? What if you want to query DNS to find out what hostname the 10.10.10.11 IP address maps back to?

```
ping -a 10.10.10.11
```

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_177_1.png)

## Figure 4.9: Querying DNS to find a hostname from an IP

This reverse lookup, converting IP addresses back to names, has shown us that the IP address 10.10.10.11 has a reverse pointer record mapping back to our DC2 server. This type of lookup is handled by a Reverse Lookup Zone inside DNS.

Now that we understand the difference between Forward and Reverse Lookup Zones, right-clicking on **Forward Lookup Zones** and selecting **New Zone...** lands us with three choices on which type of zone we would like to create:

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_178_0.png)

Figure 4.10: Creating zones

## Primary Zone

The majority of DNS zones within internal networks are primary zones. This indicates that it is the "parent," so to speak, the master copy of any particular DNS zone is going to be the primary zone for that domain. When changes and updates are made to DNS records, it always happens within the primary zone. We will create a new primary zone together in just a few minutes. You can choose for primary zones to be stored in Active Directory, or not.

## Secondary Zone

Based on their names, it would seem secondary zones might be less important than primary zones, and that is generally true. A secondary zone is simply a read-only copy of a primary zone. Because it is read-only, no updates can be made within a secondary zone, it is always syncing from the primary zone. Secondary zones can be used to spread the DNS computing power among multiple servers, taking the load off of the primary zones, but it is important to know that secondary zones are not Active Directory-integrated. When you try selecting the option to create a new secondary zone, you will notice that the AD checkbox grays out.

Inside most networks I have worked with that are based on Microsoft infrastructure, your primary zones all get set up to be Active Directory-integrated, which means they are syncing around to all of your domain controller-based DNS servers anyway. In this case, secondary zones are less likely to be needed. If you deviate from that mentality and create DNS servers that are not also domain controllers (which you can absolutely do) – then you may want to take care that you understand the use cases of secondary zones because it is more likely in this scenario that you would utilize them.

## Stub Zone

The third selection on your new zone screen is Stub Zone. Like secondary zones, stub zones are copies of primary zones but only contain certain pieces of information. Namely, stub zones contain resource records that help clients to identify and more quickly get to the full DNS servers for whatever zone they are calling. Stub zones can be AD-integrated if required.

Stub zones are enabling your DNS servers to know how to direct traffic to resolve records inside another domain. Sounds just like a conditional forwarder that we learned about in the previous chapter, right? Similar, yes, but as you know we pointed our conditional forwarder at a specific IP address, telling DNS that requests for the remote domain should always point at a particular server or servers. Stub zones contain NS records for the remote domain, so even if that remote domain makes some internal adjustments and its DNS servers change or are updated, stub zones can more accurately deal with those scenarios.

AD-integrated DNS zones replicate among domain controllers using domain replication, but non-AD-integrated DNS zones have to sync using their own process, known as zone transfers. Stub zones can be used to help make the zone transfer process more efficient and create faster name resolution for client computers.
