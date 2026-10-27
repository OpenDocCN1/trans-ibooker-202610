# When bad things happen: Resiliency patterns with Spring Cloud and Resilience4j

![](bbox=[0.772, 0.086, 0.956, 0.33])

# This chapter covers

+   - Implementing circuit breakers, fallbacks, and bulkheads
+   - Using the circuit breaker pattern to conserve client resources
+   - Using Resilience4j when a remote service fails
+   - Implementing Resilience4j’s bulkhead pattern to segregate remote resource calls
+   - Tuning Resilience4j circuit breaker and bulkhead implementations
+   - Customizing Resilience4j’s concurrency strategy

All systems, especially distributed systems, experience failure. How we build our applications to respond to that failure is a critical part of every software developer’s job. However, when it comes to building resilient systems, most software engineers only take into account the complete failure of a piece of infrastructure or critical