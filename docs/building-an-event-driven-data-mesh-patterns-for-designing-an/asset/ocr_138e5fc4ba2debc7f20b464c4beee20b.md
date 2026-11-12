The schema registry is a repository of event schema definitions that maps each schema to the event stream that it belongs to. It acts as a gatekeeping component by preventing events that do not adhere to the registered schema from entering an event stream, ensuring strict data quality. It also provides users an API to look up an event stream’s schemas as part of the process for choosing the data product best suited for their use. We’ll cover the schema registry more thoroughly in “The Role of the Schema Registry” on page 143. For now it’s sufficient to think of the schema registry as both a gatekeeping component and one that provides an easy way to look up the event-stream data product schema.

Confluent provides a commonly used and simple to integrate open source schema registry for Kafka that provides all of this functionality out of the box. As we dis‐ cussed back in “Selecting an Event Broker” on page 67, easy and free access to community-built tools and services makes implementing and maintaining a data mesh much easier, versus building and maintaining your own tools in house.

Next, let’s look at the metadata catalog.

# An Extremely Basic Metadata Catalog

The metadata catalog of our MVP platform is a simple cloud-based spreadsheet. Reg‐ istering a data product is as simple as filling in the metadata of your event stream and schema. Write permissions are restricted to data product owners and remain read‐ only for everyone else in the organization. Alternatively, only team leads can have access to modifying the spreadsheet, to avoid accidental mutations or deletions.

Table 5-1 shows an example of what a minimal basic spreadsheet could look like.

| Name | Topic | Bootstrap URI | Owner | SLA | Quality | Schema URI | Description |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Sales | gold_sales | k1.brk.kek:9093 | @bondolabs | Tier 1 | Gold | ../gold_sales | Canonical sales data, including sanitized payment types |
| Orders | gold_orders | k1.brk.kek:9093 | @smahmood | Tier 1 | Gold | ../gold_orders | Canonical orders data, excluding PII |
| Page views | page_views | x3.brk.wuu:9093 | @vsalamanca | Tier 3 | Bronze | ../page_views | Page view metrics piped in from Google Analytics |

Name indicates the data product name, while the combination of topic and bootstrap Uniform Resource Identifier (URI) indicates how you could go about connecting to the event stream, served by a Kafka broker. The owner indicates the Slack handle (or email address) to communicate with the data product owner. SLA and Quality are just two examples of the kinds of metadata made mandatory by the governance team, as discussed previously in “Metadata Standards and Requirements” on page 77.