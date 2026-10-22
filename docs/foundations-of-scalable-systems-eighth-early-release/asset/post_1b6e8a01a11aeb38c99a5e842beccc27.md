
these two instances indicates they are overloaded and things will only get worse if the request load increases.

![](img/936a72197720a5158b16541313af61c9_45_0.png)

Figure 2-8. An Example of Scaling Up a Database Server

Hopefully this simple example illustrates why scaling through simple upgrading of hardware needs to be approached carefully. Adding more hardware always increases costs, but may not always give the performance improvement you expect. Running simple experiments and taking measurements is essential for assessing the effects of hardware upgrades. It gives you solid data to guide your design, and justify costs to stakeholders.

## Summary and Further Reading

In this chapter I’ve provided a whirlwind tour of the major approaches you can utilize to scale out a system as a collection of communicating services and distributed databases. Much detail has been brushed over, and as you have no doubt realized - in software systems the devil is in the detail.

Subsequent chapters will therefore progressively start to explore these details, starting with some fundamental characteristics of distributed systems in Chapter 3 that everyone should be aware of.

Another area this chapter has skirted around is the subject of software architecture. I’ve used the term *services* for distributed components in an architecture that implement application business logic and database access. These services are independently deployed processes that communicate using remote communications mechanisms such as HTTP. In architectural terms, these services are most closely mirrored by those in the Service Oriented Architecture (SOA) pattern, an established architectural approach for building distributed systems. A more modern evolution of this approach revolves around microservices. These tend to be more cohesive, encapsulated services that promote continuous development and deployment.

If you’d like a much more in-depth discussion of these issues, and software architecture concepts in general, then Mark Richards’ and Neal Ford’s book¹⁰ is an excellent place to start.

Finally, there’s a class of *big data* software architectures that address some of the issues that come to the fore with very large data collections. One of the most prominent is data reprocessing. This occurs when data that has already been stored and analyzed needs to be re-analyzed due to code or business rule changes. This reprocessing may occur due to software fixes, or the introduction of new algorithms that can derive more insights from the original raw data. There’s a good discussion of the Lambda and Kappa architectures, both of which are prominent in this space, in Jay Krepps’s article, Questioning the Lambda Architecture.

+   5 https://redis.io/
+   6 https://memcached.org/
+   7 https://www.allthingsdistributed.com/2019/08/modern-applications-at-aws.html
+   8 https://samnewman.io/patterns/architectural/bff/
+   9 Results are courtesy of Ruijia Xiao from Northeastern University, Seattle
+   10 Mark Richards and Neal Ford, *Fundamentals of Software Architecture: An Engineering Approach*, 1st Edition, O’Reilly Media, 2020

## Chapter 3. Distributed Systems Essentials

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the author’s raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

As I described in Chapter 2, scaling a system naturally involves adding multiple independently moving parts. We run our software components on multiple machines and our databases across multiple storage nodes, all in the quest of adding more processing capacity. Consequently, our solutions are distributed across multiple machines in multiple locations, with each machine processing events concurrently, and exchanging messages over a network.

This fundamental nature of distributed systems has some profound implications on the way we design, build and operate our solutions. This chapter provides the basic ‘nature of the beast’ information you need to know to appreciate the issues and complexities of distributed software systems. We briefly cover communications networks hardware and software, remote method invocation, how to deal with the implications of communications failures, distributed coordination, and the thorny issue of time in distributed systems.

## Communications Basics

Every distributed system has software components that communicate over a network. If a mobile banking app requests the user’s current bank account balance, a (very simplified) sequence of communications occurs along the lines of:

-   1. The mobile banking app sends a request over the cellular network addressed to the bank to retrieve the user’s bank balance.
-   2. The request is routed across the Internet to where the bank’s web servers are located.
-   3. The bank’s web server authenticates the request (checks if it originated from the supposed user) and sends a request to a database server for the account balance.
-   4. The database server reads the account balance from disk and returns it to the web server
-   5. The web server sends the balance in a reply message addressed to the app, which is routed over the Internet and the cellular network until the balance magically appears on the screen of the mobile device

It almost sounds simple when you read the above, but in reality, there’s a huge amount of complexity hidden beneath this sequence of communications. Let’s examine some of these complexities in the following sections.

### Communications Hardware

The bank balance request example above will inevitably traverse multiple different networking technologies and devices. The global Internet is a heterogeneous machine, comprising different types of network communications channels and devices that shuttle many millions of messages a second across networks to their intended destinations.

Different types of communications channels exist. The most obvious categorization is wired versus wireless. For each category there are multiple network transmission hardware technologies that can ship bits from one machine to another. Each technology has different characteristics, and the ones we typically care about are speed and range.

For physically wired networks, the two most common types are local area networks (LANs) and wide area networks (WANs). LANs are networks that can connect devices at ‘building scale’, being able to transmit data over a small number (e.g. 1-2) of kilometers. Contemporary LANs can transport between 100 megabits per second (Mbps) to 1 gigabits per second (Gbps). This is known as the network’s bandwidth, or capacity. The time taken to transmit a message across a LAN – the network’s latency – is sub-millisecond with modern LAN technologies.

WANs are networks that traverse the globe and make up what we collectively call the Internet. These long-distance connections are the high speed data ‘pipelines’ connecting cities and countries and continents with fiber optic cables. These cables support a networking technology known as wavelength division multiplexing¹ which makes it possible to transmit up 171 Gbps over 400 different channels, giving more than 70 Terabits per second (Tbps) of total bandwidth for a single fiber link. The fiber cables that span the world normally comprise four or more strands of fiber, giving bandwidth capacity of hundreds of Tbps for each cable.

Latency is more complicated with WANs however. WANs transmit data over 100s to 1000s of kilometers, and the maximum speed that the data can travel in fiber optic cables is the theoretical speed of light. In reality, these cables can’t reach the speed of light, but do get pretty close to it as we can see in Table 3-1.

Table 3-1. WAN Speeds
