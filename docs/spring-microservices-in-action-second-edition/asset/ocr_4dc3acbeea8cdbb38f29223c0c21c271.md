work as well. This is because it doesn’t scale effectively and isn’t cost effective. Let’s now look at how you can implement a robust service discovery mechanism for your cloud-based applications.

# 6.2 Service discovery in the cloud

The solution for a cloud-based microservice environment is to use a service discovery mechanism that’s

+   - Highly available—Service discovery needs to be able to support a “hot” clustering environment where service lookups can be shared across multiple nodes in a service discovery cluster. If a node becomes unavailable, other nodes in the cluster should be able to take over.
+   
+     A cluster can be defined as a group of multiple server instances. All instances of this environment have an identical configuration and work together to provide high availability, reliability, and scalability. A cluster combined with a load balancer can offer failover to prevent service interruptions and session replication to store session data.

+   - Peer-to-peer—Each node in the service discovery cluster shares the state of a service instance.

+   - Load balanced—Service discovery needs to dynamically load balance requests across all service instances. This ensures that the service invocations are spread across all the service instances managed by it. In many ways, service discovery replaces the more static, manually managed load balancers found in many early web application implementations.

+   - Resilient—The service discovery’s client should cache service information locally. Local caching allows for gradual degradation of the service discovery feature so that if the service discovery service becomes unavailable, applications can still function and locate the services based on the information maintained in their local cache.

+   - Fault tolerant—Service discovery needs to detect when a service instance isn’t healthy and remove that instance from the list of available services that can take client requests. It should detect these faults with services and take action without human intervention.

In the following sections, we’re going to

+   - Walk you through the conceptual architecture of how a cloud-based service discovery agent works

+   - Show you how client-side caching and load balancing allows the service to continue to function even when the service discovery agent is unavailable

+   - Show you how to implement service discovery using Spring Cloud and Netflix’s Eureka service discovery agents