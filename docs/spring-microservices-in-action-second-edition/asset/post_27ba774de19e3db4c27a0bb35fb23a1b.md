
### 9.5.1 Use HTTPS secure sockets layer (SSL) for all service communication

In all the code examples in this book, we’ve used HTTP because HTTP is a simple protocol and doesn’t require setting up on every service before you start using the service. In a production environment, however, your microservices should communicate only through the encrypted channels provided through HTTPS and SSL. Note that the configuration and setup of HTTPS can be automated through your DevOps scripts.

### 9.5.2 Use a service gateway to access your microservices

The individual servers, service endpoints, and ports your services are running on should never be directly accessible to the client. Instead, use a service gateway to act as an entry point and gatekeeper for your service calls. Configure the network layer on the operating system or container that your microservices are running in to only accept traffic from the service gateway. Remember, the service gateway can act as a Policy Enforcement Point (PEP), which can be enforced in all services.

Putting service calls through a service gateway allows you to be consistent in how you’re securing and auditing your services. A service gateway also allows you to lock down what port and endpoints you’re going to expose to the outside world.

### 9.5.3 Zone your services into a public API and private API

Security, in general, is all about building layers of access and enforcing the concept of *least privilege*. Least privilege implies that a user should have only the bare minimum network access and privileges to do their day-to-day job. To this end, you should implement least privilege by separating your services into two distinct zones: public and private.

The public zone contains all the public APIs that will be consumed by your clients (in this book’s examples, the O-stock application). Public API microservices should carry out narrow tasks that are workflow oriented. These microservices tend to be service aggregators, pooling data and carrying out tasks across multiple services. Public microservices should also be behind their own service gateway and have their own authentication service for performing authentication and authorization. Access to public services by client applications should go through a single route protected by the service gateway. Also, the public zone should have its own authentication service.

The private zone acts as a wall to protect your core application functionality and data. It should only be accessible through a single, well-known port and should be locked down to only accept network traffic from the network subnet where the private services are running. The private zone should have its own gateway and authentication service. Public API services should authenticate against the private zone’s authentication service. All application data should at least be in the private zone’s network subnet and only accessible by microservices residing in the private zone.

### 9.5.4 Limit the attack surface of your microservices by locking down unneeded network ports

Many developers don’t take a hard look at the absolute minimum number of ports they need to open for their services to function. Configure the operating system your service is running on to only allow inbound and outbound access to the ports or a piece of infrastructure needed by your service (monitoring, log aggregation).

Don’t focus only on inbound access ports. Many developers forget to lock down their outbound ports. Locking down your outbound ports can prevent data from being leaked out of your service if an attacker has compromised the service itself. Also, make sure you look at network port access in both your public and private API zones.

## Summary

- OAuth2 is a token-based authorization framework that provides different mechanisms for protecting web service calls. These mechanisms are called *grants*.
- OpenID Connect (OIDC) is a layer on top of the OAuth2 framework that provides authentication and profile information about who is logged in to the application (identity).
- Keycloak is an open source identity and access management solution for microservices and applications. The main objective of Keycloak is to facilitate the protection of the services and applications with little or no code.
- Each application can have its own Keycloak application name and secret key.
- Each service must define what actions a role can take.
- Spring Cloud Security supports the JSON Web Token (JWT) specification. With JWT, we can inject custom fields into the specification.
- Securing our microservices involves more than just using authentication and authorization.
- In a production environment, we should use HTTPS to encrypt all calls between services.
- Use a service gateway to narrow the number of access points through which a service can be reached.
- Limit the attack surface for a service by limiting the number of inbound and outbound ports on the operating system that the service is running on.

# Event-driven architecture
with Spring Cloud Stream

## This chapter covers

- Understanding event-driven architecture processing and its relevance
- Using Spring Cloud Stream to simplify event processing
- Configuring Spring Cloud Stream
- Publishing messages with Spring Cloud Stream and Kafka
- Consuming messages with Spring Cloud Stream and Kafka
- Implementing distributed caching with Spring Cloud Stream, Kafka, and Redis

Human beings are always in a state of motion as they interact with their environment. Typically, their conversations aren’t synchronous, linear, or as narrowly defined as a request-response model. It’s something more like message-driven, where we’re constantly sending and receiving messages from the things around us. As we receive messages, we react to those messages, often interrupting the primary task that we’re working on.

This chapter is about how to design and implement our Spring-based microservices to communicate with other microservices using asynchronous messages. Using asynchronous messages to communicate between applications isn’t new. What’s new is the concept of using messages to communicate events representing changes in state. This concept is called event-driven architecture (EDA). It’s also known as message-driven architecture (MDA). What an EDA-based approach allows us to do is to build highly decoupled systems that can react to changes without being tightly coupled to specific libraries or services. When combined with microservices, EDA allows us to quickly add new functionality to our application by merely having the service listen to the stream of events (messages) being emitted by our application.

The Spring Cloud project makes it trivial to build message-based solutions through the Spring Cloud Stream subproject. Spring Cloud Stream allows us to easily implement message publication and consumption while shielding our services from the implementation details associated with the underlying messaging platform.

## 10.1 The case for messaging, EDA, and microservices

Why is messaging important in building microservice-based applications? To answer that question, let’s start with an example. For this, we’ll use the two services that we’ve used throughout the book: our licensing and organization services.

Let’s imagine that after these services are deployed to production, we find that the licensing service calls are taking an exceedingly long time when looking up information from the organization service. When we look at the usage patterns of the organization data, we find that the organization data rarely changes and that most of the data reads from the organization service are done by the primary key of the organization record. If we could cache the reads for the organization data without having to incur the cost of accessing a database, we could significantly improve the response time of the licensing service calls. To implement a caching solution, we need to consider the following three core requirements:

- 1. *Cached data needs to be consistent across all instances of the licensing service.* This means that we can’t cache the data locally within the licensing service because we want to guarantee that the same organization data is read regardless of the service instance hitting it.
- 2. *We cannot cache the organization data within the memory of the container hosting the licensing service.* The run-time container hosting our service is often restricted in size and can obtain data using different access patterns. A local cache can introduce complexity because we have to guarantee our local cache is in sync with all of the other services in the cluster.
- 3. *When an organization record changes via an update or delete, we want the licensing service to recognize that there has been a state change in the organization service.* The licensing service should then invalidate any cached data it has for that specific organization and evict it from the cache.
