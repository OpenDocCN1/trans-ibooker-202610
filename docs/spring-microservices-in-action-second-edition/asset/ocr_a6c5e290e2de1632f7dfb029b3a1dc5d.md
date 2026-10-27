gateway allows you to centralize the collection of many of your basic metrics, like the number of times the service is invoked and the service response times.

# Wait—isn’t a service gateway a single point of failure and a potential bottleneck?

Earlier in chapter 6, when we introduced Eureka, we talked about how centralized load balancers can be a single point of failure and a bottleneck for your services. A service gateway, if not implemented correctly, can carry the same risk. Keep the following in mind as you build your service gateway implementation:

+   - *Load balancers are useful when placed in front of individual groups of services.* In this case, a load balancer sitting in front of multiple service gateway instances is an appropriate design and ensures that your service gateway implementation can scale as needed. But having a load balancer sitting in front of all your service instances isn’t a good idea because it becomes a bottleneck.
+   - *Keep any code you write for your service gateway stateless.* Don’t store any information in memory for the service gateway. If you aren’t careful, you can limit the scalability of the gateway. Then, you will need to ensure that the data gets replicated across all service gateway instances.
+   - *Keep the code you write for your service gateway light.* The service gateway is the “chokepoint” for your service invocation. Complex code with multiple database calls can be the source of difficult-to-track performance problems in the service gateway.

Let’s now look at how to implement a service gateway using Spring Cloud Gateway. We’ll use Spring Cloud Gateway because it is the preferred API gateway from the Spring Cloud team. This implementation is built on Spring 5 and is a nonblocking gateway that integrates much easier with the other Spring Cloud projects we’ve used throughout the book.

# 8.2 Introducing Spring Cloud Gateway

Spring Cloud Gateway is the API gateway implementation built on Spring framework 5, Project Reactor, and Spring Boot 2.0. This gateway is a nonblocking gateway. What does nonblocking mean? Nonblocking applications are written in such a way that the main threads are never blocked. Instead, these threads are always available to serve requests and to process them asynchronously in the background to return a response once processing is done. Spring Cloud Gateway offers several capabilities, including

+   - *Mapping the routes for all the services in your application to a single URL.* The Spring Cloud Gateway isn’t limited to a single URL, however. Actually, with it, we can define multiple route entry points, making route mapping extremely fine-grained (each service endpoint gets its own route mapping). But the first and most common use case is to build a single entry point through which all service client calls will flow.