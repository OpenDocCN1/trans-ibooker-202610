
# 6.5 Using service discovery to look up a service

In this section, we will explain how we can have the licensing service call the organization service without having direct knowledge of the location of any of the organization services. The licensing service will look up the physical location of the organization using Eureka.

For our purposes, we’ll look at three different Spring/Netflix client libraries in which a service consumer can interact with the Spring Cloud Load Balancer. These libraries will move from the lowest level of abstraction for interacting with the Load Balancer to the highest. The libraries we’ll explore include

- Spring Discovery Client
- Spring Discovery Client–enabled REST template
- Netflix Feign client

Let’s walk through each of these clients and see their use in the context of the licensing service. Before we start into the specifics of the client, we wrote a few convenience classes and methods in the code. You can play with the different client types using the same service endpoint.

First, we’ve modified the src/main/java/com/optimagrowth/license/controller/LicenseController.java class to include a new route for the licensing services. This new route allows you to specify the type of client you want to use to invoke the service. This is a helper route so that as we explore each of the different methods for invoking the organization’s service via the Load Balancer, you can try each mechanism through a single route. The following listing shows the code for the new route in the LicenseController class.

## Listing 6.9 Calling the licensing service with different REST clients

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
```

The clientType parameter determines the type of Spring REST client to use.

In listing 6.9, the clientType parameter passed on the route drives the type of client we’re going to use in the code examples. The specific types we can pass in on this route include the following:

- Discovery—Uses the Discovery Client and a standard Spring RestTemplate class to invoke the organization service
- Rest—Uses an enhanced Spring RestTemplate to invoke the Load Balancer service
- Feign—Uses Netflix’s Feign client library to invoke a service via the Load Balancer

**NOTE** Because we’re using the same code for all three types of clients, you might see situations where you’ll see annotations for specific clients even when they don’t seem to be needed. For example, you’ll see both the `@EnableDiscoveryClient` and `@EnableFeignClients` annotations in the code, even when the text is only explaining one of the client types. This is so that we can use one codebase for our examples. We’ll call out these redundancies and code when these are encountered. The idea is that, as always, you choose the one that best suits your needs.

In the class `src/main/java/com/optimagrowth/license/service/LicenseService.java`, we added a simple `retrieveOrganizationInfo()` method that will resolve based on the `clientType` passed to the route. This client type is used to look up an organization service instance. The `getLicense()` method on the `LicenseService` class uses the `retrieveOrganizationInfo()` method to retrieve the organization data from a Postgres database. The following listing shows the code for the `getLicense()` service in the `LicenseService` class.

## Listing 6.10 The `getLicense()` function using multiple methods for a REST call

```
public License getLicense(String licenseId, String organizationId, String
        clientType){
    License license = licenseRepository.findByOrganizationIdAndLicenseId
            (organizationId, licenseId);
    if (null == license) {
        throw new IllegalArgumentException(String.format(
            messages.getMessage("license.search.error.message", null, null),
            licenseId, organizationId));
    }
    Organization organization = retrieveOrganizationInfo(organizationId,
            clientType);
    if (null != organization) {
        license.setOrganizationName(organization.getName());
        license.setContactName(organization.getContactName());
        license.setContactEmail(organization.getContactEmail());
        license.setContactPhone(organization.getContactPhone());
    }
    return license.withComment(config.getExampleProperty());
}
```

You can find each of the clients we built using the Spring Discovery Client, the Spring RestTemplate class, or the Feign libraries in the `src/main/java/com/optimagrowth/license/service/client` package of the licensing service. To call the `getLicense()` services with the different clients, you must call the following GET endpoint:

```
http://<licensing service Hostname/IP>:<licensing service Port>/v1/
organization/<organizationID>/license/<licenseID>/<client type (feign,
discovery, rest)>
```

### 6.5.1 Looking up service instances with Spring Discovery Client

The Spring Discovery Client offers the lowest level of access to the Load Balancer and the services registered within it. Using the Discovery Client, you can query for all the services registered with the Spring Cloud Load Balancer client and their corresponding URLs.

Next, we’ll build a simple example of using the Discovery Client to retrieve one of the organization service URLs from the Load Balancer and then call the service using a standard RestTemplate class. To use the Discovery Client, we first need to annotate the src/main/java/com/optimagrowth/license/LicenseServiceApplication.java class with the @EnableDiscoveryClient annotation as shown in the following listing.

## Listing 6.11 Setting up the bootstrap class to use the Eureka Discovery Client

```
package com.optimagrowth.license;
@SpringBootApplication
@RefreshScope
@EnableDiscoveryClient
public class LicenseServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(LicenseServiceApplication.class, args);
    }
}
```

Activates the Eureka Discovery Client

The @EnableDiscoveryClient is the trigger for Spring Cloud to enable the application to use the Discovery Client and the Spring Cloud Load Balancer libraries. Now, let’s look at our implementation of the code that calls the organization service via the Spring Discovery Client. The following listing shows this implementation. You can find this code in the src/main/java/com/optimagrowth/license/service/client/OrganizationDiscoveryClient.java file.

## Listing 6.12 Using the Discovery Client to look up information

```
@Component
public class OrganizationDiscoveryClient {

    @Autowired
    private DiscoveryClient discoveryClient;

    public Organization getOrganization(String organizationId) {
        RestTemplate restTemplate = new RestTemplate();
        List<ServiceInstance> instances =
            discoveryClient.getInstances("organization-service");

        if (instances.size()==0) return null;
        String serviceUri = String.format(
            "%s/v1/organization/%s",instances.get(0)
            .getUri().toString(),
            organizationId);
    }

Injects the Discovery Client into the class
Gets a list of all the instances of the organization services
Retrieves the service endpoint
```

```
ResponseEntity<Organization> restExchange =
        restTemplate.exchange(
        serviceUri, HttpMethod.GET,
        null, Organization.class, organizationId);

    return restExchange.getBody();
}
}
```

The first item of interest in the code is the `DiscoveryClient` class. You use this class to interact with the Spring Cloud Load Balancer. Then, to retrieve all instances of the organization services registered with Eureka, you use the `getInstances()` method, passing in the service key that you’re looking for to retrieve a list of `ServiceInstance` objects. The `ServiceInstance` class holds information about a specific instance of a service, including its hostname, port, and URI.

In listing 6.12, you take the first `ServiceInstance` class in your list to build a target URL that can then be used to call your service. Once you have a target URL, you can use a standard Spring `RestTemplate` to call your organization service and retrieve the data.

> ### The Discovery Client and real life
> 
> You should only use the Discovery Client when your service needs to query the Load Balancer to understand what services and service instances are registered with it. There are several problems with the code in listing 6.12, including these:
> 
> - *You aren’t taking advantage of the Spring Cloud client-side Load Balancer.* By calling the Discovery Client directly, you get a list of services, but it becomes your responsibility to choose which returned service instance you’re going to invoke.
> - *You’re doing too much work.* In the code, you have to build the URL that you’ll use to call your service. It’s a small thing, but every piece of code that you can avoid writing is one less piece of code that you have to debug.
> 
> Observant Spring developers might have also noticed that we directly instantiated the `RestTemplate` class in the code. This is antithetical to usual Spring REST invocations because you’ll usually have the Spring framework inject the `RestTemplate` class via the `@Autowired` annotation.
> 
> We instantiated the `RestTemplate` class in listing 6.12. Once we’ve enabled the Spring Discovery Client in the application class via `@EnableDiscoveryClient`, all REST templates managed by the Spring framework will have a Load Balancer-enabled interceptor injected into those instances. This will change how URLs are created with the `RestTemplate` class. Directly instantiating `RestTemplate` allows you to avoid this behavior.
