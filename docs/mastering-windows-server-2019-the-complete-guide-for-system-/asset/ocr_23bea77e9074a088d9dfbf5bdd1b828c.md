I have not yet created any DNS records in my new lab environment, not purposefully anyway, and yet when I open the DNS Manager console from inside the Tools menu of Server Manager, I can see a handful of records already existing. This is because when I joined each of these machines to the domain, it automatically registered these records for me so that the new servers and clients were immediately resolvable within our domain:

![](bbox=[0.13, 0.2, 0.87, 0.59])

Figure 4.1: DNS Manager

# Host record (A or AAAA)

The first kind of DNS record we are looking at is the most common type that you will work with. A host record is the one that resolves a particular name to a particular IP address. It's pretty simple, and for most of the devices on your network this will be the only kind of record that exists for them inside DNS. There are two different classes of host records that you should be aware of, even though you will likely only be using one of them for at least a few more years. The two different kinds of host records are called an A record and an AAAA record, which is pronounced Quad A. The difference between the two? A records are for IPv4 addresses and will be used in most companies for years to come. AAAA records serve the exact same purpose of resolving a name to an IP address, but are only for IPv6 addresses, and will only be useful if you use IPv6 in your network.