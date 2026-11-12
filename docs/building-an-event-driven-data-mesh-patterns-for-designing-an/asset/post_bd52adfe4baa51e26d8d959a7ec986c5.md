
In my last book, *Building Event-Driven Microservices* (O'Reilly), I introduced the term *data communication layer*, touching on many of the same principles as data mesh: treat data as a first-class citizen, formalize the structure for communication between domains, publish data to event streams for general purpose usage, and make it easy to use for both the producers and consumers of data. And while I am fond of the data communication layer terminology, the reality is that I think the language and formalized principles of data mesh provide everything we need to talk about this problem *without* introducing another “data something something” paradigm.

Dehghani’s book, *Data Mesh* (O'Reilly), showcases the theory and thought leadership of data mesh in great depth and detail, but remains necessarily agnostic of specific implementations.

In this book, we’ll look at a practical implementation of data mesh that uses the event stream as the primary data product mode for interdomain data communications. We can be a bit more pragmatic and less intense on the theory and more concrete and specific on the implementation of an event-driven design. While I think that event streams are fundamentally the best option for interdomain communication, they do come with trade-offs, and I will, of course, cover these too, mentioning nonstreaming possibilities where they are best suited.

Data mesh is based on four main principles: domain ownership, data as a product, federated governance, and self-service platform. Together, these principles help us structure a way to communicate important business data across the entire organization. We’ll evaluate these principles in more detail in the next chapter, but before we get there, let’s take a look at *why data mesh matters today*.

## An Event-Driven Data Mesh

The modern competitive requirements of big data in motion, combined with modern cloud computing, require a rethink of how businesses create, store, move, and use data. The foundation of this new data architecture is the event, the data quantum that represents real business activities, provided through a multitude of purpose-built event streams. Event streams provide the means for a central nervous system for enabling business units to access and use fundamental, self-updating data building blocks. These data building blocks join the ranks of containerization, infrastructure as a service (IaaS), continuous integration (CI) and continuous deployment (CD) pipelines, and monitoring solutions, the components on which modern cloud applications are built.

Event streams are not new. But many of the technological limitations underpinning previous event-driven architectures, such as limited scale, retention, and performance, have largely been alleviated. Modern multitenant event brokers complete with tiered storage can store an unbounded amount of data, removing the strict capacity restrictions that limited previous architectures. Producers write their important business domain data to an event stream, enabling others to couple on that stream and use the data building blocks for their own applications. Finally, consumer applications can in turn create their own event streams to share their own business facts with others, resulting in a standardized communications mesh for all to use.

Data mesh provides us with very useful concepts and language for building out this interconnected central nervous system. Figure 1-1 shows a basic example of what a data mesh could look like.

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_17_0.png)

Figure 1-1. A very basic Hello Data Mesh implementation

The team that owns operational system Alpha selects some data from their service boundary, remodels it, and writes it to a source-aligned data product, which they also own (we’ll cover data product alignments more in “The Three Data Product Alignment Types” on page 33). The team that owns operational system Beta reads data from this data product into its own service boundary, again remodeling it, transforming it, and storing only what they need.

Meanwhile, a third team connects to Alpha team’s data product and uses it to compose their own aggregate-aligned data product. This same team then uses its aggregate-aligned data product to both power a streaming analytics use case and to write a batch of files to cloud storage, where data analysts will use it to compose reports and power existing batch-based analytics jobs.

This diagram represents just the tip of the data mesh iceberg, and there remain many areas to cover. But the gist of the event-driven data mesh is to make data readily available in real time to any consumers who need it.

Many of the problems that data mesh solves have existed for a very long time. We’re now going to take a brief history tour to get a better understanding of *what* it is we’re solving and *why* data mesh is a very relevant and powerful solution.

## Using Data in the Operational Plane

Data tends to be created by an operational system *doing business things*. Eventually, that data tends to be pulled into the analytical plane for analysis and reporting purposes. In this section, we’ll focus on some of the operational plane and the common challenges of sharing business data with other operational (and analytical) services.

### The Data Monolith

Online transaction processing (OLTP) databases form the basis of much of today’s operational computer services (let’s call them “monoliths” for simplicity). Monolithic systems tend to play a big role in the operational plane, as consistent synchronous communication tends to be simpler to reason and develop than asynchronous communication. Relational databases, such as PostgreSQL and MySQL, feature heavily in monolithic applications, providing atomicity, consistency, isolation, and durability (ACID) transactions and consistent state for the application.

Together, the application and database demonstrate the following *monolith data principles*:

- *The database is the source of truth*
  The monolith relies on the underlying database to be the durable store of information for the application. Any new or updated records are first recorded into the database, making it the *definitive source of truth* for those entities.

- *Data is strongly consistent*
  The monolith’s data, when stored in a typical relational database, is strongly consistent. This provides the business logic with strong read-after-write consistency, and, thanks to transactions, it will not inadvertently access partially updated records.

- *Read-only data is readily available*
  The data stored within the monolith’s database can be readily accessed by any part of the monolith. Read-only access permissions ensure that there are no inadvertent alterations to the data.

Note that the database should be directly accessed only by the service that owns it, and not used as an integration point.

These three principles form a binding force that make monolithic architectures powerful. Your application code has read-only access to the entire span of data stored in the monolith’s database as a set of authoritative, consistent, and accessible data primitives. This foundation makes it easy to build new application functionality provided it’s in the same application. But what if you need to build a new application?

### The Difficulties of Communicating Data for Operational Concerns

A new application cannot rely on the same easy access to data primitives that it would have if it were built as part of the monolith. This would not be a problem if the new application had no need for any of the business data in the monolith. However, this is rarely the case, as businesses are effectively a set of overlapping domains, particularly the common core, with the same data serving multiple business requirements. For example, an ecommerce retailer may rely on its monolith to handle its orders, sales, and inventory, but requires a new application powered by a document-based database (or other database type) for plain-text search functionality. Figure 1-2 highlights the crux of the issue: how do we get the data from Ol’ Reliable into the new document database to power search?

![](img/4a76b8ac0abd2ab4b0aad729485a0a2b_19_0.png)

Figure 1-2. The new search service team must figure out how to get the data it needs out of the monolith and keep it up to date

This puts the new search service team in a bit of a predicament. The service needs access to the item, store, and inventory data in the monolith, but it also needs to model it all as a set of documents for the search engine. There are two common ways that teams attempt to resolve this. One is to replicate and transform the data to the search engine, in an attempt to preserve the three monolith data principles. The second is to use APIs to restructure the service boundaries of the source system, such
