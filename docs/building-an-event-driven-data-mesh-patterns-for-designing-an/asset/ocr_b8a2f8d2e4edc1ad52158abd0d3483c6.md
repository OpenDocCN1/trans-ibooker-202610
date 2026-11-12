Precisely how the schema is replaced by the unique ID depends on both the producer’s serialization logic and any limitations in the format of events in the schema registry. For example, Confluent’s Kafka Schema Registry replaces the schema with a 5-byte prefix.

Using a schema registry requires serializers and deserializers to adhere to the proprietary format. And while Confluent’s schema registry is fairly widely used with Kafka, it’s not the only schema registry, and not all schema registries will use the same format. As part of building self-service tools, your governance team will need to negotiate which schema to support for building your data mesh.

A schema registry provides other benefits in addition to network and disk I/O savings. Here are a few other significant benefits provided by a schema registry:

# Data discovery

The schema registry provides a mapping of event streams to its registered schemas. Prospective consumers can examine the schemas to see if the stream contains the data they’re interested in. Self-service tooling can provide search functionality on top of the schema registry API to search for specific fields, documentation, or metadata tags.

# Schema evolution validation

Registering a schema with the schema registry is a mandatory part of the write path of an event to a stream. This action provides a hook for validating the producer’s current schema against that stored in the registry. Did the user evolve the schema in a way that is unauthorized by the schema registry (e.g., the new schema does not support backward compatibility)? Throw an exception. Is the schema completely invalid compared to what was registered under that ID before? Throw an exception. Is the event malformed? Throw an exception. The schema registry provides a safeguard against unintentional and unauthorized changes, protecting the data integrity so that the data product can meet its quality and reliability SLAs.

# Automatically updated documentation

The schema registry provides a minimal form of automatically updated documentation for each event stream. Users can view all the registered schemas, including the names, types, and doc fields. Embedded schema docs are very useful for highlighting any idiosyncrasies or corner cases in the data. They are also far more likely to be up to date because they are embedded, as opposed to documentation maintained independently by an outside party.

# Downloadable schemas to generate code

A consumer can download schemas to generate class definitions and test events for their unit tests.