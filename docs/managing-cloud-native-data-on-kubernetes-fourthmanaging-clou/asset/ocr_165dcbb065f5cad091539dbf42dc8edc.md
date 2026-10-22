concepts shown in Figure 5-3: cells, keyspaces, shards, leaders, and replicas.

![](bbox=[0.21, 0.155, 0.795, 0.445])

Figure 5-3. Vitess cluster topology - cells, keyspaces, and shards

At a high level, a Vitess cluster consists of multiple MySQL instances which may be spread across multiple data centers or cells. Each MySQL instance takes on a role as either a leader or replica, and may be dedicated to a specific slice of a database known as a shard. Let’s consider the implications of each of these concepts for reading and writing data in Vitess.

# Cell

A typical production deployment of Vitess is spread across multiple failure domains in order to provide high availability. Vitess refers to each of these failure domains as a cell. The recommended topology is a cell per data center or cloud provider zone. While writes and replication involve communication across cell boundaries, Vitess reads are confined to the local cell to optimize performance.

# Keyspace