# Getting Started: Bootstrapping with Connectors

Connectors enable you to easily bootstrap existing data into event streams without having to totally refactor your source applications. The reality is that most systems that create data, such as business entities and operational facts, are also the only sources for it. Similarly, these systems also have thousands (or millions?) of developer hours poured into them—we’re not going to be substantially overhauling the system just to get data out of it. Instead, we need to meet the systems where they currently are. as we discussed in “Connectors” on page 101, this is where connectors come in.

Bootstrapping data starts the conversation of who owns the data, the connector, and the transformations between the internal and external data model. Modifications to the internal data domain may break the connector, which breaks the downstream data product’s SLAs. Outages and failures tend to drive the need for remediation, resulting in clearer delegation of responsibility.

![](bbox=[0.16, 0.37, 0.24, 0.45])

When bootstrapping data, be sure it’s clear who owns what parts of the process and that each party knows its responsibilities. Renegotiating social contracts underpinning data mesh is as important as technical considerations.

Bootstrapping existing data isn’t just about sourcing it from the ol’ reliable monolith—data can be held in a database of any type, including relational, document, key-value, time-series, streaming, and large files in cloud storage. And though each mode of storage is different, obtaining data typically falls into two main camps: peri\-odically querying the database or tailing a change log. Ultimately, we want to extract the important business data, model it into a form that is useful to the intended consumers, and emit it as a well-formed data product. But before we get into querying and change logs, let’s take a look at a common anti-pattern.

# Dual Writes

The dual write, as shown in Figure 8-1, is one of the most common things that people do when starting out with event-driven systems. In brief, the application’s owner tries to write an update to a database record _and_ to an event stream at the same time. However, there is no atomic write guarantee between the two since the application’s database and the event stream are completely independent data storage engines.

The tricky part to dual writes is that they work most of the time. As long as you don’t have any intermittent failures, you’re not going to encounter an issue. But every now and then you’re going to encounter an issue—the network may timeout, there may be a bug in your application code, the event broker or database may be intermittently unavailable, etc. If you do not test and monitor for these specific failure modes, you’ll