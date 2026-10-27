address of the service instance) and a logical name that an application can use to look up the service. Certain service discovery agents also require a URL sent back to the registering service, which can be used by the service discovery agent to perform health checks. The service client then communicates with the discovery agent to look up the service’s location.

![](bbox=[0.173, 0.194, 0.872, 0.584])

# 3.4.4 Communicating a microservice’s health

A service discovery agent doesn’t act only as a traffic cop that guides the client to the location of the service. In a cloud-based microservice application, you’ll often have multiple instances of a running service. Sooner or later, one of those service instances will fail. The service discovery agent monitors the health of each service instance registered with it and removes any failing service instances from its routing tables to ensure that clients aren’t sent a service instance that has failed.

After a microservice comes up, the service discovery agent will continue to monitor and ping the health check interface to ensure that that service is available. This is step 4 in figure 3.9; figure 3.13 provides context for this step.