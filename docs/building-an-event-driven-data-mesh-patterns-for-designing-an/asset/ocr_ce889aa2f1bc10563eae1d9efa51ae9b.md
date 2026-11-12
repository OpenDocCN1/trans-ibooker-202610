One final thing to note: events are immutable. You can never modify an event once it is published to the event stream. Instead, you create and publish a new event containing the necessary correction or update. Immutability is essential for ensuring that every consumer has access to precisely the same data set and can reproduce the same results of a computation at a later point in time.

# What’s an Event Stream? What Is It Not?

> The business facts are best presented as business Domain Events, can be stored and served as distributed logs of time-stamped events for any authorized consumer to access.
> 
> —Zhamak Dehghani

Event streams are hosted on an event broker, with perhaps the most common option being Apache Kafka (a personal favorite of your author). The event broker, such as in the case of Kafka, provides a structure known as a topic that we can write our events to. It also handles everything from data replication and rebalancing to client connections and access controls. Publishers write events to the event stream hosted in the broker, while consumers subscribe to event streams and receive the events.

In its most basic form, an event stream is a time-stamped sequence of business facts pertaining to a domain. Events form the basis for communicating important business data between domains in a reliable and repeatable way, leading us to the following requirements:

+   - **Immutable**
+     Events cannot be modified once written to the log. Only new events may be added.
+   
+   - **Durable and replayable**
+     Events are durable, such that they can be consumed immediately or in the future.
+     Events can be replayed by new and existing consumers alike, provided the event broker has sufficient storage to host the historical data.
+   
+   - **Scalability and indefinite storage**
+     The event broker provides high availability, scalability, and indefinite retention, allowing the event stream to become the single source of truth for specific domain data.

Event streams rely on the durable append-only log, an immutable data structure that only permits appending of new data. Once written, data cannot be altered. The log is durable, such that consumers can consume the data as many times as they need. Events are not deleted once they are read, nor are they simply discarded in the case of an absence of consumers.