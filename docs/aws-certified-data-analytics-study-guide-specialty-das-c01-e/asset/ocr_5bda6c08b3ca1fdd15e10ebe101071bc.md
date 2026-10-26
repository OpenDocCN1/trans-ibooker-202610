You can also use registration-less federated query, which is really useful for quick prototyping. All you need to do is add the prefix "lambda:<function_name>". as the catalog name, and you can access the data source.

For example, "SELECT * from "lambda:cmdb".ec2.ec2_instances".

At the time of writing this book, the following connectors are available:

+   - HBase
+   - DocumentDB
+   - DynamoDB
+   - JDBC
+   - Redis/ElastiCache
+   - Amazon CloudWatch Logs
+   - Amazon CloudWatch Metrics
+   - TDPS Data Generator

You can also write your own data source connector with Amazon Athena Query Federation SDK. An example connector is available at bit.ly/2YEm0og. The Amazon Athena Query Federation SDK includes the following key features:

+   - S3 spill
+   - Partition Pruning
+   - Parallel Scans
+   - Portable columnar memory format (Apache Arrow)
+   - Authorization
+   - Congestion control/avoidance

# Amazon Athena Custom UDFs

Amazon Athena now adds the ability to build custom *user-defined functions (UDFs)*, which provide the ability to pre- or post-process the data, provide access controls, and add custom logic during data access. You can invoke UDFs in the select or filter phase of an Amazon Athena query. Amazon Athena custom UDFs provide the following features:

+   - UDFs are powered by AWS Lambda.
+   - UDFs support network calls.
+   - UDF invocation can be used in the select and/or filter phase of the query.
+   - Athena optimizes performance, thus allowing you to focus on the business logic.

# Using Machine Learning with Amazon Athena

Amazon Athena allows you to invoke ML models for inference directly from SQL queries. You can use ML with your SQL analysis for anomaly detection, customer cohort analysis,