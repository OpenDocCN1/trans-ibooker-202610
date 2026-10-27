With the OrganizationRestTemplateClient, we’ll implement the checkRedisCache() method. The following listing shows this code.

# Listing 11.7 Adding checkRedisCache() to OrganizationRestTemplate

```
package com.optimagrowth.license.service.client;
//Rest of imports omitted for conciseness

@Component
public class OrganizationRestTemplateClient {
    @Autowired
    RestTemplate restTemplate;

    @Autowired
    Tracer tracer;

    @Autowired
    OrganizationRedisRepository redisRepository;

    private static final Logger logger =
        LoggerFactory.getLogger(OrganizationRestTemplateClient.class);

    private Organization checkRedisCache
        (String organizationId) {
        try {
            return redisRepository.findById(organizationId).orElse(null);
        } catch (Exception ex){
            logger.error("Error encountered while trying to retrieve organization {} check Redis Cache. Exception {}",
                organizationId, ex);
            return null;
        }
    }

    //Rest of class omitted for conciseness
}
```

Tracer accesses the Spring Cloud Sleuth trace information.

Implements the CheckRedisCache method

# 11.3.6 Visualizing a more complex transaction

What if we want to understand precisely which service dependencies exist between service calls? We can call the licensing service through the gateway and then query Zipkin for licensing service traces with a GET call to the licensing services using the following endpoint. Figure 11.18 shows the detailed trace of this call.

```
http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c8/license/4af05c3b-a0f3-411d-b5ff-892c62710e15
```

In figure 11.18, we can see that the call to the licensing service involves eight HTTP calls. We see the call to the gateway and then from the gateway to the licensing service; the licensing service to the gateway and the gateway to the organization; and finally, the organization to the licensing service using Apache Kafka to update the Redis cache.