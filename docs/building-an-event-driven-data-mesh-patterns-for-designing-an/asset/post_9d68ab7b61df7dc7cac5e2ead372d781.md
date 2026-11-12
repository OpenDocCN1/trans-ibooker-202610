
## Synchronize Data Products on Time Boundaries

Event-stream data products serve a wide range of consumer use cases, including, of course, sinking the data via a connector to cloud storage. Big data analytical processing by the likes of Apache Spark, Flink, Presto, or BigQuery relies on well-formed, batch-created data, often in the form of a Parquet, ORC, Avro, or other big data format. Many organizations have developed extensive data engineering, science, and analytics pipelines relying heavily on batch data. A well-built data mesh should meet the needs of these people where they are and not force them to move onto event streams.

An event stream is a continuous flow of data, whereas you can consider cloud storage data as data at rest. This means that we really need to be cognizant about how we write the data to disk and should adhere to any partitioning standards already in place by the big data folks. It’s quite common to partition data based on timestamps, be it in 1-minute, 5-minute, 30-minute, or 1-hour partitions. Partitioning allows the aforementioned big data processing engines to load only the range of data they care about and nothing else. You may, of course, partition data on many other attributes, such as event type, location, infosec regulations, and PII details, but these are policies that you’ll need to discuss with your governance team and ideally the people who know and use the data.

Temporal boundaries are important for batch processing in the big data space for several reasons. One, it’s much more efficient and affordable to only load and process the required data for computing the pipeline results instead of reading a massive data set from cloud storage into memory just to discard most of it. Second, batch processing jobs require that all of the data be present in a time slot before it can start processing. If data is collected in 30-minute chunks, it means we can kick off a processing job every 30 minutes to compute the next batch of results. Many big data analytics computations deal with temporal analysis, and having a precise time range is essential for accuracy.

Third, but not least, big data jobs often need to join on other data sets. By partitioning all cloud storage data sets in the same manner, on the same date and time boundaries, we enable the users of that data to select and choose the time ranges suitable for their use cases. If they only need the last half-hour, no problem. If they need the last 30 days, it’s available.

The simplest way to get started is to check out what sink options you have for your event broker. For example, Confluent offers an AWS S3 sink for free usage with Kafka Connect. It has an easily configurable format option (Avro, Parquet, JSON, Raw Bytes), along with a wide range of data partitioner options, including custom time-based, daily, and hourly options. The flexibility of sink solution options makes it easy for you to get your event-streaming data into the format necessary to power your existing batch data pipelines.

There’s one more important subject to take a look at before we close out the chapter. Despite all of our best efforts, events may be published out of order to an event stream and require resolution by our downstream consumers. Let’s take a look at what options we have for handling these events.

## Out-of-Order Events

While it’s not too difficult to imagine a world where each event in a stream partition is in perfect incrementing timestamp order, the reality of accomplishing it is much more challenging. Consider the pair of producers in Figure 10-6 that take user-behavior click events on a website and convert them into events.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_240_0.png)

Figure 10-6. Intermittent failures can cause events to be written out of temporal order

Instance 1 is temporarily unable to connect to the event broker but buffers the click events internally until the connection is restored. When connectivity resumes, Instance 1 can write its backlog of events to the event stream with no loss of data. However, the events in stream, time-stamped with the time that the Instance 1 received them, are now temporally out of order with the events from Instance 0.

A temporary outage is not the only scenario where you’ll encounter this issue. In fact, the most common source of out-of-order data is due to the inability to perfectly synchronize distributed clocks. Though NTP servers and atomic clocks can bring us pretty close to perfect event-time synchronization, the fact of the matter is that we’ll always have a bit of skew—a few milliseconds here, a few hundred milliseconds there. Misconfiguration, bugs, and human error also inevitably add their own sources of skew and drift.

You may choose to stamp events with the wall-clock time of the receiving event broker when it receives the event for publishing to the stream. Using the broker-received event time only papers over the clock skew issue as the true event time is when the event actually occurred, not when it was recorded to the broker. It’s up to you if you choose to use it, but you’d be better off maintaining the original event timestamps and instead commit to handling out-of-order data at the consumer side.

Out-of-order events are important to consider because they affect the state that a consumer converges to. Two independent consumers that treat out-of-order events differently can arrive at permanently different states and never converge at all. In some situations it may not matter, but in others it may be of critical importance.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_241_0.png)

I cover the major techniques that stream-processing frameworks use to process events, progress through time, and identify out-of-order data in Building Event-Driven Microservices, Chapter 6. Please refer to this chapter if you are interested in the under-the-hood details.

Since events can be written out of order and aren’t guaranteed to have monotonically increasing timestamps—how do we handle them? Let’s take a look.

## Resolving Late-Arriving Events

Events can only be considered late from the perspective of a consumer, and each consumer is free to maintain its own definition of what is late and what is not. Take for instance a consumer maintaining a time-bound window of aggregated data, say advertisement clicks per hour, derived from the source-aligned Ad Clicks stream. Windowing operations are excellent forcing functions for handling late arriving data, as they require that you answer just one simple question: “Is this specific event part of your time window, or not?”

Figure 10-7 shows the advertisements that the users clicked on, the incrementing offsets, and the event time of when the user clicked on it.

| Value | Time | Offset |
| :--- | :--- | :--- |
| addl:"CoffeeKing-3", userid:"Adam" | 100 | 1 |
| addl:"TeaQueen-7", userid:"Justin" | 500 | 2 |
| addl:"Diet Cola-0", userid:"Renata" | 200 | 3 |
| addl:"CoffeeKing-4", userid:"Adam" | 600 | 4 |
| addl:"KhlavKalash-0", userid:"Homer" | 300 | 5 |
| addl:"TeaQueen-3", userid:"Lisa" | 700 | 6 |
| addl:"MountainDont-0", userid:"Bonko" | 800 | 7 |

Figure 10-7. Out-of-order events representing advertisement clicks, based on event time

There isn’t always a well-defined line between one window of time and another. Note that from offset 2 to 5, the event time oscillates back and forth, indicating a very close race condition, possibly between two producer instances that are *almost* in sync. Overlapping oscillating event times like these are relatively common in event streams created by more than one producer instance, and you’ll need to plan accordingly for dealing with them.

Full-featured, event-processing frameworks, like Kafka Streams, Apache Spark, and Apache Flink, have functionality to select and process events in a predictable and deterministic order. Each processing thread selects the event with the lowest (earliest) timestamp and lowest (earliest) offset from among the next possible events, then dispatches it through its business logic. Each of these frameworks has its own way of keeping track of time as seen by the processing instance—Spark and Flink use watermarks, and Kafka Streams uses a stream time. I discuss this in more detail in Chapter 6 of my book *Building Event-Driven Microservices*.

When your stream processor windowing operation sees an event with an event time greater than the window cutoff, it starts a new window and closes off the old window, emitting it to the downstream output. Since the input event streams can be temporally out of order, what happens to the next event that should have gone into the initial window? Well, you have a few options:

- *Discard it*

You can just drop the event as the window is closed, and any time-based aggregations can conceivably be complete. However, this option is much like slamming the airplane door shut when you can see people sprinting to make their connection—perhaps waiting a couple more seconds isn’t too bad.

- *Keep window open and delay output*

You can also delay closing the window until a given amount of time has elapsed. You’ll gain a higher likelihood of completeness, but it’ll require that your downstream consumers can tolerate the higher latency.

- *Keep window open and output multiple updates*

You can output the windowed results as soon as the event-time threshold is hit, but then maintain the old window and the new window in parallel. The old window will stick around for the duration of the grace period, and you can merge in late-arriving events up until the grace period expires. At that point, you finally close the old window and output the results again if they have been updated during the grace period. From then on, any late-arriving events are simply dropped.

There are no hard-and-fast rules for how your consumers should handle out-of-order events—it’s really up to each consumer. Because each consumer is independent, you may find that it introduces nonconvergent behavior, *especially when directly*
