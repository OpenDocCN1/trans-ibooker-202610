For this pattern, we must specify the timeout duration, the limit refresh, and the limit for the period. The following listing shows the bootstrap.yml for the licensing service, which contains the retry configuration parameters.

# Listing 7.10 Configuring the retry pattern in the bootstrap.yml

```
//Part of bootstrap.yml omitted for conciseness

resilience4j.ratelimiter:
  instances:
    licenseService:
      timeoutDuration: 1000ms
      limitRefreshPeriod: 5000
      limitForPeriod: 5

Defines the time a thread waits for permission
Defines the number of permissions available during a limit refresh period
Defines the period of a limit refresh
```

The first parameter, timeoutDuration, lets us define the time a thread waits for permission; the default value for this parameter is 5 s (seconds). The second parameter, limitRefreshPeriod, enables us to set the period that limits the refresh. After each period, the rate limiter resets the permissions count back to the limitForPeriod value. The default value for the limitRefreshPeriod is 500 ns (nanoseconds).

The final parameter, limitForPeriod, lets us set the number of permissions available during one refresh period. The default value for the limitForPeriod is 50. The following listing demonstrates how to set up the retry pattern around all calls surrounding the lookup of licensing data from our licensing service.

# Listing 7.11 Creating a bulkhead around getLicensesByOrganization()

```
//Part of LicenseService.java omitted for conciseness

@CircuitBreaker(name= "licenseService",
    fallbackMethod= "buildFallbackLicenseList")
@RateLimiter(name = "licenseService",
    fallbackMethod = "buildFallbackLicenseList")
@Retry(name = "retryLicenseService",
    fallbackMethod = "buildFallbackLicenseList")
@Bulkhead(name= "bulkheadLicenseService",
    fallbackMethod= "buildFallbackLicenseList")
public List<License> getLicensesByOrganization(String organizationId)
    throws TimeoutException {
    logger.debug("getLicensesByOrganization Correlation id: {}",
        UserContextHolder.getContext().getCorrelationID());
    randomlyRunLong();
    return licenseRepository.findByOrganizationId(organizationId);
}

Sets the instance name and fallback method for the rate limiter pattern
```

The main difference between the bulkhead and the rate limiter pattern is that the bulkhead pattern is in charge of limiting the number of concurrent calls (for example, it only allows X concurrent calls at a time). With the rate limiter, we can limit the