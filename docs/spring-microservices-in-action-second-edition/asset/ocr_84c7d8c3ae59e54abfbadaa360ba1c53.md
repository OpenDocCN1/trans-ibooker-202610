# 1.11 Microservice logging and tracing patterns

The downside of a microservice architecture is that it’s much more difficult to debug, trace, and monitor the issues because one simple action can trigger numerous microservice calls within your application. Further chapters will cover how to implement distributed tracing with Spring Cloud Sleuth, Zipkin, and the ELK Stack. For this reason, we’ll look at the following three core logging and tracing patterns to achieve distributed tracing:

+   - Log correlation—How you tie together all the logs produced between services for a single user transaction. With this pattern, we’ll look at how to implement a correlation ID, which is a unique identifier that’s carried across all service calls in a transaction action and that can be used to tie together log entries produced from each service.
+   - Log aggregation—With this pattern, we’ll look at how to pull together all of the logs produced by your microservices (and their individual instances) into a single queryable database across all the services involved and understand the performance characteristics of the services in the transaction.
+   - Microservice tracing—We’ll explore how to visualize the flow of a client transaction across all the services involved and understand the performance characteristics of the transaction’s services.

Figure 1.15 shows how these patterns fit together. We’ll cover the logging and tracing patterns in greater detail in chapter 11.

![](bbox=[0.13, 0.5, 0.8, 0.83])

Figure 1.15 A well-thought-out logging and tracing strategy makes debugging transactions across multiple services manageable.