+   - *Compute node:* A compute node is the workhorse for your Redshift cluster and sits behind the leader node, executing queries for the user. Since Redshift has a shared-nothing MPP architecture, compute nodes work together to perform the desired task. A compute node is partitioned into multiple slices, the number of which varies between 2 and 16 and depends on the node type you configure in your Redshift cluster. A slice is an essential component in the Redshift compute node architecture, and each slice is allocated a portion of the compute node’s memory and disk space, thus acting as a virtual compute node. Each slice acting as a virtual compute node processes a portion of the work assigned to the compute node by the leader node. It is therefore essential to have an appropriate data distribution for the compute node, which is done using various mechanisms that we will discuss later. Each slice acts as a symmetric multiprocessing (SMP) unit and works with other slices to complete operations.

Redshift supports various instance types, and the choice of an instance type is directly dependent on the workload that you are planning to run on your data warehouse. Table 4.12 indicates the type of instances available with Redshift and the corresponding disk type, size, memory, number of CPUs, and number of slices.

# TABLE 4.12 Redshift instance types

| Instance Type | Disk Types | Size | Memory | # CPUs | # Slices |
|---|---|---|---|---|---|
| ra3 4xlarge | RMS | Scales up to 16 TB | 96 GB | 12 | 4 |
| ra3 16xlarge | RMS | Scales up to 64 TB | 384 GB | 48 | 16 |
| dc2 large | SSD | 160 GB | 16 GB | 2 | 2 |
| dc2 8xlarge | SSD | 2.56 TB | 244 GB | 32 | 16 |
| ds2 xlarge | Magnetic | 2 TB | 32 GB | 4 | 2 |
| ds2 8xlarge | Magnetic | 16 TB | 244 GB | 36 | 16 |

# Redshift Spectrum/Lakehouse Nodes

The data lake is an important part of the overall analytics architecture, and as discussed earlier, Redshift offers lake house (also referred to as Redshift Spectrum), which is configured with nodes that allow you to query against the data lake. Redshift lake house nodes are free of cost; you don’t pay for them, but rather pay for using them to scan your data on S3 when you request data from your data lake.

Amazon Redshift lake house is one of the first solutions that gives you one foot in the data warehouse and the other in the data lake, and hence termed as a lake house. Redshift provides the lake house capability so that you can extend your queries to Amazon