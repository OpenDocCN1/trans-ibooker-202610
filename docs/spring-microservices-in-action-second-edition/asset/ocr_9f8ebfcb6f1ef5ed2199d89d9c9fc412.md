# USING REDIS AND THE LICENSING SERVICE TO STORE AND READ ORGANIZATION DATA

Now that we have the code in place to perform operations with Redis, we can modify our licensing service so that every time the licensing service needs the organization data, it checks the Redis cache before calling the organization service. You’ll find the logic for doing this in the OrganizationRestTemplateClient class in the class file `/service/src/main/java/com/optimagrowth/license/service/client/OrganizationRestTemplateClient.java`. The next listing shows this class.

# Listing 10.15 Implementing cache logic with OrganizationRestTemplateClient

```
```java
package com.optimagrowth.license.service.client;
//Imports removed for conciseness

@Component
public class OrganizationRestTemplateClient {
    @Autowired
    RestTemplate restTemplate;
    @Autowired
    OrganizationRedisRepository redisRepository;
    private static final Logger logger =
        LoggerFactory.getLogger(OrganizationRestTemplateClient.class);

    private Organization checkRedisCache(String organizationId) {
        try {
            return redisRepository
                .findById(organizationId)
                .orElse(null);
        } catch (Exception ex) {
            logger.error("Error encountered while trying to retrieve organization{} check Redis Cache. Exception {}",
                organizationId, ex);
            return null;
        }
    }

    private void cacheOrganizationObject(Organization organization) {
        try {
            redisRepository.save(organization);
        } catch (Exception ex) {
            logger.error("Unable to cache organization {} in Redis. Exception {}",
                organization.getId(), ex);
        }
    }

    public Organization getOrganization(String organizationId){
        logger.debug("In Licensing Service.getOrganization: {}",
            UserContext.getCorrelationId());

        Organization organization = checkRedisCache(organizationId);
        if (organization != null) {
            logger.debug("I have successfully retrieved an organization {} from the redis cache: {}", organizationId,
                organization);
        }
```
```

> Autowires OrganizationRedisRepository in OrganizationRestTemplateClient

> Tries to retrieve an Organization class with its organization ID from Redis

> Saves the organization in Redis

> If you can’t retrieve data from Redis, calls the organization service to get the data from the source database to later save it in Redis