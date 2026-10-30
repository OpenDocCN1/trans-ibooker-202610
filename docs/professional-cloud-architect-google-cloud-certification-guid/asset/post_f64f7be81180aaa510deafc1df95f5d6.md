
5. Now, we can populate it with values by clicking on our new table, browsing to Data, and then clicking Insert:

![](img/de2c97ab017f36b3750a5bade111edd7_378_0.png)

Figure 12.5 – Populating the table

6. The Query Editor will appear and allow us to fill in our table data. Do this by adding the following under the VALUES section and clicking RUN:

```
INSERT INTO
professionalCloudArchitect (EmployeeId,
  NAME,
  Result)
VALUES
(1, 'Brian Gerrard' , 'Pass'),
(2, 'Konrad Clapa' , 'Pass'),
(3, 'Joe Bloggs' , 'Pass'),
(4, 'Paul McDonal' , 'Fail'),
(5, 'Alison Munro' , 'Fail');
```

Figure 12.6 – Primary key

7. Our table view will now look like this:

![](img/de2c97ab017f36b3750a5bade111edd7_379_0.png)

Figure 12.7 – Data

8. Now, let's click on the Query tab again and perform a query on our information to see who has passed the exam. Select RUN to execute the query:

![](img/de2c97ab017f36b3750a5bade111edd7_379_1.png)

Figure 12.8 – Query

9. Let's view our results, as follows:

![](img/de2c97ab017f36b3750a5bade111edd7_380_0.png)

Figure 12.9 – Results

By doing this, we get a list of team members who have passed the exam.

## IAM

Access to Google Cloud Spanner is secured with IAM. The following is a list of predefined roles, along with a short description for each:

- **Spanner Admin**: This person has complete rights to all Cloud Spanner resources in a project.
- **Spanner Database Admin**: This person has the right to list all Cloud Spanner instances and create/list/drop databases in the instances it was created in. They can grant and revoke access to a database in the project, and they can also read and write to all Cloud Spanner databases in the project.
- **Spanner Database Reader**: This person has the right to read from the Cloud Spanner database, execute queries on the database, and view the schema for the database.
- **Spanner Database User**: This person has the right to read and write to a Cloud Spanner database, execute SQL queries on the database, and view and update the schema for the database.
- **Spanner Viewer**: This person has the right to view all Cloud Spanner instances and view all Cloud Spanner databases.
- **Restore Admin:** This person has the right to restore databases from backups.
- **Backup Admin:** This person has the right to *create*, *view*, *update*, and *delete* backups.

## Quotas and limits

Cloud Spanner comes with predefined quotas. These default quotas can be changed via the **Navigation** menu and via **IAM & Admin | Quotas**. From this menu, we can review the current quotas and request an increase for these limits. We recommend that you are aware of the limits for each service as this can have an impact on your scalability. For Cloud Spanner, we should be aware of the following limits:

- There is a limit of 2 to 64 characters on the instance ID's length.
- There is a limit of 100 databases per instance.
- There is a limit of 2 to 30 characters on the database ID's length.
- There is a limit of 2 TB storage per node.
- There is a limit of a 10 MB schema size.
- There is a limit of a 10 MB schema change size.
- There is a limit of 5,000 tables per database.
- There is a limit of 1 to 128 characters for the table name's length.
- There is a limit of 1,024 columns per table.
- There is a limit of 1 to 128 characters for the column name's length.
- There is a limit of 10 MB of data per column.
- There is a limit of 16 columns in a table key.
- There is a limit of 10,000 indexes per database.
- There is a limit of 32 indexes per table.
- There is a limit of 16 columns in an index key.
- There is a limit of 1,000 function calls.
- There is a limit of 25 nodes per project, per instance configuration.

## Pricing

Cloud Spanner charges for the amount of compute capacity and the amount of storage and network bandwidth that's used. We will be charged for the following:

- For the maximum number of nodes multiplied by the hourly rate. As Spanner has moved to processing units, this might be a fraction of a full node (1,000 processing units are smaller than 1 node).
- The average amount of data in our databases over 1 month. These prices will fluctuate, depending on the location of our instances.
- Egress network traffic for some types of traffic. However, there is no charge for replication or ingress traffic.
- The amount of storage that backups use is also charged over 1 month, multiplied by the monthly rate.

> Exam Tip
> An important point to remember about Cloud Spanner is that it offers ACID transactions. If an atomic transaction involves two or more pieces of information, then all of the pieces are committed; otherwise, none are.

## Bigtable

There is a clue in the name, but Bigtable is GCP's big data NoSQL database service. Bigtable is low latency and can scale to billions of rows and thousands of columns. It's also the database that powers many of Google's core services, such as Search, Analytics, Maps, and Gmail. This makes Bigtable a great choice for analytics and real-time workloads as it's designed to handle massive workloads at low latency and high throughput.

> Exam Tip
> Bigtable can support petabytes of data and is suitable for real-time access and analytics workloads. It's a great choice for Internet of Things (IoT) applications that require frequent data ingestion or high-speed transactions.

Given Bigtable's massive scalability, we will cover the storage model and architecture. When we discuss Bigtable, we will make references to HBase. HBase is effectively an open source implementation of the Bigtable architecture and follows the same design philosophies. Bigtable stores its data in tables, which are stored in a key/value map. Each table is comprised of rows, which will describe a single entity.

Tables are also comprised of columns, which contain individual values for each row. Rows are indexed by a row key, and columns that are related to one another are grouped into a column family. Bigtable only offers basic operations such as *create, read, update,* and *delete*. This means it has some good use cases while also not being great for others. Bigtable should not be used for transaction support – as we mentioned previously, CloudSQL or Spanner would be better suited for OLTP.

The following diagram shows the architecture of Bigtable:

![](img/de2c97ab017f36b3750a5bade111edd7_383_0.png)

Figure 12.10 – Bigtable architecture

Here, we can see that the client's requests go through a frontend server before they are sent to a Bigtable node. These nodes make up a Bigtable cluster belonging to a Bigtable instance, which acts as a container for the cluster.

Each node in the cluster handles a subset of the requests. We can increase the number of simultaneous requests a cluster can handle by adding more nodes. The preceding diagram only shows a single cluster, but replication can be enabled by adding a second cluster. With a second cluster, we can send different types of traffic to specific clusters and, of course, availability is increased.

To help balance out the workload of queries, Bigtable is sharded into blocks on contiguous rows. These are referred to as tablets and are stored on Google's filesystem – Colossus – in Sorted Strings Table (SSTable) format. SSTable stores immutable row fragments in a sorted order based on row keys. Each table will be associated with a specific Bigtable node, and writes are stored in the Colossus shared log as soon as they have been acknowledged. We should also note that data is never stored on Bigtable nodes. However, these nodes have pointers to a set of tablets that are stored in Colossus, meaning that if a Bigtable node fails, then no data loss is suffered.

Now that we understand the high-level architecture, let's look at the configuration of Bigtable.

## Bigtable configuration

In this section, we will look at the key components that make up Bigtable. We will discuss the following:

- Instances
- Clusters
- Nodes
- Schema
- Replication

## Instances

Cloud Bigtable is made up of instances that will contain up to four clusters that our applications can connect to. Each cluster contains nodes. We should imagine an instance as a container for our clusters and nodes. We can modify the number of nodes in our cluster and the number of clusters in our instance. When we create our instance, as expected, we can select the region and zone that we wish to deploy to and we can also select the type of storage, either SSD or HDD. The location and the storage type are both permanent choices.
