In a document database, you can simply add the stats to the document. No table creation is needed, and no expensive joins are required at runtime, making it simple to develop and cheaper to maintain.

# Amazon DocumentDB Overview

Amazon DocumentDB is a fast, scalable, highly available and fully managed MongoDB-compatible service. It has the following key features that make it an ideal fit for storing, processing, and analyzing documents:

+   - It can handle millions of requests at a single-digit millisecond latency.
+   - It is deeply integrated with the AWS platform.
+   - You can use the same code, drivers, and tools that you use with MongoDB.
+   - It is secure.
+   - It is fully managed.

Amazon DocumentDB supports flexible storage, where the storage volume grows as your database storage needs to grow in increments of 10 GB up to a maximum of 64 TB. You don’t need to provision any excess storage for your cluster to handle growth as this is done behind the scenes. For read-heavy applications, Amazon DocumentDB can support up to 15 read replica instances. Since the replicas share the same underlying storage, you can reduce the overall cost by eliminating the need to perform writes at read-replica nodes. The replica lag time is reduced to a single-digit number of milliseconds and provides you with better response for your read heavy workloads. Amazon DocumentDB provides a cluster endpoint for read/write and a reader endpoint for reading data, which allows applications to connect without having to track replicas as they are added or removed. You can add read replicas in minutes regardless of the storage volume sizes. You can scale the compute and memory resources of your cluster up and down, and these scaling operations complete within a few minutes.

Amazon DocumentDB continuously monitors the health of your cluster, and upon detecting an instance failure, it will automatically restart the instance and associated processes. Amazon DocumentDB doesn’t require a crash recovery replay of database redo logs, which greatly reduces restart times. Amazon DocumentDB also isolates the database cache from the database process, enabling the cache to survive an instance restart. Upon failure, one of the 15 read replicas provisioned can act as a failover instance, and in case a customer hasn’t provisioned a read replica, a new Amazon DocumentDB instance is created automatically.

Amazon DocumentDB also allows point-in-time recovery (PITR), which allows you to restore your cluster to any second during the retention period, up to the last 5 minutes. The retention period can be configured up to 15 days, with backups being stored in Amazon S3. Backups are done in an automatic incremental fashion without impacting the cluster performance.