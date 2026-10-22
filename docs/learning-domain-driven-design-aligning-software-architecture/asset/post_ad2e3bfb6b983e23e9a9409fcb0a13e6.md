
bounded context. Suppose the downstream bounded context cannot conform to the upstream bounded context’s model. In this case, a more elaborate technical solution is required that can facilitate communication by translating the bounded contexts’ models.

This translation can be handled by one, or sometimes both, sides: the downstream bounded context can adapt the upstream bounded context’s model to its needs using an anticorruption layer (ACL), while the upstream bounded context can act as an open-host service (OHS) and protect its consumers from changes to its implementation model by using an integration-specific published language. Since the translation logic is similar for both the anticorruption layer and the open-host service, this chapter covers the implementation options without differentiating between the patterns and mentions the differences only in exceptional cases.

The model’s translation logic can be either stateless or stateful. Stateless translation happens on the fly, as incoming (OHS) or outgoing (ACL) requests are issued, while stateful translation involves a more complicated translation logic that requires a database. Let’s see design patterns for implementing both types of model translation.

## Stateless Model Translation

For stateless model translation, the bounded context that owns the translation (OHS for upstream, ACL for downstream) implements the proxy design pattern to interject the incoming and outgoing requests and map the source model to the bounded context’s target model. This is depicted in Figure 9-1.

![](img/b742a71dea5d5dd509847f82600a6ad1_210_0.png)

Implementation of the proxy depends on whether the bounded contexts are communicating synchronously or asynchronously.

## Synchronous

The typical way to translate models used in synchronous communication is to embed the transformation logic in the bounded context’s codebase, as shown in Figure 9-2. In an open-host service, translation to the public language takes place when processing incoming requests, and in an anticorruption layer, it occurs when calling the upstream bounded context.

![](img/b742a71dea5d5dd509847f82600a6ad1_211_0.png)

In some cases, it can be more cost-effective and convenient to offload the translation logic to an external component such as an API gateway pattern. The API gateway component can be an open source software-based solution such as Kong or KrakenD, or it can be a cloud vendor’s managed service such as AWS API Gateway, Google Apigee, or Azure API Management.

For bounded contexts implementing the open-host pattern, the API gateway is responsible for converting the internal model into the integration-optimized published language. Moreover, having an explicit API gateway can alleviate the process of managing and serving multiple versions of the bounded context’s API, as depicted in Figure 9-3.

![](img/b742a71dea5d5dd509847f82600a6ad1_212_0.png)

Figure 9-3. Exposing different versions of the published language

Anticorruption layers implemented using an API gateway can be consumed by multiple downstream bounded contexts. In such cases, the anticorruption layer acts as an integration-specific bounded context, as shown in Figure 9-4.

![](img/b742a71dea5d5dd509847f82600a6ad1_212_1.png)

Figure 9-4. Shared anticorruption layer

Such bounded contexts, which are mainly in charge of transforming models for more convenient consumption by other components, are often referred to as interchange contexts.

## Asynchronous

To translate models used in asynchronous communication you can implement a message proxy: an intermediary component subscribing to messages coming from the source bounded context. The proxy will apply the required model transformations and forward the resultant messages to the target subscriber (see Figure 9-5).

![](img/b742a71dea5d5dd509847f82600a6ad1_213_0.png)

Figure 9-5. Translating models in asynchronous communication

In addition to translating the messages’ model, the intercepting component can also reduce the noise on the target bounded context by filtering out irrelevant messages.

Asynchronous model translation is essential when implementing an open host service. It’s a common mistake to design and expose a published language for the model’s objects and allow domain events to be published as they are, thereby exposing the bounded context’s implementation model. Asynchronous translation can be used to intercept the domain events and convert them into a published language, thus providing better encapsulation of the bounded context’s implementation details (see Figure 9-6).

Moreover, translating messages to the published language enables differentiating between private events that are intended for the bounded context’s internal needs and public events that are designed for integration with other bounded contexts. We’ll revisit and expand on the topic of private/public events in Chapter 15, where we discuss the relationship between domain-driven design and event-driven architecture.

![](img/b742a71dea5d5dd509847f82600a6ad1_213_1.png)

Figure 9-6. Domain events in a published language

## Stateful Model Translation

For more significant model transformations—for example, when the translation mechanism has to aggregate the source data or unify data from multiple sources into a single model—a stateful translation may be required. Let’s discuss each of these use cases in detail.

## Aggregating incoming data

Let’s say a bounded context is interested in aggregating incoming requests and processing them in batches for performance optimization. In this case, aggregation may be required both for synchronous and asynchronous requests (see Figure 9-7).

![](img/b742a71dea5d5dd509847f82600a6ad1_214_0.png)

Figure 9-7. Batching requests

Another common use case for aggregation of source data is combining multiple fine-grained messages into a single message containing the unified data, as depicted in Figure 9-8.

![](img/b742a71dea5d5dd509847f82600a6ad1_215_0.png)

Figure 9-8. Unifying incoming events

Model transformation that aggregates incoming data cannot be implemented using an API gateway, and thus requires more elaborate, stateful processing. To track the incoming data and process it accordingly, the translation logic requires its own persistent storage (see Figure 9-9).

![](img/b742a71dea5d5dd509847f82600a6ad1_215_1.png)

Figure 9-9. Stateful model transformation

In some use cases, you can avoid implementing a custom solution for a stateful translation by using off-the-shelf products; for example, a stream-process platform (Kafka, AWS Kinesis, etc.), or a batching solution (Apache NiFi, AWS Glue, Spark, etc.).

## Unifying multiple sources

A bounded context may need to process data aggregates from multiple sources, including other bounded contexts. A typical example for this is the backend-for-frontend pattern,¹ in which the user interface has to combine data originating from multiple services.

Another example is a bounded context that must process data from multiple other contexts and implement complex business logic to process all the data. In this case, it can be beneficial to decouple the integration and business logic complexities by fronting the bounded context with an anticorruption layer that aggregates data from all other bounded contexts, as shown in Figure 9-10.

![](img/b742a71dea5d5dd509847f82600a6ad1_216_0.png)

Figure 9-10. Simplifying the integration model using the anticorruption layer pattern

## Integrating Aggregates

In Chapter 6, we discussed that one of the ways aggregates communicate with the rest of the system is by publishing domain events. External components can subscribe to these domain events and execute their logic. But how are domain events published to a message bus?

Before we get to the solution, let’s examine a few common mistakes in the event publishing process and the consequences of each approach. Consider the following code:
