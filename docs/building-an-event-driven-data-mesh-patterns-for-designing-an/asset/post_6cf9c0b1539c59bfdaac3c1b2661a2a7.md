
## Example 3-2. Showcasing joins with Flink SQL

```
CREATE TABLE Inventory (
 item_id VARCHAR,
 quantity BIGINT,
 timestamp TIMESTAMP(3),
 PRIMARY KEY (item_id) ENFORCED,
) WITH (
 'connector' = 'kafka',
 'topic' = 'inventory',
 'properties.bootstrap.servers' = 'localhost:9092',
 'properties.group.id' = 'my_app_group_id',
 'format' = 'avro',
 'scan.startup.mode' = 'earliest-offset'
);
```

```
CREATE TABLE Items (
 item_id VARCHAR PRIMARY KEY,
 name VARCHAR,
 description VARCHAR,
 brand VARCHAR,
 timestamp TIMESTAMP(3),
 PRIMARY KEY (item_id) ENFORCED,
) WITH (
 'connector' = 'kafka',
 'topic' = 'items',
 'properties.bootstrap.servers' = 'localhost:9092',
 'properties.group.id' = 'my_app_group_id',
 'format' = 'avro',
 'scan.startup.mode' = 'earliest-offset'
);
```

```
CREATE TABLE Enriched_Item_Inventory AS
SELECT *
FROM INVENTORY
INNER JOIN ITEMS
ON ITEMS.item_id = INVENTORY.item_id;
```

The Flink SQL code is concise, clean, and powerful. You can write extremely useful data transformations quickly and easily, leveraging the Kappa architecture with little heavy lifting on your part. Kafka Streams and Flink are among the most popular stream-processing frameworks, and either one is a good choice if you're looking for a full-featured, event-driven framework. For the sake of brevity, I won't cover each of the possible options and frameworks but I do encourage you to familiarize yourself with your options before selecting one for your use cases.

Kappa architecture is the best way to build and use event-stream data products. It is simple and effective, allowing consumers to use a single data product API for acquiring both historical and real-time data. Despite Kappa's clear advantages, you will likely encounter recommendations to use the Lambda architecture, based on the now-false assumption that an event stream cannot indefinitely retain data. Let's now take a look at exactly what the Lambda architecture is and why it falls short of Kappa.

## The Lambda Architecture and Why It Doesn't Work for Data Mesh

The Lambda architecture is a composite of offline historical batch data and online event-streamed data. In this architecture, consumers obtain their data from these two sources and must recombine it into a consistent model within their own domain. There are two main versions of this architecture, and we'll look at each in turn.

In the first version, the batch data layer comprises the results of some sort of computation made by the domain owner, such as a materialization or aggregation. At startup time, the consumer loads the batch layer data into its context, then switches over to the event-streaming layer to read and process events.

Figure 3-10 shows a simplified implementation of the Lambda architecture. The producer writes a new event (1) to both the event stream (2) and the batch data layer (3). Note that the batch data store does not source its data from the event stream but directly incorporates the new data (1) into its database. The serving layer (4) contains two periodically updated materialized views of the batch data. In this case, one view is for consumer A and the other view is for consumer B.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_75_0.png)

Figure 3-10. Lambda architecture, with data written to both batch and the event stream

Why two materialized views? Consider two consumers with different yet similar requirements. One consumer needs an hourly net sales aggregation, split up by sales area, while the other needs daily gross sales, excluding seasonal items. Without access to the full history of events, the two consumers remain beholden to the Lambda batch owner to maintain, compute, and provide the historical results to them.

You may also notice another problem with this approach—the consumers each require their own business logic requirements to be computed and maintained by the serving layer’s owner! This leads to a shared responsibility model where the consumers do not have full ownership of their business logic. If they need to change their business logic, it must then be changed at the producer side during the composition of the batch data and at the consumer side for merging in the event-stream data.

In the second version of Lambda architecture, the batch data layer is simply a long-term store for events maintained outside of the event broker, as shown in Figure 3-11.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_76_0.png)

Figure 3-11. Lambda batch store built from event stream

In this version, new data (1) is written to the event stream (2) and eventually copied (3) into the batch data store (4). The batch store applies no special business logic and is simply a secondary store for immutable, incrementing events, exposed via a queryable API (5) for consumers (6). Each consumer is responsible for side-loading/boot-strapping the events they need from the batch data store into their own context, building up their own state as they go. Once they build up their current state, they then switch over to the real-time event stream (2).

This places the business logic related to the interpretation and modeling of events squarely within the domain of each consumer. The producer is no longer required to support any custom, precomputed views for the consumer, greatly simplifying operational concerns.

Now, you may think this Lambda variation is a bit odd—why use a second state store if you’re just going to store raw events? Remember when I mentioned the tendency of designs to assume it’s not possible to store events in a broker indefinitely? Lambda architecture is based on the notion that it’s simply not possible to store a comprehensive history of events in an event broker.

However, modern event brokers, in conjunction with cheap disk and cheap cloud storage, have made indefinite retention a reality. And though once promising, the Lambda architecture has proven to be more complicated and difficult to implement than many originally thought. Let's look at some of the main issues that make Lambda architecture fiendishly difficult to use at scale:

### The producer must maintain two code paths

One path handles the insertion of data into the batch data store and one constructs events to publish to the event stream. Atomic commits for both the event stream and the batch data store are not readily available, so you run the very real risk of duplicate or missing data. Reading the data from the batch may give you different results than reading from the stream.

### The consumer must maintain two code paths

Just as the producer must maintain two code paths for publishing data, the consumer must maintain two code paths for resolving data. Both the stream and the batch representations of the data need to be consistent, which is hard to do in practice, especially over a long period of time as data changes and evolves.

### A batch data-sourced consumer and a stream-sourced consumer may not converge on the same result

You have one consumer running since day 0. It has never loaded any data from the batch data store. Its internal state has been constructed solely by consuming and aggregating the event stream. You start a second consumer, which is an identical copy of the first, but it bootstraps its state from the batch data store. Once they're both up to date with the latest offset, you pause the inputs of new data. Are their states identical? The answer should be a resounding “Yes!”, but unfortunately, in practice, it is usually a sad “No.” Resolving consistent state from multiple sources with multiple code paths tends to be very complex in practice.

### The batch data store and stream data models must evolve in sync

As the source domain model evolves, the representation of the data in the event streams usually changes too—but so too does the format of the batch store. The producer must ensure that what it writes to the event stream is fully consistent with the model in the batch serving layer. And through all of this, a historical consumer reading mostly from batch and a real-time consumer who only ever reads from the event stream must end up with copies of identical data. If this sounds like it’s difficult to do well, you’re right. I’ve seldom seen it used with success.

### Merging multiple Lambda-based data products is difficult

The final nail in the coffin of the Lambda architecture is due to the difficulty of merging multiple Lambda data products. Each Lambda data product maintains its own rules pertaining to event-stream retention, when the event data is folded
