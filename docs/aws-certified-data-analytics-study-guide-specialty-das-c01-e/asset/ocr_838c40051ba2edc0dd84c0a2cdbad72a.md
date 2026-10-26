+   - Streaming workloads are easier to persist.
+     **Machine Learning** Machine learning workloads may have a varying latency, which can be from a few milliseconds to a few minutes to hours of processing. A typical use case involves predicting customer churn, recommending products, or forecasting demand for a particular product. An example of machine learning workload is Spark ML on Amazon EMR and Amazon SageMaker.

Let’s look at some of the data processing and analysis tools available on the AWS platform.

# Amazon Athena

*Amazon Athena* is an interactive querying service that makes it easy to analyze data in S3 using standard SQL. It is serverless in nature, which means that you don’t have to manage and maintain any infrastructure but rather you can start analyzing data immediately. Amazon Athena supports the schema-on-read concepts, which is essential in this age of growing semi-structured and multi-structured data. It supports ANSI SQL operators and functions, which allows you to port queries from other analytical tools. Amazon Athena is massively scalable, which means that you don’t have to manually scale the infrastructure when the data size grows. It also allows you to run cross-region queries and manage your costs.

Amazon Athena has the following key benefits:

+   - Query data directly on S3 and other sources – Amazon Athena enables data engineers, data analysts, and data scientists to execute SQL queries against data stored in relational, nonrelational, and custom data sources. Before November 2019, Athena was primarily used to query data from S3 (an object store); however, with the announcement of Athena Federated Query (amzn.to/2YtsR3u), Amazon Athena can now be used to query multiple data sources using built-in and custom connectors.
+   - Serverless – Amazon Athena is serverless, and hence you don’t have to manage any infrastructure. You don’t need to manage configuration, software updates, failures, or infrastructure scalability. Users can focus on the core business problems rather than the infrastructure required.
+   - Highly performant – Amazon Athena provides superior performance by accessing the data in a parallel fashion.
+   - Pay-as-you-go model – Amazon Athena has a simple pay-as-you-go model where you are charged per query, and it offers best practices to reduce the cost of the query by using optimal file formats, compression and partitioning of data.

Amazon Athena uses Apache Presto for SQL queries and Apache Hive for DDL functionality behind the scenes.