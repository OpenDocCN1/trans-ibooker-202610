+   - Sets up a separate thread pool for the `getLicensesByOrganization()` call
+   - Creates the bulkhead configuration in the `bootstrap.yml` file
+   - With the semaphore approach, sets `maxConcurrentCalls` and `maxWaitDuration`
+   - With the thread pool approach, sets `maxThreadPoolSize`, `coreThreadPoolSize`, `queueCapacity`, and `keepAliveDuration`

The following listing shows the bootstrap.yml for the licensing service with these bulkhead configuration parameters.

# Listing 7.6  Configuring the bulkhead pattern for the licensing service

```
```yaml
# Part of bootstrap.yml omitted for conciseness

resilience4j.bulkhead:
  instances:
    bulkheadLicenseService:
      maxWaitDuration: 10ms
      maxConcurrentCalls: 20

resilience4j.thread-pool-bulkhead:
  instances:
    bulkheadLicenseService:
      maxThreadPoolSize: 1
      coreThreadPoolSize: 1
      queueCapacity: 1
      keepAliveDuration: 20ms
```
```

![](bbox=[0.45, 0.31, 0.797, 0.53])

Resilience4j also lets us customize the behavior of the bulkhead patterns through the application’s properties. Like the circuit breaker, we can create as many instances as we want, and each instance can have different configurations. Listing 7.6 contains the following properties:

+   - `maxWaitDuration`—Sets the maximum amount of time to block a thread when entering a bulkhead. The default value is 0.
+   - `maxConcurrentCalls`—Sets the maximum number of concurrent calls allowed by the bulkhead. The default value is 25.
+   - `maxThreadPoolSize`—Sets the maximum thread pool size. The default value is `Runtime.getRuntime().availableProcessors()`.
+   - `coreThreadPoolSize`—Sets the core thread pool size. The default value is `Runtime.getRuntime().availableProcessors()`.
+   - `queueCapacity`—Sets the capacity of the queue. The default value is 100.
+   - `keepAliveDuration`—Sets the maximum time that idle threads will wait for new tasks before terminating. This happens when the number of threads is higher than the number of core threads. The default value is 20 ms.