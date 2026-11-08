
Recovery point objective is a measure of how much data it is acceptable to lose in the event of a disaster. For example, if your only DR solution is backups and backups are scheduled to be taken every hour, you can achieve a recovery point objective of one hour. Recovery time objective is a measure of how long it will take to recover a solution after a failure. For example, if you have a recovery time objective of 30 minutes, then you must be able to restore service within half an hour.

It is important to determine the cost of downtime for your application, as this is one of the main drivers to determine your level of availability. The cost of downtime consists of both tangible and intangible costs. Tangible costs can be calculated, whereas intangible costs need to be estimated.

Redundant infrastructure helps you to maintain availability of your applications and services. A redundant server will be classified as hot, warm, or cold. A hot standby server is one which is kept synchronized with the live server and configured to allow automatic failover. This is suitable for HA scenarios. A warm standby server is one which is kept synchronized with the live server, but is not configured to failover automatically. Instead, an engineer must perform the failover manually. This is suitable for DR scenarios. A cold standby server is not kept synchronized with the live server and therefore cannot be failed over automatically. A cold standby server is suitable for DR scenarios where all data is read-only and never modified.

© Peter A. Carter 2020 P. A. CarterSQL Server 2019 AlwaysOn[https://doi.org/10.1007/978-1-4842-6479-9_2](https://doi.org/10.1007/978-1-4842-6479-9_2) Begin Abstract

# 2. Understanding High Availability and Disaster Recovery Technologies

Peter A. Carter^( [1](#394392_3_En_2_Chapter.xhtml#Aff2) ) (1) Botley, UK End Abstract

SQL Server provides a full suite of technologies for implementing high availability and disaster recovery. The following sections provide an overview of these technologies and discuss their most appropriate uses.

## AlwaysOn Failover Clustering

A Windows cluster is a technology for providing high availability in which a group of up to 64 servers works together to provide redundancy. An AlwaysOn Failover Clustered Instance (`FCI`) is an instance of SQL Server that spans the servers within this group. If one of the servers within this group fails, another server takes ownership of the instance. Its most appropriate usage is for high availability scenarios where the databases are large or have high write profiles. This is because clustering relies on shared storage, meaning the data is only written to disk once. With SQL Server–level HA technologies, write operations occur on the primary database and then again on all secondary databases, before the commit on the primary completes. This can cause performance issues. Even though it is possible to stretch a cluster across multiple sites, this involves SAN replication, which means that a cluster is normally configured within a single site.

Each server within a cluster is called a `node`. Therefore, if a cluster consists of three servers, it is known as a three-node cluster. Each node within a cluster has the SQL Server binaries installed, but the SQL Server service is only started on one of the nodes, which is known as the `active node`. Each node within the cluster also shares the same storage for the SQL Server data and log files. The storage, however, is only attached to the active node.

Tip

In geographically dispersed clusters (known as geoclusters or stretch clusters), each server is attached to different storage. The volumes are updated by SAN replication or Windows Storage Replica (a Windows Server technology, introduced in Windows Server 2016, which performs storage replication). The cluster regards the two volumes as a single, shared volume, which can only be attached to one node at a time.

If the active node fails, then the SQL Server service is stopped and the storage is detached. The storage is then reattached to one of the other nodes in the cluster, and the SQL Server service is started on this node, which is now the active node. The instance is also assigned its own network name and IP address, which are also bound to the active node. This means that applications can connect seamlessly to the instance, regardless of which node has ownership.

The diagram in Figure [2-1](#394392_3_En_2_Chapter.xhtml#Fig1) illustrates a two-node cluster. It shows that although the databases are stored on a shared storage array, each node still has a dedicated system volume. This volume contains the SQL Server binaries. It also illustrates how the shared storage, IP address, and network name are rebound to the passive node in the event of failover. ![Figure 2-1 ¶ Two-Node Cluster](images/394392_3_En_2_Chapter/394392_3_En_2_Fig1_HTML.jpg)

Figure 2-1 ¶ Two-Node Cluster

### Active/Active Configuration

Although the diagram in Figure [2-1](#394392_3_En_2_Chapter.xhtml#Fig1) illustrates an active/passive configuration , it is also possible to have an active/active configuration. Although it is not possible for more than one node at a time to own a single instance, and therefore it is not possible to implement load balancing, it is, however, possible to install multiple instances on a cluster, and a different node may own each instance. In this scenario, each node has its own unique network name and IP address. Each instance’s shared storage also consists of a unique set of volumes.

Therefore, in an active/active configuration, during normal operations, `Node1` may host `Instance1` and `Node2` may host `Instance2`. If `Node1` fails, both instances are then hosted by `Node2`, and vice versa. The diagram in Figure [2-2](#394392_3_En_2_Chapter.xhtml#Fig2) illustrates a two-node active/active cluster. ![Figure 2-2 ¶ Active/Active Cluster ](images/394392_3_En_2_Chapter/394392_3_En_2_Fig2_HTML.jpg)

Figure 2-2 ¶ Active/Active Cluster

Caution

In an active/active cluster, it is important to consider resources in the event of failover. For example, if each node has 128GB of RAM and the instance hosted on each node is using 96GB of RAM and locking pages in memory, then when one node fails over to the other node, this node fails as well, because it does not have enough memory to allocate to both instances. Make sure you plan both memory and processor requirements as if the two nodes are a single server. For this reason, active/active clusters are not generally recommended for SQL Server.

### Three-Plus Node Configurations

As previously mentioned, it is possible to have up to 64 nodes in a cluster . When you have three or more nodes, it is unlikely that you will want to have a single active node and two redundant nodes, due to the associated costs. Instead, you can choose to implement an `N+1` or `N+M` configuration.

In an `N+1` configuration, you have multiple active nodes and a single passive node. If a failure occurs on any of the active nodes, they fail over to the passive node. The diagram in Figure [2-3](#394392_3_En_2_Chapter.xhtml#Fig3) depicts a three-node `N+1` cluster. ![Figure 2-3 ¶ Three-Node N+1 Configuration](images/394392_3_En_2_Chapter/394392_3_En_2_Fig3_HTML.jpg)

Figure 2-3 ¶ Three-Node N+1 Configuration

In an `N+1` configuration, in a multifailure scenario, multiple nodes may fail over to the passive node. For this reason, you must be very careful when you plan resources to ensure that the passive node is able to support multiple instances. However, you can mitigate this issue by using an `N+M` configuration.
