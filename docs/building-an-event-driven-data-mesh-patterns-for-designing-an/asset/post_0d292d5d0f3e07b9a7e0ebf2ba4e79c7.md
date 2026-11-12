
In the following sections, we'll take a look at each of these levels in greater detail. We'll evaluate the platform levels, identify strengths and shortcomings, and discuss how to incrementally resolve the deficiencies.

As you source feedback and observe use cases, you'll discover the areas that need more investment that will lead you from one level to the next. Focus on getting incremental value, and ensure that the features you're adding are indeed solving the use case problems that people have. A data mesh self-service platform needs to be only as complicated as you make it—so what's the minimum that you can get away with? Let's find out.

## Level 1: The Minimal Viable Platform

The MVP is meant to act as a starting point for your data mesh journey. The MVP is not an ideal implementation by any means—it contains just enough functionality to implement the core data mesh principles and provides a quick and dirty yet strong base for your users to gain practical experience.

The four main components in the MVP include an event broker, a schema registry, a metadata catalog, and a connector service. Event broker selection was covered in “Selecting an Event Broker” on page 67, and for the purposes of this chapter we’re going to consider using Kafka as our broker of choice.

Let’s take a look at the remaining three components in turn.

### The Schema Registry

An event schema definition details the names, types, ranges, mandatory fields, default values, and documentation of an event, much like a table schema details the data for a relational database table. Chapter 6 will cover schemas in far more detail, but for the purposes of this chapter, you can think of a schema as a strongly typed data definition that gives both the producer and the consumer of the data a common protocol. Example 5-1 shows a simplified Person Protobuf schema that details the id, name, and height of a Person object, published to an event stream.

### Example 5-1. Simplified Person Protobuf schema definition

```
message Person {
  int32 id = 1;
  string name = 2;
  int32 height = 3;
}
```

The schema registry is a repository of event schema definitions that maps each schema to the event stream that it belongs to. It acts as a gatekeeping component by preventing events that do not adhere to the registered schema from entering an event stream, ensuring strict data quality. It also provides users an API to look up an event stream’s schemas as part of the process for choosing the data product best suited for their use. We’ll cover the schema registry more thoroughly in “The Role of the Schema Registry” on page 143. For now it’s sufficient to think of the schema registry as both a gatekeeping component and one that provides an easy way to look up the event-stream data product schema.

Confluent provides a commonly used and simple to integrate open source schema registry for Kafka that provides all of this functionality out of the box. As we discussed back in “Selecting an Event Broker” on page 67, easy and free access to community-built tools and services makes implementing and maintaining a data mesh much easier, versus building and maintaining your own tools in house.

Next, let’s look at the metadata catalog.

### An Extremely Basic Metadata Catalog

The metadata catalog of our MVP platform is a simple cloud-based spreadsheet. Registering a data product is as simple as filling in the metadata of your event stream and schema. Write permissions are restricted to data product owners and remain read-only for everyone else in the organization. Alternatively, only team leads can have access to modifying the spreadsheet, to avoid accidental mutations or deletions.

Table 5-1 shows an example of what a minimal basic spreadsheet could look like.

| Name | Topic | Bootstrap URI | Owner | SLA | Quality | Schema URI | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sales | gold_sales | k1.brk.kek:9093 | @bondolabs | Tier 1 | Gold | ../gold_sales | Canonical sales data, including sanitized payment types |
| Orders | gold_orders | k1.brk.kek:9093 | @smahmood | Tier 1 | Gold | ../gold_orders | Canonical orders data, excluding PII |
| Page views | page_views | x3.brk.wuu:9093 | @vsalamanca | Tier 3 | Bronze | ../page_views | Page view metrics piped in from Google Analytics |

Name indicates the data product name, while the combination of topic and bootstrap Uniform Resource Identifier (URI) indicates how you could go about connecting to the event stream, served by a Kafka broker. The owner indicates the Slack handle (or email address) to communicate with the data product owner. SLA and Quality are just two examples of the kinds of metadata made mandatory by the governance team, as discussed previously in “Metadata Standards and Requirements” on page 77. Schema URI provides a link to the schema registry API that produces a human readable page of the schema for that event stream. Finally, description offers a human readable string of text describing the data product and explaining any context that may otherwise be missing from the metadata.

A spreadsheet is a very easy and simple way to get started with communicating what data products are available, where they’re located, their format, and who owns them. You could also choose to use something like a relational database, but given that at this stage of the product there’s no need for a programmatic API, a shared spreadsheet tends to be the absolute quickest way to get started.

Next, let’s take a look at the last component: connectors.

### Connectors

Connectors allow you to easily *source* data from a database and write it into an event stream. They also allow you to *sink* data, reading it from an event stream and writing into a destination database. Individual connectors typically run on top of a distributed connector framework, offering scalability, redundancy, and resource isolation. The connect framework generally comes as an add-on component for the event broker, such as Kafka and Kafka Connect.

Connectors provide a rudimentary way to construct data products by enabling you to extract data from a source database, transform it, and sink it to an event stream. Kafka offers a whole host of connectors, enabling you to source data from MySQL, Postgres, Oracle, MongoDB, Microsoft SQL Server, and DynamoDB (just to name a few). Similar to source connectors, matching sink connectors provide an easy way to take data in the event stream and write it to the database of your choice. You can also write your own custom connectors to connect to less common or legacy systems that don’t have readily available connectors.

Transformations are an essential part of selecting and modeling a data product and in the subsequent, more mature platforms in this chapter we will evaluate some more powerful options. But for our MVP, we can look at using the built-in transformation components to remodel and select just the data we want from our source. Kafka’s *single message transforms* allow you to chain together out-of-the-box transformations to filter, modify, and transform your data with minimal effort. Just as with custom connector code, you can also implement your own custom transformations if none of the existing ones suit your needs.

We’re going to talk more about bootstrapping data from existing systems using connectors later in Chapter 8. For now, let’s wrap up this MVP.

### Level 1 Wrap-Up: How Does It Work?

Figure 5-1 illustrates the four main components of the MVP along with a basic registration and usage workflow.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_115_0.png)

Figure 5-1. The minimal viable self-service platform for powering an event-driven data mesh

The data product owner creates a source connector (1) and grants it read permissions to a specific table in their team’s database. The connector starts up, reads the table schema, and automatically creates a schema (2) to register with the schema registry. Next, the connector incrementally reads the source table rows and converts it automatically into events that match the registered schema. The events are then written into the event stream associated with that table (3).

The data product owner can then choose to register the event stream as a data product (4). Using their write-access permissions, they add the governance-required metadata to the spreadsheet, tag themselves as the owner of that product, and complete any other mandatory fields. They also link the metadata in the spreadsheet to the URL of the schema stored in the event broker (5). The schema registry is used not only for serializing and deserializing events but also for data discovery purposes for human users by answering the question “What is the format of data in this event stream?”
