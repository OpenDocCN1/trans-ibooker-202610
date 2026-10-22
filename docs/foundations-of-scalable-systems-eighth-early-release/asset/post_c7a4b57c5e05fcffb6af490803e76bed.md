
JSON and store, query and manipulate your data directly as a document. No elaborate object-relational mapper is needed. The result is intuitive and simpler business logic.

The initial popularity of MongoDB was driven by its ease of programming and use. The underlying storage engine in the early releases, known as MMAPv1,⁴ was however less than ideal. MMAPv1 implements memory mapped files using the mmap() system call. Documents in the same logical groupings, known as *collections*, are allocated contiguously on disk. This is great for sequential read performance. But if an object grows in size, new space has to be allocated and all document indexes updated. This can be a costly operation, and lead to disk fragmentation.

To minimize this cost, MMAPv1 initially allocates documents with additional space to accommodate growth. A solution indeed, but perhaps not the most space efficient and scalable. In addition, document locks for updates are obtained at very coarse grain levels (e.g in various releases, server, database, collection), causing less than spectacular write performance.

Around 2015, the development team re-engineered MongoDB to support a pluggable storage engine architecture. Soon after, a new storage engine, WiredTiger, became default in MongoDB v3.2. WiredTiger addresses many of the shortcomings of MMAPv1.⁵ It introduces optimistic concurrency control and document-level locking, compression, operational journaling and checkpointing for crash recovery, and its own internal cache for improved performance.

## Data Model and API

MongoDB documents are basically JSON objects with a set of extended types defined in the Binary JSON (BSON) specification. Documents are stored in BSON format, and organized in databases comprising one or more collections. Collections are equivalent to a relational database table, but without a defined schema. This means MongoDB collections do not enforce a structure on documents. Documents with different structures can be stored

in the same collection. This is a schemaless, or schema-on-read approach that requires the application to interpret a document structure on access.

MongoDB documents are composed of name-value pairs. The value of a field may be any BSON data type. Documents can also incorporate other documents, known as embedded or nested documents, and arrays of values or documents. Every document has an _id field which acts as the primary key. Applications can set this key value on document creation, or allow the MongoDB client to automatically allocate a unique value. You can also define secondary indexes on any field, sub-field or on multiple fields - a compound key - in a collection.

An example document that you might find in a skier management system is shown in the code below. The field skiresorts is represented as an array of strings, and each different ski day is represented by an element in an array of nested documents.

```
{
  _id: 6788321471
  name: { first: "Ian", last: "Gorton" }
  location: "USA-WA-Seattle",
  skiresorts: ["Crystal Mountain", "Mission Ridge"]
  numdays: 2
    season21 {
      {
        day: 1
        resort: "Crystal Mountain",
        vertical: 30701
        lifts: 27
        date: "12/1/2021"
      }
      {
        day: 2
        resort: "Mission Ridge",
        vertical: 17021
        lifts: 10
        date: "12/8/2021"
      }
    }
}
```

As there is no uniform document structure in a collection, the storage engine needs to persist field names and values for every document. For small documents, long field names may end up representing the majority of the document size. Shorter field names can reduce the size of the document on disk,⁶ and at scale, in a collection with many millions of documents, this saving will become significant. Optimized document sizes reduce disk usage, memory and cache consumption, and network bandwidth. As usual, at scale, small optimizations can pay back many times in minimizing resource utilization.

To manipulate documents, MongoDB provides APIs for basic CRUD operations. There is a .find() method for collections with an extensive set of conditions and operators that emulate an SQL SELECT statement for documents in a single collection. MongoDB supports aggregate queries with the $match and $group operators, and the $lookup operator provides SQL JOIN-like behavior across collections in the same database. A simple example of querying a collection is shown in the following. The .find() operation returns all documents for skiers who have registered more than 20 ski days from the skiers2021 collection

```
db.skiers2021.find({ numdays: { $gt: 20 } })
```

Write operations to a single document in MongoDB are atomic. For this reason, if you denormalize your data model to make extensive use of nested documents, you can avoid the complexities of updating multiple documents and distributed transactions in your application code. Before MongoDB version 4.0, this was essentially the only way to ensure consistency for multi-document updates without complex application logic to handle failures.

Since version 4.0, support for ACID, multi-document transactions has been implemented. MongoDB transactions use two-phase commit⁷ and leverage the underlying WiredTiger storage engine’s snapshot isolation capabilities. Snapshot isolation is a weaker guarantee than the serialization implied by the ACID semantics. This enables higher performance than serialization and avoids most, but not all, of the concurrency anomalies that

serializability avoids. Snapshot isolation is actually the default in many relational databases, including Oracle and PostgreSQL.

## Distribution and Replication

To scale horizontally, you can choose between two data partitioning, or sharding strategies with MongoDB. These are hash-based and range-based sharding. You define a shard key for each document based on one or more field values. Upon document creation, MongoDB then chooses a database shard to store the document based on either:

- The result of a hash function applied to the shard key
- The shard that is defined to store the shard key range within which the key resides

Sharded deployments in MongoDB require you to deploy three distinct database components. The *mongod* process is the MongoDB database daemon that must run on every shard. The *mongos* process is responsible for processing database client queries by routing requests to the targeted shard(s) and returning the results to the client. Clients issue MongoDB API calls using a MongoDB driver. *Config Servers* store database cluster configuration metadata, which the *mongos* uses to route queries to the correct shards based on shard key values. This architecture is depicted in Figure 13-2.

![](img/936a72197720a5158b16541313af61c9_357_0.png)

Figure 13-2. MongoDB Database Partitioning Architecture

The mongos process acts as a proxy between the client’s MongoDB driver and the database shards. All client requests must pass through a mongos instance. A mongos has no persistent state, and simply caches the cluster configuration information it obtains from the Config Servers.

The mongos process is the client’s only query interface. It is therefore critical for performance and scalability that sufficient mongos processing capacity is available. Precisely how you configure mongos deployments is highly dependent on your applications needs, and MongoDB provides you with flexibility to design your system to satisfy the required workload. There are three basic alternatives, as depicted in Figure 13-3:

### Configuration (a)

Deploy a *mongos* on each application server that acts as a MongoDB client. This reduces latency by making every client request to mongos a local call.

### Configuration (b)

Deploy a *mongos* on every database shard. In this configuration, a *mongos* can communicate with the shard locally.

### Configuration (c)

Deploy a collection of *mongos* on their own dedicated hardware. You incur additional network latency communicating with the client and database shards. The trade-off is that the *mongos* load is eliminated from the application server and database nodes, and the *mongos* processes are allocated more exclusive processing capacity.
