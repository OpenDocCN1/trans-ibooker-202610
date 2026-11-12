![](bbox=[0.17, 0.1, 0.83, 0.33])

Figure 2-1. Spark architecture diagram (placeholder - replace SparkContext with SparkSession)

Figure 2-1 depicts the Spark architecture through high-level components. Spark applications run as independent sets of processes on a cluster or locally. At a high level, a Spark application is comprised of a driver process, cluster manager and a set of executor processes. The driver program is the central component and responsible for distributing tasks across executor processes. There will always be just one driver process. When we talk about scaling, we mean increasing the number of executors. The cluster manager simply manages resources.

Spark is a distributed, data-parallel compute engine. In the data-parallel model, more data partitions equals more parallelism. Partitioning allows for efficient parallelism. A distributed scheme of breaking up data into chunks or partitions allows Spark executors to process only data that is close to them, minimizing network bandwidth. That is, each executor’s core is assigned its own data partition to work on. Remember this whenever a choice related to partitioning comes up.

Spark programming starts with a data set, usually residing in some form of distributed, persistent storage like HDFS or a cloud-based solution such as AWS S3 and in a format like Parquet. Writing a Spark program typically consists of a few steps:

+   1. Define a set of transformations on the input data set.

+   2. Invoke actions that output the transformed data sets to persistent storage or return results to the driver’s local memory. These actions will ideally be