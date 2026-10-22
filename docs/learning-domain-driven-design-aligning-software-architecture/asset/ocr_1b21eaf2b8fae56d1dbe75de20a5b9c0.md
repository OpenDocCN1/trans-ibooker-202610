Another example is a bounded context that must process data from multiple other contexts and implement complex business logic to process all the data. In this case, it can be beneficial to decouple the integration and business logic complexities by fronting the bounded context with an anticorruption layer that aggregates data from all other bounded contexts, as shown in Figure 9-10.

![](bbox=[0.13, 0.24, 0.87, 0.56])

Figure 9-10. Simplifying the integration model using the anticorruption layer pattern

# Integrating Aggregates

In Chapter 6, we discussed that one of the ways aggregates communicate with the rest of the system is by publishing domain events. External components can subscribe to these domain events and execute their logic. But how are domain events published to a message bus?

Before we get to the solution, let’s examine a few common mistakes in the event publishing process and the consequences of each approach. Consider the following code: