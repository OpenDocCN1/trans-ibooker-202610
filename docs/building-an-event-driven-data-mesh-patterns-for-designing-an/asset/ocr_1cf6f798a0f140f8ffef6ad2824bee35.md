![](bbox=[0.142, 0.083, 0.856, 0.253])

Figure 3-6. A queue with two subscribers each processing a subset of events

Historically, queue brokers have required a maximum retention time for events stored in the queue. If events are not processed within a certain time frame, they are marked as dead, are no longer delivered to the subscribers, and are subsequently deleted. Similar to ephemeral communications, time-based retention and non-replayable data has influenced the (incorrect) idea that brokers cannot be used to retain data indefinitely.

Event-driven data products need replayability and _each_ consumer must get _all_ of the events in _precisely the same order_ that they were published.

![](bbox=[0.178, 0.48, 0.252, 0.556])

Modern queue brokers tend to support both replayability and infinite retention of events via the adoption of the durable append-only log, which we will cover more in the next section. For example, both Solace and RabbitMQ Streams allow for individual consumers to replay queue events. However, these systems struggle to support the strict ordering semantics, indefinite replayability, and scalability requirements of a modern data mesh.

Queues are best used at the individual application level to act as a _buffer_, either for input events or for interprocess communications. They are very useful at ensuring that each event is processed by only one consumer, but are not suitable as a conduit for communicating complete data sets as event-driven data products.

In the next section, we'll take a look at how consumers can use events provided by the event stream.