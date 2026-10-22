# DISTRIBUTING GRAPH DATABASES

Graph databases are commonly included in the NoSQL database categorization. They are however a little bit of an outsider. Graph data structures, as implemented by graph databases, explicitly represent relationships between nodes in the graph. This means that, just like with relational databases, how to partition the data is not obvious.

The core of the problem is how can a graph be partitioned into subgraphs that can then be distributed across multiple nodes and support efficient query processing. This is both theoretically and practically a hard problem, especially at the scale of contemporary graphs with billions of nodes and relationships. A solution would for example have to take into account access patterns to try and ensure queries don’t constantly follow relationships that point to remote data.

For these reasons, partitioning a graph database requires human guidance. For example, neo4j’s Fabric²³ extension allows a graph to be manually partitioned. Fabric creates what is essentially a proxy database to support queries that traverse relationships between nodes on different servers.

In summary, graph databases are non-trivial to scale out to improve performance. But give one enough compute resources, memory and disk in a single big server, and graph database engines can do some remarkable things.

Partitioning, commonly known as sharding, requires an algorithm to distribute the data objects in a logical database collection across multiple server nodes. Ideally, a sharding algorithm should evenly distribute data across the available resources. Namely if you have one hundred million objects and ten identical database servers, each shard will have ten million objects resident locally.

Sharding requires a shard or partition key that is used to allocate a given data object to a specific partition. When a new object is created, the shard key maps the object to a specific partition that resides on a server. When a query needs to access an object, it supplies the shard key so the database engine can locate the object on the server it resides. This is illustrated in Figure 10-7.