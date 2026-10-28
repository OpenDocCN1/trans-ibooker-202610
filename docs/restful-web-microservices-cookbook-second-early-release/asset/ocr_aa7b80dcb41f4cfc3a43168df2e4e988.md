# Related

+   - Chapter 2
+   - Chapter 3
+   - Chapter 4
+   - [Link to Come]

# 4.17 Using Semantic Proxies to Access Non-Compliant Services

At the heart of the recipes in this book is the notion of supporting resilient API clients through the use of hypermedia-based service interfaces. But there are times when the services we need to expose to the network do not implement hypermedia interactions (e.g. an FTP upload service). And, in some cases, we don’t have the ability to change those services since they are operated by a third-party outside our own IT ecosystem.

# Problem

How can we safely expose non-compliant services in our RESTful Web Microservices ecosystem? When is it a good choice to create a local proxy service that wraps other, possibly remote third-party services? What does it take to create stable, reliable proxies for services we did not design and do not maintain?

# Solution

There are times when we need the functionality of an existing service but the service design was not meant to support adaptable, evolvable interfaces in a RESTful ecosystem. In these case, we need to create local, compliant service proxies that act as “translation” devices between those non-compliant services and the rest of our service ecosystem.

Typically, we need to design the *desired* service interface and then work up our own local code within the API “wrapper” that translates RESTful