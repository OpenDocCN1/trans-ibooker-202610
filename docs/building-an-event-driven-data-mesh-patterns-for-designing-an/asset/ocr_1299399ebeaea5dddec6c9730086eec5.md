doesn’t work if Adam moves from Madagascar to Ethiopia, and then soon thereafter moves to Canada, causing two events to occur. The consumer business logic would not be able to trigger on this sequence of events since it doesn’t maintain any state. In practice, the theoretical stateless consumer is seldomly realized, since the vast majority of services of any reasonable complexity need to maintain state.

There are also a few drawbacks to this design:

# Compaction is difficult

Deleting an event using the before/after logic results in the after field being set to null—but the entire value itself is not null. By default, event brokers like Apache Kafka will not recognize this as a tombstone and thus will not delete it. While it may be technically possible to rewrite the compaction logic, it usually isn’t feasible, especially if you are relying heavily on SaaS solutions.

There are, however, some options, depending on which tooling you use. Debezium has worked around this limitation by allowing you to produce a tombstone after the before/after event, generating two events instead of just one.

According to Debezium’s documentation:

> A database DELETE operation causes Debezium to generate two Kafka records:
> 
> * A record that contains "op": "d", the before row data, and some other fields.
> 
> * A tombstone record that has the same key as the deleted row and a value of null. This record is a marker for Apache Kafka. It indicates that log compaction can remove all records that have this key.

# Risk of leftover information

As we saw earlier, previous data may be accidentally maintained indefinitely in the before field unless you issue a series of deletions.

# Doubled data storage and network usage

Before/after events double (on average) the amount of data going over the wire and stored on disk. Consumers, producers, and the event broker each bear part of this load. In some cases this may be trivial. Seldom-updated events or those with low volume are probably nothing to worry about, but extremely high volume event streams can quickly add up the costs. This can be particularly expensive depending on the cross-regional data transfer fees associated with high-availability producer, consumer, and event broker deployments.

I recommend using current state events over before/after when designing event-stream data products. The consumers will need to maintain state for the records they care about for their business processes, but disk space is relatively cheap and they need to select only a subset of the domain that they need. This also simplifies