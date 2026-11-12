
## Materializing Events

Materialization is the process of consuming an event and merging it into your own local data store. If a record with the same primary key already exists in that store, it is usually simply overwritten, though you may choose to implement more complex merging logic if you choose.

In practice, consumers don't usually need to store the entire event contents, but only the subset of data relevant to its domain. This substantially reduces the amount of data stored and processed, and helps keep the footprint of the consumer service small.

Figure 3-7 shows a consumer materializing the item event stream into its own state store. The consumer has decided to keep only the most recent record and to discard the older values for a given item. In this example, item key = 123 has been updated by the change of type from hat to helmet, as represented by the event at offset 2. Upon consumption, the consumer commonly evicts the previous materialized value for that key (offset 0) by overwriting the data in the database with the newest values. In some cases, you may want to keep a list of previous entries for a given key, a subject we'll cover more in “Slowly Changing Dimensions” on page 193.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_69_0.png)

Figure 3-7. A consumer materializing the item event stream into its local database

Thanks to materialization, consumers can also execute logic based on changes from one event to another, since they retain the “current” state in memory and obtain the “new” state from the event stream. They can infer changes to any field within the event, which provides them with exceptional flexibility in how they use and process the event data.

Up next: materialization's fancier and often more complex cousin, aggregation.

## Aggregating Events

An _aggregation_ is an entity composed by merging multiple state events together. An aggregation could be as simple as a running sum but could also entail using multiple event types to build a more complex result. Figure 3-8 shows two instances of a single consumer application processing two partitions from a single event stream.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_70_0.png)

_Figure 3-8. Aggregating state local to each consumer instance. The key is represented by the shape of the event._

Consumer instance 0 has processed all of the events in its assigned partition 0 and is awaiting new incoming events. The database within its boundary shows the current SUM aggregations that it has computed. Meanwhile, Consumer instance 1 is still processing historical data from its assigned partition 1 and is catching up to the current head event.

One of the major advantages of event-driven data products is that they make it very easy for you, as a consumer, to build your own aggregates. Instead of relying on the data product owner to build your aggregates for you, you can simply ingest the data into your own domain and aggregate it according to your own needs. If your derived data product is “just” an aggregation, you could consider republishing it to the data mesh as an aggregate-aligned data product (recall “The Three Data Product Alignment Types” on page 33). Alternatively, you may choose to make a consumer-aligned data product if you end up building an aggregation with more complex business logic that joins and mixes multiple input streams.

While this strategy does require extra work on your part, it also provides you with substantial _operational freedom_: you can simply change your computation and replay the event stream to rebuild the results instead of relying on the upstream data product owner to redefine and re-create the aggregate for you.

However, building your own aggregates brings the risk that a peer in another domain may be computing the same aggregate, with logic that doesn’t match yours. As your data mesh evolves, expect to see aggregations in consumer domains moved upstream to become formal aggregate data products, reducing the risk of similar-yet-different and duplicate aggregates.

## Operations, Analytics, and Limited Resources

Renegotiating team responsibilities is part of implementing a data mesh. But one of the risks is that this renegotiation can become untenable for those holding new responsibilities, especially if they lack sufficient resources. For example, a very popular source domain may find itself responsible for creating a large number of different kinds of data products but be unable to dedicate sufficient resources to the efforts.

The reality is that we live in a world with multiple competing requirements. While data mesh can prescribe clean and neat boundaries about who _should_ be responsible for creating, building, and maintaining a data product, the reality is often quite a bit messier. Operational concerns tend to take precedence over creating new data products _unless those data products also serve operational use cases_. For example, the sales data product of “Source-aligned data products” on page 33 illustrates operational concerns—a sale has occurred, the data is published in an event, and consumers will now react to it to fulfill it. At the same time, this data product can also be used to compute analytical results, such as the daily sales aggregate-aligned data product in “Aggregate-aligned data products” on page 33.

Building your own aggregates from source-aligned and other aggregate-aligned data products is an essential escape hatch to the problem of insufficient resources. Source-aligned data products are fairly easy to create in comparison to aggregates, and the fact that they can be tailored to support operational use cases tends to give them a much higher priority in the responsibility queue of the data domain owner. When resources are tight, focus on creating general-purpose source-aligned data products that can be used by many different teams.

There remains a popular though incorrect notion that event streams are incapable of storing data indefinitely. And that even if you _could_ do it, you shouldn’t, because they’re not built for that purpose. This mentality is an extremely outdated viewpoint, often based on the historical limitations of messaging systems, queues, and nascent event brokers. In the next section, we'll evaluate the role of event streams from the lens of data through time, the Kappa and Lambda architectures, and the modern way to think about event streams.

## The Kappa Architecture

The Kappa architecture posits the use of an event stream as the source of both current and historical data. Consumers who want a complete picture of the data simply set their offset to the beginning of the stream and consume all of the data in sequence, eventually reaching the real-time head of the stream. This architecture is in contrast to the Lambda architecture, which posits two separate storage layers, one for historical data and one for near real-time data. We'll come back to Lambda in a bit.

The Kappa architecture was first presented in 2014 by Jay Kreps, co-creator of Apache Kafka and cofounder of Confluent. The Kappa architecture has only relatively recently become feasible, in large part due to technical advances in modern event brokers.

Indefinite retention provided by tiered storage is now the norm for modern event brokers, making it easy and affordable to store as much data as you need in the stream. Deletion of old records through compaction keeps the size of streams built for ECST to a minimum, in line with the scope of the actual data domain. Figure 3-9 shows the end-to-end workflow in a Kappa architecture. The producer (1) writes events as they occur into the event stream (2), and each consumer is responsible for processing that data, including storing it in its own internal state store (3).

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_72_0.png)

Figure 3-9. In the Kappa architecture, each consumer independently consumes events and builds its own state

Of course there are some trade-offs with the Kappa architecture. For example, you must build your service’s state by processing the entire history of events. For extremely large data sets, particularly if there is an insufficient partition count, you could be looking at many hours. New and existing applications that need to build or rebuild their state will need to account for that with historical data processing time.

Consumer applications can rely on maintaining their own snapshots of materialized state to bootstrap loading their application. In the event of an outage or a failure, a restarted application can simply load its own personal snapshot, complete with offset tracking history, and resume processing from precisely where it left off. The application remains responsible for building and loading its own snapshots, though this functionality comes out of the box with leading stream-processing technologies such as Apache Kafka Streams, Apache Flink, and Apache Spark.

The Kappa architecture is key to building decoupled event-driven services (and microservices) and provides your applications’ builders with unparalleled flexibility for application development. The business logic for consuming, transforming, storing, and reacting to state changes is completely within the consumer’s control. Modern cloud service providers offer easy access for requisitioning storage space, memory, compute, network I/O, and durability to help build and use data products.

Example 3-1 shows a Kafka Streams application with two KTables, which is just a stream materialized into a table using ECST. Next, the inventory KTable and the sales KTable are joined using a nonwindowed INNER join to create a KTable of denormalized and enriched item inventory. Stream-processing frameworks make it very easy to handle event streams, build up internal state using ECST, and merge and join data from various data products, in just a few lines of code.

## Example 3-1. Showcasing joins with Kafka Streams

```
StreamsBuilder builder = new StreamsBuilder();
//Materialized state of the "inventory" stream
KTable inventory = builder.table("inventory")
//Materialized state of the "items" stream
KTable sales = builder.table("items")

//Join events on primary key and apply custom business logic
//Note that inventory and items need to be keyed on the same itemId
KTable enrichedItemInventory = inventory.join(items, ...)
```

Something similar can be accomplished in SQL code using Apache Flink. In Example 3-2, Inventory and Items are materialized tables based on the associated Kafka topics. An Enriched_Item_Inventory table is created by INNER joining on the item_id (primary key) of the two tables.
