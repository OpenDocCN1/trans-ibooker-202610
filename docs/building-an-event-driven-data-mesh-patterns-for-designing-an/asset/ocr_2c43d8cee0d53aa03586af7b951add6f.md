![](bbox=[0.14, 0.08, 0.85, 0.25])

# Figure 1-5. A typical data warehouse ETL workflow

Historically, data warehouses have proven to be successful at providing a means for analytical results. But the ever-increasing data and compute loads required larger disk drives and more powerful compute chips and ultimately ran into the physical limits of computer hardware. So instead of further scaling up, it became time to scale out.

The need for massive scale was plainly evident to Google, which published **The Google File System** in October, 2003. This is the era that saw the birth of big data and caused a massive global rethink of how we create, store, process, and, ultimately, use data.

![](bbox=[0.18, 0.47, 0.25, 0.54])

Many modern data warehouses also offer high scalability, but it wasn't until the advent of the big data revolution that this became a reality. Historically, data warehouses were limited by the same factors as any other system operating on a single server.

**Apache Hadoop** quickly caught on as the definitive way to solve the scaling problems facing traditional OLAP systems. Because it's free and open source, it could be used by any company, anywhere, provided you could figure out how to manage the infrastructure requirements. It also provided a new way to compute analytics, one where you were no longer constrained to a proprietary system limited by the resources of a single computer.

Hadoop introduced the Hadoop Distributed File System (HDFS), a durable, fault-tolerant filesystem that made it possible to create, store, and process truly massive data sets spanning multiple commodity hardware nodes. While HDFS has now been largely supplanted by options such as Amazon S3 and Google Cloud Storage, it paved the way for a bold new idea: copy all of the data you need into a single logical location, regardless of size, and apply processing to clean up, sort out, and remodel data into the required format for deriving important business analytics.