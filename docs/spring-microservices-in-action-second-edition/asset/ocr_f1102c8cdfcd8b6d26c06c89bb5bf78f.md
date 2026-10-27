# 6.5 Using service discovery to look up a service

In this section, we will explain how we can have the licensing service call the organization service without having direct knowledge of the location of any of the organization services. The licensing service will look up the physical location of the organization using Eureka.

For our purposes, we’ll look at three different Spring/Netflix client libraries in which a service consumer can interact with the Spring Cloud Load Balancer. These libraries will move from the lowest level of abstraction for interacting with the Load Balancer to the highest. The libraries we’ll explore include

+   - Spring Discovery Client
+   - Spring Discovery Client–enabled REST template
+   - Netflix Feign client

Let’s walk through each of these clients and see their use in the context of the licensing service. Before we start into the specifics of the client, we wrote a few convenience classes and methods in the code. You can play with the different client types using the same service endpoint.

First, we’ve modified the src/main/java/com/optimagrowth/license/controller/LicenseController.java class to include a new route for the licensing services. This new route allows you to specify the type of client you want to use to invoke the service. This is a helper route so that as we explore each of the different methods for invoking the organization’s service via the Load Balancer, you can try each mechanism through a single route. The following listing shows the code for the new route in the LicenseController class.

# Listing 6.9 Calling the licensing service with different REST clients

```
@RequestMapping(value="/{licenseId}/{clientType}",
        method = RequestMethod.GET)
public License getLicensesWithClient(
    @PathVariable("organizationId") String organizationId,
    @PathVariable("licenseId") String licenseId,
    @PathVariable("clientType") String clientType) {
        return licenseService.getLicense(organizationId,
            licenseId, clientType);
}

The clientType parameter determines the type of Spring REST client to use.
```

In listing 6.9, the clientType parameter passed on the route drives the type of client we’re going to use in the code examples. The specific types we can pass in on this route include the following:

+   - Discovery—Uses the Discovery Client and a standard Spring RestTemplate class to invoke the organization service
+   - Rest—Uses an enhanced Spring RestTemplate to invoke the Load Balancer service
+   - Feign—Uses Netflix’s Feign client library to invoke a service via the Load Balancer