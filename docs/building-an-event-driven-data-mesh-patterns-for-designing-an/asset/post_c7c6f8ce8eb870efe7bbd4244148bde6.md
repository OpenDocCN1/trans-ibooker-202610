
![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_225_0.png)

Figure 9-6. A self-service UI showing both a Hive table and Kafka topic mode for a single data product

## Summary

A data mesh is inherently multimodal, and data products can be provided via a variety of means. Event streams remain the best option for the majority of data products, as it is far easier to power both operational and analytical use cases through a stream than a batch of files at rest.

Batch-based compute has been the predominant way to build data pipelines and provide analytical results since the inception of big data. Replacing the batch-computed files with an event-stream source is one of the most common first integration steps. A sink connector recomposes the event stream into a batch of files, matching the format and aggregation requirements. The end result is a set of event streams that can power real-time operational and analytical systems while simultaneously powering the sources that power the existing batch-based data pipelines.

How you choose to classify a batch of files created by a sink connector is up to you. You may choose to consider it to be two modes of the same data product, but you must ensure singular ownership and responsibility. You may also choose to consider them different data products. An event stream may be source-aligned, while the data sunk to the cloud storage may be an hourly aggregate-aligned data product. Lineage and dependency tracking remain important for discovery of closely related data products.

Self-service platform support for multimodal data products remains a key requirement. Aside from discovery, you’ll need to ensure that your data mesh users can register their services as consumers and manage the publication of their own nonstreaming data products. Metadata requirements will vary from product mode to product mode, as will the means of secure access, information handling, scaling, and evolution. Ensure that representatives from your federated governance team have carefully evaluated and prioritized the features necessary to support your data product modes.

There’s one more subject to deal with as we approach the end of the book. Distributed systems must often deal with eventual consistency, and a distributed and asynchronous event-driven architecture is no exception, as we will see in the next chapter.

## Eventual Consistency

Eventual consistency is one of the main concerns that people have with distributed systems and event-driven data products. But eventual consistency can mean different things to different people. For example, an application developer may be using a database that doesn't offer consistent read-after-writes, such as in the case of a large distributed database or when using event sourcing to build up state. In an event-driven data mesh, we're more concerned with the effects of multiple consumer systems subscribing to event-driven data products and how to work with individual consumers each reading data at their own rate.

There are people who have been looking at, working on, and thinking about eventual consistency for quite a long time. Pat Helland is just such a person and has written an excellent piece that collates insights and opinions from numerous thought leaders on the subject.

> Since Doug [Terry] coined the phrase eventual consistency in the Bayou paper in 1995,
> I was interested in his perspective. When he defined eventual consistency, it meant that
> for each object in a collection of objects, all replicas of each object will eventually
> have the same value. Then, he said: “Yeah, I should have called it eventual convergence.”
> 
> —Pat Helland

Helland goes on to discuss a definition by Peter Alvaro, from his 2015 PhD thesis “Data-Centric Programming for Distributed Systems”:

> A system is convergent or “eventually consistent” if, when all messages have been delivered, all replicas agree on the set of stored values.
> 
> —Peter Alvaro

Both Terry and Alvaro converged (ha!) on the same definition of eventual consistency, putting the focus on independent replicas eventually converging on the same set of stored values. We’ll keep using the “eventual consistency” terminology, but keep in mind we’re really talking about convergence of data.

A consumer that is continually materializing an event stream can easily provide you information on what data it does have in its data store, but it can’t tell you what it doesn’t have. Because the data store is eventually converging, it may tell you it doesn’t have a piece of data when queried, but then immediately receive and process that data in the very next clock cycle. However, a consumer does have the ability to tell you if it is caught up to a given offset, and we can use this knowledge when resolving questions about convergence.

Many of the questions and concerns regarding eventual consistency in the event-driven world stem from a concern that “bad things” will happen because of it. It’s often used as a threatening term, listed in the cons section of a architectural rundown treatise. But it doesn’t have to be a con, because there are only a few big things to watch out for. There are two main reasons why two independent consumer services may have not yet converged:

### *A service is lagging behind*

All of the data in the data products is consistent, but one service is simply lagging behind on its consumption, processing, and storage of the data.

### *A data product is lagging behind*

The data within the data products is *not* consistent, but each service is fully up to date with the latest data. For an event-driven data product, either the producer has failed to write the data or the event stream may be unavailable. In the worst case scenario, the data product would lag so far behind that it may violate its SLAs.

Note that these two options are not mutually exclusive. It is entirely possible that one or more consumers’ services *and* one or more streams are both lacking data. Although convergence will eventually bring consistency, it will only be fleeting until again our services and data products are temporarily lacking data and in need of processing to catch up.

The thing is, eventual consistency isn’t really as impactful as you may think, and we’ll examine some good ways to handle it later in this chapter. The vast majority of time your event-driven services will be up to date with the latest events and effectively within the same time bubble, much like regular old synchronous services. This is one of the reasons why I think it generates a lot of apprehension—you never know *when* your service is going to start lagging and shifting into its own time bubble, and if it does, how to detect it and what to do about it.

The crux of the matter is that eventual consistency really only starts to become an issue when one independent context asks a synchronous question to another independent context, with no guarantees that their internal data sets are synchronized.

## Converging on Consistency, One Event at a Time

Each event-driven processing instance effectively exists in its own time bubble, with its internal time based solely on the event timestamps that it has consumed and integrated into its state. The vast majority of event streams provide data in an incrementing offset and timestamp order, though certainly the events can also be out of order (more on this later in the chapter). Thus, while the consumer service is free to look at the wall-clock time, its own internal time is based completely on the timestamps of the events that it has consumed.

Take Figure 10-1, which shows two independent consumers reading from a single event stream. Each service is fully independent, chugging along on processing the events, applying business logic, and saving the data in state.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_230_0.png)

Figure 10-1. The crux of the eventual consistency issue

The results that each service provides to the outside world, be it by request-response API, an output event stream, or other means, are very unlikely to be precisely synced with similarly materialized data in another service. This is where discrepancies and confusing results can creep in. Consumer 2 at time t=900 asks Consumer 1 at t=220 for its copy of Sunflowers data. Having never even heard of Sunflowers, all Consumer 1 can do is reply with Error, not found.

In contrast to event-driven data product consumers, think of two synchronous services that communicate over request-response APIs and that own and store all of their data within their own services. When one of these services issues an API call to another, it’s not thinking, “I wonder what time it is over there.”
