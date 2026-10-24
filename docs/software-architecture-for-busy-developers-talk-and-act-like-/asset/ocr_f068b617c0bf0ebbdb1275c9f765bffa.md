![](bbox=[0.2, 0.1, 0.8, 0.4])

Figure 6.9 – SAGA choreography-based pattern

If one of the participants fails, it will publish its failure event, which will be captured by the others to invalidate their local transaction. However, the choreography pattern makes it harder to understand the full picture when a single transaction involves many participants. You can see an example of SAGA using Azure services at https://github.com/Azure-Samples/saga-orchestration-serverless. Now, let's look at CQRS.

# Command Query Request Segregation (CQRS)

In a nutshell, CQRS is a pattern that segregates commands (something that mutates state) and queries (something that returns data without mutating state). CQRS is often confused with Command Query Segregation (CQS). The latter is the strict definition of the preceding definition. This is because you can segregate queries and commands even when targeting the same data store. True CQRS implies request segregation, which means that very different systems and data stores can handle requests and commands. This gives you the ability to increase the speed of both write and read operations and be able to scale them independently. By splitting the data stores (or having read-only and write-only partitions), heavy read operations do not impact the write performance and vice versa.