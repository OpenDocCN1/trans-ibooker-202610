
## Eventual consistency

This model guarantees that all data will eventually be reflected in the platform, though it may take some time to propagate. Eventual consistency is a weak guarantee and can work for data that isn’t critical.

## Strong consistency

This model guarantees that all events are processed in order. The platform confirms propagation of any update to all copies of the data; once any entity or node makes an update, all replicas are locked until they have been updated.

Providing strong consistency can be difficult. If the velocity of events is so high that the streaming platform can barely keep up, it can be challenging, if not impossible, to design the system in such a way that all messages will be received correctly. The design constraints imposed by strong consistency are therefore unacceptable for various use cases, making eventual consistency the only possible approach. In these cases, you may need to take additional measures to ensure it is correct. For example, you could use an extra database to persist all the data and reconcile occasionally to ensure all data has been correctly synchronized. You can also use additional tooling, such as CDC, to ensure all changes are correctly delivered, or you could build an extra service to ensure consistency by reading, validating, and redelivering missing messages.

## Processing Methods

There are different processing methods within the eventual and strong consistency models. At-least-once processing ensures that messages don’t get lost, but doesn’t guarantee that all messages are unique and delivered only once. It’s acceptable to have duplicates and for events to be processed multiple times.

Exactly-once processing, also called the delivered-only-once model, guarantees that each event is delivered exactly and only once. No messages are lost, nor are there duplicates. Implementing this model might require additional redelivery patterns. Some engineers use additional identifiers to validate the uniqueness of every message.

The last model is called at-most-once or no-guarantee processing. In this scenario, no guarantees are given that all messages will be delivered correctly. Messages are allowed to get lost or can be delivered multiple times.

You can combine different models, depending on the requirements of your use case and other constraints. An eventual consistency model, for example, can be used to initially deliver messages without acknowledging receipts, and occasionally all messages can be compared, with duplicates removed and missing messages redelivered to ensure that they’ve been delivered exactly and only once.

## Message Order

Another typical challenge with streaming is the message order. In some cases messages are required to be strictly ordered, for example, by order of creation. Kafka allows you to do this, but only using one partition. If you need more partitions for scalability, the ordering must be based on an additional property in the message, such as the row_no. This might require the delivering party to make some adjustments in the delivery. Another approach is to consume all messages and reorder them on the consuming side.

## Dead Letter Queue

Some streaming platforms have a dead letter queue, which is an additional queue for messages that aren't delivered successfully. These might include messages that exceed the size limit, use an incorrect message format, are sent to an event stream that doesn't exist, or aren't processed within a set time frame. In these cases, messages aren't removed from the system, but rather are placed in the dead letter queue. Typically, a developer needs to manually examine what went wrong. Often, the problem is a change to the providing application that wasn't thoroughly tested; in such a scenario, the problematic messages need to be moved or submitted again to the original queue.11

11 For more on this topic, see Ning Xia's blog post “Building Reliable Reprocessing and Dead Letter Queues with Apache Kafka”.

## Streaming Interoperability

For interfaces between publishers and subscribers, you'll have to make some technology and design decisions. Some platforms dictate how the providing applications must connect to the platform and what protocol they must use. Other platforms are more open and support a wider range of message protocols to allow transparent interaction between multiple programming languages. A neutral way and popular choice is to use a protocol that encodes messages using an interface description language (IDL) for data format and type validations.

Another option is to look at a framework that supports data serialization. Data serialization frameworks play an important role in compatibility, versioning, and portability when moving data around interfaces. From these standpoints, these frameworks take care of consistent translation to a standardized data format. After translation, many systems, programming languages, and other frameworks can start to use the data. These serialization frameworks also improve data processing with features such as compressing and security (encryption). This makes serialization ideal for distributing data to the data product architecture. Popular serialization framework and protocol options include Apache Avro, Protocol Buffers, Apache Thrift, MQTT, and AMQP.

Another way to produce and consume messages from streaming platforms is to use RESTful APIs that support JSON and XML messages. 12 This is a good choice for online channels or web environments. JSON has the advantage of being schemaless and flexible, but it has one major drawback: it doesn't enforce compatibility as it doesn't enforce schema encoding. So, there's a trade-off to be made between message flexibility and enforcement.

## Governance and Self-Service

An event-driven architecture comes with additional metadata capabilities and requirements. The most important of these are the event stream registry (or schema registry), which is used for purposes such as:

- **Ownership registration**

Events are always owned. The registry keeps track of event ownership, and can also be used to classify message queues and event streams as internal or domain-to-domain.

- **Schema document management**

It's recommended to manage the schema layout and documentation of all of your messages. For XML and JSON, these are logically the XML or JSON schemas. Serialization formats, like Avro, use their own IDLs. A recommendation for documenting and maintaining events is to look at AsyncAPI, which is an open source initiative that seeks to improve the current state of event-driven architectures. For managing all your schemas and documentation together, the preferred approach is to use a tool like a registry.

- **Version management**

Schemas must be versioned. If they aren't versioned, there will be breaking changes that can directly affect event consumers. For managing schema versions, a registry is the preferred solution.

- **Lineage**

Unfortunately, lineage for events can be complex and depends on the stream processing engine that is being used. Depending on your situation, you may need to do some custom coding or implementation; for example, invoking the APIs as part of your catalog or data governance solution.

EDA's metadata becomes essential when improving the target operating model through self-service. For example, you might want to allow domains to reset their offsets or provision queues and event streams themselves, and have those requests approved automatically. Or you might want domains to design their own REST proxy templates and automatically deploy them using the right security policies. You can also enable domains to apply data filtering before passing messages on to different consumers. My recommendation is to enable these patterns one by one, keeping self-service in mind.

## Wrapping Up

Wow, that was a lot of content on events! I hope you now recognize their power, but also see the complexity that comes with them. With events, we can connect applications, federate data, digitalize processes, and improve internal and external communication. They allow us to understand how systems react to each other, which helps us improve our daily business operations.

Stream processing, as you’ve seen, is complex. To continuously capture data and ingest streams into a platform, you need to enrich your architecture with many additional components. Given the complexity and diversity of the landscape, you can end up using many different toolsets, frameworks, and additional services to, for example, ensure exactly-once processing. My recommendation is to build up slowly and set several ground rules:

- Standardize on services and platforms you offer to your domain teams. Strike a good balance; some tools and services are more specialized, while others try to be as comprehensive as possible. A good architecture is mutually exclusive and collectively exhaustive, so it’s comprehensive without too much overlap between what features services offer.

- Set standards for event processing: events are immutable by definition, as they represent things that have already happened.

- Make event producers responsible for the data ingestion: their role is to guarantee “at least once” delivery. Consumers, on the other side, should guarantee “at most once” delivery.

- Segregate event streams: each service must own its event stream from a particular domain.

- Advocate eventual consistency when moving data across domain boundaries.

- Be careful about business logic. Keep your platforms and services for domain-to-domain communication as agnostic as possible.

- Strive for clear separation between internal application and domain-to-domain distribution. Apply all the principles from the previous chapters when crossing
