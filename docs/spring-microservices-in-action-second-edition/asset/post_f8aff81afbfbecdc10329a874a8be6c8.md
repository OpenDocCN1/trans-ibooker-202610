
## 6.2.1 The architecture of service discovery

To begin our discussion around service discovery, we need to understand four concepts. These general concepts are often shared across all service discovery implementations:

- Service registration—How a service registers with the service discovery agent
- Client lookup of service address—How a service client looks up service information
- Information sharing—How nodes share service information
- Health monitoring—How services communicate their health back to the service discovery agent

The principal objective of service discovery is to have an architecture where our services indicate where they are physically located instead of having to manually configure their location. Figure 6.2 shows how service instances are added and removed, and how they update the service discovery agent and become available to process user requests.

Figure 6.2 shows the flow of the previous four bulleted points (service registration, service discovery lookup, information sharing, and health monitoring) and what typically occurs when we implement a service discovery pattern. In the figure, one or

![](img/3c6dc613426280ca3eafe43dea24391e_182_0.png)

Figure 6.2 As service instances are added or removed, the service discovery nodes are updated and made available to process user requests.

more service discovery nodes have started. These service discovery instances usually don’t have a load balancer that sits in front of them.

As service instances start, they’ll register their physical location, path, and port that one or more service discovery instances can use to access the instances. While each instance of a service has a unique IP address and port, each service instance that comes up registers under the same service ID. A service ID is nothing more than a key that uniquely identifies a group of the same service instances.

A service usually only registers with one service discovery service instance. Most service discovery implementations use a peer-to-peer model of data propagation, where the data around each service instance is communicated to all the other nodes in the cluster. Depending on the service discovery implementation, the propagation mechanism might use a hardcoded list of services to propagate to or use a multicasting protocol like the gossip or infection-style protocol to allow other nodes to “discover” changes in the cluster.

> NOTE If you are interested in knowing more about the gossip or infection-style protocols, we highly recommend you review the following: Consul’s “Gossip Protocol” (https://www.consul.io/docs/internals/gossip.html) or Brian Storti’s post, “SWIM: The scalable membership protocol” (https://www.brianstorti.com/swim/).

Finally, each service instance pushes to or pulls from its status by the service discovery service. Any services failing to return a good health check are removed from the pool of available service instances. Once a service is registered with a service discovery service, it’s ready to be used by an application or service that needs to make use of its capabilities. Different models exist for a client to discover a service.

As a first approach, the client relies solely on the service discovery engine to resolve service locations each time a service is called. With this approach, the service discovery engine is invoked each time a call to a registered microservice instance is made. Unfortunately, this approach is brittle because the service client is completely dependent on the service discovery engine to find and invoke a service.

A more robust approach uses what’s called client-side load balancing. This mechanism uses an algorithm like zone-specific or round-robin to invoke the instances of the calling services. When we say “round-robin algorithm load balancing,” we are referring to a way of distributing client requests across several servers. This consists of forwarding a client request to each of the servers in turn. An advantage of using the client-side load balancer with Eureka is that when a service instance goes down, it is removed from the registry. Once that is done, the client-side load balancer updates itself without manual intervention by establishing constant communication with the registry service. Figure 6.3 illustrates this approach.

- 2 Each time a client wants to call the service, the service consumer looks up the location information for the service from the cache. Usually, client-side caching will use a simple load-balancing algorithm like the round-robin load-balancing algorithm to ensure that service calls are spread across multiple service instances.

- 3 The client then periodically contacts the discovery service and refreshes its cache of service instances. The client cache is eventually consistent, but there’s always a risk that when the client contacts the service discovery instance for a refresh and calls are made, calls might be directed to a service instance that isn’t healthy.

If during the course of calling a service, the service call fails, the local service discovery cache is invalidated and the service discovery client will attempt to refresh its entries from the service discovery agent. Let’s now take the generic service discovery pattern and apply it to our O-stock problem domain.

## 6.2.2 Service discovery in action using Spring and Netflix Eureka

In this section, we will implement service discovery by setting up a service discovery agent and then register two services with the agent. With this implementation, we’ll use the information retrieved by the service discovery to call a service from another service. Spring Cloud offers multiple methods for looking up information from a service discovery agent. We’ll walk through the strengths and weaknesses of each approach.

Again, Spring Cloud makes this type of setup trivial to undertake. We’ll use Spring Cloud and Netflix’s Eureka Service Discovery engine to implement your service discovery pattern. For the client-side load balancing, we’ll use the Spring Cloud Load Balancer.

> NOTE In this chapter, we are not going to use Ribbon. Ribbon was the de facto client-side load balancer for REST-based communications among applications using Spring Cloud. Although Netflix Ribbon client-side load balancing was a stable solution, it has now entered a maintenance mode, so unfortunately, it will not be developed anymore.

In this section, we will explain how to use the Spring Cloud Load Balancer, which is a replacement for Ribbon. Currently, the Spring Cloud Load Balancer is still under active development, so expect new functionalities soon. In the previous two chapters, we kept our licensing service simple and included the organization’s name for the licenses with the license data. In this chapter, we’ll break the organization information into its own service. Figure 6.4 shows the implementation of the client-side caching with Eureka for our O-stock microservices.

When the licensing service is invoked, it will call the organization service to retrieve the organization information associated with the designated organization ID.

3. Periodically, Spring Cloud Load Balancer refreshes its cache of IP addresses.

![](img/3c6dc613426280ca3eafe43dea24391e_186_0.png)

2. When the licensing service calls the organization service, it uses the Load Balancer to see if the organization service IPs are cached locally.

1. As service instances start, they will register their IPs with Eureka.

Figure 6.4 By implementing client-side caching and Eureka with O-stock’s licensing and organization services, you can lessen the load on the Eureka Servers and improve client stability if Eureka becomes unavailable.

The actual resolution of the organization service’s location is held in a service discovery registry. For this example, we’ll register two instances of the organization service with a service discovery registry and then use client-side load balancing to look up and cache the registry in each service instance. Figure 6.4 shows this arrangement:

- 1. As the services are bootstrapped, the licensing and organization services register with the Eureka service. This registration process tells Eureka the physical location and port number of each service instance, along with a service ID for the service being started.
- 2. When the licensing service calls to the organization service, it uses the Spring Cloud Load Balancer to provide client-side load balancing. The Load Balancer contacts the Eureka service to retrieve service location information and then caches it locally.
- 3. Periodically, the Spring Cloud Load Balancer will ping the Eureka service and refresh its local cache of service locations.

Any new organization service instance is now visible to the licensing service locally, while any unhealthy instances are removed from the local cache. We’ll implement this design by setting up our Spring Cloud Eureka service.
